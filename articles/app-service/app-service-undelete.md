---
title: Obnovit odstraněné aplikace
description: Přečtěte si, jak obnovit odstraněnou aplikaci v Azure App Service. Vyhněte se starostí nechtěné odstraněné aplikace.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: c3c79944aa4add0a32dbb584b13606e32e146a1a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050297"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Obnovení odstraněné aplikace App Service pomocí PowerShellu

Pokud jste nastali omylem odstranit aplikaci v Azure App Service, můžete ji obnovit pomocí příkazů z [modulu AZ PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0).

> [!NOTE]
> Odstraněné aplikace se po počátečním odstranění odstraní ze systému 30 dní. Jakmile se aplikace vyprázdní, nebude možné ji obnovit.
>

> [!NOTE]
> Funkce Undelete není pro plán spotřeby podporovaná.
>

## <a name="re-register-app-service-resource-provider"></a>Znovu zaregistrovat App Service poskytovatel prostředků
Někteří zákazníci se můžou setkat v rámci problému, při kterém se nepovede načíst seznam odstraněných aplikací. Chcete-li tento problém vyřešit, spusťte následující příkaz:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Vypsat odstraněné aplikace

Pokud chcete získat kolekci odstraněných aplikací, můžete použít `Get-AzDeletedWebApp` .

Podrobnosti o konkrétní odstraněné aplikaci můžete použít:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

Podrobné informace zahrnují:

- **DeletedSiteId**: jedinečný identifikátor pro aplikaci, který se používá pro scénáře, ve kterých se odstranilo víc aplikací se stejným názvem.
- **SubscriptionId**: předplatné obsahující odstraněný prostředek
- **Umístění**: umístění původní aplikace
- **ResourceGroupName**: název původní skupiny prostředků
- **Name**: název původní aplikace.
- **Slot**: název slotu.
- **Čas odstranění**: kdy se aplikace odstranila  

## <a name="restore-deleted-app"></a>Obnovit odstraněnou aplikaci
>[!NOTE]
> `Restore-AzDeletedWebApp`nepodporuje se pro aplikace Function App.

Po identifikaci aplikace, kterou chcete obnovit, ji můžete obnovit pomocí `Restore-AzDeletedWebApp` .

```powershell
Restore-AzDeletedWebApp -TargetResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Nasazovací sloty nejsou v rámci vaší aplikace obnoveny. Pokud potřebujete obnovit přípravný slot, použijte `-Slot <slot-name>` příznak.
>

Vstupy pro příkaz jsou:

- **Cílová skupina prostředků**: cílová skupina prostředků, do které se aplikace obnoví
- **Název**: název aplikace by měl být globálně jedinečný.
- **TargetAppServicePlanName**: App Service plán propojený s aplikací

Ve výchozím nastavení `Restore-AzDeletedWebApp` obnoví vaše konfigurace aplikace i jakýkoli obsah. Chcete-li obnovit pouze obsah, použijte `-RestoreContentOnly` příznak s tímto rutiny.

> [!NOTE]
> Pokud byla aplikace hostována a poté odstraněna z App Service Environment pak ji lze obnovit pouze v případě, že odpovídající App Service Environment stále existují.
>

Úplný odkaz na rutiny najdete tady: [Restore-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
