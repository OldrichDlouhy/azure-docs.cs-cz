---
title: Azure Active Directory osvědčené postupy a doporučení B2B
description: Naučte se osvědčené postupy a doporučení pro přístup uživatelů typu Host (B2B) k podnikovým uživatelům v Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54f5721ef606b6ea916f5a00031c58f5e2adeb0e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80050846"
---
# <a name="azure-active-directory-b2b-best-practices"></a>Doporučené postupy Azure Active Directory B2B
Tento článek obsahuje doporučení a osvědčené postupy pro spolupráci B2B (Business-to-Business) v Azure Active Directory (Azure AD).

   > [!IMPORTANT]
   > **Od 31. března 2021**přestane společnost Microsoft podporovat uplatnění pozvánky tím, že pro scénáře spolupráce B2B vytvoří nespravované účty a klienty Azure AD. V přípravě doporučujeme zákazníkům, aby se přihlásili k [e-mailu ověřování jednorázovým heslem](one-time-passcode.md). Uvítáme vaše názory na tuto funkci Public Preview a zajímáme si vytváření ještě více způsobů, jak spolupracovat.

## <a name="b2b-recommendations"></a>Doporučení B2B
| Doporučení | Komentáře |
| --- | --- |
| Optimální prostředí přihlašování federovat pomocí zprostředkovatelů identity | Kdykoli je to možné, federovat přímo s poskytovateli identity, aby pozvaní uživatelé mohli přihlašovat se ke sdíleným aplikacím a prostředkům, aniž by museli vytvářet účty Microsoft (účty spravované služby) nebo účty Azure AD. Pomocí [funkce Google Federation](google-federation.md) můžete uživatelům typu Host B2B, aby se přihlásili pomocí svých účtů Google. Nebo můžete použít [funkci přímé federace (Preview)](direct-federation.md) k nastavení přímé federace s jakoukoli organizací, jejíž zprostředkovatel identity (IDP) podporuje protokol SAML 2,0 nebo WS-dodaný protokol. |
| Pro hosty B2B, kteří se nemůžou ověřit jiným způsobem, použijte funkci jednorázového hesla (Preview) e-mailu. | Funkce [jednorázového hesla (Preview) e-mailu](one-time-passcode.md) ověřuje uživatele typu Host B2B, když se nemůžou ověřit jiným způsobem jako Azure AD, účet Microsoft (MSA) nebo Google Federation. Když uživatel typu Host uplatňuje pozvánku nebo přistupuje ke sdílenému prostředku, může požádat o dočasný kód, který se pošle na svou e-mailovou adresu. Pak tento kód zadá, aby bylo možné pokračovat v přihlašování. |
| Přidání firemního brandingu na přihlašovací stránku | Přihlašovací stránku můžete přizpůsobit tak, aby byla intuitivnější pro uživatele typu Host B2B. Podívejte se na téma Jak [Přidat Branding společnosti pro přihlášení a přístup ke stránkám na panelu](../fundamentals/customize-branding.md). |
| Přidejte prohlášení o zásadách ochrany osobních údajů do prostředí pro vyplňování uživatelů typu hosta B2B. | Adresu URL prohlášení o zásadách ochrany osobních údajů vaší organizace můžete přidat do prvního procesu uplatnění pozvánky na pozvání, aby pozvaní uživatelé museli před pokračováním souhlasit s vašimi podmínkami ochrany osobních údajů. Viz [Postup: Přidání informací o ochraně osobních údajů vaší organizace v Azure Active Directory](https://aka.ms/adprivacystatement). |
| Použití funkce Hromadná Pozvánka (Preview) k pozvání více uživatelů typu Host pro B2B ve stejnou dobu | Pomocí funkce hromadné pozvánky ve verzi Preview v Azure Portal můžete do vaší organizace pozvat více uživatelů typu Host. Tato funkce umožňuje odeslat soubor CSV pro vytvoření uživatelů typu Host B2B a hromadně odesílat pozvánky. Přečtěte si [kurz pro hromadné pozvání uživatelů B2B](tutorial-bulk-invite.md). |
| Vyhovět zásadám podmíněného přístupu pro Multi-Factor Authentication (MFA) | Doporučujeme vynucovat zásady MFA u aplikací, které chcete sdílet s uživateli B2B pro partnery. Vícefaktorové ověřování se v aplikacích ve vašem tenantovi bude konzistentně uplatňovat bez ohledu na to, jestli partnerská organizace používá MFA. Podívejte se [na podmíněný přístup pro uživatele spolupráce B2B](conditional-access.md). |
| Pokud vynucujete zásady podmíněného přístupu na základě zařízení, pomocí seznamů vyloučení povolte přístup uživatelům B2B. | Pokud jsou ve vaší organizaci povolené zásady podmíněného přístupu na základě zařízení, zablokují se zařízení uživatelů hosta B2B, protože nejsou spravovaná vaší organizací. Můžete vytvořit seznam vyloučení, který obsahuje konkrétní uživatele, aby je vyloučil ze zásad podmíněného přístupu na základě zařízení. Podívejte se [na podmíněný přístup pro uživatele spolupráce B2B](conditional-access.md). |
| Při poskytování přímých odkazů na uživatele typu Host B2B použijte adresu URL specifickou pro tenanta. | Jako alternativu k e-mailu s pozvánkou můžete hostům poskytnout přímý odkaz na vaši aplikaci nebo portál. Tento přímý odkaz musí být specifický pro tenanta, což znamená, že musí zahrnovat ID tenanta nebo ověřenou doménu, aby se host mohla ověřit ve vašem tenantovi, kde se nachází sdílená aplikace. Podívejte [se na možnosti pro vyplacení uživatele typu Host](redemption-experience.md). |
| Při vývoji aplikace použijte k určení uživatelského prostředí typu Host možnost UserType.  | Pokud vyvíjíte aplikaci a chcete pro uživatele klientů a uživatele typu Host poskytovat různá prostředí, použijte vlastnost UserType. Deklarace UserType není aktuálně zahrnutá v tokenu. Aplikace by měly používat rozhraní Microsoft Graph API k dotazování adresáře, aby uživatel získal své UserType. |
| Změnit vlastnost UserType *pouze* v případě, že se změní vztah uživatele k organizaci | I když je možné použít PowerShell k převedení vlastnosti UserType pro uživatele z člena na hosta (a naopak), měli byste změnit tuto vlastnost pouze v případě, že se změní vztah uživatele k vaší organizaci. Podívejte [se na vlastnosti uživatele typu Host B2B](user-properties.md).|

## <a name="next-steps"></a>Další kroky

[Správa sdílení B2B](delegate-invitations.md)
