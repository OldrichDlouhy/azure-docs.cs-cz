---
title: Rychlý Start – vytvoření Azure Key Vault s využitím Azure PowerShell
description: Rychlý Start ukazující, jak vytvořit Azure Key Vault pomocí Azure PowerShell
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/2019
ms.author: mbaldwin
ms.openlocfilehash: e24f1e546de1ce01896e271dbc9155c47f6c2bb6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102022"
---
# <a name="quickstart-create-a-key-vault-using-powershell"></a>Rychlý Start: vytvoření trezoru klíčů pomocí PowerShellu

Azure Key Vault je cloudová služba, která poskytuje zabezpečené úložiště klíčů, [tajných](../secrets/index.yml) [kódů](../keys/index.yml)a [certifikátů](../certificates/index.yml). Další informace o Key Vault najdete v tématu [o Azure Key Vault](overview.md). Další informace o tom, co je možné uložit v trezoru klíčů, najdete v tématu [informace o klíčích, tajných klíčích a certifikátech](about-keys-secrets-certificates.md).

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

V tomto rychlém startu vytvoříte Trezor klíčů s [Azure PowerShell](/powershell/azure/). Pokud se rozhodnete nainstalovat a používat PowerShell místně, vyžaduje tento kurz Azure PowerShell verze modulu 1.0.0 nebo novější. `$PSVersionTable.PSVersion`Pro nalezení verze zadejte. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzAccount` pro vytvoření připojení k Azure.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```azurepowershell-interactive
New-AzResourceGroup -Name 'myResourceGroup" -Location "EastUS"
```

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů.

Ve skupině prostředků z předchozího kroku vytvořte Key Vault. Budete muset zadat několik informací:

- Název trezoru klíčů: řetězec na 3 až 24 znaků, který může obsahovat jenom číslice (0-9), písmena (A-z, A-Z) a spojovníky (-).

  > [!Important]
  > Každý Trezor klíčů musí mít jedinečný název. V následujících příkladech nahraďte <název trezoru klíčů jedinečných> s názvem vašeho trezoru klíčů.

- Název skupiny prostředků: **myResourceGroup**.
- Umístění: **EastUS**.

```azurepowershell-interactive
New-AzKeyVault -Name "&lt;your-unique-key-vault-name&gt; -ResourceGroupName "myResourceGroup" -Location "East US"
```

Výstup této rutiny zobrazuje vlastnosti nově vytvořeného trezoru klíčů. Poznamenejte si hodnoty dvou vlastností uvedených níže:

- **Název trezoru**: název, který jste zadali do parametru--name výše.
- **Identifikátor URI trezoru**: v tomto příkladu se jedná o https://a &lt; jedinečné úložiště klíčů – název &gt; . Vault.Azure.NET/. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

V tuto chvíli je váš účet Azure jediným účtem s oprávněním provádět jakékoli operace s tímto novým trezorem.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty a kurzy v této kolekci vycházejí z tohoto rychlého startu. Pokud chcete pokračovat v práci s dalšími rychlými starty a kurzy, možná budete chtít tyto prostředky zachovat.

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků a všech souvisejících prostředků použít příkaz Azure PowerShell [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) .

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili Key Vault a uložili do něj tajný klíč. Další informace o Key Vault a o tom, jak je integrovat s vašimi aplikacemi, najdete dál v článcích níže.

- Přečtěte si [přehled Azure Key Vault](overview.md)
- Přečtěte si referenční informace pro [rutiny Azure PowerShell Key Vault](/powershell/module/az.keyvault/?view=azps-2.6.0#key_vault)
- Kontrola [Azure Key Vault osvědčených postupů](best-practices.md)
