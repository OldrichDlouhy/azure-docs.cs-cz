---
title: Řešení potíží se spoluprací B2B – Azure Active Directory | Microsoft Docs
description: Náprava pro běžné problémy s Azure Active Directory spolupráce B2B
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: troubleshooting
ms.date: 03/19/2020
tags: active-directory
ms.author: mimart
author: msmimart
ms.reviewer: mal
ms.custom:
- it-pro
- seo-update-azuread-jan"
ms.collection: M365-identity-device-management
ms.openlocfilehash: a835872d4a4fedfb5ef72a1cb9499e4f969adcd3
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202102"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Řešení potíží s Azure Active Directory spolupráce B2B

Tady je několik oprav pro běžné problémy s spoluprací B2B Azure Active Directory (Azure AD).

   > [!IMPORTANT]
   > **Od 31. března 2021**přestane společnost Microsoft podporovat uplatnění pozvánky tím, že pro scénáře spolupráce B2B vytvoří nespravované účty a klienty Azure AD. V přípravě doporučujeme zákazníkům, aby se přihlásili k [e-mailu ověřování jednorázovým heslem](one-time-passcode.md). Uvítáme vaše názory na tuto funkci Public Preview a zajímáme si vytváření ještě více způsobů, jak spolupracovat.

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>Přidal (a) jsem externího uživatele, ale nevidí ho v globálním adresáři nebo v rámci výběru osob.

V případech, kdy v seznamu nejsou naplněny externí uživatelé, může replikace trvat několik minut.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Uživatel typu Host B2B se nezobrazuje na SharePointu Online/na OneDrivu pro výběr osob.

Možnost Hledat stávající uživatele typu Host ve výběru osob v SharePointu Online (SPO) je ve výchozím nastavení VYPNUTá, aby odpovídala staršímu chování.

Tuto funkci můžete povolit pomocí nastavení ' ShowPeoplePickerSuggestionsForGuestUsers ' na úrovni tenanta a kolekce webů. Funkci můžete nastavit pomocí rutin set-SPOTenant a set-SPOSite, které umožní členům Hledat v adresáři všechny existující uživatele typu Host. Změny v oboru tenanta neovlivňují již zřízené SPO lokality.

## <a name="invitations-have-been-disabled-for-directory"></a>Pozvánky byly pro adresář zakázané.

Pokud jste upozorněni, že nemáte oprávnění pozvat uživatele, ověřte, zda je uživatelský účet autorizován pro pozvání externích uživatelů v části Azure Active Directory > nastavení uživatele > externích uživatelů > spravovat nastavení externí spolupráce:

![Snímek obrazovky zobrazující nastavení externích uživatelů](media/troubleshoot/external-user-settings.png)

Pokud jste tato nastavení nedávno změnili nebo přiřadili k uživateli roli pozvat hosta, může se stát, že se změny projeví 15-60 minut.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>Uživatel, kterého jsem pozval, obdrží při uplatnění chyby chybu

Mezi běžné chyby patří:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>Správce pozvání nepovolil, aby se EmailVerified uživatelé ve svém tenantovi vytvořili.

Při pozvání uživatelů, jejichž organizace používá Azure Active Directory, ale pokud účet konkrétního uživatele neexistuje (například uživatel v Azure AD contoso.com neexistuje). Správce contoso.com může mít zavedenou zásadu zabraňující vytváření uživatelů. Uživatel musí u svého správce zjistit, jestli je povolený externí uživatel. Správce externího uživatele může potřebovat povolit e-mailem ověřeným uživatelům ve své doméně (viz tento [článek](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) , který umožňuje ověřeným uživatelům poslat e-mail).

![Chyba informující, že tenant nepovoluje uživatelům ověřené e-maily](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Externí uživatel už v federované doméně neexistuje.

Pokud používáte ověřování federace a uživatel již v Azure Active Directory neexistuje, nelze uživatele pozvat.

Aby bylo možné tento problém vyřešit, musí správce externího uživatele synchronizovat účet uživatele s Azure Active Directory.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Jak \# je, což není obvykle platný znak, se synchronizuje se službou Azure AD?

\#je vyhrazený znak v UPN pro spolupráci Azure AD B2B nebo externí uživatele, protože pozvaný účet user@contoso.com se user_contoso. com # ext # @fabrikam.onmicrosoft.com . Proto se \# v UPN přicházejících z místního prostředí nemůžete přihlásit k Azure Portal. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Při přidávání externích uživatelů do synchronizované skupiny se zobrazí chyba

Externí uživatele je možné přidat pouze do skupin přiřazených nebo zabezpečení, nikoli do skupin, které jsou v místním prostředí.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Můj externí uživatel nedostal e-mail k uplatnění.

Pozvánka by měla zkontrolovat pomocí filtru poskytovatele internetových služeb nebo spamu, aby bylo zajištěno, že je povolena následující adresa:Invites@microsoft.com

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Všimněte si, že vlastní zpráva nezahrnuje zprávy pozvánky v časech

V rámci dodržování zákonů na ochranu osobních údajů naše rozhraní API do e-mailové pozvánky neobsahují vlastní zprávy v těchto případech:

- Pozvánka nemá v tenantovi pro pozvánku e-mailovou adresu.
- Když objekt zabezpečení AppService odešle pozvánku

Pokud je tento scénář pro vás důležitý, můžete potlačit e-mail s pozvánkami rozhraní API a poslat ho e-mailovým mechanismem podle vašeho výběru. Projděte si právní poradce pro organizaci a ujistěte se, že všechny e-maily, které odesíláte tímto způsobem, jsou také v souladu s zákony

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>Při pokusu o přihlášení k prostředku Azure se zobrazí chyba "AADSTS65005".

Uživatel, který má účet Guest, se nemůže přihlásit a přijímá následující chybovou zprávu:

```plaintext
    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.
```

Uživatel má uživatelský účet Azure a je to virového tenanta, který byl opuštěn nebo nespravovaný. Kromě toho neexistují žádní globální správci nebo správci společnosti v tenantovi.

Chcete-li tento problém vyřešit, musíte převzít klienta, který se zrušil. Přečtěte si, jak [přebírat nespravovaný adresář jako správce v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover). K tomu, aby bylo možné poskytnout přímý důkaz, který ovládáte v oboru názvů, musíte mít také přístup k internetovému serveru DNS pro příslušnou příponu domény. Až se klient vrátí do spravovaného stavu, poradí se se zákazníkem, ať už se jedná o nejlepší možnost pro svou organizaci.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Uživatel typu host s klientem za běhu nebo virového tenanta nemůže resetovat heslo.

Pokud je tenant identity za běhu (JIT) nebo virového tenanta (což znamená, že se jedná o samostatného nespravovaného tenanta Azure), může heslo resetovat jenom uživatel typu Host. Někdy organizace [převezme správu virového tenanta](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) , které se vytvoří, když zaměstnanci použijí své pracovní e-mailové adresy k registraci služeb. Až organizace převezme klienta virového, může resetování hesla uživatele nebo povolení SSPR jenom správce v této organizaci. V případě potřeby můžete jako pozvánku do organizace odebrat uživatelský účet hosta z adresáře a znovu odeslat pozvánku.

## <a name="a-guest-user-is-unable-to-use-the-azuread-powershell-v1-module"></a>Uživatel typu Host nemůže použít modul AzureAD PowerShell v1.

Od 18. listopadu 2019 se zablokují uživatelům typu Host v adresáři (definovaným jako uživatelské účty, u kterých se vlastnost **userType** rovná **Host**), aby používal modul AzureAD PowerShell v1. V opačném případě bude uživatel muset být buď členem uživatele (kde **userType** má hodnotu **člen**), nebo použít modul AzureAD PowerShell v2.

## <a name="in-an-azure-us-government-tenant-i-cant-invite-a-b2b-collaboration-guest-user"></a>V tenantovi Azure pro státní správu USA nemůžu pozvat uživatele typu Host pro spolupráci B2B.

V cloudu pro státní správu Azure USA je spolupráce B2B v současnosti podporovaná jenom mezi klienty, kteří jsou v cloudu Azure USA a kteří podporují spolupráci B2B. Pokud uživatele v tenantovi, který není součástí cloudu pro státní správu Azure USA nebo který ještě nepodporuje spolupráci B2B, se zobrazí chyba. Podrobnosti a omezení najdete v tématu [variace Azure Active Directory Premium P1 a P2](https://docs.microsoft.com/azure/azure-government/documentation-government-services-securityandidentity#azure-active-directory-premium-p1-and-p2).

## <a name="next-steps"></a>Další kroky

[Získejte podporu pro spolupráci B2B.](get-support.md)
