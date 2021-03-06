---
title: Osobní údaje
description: Naučte se spravovat osobní data přidružená k Azure Resource Manager operací.
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 22cfc1b6096980f3d10db404a1c4e02f2de355d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75485257"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Správa osobních údajů spojených s Azure Resource Manager

Aby nedocházelo k odhalení citlivých informací, odstraňte všechny osobní údaje, které jste mohli poskytnout v nasazeních, skupinách prostředků nebo značkách. Azure Resource Manager poskytuje operace, které vám umožní spravovat osobní údaje, které jste mohli poskytnout v nasazeních, skupinách prostředků nebo značkách.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Odstranění osobních údajů v historii nasazení

V případě nasazení Správce prostředků zachovává hodnoty parametrů a stavové zprávy v historii nasazení. Tyto hodnoty jsou trvalé, dokud neodstraníte nasazení z historie. Pokud chcete zjistit, jestli jste v těchto hodnotách zadali osobní údaje, Seznamte se s nasazeními. Pokud najdete osobní údaje, odstraňte nasazení z historie.

K vypsání **nasazení** v historii použijte:

* [Seznam podle skupiny prostředků](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [AZ Group Deployment list](/cli/azure/group/deployment#az-group-deployment-list)

Pokud chcete z historie odstranit **nasazení** , použijte:

* [Odstranit](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [AZ Group Deployment DELETE](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Odstranění osobních údajů v názvech skupin prostředků

Název skupiny prostředků přetrvává, dokud skupinu prostředků neodstraníte. Pokud chcete zjistit, jestli jste v názvech zadali osobní údaje, Seznamte se se skupinami prostředků. Pokud najdete osobní údaje, [přesuňte prostředky](move-resource-group-and-subscription.md) do nové skupiny prostředků a odstraňte skupinu prostředků s osobními údaji v názvu.

K vypsání **skupin prostředků**použijte:

* [Seznam](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [AZ Group list](/cli/azure/group#az-group-list)

Pokud chcete odstranit **skupiny prostředků**, použijte:

* [Odstranit](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Odstranění osobních údajů ve značkách

Názvy značek a hodnoty jsou trvalé, dokud neodstraníte nebo nezměníte značku. Pokud chcete zjistit, jestli jste ve značkách zadali osobní údaje, Seznamte se se značkami. Pokud najdete osobní údaje, odstraňte tyto značky.

Chcete-li zobrazit seznam **značek**, použijte:

* [Seznam](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [AZ tag list](/cli/azure/tag#az-tag-list)

Chcete-li odstranit **značky**, použijte:

* [Odstranit](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [AZ tag DELETE](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Další kroky
* Přehled Azure Resource Manager najdete v tématu [co je správce prostředků?](overview.md)