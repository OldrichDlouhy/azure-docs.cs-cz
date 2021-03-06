---
title: Vytvoření ukázkové aplikace v Azure Portal
titleSuffix: Azure Cognitive Search
description: Spusťte Průvodce vytvořením ukázkové aplikace (Preview) a vygenerujte HTML stránky a skript pro provozní webovou aplikaci. Stránka obsahuje panel hledání, oblast výsledků, postranní panel a podporu typeahead.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 07/01/2020
ms.openlocfilehash: c6ab5c2cae2bb966c2b040b40dbf36e56a54411b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496748"
---
# <a name="quickstart-create-a-demo-app-in-the-portal-azure-cognitive-search"></a>Rychlý Start: vytvoření ukázkové aplikace na portálu (Azure Kognitivní hledání)

Pomocí průvodce **vytvořením ukázkové aplikace** Azure Portal vygenerujte webovou aplikaci ve stylu "localhost", která běží v prohlížeči. V závislosti na konfiguraci je vygenerovaná aplikace při prvním použití funkční, s živým připojením jen pro čtení ke vzdálenému indexu. Výchozí aplikace může obsahovat panel hledání, oblast výsledků, filtry bočního panelu a podporu typeahead.

Ukázková aplikace vám pomůže vizualizovat, jak bude index fungovat v klientské aplikaci, ale není určený pro produkční scénáře. Klientské aplikace by měly zahrnovat zabezpečení, zpracování chyb a logiku hostování, kterou neposkytuje vygenerovaná stránka HTML. Až budete připraveni vytvořit klientskou aplikaci, přečtěte si téma [Vytvoření první aplikace pro vyhledávání pomocí sady .NET SDK](tutorial-csharp-create-first-app.md) pro další kroky.

## <a name="prerequisites"></a>Požadavky

Než začnete, musíte mít následující:

+ Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/).

+ Služba Azure Kognitivní hledání. [Vytvořte službu](search-create-service-portal.md) nebo [vyhledejte existující službu](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) v rámci aktuálního předplatného. Pro tento rychlý Start můžete použít bezplatnou službu. 

+ [Microsoft Edge (nejnovější verze)](https://www.microsoft.com/edge) nebo Google Chrome.

+ [Index hledání](search-what-is-an-index.md) , který se má použít jako základ vaší vygenerované aplikace 

  V tomto rychlém startu se používá integrovaná ukázková data a index reálného majetku, protože obsahuje miniatury (Průvodce podporuje přidávání imagí na stránku výsledků). Pokud chcete vytvořit index použitý v tomto cvičení, spusťte průvodce **importem dat** a vyberte zdroj dat *realestate-US-Sample* .

  ![Stránka zdroje dat pro ukázková data](media/search-create-app-portal/import-data-realestate.png)

Až bude index připravený k použití, přejděte k dalšímu kroku.

## <a name="start-the-wizard"></a>Spustit Průvodce

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) pomocí svého účtu Azure.

1. [Vyhledejte vyhledávací službu](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) a na stránce Přehled v odkazech uprostřed stránky vyberte **indexy**. 

1. V seznamu existujících indexů vyberte *realestate-US-Sample-index* .

1. V horní části stránky index vyberte **vytvořit ukázkovou aplikaci (Preview)** a spusťte tak průvodce.

1. Na první stránce průvodce vyberte **Povolit sdílení prostředků mezi zdroji (CORS)** a přidejte do definice indexu podporu CORS. Tento krok je nepovinný, ale vaše místní webová aplikace se nebude bez něj připojovat ke vzdálenému indexu.

## <a name="configure-search-results"></a>Konfigurace výsledků hledání

Průvodce poskytuje základní rozložení pro vykreslené výsledky hledání, které obsahují místo pro miniaturu obrázku, název a popis. Zálohování každého z těchto prvků je pole v indexu, které poskytuje data. 

1. V části Miniatura vyberte pole *Miniatura* v indexu *realestate-US-Sample* . Tato ukázka se stane zahrnutím miniatur obrázků do formátu obrázků adres URL uložených v poli s názvem *Miniatura*. Pokud váš index neobsahuje obrázky, ponechte toto pole prázdné.

1. V části název vyberte pole, které vyjadřuje jedinečnost každého dokumentu. V této ukázce je ID výpisu rozumného výběru.

1. V poli Popis zvolte pole, které poskytuje podrobné informace, které mohou někomu pomáhat při rozhodování, zda kliknout do konkrétního dokumentu.

   ![Stránka zdroje dat pro ukázková data](media/search-create-app-portal/configure-results.png)

## <a name="add-a-sidebar"></a>Přidat postranní panel

Vyhledávací služba podporuje omezující navigaci, která se často vykresluje jako postranní panel. Omezující vlastnosti jsou založené na filtrovacích a tváře polích, jak je vyjádřeno ve schématu indexu.

V Azure Kognitivní hledání je nahodnocená navigace kumulativním prostředím pro filtrování. Výběr více filtrů v rámci kategorie rozšiřuje výsledky (například výběr Praha a Bellevue v rámci města). V různých kategoriích výběr více filtrů zužuje výsledky.

> [!TIP]
> Úplné schéma indexu můžete zobrazit na portálu. Vyhledejte odkaz **definice indexu (JSON)** na stránce Přehled v jednotlivých indexech. Pole, která jsou kvalifikována pro přecházení s omezujícími vlastnostmi, mají "filtrovatelné: true" a "FACET: true" atributy.

Přijměte aktuální výběr omezujících vlastností a pokračujte na další stránku.


## <a name="add-typeahead"></a>Přidat typeahead

Funkce typeahead je k dispozici ve formě automatického dokončování a návrhů dotazů. Průvodce podporuje návrhy dotazů. Na základě vstupů z klávesnice, které poskytuje uživatel, služba Search vrátí seznam "dokončených" řetězců dotazů, které je možné vybrat jako vstup.

Návrhy jsou povolené pro konkrétní definice polí. Průvodce vám nabídne možnosti konfigurace, kolik informací je součástí návrhu. 

Následující snímek obrazovky ukazuje možnosti v průvodci, juxtaposed s vykreslenou stránkou v aplikaci. Můžete vidět, jak se používají výběry polí, a jak se má v návrhu zahrnout nebo vyloučit označení "Zobrazit název pole".

![Konfigurace návrhů dotazů](media/search-create-app-portal/suggestions.png)

## <a name="create-download-and-execute"></a>Vytvoření, stažení a spuštění

1. Vyberte **vytvořit ukázkovou aplikaci** pro vygenerování souboru HTML.

1. Po zobrazení výzvy vyberte **Stáhnout aplikaci** a Stáhněte si soubor.

1. Otevřete tento soubor. Měla by se zobrazit stránka podobná následujícímu snímku obrazovky. Zadejte termín a použijte filtry k zúžení výsledků. 

Základní index se skládá z fiktivních generovaných dat, která byla duplikována v rámci dokumentů, a popisy se někdy neshodují s obrázkem. Pokud vytváříte aplikaci na základě vlastních indexů, můžete očekávat ucelenější prostředí.

![Spuštění aplikace](media/search-create-app-portal/run-app.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud pracujete s vlastním předplatným, je vhodné vždy na konci projektu zkontrolovat, jestli budete vytvořené prostředky ještě potřebovat. Prostředky, které necháte běžet, vás stojí peníze. Prostředky můžete odstraňovat jednotlivě nebo můžete odstranit skupinu prostředků a odstranit tak celou sadu prostředků najednou.

Prostředky můžete najít a spravovat na portálu pomocí odkazu **všechny prostředky** nebo **skupiny prostředků** v levém navigačním podokně.

Pokud používáte bezplatnou službu, pamatujte na to, že jste omezeni na tři indexy, indexery a zdroje dat. Jednotlivé položky na portálu můžete odstranit, aby zůstaly pod limitem. 

## <a name="next-steps"></a>Další kroky

I když je výchozí aplikace užitečná při počátečním a malém úkolu, podrobnější informace o rozhraních API vám pomůžou porozumět konceptům a pracovním postupům na hlubší úrovni:

> [!div class="nextstepaction"]
> [Vytvoření indexu pomocí sady .NET SDK](https://docs.microsoft.com/azure/search/search-create-index-dotnet)