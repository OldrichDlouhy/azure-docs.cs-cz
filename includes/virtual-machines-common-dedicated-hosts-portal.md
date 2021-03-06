---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/10/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 427117fe47294a1db1fa8d3fa1e46ee1efb91b4d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "79129504"
---
## <a name="limitations"></a>Omezení

- Sady škálování virtuálních počítačů se na vyhrazených hostitelích aktuálně nepodporují.
- Typy velikosti a hardwaru, které jsou dostupné pro vyhrazené hostitele, se v jednotlivých oblastech liší. Další informace najdete na [stránce s cenami](https://aka.ms/ADHPricing) hostitele.

## <a name="create-a-host-group"></a>Vytvoření skupiny hostitelů

**Skupina hostitelů** je nový prostředek, který představuje kolekci vyhrazených hostitelů. Vytvoříte skupinu hostitelů v oblasti a zóně dostupnosti a přidáte do ní hostitele. Při plánování vysoké dostupnosti jsou k dispozici další možnosti. U vyhrazených hostitelů můžete použít jednu z následujících možností: 
- Rozsah napříč několika zónami dostupnosti. V takovém případě je nutné mít skupinu hostitelů v každé z zón, které chcete použít.
- Rozložit napříč několika doménami selhání, které jsou namapované na fyzické racky. 
 
V obou případech je nutné zadat počet domén selhání pro skupinu hostitelů. Pokud nechcete rozsah domén selhání ve skupině, použijte počet domén selhání 1. 

Můžete se také rozhodnout použít jak zóny dostupnosti, tak i domény selhání. 

V tomto příkladu vytvoříme skupinu hostitelů s použitím 1 zóny dostupnosti a 2 domén selhání. 


1. Otevřete Azure [Portal](https://portal.azure.com).
1. V levém horním rohu vyberte **vytvořit prostředek** .
1. Vyhledejte **skupinu hostitelů** a pak z výsledků vyberte **skupiny hostitelů** .
1. Na stránce **skupiny hostitelů** vyberte **vytvořit**.
1. Vyberte předplatné, které chcete použít, a pak vyberte **vytvořit novou** a vytvořte novou skupinu prostředků.
1. Jako **název** zadejte *myDedicatedHostsRG* a pak vyberte **OK**.
1. Jako **název skupiny hostitelů**zadejte *myHostGroup*.
1. V **oblasti umístění**vyberte **východní USA**.
1. V **oblasti dostupnost**vyberte **1**.
1. V případě **počtu domén selhání**vyberte **2**.
1. Vyberte **zkontrolovat + vytvořit** a potom počkejte na ověření.
1. Jakmile se zobrazí zpráva s **potvrzením ověření** , vyberte **vytvořit** a vytvořte skupinu hostitelů.

Vytvoření skupiny hostitelů by nemělo chvíli trvat.

## <a name="create-a-dedicated-host"></a>Vytvoření vyhrazeného hostitele

Teď ve skupině hostitelů vytvořte vyhrazeného hostitele. Kromě názvu pro hostitele je nutné zadat SKU pro hostitele. SKU hostitele zachytí podporovanou řadu virtuálních počítačů a také generování hardwaru pro vyhrazeného hostitele.

Další informace o SKU a cenách hostitelů najdete v tématu [ceny za vyhrazené hostitele Azure](https://aka.ms/ADHPricing).

Pokud pro skupinu hostitelů nastavíte počet domén selhání, budete požádáni o zadání domény selhání pro hostitele.  

1. V levém horním rohu vyberte **vytvořit prostředek** .
1. Vyhledejte **vyhrazeného hostitele** a pak z výsledků vyberte **vyhrazené hostitele** .
1. Na stránce **vyhrazení hostitelé** vyberte **vytvořit**.
1. Vyberte předplatné, které chcete použít.
1. Jako **skupinu prostředků**vyberte *myDedicatedHostsRG* .
1. V části **Podrobnosti o instanci**zadejte *myHost* pro **název** a vyberte *východní USA* pro umístění.
1. V **části hardwarový profil**vyberte *Standard Es3 Family – typ 1* pro **rodinu velikostí**vyberte *myHostGroup* pro **skupinu hostitelů** a pak pro **doménu selhání**vyberte *1* . Pro zbývající pole ponechte výchozí hodnoty.
1. Až budete hotovi, vyberte **zkontrolovat + vytvořit** a počkejte na ověření.
1. Jakmile se zobrazí zpráva s **potvrzením ověření** , vyberte **vytvořit** a vytvořte hostitele.


