---
title: Aplikace sfctl pro rozhraní příkazového řádku Azure Service Fabric
description: Přečtěte si o sfctl rozhraní příkazového řádku Azure Service Fabric. Obsahuje seznam příkazů pro správu prostředků aplikace Service Fabric mesh.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 835369116b07b74c666fba271476f1cba5a708b8
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259951"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Získejte a odstraňte prostředky aplikace.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
| odstranění | Odstraní prostředek aplikace. |
| list | Zobrazí seznam všech prostředků aplikace. |
| show | Získá prostředek aplikace se zadaným názvem. |

## <a name="sfctl-mesh-app-delete"></a>odstranění aplikace sfctl Mesh
Odstraní prostředek aplikace.

Odstraní prostředek aplikace identifikovaný názvem.

### <a name="arguments"></a>Arguments

|Argument|Popis|
| --- | --- |
| --Name-n [povinné] | Název aplikace |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --ladění | Zvyšte úroveň podrobností protokolování, aby se zobrazily všechny protokoly ladění. |
| --Help-h | Zobrazí tuto zprávu s upozorněním a ukončí. |
| --výstup-o | Výstupní formát.  Povolené hodnoty \: : JSON, jsonc, Table, TSV.  Výchozí \: JSON. |
| --dotaz | Řetězec dotazu JMESPath \:Další informace a příklady najdete v tématu http//jmespath.org/. |
| --verbose | Zvyšte úroveň podrobností protokolování. Použijte--Debug pro úplné protokoly ladění. |

## <a name="sfctl-mesh-app-list"></a>seznam aplikací sfctl pro síť
Zobrazí seznam všech prostředků aplikace.

Získá informace o všech prostředcích aplikace v dané skupině prostředků. Tyto informace zahrnují popis a další vlastnosti aplikace.

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --ladění | Zvyšte úroveň podrobností protokolování, aby se zobrazily všechny protokoly ladění. |
| --Help-h | Zobrazí tuto zprávu s upozorněním a ukončí. |
| --výstup-o | Výstupní formát.  Povolené hodnoty \: : JSON, jsonc, Table, TSV.  Výchozí \: JSON. |
| --dotaz | Řetězec dotazu JMESPath \:Další informace a příklady najdete v tématu http//jmespath.org/. |
| --verbose | Zvyšte úroveň podrobností protokolování. Použijte--Debug pro úplné protokoly ladění. |

## <a name="sfctl-mesh-app-show"></a>zobrazení aplikace sfctl Mesh
Získá prostředek aplikace se zadaným názvem.

Načte informace o prostředku aplikace s daným názvem. Tyto informace zahrnují popis a další vlastnosti aplikace.

### <a name="arguments"></a>Arguments

|Argument|Popis|
| --- | --- |
| --Name-n [povinné] | Název aplikace |

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| --ladění | Zvyšte úroveň podrobností protokolování, aby se zobrazily všechny protokoly ladění. |
| --Help-h | Zobrazí tuto zprávu s upozorněním a ukončí. |
| --výstup-o | Výstupní formát.  Povolené hodnoty \: : JSON, jsonc, Table, TSV.  Výchozí \: JSON. |
| --dotaz | Řetězec dotazu JMESPath \:Další informace a příklady najdete v tématu http//jmespath.org/. |
| --verbose | Zvyšte úroveň podrobností protokolování. Použijte--Debug pro úplné protokoly ladění. |


## <a name="next-steps"></a>Další kroky
- [Nastavte](service-fabric-cli.md) Service Fabric CLI.
- Naučte se používat rozhraní příkazového řádku Service Fabric s použitím [ukázkových skriptů](./scripts/sfctl-upgrade-application.md).
