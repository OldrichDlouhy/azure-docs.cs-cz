---
title: Vytvoření a nasazení specifikace šablony
description: Naučte se vytvořit specifikaci šablony ze šablony ARM. Pak nasaďte specifikaci šablony do skupiny prostředků v rámci vašeho předplatného.
author: tfitzmac
ms.date: 07/20/2020
ms.topic: quickstart
ms.author: tomfitz
ms.openlocfilehash: b2667e63f7cac5d1e3ad8475501ff909e8f6f3c1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102017"
---
# <a name="quickstart-create-and-deploy-template-spec-preview"></a>Rychlý Start: vytvoření a nasazení specifikace šablony (Preview)

V tomto rychlém startu se dozvíte, jak zabalit šablonu Azure Resource Manager (šablonu ARM) do [specifikace šablony](template-specs.md) a pak nasadit tuto specifikaci šablony. Vaše specifikace šablony obsahuje šablonu ARM, která nasazuje účet úložiště.

## <a name="prerequisites"></a>Předpoklady

Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

> [!NOTE]
> Specifikace šablony jsou momentálně ve verzi Preview. Pokud ho chcete použít, musíte se [zaregistrovat v seznamu čekání](https://aka.ms/templateSpecOnboarding).
>
> Po získání schválení ze seznamu čekání se zobrazí pokyny k instalaci modulu PowerShell Preview.

## <a name="create-template-spec"></a>Vytvořit specifikaci šablony

Specifikace šablony je nový typ prostředku s názvem **Microsoft. Resources/templateSpecs**. K vytvoření specifikace šablony můžete použít Azure PowerShell nebo šablonu ARM. Ve všech možnostech potřebujete šablonu ARM, která je zabalena do specifikace šablony.

V prostředí PowerShell je šablona ARM předána jako parametr příkazu. Šablona ARM, která se zabalí v rámci specifikace šablony, je vložená do definice specifikace šablony.

Níže jsou uvedené možnosti.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Když vytvoříte specifikaci šablony pomocí PowerShellu, můžete předat místní šablonu. Zkopírujte následující šablonu a uložte ji místně do souboru s názvem **azuredeploy.jsv**. Tento rychlý Start předpokládá, že jste uložili do cesty **c:\Templates\azuredeploy.js** , ale můžete použít libovolnou cestu.

   :::code language="json" source="~/quickstart-templates/101-storage-account-create/azuredeploy.json":::

1. Vytvořte novou skupinu prostředků, která bude obsahovat specifikaci šablony.

   ```azurepowershell
   New-AzResourceGroup `
     -Name templateSpecRG `
     -Location westus2
   ```

1. Pak vytvořte v této skupině prostředků specifikaci šablony. Dáte nové šabloně specifikace název **storageSpec**.

   ```powershell
   New-AzTemplateSpec `
     -ResourceGroupName templateSpecRG `
     -Name storageSpec `
     -Version "1.0.0.0" `
     -Location westus2 `
     -TemplateJsonFile "c:\Templates\azuredeploy.json"
   ```

# <a name="arm-template"></a>[Šablona ARM](#tab/azure-resource-manager)

1. Když použijete šablonu ARM k vytvoření specifikace šablony, šablona je vložena do definice prostředků. Zkopírujte následující šablonu a uložte ji místně jako **azuredeploy.jsna**. Tento rychlý Start předpokládá, že jste uložili do cesty **c:\Templates\azuredeploy.js** , ale můžete použít libovolnou cestu.

   > [!NOTE]
   > V vložené šabloně musí být všechny levé hranaté závorky uvozené druhou levou hranatou závorkou. `[[`Místo použijte `[` .

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "functions": [],
       "variables": {},
       "resources": [
           {
               "type": "Microsoft.Resources/templateSpecs",
               "apiVersion": "2019-06-01-preview",
               "name": "storageSpec",
               "location": "westus2",
               "properties": {
                   "displayName": "Storage template spec"
               },
               "tags": {},
               "resources": [
                   {
                       "type": "versions",
                       "apiVersion": "2019-06-01-preview",
                       "name": "1.0.0.1",
                       "location": "westus2",
                       "dependsOn": [ "storageSpec" ],
                       "properties": {
                           "template": {
                               "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                               "contentVersion": "1.0.0.0",
                               "parameters": {
                                   "storageAccountType": {
                                       "type": "string",
                                       "defaultValue": "Standard_LRS",
                                       "allowedValues": [
                                           "Standard_LRS",
                                           "Standard_GRS",
                                           "Standard_ZRS",
                                           "Premium_LRS"
                                       ],
                                       "metadata": {
                                           "description": "Storage Account type"
                                       }
                                   },
                                   "location": {
                                       "type": "string",
                                       "defaultValue": "[[resourceGroup().location]",
                                       "metadata": {
                                           "description": "Location for all resources."
                                       }
                                   }
                               },
                               "variables": {
                                   "storageAccountName": "[[concat('store', uniquestring(resourceGroup().id))]"
                               },
                               "resources": [
                                   {
                                       "type": "Microsoft.Storage/storageAccounts",
                                       "apiVersion": "2019-04-01",
                                       "name": "[[variables('storageAccountName')]",
                                       "location": "[[parameters('location')]",
                                       "sku": {
                                           "name": "[[parameters('storageAccountType')]"
                                       },
                                       "kind": "StorageV2",
                                       "properties": {}
                                   }
                               ],
                               "outputs": {
                                   "storageAccountName": {
                                       "type": "string",
                                       "value": "[[variables('storageAccountName')]"
                                   }
                               }
                           }
                       },
                       "tags": {}
                   }
               ]
           }
       ],
       "outputs": {}
   }
   ```

1. Pomocí Azure CLI nebo PowerShellu vytvořte novou skupinu prostředků.

   ```azurecli
   az group create \
     --name templateSpecRG \
     --location westus2
   ```

   ```azurepowershell
   New-AzResourceGroup `
     -Name templateSpecRG `
     -Location westus2
   ```

1. Nasaďte šablonu pomocí Azure CLI nebo PowerShellu.

   ```azurecli
   az deployment group create \
     --name templateSpecRG \
     --template-file "c:\Templates\azuredeploy.json"
   ```

   ```azurepowershell
   New-AzResourceGroupDeployment `
     -ResourceGroupName templateSpecRG `
     -TemplateFile "c:\Templates\azuredeploy.json"
   ```

---

## <a name="deploy-template-spec"></a>Nasadit specifikaci šablony

Nyní můžete nasadit specifikaci šablony. nasazení specifikace šablony je stejně jako nasazení šablony, kterou obsahuje, s výjimkou toho, že předáte ID prostředku specifikace šablony. Použijete stejné příkazy pro nasazení a v případě potřeby předáte hodnoty parametrů pro specifikaci šablony.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Vytvořte skupinu prostředků, která bude obsahovat nový účet úložiště.

   ```azurepowershell
   New-AzResourceGroup `
     -Name storageRG `
     -Location westus2
   ```

1. Získejte ID prostředku specifikace šablony.

   ```azurepowershell
   $id = (Get-AzTemplateSpec -ResourceGroupName templateSpecRG -Name storageSpec -Version "1.0.0.0").Version.Id
   ```

1. Nasaďte specifikaci šablony.

   ```azurepowershell
   New-AzResourceGroupDeployment `
     -TemplateSpecId $id `
     -ResourceGroupName demoRG
   ```

# <a name="arm-template"></a>[Šablona ARM](#tab/azure-resource-manager)

1. Zkopírujte následující šablonu a uložte ji místně do souboru s názvem **storage.jsv**.

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "functions": [],
       "variables": {},
       "resources": [
           {
               "type": "Microsoft.Resources/deployments",
               "apiVersion": "2020-06-01",
               "name": "demo",
               "properties": {
                   "templateLink": {
                       "id": "[resourceId('templateSpecRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0.0.0')]"
                   },
                   "parameters": {
                   },
                   "mode": "Incremental"
               }
           }
       ],
       "outputs": {}
   }
   ```

1. Pomocí Azure CLI nebo PowerShellu vytvořte novou skupinu prostředků pro účet úložiště.

   ```azurecli
   az group create \
     --name storageRG \
     --location westus2
   ```

   ```azurepowershell
   New-AzResourceGroup `
     -Name storageRG `
     -Location westus2
   ```

1. Nasaďte šablonu pomocí Azure CLI nebo PowerShellu.

   ```azurecli
   az deployment group create \
     --name storageRG \
     --template-file "c:\Templates\storage.json"
   ```

   ```azurepowershell
   New-AzResourceGroupDeployment `
     -ResourceGroupName storageRG `
     -TemplateFile "c:\Templates\storage.json"
   ```

---

## <a name="grant-access"></a>Udělení přístupu

Pokud chcete umožnit dalším uživatelům ve vaší organizaci nasazovat specifikace šablony, musíte jim udělit oprávnění ke čtení. Roli Čtenář můžete přiřadit skupině Azure AD pro skupinu prostředků, která obsahuje specifikace šablony, které chcete sdílet. Další informace najdete v tématu [kurz: udělení přístupu skupině k prostředkům Azure pomocí Azure PowerShell](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete vyčistit prostředek, který jste nasadili v rámci tohoto rychlého startu, odstraňte obě skupiny prostředků, které jste vytvořili.

1. Na webu Azure Portal vyberte v nabídce nalevo Skupina prostředků.

1. Do pole filtrovat podle názvu zadejte název skupiny prostředků (templateSpecRG a storageRG).

1. Vyberte název skupiny prostředků.

1. V horní nabídce vyberte Odstranit skupinu prostředků.

## <a name="next-steps"></a>Další kroky

Další informace o vytvoření specifikace šablony, která obsahuje propojené šablony, najdete v tématu [Vytvoření šablony specifikace propojené šablony](template-specs-create-linked.md).
