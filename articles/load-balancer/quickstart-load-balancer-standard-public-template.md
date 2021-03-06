---
title: 'Rychlý Start: vytvoření Load Balancer – šablona Azure'
titleSuffix: Azure Load Balancer
description: V tomto rychlém startu se dozvíte, jak vytvořit nástroj pro vyrovnávání zatížení pomocí šablony Azure Resource Manager.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2020
ms.author: allensu
ms.custom: mvc,subject-armqs
ms.openlocfilehash: ebf2f926f5be86ffee5f3a3e30277962a6060762
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2020
ms.locfileid: "85479756"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-by-using-an-arm-template"></a>Rychlý Start: vytvoření Load Balancer pro vyrovnávání zatížení virtuálních počítačů pomocí šablony ARM

Vyrovnávání zatížení zajišťuje vyšší úroveň dostupnosti a škálování tím, že rozprostírá příchozí požadavky na více virtuálních počítačů. V tomto rychlém startu se dozvíte, jak nasadit šablonu Azure Resource Manager (šablonu ARM), která vytvoří pro vyrovnávání zatížení virtuálních počítačů standardní nástroj pro vyrovnávání zatížení. Použití šablony ARM bere v porovnání s jinými metodami nasazení méně kroků.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Pokud vaše prostředí splňuje požadavky a Vy jste obeznámeni s používáním šablon ARM, vyberte tlačítko **nasadit do Azure** . Šablona se otevře v Azure Portal.

[![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-load-balancer-standard-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

## <a name="review-the-template"></a>Kontrola šablony

Šablona použitá v tomto rychlém startu je ze [šablon Azure pro rychlý Start](https://azure.microsoft.com/resources/templates/101-load-balancer-standard-create/).

Load Balancer a veřejné SKU IP adres se musí shodovat. Při vytváření Standard Load Balancer musíte také vytvořit novou standardní veřejnou IP adresu, která je nakonfigurovaná jako front-end pro nástroj pro vyrovnávání zatížení úrovně Standard. Pokud chcete vytvořit základní Load Balancer, použijte [tuto šablonu](https://azure.microsoft.com/resources/templates/201-2-vms-loadbalancer-natrules/). Microsoft doporučuje pro produkční úlohy používat standardní SKU.

:::code language="json" source="~/quickstart-templates/101-load-balancer-standard-create/azuredeploy.json" range="1-324" highlight="57-122":::

V šabloně bylo definováno více prostředků Azure:

- [**Microsoft. Network/loadBalancers**](/azure/templates/microsoft.network/loadbalancers)
- [**Microsoft. Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses): pro nástroj pro vyrovnávání zatížení a pro každý ze tří virtuálních počítačů.
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. COMPUTE/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines) (3 z nich).
- [**Microsoft. Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) (3 z nich).
- [**Microsoft. COMPUTE/VirtualMachine/Extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) (3 z nich): použijte ke konfiguraci služby IIS a webových stránek.

Další šablony, které souvisejí s Azure Load Balancer, najdete v tématu [šablony pro rychlý Start Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Nasazení šablony

1. Vyberte **vyzkoušet** z následujícího bloku kódu a otevřete Azure Cloud Shell a pak postupujte podle pokynů pro přihlášení k Azure.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Počkejte, dokud se nezobrazí výzva z konzoly.

1. Pro zkopírování skriptu PowerShellu vyberte **Kopírovat** z předchozího bloku kódu.

1. Klikněte pravým tlačítkem na Podokno konzole prostředí a pak vyberte **Vložit**.

1. Zadejte hodnoty.

   Nasazení šablony vytvoří tři zóny dostupnosti. Zóny dostupnosti se podporují jenom v [určitých oblastech](../availability-zones/az-overview.md). Použijte jednu z podporovaných oblastí. Pokud si nejste jistí, zadejte **centralus**.

   Název skupiny prostředků je název projektu s připojeným **RG** . Název skupiny prostředků budete potřebovat v další části.

Nasazení šablony trvá přibližně 10 minut. Výstup je po dokončení podobný tomuto:

![Výstup nasazení PowerShellu pro Azure Standard Load Balancer Správce prostředků](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-powershell-output.png)

Azure PowerShell slouží k nasazení šablony. Kromě Azure PowerShell můžete použít také Azure Portal, Azure CLI a REST API. Další informace o dalších metodách nasazení najdete v tématu [Nasazení šablon](../azure-resource-manager/templates/deploy-portal.md).

## <a name="review-deployed-resources"></a>Kontrola nasazených prostředků

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

1. V levém podokně vyberte **skupiny prostředků** .

1. Vyberte skupinu prostředků, kterou jste vytvořili v předchozí části. Výchozí název skupiny prostředků je název projektu s připojeným **RG** .

1. Vyberte nástroj pro vyrovnávání zatížení. Výchozím názvem je název projektu s připojenou hodnotou **----9,1** .

1. Zkopírujte pouze část veřejné IP adresy IP adresy a vložte ji do adresního řádku prohlížeče.

   ![Azure Load Balancer úrovně Standard – veřejná IP adresa šablony Správce prostředků](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-deployment-public-ip.png)

    Prohlížeč zobrazí výchozí stránku webového serveru Internetová informační služba (IIS).

   ![Webový server služby IIS](./media/quickstart-load-balancer-standard-public-template/load-balancer-test-web-page.png)

Pokud chcete zobrazit distribuci provozu nástroje pro vyrovnávání zatížení napříč všemi třemi virtuálními počítači, můžete vynutit aktualizaci webového prohlížeče z klientského počítače.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud je už nepotřebujete, odstraňte skupinu prostředků, nástroj pro vyrovnávání zatížení a všechny související prostředky. Provedete to tak, že přejdete na Azure Portal, vyberete skupinu prostředků, která obsahuje nástroj pro vyrovnávání zatížení, a pak vyberete **Odstranit skupinu prostředků**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili standardní nástroj pro vyrovnávání zatížení, připojili jste k němu virtuální počítače, nakonfigurovali pravidlo provozu nástroje pro vyrovnávání zatížení, provedli sondu stavu a pak otestovali Nástroj pro vyrovnávání zatížení.

Pokud se chcete dozvědět víc, přejděte k kurzům Load Balancer.

> [!div class="nextstepaction"]
> [Kurzy o službě Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
