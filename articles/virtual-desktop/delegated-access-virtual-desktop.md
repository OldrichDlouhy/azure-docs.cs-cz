---
title: Delegovaný přístup na virtuálním počítači s Windows – Azure
description: Jak delegovat možnosti správy pro nasazení virtuálních klientů s Windows, včetně příkladů.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2dc96f587a9e5db9d9810a4d1ab7d32c4ff49f7d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289865"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Delegovaný přístup ve Windows Virtual Desktop

>[!IMPORTANT]
>Tento obsah se vztahuje na virtuální plochu Windows s Azure Resource Manager objekty virtuálních klientů Windows. Pokud používáte virtuální plochu Windows (Classic) bez Azure Resource Manager objektů, přečtěte si [Tento článek](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md).

Virtuální klient Windows má delegovaný přístupový model, který umožňuje definovat množství přístupu, které může určitý uživatel přiřadit roli. Přiřazení role má tři součásti: objekt zabezpečení, definice role a obor. Model delegovaného přístupu k virtuálním plochám Windows je založený na modelu Azure RBAC. Další informace o konkrétních přiřazeních rolí a jejich součástech najdete v tématu [Přehled řízení přístupu na základě role v Azure](../role-based-access-control/built-in-roles.md).

Delegovaný přístup k virtuálním plochám Windows podporuje následující hodnoty pro každý prvek přiřazení role:

* Objekt zabezpečení
    * Uživatelé
    * Skupiny uživatelů
    * Instanční objekty
* Definice role
    * Vestavěné role
    * Vlastní role
* Rozsah
    * Fondy hostitelů
    * Skupiny aplikací
    * Pracovní prostory

## <a name="powershell-cmdlets-for-role-assignments"></a>Rutiny PowerShellu pro přiřazení rolí

Než začnete, nezapomeňte postupovat podle pokynů v tématu [nastavení modulu PowerShellu](powershell-module.md) pro nastavení modulu PowerShellu pro virtuální počítače s Windows, pokud jste to ještě neudělali.

Virtuální počítač s Windows používá řízení přístupu na základě role (RBAC) Azure při publikování skupin aplikací uživatelům nebo skupinám uživatelů. Role uživatele virtualizace plochy je přiřazena uživateli nebo skupině uživatelů a oborem je skupina aplikací. Tato role poskytuje uživateli speciální přístup k datům ve skupině aplikací.  

Spuštěním následující rutiny přidejte Azure Active Directory uživatele do skupiny aplikací:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'  
```

Spuštěním následující rutiny přidejte Azure Active Directory skupinu uživatelů do skupiny aplikací:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

## <a name="next-steps"></a>Další kroky

Podrobnější seznam rutin PowerShellu, které můžou jednotlivé role použít, najdete v [referenčních informacích k PowerShellu](/powershell/windows-virtual-desktop/overview).

Úplný seznam rolí podporovaných v Azure RBAC najdete v tématu [předdefinované role Azure](../role-based-access-control/built-in-roles.md).

Pokyny, jak nastavit prostředí virtuálních počítačů s Windows, najdete v tématu [prostředí virtuálních počítačů s Windows](environment-setup.md).
