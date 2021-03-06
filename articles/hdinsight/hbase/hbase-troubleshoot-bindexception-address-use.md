---
title: BindException – adresa se už používá ve službě Azure HDInsight.
description: BindException – adresa se už používá ve službě Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 80f984643d6d8be88b381881c6fc1cb1cb5f1815
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75887338"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Scénář: BindException-Address se už používá ve službě Azure HDInsight.

Tento článek popisuje postup řešení potíží a možná řešení potíží při komunikaci s clustery Azure HDInsight.

## <a name="issue"></a>Problém

Operaci restartování na serveru oblasti Apache Hbas se nepodařilo dokončit. V `region-server.log` adresáři v `/var/log/hbase` pracovních uzlech, kde se nepovede Server oblasti Start, se může zobrazit chybová zpráva podobná následující:

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Příčina

Restartování serverů oblastí Apache HBA během náročné aktivity úlohy. Níže najdete informace o tom, co se stane na pozadí, když uživatel zahájí operaci restartování na Hbach serveru oblasti z uživatelského rozhraní Apache Ambari:

1. Agent Ambari odešle požadavek na zastavení na server oblasti.

1. Agent Ambari počká 30 sekund, aby se server oblasti mohl řádně vypnout.

1. Pokud se vaše aplikace nadále připojuje k serveru oblastí, server se nevypne hned. Časový limit 30 sekund vyprší, než dojde k vypnutí.

1. Po 30 sekundách pošle agent Ambari na server oblasti příkaz Force-Kill ( `kill -9` ).

1. Z důvodu tohoto náhlého vypnutí se i když proces serveru oblasti ukončí, port přidružený k procesu se nemusí uvolnit, což nakonec vede k tomu `AddressBindException` .

## <a name="resolution"></a>Řešení

Před zahájením restartování snižte zatížení serverů oblastí HBA. Je také vhodné nejprve vyprázdnit všechny tabulky. Referenční informace o tom, jak vyprázdnit tabulky, najdete v tématu [HDInsight hbas: jak zlepšit dobu restartování clusteru Apache HBA vyprázdněním tabulek](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Případně se pokuste ručně restartovat servery oblastí na pracovních uzlech pomocí následujících příkazů:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Další kroky

Pokud jste se nedostali k problému nebo jste nedokázali problém vyřešit, přejděte k jednomu z následujících kanálů, kde najdete další podporu:

* Získejte odpovědi od odborníků na Azure prostřednictvím [podpory komunity Azure](https://azure.microsoft.com/support/community/).

* Připojte se k [@AzureSupport](https://twitter.com/azuresupport) oficiálnímu Microsoft Azuremu účtu pro zlepšení prostředí pro zákazníky. Propojování komunity Azure se správnými zdroji informací: odpovědi, podpora a odborníci.

* Pokud potřebujete další pomoc, můžete odeslat žádost o podporu z [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). V řádku nabídek vyberte **Podpora** a otevřete centrum pro **pomoc a podporu** . Podrobnější informace najdete v tématu [jak vytvořit žádost o podporu Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Přístup ke správě předplatných a fakturační podpoře jsou součástí vašeho předplatného Microsoft Azure a technická podpora je poskytována prostřednictvím některého z [plánů podpory Azure](https://azure.microsoft.com/support/plans/).
