---
title: Azure PowerShell – povolit dvojité šifrování na discích spravovaných v klidovém umístění
description: Pro data spravovaného disku pomocí Azure PowerShell zapněte dvojité šifrování v klidovém formátu.
author: roygara
ms.date: 07/10/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-windows
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 5e70d434fcb297ff39b32a83b89a86e85fe9564f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088440"
---
# <a name="azure-powershell---enable-double-encryption-at-rest-on-your-managed-disks"></a>Azure PowerShell – na spravovaných discích povolit dvojité šifrování v klidovém prostředí

Azure Disk Storage podporuje dvojité šifrování v klidovém umístění pro spravované disky. Koncepční informace o šifrovaném šifrování v klidovém umístění a dalších typech šifrování spravovaného disku naleznete v části [dvojité šifrování v klidovém oddílu v](disk-encryption.md#double-encryption-at-rest) článku o šifrování disku.

## <a name="supported-regions"></a>Podporované oblasti

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](../../../includes/virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="prerequisites"></a>Předpoklady

Nainstalujte nejnovější [verzi Azure PowerShell](/powershell/azure/install-az-ps)a přihlaste se k účtu Azure pomocí [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-4.3.0).

## <a name="getting-started"></a>Začínáme

1. Vytvořte instanci Azure Key Vault a šifrovací klíč.

    Při vytváření instance Key Vault musíte povolit ochranu po tichém odstranění a vyprázdnění. Obnovitelné odstranění zajistí, že Key Vault obsahuje odstraněný klíč pro dané období uchování (výchozí hodnota je 90 den). Vyčištění ochrany zajišťuje, že odstraněný klíč nebude možné trvale odstranit, dokud doba uchování neuplyne. Tato nastavení chrání před ztrátou dat kvůli náhodnému odstranění. Tato nastavení jsou povinná při použití Key Vault k šifrování spravovaných disků.
    
    ```powershell
    $ResourceGroupName="yourResourceGroupName"
    $LocationName="westus2"
    $keyVaultName="yourKeyVaultName"
    $keyName="yourKeyName"
    $keyDestination="Software"
    $diskEncryptionSetName="yourDiskEncryptionSetName"

    $keyVault = New-AzKeyVault -Name $keyVaultName -ResourceGroupName $ResourceGroupName -Location $LocationName -EnableSoftDelete -EnablePurgeProtection

    $key = Add-AzKeyVaultKey -VaultName $keyVaultName -Name $keyName -Destination $keyDestination  
    ```

1.  Vytvořte DiskEncryptionSet pomocí encryptionType sady jako EncryptionAtRestWithPlatformAndCustomerKeys. V šabloně Azure Resource Manager (ARM) použijte rozhraní API verze **2020-05-01** . 
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/managed-disks-powershell-getting-started/master/DoubleEncryption/CreateDiskEncryptionSetForDoubleEncryption.json" `
    -diskEncryptionSetName $diskEncryptionSetName `
    -keyVaultId $keyVault.ResourceId `
    -keyVaultKeyUrl $key.Key.Kid `
    -encryptionType "EncryptionAtRestWithPlatformAndCustomerKeys" `
    -region $LocationName
    ```

1. Udělte DiskEncryptionSet prostředku přístup k trezoru klíčů.

    > [!NOTE]
    > V případě, že Azure může v Azure Active Directory vytvořit identitu vašeho DiskEncryptionSetu, může to několik minut trvat. Pokud při spuštění následujícího příkazu dojde k chybě, například "Nejde najít objekt služby Active Directory", počkejte pár minut a zkuste to znovu.

    ```powershell  
    $des=Get-AzDiskEncryptionSet -name $diskEncryptionSetName -ResourceGroupName $ResourceGroupName
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $des.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
    ```

## <a name="next-steps"></a>Další kroky

Teď, když jste vytvořili a nakonfigurovali tyto prostředky, je můžete použít k zabezpečení svých spravovaných disků. Následující odkazy obsahují ukázkové skripty, z nichž každý má odpovídající scénář, který můžete použít k zabezpečení svých spravovaných disků.

- [Azure PowerShell – povolení klíčů spravovaných zákazníkem pomocí disků spravovaných šifrováním na straně serveru](disks-enable-customer-managed-keys-powershell.md)
- [Ukázky šablon Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)