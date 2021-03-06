---
title: Kurz – vyčištění prostředků sítě pro síť Azure Service Fabric
description: Naučte se, jak odebrat prostředky Azure Service Fabric Mesh, aby vám nebyly účtovány prostředky, které už nepoužíváte.
author: dkkapur
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: b8ce3c795bc9ad212331ce1c1f413fe7fd6da909
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246741"
---
# <a name="tutorial-remove-azure-resources"></a>Kurz: Odebrání prostředků Azure

V tomto kurzu, který je pátou částí série, se dozvíte, jak odstranit aplikaci a její prostředky, abyste za ně nemuseli platit.

Co se v tomto kurzu naučíte:
> [!div class="checklist"]
> * Vyčistit prostředky používané aplikací tak, aby vám za ně nenabíhaly poplatky

V této sérii kurzů se naučíte:
> [!div class="checklist"]
> * [Vytvoření aplikace Service Fabric Mesh v sadě Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Ladit aplikaci Service Fabric Mesh běžící v místním clusteru pro vývoj](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Nasadit aplikaci Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * [Upgrade aplikace Service Fabric Mesh](service-fabric-mesh-tutorial-upgrade.md)
> * Vyčištění prostředků Service Fabric Mesh

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Požadavky

Než začnete s tímto kurzem:

* Pokud jste nenasadili aplikaci seznamu úkolů, postupujte podle pokynů v článku o [publikování webové aplikace Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="clean-up-resources"></a>Vyčištění prostředků

Toto je konec tohoto kurzu. Jakmile budete s vytvořenými prostředky hotovi, odstraňte je, aby vám nebyly účtovány prostředky, které už nepoužíváte. To je zvlášť důležité, protože Mesh je bezserverová služba účtovaná po sekundách. Další informace o cenách služby Mesh najdete na adrese https://aka.ms/sfmeshpricing.

Jednou z výhod nabízených Azure je to, že když vytvoříte prostředky přidružené ke konkrétní skupině prostředků, při odstranění této skupiny prostředků se odstraní všechny přidružené prostředky. Nemusíte je tak odstraňovat jeden po druhém.

Protože jste při vytváření aplikace seznamu úkolů vytvořili novou skupinu prostředků, můžete ji bezpečně odstranit, čímž odstraníte všechny přidružené prostředky.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Alternativně můžete skupinu prostředků **sfmeshTutorial1RG** odstranit [z portálu](../azure-resource-manager/management/manage-resource-groups-portal.md#delete-resource-groups). 

## <a name="next-steps"></a>Další kroky

Nyní, když jste dokončili publikování aplikace Service Fabric Mesh do Azure, vyzkoušejte toto:

* Pokud se chcete podívat na další příklad komunikace mezi službami, prohlédněte si [ukázku hlasovací aplikace](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp).
* Další informace o modelu prostředků Service Fabric najdete v článku o [modelu prostředků Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
* Další informace o službě Service Fabric Mesh najdete v článku s [přehledem služby Service Fabric Mesh](service-fabric-mesh-overview.md).
* Přečtěte si o [Cloud Shellu](../cloud-shell/overview.md).
