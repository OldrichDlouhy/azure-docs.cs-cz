---
title: Vytvoření a konfigurace trezoru klíčů pro službu Azure Disk Encryption
description: Tento článek popisuje kroky pro vytvoření a konfiguraci trezoru klíčů pro použití s Azure Disk Encryption
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: acd2ae54d81fb508d5f8c02262cf8c2f0f071fb5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080603"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Vytvoření a konfigurace trezoru klíčů pro Azure Disk Encryption

Azure Disk Encryption používá Azure Key Vault k řízení a správě šifrovacích klíčů a tajných klíčů disku.  Další informace o trezorech klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/general/overview.md) a [zabezpečení trezoru klíčů](../key-vault/general/secure-your-key-vault.md).

Vytvoření a konfigurace trezoru klíčů pro použití s Azure Disk Encryption zahrnuje tři kroky:

1. V případě potřeby vytvořte skupinu prostředků.
2. Vytváří se Trezor klíčů. 
3. Nastavují se zásady rozšířeného přístupu trezoru klíčů.

Tyto kroky jsou znázorněné v následujících rychlých startech:

Můžete také, pokud chcete, vygenerovat nebo importovat klíč šifrování klíče (KEK).

## <a name="install-tools-and-connect-to-azure"></a>Nainstalovat nástroje a připojit se k Azure

Kroky v tomto článku se dají dokončit pomocí [Azure CLI](/cli/azure/), [Azure PowerShell Az Module](/powershell/azure/)nebo [Azure Portal](https://portal.azure.com).

### <a name="connect-to-your-azure-account"></a>Připojení k účtu Azure

Než začnete používat rozhraní příkazového řádku Azure CLI nebo Azure PowerShell, musíte se nejdřív připojit k předplatnému Azure. Provedete to tak, že [se přihlásíte pomocí Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), [přihlásíte se pomocí Azure PowerShell](/powershell/azure/authenticate-azureps?view=azps-2.5.0)nebo po zobrazení výzvy zadáte přihlašovací údaje do Azure Portal.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Další kroky

- [Přehled Azure Disk Encryption](disk-encryption-overview.md)
- [Šifrování sady škálování virtuálních počítačů pomocí Azure CLI](disk-encryption-cli.md)
- [Šifrování virtuálních počítačů pomocí Azure PowerShell](disk-encryption-powershell.md)
