---
title: Změna velikosti virtuálního počítače s Windows v Azure
description: Změňte velikost virtuálního počítače, která se používá pro virtuální počítač Azure.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: sizes
ms.workload: infrastructure
ms.topic: how-to
ms.date: 01/13/2020
ms.author: cynthn
ms.openlocfilehash: 552b397a93978d2790a69e9574b4ccf256e478b8
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289642"
---
# <a name="resize-a-windows-vm"></a>Změna velikosti virtuálního počítače s Windows

V tomto článku se dozvíte, jak přesunout virtuální počítač na jinou [Velikost virtuálního počítače](sizes.md).

Po vytvoření virtuálního počítače můžete virtuální počítač škálovat nahoru nebo dolů změnou velikosti virtuálního počítače. V některých případech je nutné nejprve zrušit přidělení virtuálního počítače. K tomu může dojít v případě, že nová velikost není k dispozici v hardwarovém clusteru, který je aktuálně hostitelem virtuálního počítače.

Pokud virtuální počítač používá Premium Storage, ujistěte se, že zvolíte verzi **s** , abyste získali Premium Storage podporu. Například vyberte Standard_E4**s**_v3 namísto Standard_E4_v3.

## <a name="use-the-portal"></a>Použití portálu

1. Otevřete web [Azure Portal](https://portal.azure.com).
1. Otevřete stránku pro virtuální počítač.
1. V nabídce vlevo vyberte **Velikost**.
1. Ze seznamu dostupných velikostí vyberte novou velikost a pak vyberte **změnit velikost**.


Pokud je virtuální počítač momentálně spuštěný, změna jeho velikosti způsobí jeho restartování. Zastavení virtuálního počítače může odhalit další velikosti.

## <a name="use-powershell-to-resize-a-vm-not-in-an-availability-set"></a>Použití PowerShellu k změně velikosti virtuálního počítače, který není ve skupině dostupnosti

Nastavte některé proměnné. Nahraďte hodnoty vlastními informacemi.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Vypíše velikosti virtuálních počítačů, které jsou k dispozici v hardwarovém clusteru, ve kterém je virtuální počítač hostovaný. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Pokud je požadovaná velikost uvedena, spusťte následující příkazy, abyste změnili velikost virtuálního počítače. Pokud požadovaná velikost není uvedena, pokračujte ke kroku 3.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

Pokud požadovaná velikost není uvedená, spusťte následující příkazy, čímž zrušíte přidělení virtuálního počítače, jeho velikost a restartujte virtuální počítač. Nahraďte **\<newVMsize>** velikostí, kterou chcete.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> Zrušení přidělení virtuálního počítače uvolní všechny dynamické IP adresy přiřazené k virtuálnímu počítači. Operační systém a datové disky nejsou ovlivněny. 
> 
> 

## <a name="use-powershell-to-resize-a-vm-in-an-availability-set"></a>Změna velikosti virtuálního počítače ve skupině dostupnosti pomocí PowerShellu

Pokud není nová velikost pro virtuální počítač ve skupině dostupnosti v hardwarovém clusteru, který je hostitelem virtuálního počítače, k dispozici, pak bude potřeba uvolnit všechny virtuální počítače ve skupině dostupnosti, aby se změnila velikost virtuálního počítače. Po změně velikosti jednoho virtuálního počítače možná budete muset aktualizovat velikost ostatních virtuálních počítačů ve skupině dostupnosti. Pokud chcete změnit velikost virtuálního počítače ve skupině dostupnosti, proveďte následující kroky.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Vypíše velikosti virtuálních počítačů, které jsou k dispozici v hardwarovém clusteru, ve kterém je virtuální počítač hostovaný. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Pokud je požadovaná velikost uvedena, spusťte následující příkazy, abyste změnili velikost virtuálního počítače. Pokud v seznamu není, přečtěte si další část.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Pokud požadovaná velikost není uvedená, pokračujte podle následujících kroků a Nadělte všechny virtuální počítače ve skupině dostupnosti, změňte velikost virtuálních počítačů a restartujte je.

Zastavte všechny virtuální počítače ve skupině dostupnosti.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Změňte velikost virtuálních počítačů ve skupině dostupnosti a restartujte je.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Další kroky

Pro další škálovatelnost spusťte více instancí virtuálních počítačů a nahorizontální navýšení kapacity. Další informace najdete v tématu [Automatické škálování počítačů s Windows v sadě škálování virtuálního počítače](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md).
