---
title: Jak monitorovat dostupnost clusteru pomocí Apache Ambari ve službě Azure HDInsight
description: Naučte se používat Apache Ambari k monitorování stavu a dostupnosti clusteru.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/01/2020
ms.openlocfilehash: 615e23dc388f36f5ae1cd7e0d846acc14ffa2236
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086411"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-in-azure-hdinsight"></a>Jak monitorovat dostupnost clusteru pomocí Apache Ambari ve službě Azure HDInsight

Clustery HDInsight zahrnují Apache Ambari, který poskytuje informace o stavu na první pohled a předdefinované výstrahy.

V tomto článku se dozvíte, jak pomocí Ambari monitorovat cluster a provedou několik příkladů konfigurace výstrahy Ambari, monitorování míry dostupnosti uzlu a vytvoření výstrahy Azure Monitor, která se aktivuje při přijetí prezenčního signálu z jednoho nebo více uzlů během pěti hodin.

## <a name="dashboard"></a>Řídicí panel

K řídicímu panelu Ambari se dostanete tak, že v části **řídicí panely clusteru** v tématu Přehled služby HDInsight v Azure Portal, jak vidíte níže, vyberete odkaz **Domů Ambari** . K němu můžete případně přejít tak, že přejdete do `https://CLUSTERNAME.azurehdinsight.net` prohlížeče, kde název_clusteru je název vašeho clusteru.

![Zobrazení portálu prostředků HDInsight](media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png)

Pak se zobrazí výzva k zadání uživatelského jména a hesla pro přihlášení ke clusteru. Zadejte přihlašovací údaje, které jste zvolili při vytváření clusteru.

Pak přejdete na řídicí panel Ambari, který obsahuje widgety, které znázorňují několik metriky, které vám poskytnou rychlý přehled o stavu clusteru HDInsight. Tyto pomůcky ukazují metriky, jako je počet aktivních datanodes (pracovních uzlů) a dostupných deníkových uzlů (Zookeeper Node), NameNodes (hlavní uzly) v čase a metriky specifické pro určité typy clusterů, jako je například doba provozu PŘÍZového správce pro clustery Spark a Hadoop.

![Apache Ambari použití zobrazení řídicího panelu](media/hdinsight-cluster-availability/apache-ambari-dashboard.png)

## <a name="hosts--view-individual-node-status"></a>Hostitelé – zobrazit stav jednotlivých uzlů

Můžete také zobrazit informace o stavu pro jednotlivé uzly. Výběrem karty **hostitelé** zobrazíte seznam všech uzlů v clusteru a zobrazíte základní informace o jednotlivých uzlech. Zelená šipka vlevo od každého názvu uzlu znamená, že všechny komponenty jsou v uzlu. Pokud je komponenta na uzlu mimo provoz, zobrazí se místo zelené kontroly červený trojúhelník.

![Zobrazení hostitelů pro HDInsight Apache Ambari](media/hdinsight-cluster-availability/apache-ambari-hosts1.png)

Pak můžete vybrat **název** uzlu a zobrazit tak podrobnější metriky hostitele pro daný uzel. Toto zobrazení ukazuje stav/dostupnost každé jednotlivé součásti.

![Apache Ambari hostuje jedno zobrazení uzlů](media/hdinsight-cluster-availability/apache-ambari-hosts-node.png)

## <a name="ambari-alerts"></a>Ambari výstrahy

Ambari také nabízí několik konfigurovatelných výstrah, které mohou poskytnout oznámení o určitých událostech. Když se aktivují výstrahy, zobrazují se v levém horním rohu Ambari červeným znakem, který obsahuje počet výstrah. Výběrem této příznaku se zobrazí seznam aktuálních výstrah.

![Počet aktuálních výstrah Apache Ambari](media/hdinsight-cluster-availability/apache-ambari-alerts.png)

Chcete-li zobrazit seznam definic výstrah a jejich stavů, vyberte kartu **výstrahy** , jak je uvedeno níže.

![Zobrazení definic upozornění Ambari](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari nabízí mnoho předdefinovaných výstrah souvisejících s dostupností, včetně:

| Název výstrahy                        | Description   |
|---|---|
| Shrnutí stavu datauzel           | Tato výstraha na úrovni služby se aktivuje, pokud existují chybné datauzly.|
| Stav vysoké dostupnosti NameNode | Tato výstraha na úrovni služby se aktivuje, pokud není spuštěný aktivní NameNode nebo pohotovostní NameNode.|
| Procento dostupných dostupných deníkových uzlů    | Tato výstraha se aktivuje, pokud je počet vypnutých dostupných deníkových uzlů v clusteru větší než nakonfigurovaná kritická prahová hodnota. Agreguje výsledky kontrol procesu JournalNode. |
| Procento dostupných datanode       | Tato výstraha se aktivuje, pokud je počet nefunkčních uzlů v clusteru větší než nakonfigurovaná kritická prahová hodnota. Agreguje výsledky kontrol procesů datanode.|

Úplný seznam upozornění Ambari, které vám pomůžou monitorovat dostupnost clusteru, najdete [tady](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui).

Chcete-li zobrazit podrobnosti výstrahy nebo upravit kritéria, vyberte **název** výstrahy. Jako příklad Vezměte v úvahu **souhrn stavu datauzel** . Můžete zobrazit popis výstrahy a také specifická kritéria, která aktivují upozornění "upozornění" nebo "kritická" a interval kontroly pro kritéria. Chcete-li upravit konfiguraci, vyberte tlačítko **Upravit** v pravém horním rohu pole konfigurace.

![Konfigurace upozornění Apache Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Tady můžete popis upravit a důležitější, což je důležitější interval a prahové hodnoty pro upozornění nebo kritické výstrahy.

![Zobrazení úprav konfigurace upozornění Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

V tomto příkladu můžete nastavit 2 chybné datauzly, které aktivují kritickou výstrahu, a 1 špatný datauzel aktivuje pouze upozornění. Po dokončení úprav vyberte **Uložit** .

## <a name="email-notifications"></a>E-mailová oznámení

Volitelně můžete také nakonfigurovat e-mailová oznámení pro Ambari výstrahy. Pokud to chcete provést, klikněte na kartě **výstrahy** v levém horním rohu na tlačítko **Akce** a potom na **spravovat oznámení.**

![Akce spravovat oznámení Ambari](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Otevře se dialogové okno pro správu oznámení výstrah. Vyberte v **+** dolní části dialogového okna a vyplňte požadovaná pole a poskytněte Ambari podrobnosti e-mailového serveru, ze kterých se mají posílat e-maily.

> [!TIP]
> Nastavení e-mailových oznámení Ambari může být dobrým způsobem, jak přijímat výstrahy na jednom místě při správě mnoha clusterů HDInsight.

## <a name="next-steps"></a>Další kroky

- [Dostupnost a spolehlivost clusterů Apache Hadoop v HDInsight](hdinsight-high-availability-linux.md)
- [Dostupnost clusterů – protokoly Azure Monitoru](./cluster-availability-monitor-logs.md)
- [Použití protokolů Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)
- [E-mailová oznámení Apache Ambari](apache-ambari-email.md)
