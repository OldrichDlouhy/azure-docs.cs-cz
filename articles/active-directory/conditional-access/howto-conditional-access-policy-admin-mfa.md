---
title: Podmíněný přístup – vyžaduje MFA pro správce – Azure Active Directory
description: Vytvoření vlastních zásad podmíněného přístupu, které vyžadují, aby správci prováděli vícefaktorové ověřování
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a2830db6132e665d3f876a535fb645e936971b2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87283057"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Podmíněný přístup: vyžadovat MFA pro správce

Účtům, kterým jsou přiřazena práva pro správu, cílí útočníci. Vyžadování vícefaktorového ověřování (MFA) u těchto účtů představuje snadný způsob, jak snížit riziko narušení těchto účtů.

Společnost Microsoft doporučuje vyžadovat vícefaktorové ověřování na následujících rolích minimálně:

* Správce fakturace
* Správce podmíněného přístupu
* Správce Exchange
* Globální správce
* HelpDesk (heslo) – správce
* Správce hesel
* Správce zabezpečení
* Správce SharePointu
* Správce uživatelů

Organizace si můžou zvolit, že se mají zahrnout nebo vyloučit role podle potřeby.

## <a name="user-exclusions"></a>Vyloučení uživatelů

Zásady podmíněného přístupu jsou výkonné nástroje. doporučujeme, abyste z zásad vyloučili následující účty:

* **Nouzový přístup** nebo účty pro **přerušení** , které zabrání uzamknutí účtu na úrovni tenanta. V nepravděpodobném scénáři jsou všichni správci zamčeni z vašeho tenanta, účet pro správu pro nouzový přístup se dá použít k přihlášení k tenantovi a přijímá kroky pro obnovení přístupu.
   * Další informace najdete v článku [Správa účtů pro nouzový přístup ve službě Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Účty služeb** a **instanční objekty**, jako je například účet Azure AD Connect Sync. Účty služeb jsou neinteraktivní účty, které nejsou vázány na konkrétního uživatele. Obvykle jsou používány back-endové služby, které umožňují programový přístup k aplikacím, ale používají se také pro přihlášení k systémům pro účely správy. Účty služby by měly být vyloučené, protože MFA nelze dokončit programově. Podmíněný přístup neblokuje volání prováděná instančními objekty.
   * Pokud má vaše organizace tyto účty používané ve skriptech nebo v kódu, zvažte jejich nahrazení pomocí [spravovaných identit](../managed-identities-azure-resources/overview.md). Jako dočasné řešení můžete tyto konkrétní účty z základní zásady vyloučit.

## <a name="create-a-conditional-access-policy"></a>Vytvoření zásady podmíněného přístupu

Následující kroky vám pomůžou vytvořit zásadu podmíněného přístupu, která bude vyžadovat, aby tyto přiřazené role správce prováděly službu Multi-Factor Authentication.

1. Přihlaste se k **Azure Portal** jako globální správce, správce zabezpečení nebo správce podmíněného přístupu.
1. Vyhledejte **Azure Active Directory**  >  **Security**  >  **podmíněný přístup**zabezpečení.
1. Vyberte **nové zásady**.
1. Zadejte název zásady. Pro názvy svých zásad doporučujeme organizacím vytvořit smysluplný Standard.
1. V části **přiřazení**vyberte **Uživatelé a skupiny** .
   1. V části **Zahrnout**vyberte **role adresáře (Preview)** a minimálně jednu z následujících rolí:
      * Správce ověřování
      * Správce fakturace
      * Správce podmíněného přístupu
      * Správce Exchange
      * Globální správce
      * Správce helpdesku
      * Správce hesel
      * Správce zabezpečení
      * Správce SharePointu
      * Správce uživatelů
   1. V části **vyloučit**vyberte **Uživatelé a skupiny** a zvolte účty pro nouzový přístup nebo rozklad vaší organizace. 
   1. Vyberte **Hotovo**.
1. V části **cloudové aplikace nebo akce**  >  **Include**vyberte **všechny cloudové aplikace**a vyberte **Hotovo**.
1. V části **podmínky**  >  **klientské aplikace (Preview)** v části **Vyberte klientské aplikace, na které se bude tato zásada vztahovat, aby** se vybrala všechna výchozí nastavení a vyberte **Hotovo**.
1. V části **řízení přístupu**  >  **udělení**přístupu vyberte **udělit přístup**, **vyžadovat vícefaktorové ověřování**a vyberte **Vybrat**.
1. Potvrďte nastavení a nastavte **možnost povolit zásadu** na **zapnuto**.
1. Vyberte **vytvořit** a vytvořte tak, aby se zásady povolily.

## <a name="next-steps"></a>Další kroky

[Společné zásady podmíněného přístupu](concept-conditional-access-policy-common.md)

[Určení dopadu pomocí režimu pouze sestavy podmíněného přístupu](howto-conditional-access-report-only.md)

[Simulace chování při přihlašování pomocí nástroje pro What If podmíněného přístupu](troubleshoot-conditional-access-what-if.md)
