---
title: Překročila se kvóta nasazení.
description: Popisuje, jak vyřešit chybu s více než 800 nasazeními v historii skupiny prostředků.
ms.topic: troubleshooting
ms.date: 06/25/2020
ms.openlocfilehash: 1b0c3de6007964b487a13e71cd43bd984cd970f1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391175"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Vyřešit chybu, pokud je počet nasazení vyšší než 800

Každá skupina prostředků je v historii nasazení omezená na 800 nasazení. Tento článek popisuje chybu, která se zobrazí, když se nasazení nepovede, protože by překročilo povolené nasazení 800. Chcete-li tuto chybu vyřešit, odstraňte nasazení z historie skupiny prostředků. Odstranění nasazení z historie neovlivní žádné z nasazených prostředků.

> [!NOTE]
> Azure Resource Manager se brzy začnou automaticky odstraňovat nasazení z historie, protože se blíží limitu. Tato chyba se může zobrazit i v případě, že jste se rozhodli automatické odstranění. Další informace najdete v tématu [Automatické odstraňování z historie nasazení](deployment-history-deletions.md).

## <a name="symptom"></a>Příznak

Během nasazování se zobrazí chyba s oznámením, že aktuální nasazení překročí kvótu nasazení 800.

## <a name="solution"></a>Řešení

### <a name="azure-cli"></a>Azure CLI

Pomocí příkazu [AZ Deployment Group Delete](/cli/azure/group/deployment) odstraňte nasazení z historie.

```azurecli-interactive
az deployment group delete --resource-group exampleGroup --name deploymentName
```

Pokud chcete odstranit všechna nasazení starší než pět dnů, použijte:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

Aktuální počet můžete v historii nasazení získat pomocí následujícího příkazu:

```azurecli-interactive
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Pomocí příkazu [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) z historie odstraňte nasazení.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Pokud chcete odstranit všechna nasazení starší než pět dnů, použijte:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

Aktuální počet můžete v historii nasazení získat pomocí následujícího příkazu:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Řešení třetích stran

Následující externí řešení řeší konkrétní scénáře:

* [Řešení Azure Logic Apps a PowerShellu](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [Rozšíření AzDevOps](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
