---
title: Vizualizace protokolů a metrik služby Azure Kognitivní hledání pomocí Power BI
description: Vizualizace protokolů a metrik služby Azure Kognitivní hledání pomocí Power BI
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: d529e4e4b79f0c34eb8f95b028cca730b316d5f6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87060303"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Vizualizace protokolů a metrik služby Azure Kognitivní hledání pomocí Power BI
[Azure kognitivní hledání](https://docs.microsoft.com/azure/search/search-what-is-azure-search) umožňuje ukládat protokoly operací a metriky služeb o vaší vyhledávací službě v účtu Azure Storage. Tato stránka poskytuje pokyny k vizualizaci těchto informací prostřednictvím aplikace Power BI Template. Aplikace poskytuje podrobné přehledy o vaší vyhledávací službě, včetně informací o vyhledávání, indexování, operacích a metrikách služeb.

Můžete najít Power BI App template **kognitivní hledání Azure: Analýza protokolů a metrik** na [tržišti aplikací pro Power BI](https://appsource.microsoft.com/marketplace/apps).

## <a name="how-to-get-started-with-the-app"></a>Jak začít s aplikací

1. Povolit protokolování metrik a prostředků pro vyhledávací službu:

    1. Umožňuje vytvořit nebo identifikovat existující [účet Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) , do kterého můžete archivovat protokoly.
    1. Přejděte do služby Azure Kognitivní hledání v Azure Portal
    1. V části monitorování v levém sloupci vyberte **nastavení diagnostiky** .

        ![Snímek obrazovky ukazující, jak vybrat nastavení diagnostiky v části Monitorování služby Azure Kognitivní hledání.](media/search-monitor-logs-powerbi/diagnostic-settings.png)

    1. Vybrat **+ Přidat nastavení diagnostiky**
    1. Zaregistrujte se **do účtu úložiště**, zadejte informace o svém účtu úložiště a pak ověřte **OperationLogs** a **AllMetrics** .

        ![Snímek obrazovky ukazující, jak provést výběry pro metriky a protokolování prostředků na stránce nastavení diagnostiky.](media/search-monitor-logs-powerbi/add-diagnostic-setting.png)
    1. Vyberte **Uložit**.

1. Po povolení protokolování můžete pomocí vyhledávací služby začít generovat protokoly a metriky. Může to trvat až hodinu, než se kontejnery zobrazí v úložišti objektů BLOB v těchto protokolech. Zobrazí se kontejner **Insights-logs-operationlogs** pro protokoly pro vyhledávání a kontejner Insights- **Metrics-pt1m** pro metriky.

1. Najděte aplikaci Azure Kognitivní hledání Power BI na [webu tržiště aplikací pro Power BI](https://appsource.microsoft.com/marketplace/apps) a nainstalujte ji do nového nebo existujícího pracovního prostoru. Aplikace se nazývá **Azure kognitivní hledání: Analýza protokolů a metrik**.

1. Po instalaci aplikace vyberte aplikaci ze seznamu aplikací v Power BI.

    ![Snímek obrazovky s aplikací Azure Kognitivní hledání, kterou chcete vybrat ze seznamu aplikací](media/search-monitor-logs-powerbi/azure-search-app-tile.png)

1. Vyberte **připojit** a připojte data.

    ![Snímek obrazovky, který ukazuje, jak se připojit k datům v aplikaci Azure Kognitivní hledání.](media/search-monitor-logs-powerbi/get-started-with-your-new-app.png)

1. Zadejte název účtu úložiště, který obsahuje vaše protokoly a metriky. Ve výchozím nastavení se aplikace bude považovat za posledních 10 dnů, ale tato hodnota se dá změnit s parametrem **dny** .

    ![Snímek obrazovky ukazující, jak zadat název účtu úložiště a počet dní k dotazování na stránce připojit k Azure Kognitivní hledání.](media/search-monitor-logs-powerbi/connect-to-storage-account.png)

1. Jako metodu ověřování vyberte **klíč** a zadejte svůj klíč účtu úložiště. Jako úroveň ochrany osobních údajů vyberte **soukromá** . Klikněte na Přihlásit se a zahajte proces načítání.

    ![Snímek obrazovky ukazující, jak zadat metodu ověřování, klíč účtu a úroveň ochrany osobních údajů na stránce připojit k Azure Kognitivní hledání.](media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png)

1. Počkejte, až se data aktualizují. To může nějakou dobu trvat v závislosti na tom, kolik dat máte. Můžete zjistit, zda se data stále aktualizují na základě níže uvedeného indikátoru.

    ![Snímek obrazovky ukazující, jak číst informace na stránce pro aktualizaci dat](media/search-monitor-logs-powerbi/workspace-view-refreshing.png)

1. Po dokončení aktualizace dat vyberte **sestavu Azure kognitivní hledání** a zobrazte sestavu.

    ![Snímek obrazovky ukazující, jak vybrat sestavu Azure Kognitivní hledání na stránce aktualizace dat](media/search-monitor-logs-powerbi/workspace-view-select-report.png)

1. Po otevření sestavy nezapomeňte stránku aktualizovat, aby se otevírala s daty.

    ![Snímek obrazovky Power BI sestavy služby Azure Kognitivní hledání.](media/search-monitor-logs-powerbi/powerbi-search.png)

## <a name="how-to-change-the-app-parameters"></a>Jak změnit parametry aplikace
Pokud chcete vizualizovat data z jiného účtu úložiště nebo změnit počet dnů, na které se má dotaz zadat dotaz, změňte parametry **dnů** a **StorageAccount** podle následujících kroků.

1. Přejděte do aplikací Power BI, Najděte aplikaci Azure Kognitivní hledání a kliknutím na tlačítko **Upravit aplikaci** zobrazíte pracovní prostor.

    ![Snímek obrazovky ukazující, jak vybrat tlačítko Upravit aplikaci pro aplikaci Azure Kognitivní hledání](media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png)

1. V možnostech datové sady vyberte **Nastavení** .

    ![Snímek obrazovky ukazující, jak vybrat nastavení z možností datové sady vyhledávací služby Azure Congnitive](media/search-monitor-logs-powerbi/workspace-view-select-settings.png)

1. Na kartě datové sady změňte hodnoty parametrů a vyberte **použít**. Pokud dojde k potížím s připojením, aktualizujte přihlašovací údaje ke zdroji dat na stejné stránce.

1. Přejděte zpátky do pracovního prostoru a vyberte **aktualizovat hned** z možností datové sady.

    ![Snímek obrazovky ukazující, jak vybrat aktualizovat z možností datové sady Azure Congnitive Search](media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png)

1. Otevřete sestavu pro zobrazení aktualizovaných dat. Je také možné, že budete muset aktualizovat sestavu, aby se zobrazila nejnovější data.

## <a name="troubleshooting"></a>Poradce při potížích
Pokud zjistíte, že se data nezobrazí, postupujte podle těchto kroků pro řešení potíží:

1. Otevřete sestavu a aktualizujte stránku, abyste měli jistotu, že si prohlížíte nejnovější data. V sestavě je možnost aktualizovat data. Tuto možnost vyberte, pokud chcete získat nejnovější data.

1. Ujistěte se, že zadaný název účtu úložiště a přístupový klíč jsou správné. Název účtu úložiště by měl odpovídat účtu nakonfigurovanému s protokoly služby Search.

1. Ujistěte se, že váš účet úložiště obsahuje kontejnery **Insights-logs-operationlogs** a **Insights-Metrics-pt1m** a každý kontejner obsahuje data. Protokoly a metriky budou v rámci několika vrstev složek.

1. Zkontrolujte, jestli se datová sada ještě neaktualizovala. Indikátor stavu aktualizace se zobrazí v kroku 8 výše. Pokud se stále aktualizuje, počkejte, až se aktualizace dokončí, aby se tato sestava otevřela a aktualizovala.

## <a name="next-steps"></a>Další kroky
[Další informace o Azure Kognitivní hledání](https://docs.microsoft.com/azure/search/)

[Co je Power BI?](https://docs.microsoft.com/power-bi/fundamentals/power-bi-overview)

[Základní pojmy pro návrháře ve službě Power BI](https://docs.microsoft.com/power-bi/service-basic-concepts)