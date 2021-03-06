---
title: Omezení
description: Omezení kódu pro funkce sady SDK
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 17f98f452764abdc8458cdc38661d464ecb0a60d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808549"
---
# <a name="limitations"></a>Omezení

Množství funkcí má velikost, počet nebo jiná omezení.

## <a name="azure-frontend"></a>Azure front-end

* Celkový počet instancí AzureFrontend na proces: 16.
* Celkový počet instancí AzureSession na AzureFrontend: 16.

## <a name="objects"></a>Objekty

* Celkem přípustných objektů jednoho typu (entita, CutPlaneComponent atd.): 16 777 215.
* Celkový povolený počet aktivních vyjmutých ploch: 8.

## <a name="geometry"></a>Geometrie

* Celkový počet povolených materiálů v prostředku: 65 535.
* Maximální rozměr jedné textury: 16 384 × 16 384. Větší zdrojové textury se škálují podle procesu převodu.

## <a name="overall-number-of-polygons"></a>Celkový počet mnohoúhelníků

Povolený počet mnohoúhelníků pro všechny načtené modely závisí na velikosti virtuálního počítače, který je předaný do [REST API správy relace](../how-tos/session-rest-api.md#create-a-session):

| Velikost virtuálního počítače | Maximální počet mnohoúhelníků |
|:--------|:------------------|
|standardní| 20 000 000 |
|nárok| bez omezení |

Podrobnější informace o tomto omezení najdete v části věnované [velikosti virtuálního počítače](../reference/vm-sizes.md) .

## <a name="platform-limitations"></a>Omezení platformy

**Stolní počítač s Windows 10**

* UWP/x86 je jediná podporovaná platforma UWP. UWP/x64 se nepodporuje.
* Jedinou podporovanou platformou Win32 je Win32/x64. Win32/x86 se nepodporuje.

**HoloLens 2**

* Funkce [vykreslování z PV fotoaparátu](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in) není podporována.
