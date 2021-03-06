---
title: Co je spolupráce B2B v Azure Active Directory?
description: Spolupráce B2B v Azure Active Directory podporuje přístup uživatelů typu host, abyste mohli s externími partnery bezpečně sdílet prostředky a spolupracovat s nimi.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89e1868d8e9a346d5a791c9fa8ae7682ca2b6807
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905132"
---
# <a name="what-is-guest-user-access-in-azure-active-directory-b2b"></a>Co je přístup uživatelů typu host při spolupráci B2B v Azure Active Directory?

Při spolupráci B2B (business-to-business) v Azure Active Directory (Azure AD) můžete bezpečně sdílet aplikace a služby společnosti s uživateli typu host z libovolné organizace a současně si zachovat kontrolu nad vlastními podnikovými daty. Spolupracujte bezpečně s velkými i malými externími partnery, i když nemají Azure AD ani oddělení IT. Díky jednoduchému postupu založenému na pozvánce a jejím uplatnění můžou partneři, kteří přistupují k vašim firemním prostředkům, používat vlastní přihlašovací údaje. Vývojáři můžou v rozhraní API pro spolupráci B2B v Azure AD přizpůsobit postup pozvání nebo napsat aplikace, jako jsou portály pro samoobslužnou registraci.

Podívejte se na video, ve kterém se dozvíte, jak bezpečně spolupracovat s uživateli typu host tím, že je pozvete, aby k přihlášení do vašich podnikových aplikací a služeb používali vlastní identity.

Následující video nabízí praktický přehled.

>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

   > [!IMPORTANT]
   > **Od 31. března 2021**přestane společnost Microsoft podporovat uplatnění pozvánky tím, že pro scénáře spolupráce B2B vytvoří nespravované účty a klienty Azure AD. V přípravě doporučujeme zákazníkům, aby se přihlásili k [e-mailu ověřování jednorázovým heslem](one-time-passcode.md). Uvítáme vaše názory na tuto funkci Public Preview a zajímáme si vytváření ještě více způsobů, jak spolupracovat.

## <a name="collaborate-with-any-partner-using-their-identities"></a>Spolupráce s partnery s využitím jejich vlastních identit

Při spolupráci B2B v Azure AD partner používá ke správě identit vlastní řešení. Vaší organizaci tak v této souvislosti nevznikají další režijní náklady.

- Partner používá vlastní identity a přihlašovací údaje, a nepotřebuje k tomu Azure AD.
- Nemusíte spravovat externí účty ani hesla.
- Nemusíte synchronizovat účty ani spravovat jejich životní cyklus.  

![Snímek obrazovky zobrazující stránku přidat členy](media/what-is-b2b/add-member.png)

## <a name="invite-guest-users-with-a-simple-invitation-and-redemption-process"></a>Pozvání uživatelů typu host prostřednictvím jednoduché pozvánky a jejího uplatnění

Uživatelé typu host používají při přihlášení k vašim aplikacím a službám vlastní pracovní, školní nebo sociální identitu. Pokud uživatel typu Host nemá účet účet Microsoft nebo Azure AD, vytvoří se pro ně při uplatnění své pozvánky jedna z nich. 

- K pozvání uživatelů typu host použijte e-mailovou identitu, kterou si sami vyberou.
- Pošlete jim přímo odkaz na aplikaci nebo jim pošlete pozvánku do vlastního přístupového panelu uživatele typu host.
- Uživatelé typu host provedou při přihlášení a uplatnění pozvánky několik jednoduchých kroků.

![Snímek obrazovky se stránkou pro kontrolu oprávnění](media/what-is-b2b/consentscreen.png)

## <a name="use-policies-to-securely-share-your-apps-and-services"></a>Použití zásad při bezpečném sdílení aplikací a služeb

K ochraně firemního obsahu můžete použít zásady autorizace. Zásady podmíněného přístupu, jako je Multi-Factor Authentication, se dají vymáhat:

- Na úrovni tenanta
- Na úrovni aplikace
- Pro konkrétní uživatele typu host, abyste chránili firemní aplikace a data

![Snímek obrazovky znázorňující možnost podmíněný přístup](media/what-is-b2b/tutorial-mfa-policy-2.png)


## <a name="easily-add-guest-users-in-the-azure-ad-portal"></a>Jednoduché přidání uživatelů typu host na webu Azure Portal

Jako správce můžete do své organizace na webu Azure Portal jednoduše přidat uživatele typu host.

- V Azure AD můžete vytvořit nového uživatele typu host podobně, jako byste přidávali nového uživatele.
- Uživateli typu host ihned přijde přizpůsobená pozvánka, ze které se může přihlásit ke svému přístupovému panelu.
- Uživatele typu host můžete v adresáři přiřazovat k aplikacím nebo skupinám.  

![Snímek obrazovky se stránkou pro novou položku pozvánky uživatele hosta](media/what-is-b2b/add-a-b2b-user-to-azure-portal.png)

## <a name="let-application-and-group-owners-manage-their-own-guest-users"></a>Nechte vlastníky aplikací a skupin spravovat vlastní uživatele typu host

Správu uživatelů typu host můžete delegovat na vlastníky aplikací, aby do libovolné aplikace, kterou chtějí sdílet, mohli přímo přidávat uživatele typu host. Přitom nezáleží, jestli je aplikace od Microsoftu.

- Správci nastaví samoobslužnou správu aplikace a skupiny.
- Uživatelé, kteří nejsou správci, použijí k přidání uživatelů typu host do aplikací nebo skupin [přístupový panel](https://myapps.microsoft.com).

![Snímek obrazovky zobrazující přístupový panel pro uživatele typu Host](media/what-is-b2b/access-panel-manage-app.png)

## <a name="customize-the-onboarding-experience-for-b2b-guest-users"></a>Přizpůsobení prostředí připojování pro uživatele typu Host B2B

Přizpůsobte svým externím partnerům způsoby přizpůsobení potřebám vaší organizace.

- Využijte [správu nároků Azure AD](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) ke konfiguraci zásad, které [spravují přístup pro externí uživatele](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users#how-access-works-for-external-users).
- Použijte [rozhraní API pro pozvánky B2B pro spolupráci](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) k přizpůsobení prostředí připojování.

## <a name="integrate-with-identity-providers"></a>Integrace s poskytovateli identity

Azure AD podporuje externí zprostředkovatele identity, jako je Facebook, účty Microsoft, Google nebo poskytovatelé podnikových identit. Můžete nastavit federaci s poskytovateli identity, aby se externí uživatelé mohli přihlašovat pomocí svých stávajících sociálních nebo podnikových účtů, a nemusíte vytvářet nový účet jenom pro vaši aplikaci. Přečtěte si další informace o zprostředkovatelích identity pro externí identity.

![Snímek obrazovky zobrazující stránku zprostředkovatelé identit](media/what-is-b2b/identity-providers.png)


## <a name="create-a-self-service-sign-up-user-flow-preview"></a>Vytvoření samoobslužného uživatelského toku pro registraci (Preview)

Pomocí samoobslužného uživatelského toku registrace můžete vytvořit prostředí pro registraci externích uživatelů, kteří chtějí přistupovat k aplikacím. V rámci toku registrace můžete poskytnout možnosti pro různé poskytovatele identity v sociálních sítích nebo podnikových organizacích a shromažďovat informace o uživateli. Přečtěte si o [samoobslužném přihlášení a způsobu jeho nastavení](self-service-sign-up-overview.md).

[Konektory rozhraní API](api-connectors-overview.md) můžete použít také k integraci uživatelských toků samoobslužné registrace k externím cloudovým systémům. Můžete se spojit s vlastními pracovními postupy schvalování, provádět ověřování identity, ověřovat informace poskytované uživateli a další.

![Snímek obrazovky zobrazující stránku toků uživatelů](media/what-is-b2b/self-service-sign-up-user-flow-overview.png)
<!--TODO: Add screenshot with API connectors -->

## <a name="next-steps"></a>Další kroky

- [Pokyny k licencování spolupráce B2B v Azure AD](licensing-guidance.md)
- [Přidání uživatelů typu host na portálu pro spolupráci B2B](add-users-administrator.md)
- [Vysvětlení postupu při uplatnění pozvánky](redemption-experience.md)
- A jako vždy se můžete spojit s naším produktovým týmem a sdělit mu své připomínky, diskutovat nebo posílat své návrhy prostřednictvím [technické komunity Microsoftu](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
