---
title: Odstranění uzlů pro řešení VMware podle CloudSimple – Azure
description: Naučte se odstraňovat uzly z nasazení VMWare pomocí CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 122e0636f54e066ae86ed2d19cefe5863b026293
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024734"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-cloudsimple"></a>Odstranění uzlů z řešení Azure VMware podle CloudSimple

Po vytvoření se CloudSimple uzly.  Uzly musí být odstraněny, aby se zastavilo měření uzlů.  Odstraníte uzly, které se nepoužívají z Azure Portal.

## <a name="before-you-begin"></a>Než začnete

Uzel se dá odstranit jenom za následujících podmínek:

* Privátní cloud vytvořený pomocí uzlů je odstraněný.  Informace o odstranění privátního cloudu najdete v tématu [Odstranění řešení Azure VMware pomocí CloudSimple privátního cloudu](delete-private-cloud.md).
* Uzel byl odebrán z privátního cloudu zmenšením privátního cloudu.  Pokud chcete zmenšit privátní cloud, přečtěte si téma [zmenšení řešení Azure VMware pomocí CloudSimple privátního cloudu](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com).

## <a name="delete-cloudsimple-node"></a>Odstranit uzel CloudSimple

1. Vyberte **Všechny služby**.

2. Vyhledejte **uzly CloudSimple**.

   ![Hledat uzly CloudSimple](media/create-cloudsimple-node-search.png)

3. Vyberte **uzly CloudSimple**.

4. Vyberte uzly, které nepatří do privátního cloudu, který se má odstranit.  Sloupec **název privátního cloudu** zobrazuje název privátního cloudu, ke kterému uzel patří.  Pokud uzel není používán privátním cloudem, hodnota bude prázdná. 

    ![Vybrat uzly CloudSimple](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Odstranit lze pouze uzly, které nejsou součástí privátního cloudu.

## <a name="next-steps"></a>Další kroky

* Informace o [privátním cloudu](cloudsimple-private-cloud.md)
