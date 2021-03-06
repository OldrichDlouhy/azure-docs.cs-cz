---
title: Obsáhlý Learning a strojové učení
titleSuffix: Azure
description: Seznamte se s tím, jak obsáhlý Learning souvisí se strojovým učením a umělou logikou. Obsáhlý Learning se používá ve scénářích, jako je odhalování podvodů, rozpoznávání hlasových &, rozpoznávání obličeje, analýza mínění a prognózování časových řad.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 03/05/2020
ms.openlocfilehash: b024010583ba1c6e0ffdf663f7335011ce212bf1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81414587"
---
# <a name="deep-learning-vs-machine-learning"></a>Obsáhlý Learning a strojové učení

Tento článek vám pomůže porovnat hloubkové učení a strojové učení. Dozvíte se, jak tyto dvě koncepce porovnávají a jak se vejdou do širší kategorie umělých inteligentních funkcí. Tento článek také popisuje, jak se dá využít obsáhlý Learning v reálných scénářích, jako je odhalování podvodů, rozpoznávání hlasu a obličeje, mínění Analytics a prognózy časových řad.

## <a name="deep-learning-machine-learning-and-ai"></a>Obsáhlý Learning, strojové učení a AI

![Diagram vztahů: AI vs. Machine Learning vs. obsáhlý Learning](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Zvažte následující definice pro pochopení hloubkového učení vs. strojové učení vs. AI:

- **Obsáhlý Learning** je podmnožinou strojového učení založeného na umělých neuronovéch sítích. _Proces výuky_ je _hluboký_ , protože struktura umělých neuronové sítí sestává z několika vstupních, výstupních a skrytých vrstev. Každá vrstva obsahuje jednotky, které transformují vstupní data na informace, které může další vrstva použít pro určitou prediktivní úlohu. Díky této struktuře se může počítač seznámit s vlastním zpracováním dat.

- **Machine Learning** je podmnožinou umělých inteligentních funkcí, které využívají techniky (jako je obsáhlý Learning), která umožňuje počítačům používat prostředí pro zlepšení úloh. _Proces výuky_ je založený na následujících krocích:

   1. Data se zakládá do algoritmu. (V tomto kroku můžete k modelu zadat další informace, například prováděním extrakce funkcí.)
   1. Tato data slouží k učení modelu.
   1. Otestujte a nasaďte model.
   1. Využití nasazeného modelu k provedení automatizované prediktivní úlohy. (Jinými slovy, zavolejte a použijte nasazený model pro příjem předpovědi vrácených modelem.)

- **Umělá inteligentní funkce (AI)** je technika, která umožňuje počítačům napodobovat lidské inteligentní funkce. Zahrnuje Machine Learning. 
 
Je důležité pochopit vztah mezi AI, Machine Learningem a hlubokou výukou. Machine Learning je způsob, jak dosáhnout umělých inteligentních funkcí. Pomocí technik strojového učení a hloubkového učení můžete sestavovat počítačové systémy a aplikace, které budou často přidruženy k lidským inteligentním funkcím. Mezi tyto úlohy patří rozpoznávání obrázků, rozpoznávání řeči a překlad jazyka.

## <a name="techniques-of-deep-learning-vs-machine-learning"></a>Techniky hloubkového učení vs. Machine Learning 

Teď, když máte přehled o strojovém učení vs. hluboké učení, porovnáme tyto dvě techniky. Ve strojovém učení musí být algoritmus znám, jak zajistit přesnou předpověď tím, že se zaměříte na více informací (například provedením extrakce funkcí). V obsáhlém učení se algoritmus může naučit, jak zajistit přesnou předpověď prostřednictvím vlastního zpracování dat, a to díky umělé struktuře neuronové sítě.

Následující tabulka porovnává tyto dvě metody podrobněji:

| |Všechny Machine Learning |Jenom obsáhlý Learning|
|---|---|---|
|  **Počet datových bodů** | Pomocí malých objemů dat můžete vytvořit předpovědi. | K zajištění předpovědi je potřeba použít velké množství školicích dat. |
|  **Závislosti hardwaru** | Může pracovat na méně koncových počítačích. Nepotřebuje velké množství výpočetního výkonu. | Závisí na vysoce koncových počítačích. Tím je v podstatě velký počet operací násobení matice. GPU může efektivně optimalizovat tyto operace. |
|  **Featurization proces** | Vyžaduje, aby funkce byly přesně identifikovány a vytvořeny uživateli. | Zjišťuje funkce na nejvyšší úrovni z dat a vytváří nové funkce sama o sobě. |
|  **Postup učení** | Rozděluje studijní proces do menších kroků. Pak sloučí výsledky z každého kroku do jednoho výstupu. | Projděte si proces učení tím, že tento problém vyřešíte na uceleném základě. |
|  **Doba spouštění** | Provede poměrně krátkou dobu, od několika sekund až po několik hodin. | Výuka obvykle trvá příliš dlouho, protože algoritmus hloubkového učení zahrnuje mnoho vrstev. |
|  **Výstup** | Výstupem je obvykle číselná hodnota, jako je například skóre nebo klasifikace. | Výstup může mít několik formátů, jako je text, skóre nebo zvuk. |

## <a name="deep-learning-use-cases"></a>Případy použití hloubkového učení

Z důvodu umělé struktury Neuronové sítě je obsáhlý Learning v Excelu v identifikaci vzorů nestrukturovaných dat, jako jsou obrázky, zvuk, video a text. Z tohoto důvodu obsáhlé učení rychle transformuje spoustu odvětví, včetně zdravotní péče, energie, financí a dopravy. Tyto obory nyní přemýšlení o tradičních obchodních procesech. 

Některé z nejběžnějších aplikací pro obsáhlý Learning jsou popsané v následujících odstavcích.

### <a name="named-entity-recognition"></a>Rozpoznávání pojmenovaných entit

Rozpoznávání pojmenovaných entit je metoda hloubkového učení, která přebírá text jako vstup a transformuje ho do předem určené třídy. Tato nová informace může být poštovním kódem, datum, ID produktu. Informace pak mohou být uloženy ve strukturovaném schématu, aby bylo možné sestavit seznam adres nebo sloužit jako srovnávací test pro modul ověřování identity.

### <a name="object-detection"></a>Detekce objektů

Obsáhlý Learning se použil v mnoha případech použití detekce objektů. Detekce objektu se skládá ze dvou částí: klasifikace obrázku a pak lokalizace imagí. _Klasifikace_ obrázku identifikuje objekty obrázku, například auta nebo lidi. _Lokalizace_ obrázků poskytuje konkrétní umístění těchto objektů. 

Detekce objektů se už používá v oborech, jako jsou hry, maloobchodní prodej, cestovní ruch a osobní automobily.

### <a name="image-caption-generation"></a>Generování titulku obrázku

Podobně jako rozpoznávání obrázku v titulkování obrázků pro daný obrázek systém musí vygenerovat titulek, který popisuje obsah obrázku. Když můžete objekty v fotografiích detekovat a označovat, je dalším krokem Změna popisků na popisné věty. 

Obvykle aplikace titulkování obrázků používají sítě neuronové konvoluční k identifikaci objektů v imagi a následné použití opakující se sítě neuronové k zapnutí popisků v konzistentních větách.

### <a name="machine-translation"></a>Strojový překlad

Strojový překlad používá slova nebo věty z jednoho jazyka a automaticky je překládá do jiného jazyka. Strojový překlad je delší dobu, ale obsáhlý Learning dosahuje působivých výsledků ve dvou konkrétních oblastech: automatický překlad textu (a převod řeči na text) a automatický překlad obrázků.

Díky příslušné transformaci dat může neuronové síť pochopit textové, zvukové a vizuální signály. Strojový překlad lze použít k identifikaci fragmentů zvuku ve větších zvukových souborech a přepisovat mluveného slova nebo obrázku jako textu.

### <a name="text-analytics"></a>Analýza textu

Analýza textu založená na metodách hloubkového učení zahrnuje analýzu velkých objemů textových dat (například příjem lékařských dokumentů nebo příjmů výdajů), rozpoznávání vzorů a vytváření uspořádaných a stručných informací.

Společnosti využívají k analýze textu v rámci hloubkového učení k detekci obchodování zasvěcených osob a dodržování předpisů pro státní správu. Dalším běžným příkladem je pojišťovací podvod: Text Analytics se často používá k analýze velkých objemů dokumentů, aby se zjistilo, že se poškodí pojistka pojistného pojištění. 

## <a name="artificial-neural-networks"></a>Umělé sítě neuronové

Umělé sítě neuronové se vytváří pomocí vrstev připojených uzlů. Modely hloubkového učení používají neuronové sítě, které mají velký počet vrstev. 

V následujících částech se prozkoumá nejoblíbenější umělá neuronové síť typologies.

### <a name="feedforward-neural-network"></a>Síť neuronové Feedforward

Síť neuronové Feedforward je nejzákladnější typ umělé neuronové sítě. V Feedforward síti se informace pohybují pouze v jednom směru ze vstupní vrstvy do výstupní vrstvy. Feedforward neuronové Networks transformují vstup tím, že ho umístí prostřednictvím řady skrytých vrstev. Každá vrstva se skládá ze sady neurons a každá vrstva je plně připojená ke všem neurons ve vrstvě. Poslední plně připojená vrstva (výstupní vrstva) představuje vygenerované předpovědi.

### <a name="recurrent-neural-network"></a>Znovu aktuální neuronové síť

Opakující se neuronové sítě jsou široce využívanou umělou neuronovéou síť. Tyto sítě ukládají výstup vrstvy a předává je zpět do vstupní vrstvy, aby bylo možné odhadnout výsledek vrstvy. Opakující se neuronové sítě mají skvělé možnosti učení. Jsou často používány pro komplexní úlohy, jako je například prognózování časových řad, učení rukopisu a rozpoznávání jazyka.

### <a name="convolutional-neural-networks"></a>Konvoluční neuronové sítě

Síť neuronové konvoluční je zvláště efektivní umělá síť neuronové a představuje jedinečnou architekturu. Vrstvy jsou uspořádány do tří rozměrů: šířka, Výška a hloubka. Neurons v jedné vrstvě se připojí k neurons v následující vrstvě, ale pouze do malých oblastí neurons vrstvy. Konečný výstup je snížen na jeden vektor skóre pravděpodobnosti uspořádané podél rozměru hloubky. 

Sítě konvoluční neuronové se používají v oblastech, jako je rozpoznávání videa, rozpoznávání obrázků a systémy doporučení.

## <a name="next-steps"></a>Další kroky

Následující články ukazují, jak používat technologii pro hloubkové učení v [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/?WT.mc_id=docs-article-lazzeri):

- [Klasifikace rukou psaných číslic pomocí modelu TensorFlow](https://docs.microsoft.com/azure/machine-learning/how-to-train-tensorflow?WT.mc_id=docs-article-lazzeri)

- [Klasifikace rukou psaných číslic pomocí TensorFlow Estimator a Keras](https://docs.microsoft.com/azure/machine-learning/how-to-train-keras?WT.mc_id=docs-article-lazzeri)

- [Klasifikace imagí pomocí modelu Pytorch](https://docs.microsoft.com/azure/machine-learning/how-to-train-pytorch?WT.mc_id=docs-article-lazzeri)

- [Klasifikace rukou psaných číslic pomocí modelu zřetězení](https://docs.microsoft.com/azure/machine-learning/how-to-train-ml-models)

K výběru algoritmů pro model použijte taky [tahákový list s algoritmem Machine Learning](algorithm-cheat-sheet.md) .
