---
title: Azure Disk Encryption pro virtuální počítače a sady škálování virtuálních počítačů
description: Tento článek poskytuje přehled Azure Disk Encryption
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 10/15/2019
ms.custom: seodec18
ms.openlocfilehash: c881b2b9743766e4d35e6cb05f6f3469803850bc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80062130"
---
# <a name="azure-disk-encryption-for-virtual-machines-and-virtual-machine-scale-sets"></a>Azure Disk Encryption pro virtuální počítače a sady škálování virtuálních počítačů

Azure Disk Encryption se dá použít pro virtuální počítače se systémem Linux i Windows i pro službu Virtual Machine Scale Sets. 

## <a name="linux-virtual-machines"></a>Virtuální počítače s Linuxem

Následující články obsahují pokyny k šifrování virtuálních počítačů se systémem Linux.

### <a name="current-version-of-azure-disk-encryption"></a>Aktuální verze Azure Disk Encryption

- [Přehled služby Azure Disk Encryption pro virtuální počítače s Linuxem](../../virtual-machines/linux/disk-encryption-overview.md)
- [Scénáře služby Azure Disk Encryption na virtuálních počítačích s Linuxem](../../virtual-machines/linux/disk-encryption-linux.md)
- [Vytvoření a šifrování virtuálního počítače s Linuxem s využitím Azure CLI](../../virtual-machines/linux/disk-encryption-cli-quickstart.md)
- [Vytvoření a šifrování virtuálního počítače s Linuxem s využitím Azure PowerShellu](../../virtual-machines/linux/disk-encryption-powershell-quickstart.md)
- [Vytvoření a šifrování virtuálního počítače s Linuxem s využitím webu Azure Portal](../../virtual-machines/linux/disk-encryption-portal-quickstart.md)
- [Azure Disk Encryption schéma rozšíření pro Linux](../../virtual-machines/extensions/azure-disk-enc-linux.md)
- [Vytvoření a konfigurace trezoru klíčů pro službu Azure Disk Encryption](../../virtual-machines/linux/disk-encryption-key-vault.md)
- [Ukázkové skripty pro službu Azure Disk Encryption](../../virtual-machines/linux/disk-encryption-sample-scripts.md)
- [Řešení potíží se službou Azure Disk Encryption](../../virtual-machines/linux/disk-encryption-troubleshooting.md)
- [Nejčastější dotazy ke službě Azure Disk Encryption](../../virtual-machines/linux/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure Disk Encryption s Azure AD (předchozí verze)

- [Přehled Azure Disk Encryption s Azure AD pro virtuální počítače se systémem Linux](../../virtual-machines/linux/disk-encryption-overview-aad.md)
- [Azure Disk Encryption se scénáři Azure AD na virtuálních počítačích se systémem Linux](../../virtual-machines/linux/disk-encryption-linux.md)
- [Vytvoření a konfigurace trezoru klíčů pro Azure Disk Encryption s využitím Azure AD (předchozí verze)](../../virtual-machines/linux/disk-encryption-key-vault-aad.md)

## <a name="windows-virtual-machines"></a>Virtuální počítače s Windows

Následující články obsahují pokyny k šifrování virtuálních počítačů s Windows.

### <a name="current-version-of-azure-disk-encryption"></a>Aktuální verze Azure Disk Encryption

- [Přehled služby Azure Disk Encryption pro virtuální počítače s Windows](../../virtual-machines/windows/disk-encryption-overview.md)
- [Scénáře služby Azure Disk Encryption na virtuálních počítačích s Windows](../../virtual-machines/windows/disk-encryption-windows.md)
- [Vytvoření a šifrování virtuálního počítače s Windows s využitím Azure CLI](../../virtual-machines/windows/disk-encryption-cli-quickstart.md)
- [Vytvoření a šifrování virtuálního počítače s Windows s využitím Azure PowerShellu](../../virtual-machines/windows/disk-encryption-powershell-quickstart.md)
- [Vytvoření a šifrování virtuálního počítače s Windows s využitím webu Azure Portal](../../virtual-machines/windows/disk-encryption-portal-quickstart.md)
- [Azure Disk Encryption schéma rozšíření pro Windows](../../virtual-machines/extensions/azure-disk-enc-windows.md)
- [Vytvoření a konfigurace trezoru klíčů pro službu Azure Disk Encryption](../../virtual-machines/windows/disk-encryption-key-vault.md)
- [Ukázkové skripty pro službu Azure Disk Encryption](../../virtual-machines/windows/disk-encryption-sample-scripts.md)
- [Řešení potíží se službou Azure Disk Encryption](../../virtual-machines/windows/disk-encryption-troubleshooting.md)
- [Nejčastější dotazy ke službě Azure Disk Encryption](../../virtual-machines/windows/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure Disk Encryption s Azure AD (předchozí verze)

- [Přehled Azure Disk Encryption s Azure AD pro virtuální počítače s Windows](../../virtual-machines/windows/disk-encryption-overview-aad.md)
- [Azure Disk Encryption se scénáři Azure AD na virtuálních počítačích s Windows](../../virtual-machines/windows/disk-encryption-windows.md)
- [Vytvoření a konfigurace trezoru klíčů pro Azure Disk Encryption s využitím Azure AD (předchozí verze)](../../virtual-machines/windows/disk-encryption-key-vault-aad.md)

## <a name="virtual-machine-scale-sets"></a>Škálovací sady virtuálních počítačů

Následující články obsahují pokyny k šifrování služby Virtual Machine Scale Sets.

- [Přehled Azure Disk Encryption pro Virtual Machine Scale Sets](../../virtual-machine-scale-sets/disk-encryption-overview.md) 
- [Šifrování sady škálování virtuálních počítačů pomocí Azure CLI](../../virtual-machine-scale-sets/disk-encryption-cli.md) 
- [Pomocí Azure PowerShellu Zašifrujte služby Virtual Machine Scale Sets](../../virtual-machine-scale-sets/disk-encryption-powershell.md).
- [Šifrování virtuálních počítačů pomocí Azure Resource Manager](../../virtual-machine-scale-sets/disk-encryption-azure-resource-manager.md)
- [Vytvoření a konfigurace trezoru klíčů pro Azure Disk Encryption](../../virtual-machine-scale-sets/disk-encryption-key-vault.md)
- [Použití Azure Disk Encryption s pořadím rozšíření pro škálování sady virtuálních počítačů](../../virtual-machine-scale-sets/disk-encryption-extension-sequencing.md)

## <a name="next-steps"></a>Další kroky

- [Přehled šifrování Azure](encryption-overview.md)
- [Šifrování dat v klidovém stavu](encryption-atrest.md)
- [Osvědčené postupy šifrování a zabezpečení dat](data-encryption-best-practices.md)
