---
title: 'Kurz: vytvoření a nasazení vlastní dovednosti pomocí Azure Machine Learning'
titleSuffix: Azure Cognitive Search
description: V tomto kurzu se dozvíte, jak pomocí Azure Machine Learning vytvořit a nasadit vlastní dovednost pro kanál rozšíření AI pro Azure Kognitivní hledání.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 06/10/2020
ms.openlocfilehash: 69618604c38d82567260e45d651df523055c5f7b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86245326"
---
# <a name="tutorial-build-and-deploy-a-custom-skill-with-azure-machine-learning"></a>Kurz: sestavení a nasazení vlastní dovednosti pomocí Azure Machine Learning 

V tomto kurzu použijete [datovou sadu přezkoumání hotelu](https://www.kaggle.com/datafiniti/hotel-reviews) (distribuovanou v rámci licence Creative-4,0 License [CC-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode.txt)) a vytvoříte [vlastní dovednost](https://docs.microsoft.com/azure/search/cognitive-search-aml-skill) pomocí Azure Machine Learning k extrakci mínění založených na aspektech z revizí. To umožňuje, aby přiřazení pozitivních a záporných mínění v rámci stejné revize bylo správně přiřazené k identifikovaným entitám, jako jsou například zaměstnanci, místnosti, předsálí nebo fondy.

Pro výuku modelu mínění založeného na aspektech v Azure Machine Learning budete používat [úložiště recepty NLP](https://github.com/microsoft/nlp-recipes/tree/master/examples/sentiment_analysis/absa). Model se pak nasadí jako koncový bod v clusteru Azure Kubernetes. Po nasazení se koncový bod přidá do kanálu pro rozšíření jako AML dovednost pro použití službou Kognitivní hledání.

Jsou k dispozici dvě datové sady. Pokud chcete model naučit sami sebe, je vyžadován soubor hotel_reviews_1000.csv. Chcete přeskočit krok školení? Stáhněte hotel_reviews_100.csv.

> [!div class="checklist"]
> * Vytvoření instance služby Azure Kognitivní hledání
> * Vytvořte pracovní prostor Azure Machine Learning (vyhledávací služba a pracovní prostor by měly být ve stejném předplatném).
> * Výuka a nasazení modelu do clusteru Azure Kubernetes
> * Propojení kanálu rozšíření AI s nasazeným modelem
> * Ingestovat výstup z nasazeného modelu jako vlastní dovednosti

> [!IMPORTANT] 
> Tato dovednost je aktuálně ve verzi Public Preview. Funkce Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). V tuto chvíli není podporovaná žádná podpora sady .NET SDK.

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure – Získejte [bezplatné předplatné](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Služba Kognitivní hledání](https://docs.microsoft.com/azure/search/search-get-started-arm)
* [Prostředek Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows)
* [Účet Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal)
* [Pracovní prostor služby Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace)

## <a name="setup"></a>Nastavení

* Naklonujte nebo Stáhněte obsah [ukázkového úložiště](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/AzureML-Custom-Skill).
* Extrahuje obsah, pokud je stahování souborem zip. Ujistěte se, že jsou soubory pro čtení i zápis.
* Při nastavování účtů a služeb Azure zkopírujte názvy a klíče do snadno dostupného textového souboru. Názvy a klíče budou přidány do první buňky v poznámkovém bloku, kde jsou definovány proměnné pro přístup ke službám Azure.
* Pokud nejste obeznámeni s Azure Machine Learning a jejími požadavky, budete si chtít tyto dokumenty před začátkem začít:
 * [Konfigurace vývojového prostředí pro Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment)
 * [Vytváření a Správa pracovních prostorů Azure Machine Learning v Azure Portal](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace)
 * Při konfiguraci vývojového prostředí pro Azure Machine Learning zvažte použití [cloudové výpočetní instance](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#compute-instance) pro rychlost a usnadnění práce v části Začínáme.
* Nahrajte soubor DataSet do kontejneru v účtu úložiště. Větší soubor je nutný, pokud chcete provést krok školení v poznámkovém bloku. Pokud budete chtít přeskočit krok školení, je doporučeno zmenšit soubor.

## <a name="open-notebook-and-connect-to-azure-services"></a>Otevřete Poznámkový blok a připojte se ke službám Azure.

1. Zadejte všechny požadované informace pro proměnné, které umožní přístup ke službám Azure v první buňce a spustí buňku.
1. Po spuštění druhé buňky se potvrdí, že jste se připojili ke službě vyhledávání pro vaše předplatné.
1. Oddíly 1,1 – 1,5 vytvoří úložiště dat služby Search, dovednosti, index a indexer.

V tomto okamžiku se můžete rozhodnout, že budete chtít přeskočit kroky k vytvoření sady školicích dat a experimentovat Azure Machine Learning a přeskočit přímo k registraci dvou modelů, které jsou k dispozici ve složce modely úložiště GitHub. Pokud přeskočíte tyto kroky, přejděte v poznámkovém bloku do části 3,5 a zapište si skript bodování. Ušetří se čas; dokončení kroků stažení a nahrání dat může trvat až 30 minut.

## <a name="creating-and-training-the-models"></a>Vytváření a školení modelů

Oddíl 2 obsahuje šest buněk, které stáhnou soubor vkládání šetrnější z úložiště recepty NLP. Po stažení se soubor nahraje do úložiště dat Azure Machine Learning. Soubor. zip má asi 2G a při provádění těchto úkolů bude nějakou dobu trvat. Po nahrání se pak vyextrahují školicí data a teď jste připraveni přejít na oddíl 3.

## <a name="train-the-aspect-based-sentiment-model-and-deploy-your-endpoint"></a>Vyškolení modelu mínění založeného na aspektech a nasazení koncového bodu

Část 3 poznámkového bloku bude vytvářet modely vytvořené v části 2, registrovat tyto modely a nasazovat je jako koncový bod v clusteru Azure Kubernetes. Pokud nejste obeznámeni s Azure Kubernetes, důrazně doporučujeme před pokusem o vytvoření clusteru odvození zkontrolovat následující články:

* [Přehled služby Azure Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes)
* [Základní koncepty Kubernetes pro Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads)
* [Kvóty, omezení velikosti virtuálních počítačů a dostupnost oblastí ve službě Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/quotas-skus-regions)

Vytvoření a nasazení clusteru odvození může trvat až 30 minut. Při testování webové služby před přechodem k posledním krokům se doporučuje aktualizovat dovednosti a spustit indexer.

## <a name="update-the-skillset"></a>Aktualizace dovednosti

Oddíl 4 v poznámkovém bloku obsahuje čtyři buňky, které aktualizují dovednosti a indexer. Alternativně můžete pomocí portálu vybrat a použít novou dovednost na dovednosti a pak spustit indexer a aktualizovat vyhledávací službu.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Active-Learning-with-Azure-Cognitive-Search/player#time=19m35s:paused/03/player]

Na portálu přejít na dovednosti a vyberte odkaz definice dovednosti (JSON). Portál zobrazí kód JSON vašeho dovednosti, který byl vytvořen v první buňce poznámkového bloku. Napravo od zobrazení je rozevírací nabídka, ve které můžete vybrat šablonu definice dovednosti. Vyberte šablonu Azure Machine Learning (AML). Zadejte název pracovního prostoru Azure ML a koncový bod modelu nasazeného do odvozeného clusteru. Šablona bude aktualizována pomocí identifikátoru URI a klíče koncového bodu.

> [!div class="mx-imgBorder"]
> ![Šablona definice dovednosti](media/cognitive-search-aml-skill/portal-aml-skillset-definition.png)

Zkopírujte šablonu dovednosti z okna a vložte ji do definice dovednosti na levé straně. Upravte šablonu, aby poskytovala chybějící hodnoty pro:

* Název
* Popis
* Kontext
* název a zdroj vstupů
* název a cílový_název pro výstupy

Uložte dovednosti.

Po uložení dovednosti přejít na indexer a vybrat odkaz na definici indexeru (JSON). Portál zobrazí kód JSON indexeru, který byl vytvořen v první buňce poznámkového bloku. Mapování polí výstup bude nutné aktualizovat pomocí dalších mapování polí, aby bylo zajištěno, že indexer bude schopen pracovat a správně předat. Uložte změny a pak vyberte spustit. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud pracujete s vlastním předplatným, je vhodné vždy na konci projektu zkontrolovat, jestli budete vytvořené prostředky ještě potřebovat. Prostředky, které necháte běžet, vás stojí peníze. Můžete odstraňovat prostředky jednotlivě nebo odstraněním skupiny prostředků odstranit celou sadu prostředků najednou.

Prostředky můžete najít a spravovat na portálu pomocí odkazu **všechny prostředky** nebo **skupiny prostředků** v levém navigačním podokně.

Pokud používáte bezplatnou službu, pamatujte na to, že jste omezeni na tři indexy, indexery a zdroje dat. Jednotlivé položky na portálu můžete odstranit, aby zůstaly pod limitem.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Kontrola webového rozhraní API](https://docs.microsoft.com/azure/search/cognitive-search-custom-skill-web-api) 
>  pro vlastní dovednosti [Další informace o přidání vlastních dovedností do kanálu pro obohacení](https://docs.microsoft.com/azure/search/cognitive-search-custom-skill-interface)