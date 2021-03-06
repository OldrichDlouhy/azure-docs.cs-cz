---
title: 'Rychlý Start: vytvoření Azure Firewall s více veřejnými IP adresami – šablona Správce prostředků'
description: Naučte se používat šablonu Azure Resource Manager (šablonu ARM) k vytvoření Azure Firewall s více veřejnými IP adresami.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 7e89188e7ebc979c403b86ee26c876e8c40aa208
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260554"
---
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---arm-template"></a>Rychlý Start: vytvoření Azure Firewall s více veřejnými IP adresami – šablona ARM

V tomto rychlém startu použijete šablonu Azure Resource Manager (šablona ARM) k nasazení Azure Firewall s více veřejnými IP adresami. Nasazená brána firewall má pravidla shromažďování pravidel NAT, která umožňují připojení RDP ke dvěma virtuálním počítačům s Windows serverem 2019.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Další informace o Azure Firewall s více veřejnými IP adresami najdete v tématu [nasazení Azure firewall s více veřejnými IP adresami pomocí Azure PowerShell](deploy-multi-public-ip-powershell.md).

Pokud vaše prostředí splňuje požadavky a Vy jste obeznámeni s používáním šablon ARM, vyberte tlačítko **nasadit do Azure** . Šablona se otevře v Azure Portal.

[![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

## <a name="prerequisites"></a>Předpoklady

- Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Kontrola šablony

Tato šablona vytvoří Azure Firewall se dvěma veřejnými IP adresami, včetně nezbytných prostředků pro podporu Azure Firewall.

Šablona použitá v tomto rychlém startu je ze [šablon Azure pro rychlý Start](https://azure.microsoft.com/resources/templates/fw-docs-qs).

:::code language="json" source="~/quickstart-templates/fw-docs-qs/azuredeploy.json" range="001-391" highlight="238-370":::

V šabloně je definováno víc prostředků Azure:

- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)

## <a name="deploy-the-template"></a>Nasazení šablony

Nasazení šablony ARM do Azure:

1. Vyberte **nasadit do Azure** a přihlaste se k Azure a otevřete šablonu. Šablona vytvoří Azure Firewall, síťovou infrastrukturu a dva virtuální počítače.

   [![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

2. Na portálu na stránce **vytvořit Azure firewall s více adresami veřejné IP adresy** zadejte nebo vyberte následující hodnoty:
   - Předplatné: vyberte z existujících předplatných. 
   - Skupina prostředků: vyberte z existujících skupin prostředků nebo vyberte **vytvořit novou**a vyberte **OK**.
   - Umístění: vyberte umístění.
   - Uživatelské jméno správce: zadejte uživatelské jméno pro uživatelský účet správce. 
   - Heslo správce: zadejte heslo nebo klíč správce.

3. Vyberte Souhlasím **s podmínkami a ujednáními uvedenými nahoře** a pak vyberte **koupit**. Dokončení nasazení může trvat 10 minut nebo i déle.

## <a name="validate-the-deployment"></a>Ověření nasazení

V Azure Portal zkontrolujte nasazené prostředky. Poznamenejte si veřejné IP adresy brány firewall.  

Pomocí Připojení ke vzdálené ploše se připojte k veřejným IP adresám brány firewall. Úspěšná připojení ukazují pravidla brány firewall NAT, která umožňují připojení k back-end serverům.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Když už nepotřebujete prostředky, které jste vytvořili v bráně firewall, odstraňte skupinu prostředků. Tím se odebere brána firewall a všechny související prostředky.

Pokud chcete odstranit skupinu prostředků, zavolejte `Remove-AzResourceGroup` rutinu:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Kurz: nasazení a konfigurace Azure Firewall v hybridní síti pomocí Azure Portal](tutorial-hybrid-portal.md)