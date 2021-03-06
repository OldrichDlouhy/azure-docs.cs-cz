---
title: Společné umístění virtuálních počítačů se systémem Linux
description: Přečtěte si, jak se ve společném umístění prostředků virtuálních počítačů Azure dá vylepšit latence.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 000c8c8955314060ae9bdedb96b086a509fb3e96
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2020
ms.locfileid: "87368638"
---
# <a name="co-locate-resources-for-improved-latency"></a>Společné umístění prostředků pro lepší latenci

Při nasazování aplikace v Azure vytvoří rozšíření instancí napříč oblastmi nebo zónami dostupnosti latenci sítě, což může mít vliv na celkový výkon vaší aplikace. 

## <a name="proximity-placement-groups"></a>Skupiny umístění bezkontaktní komunikace

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Další kroky

Nasaďte virtuální počítač do [skupiny umístění blízkosti](proximity-placement-groups.md) pomocí Azure CLI.

Naučte se [testovat latenci sítě](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Naučte se [optimalizovat propustnost sítě](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

Naučte [se používat skupiny umístění pro Proximity s aplikacemi SAP](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
