---
title: Ukázkový skript Azure CLI – Vytvoření virtuálního počítače s Windows Serverem 2016 a službou IIS pomocí DSC
description: Ukázkový skript Azure CLI – Vytvoření virtuálního počítače s Windows Serverem 2016 a službou IIS pomocí DSC
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 9e4333891d701e1c505e5f45f9012057aa07d808
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088627"
---
# <a name="create-a-vm-with-iis-using-dsc"></a>Vytvoření virtuálního počítače se službou IIS pomocí DSC

Tento skript vytvoří virtuální počítač a pomocí rozšíření vlastních skriptů DSC virtuálního počítače Azure nainstaluje a nakonfiguruje službu IIS. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

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
| [az vm create](/cli/azure/vm) | Vytvoří virtuální počítač a připojí ho k síťové kartě, virtuální síti, podsíti a skupině NSG. Tento příkaz také určuje image virtuálního počítače, která se má použít, a přihlašovací údaje pro správu.  |
| [az vm extension set](/cli/azure/vm) | Přidá do virtuálního počítače rozšíření vlastních skriptů, které vyvolá skript pro instalaci služby IIS. |
| [az vm open-port](/cli/azure/vm) | Vytvoří pravidlo skupiny zabezpečení sítě, které povolí příchozí provoz. V této ukázce se otevře port 80 pro přenosy HTTP. |
| [az group delete](/cli/azure/vm/extension) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](/cli/azure).

Další ukázkové skripty rozhraní příkazového řádku pro virtuální počítače najdete v [dokumentaci k virtuálním počítačům Azure s Windows](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
