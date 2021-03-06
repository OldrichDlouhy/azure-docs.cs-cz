---
title: Jaká je hierarchie úložiště Azure NetApp Files | Microsoft Docs
description: Popisuje hierarchii úložiště, včetně účtů, fondů kapacity a svazků služby Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/15/2020
ms.author: b-juche
ms.openlocfilehash: 0b150491fff953434062cc583566e1113947a679
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86504899"
---
# <a name="what-is-the-storage-hierarchy-of-azure-netapp-files"></a>Jaká je hierarchie úložiště Azure NetApp Files

Před vytvořením svazku ve službě Azure NetApp Files musíte zakoupit a nastavit fond pro zřízenou kapacitu.  Pokud chcete nastavit fond kapacity, musíte mít účet NetApp. Pochopení hierarchie úložiště pomáhá nastavit a spravovat prostředky Azure NetApp Files.

> [!IMPORTANT] 
> Azure NetApp Files aktuálně nepodporuje migraci prostředků mezi předplatnými.

## <a name="netapp-accounts"></a><a name="azure_netapp_files_account"></a>Účty NetApp

- Účet NetApp slouží jako správní seskupení základních fondů kapacity.  
- Účet NetApp není stejný jako obecný účet úložiště Azure. 
- Účet NetApp se vztahuje na oblast.   
- V rámci jedné oblasti můžete mít víc účtů NetApp, ale každý účet NetApp se váže pouze k jedné oblasti.

## <a name="capacity-pools"></a><a name="capacity_pools"></a>Fondy kapacit

- Fond kapacity se měří podle jeho zřízené kapacity.  
- Kapacita se zřizuje v pevných SKU, které jste si koupili (například na 4 TiB kapacitě).
- Fond kapacity může mít jenom jednu úroveň služeb.  
- Každý fond kapacit může patřit pouze k jednomu účtu NetApp. V rámci účtu NetApp ale můžete mít několik fondů kapacit.  
- Fond kapacity se nedá mezi účty NetApp přesouvat.   
  Například v následujícím [konceptuálním diagramu hierarchie úložiště](#conceptual_diagram_of_storage_hierarchy) nejde přesunout fond kapacity 1 z účtu NetApp oblasti USA – východ do účtu NetApp oblasti USA – západ 2.  
- Fond kapacit nejde odstranit, dokud se neodstraní všechny svazky v rámci fondu kapacit.

## <a name="volumes"></a><a name="volumes"></a>Svazků

- Svazek se měří podle spotřeby logické kapacity a je škálovatelný. 
- Spotřeba kapacity svazku se počítá proti zřízené kapacitě příslušného fondu.
- Každý svazek patří jenom do jednoho fondu, ale fond může obsahovat několik svazků. 
- Svazek se nedá přesunout mezi fondy kapacit. <!--Within the same NetApp account, you can move a volume across pools.  -->   
  Například v [koncepčním diagramu hierarchie úložiště](#conceptual_diagram_of_storage_hierarchy) můžete přesunout svazky z fondu kapacity 1 do fondu kapacity 2.

## <a name="conceptual-diagram-of-storage-hierarchy"></a><a name="conceptual_diagram_of_storage_hierarchy"></a>Konceptuální diagram hierarchie úložiště 
Následující příklad znázorňuje vztahy předplatného Azure, účtů NetApp, fondů kapacity a svazků.   

![Konceptuální diagram hierarchie úložiště](../media/azure-netapp-files/azure-netapp-files-storage-hierarchy.png)

## <a name="next-steps"></a>Další kroky

- [Omezení prostředků pro službu Azure NetApp Files](azure-netapp-files-resource-limits.md)
- [Registrace služby Azure NetApp Files](azure-netapp-files-register.md)
