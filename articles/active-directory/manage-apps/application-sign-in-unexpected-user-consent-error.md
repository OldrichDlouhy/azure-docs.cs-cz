---
title: Neočekávaná chyba při provádění souhlasu s aplikací | Microsoft Docs
description: Popisuje chyby, ke kterým může dojít během procesu souhlasu s aplikací a o tom, co s nimi můžete dělat.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2a7709cf0522727257025b2dddc495b20fe8448
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763750"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Neočekávaná chyba při provádění souhlasu s aplikací

Tento článek popisuje chyby, ke kterým může dojít během procesu souhlasu s aplikací. Pokud řešíte problémy s neočekávaným souhlasem, které neobsahují žádné chybové zprávy, přečtěte si téma [scénáře ověřování pro Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Mnoho aplikací, které jsou integrovány s Azure Active Directory, vyžaduje oprávnění k přístupu k jiným prostředkům, aby fungovaly. Pokud jsou tyto prostředky integrovány i Azure Active Directory, jsou oprávnění pro přístup k nim často požadována pomocí společného souhlasu architektury. Zobrazí se výzva k vyjádření souhlasu, která obvykle nastane při prvním použití aplikace, ale může se objevit i při dalším použití aplikace.

Aby mohl uživatel udělit souhlas s oprávněními, které aplikace vyžaduje, musí být splněny určité podmínky. Nejsou-li tyto podmínky splněny, může dojít k následujícím chybám.

## <a name="requesting-not-authorized-permissions-error"></a>Požaduje se chyba neautorizované oprávnění.
* **AADSTS90093:** &lt; clientAppDisplayName &gt; požaduje jedno nebo více oprávnění, která nemáte autorizaci udělit. Obraťte se na správce, který může tuto aplikaci vyjádřit vaším jménem.
* **AADSTS90094:** &lt; clientAppDisplayName &gt; potřebuje oprávnění pro přístup k prostředkům ve vaší organizaci, které může udělit jenom správce. Please ask an admin to grant permission to this app before you can use it. (Test udělení souhlasu vyžaduje ve vaší organizaci pro přístup k prostředkům oprávnění, které může udělit pouze správce. Než budete moct tuto aplikaci použít, požádejte správce o udělení oprávnění.)

K této chybě dochází, když se uživatel, který není správcem společnosti, pokusí použít aplikaci požadující oprávnění, která může udělit jenom správce. Tuto chybu může vyřešit správce, který uděluje přístup k aplikaci jménem své organizace.

K této chybě může dojít také v případě, že uživatel brání v souhlasu s aplikací kvůli tomu, že je žádost o oprávnění riskantní. V takovém případě se událost auditu bude protokolovat jako kategorie "ApplicationManagement", typ aktivity "souhlas s aplikací" a důvod stavu "riziková aplikace zjištěná".

## <a name="policy-prevents-granting-permissions-error"></a>Zásada zabraňuje udělení oprávnění k chybě.
* **AADSTS90093:** Správce &lt; tenantDisplayName &gt; nastavil zásadu, která vám zabrání v udělení &lt; názvu aplikace oprávnění, která &gt; požaduje. Kontaktujte správce &lt; tenantDisplayName &gt; , který vám může udělit oprávnění k této aplikaci vaším jménem.

K této chybě dojde, když správce společnosti vypne možnost souhlasu uživatelů s aplikacemi, pak se uživatel bez oprávnění správce pokusí použít aplikaci, která vyžaduje souhlas. Tuto chybu může vyřešit správce, který uděluje přístup k aplikaci jménem své organizace.

## <a name="intermittent-problem-error"></a>Chyba přerušovaného problému
* **AADSTS90090:** Vypadá to, že při procesu přihlašování došlo k přerušovanému problému se záznamem oprávnění, která jste se pokusili udělit &lt; clientAppDisplayName &gt; . Zkuste to znovu později.

Tato chyba znamená, že došlo k občasnému problému na straně služby. Dá se vyřešit opakovaným pokusem o vyjádření souhlasu s aplikací.

## <a name="resource-not-available-error"></a>Chyba prostředku není k dispozici
* **AADSTS65005:** Aplikace &lt; clientAppDisplayName &gt; požadovala oprávnění pro přístup k &lt; resourceAppDisplayName prostředku &gt; , který není k dispozici. 

Obraťte se na vývojáře aplikace.

##  <a name="resource-not-available-in-tenant-error"></a>Prostředek není k dispozici v chybě tenanta.
* **AADSTS65005:** &lt; clientAppDisplayName &gt; žádá o přístup k resourceAppDisplayName prostředků &lt; &gt; , který není k dispozici ve vaší organizaci &lt; tenantDisplayName &gt; . 

Zajistěte, aby byl tento prostředek k dispozici, nebo se obraťte na správce služby &lt; tenantDisplayName &gt; .

## <a name="permissions-mismatch-error"></a>Chyba neshody oprávnění
* **AADSTS65005:** Aplikace požádala o souhlas s přístupem k &lt; resourceAppDisplayName prostředků &gt; . Tato žádost se nezdařila, protože neodpovídá tomu, jak byla aplikace předem nakonfigurovaná během registrace aplikace. Obraťte se na dodavatele aplikace. * *

K těmto chybám dochází, když aplikace, se kterou se uživatel snaží souhlasit, požaduje oprávnění k přístupu k aplikaci prostředků, kterou nelze najít v adresáři organizace (tenant). K této situaci může dojít z několika důvodů:

-   Vývojář klientské aplikace nakonfiguroval svou aplikaci nesprávně, což způsobuje, že žádá o přístup k neplatnému prostředku. V takovém případě musí vývojář aplikace aktualizovat konfiguraci klientské aplikace, aby tento problém vyřešil.

-   Instanční objekt představující cílovou aplikaci prostředků v organizaci neexistuje nebo existoval v minulosti, ale byl odebrán. Chcete-li tento problém vyřešit, musí být v organizaci zřízen instanční objekt pro aplikaci prostředků, aby klientská aplikace mohla pro něj požádat o oprávnění. Instanční objekt se dá zřídit řadou způsobů v závislosti na typu aplikace, včetně:

    -   Získání předplatného pro aplikaci prostředků (publikované aplikace Microsoftu)

    -   Souhlas s aplikací prostředků

    -   Udělení oprávnění aplikace prostřednictvím Azure Portal

    -   Přidání aplikace z Galerie aplikací Azure AD

## <a name="next-steps"></a>Další kroky 

[Aplikace, oprávnění a souhlas v Azure Active Directory (koncový bod V1)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Rozsahy, oprávnění a souhlas v Azure Active Directory (koncový bod verze 2.0)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


