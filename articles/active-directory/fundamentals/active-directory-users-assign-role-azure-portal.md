---
title: Přiřazení rolí Azure AD uživatelům – Azure Active Directory | Microsoft Docs
description: Pokyny, jak přiřadit role správců a uživatelů bez oprávnění správce k Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 06/15/2020
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5dc42d8308b8e20a647f5f64867fd78a1a09c96
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223997"
---
# <a name="assign-administrator-and-non-administrator-roles-to-users-with-azure-active-directory"></a>Přiřazení rolí správců a uživatelů bez oprávnění správce k Azure Active Directory

Pokud v Azure Active Directory (Azure AD) potřebuje některý z uživatelů oprávnění ke správě prostředků Azure AD, musíte jim přiřadit roli, která poskytuje potřebná oprávnění. Informace o tom, které role spravují prostředky Azure a které role spravují prostředky Azure AD, najdete v tématu [role správců klasického předplatného, role Azure a role Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

Další informace o dostupných rolích služby Azure AD najdete v tématu [přiřazení rolí správce v Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Pokud chcete přidat uživatele, přečtěte si téma [Přidání nových uživatelů do Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Přiřazení rolí

Běžný způsob, jak přiřadit role Azure AD uživateli, je na stránce **přiřazené role** pro uživatele. Můžete také poskytnout oprávnění ke zvýšení úrovně za běhu do role pomocí Privileged Identity Management (PIM). Další informace o tom, jak používat PIM, najdete v tématu [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

> [!Note]
> Pokud máte plán licencí Azure AD Premium P2 a už používáte PIM, provedou se všechny úlohy správy rolí v [prostředí Privileged Identity Management](../users-groups-roles/directory-manage-roles-portal.md).
>
> ![Role Azure AD spravované v PIM pro uživatele, kteří už používají PIM a mají licenci Premium P2](./media/active-directory-users-assign-role-azure-portal/pim-manages-roles-for-p2.png)

## <a name="assign-a-role-to-a-user"></a>Přiřazení role uživateli

1. Pro adresář přejdete na [Azure Portal](https://portal.azure.com/) a přihlaste se pomocí účtu globálního správce.

2. Vyhledejte a vyberte **Azure Active Directory**.

      ![Azure Portal hledání Azure Active Directory](media/active-directory-users-assign-role-azure-portal/search-azure-active-directory.png)

3. Vyberte **Uživatelé**.

4. Vyhledejte a vyberte uživatele, který získává přiřazení role. Například _Alain Charon_.

      ![Stránka všichni uživatelé – vybrat uživatele](media/active-directory-users-assign-role-azure-portal/directory-role-select-user.png)

5. Na stránce **Alain Charon-profil** vyberte **přiřazené role**.

    Zobrazí se stránka **Alain Charon – role pro správu** .

6. Vyberte **Přidat přiřazení**, vyberte roli, kterou chcete přiřadit Alain (například _správce aplikace_), a pak zvolte **Vybrat**.

    ![Stránka přiřazených rolí – zobrazuje vybranou roli](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    Role správce aplikace je přiřazená Alain Charon a zobrazí se na stránce **role Alain Charon-administrativa** .

## <a name="remove-a-role-assignment"></a>Odebrání přiřazení role

Pokud potřebujete odebrat přiřazení role z uživatele, můžete to udělat taky ze stránky **role pro správu Alain Charon** .

### <a name="to-remove-a-role-assignment-from-a-user"></a>Odebrání přiřazení role z uživatele

1. Vyberte **Azure Active Directory**, vyberte **uživatele**a pak vyhledejte a vyberte uživatele, který získal přiřazení role odebrané. Například _Alain Charon_.

2. Vyberte **přiřazené role**, vyberte **správce aplikace**a pak vyberte **odebrat přiřazení**.

    ![Stránka přiřazené role, zobrazení vybrané role a možnosti odebrat](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    Role správce aplikace se odebere z Alain Charon a už se nezobrazuje na stránce **role Alain Charon-administrativa** .

## <a name="next-steps"></a>Další kroky

- [Přidání nebo odstranění uživatelů](add-users-azure-active-directory.md)

- [Přidat nebo změnit informace o profilu](active-directory-users-profile-azure-portal.md)

- [Přidání uživatelů typu host z jiného adresáře](../b2b/what-is-b2b.md)

Další úlohy správy uživatelů, které můžete rezervovat, jsou k dispozici v [dokumentaci Azure Active Directory správy uživatelů](../users-groups-roles/index.yml).
