---
title: Ukázkový skript Azure CLI – Vytvoření virtuálního počítače s Windows Serverem
description: Ukázkový skript Azure CLI – Vytvoření virtuálního počítače s Windows Serverem
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 05d79d42ecf2f7ba5de3573cfe4c9d00d7749616
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079906"
---
# <a name="create-a-virtual-machine-with-the-azure-cli"></a>Vytvořit virtuální počítač pomocí Azure CLI

Tento skript vytvoří virtuální počítač Azure s Windows Serverem 2016. Po spuštění skriptu můžete k virtuálnímu počítači přistupovat prostřednictvím připojení ke vzdálené ploše.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spuštěním následujícího příkazu odeberte skupinu prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k vytvoření skupiny prostředků, virtuálního počítače a všech souvisejících prostředků používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az group create](/cli/azure/group) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [az network vnet create](/cli/azure/network/vnet) | Vytvoří virtuální síť Azure a podsíť. |
| [az network public-ip create](/cli/azure/network/public-ip) | Vytvoří veřejnou IP adresu se statickou IP adresou a přidruženým názvem DNS. |
| [az network nsg create](/cli/azure/network/nsg) | Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi internetem a virtuálním počítačem. |
| [az network nic create](/cli/azure/network/nic) | Vytvoří virtuální síťovou kartu a připojí ji k virtuální síti, podsíti a skupině NSG. |
| [az vm create](/cli/azure/vm) | Vytvoří virtuální počítač a připojí ho k síťové kartě, virtuální síti, podsíti a skupině NSG. Tento příkaz také určuje image virtuálního počítače, která se má použít, a přihlašovací údaje pro správu.  |
| [az group delete](/cli/azure/vm/extension) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](/cli/azure).

Další ukázkové skripty rozhraní příkazového řádku pro virtuální počítače najdete v [dokumentaci k virtuálním počítačům Azure s Windows](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
