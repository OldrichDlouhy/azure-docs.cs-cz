---
title: Vytvoření a šifrování virtuálního počítače s Windows s využitím Azure CLI
description: V tomto rychlém startu se dozvíte, jak pomocí Azure CLI vytvořit a zašifrovat virtuální počítač s Windows.
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: aa0dd0bf55d51800d6a9b4283aa9e653887bf2f1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82082794"
---
# <a name="quickstart-create-and-encrypt-a-windows-vm-with-the-azure-cli"></a>Rychlý Start: vytvoření a šifrování virtuálního počítače s Windows pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. V tomto rychlém startu se dozvíte, jak pomocí rozhraní příkazového řádku Azure vytvořit a zašifrovat virtuální počítač s Windows serverem 2016 (VM).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku Azure CLI místně, musíte mít spuštěnou verzi Azure CLI 2.0.30 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v umístění *eastus* :

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm?view=azure-cli-latest#az-vm-create). Následující příklad vytvoří virtuální počítač s názvem *myVM*. Tento příklad používá jako uživatelské jméno správce *azureuser* a jako heslo *myPassword12*.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

Vytvoření virtuálního počítače a podpůrných prostředků trvá několik minut. Následující příklad ukazuje, že operace vytvoření virtuálního počítače byla úspěšná.

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Vytvoření Key Vault nakonfigurovaného pro šifrovací klíče

Azure Disk Encryption ukládá šifrovací klíč do Azure Key Vault. Vytvořte Key Vault pomocí [AZ datatrezor Create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create). Pokud chcete povolit Key Vault ukládání šifrovacích klíčů, použijte parametr--Enabled-pro-Disk-Encryption.
> [!Important]
> Každý Key Vault musí mít jedinečný název. Následující příklad vytvoří Key Vault s názvem *myKV*, ale je nutné, abyste si pojmenovali něco jiného.

```azurecli-interactive
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>Zašifrovat virtuální počítač

Pomocí příkaz [AZ VM Encryption](/cli/azure/vm/encryption?view=azure-cli-latest)Zašifrujte virtuální počítač zadáním jedinečného názvu Key Vault parametru--Disk-Encryption-File trezor.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

Můžete ověřit, že je na vašem VIRTUÁLNÍm počítači povolené šifrování pomocí [AZ VM show](/cli/azure/vm/encryption#az-vm-encryption-show) .

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Ve vráceném výstupu se zobrazí následující:

```
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a Key Vault použít příkaz [AZ Group Delete](/cli/azure/group) .

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili virtuální počítač, vytvořili Key Vault, která byla povolená pro šifrovací klíče, a zašifroval virtuální počítač.  V dalším článku najdete další informace o požadavcích na službu Azure Disk Encryption pro virtuální počítače IaaS.

> [!div class="nextstepaction"]
> [Přehled Azure Disk Encryption](disk-encryption-overview.md)
