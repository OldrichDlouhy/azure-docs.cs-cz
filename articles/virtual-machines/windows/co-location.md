---
title: Společné umístění virtuálních počítačů pro lepší latenci
description: Přečtěte si, jak se ve společném umístění prostředků virtuálních počítačů Azure dá vylepšit latence.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: b5a3c0a582b1e9dfbcf81968ebc9d0c7a0a4f75e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288307"
---
# <a name="co-locate-resource-for-improved-latency"></a>Společně umístit prostředek pro lepší latenci

Při nasazování aplikace v Azure vytvoří rozšíření instancí napříč oblastmi nebo zónami dostupnosti latenci sítě, což může mít vliv na celkový výkon vaší aplikace. 


## <a name="proximity-placement-groups"></a>Skupiny umístění bezkontaktní komunikace 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Další kroky

Nasaďte virtuální počítač do [skupiny umístění blízkosti](proximity-placement-groups.md) pomocí Azure PowerShell.

Naučte se [testovat latenci sítě](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Naučte se [optimalizovat propustnost sítě](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

Naučte [se používat skupiny umístění pro Proximity s aplikacemi SAP](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
