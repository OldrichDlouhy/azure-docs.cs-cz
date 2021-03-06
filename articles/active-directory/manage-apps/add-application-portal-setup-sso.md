---
title: 'Rychlý Start: nastavení jednotného přihlašování (SSO) pro aplikaci ve vašem tenantovi Azure Active Directory (Azure AD)'
description: Tento rychlý Start vás provede procesem nastavení jednotného přihlašování (SSO) pro aplikaci ve vašem tenantovi Azure Active Directory (Azure AD).
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 07/01/2020
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: b19427070d982918584c13c25518cffe55497000
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223325"
---
# <a name="quickstart-set-up-single-sign-on-sso-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Rychlý Start: nastavení jednotného přihlašování (SSO) pro aplikaci ve vašem tenantovi Azure Active Directory (Azure AD)

Začněte s zjednodušeným přihlášením uživatelů nastavením jednotného přihlašování (SSO) pro aplikaci, kterou jste přidali do svého tenanta Azure Active Directory (Azure AD). Když nastavíte jednotné přihlašování, uživatelé se budou moct přihlašovat k aplikaci pomocí svých přihlašovacích údajů Azure AD. Jednotné přihlašování je součástí bezplatné edice služby Azure AD.

## <a name="prerequisites"></a>Předpoklady

K nastavení jednotného přihlašování pro aplikaci, kterou jste přidali do tenanta Azure AD, budete potřebovat:

- Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Jedna z následujících rolí: globální správce, správce cloudové aplikace, správce aplikace nebo vlastník instančního objektu.
- Aplikace, která podporuje jednotné přihlašování a která už byla předem nakonfigurovaná a přidaná do galerie Azure AD. Většina aplikací může používat službu Azure AD pro jednotné přihlašování. Aplikace v galerii Azure AD jsou předem nakonfigurované. Pokud vaše aplikace není uvedená nebo se jedná o aplikaci vytvořenou jako aplikace, můžete ji dál používat s Azure AD. Projděte si kurzy a další dokumentaci v obsahu. Tento rychlý Start se zaměřuje na aplikace, které byly předem nakonfigurované pro jednotné přihlašování a které vývojáři aplikací přidají do galerie Azure AD.
- Volitelné: dokončování [zobrazení vašich aplikací](view-applications-portal.md)
- Volitelné: dokončení [Přidání aplikace](add-application-portal.md)
- Volitelné: dokončení [Konfigurace aplikace](add-application-portal-configure.md)


>[!IMPORTANT]
>K otestování kroků v tomto rychlém startu použijte nevýrobní prostředí.


## <a name="enable-single-sign-on-for-an-app"></a>Povolení jednotného přihlašování pro aplikaci

Po dokončení přidávání aplikace do tenanta služby Azure AD se zobrazí stránka přehled. Pokud konfigurujete aplikaci, která již byla přidána, podívejte se do prvního rychlého startu. Provede vás zobrazením aplikací přidaných do tenanta. 

Nastavení jednotného přihlašování pro aplikaci:

1. Na portálu Azure AD vyberte **podnikové aplikace**. Pak vyhledejte a vyberte aplikaci, kterou chcete nastavit pro jednotné přihlašování.
1. V části **Spravovat** vyberte **jednotné přihlašování** . otevře se podokno **jednotného přihlašování** pro úpravy.

    :::image type="content" source="media/add-application-portal-setup-sso/configure-sso.png" alt-text="Snímek obrazovky zobrazující konfigurační stránku jednotného přihlašování na portálu Azure AD.":::

1. Vyberte **SAML** a otevřete stránku konfigurace jednotného přihlašování. V tomto příkladu je aplikace, kterou konfigurujeme pro jednotné přihlašování, GitHub. Po nastavení GitHubu se uživatelé můžou přihlásit k GitHubu pomocí svých přihlašovacích údajů z vašeho tenanta Azure AD.

    :::image type="content" source="media/add-application-portal-setup-sso/github-sso.png" alt-text="Snímek obrazovky se zobrazením konfigurační stránky jednotného přihlašování na GitHubu.":::

1. Proces konfigurace aplikace pro použití Azure AD pro jednotné přihlašování založené na SAML se liší v závislosti na aplikaci. K dispozici je odkaz na pokyny pro GitHub. Pokyny pro další aplikace najdete v tématu [kurzy pro integraci SaaSch aplikací s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/).
1. Postupujte podle pokynů průvodce pro nastavení jednotného přihlašování pro aplikaci. Mnoho aplikací má specifické požadavky na odběr pro funkce jednotného přihlašování. Například GitHub vyžaduje předplatné Enterprise.

    :::image type="content" source="media/add-application-portal-setup-sso/github-pricing.png" alt-text="Snímek obrazovky s možností jednotného přihlašování v podnikovém předplatném na stránce s cenami GitHubu.":::


## <a name="next-step"></a>Další krok

- [Odstranění aplikace](delete-application-portal.md)
