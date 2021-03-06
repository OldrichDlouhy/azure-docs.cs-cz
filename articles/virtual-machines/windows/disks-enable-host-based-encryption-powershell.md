---
title: Azure PowerShell – povolení kompletního šifrování na hostiteli virtuálního počítače
description: Jak povolit kompletní šifrování pro virtuální počítače Azure pomocí šifrování na hostiteli.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 07/10/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 6cb6235c5c1a34cb3f48d315adee565591bb72c4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088457"
---
# <a name="enable-end-to-end-encryption-using-encryption-at-host---azure-powershell"></a>Povolení kompletního šifrování pomocí šifrování na hostiteli – Azure PowerShell

Pokud povolíte šifrování na hostiteli, data uložená na hostiteli virtuálního počítače se zašifrují v klidovém stavu a toky se zašifrují do služby úložiště. Koncepční informace o šifrování na hostiteli a také o dalších typech šifrování spravovaného disku najdete v tématu [šifrování v rámci hostitele – koncové šifrování pro data virtuálních počítačů](disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

## <a name="restrictions"></a>Omezení

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Podporované oblasti

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Podporované velikosti virtuálních počítačů

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

Velikosti virtuálních počítačů můžete zjistit také programově. Informace o tom, jak je načíst programově, najdete v části [hledání podporovaných velikostí virtuálních počítačů](#finding-supported-vm-sizes) .

## <a name="prerequisites"></a>Předpoklady

Aby bylo možné používat šifrování na hostiteli pro vaše virtuální počítače nebo služby Virtual Machine Scale Sets, musíte ve svém předplatném mít povolenou funkci. Pokud chcete encryptionAtHost@microsoft funkci povolit pro vaše předplatná, odešlete e-mail na adresu. com s ID předplatného.

### <a name="create-an-azure-key-vault-and-diskencryptionset"></a>Vytvoření Azure Key Vault a DiskEncryptionSet

Jakmile je tato funkce povolená, budete muset nastavit Azure Key Vault a DiskEncryptionSet, pokud jste to ještě neudělali.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-powershell](../../../includes/virtual-machines-disks-encryption-create-key-vault-powershell.md)]

## <a name="enable-encryption-at-host-for-disks-attached-to-vm-and-virtual-machine-scale-sets"></a>Povolit šifrování na hostiteli pro disky připojené k virtuálnímu počítači a sadě škálování virtuálních počítačů

Šifrování můžete povolit na hostiteli nastavením nové vlastnosti EncryptionAtHost v securityProfile virtuálních počítačů nebo virtuálních počítačů pomocí rozhraní API verze **2020-06-01** a vyšší.

`"securityProfile": { "encryptionAtHost": "true" }`

## <a name="example-scripts"></a>Ukázkové skripty

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-customer-managed-keys"></a>Povolení šifrování na hostiteli pro disky připojené k virtuálnímu počítači pomocí klíčů spravovaných zákazníkem

Vytvořte virtuální počítač se spravovanými disky pomocí identifikátoru URI prostředku DiskEncryptionSet, který jste vytvořili dříve.

Nahraďte `<yourPassword>` , `<yourVMName>` , `<yourVMSize>` , `<yourDESName>` , `<yoursubscriptionID>` , a `<yourResourceGroupName>` a `<yourRegion>` potom spusťte skript.

```PowerShell
$password=ConvertTo-SecureString -String "<yourPassword>" -AsPlainText -Force
New-AzResourceGroupDeployment -ResourceGroupName <yourResourceGroupName> `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithCMK.json" `
-virtualMachineName "<yourVMName>" `
-adminPassword $password `
-vmSize "<yourVMSize>" `
-diskEncryptionSetId "/subscriptions/<yoursubscriptionID>/resourceGroups/<yourResourceGroupName>/providers/Microsoft.Compute/diskEncryptionSets/<yourDESName>" `
-region "<yourRegion>"
```

### <a name="enable-encryption-at-host-for-disks-attached-to-a-vm-with-platform-managed-keys"></a>Povolit šifrování na hostiteli pro disky připojené k virtuálnímu počítači pomocí klíčů spravovaných platformou

Nahraďte `<yourPassword>` , `<yourVMName>` ,, `<yourVMSize>` `<yourResourceGroupName>` a a `<yourRegion>` potom spusťte skript.

```PowerShell
$password=ConvertTo-SecureString -String "<yourPassword>" -AsPlainText -Force
New-AzResourceGroupDeployment -ResourceGroupName <yourResourceGroupName> `
-TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/EncryptionAtHost/CreateVMWithDisksEncryptedAtHostWithPMK.json" `
-virtualMachineName "<yourVMName>" `
-adminPassword $password `
-vmSize "<yourVMSize>" `
-region "<yourRegion>"
```

## <a name="finding-supported-vm-sizes"></a>Hledání podporovaných velikostí virtuálních počítačů

Starší verze virtuálních počítačů se nepodporují. Seznam podporovaných velikostí virtuálních počítačů najdete v těchto verzích:

Volání [rozhraní API SKU prostředku](/rest/api/compute/resourceskus/list) a kontrola, zda `EncryptionAtHostSupported` je funkce nastavena na **hodnotu true**.

```json
    {
        "resourceType": "virtualMachines",
        "name": "Standard_DS1_v2",
        "tier": "Standard",
        "size": "DS1_v2",
        "family": "standardDSv2Family",
        "locations": [
        "CentralUSEUAP"
        ],
        "capabilities": [
        {
            "name": "EncryptionAtHostSupported",
            "value": "True"
        }
        ]
    }
```

Nebo zavolejte rutinu PowerShellu [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku?view=azps-3.8.0) .

```powershell
$vmSizes=Get-AzComputeResourceSku | where{$_.ResourceType -eq 'virtualMachines' -and $_.Locations.Contains('CentralUSEUAP')} 

foreach($vmSize in $vmSizes)
{
    foreach($capability in $vmSize.capabilities)
    {
        if($capability.Name -eq 'EncryptionAtHostSupported' -and $capability.Value -eq 'true')
        {
            $vmSize

        }

    }
}
```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nakonfigurovali tyto prostředky, je můžete použít k zabezpečení svých spravovaných disků. Následující odkaz obsahuje ukázkové skripty, z nichž každý má odpovídající scénář, který můžete použít k zabezpečení svých spravovaných disků.

[Ukázky šablon Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
