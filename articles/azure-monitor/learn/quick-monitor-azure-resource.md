---
title: Monitorování prostředku Azure pomocí Azure Monitor
description: Naučte se shromažďovat a analyzovat data pro prostředek Azure v Azure Monitor.
ms. subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: 0e29b25f5d846cae1563ea90271cf007d02e248c
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87324263"
---
# <a name="quickstart-monitor-an-azure-resource-with-azure-monitor"></a>Rychlý Start: monitorování prostředku Azure pomocí Azure Monitor
[Azure monitor](../overview.md) spustí shromažďování dat z prostředků Azure okamžikem, kdy se vytváří. V tomto rychlém startu najdete Stručný návod k datům, která se automaticky shromažďují pro určitý prostředek, a o tom, jak je zobrazit v Azure Portal pro konkrétní prostředek. Později můžete přidat konfiguraci pro shromažďování dalších dat a můžete přejít do nabídky Azure Monitor a použít stejné nástroje pro přístup k datům shromážděným pro všechny prostředky v rámci vašeho předplatného.

Podrobnější popis monitorování dat shromážděných z prostředků Azure najdete v tématu [monitorování prostředků Azure pomocí Azure monitor](../insights/monitor-azure-resource.md).


## <a name="sign-in-to-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com). 


## <a name="overview-page"></a>Stránka Přehled
Mnoho služeb bude obsahovat data monitorování na stránce s **přehledem** jako rychlý přehled jejich provozu. Tato akce bude typicky založena na podmnožině metrik platforem uložených v Azure Monitorch metrik.

1. Vyhledejte prostředek Azure v rámci vašeho předplatného.
2. Přejít na stránku **Přehled** a poznamenejte si, jestli jsou zobrazená nějaká data o výkonu. Tato data poskytne Azure Monitor. Níže uvedený příklad je stránka s **přehledem** pro účet služby Azure Storage a uvidíte, že se zobrazuje více metrik.

    ![Stránka Přehled](media/quick-monitor-azure-resource/overview.png)

3. Kliknutím na kterýkoli z grafů otevřete data v Průzkumníkovi metrik, který je popsán níže.

## <a name="view-the-activity-log"></a>Zobrazit protokol aktivit
Protokol aktivit nabízí přehled o operacích u jednotlivých prostředků Azure v rámci předplatného. Tyto informace budou zahrnovat informace o tom, jakým způsobem dojde k vytvoření nebo úpravě prostředku, při spuštění úlohy nebo při určité operaci.

1. V horní části nabídky prostředku vyberte **Protokol aktivit**.
2. Aktuální filtr je nastaven na události související s vaším prostředkem. Pokud nevidíte žádné události, zkuste změnit časový **rozsah tak, že změníte** časový rozsah.

    ![Protokol aktivit](media/quick-monitor-azure-resource/activity-log-resource.png)

4. Pokud chcete zobrazit události z jiných prostředků v rámci vašeho předplatného, změňte kritéria ve filtru nebo dokonce odeberte vlastnosti filtru.

    ![Protokol aktivit](media/quick-monitor-azure-resource/activity-log-all.png)



## <a name="view-metrics"></a>Zobrazit metriky
Metriky jsou číselné hodnoty, které popisují určitý aspekt prostředku v určitou dobu. Azure Monitor automaticky shromažďuje metriky platforem v jednom minutovém intervalu ze všech prostředků Azure. Tyto metriky můžete zobrazit pomocí Průzkumníka metrik.

1. V části **monitorování** v nabídce prostředku vyberte možnost **metriky**. Otevře se Průzkumník metrik s oborem nastaveným na váš prostředek.
2. Kliknutím na **Přidat metriku** přidáte metriku do grafu.
   
   ![Průzkumník metrik](media/quick-monitor-azure-resource/metrics-explorer-01.png)
   
4. Vyberte **metriku** z rozevíracího seznamu a potom **agregaci**. To definuje, jak budou shromážděné hodnoty vzorkovat v každém časovém intervalu.

    ![Průzkumník metrik](media/quick-monitor-azure-resource/metrics-explorer-02.png)

5. Kliknutím na **Přidat metriku** přidejte do grafu další kombinace metriky a agregace.

    ![Průzkumník metrik](media/quick-monitor-azure-resource/metrics-explorer-03.png)



## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste si prohlíželi protokol aktivit a metriky pro prostředek Azure, které se automaticky shromažďují pomocí Azure Monitor. Pokračujte dalším rychlým startem, který vám ukáže, jak shromažďovat protokol aktivit do Log Analytics pracovního prostoru, kde je lze analyzovat pomocí [dotazů protokolu](../log-query/log-query-overview.md).

> [!div class="nextstepaction"]
> [Odeslání protokolu aktivit Azure do pracovního prostoru Log Analytics]()

