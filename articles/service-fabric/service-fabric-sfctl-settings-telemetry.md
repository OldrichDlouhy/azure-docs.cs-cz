---
title: Azure Service Fabric CLI – telemetrie nastavení sfctl
description: Přečtěte si o sfctl rozhraní příkazového řádku Azure Service Fabric. Obsahuje seznam příkazů pro konfiguraci telemetrie sfctl.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: ef720a14617b4131474d50875701d0ef27df4151
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86245499"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Nakonfigurujte nastavení telemetrie jako místní pro tuto instanci sfctl.

Telemetrie Sfctl shromažďuje název příkazu bez zadaných parametrů nebo jejich hodnot, verze Sfctl, typ operačního systému, verze Pythonu, úspěch nebo neúspěch příkazu, vrácená chybová zpráva.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
| sada-telemetrie | Zapnutí nebo vypnutí telemetrie. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sada telemetrie nastavení sfctl – telemetrie
Zapnutí nebo vypnutí telemetrie.

### <a name="arguments"></a>Arguments

|Argument|Popis|
| --- | --- |
| --vypnuto | Vypněte telemetrii. |
| --zapnuto | Zapnout telemetrii. Toto je výchozí hodnota. |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --ladění | Zvyšte úroveň podrobností protokolování, aby se zobrazily všechny protokoly ladění. |
| --Help-h | Zobrazí tuto zprávu s upozorněním a ukončí. |
| --výstup-o | Výstupní formát.  Povolené hodnoty \: : JSON, jsonc, Table, TSV.  Výchozí \: JSON. |
| --dotaz | Řetězec dotazu JMESPath \:Další informace a příklady najdete v tématu http//jmespath.org/. |
| --verbose | Zvyšte úroveň podrobností protokolování. Použijte--Debug pro úplné protokoly ladění. |

### <a name="examples"></a>Příklady

Vypněte telemetrii.

```
sfctl settings telemetry set_telemetry --off
```

Zapnout telemetrii.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Další kroky
- [Nastavte](service-fabric-cli.md) Service Fabric CLI.
- Naučte se používat rozhraní příkazového řádku Service Fabric s použitím [ukázkových skriptů](./scripts/sfctl-upgrade-application.md).
