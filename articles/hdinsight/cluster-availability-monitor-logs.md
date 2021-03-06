---
title: Jak monitorovat dostupnost clusteru pomocí protokolů Azure Monitor v HDInsight
description: Naučte se používat protokoly Azure Monitor k monitorování stavu a dostupnosti clusteru.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 05/01/2020
ms.openlocfilehash: 25bda7ed94eef20e22bcf717780d08a3ea5e6521
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077214"
---
# <a name="how-to-monitor-cluster-availability-with-azure-monitor-logs-in-hdinsight"></a>Jak monitorovat dostupnost clusteru pomocí protokolů Azure Monitor v HDInsight

Clustery HDInsight zahrnují integraci protokolů Azure Monitor, která poskytuje metriky a protokoly Queryable a také konfigurovatelné výstrahy. V tomto článku se dozvíte, jak pomocí Azure Monitor monitorovat cluster.

## <a name="azure-monitor-logs-integration"></a>Integrace protokolů Azure Monitor

Protokoly Azure Monitor umožňují shromažďování a agregaci dat vygenerovaných několika prostředky, jako jsou clustery HDInsight, a jejich shromáždění a agregace na jednom místě, abyste dosáhli sjednoceného prostředí monitorování.

Za předpokladu budete potřebovat Log Analytics pracovní prostor pro ukládání shromážděných dat. Pokud jste ho ještě nevytvořili, můžete postupovat podle pokynů zde: [Vytvoření pracovního prostoru Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Povolit integraci protokolů Azure Monitor HDInsight

Na stránce prostředek clusteru HDInsight na portálu vyberte **Azure monitor**. Pak vyberte **Povolit** a v rozevíracím seznamu vyberte svůj pracovní prostor Log Analytics.

![Sada HDInsight Operations Management Suite](media/cluster-availability-monitor-logs/azure-portal-monitoring.png)

## <a name="query-metrics-and-logs-tables"></a>Tabulky metrik a protokolů dotazů

Jakmile je integrace protokolu Azure Monitor povolená (může to trvat několik minut), přejděte do prostředku **pracovního prostoru Log Analytics** a vyberte **protokoly**.

![Protokoly Log Analytics pracovního prostoru](media/cluster-availability-monitor-logs/hdinsight-portal-logs.png)

Protokoluje seznam několika ukázkových dotazů, například:

| Název dotazu                      | Description                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Dostupnost počítačů v dnešní době    | Graf počtu počítačů odesílajících protokoly, každou hodinu                     |
| Zobrazit prezenční signály                 | Zobrazit seznam všech prezenčních signálů počítače za poslední hodinu                           |
| Poslední prezenční signál každého počítače | Zobrazit poslední prezenční signál odeslaný jednotlivými počítači                             |
| Nedostupné počítače           | Zobrazí seznam všech známých počítačů, které neodeslaly prezenční signál za posledních 5 hodin. |
| Míra dostupnosti               | Vypočítat míru dostupnosti každého připojeného počítače                |

Jako příklad spusťte dotaz ukázka **míry dostupnosti** tak, že v tomto dotazu vyberete **Spustit** , jak je znázorněno na snímku obrazovky výše. Tím se v procentech zobrazí míra dostupnosti jednotlivých uzlů v clusteru. Pokud jste povolili více clusterů HDInsight, aby odesílaly metriky do stejného pracovního prostoru Log Analytics, zobrazí se v těchto clusterech míra dostupnosti pro všechny uzly.

![Vzorový dotaz k protokolu dostupnosti v pracovním prostoru Log Analytics](media/cluster-availability-monitor-logs/portal-availability-rate.png)

> [!NOTE]  
> Míra dostupnosti se měří v průběhu 24 hodin, takže cluster bude muset běžet aspoň 24 hodin, než uvidíte správné sazby dostupnosti.

Tuto tabulku můžete připnout na sdílený řídicí panel tak, že kliknete na **připnout** v pravém horním rohu. Pokud nemáte žádné zapisovatelné řídicí panely, uvidíte, jak ho vytvořit: [vytváření a sdílení řídicích panelů v Azure Portal](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard).

## <a name="azure-monitor-alerts"></a>Výstrahy Azure Monitor

Můžete také nastavit výstrahy Azure Monitor, které se aktivují, když hodnota metriky nebo výsledky dotazu splňují určité podmínky. Můžete například vytvořit upozornění k odeslání e-mailu, když jeden nebo více uzlů neodeslal prezenční signál během 5 hodin (tzn. že se předpokládá, že není k dispozici).

V části **protokoly**Spusťte ukázkový dotaz **nedostupných počítačů** , a to tak, že v tomto dotazu vyberete **Spustit** , jak je znázorněno níže.

![Ukázka Log Analytics v pracovním prostoru zaznamenává nedostupné počítače](media/cluster-availability-monitor-logs/portal-unavailable-computers.png)

Pokud jsou k dispozici všechny uzly, tento dotaz by nyní měl vracet nulový výsledek. Kliknutím na **nové pravidlo výstrahy** zahajte konfiguraci upozornění pro tento dotaz.

![Nové pravidlo výstrahy Log Analytics pracovního prostoru](media/cluster-availability-monitor-logs/portal-logs-new-alert-rule.png)

Existují tři komponenty výstrahy: *prostředek* , pro který chcete vytvořit pravidlo (Log Analytics pracovní prostor v tomto případě), *podmínku* pro aktivaci výstrahy a *skupiny akcí* , které určují, co se stane, když se výstraha aktivuje.
Klikněte na **název podmínky**, jak je znázorněno níže, a dokončete konfiguraci logiky signálu.

![Podmínka vytvoření pravidla pro upozornění na portál](media/cluster-availability-monitor-logs/portal-condition-title.png)

Tím se otevře **Konfigurace signálu**.

Nastavte oddíl **Alert Logic** následujícím způsobem:

*Podle: počet výsledků, podmínka: větší než, prahová hodnota: 0.*

Vzhledem k tomu, že tento dotaz vrátí nedostupné uzly jako výsledky, pokud je počet výsledků stále větší než 0, výstraha by se měla aktivovat.

V části **vyhodnocováno na základě** oddílu nastavte **dobu** a **četnost** podle toho, jak často chcete kontrolovat nedostupné uzly.

Pro účely této výstrahy se chcete ujistit, že **perioda = frekvence.** Další informace o období, četnosti a dalších parametrech výstrahy najdete [tady](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Vyberte **Hotovo** , až budete hotovi s konfigurací logiky signálu.

![Pravidlo výstrahy konfiguruje logiku signálu](media/cluster-availability-monitor-logs/portal-configure-signal-logic.png)

Pokud ještě nemáte existující skupinu akcí, klikněte na **vytvořit nový** v části **skupiny akcí** .

![Pravidlo výstrahy vytvoří novou skupinu akcí.](media/cluster-availability-monitor-logs/portal-create-new-action-group.png)

Otevře se okno **Přidat skupinu akcí**. Vyberte **název skupiny akcí**, **krátké jméno**, **předplatné**a **skupinu prostředků.** V části **Akce** zvolte **název akce** a jako **typ akce** vyberte **e-mail/SMS/odeslat/hlas** .

> [!NOTE]
> K dispozici je několik dalších akcí, které se můžou aktivovat kromě e-mailu, SMS/nabízeného/hlasu, jako je Azure Functions, LogicApp, Webhook, ITSM a Automation Runbook. [Víc se uč.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Tím se otevře **e-mail/SMS/nabízený/hlas**. Vyberte **jméno** příjemce, **zaškrtněte** políčko **e-mail** a zadejte e-mailovou adresu, na kterou chcete odeslat výstrahu. V **e-mailu/SMS/Push/Voice**vyberte **OK** a potom do **Přidat skupinu akcí** dokončete konfiguraci skupiny akcí.

![Pravidlo výstrahy vytvoří přidat skupinu akcí.](media/cluster-availability-monitor-logs/portal-add-action-group.png)

Po zavření těchto oken by se měla zobrazit vaše skupina akcí uvedená v části **skupiny akcí** . Nakonec dokončete část **Podrobnosti výstrahy** zadáním názvu a **popisu** **pravidla výstrahy** a zvolením **závažnosti**. Kliknutím na **vytvořit pravidlo výstrahy** dokončete.

![Portál vytvoří dokončení pravidla výstrahy.](media/cluster-availability-monitor-logs/portal-create-alert-rule-finish.png)

> [!TIP]
> Možnost určit **závažnost** je výkonný nástroj, který se dá použít při vytváření více výstrah. Můžete například vytvořit jednu výstrahu, která vyvolá upozornění (závažnost 1) v případě, že dojde k výpadku jednoho hlavního uzlu, a další výstrahu, která vyvolává kritické (závažnost 0) v nepravděpodobném případě, že oba hlavní uzly vycházejí dolů.

Pokud je splněna podmínka této výstrahy, výstraha se aktivuje a obdržíte e-mail s podrobnostmi výstrahy, jako je tato:

![Příklad e-mailu s výstrahou Azure Monitor](media/cluster-availability-monitor-logs/portal-oms-alert-email.png)

Kliknutím na **výstrahy** v **pracovním prostoru Log Analytics**můžete zobrazit také všechny výstrahy, které byly aktivovány, seskupené podle závažnosti.

![Výstrahy Log Analytics pracovního prostoru](media/cluster-availability-monitor-logs/hdi-portal-oms-alerts.png)

Výběr u seskupení závažnosti (tj. **závažnost 1, jak je** zvýrazněný výše) zobrazí záznamy pro všechny výstrahy této závažnosti, které byly aktivovány níže:

![Log Analytics pracovní prostor závažnost jednu výstrahu](media/cluster-availability-monitor-logs/portal-oms-alerts-sev1.png)

## <a name="next-steps"></a>Další kroky

* [Dostupnost clusteru – Apache Ambari](./hdinsight-cluster-availability.md)
* [Použití protokolů Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)
