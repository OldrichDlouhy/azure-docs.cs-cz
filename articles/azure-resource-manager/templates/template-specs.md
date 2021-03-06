---
title: Přehled specifikací šablon
description: Popisuje, jak vytvořit specifikace šablony a sdílet je s ostatními uživateli ve vaší organizaci.
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 47dcf44b35ad5c0b77dd0b88d683071a7f2f4ecb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87096679"
---
# <a name="azure-resource-manager-template-specs-preview"></a>Azure Resource Manager specifikace šablon (Preview)

Specifikace šablony je nový typ prostředku pro uložení šablony Azure Resource Manager (šablona ARM) v Azure pro pozdější nasazení. Tento typ prostředku umožňuje sdílet šablony ARM s ostatními uživateli ve vaší organizaci. Stejně jako u jakéhokoli jiného prostředku Azure můžete ke sdílení specifikace šablony použít řízení přístupu na základě role (RBAC). uživatelé potřebují k nasazení šablony jenom přístup pro čtení ke specifikaci šablony, takže šablonu můžete sdílet, aniž by ji ostatní mohli upravovat.

**Microsoft. Resources/templateSpecs** je nový typ prostředku pro specifikace šablon. Skládá se z hlavní šablony a libovolného počtu propojených šablon. Azure bezpečně ukládá specifikace šablon do skupin prostředků. Specifikace šablon podporují [správu verzí](#versioning).

K nasazení specifikace šablony použijte standardní nástroje Azure, jako je PowerShell, Azure CLI, Azure Portal, REST a další podporované sady SDK a klienti. Použijte stejné příkazy a předejte stejné parametry pro šablonu.

Výhodou použití specifikací šablon je, že týmy ve vaší organizaci nemusejí znovu vytvářet ani kopírovat šablony pro běžné scénáře. Vytvoříte kanonické šablony a sdílíte je. Šablony, které zahrnete do specifikace šablony, by měli ověřit správci ve vaší organizaci, aby postupoval podle požadavků a pokynů pro organizaci.

> [!NOTE]
> Specifikace šablony jsou momentálně ve verzi Preview. Pokud ho chcete použít, musíte se [zaregistrovat v seznamu čekání](https://aka.ms/templateSpecOnboarding).

## <a name="create-template-spec"></a>Vytvořit specifikaci šablony

Následující příklad ukazuje jednoduchou šablonu pro vytvoření účtu úložiště v Azure.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "location": "[resourceGroup().location]",
      "kind": "StorageV2",
      "sku": {
        "name": "Premium_LRS",
        "tier": "Premium"
      }
    }
  ]
}
```

Když vytvoříte specifikaci šablony, příkazy PowerShellu nebo rozhraní příkazového řádku se přenesou do hlavního souboru šablony. Pokud hlavní šablona odkazuje na propojené šablony, budou příkazy vyhledat a zabalit je k vytvoření specifikace šablony. Další informace najdete v tématu [Vytvoření specifikace šablony s propojenými šablonami](#create-a-template-spec-with-linked-templates).

Vytvořte specifikaci šablony pomocí:

```azurepowershell
New-AzTemplateSpec -Name storageSpec -Version 1.0 -ResourceGroupName templateSpecsRg -TemplateJsonFile ./mainTemplate.json
```

Všechny specifikace šablon v předplatném můžete zobrazit pomocí:

```azurepowershell
Get-AzTemplateSpec
```

Podrobnosti o specifikaci šablony včetně jejích verzí můžete zobrazit pomocí:

```azurepowershell
Get-AzTemplateSpec -ResourceGroupName templateSpecsRG -Name storageSpec
```

## <a name="deploy-template-spec"></a>Nasadit specifikaci šablony

Po vytvoření specifikace šablony ji mohou nasadit uživatelé s přístupem **pro čtení** do specifikace šablony. Informace o udělení přístupu najdete v tématu [kurz: udělení přístupu skupině k prostředkům Azure pomocí Azure PowerShell](../../role-based-access-control/tutorial-role-assignments-group-powershell.md).

Specifikace šablony se dají nasadit prostřednictvím portálu, PowerShellu, rozhraní příkazového řádku Azure nebo jako propojené šablony ve větším nasazení šablony. Uživatelé v organizaci mohou nasadit specifikaci šablony do libovolného oboru v Azure (skupina prostředků, předplatné, skupina pro správu nebo tenant).

Místo předání cesty nebo identifikátoru URI pro šablonu nasadíte specifikaci šablony zadáním jejího ID prostředku. ID prostředku má následující formát:

**/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Resources/templateSpecs/{template-spec-name}/versions/{template-spec-version}**

Všimněte si, že ID prostředku obsahuje číslo verze specifikace šablony.

Například můžete nasadit specifikaci šablony pomocí následujícího příkazu PowerShellu.

```azurepowershell
$id = "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/templateSpecsRG/providers/Microsoft.Resources/templateSpecs/storageSpec/versions/1.0"

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

V praxi se obvykle spouštíte `Get-AzTemplateSpec` a získáte ID specifikace šablony, kterou chcete nasadit.

```azurepowershell
$id = (Get-AzTemplateSpec -Name storageSpec -ResourceGroupName templateSpecsRg -Version 1.0).Version.Id

New-AzResourceGroupDeployment `
  -TemplateSpecId $id `
  -ResourceGroupName demoRG
```

## <a name="create-a-template-spec-with-linked-templates"></a>Vytvoření specifikace šablony s propojenými šablonami

Pokud hlavní šablona pro specifikaci šablony odkazuje na propojené šablony, příkazy PowerShellu a rozhraní příkazového řádku můžou automaticky vyhledávat a zabalit propojené šablony z místního disku. Nemusíte ručně konfigurovat účty úložiště nebo úložiště k hostování specifikací šablon – vše je v prostředku specifikace šablony obsaženo samostatně.

Následující příklad se skládá z hlavní šablony se dvěma propojenými šablonami. Příklad je pouze výňatek z šablony. Všimněte si, že používá vlastnost s názvem `relativePath` pro odkazování na jiné šablony. `apiVersion` `2020-06-01` Pro prostředek nasazení je nutné použít nebo novější.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/webapp.json"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "relativePath": "artifacts/database.json"
                }
            }
        }
    ],
    "outputs": {}
}

```

Pokud se pro předchozí příklad spustí příkaz PowerShell nebo CLI, vyhledá příkaz tři soubory – hlavní šablonu, šablonu webové aplikace ( `webapp.json` ) a šablonu databáze ( `database.json` ) a zabalí je do specifikace šablony.

Další informace najdete v tématu [kurz: Vytvoření specifikace šablony s propojenými šablonami](template-specs-create-linked.md).

## <a name="deploy-template-spec-as-a-linked-template"></a>Nasazení specifikace šablony jako propojené šablony

Po vytvoření specifikace šablony ji snadno znovu použijete ze šablony ARM nebo jiné specifikace šablony. Odkaz na specifikaci šablony můžete propojit přidáním jejího ID prostředku do šablony. Specifikace propojené šablony se automaticky nasadí při nasazení hlavní šablony. Toto chování vám umožní vyvíjet modulární specifikace šablony a znovu je použít podle potřeby.

Můžete například vytvořit šablonu specifikace, která nasadí síťové prostředky, a další specifikaci šablony, která nasadí prostředky úložiště. V šablonách ARM propojíte tyto dvě specifikace šablony, kdykoli budete potřebovat nakonfigurovat sítě nebo prostředky úložiště.

Následující příklad je podobný jako předchozí příklad, ale `id` vlastnost slouží k propojení na specifikaci šablony namísto `relativePath` vlastnosti pro propojení s místní šablonou. Používá `2020-06-01` se pro verzi rozhraní API pro prostředek nasazení. V příkladu jsou specifikace šablony ve skupině prostředků s názvem **templateSpecsRG**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "networkingDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'networkingSpec', '1.0')]"
                }
            }
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "storageDeployment",
            ...
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "id": "[resourceId('templateSpecsRG', 'Microsoft.Resources/templateSpecs/versions', 'storageSpec', '1.0')]"
                }
            }
        }
    ],
    "outputs": {}
}
```

Další informace o propojování specifikací šablon najdete v tématu [kurz: nasazení specifikace šablony jako propojené šablony](template-specs-deploy-linked-template.md).

## <a name="versioning"></a>Správa verzí

Když vytvoříte specifikaci šablony, zadáte pro ni číslo verze. Při iteraci kódu šablony můžete buď aktualizovat existující verzi (pro opravy hotfix) nebo publikovat novou verzi. Verze je textový řetězec. Můžete se rozhodnout, že budete postupovat podle systému správy verzí, včetně sémantické správy verzí. Uživatelé specifikace šablony můžou poskytnout číslo verze, které chtějí použít při jeho nasazení.

## <a name="next-steps"></a>Další kroky

* Chcete-li vytvořit a nasadit specifikace šablony, přečtěte si téma [rychlý Start: vytvoření a nasazení specifikace šablony](quickstart-create-template-specs.md).

* Další informace o propojování šablon v rámci specifikací šablon najdete v tématu [kurz: Vytvoření specifikace šablony s propojenými šablonami](template-specs-create-linked.md).

* Další informace o nasazení specifikace šablony jako propojené šablony najdete v tématu [kurz: nasazení specifikace šablony jako propojené šablony](template-specs-deploy-linked-template.md).
