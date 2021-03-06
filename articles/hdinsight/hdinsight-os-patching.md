---
title: Konfigurace plánu oprav operačního systému pro clustery Azure HDInsight
description: Naučte se konfigurovat plán oprav operačního systému pro clustery HDInsight se systémem Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: ddc70ccbbb5c964f16b078470517ce667bc878f1
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86082637"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurace plánu oprav operačního systému pro clustery HDInsight se systémem Linux

> [!IMPORTANT]
> Image Ubuntu budou k dispozici pro nové vytvoření clusteru Azure HDInsight do tří měsíců od jejich publikování. Od ledna 2019 nejsou spuštěné clustery automaticky opraveny. Pokud chtějí zákazníci opravit běžící cluster, musí použít akce skriptu nebo jiné mechanismy. Nově vytvořené clustery budou mít vždycky nejnovější dostupné aktualizace, včetně nejnovějších oprav zabezpečení.

HDInsight poskytuje podporu pro provádění běžných úloh v clusteru, jako je například instalace oprav operačního systému, aktualizace zabezpečení a restartování uzlů. Tyto úlohy se provádějí pomocí následujících dvou skriptů, které se dají spustit jako [akce skriptu](hdinsight-hadoop-customize-cluster-linux.md), a nakonfigurují se pomocí parametrů:

- `schedule-reboots.sh`– Proveďte okamžité restartování nebo naplánujte restart na uzlech clusteru.
- `install-updates-schedule-reboots.sh`– Nainstaluje všechny aktualizace, jenom aktualizace jádra a zabezpečení, nebo jenom aktualizace jádra.

> [!NOTE]  
> Akce skriptu nebudou automaticky používat aktualizace pro všechny budoucí aktualizační cykly. Spusťte skripty pokaždé, když se musí použít nové aktualizace, aby se nainstalovaly aktualizace, a pak se virtuální počítač restartuje.

## <a name="preparation"></a>Příprava

Oprava v reprezentativním neprodukčním prostředí před nasazením do produkčního prostředí. Vytvořte plán pro odpovídající testování vašeho systému před skutečným opravou.

Od času po dobu od relace SSH s vaším clusterem se může zobrazit zpráva, že je k dispozici upgrade. Zpráva může vypadat nějak takto:

```
New release '18.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade it
```

Oprava je volitelná a podle vašeho uvážení.

## <a name="restart-nodes"></a>Restartovat uzly
  
Plán skriptu [–](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh)restartování, nastaví typ restartování, který bude proveden na počítačích v clusteru. Při odesílání akce skriptu nastavte, aby se nastavily na všech třech typech uzlů: hlavní uzel, pracovní uzel a Zookeeper. Pokud se skript nepoužije na typ uzlu, virtuální počítače pro tento typ uzlu se neaktualizují ani nerestartují.

`schedule-reboots script`Přijímá jeden číselný parametr:

| Parametr | Přípustné hodnoty | Definice |
| --- | --- | --- |
| Typ restartování, který se má provést | 1 nebo 2 | Hodnota 1 povolí restart plánu (naplánováno během 12-24 hodin). Hodnota 2 umožňuje okamžité restartování (5 minut). Pokud není zadán žádný parametr, výchozí hodnota je 1. |  

## <a name="install-updates-and-restart-nodes"></a>Instalovat aktualizace a restartovat uzly

Skript [install-Updates-Schedule-Reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) poskytuje možnosti pro instalaci různých typů aktualizací a restartování virtuálního počítače.

`install-updates-schedule-reboots`Skript přijímá dva číselné parametry, jak je popsáno v následující tabulce:

| Parametr | Přípustné hodnoty | Definice |
| --- | --- | --- |
| Typ aktualizací, které se mají nainstalovat | 0, 1 nebo 2 | Hodnota 0 nainstaluje pouze aktualizace jádra. Hodnota 1 nainstaluje všechny aktualizace a 2 nainstaluje pouze aktualizace jádra a zabezpečení. Pokud není zadán žádný parametr, výchozí hodnota je 0. |
| Typ restartování, který se má provést | 0, 1 nebo 2 | Hodnota 0 zakáže restart. Hodnota 1 umožňuje naplánovat restart a 2 umožňuje okamžité restartování. Pokud není zadán žádný parametr, výchozí hodnota je 0. Uživatel musí změnit vstupní parametr 1 na vstupní parametr 2. |

> [!NOTE]
> Po použití v existujícím clusteru musíte označit skript jako trvalý. Jinak budou všechny nové uzly vytvořené prostřednictvím operací škálování používat výchozí plán oprav. Použijete-li skript jako součást procesu vytváření clusteru, bude automaticky trvalý.

## <a name="next-steps"></a>Další kroky

Konkrétní kroky týkající se použití akcí skriptů najdete v následujících částech v tématu [Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md):

- [Použití akce skriptu během vytváření clusteru](hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)
- [Použití akce skriptu u běžícího clusteru](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
