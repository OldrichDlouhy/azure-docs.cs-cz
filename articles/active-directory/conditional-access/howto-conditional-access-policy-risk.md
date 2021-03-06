---
title: Podmíněný přístup na základě rizik přihlašování – Azure Active Directory
description: Vytvoření zásad podmíněného přístupu pomocí rizika přihlašování Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce687ae1f47b20bb5fff3827e7bcbd5d7edf2d83
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024355"
---
# <a name="conditional-access-sign-in-risk-based-conditional-access"></a>Podmíněný přístup: podmíněný přístup na základě rizik přihlašování

Většina uživatelů se chová běžným způsobem, který je možné sledovat. Když se mimo tento běžný způsob vychýlí, může být nebezpečné jim povolit se normálně přihlásit. Je možné, že budete chtít tohoto uživatele zablokovat nebo ho můžete jednoduše požádat, aby provedl ověřování pomocí vícefaktorového ověřování, aby prokázal, že jsou ve skutečnosti, na kterých říkají. 

Riziko přihlášení představuje pravděpodobnost, že daný požadavek na ověření není autorizovaný vlastníkem identity. Organizace s Azure AD Premium licencemi P2 můžou vytvářet zásady podmíněného přístupu, které obsahují [Azure AD Identity Protection rozpoznávání rizik při přihlašování](../identity-protection/concept-identity-protection-risks.md#sign-in-risk).

Existují dvě místa, kde je možné tuto zásadu přiřadit. Organizace by si měli vybrat jednu z následujících možností, aby bylo možné povolit zásady podmíněného přístupu na základě rizik přihlašování vyžadující zabezpečenou změnu hesla.

## <a name="enable-with-conditional-access-policy"></a>Povolit se zásadami podmíněného přístupu

1. Přihlaste se k **Azure Portal** jako globální správce, správce zabezpečení nebo správce podmíněného přístupu.
1. Vyhledejte **Azure Active Directory**  >  **Security**  >  **podmíněný přístup**zabezpečení.
1. Vyberte **nové zásady**.
1. Zadejte název zásady. Pro názvy svých zásad doporučujeme organizacím vytvořit smysluplný Standard.
1. V části **Přiřazení** vyberte **Uživatelé a skupiny**.
   1. V části **Zahrnout**vyberte **Všichni uživatelé**.
   1. V části **vyloučit**vyberte **Uživatelé a skupiny** a zvolte účty pro nouzový přístup nebo rozklad vaší organizace. 
   1. Vyberte **Done** (Hotovo).
1. V části **cloudové aplikace nebo akce**  >  **Include**vyberte **všechny cloudové aplikace**.
1. V části **podmínky**  >  **rizika uživatele**nastavte **Konfigurovat** na **Ano**. V části **Vyberte úroveň rizika přihlašování, na kterou se budou tyto zásady vztahovat** . 
   1. Vyberte **vysoké** a **střední**.
   1. Vyberte **Done** (Hotovo).
1. V části **řízení přístupu**  >  **udělení**přístupu vyberte **udělit přístup**, **vyžadovat vícefaktorové ověřování**a vyberte **Vybrat**.
1. Potvrďte nastavení a nastavte **možnost povolit zásadu** na **zapnuto**.
1. Vyberte **vytvořit** a vytvořte tak, aby se zásady povolily.

## <a name="enable-through-identity-protection"></a>Povolit prostřednictvím Identity Protection

1. Přihlaste se k **portálu Azure Portal**.
1. Vyberte **všechny služby**a pak vyhledejte **Azure AD Identity Protection**.
1. Vyberte **zásady rizik přihlašování**.
1. V části **přiřazení**vyberte **Uživatelé**.
   1. V části **Zahrnout**vyberte **Všichni uživatelé**.
   1. V části **vyloučit**vyberte **Vybrat vyloučené uživatele**, zvolte nouzový přístup nebo účty pro přerušení vaší organizace a vyberte **Vybrat**.
   1. Vyberte **Done** (Hotovo).
1. V části **podmínky**vyberte možnost **riziko přihlášení**a pak zvolte **střední a vyšší**.
   1. Vyberte **Vybrat**a pak **Hotovo**.
1. V části **ovládací prvky**  >  **přístup**zvolte možnost **povolený přístup**a pak vyberte **vyžadovat vícefaktorové ověřování**.
   1. Vyberte **Vybrat**.
1. Nastavte **zásadu Vynutilit** na **zapnuto**.
1. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky

[Společné zásady podmíněného přístupu](concept-conditional-access-policy-common.md)

[Podmíněný přístup na základě rizik uživatelů](howto-conditional-access-policy-risk-user.md)

[Určení dopadu pomocí režimu pouze sestavy podmíněného přístupu](howto-conditional-access-report-only.md)

[Simulace chování při přihlašování pomocí nástroje pro What If podmíněného přístupu](troubleshoot-conditional-access-what-if.md)

[Co je Azure Active Directory Identity Protection?](../identity-protection/overview.md)
