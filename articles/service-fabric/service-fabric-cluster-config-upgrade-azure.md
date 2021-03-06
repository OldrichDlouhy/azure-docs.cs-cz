---
title: Upgrade konfigurace clusteru Azure Service Fabric
description: Naučte se upgradovat konfiguraci, která spouští Cluster Service Fabric v Azure pomocí šablony Správce prostředků.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 010f2b5d6dee3a5985c421e468bcd28b18148e5a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247723"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Upgrade konfigurace clusteru v Azure 

Tento článek popisuje, jak přizpůsobit různá nastavení prostředků infrastruktury pro cluster Service Fabric. Pro clustery hostované v Azure můžete nastavení přizpůsobit prostřednictvím [Azure Portal](https://portal.azure.com) nebo pomocí Azure Resource Manager šablony.

> [!NOTE]
> V portálu nejsou k dispozici všechna nastavení a je [osvědčeným postupem jejich přizpůsobení pomocí šablony Azure Resource Manager](./service-fabric-best-practices-infrastructure-as-code.md); Portál je určen pouze Service Fabric Dev\Test scénář.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Přizpůsobení nastavení clusteru pomocí šablon Správce prostředků
Clustery Azure je možné nakonfigurovat prostřednictvím šablony Správce prostředků JSON. Další informace o různých nastaveních najdete v tématu [nastavení konfigurace pro clustery](service-fabric-cluster-fabric-settings.md). Například následující postup ukazuje, jak přidat nové nastavení *MaxDiskQuotaInMB* do části *Diagnostika* pomocí Azure Resource Explorer.

1. Přejděte na https://resources.azure.com.
2. Přejděte do svého předplatného rozšířením **předplatných**  ->  **\<Your Subscription>**  ->  **resourceGroups**  ->  **\<Your Resource Group>**  ->  **poskytovatelé**resourceGroups a  ->  clustery**Microsoft. ServiceFabric.**  ->  **clusters** -> **\<Your Cluster Name>**
3. V pravém horním rohu vyberte možnost **čtení/zápis.**
4. Vyberte **Upravit** a aktualizujte `fabricSettings` element JSON a přidejte nový element:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Nastavení clusteru můžete také přizpůsobit jedním z následujících způsobů pomocí Azure Resource Manager:

- Pomocí [Azure Portal](../azure-resource-manager/templates/export-template-portal.md) můžete exportovat a aktualizovat šablonu správce prostředků.
- K exportu a aktualizaci šablony Správce prostředků použijte [PowerShell](../azure-resource-manager/management/manage-resources-powershell.md) .
- K exportu a aktualizaci šablony Správce prostředků použijte rozhraní příkazového [řádku Azure](../azure-resource-manager/management/manage-resources-cli.md) .
- Použijte příkazy Azure PowerShell [set-AzServiceFabricSetting](/powershell/module/az.servicefabric/set-azservicefabricsetting) a [Remove-AzServiceFabricSetting](/powershell/module/az.servicefabric/remove-azservicefabricsetting) , abyste nastavení změnili přímo.
- Pomocí příkazů Azure CLI [AZ SF cluster Setting](/cli/azure/sf/cluster/setting) upravíte nastavení přímo.

## <a name="next-steps"></a>Další kroky
* Přečtěte si o [nastaveních clusteru Service Fabric](service-fabric-cluster-fabric-settings.md).
* Přečtěte si, jak [škálovat cluster na úrovni a](service-fabric-cluster-scale-in-out.md).
