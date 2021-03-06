---
title: Uživatelské prostředí s Azure AD Identity Protection
description: Činnost koncového uživatele Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc10fb4f9894a355c9eed024ae9f87747214999b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "72886997"
---
# <a name="user-experiences-with-azure-ad-identity-protection"></a>Uživatelské prostředí s Azure AD Identity Protection

Pomocí Azure Active Directory Identity Protection můžete:

* Vyžadovat, aby se uživatelé zaregistrovali pro Azure Multi-Factor Authentication (MFA)
* Automatizovaná náprava rizikových přihlášení a ohrožených uživatelů

Všechny zásady ochrany identity mají dopad na přihlašovací prostředí pro uživatele. Umožnění, aby se uživatelé zaregistrovali a používali nástroje jako Azure MFA a Samoobslužné resetování hesla, můžou dopad snížit. Tyto nástroje spolu s příslušnými volbami zásad poskytují uživatelům možnost automatického nápravy, když ji potřebují.

## <a name="multi-factor-authentication-registration"></a>Registrace Multi-Factor Authentication

Když zapnete zásadu ochrany identit, která vyžaduje registraci Multi-Factor Authentication a cílíte na všechny vaše uživatele, bude mít možnost využít Azure MFA k samoobslužné nápravě v budoucnu. Po nakonfigurování této zásady budou mít uživatelé po dobu 14 dní, kdy se můžou rozhodnout zaregistrovat a na konci, se budou muset zaregistrovat. Níže jsou uvedené možnosti pro uživatele. Další informace najdete v dokumentaci pro koncové uživatele v článku [Přehled dvou faktorů ověřování a pracovního nebo školního účtu](../user-help/user-help-two-step-verification-overview.md).

### <a name="registration-interrupt"></a>Přerušení registrace

1. Při přihlášení k libovolné aplikaci integrované v Azure AD získá uživatel oznámení o požadavku na nastavení účtu pro službu Multi-Factor Authentication. Tato zásada se taky aktivuje v prostředí Windows 10 pro nové uživatele s novým zařízením.
   
    ![Jsou požadovány další informace](./media/concept-identity-protection-user-experience/identity-protection-experience-more-info-mfa.png)

1. Dokončete kroky průvodce pro registraci k Azure Multi-Factor Authentication a dokončete přihlášení.

## <a name="risky-sign-in-remediation"></a>Riziková náprava při přihlašování

Když správce nakonfiguroval zásadu pro rizika přihlašování, budou mít postižení uživatelé oznámení, že se pokusí přihlásit a aktivovat úroveň rizika zásad. 

### <a name="risky-sign-in-self-remediation"></a>Rizikové přihlašování při automatické nápravě

1. Uživatel je informován o tom, že při přihlašování byl zjištěn nějaký neobvyklý, například přihlašování z nového umístění, zařízení nebo aplikace.
   
    ![Něco neobvyklého dotazu](./media/concept-identity-protection-user-experience/120.png)

1. Uživatel musí prokázat svoji identitu tím, že dokončí Azure MFA s jednou z dříve registrovaných metod. 

### <a name="risky-sign-in-administrator-unblock"></a>Odblokování rizik správce přihlášení

Správci se můžou rozhodnout blokování uživatelů při přihlášení v závislosti na jejich úrovni rizika. Chcete-li se odblokovat, musí se koncoví uživatelé obrátit na pracovníky IT, jinak se můžou pokusit přihlásit ze známého umístění nebo zařízení. V tomto případě není možnost samoobslužné nápravy prováděná službou Multi-Factor Authentication.

![Blokováno zásadami rizik přihlašování](./media/concept-identity-protection-user-experience/200.png)

Pracovníci IT můžou postupovat podle pokynů v části zrušení [blokování uživatelů](howto-identity-protection-remediate-unblock.md#unblocking-based-on-sign-in-risk) , aby se uživatelé mohli znovu přihlašovat.

## <a name="risky-user-remediation"></a>Riziková náprava uživatelů

Když se nakonfigurovaly zásady rizik uživatelů, musí si uživatelé, kteří splňují riziko ohrožení zabezpečení uživatele, projít tok obnovení před tím, než se budou moct přihlásit. 

### <a name="risky-user-self-remediation"></a>Riziková náprava uživatele

1. Uživatel je informován o ohrožení zabezpečení jejich účtu z důvodu podezřelé aktivity nebo nevrácených přihlašovacích údajů.
   
    ![Odstranění rizika](./media/concept-identity-protection-user-experience/101.png)

1. Uživatel musí prokázat svoji identitu tím, že dokončí Azure MFA s jednou z dříve registrovaných metod. 
1. Nakonec uživatel bude muset změnit heslo pomocí samoobslužného resetování hesla, protože někdo jiný mohl mít přístup ke svému účtu.

## <a name="risky-sign-in-administrator-unblock"></a>Odblokování rizik správce přihlášení

Správci se můžou rozhodnout blokování uživatelů při přihlášení v závislosti na jejich úrovni rizika. Chcete-li se odblokovat, musí se koncoví uživatelé obrátit na pracovníky IT. Automatické opravy pomocí služby Multi-Factor Authentication a samoobslužného resetování hesla není v tomto případě možnost.

![Blokováno zásadami rizika uživatele](./media/concept-identity-protection-user-experience/104.png)

Pracovníci IT můžou postupovat podle pokynů v části zrušení [blokování uživatelů](howto-identity-protection-remediate-unblock.md#unblocking-based-on-user-risk) , aby se uživatelé mohli znovu přihlašovat.

## <a name="see-also"></a>Viz také:

- [Oprava rizik a odblokování uživatelů](howto-identity-protection-remediate-unblock.md)

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) 
