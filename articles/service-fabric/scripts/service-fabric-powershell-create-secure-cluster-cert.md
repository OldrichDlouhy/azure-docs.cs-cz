---
title: Vytvoření clusteru Service Fabric v PowerShellu
description: Ukázka skriptu Azure PowerShell – vytvořte Service Fabric cluster zabezpečený pomocí certifikátu X. 509.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/19/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: f6b900bba178d4180d48ed3b89ec1e4d6cb49d7b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87076122"
---
# <a name="create-a-service-fabric-cluster"></a>Vytvoření clusteru Service Fabric

Tento ukázkový skript vytvoří cluster Service Fabric s pěti uzly zabezpečený pomocí certifikátu X.509.  Příkaz vytvoří certifikát podepsaný svým držitelem a nahraje ho do nového trezoru klíčů. Certifikát se také zkopíruje do místního adresáře.  Nastavením parametru *-OS* vyberte verzi systému Windows nebo Linux, která bude spuštěna na uzlech clusteru.  Podle potřeby upravte parametry.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

V případě potřeby nainstalujte Azure PowerShell pomocí instrukcí v [příručce Azure PowerShell](/powershell/azure/) a pak spuštěním příkazu `Connect-AzAccount` vytvořte připojení k Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázkového skriptu můžete pomocí následujícího příkazu odebrat skupinu prostředků, cluster a všechny související prostředky.

```powershell
$groupname="mysfclustergroup"
Remove-AzResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [New-AzServiceFabricCluster](/powershell/module/az.servicefabric/New-azServiceFabricCluster) | Vytvoří nový cluster Service Fabric. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](/powershell/azure/).

Další ukázky Azure PowerShellu pro Azure Service Fabric najdete v [ukázkách Azure PowerShellu](../service-fabric-powershell-samples.md).
