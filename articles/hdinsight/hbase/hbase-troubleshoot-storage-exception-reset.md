---
title: Výjimka úložiště po resetování připojení ve službě Azure HDInsight
description: Výjimka úložiště po resetování připojení ve službě Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: a7af6407191577112f936bfb9048985e85c868ea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75887219"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Scénář: výjimka úložiště po resetování připojení ve službě Azure HDInsight

Tento článek popisuje postup řešení potíží a možná řešení potíží při komunikaci s clustery Azure HDInsight.

## <a name="issue"></a>Problém

Nepovedlo se vytvořit novou tabulku Apache HBA.

## <a name="cause"></a>Příčina

Během procesu zkracování tabulky došlo k potížím s připojením úložiště. Položka tabulky byla odstraněna v tabulce metadat HBA. Byl odstraněn veškerý soubor objektu blob, ale jeden.

I když se v úložišti nevolal žádný objekt BLOB složky `/hbase/data/default/ThatTable` . Ovladač WASB zjistil existenci výše uvedeného souboru objektu BLOB a neumožňuje vytvořit žádný objekt BLOB s názvem `/hbase/data/default/ThatTable` , protože předpokládá, že existovaly nadřazené složky, takže vytvoření tabulky se nezdaří.

## <a name="resolution"></a>Řešení

1. V uživatelském rozhraní Apache Ambari restartujte aktivní HMaster. To umožní, aby se jeden ze dvou úspor v pohotovostním HMaster stal aktivním a nový aktivní HMaster znovu nasadí informace o tabulce metadat. Proto se `already-deleted` tabulka v uživatelském rozhraní HMaster nezobrazí.

1. Osamocený soubor blob můžete najít z nástrojů uživatelského rozhraní, jako je Průzkumník cloudu nebo spuštění příkazu `hdfs dfs -ls /xxxxxx/yyyyy` . Spuštěním `hdfs dfs -rmr /xxxxx/yyyy` odstraňte tento objekt BLOB. Například, `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Nyní můžete vytvořit novou tabulku se stejným názvem v okně HBA.

## <a name="next-steps"></a>Další kroky

Pokud jste se nedostali k problému nebo jste nedokázali problém vyřešit, přejděte k jednomu z následujících kanálů, kde najdete další podporu:

* Získejte odpovědi od odborníků na Azure prostřednictvím [podpory komunity Azure](https://azure.microsoft.com/support/community/).

* Připojte se k [@AzureSupport](https://twitter.com/azuresupport) oficiálnímu Microsoft Azuremu účtu pro zlepšení prostředí pro zákazníky. Propojování komunity Azure se správnými zdroji informací: odpovědi, podpora a odborníci.

* Pokud potřebujete další pomoc, můžete odeslat žádost o podporu z [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). V řádku nabídek vyberte **Podpora** a otevřete centrum pro **pomoc a podporu** . Podrobnější informace najdete v tématu [jak vytvořit žádost o podporu Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Přístup ke správě předplatných a fakturační podpoře jsou součástí vašeho předplatného Microsoft Azure a technická podpora je poskytována prostřednictvím některého z [plánů podpory Azure](https://azure.microsoft.com/support/plans/).
