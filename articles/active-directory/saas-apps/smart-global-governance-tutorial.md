---
title: 'Kurz: integrace služby Azure AD SSO s inteligentním globálním zásadou správy'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Smart Global zásad správného řízení.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c31613e-f30d-47d9-af51-001345b6db10
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/04/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91200ba4561aa6b715149d91beee8ac9d0375657
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170458"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-smart-global-governance"></a>Kurz: Azure Active Directory integraci jednotného přihlašování pomocí inteligentního globálního řízení

V tomto kurzu se dozvíte, jak integrovat inteligentní řízení globálních zásad správného řízení s Azure Active Directory (Azure AD). Při integraci inteligentního řízení globálních zásad v Azure AD můžete:

* Pomocí Azure AD můžete řídit, kdo má přístup k inteligentnímu řízení globálního řízení.
* Umožněte uživatelům, aby se automaticky přihlásili k inteligentnímu řízení globálního řízení pomocí svých účtů Azure AD.
* Spravujte své účty v jednom centrálním umístění: Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [jednotné přihlašování k aplikacím v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Předplatné inteligentního globálního řízení s povoleným jednotným přihlašováním (SSO).

## <a name="tutorial-description"></a>Popis kurzu

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

Chytré globální zásady správného řízení podporují jednotné přihlašování iniciované v SP a IDP.

Po konfiguraci inteligentního řízení globálních zásad správného řízení můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Ovládací prvky relace přesahují podmíněný přístup. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-smart-global-governance-from-the-gallery"></a>Přidání inteligentního řízení globálního řízení z Galerie

Pokud chcete nakonfigurovat integraci inteligentního globálního řízení do služby Azure AD, musíte do seznamu spravovaných aplikací SaaS přidat inteligentní řízení globálního řízení z galerie.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo pomocí osobního účet Microsoft.
1. V levém podokně vyberte **Azure Active Directory**.
1. Přejít na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** zadejte do vyhledávacího pole **inteligentní řízení globálního řízení** .
1. Na panelu výsledků vyberte **inteligentní zásady správného řízení** a pak přidejte aplikaci. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-sso-for-smart-global-governance"></a>Konfigurace a testování jednotného přihlašování služby Azure AD pro Smart Global zásad správného řízení

Nakonfigurujete a otestujete jednotné přihlašování Azure AD s využitím inteligentního globálního řízení pomocí testu uživatele s názvem B. Simon. Aby jednotné přihlašování fungovalo, je potřeba vytvořit vztah propojení mezi uživatelem služby Azure AD a odpovídajícím uživatelem v inteligentním globálním řízení.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí inteligentního globálního řízení, provedete tyto kroky vysoké úrovně:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** , aby vaši uživatelé mohli používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** pro testování jednotného přihlašování Azure AD.
    1. **[Udělte k testovacímu uživateli přístup](#grant-access-to-the-test-user)** , aby mohl uživatel používat jednotné přihlašování Azure AD.
1. **[NAKONFIGURUJTE jednotné přihlašování inteligentního řízení](#configure-smart-global-governance-sso)** na straně aplikace.
    1. **[Vytvořte uživatele inteligentního globálního zásad správného řízení](#create-a-smart-global-governance-test-user)** jako protějšek pro reprezentaci uživatele v Azure AD.
1. **[Otestujte jednotné přihlašování](#test-sso)** a ověřte, jestli konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace **inteligentního globálního řízení** aplikace v části **Spravovat** vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** vyberte tlačítko tužky pro **základní konfiguraci SAML** a upravte nastavení:

   ![Tlačítko tužky pro základní konfiguraci SAML](common/edit-urls.png)

1. Pokud chcete nakonfigurovat aplikaci v režimu iniciované v IDP, proveďte následující kroky v části **základní konfigurace SAML** .

    a. Do pole **identifikátor** zadejte jednu z těchto adres URL:

    - `https://eu-fr-south.console.smartglobalprivacy.com/platform/authentication-saml2/metadata`
    - `https://eu-fr-south.console.smartglobalprivacy.com/dpo/authentication-saml2/metadata`

    b. Do pole **Adresa URL odpovědi** zadejte jednu z těchto adres URL:

    - `https://eu-fr-south.console.smartglobalprivacy.com/platform/authentication-saml2/acs`
    - `https://eu-fr-south.console.smartglobalprivacy.com/dpo/authentication-saml2/acs`

1. Chcete-li nakonfigurovat aplikaci v režimu iniciované SP, vyberte možnost **nastavit další adresy URL** a proveďte následující krok.

   - Do pole **Adresa URL pro přihlášení** zadejte jednu z těchto adres URL:

    - `https://eu-fr-south.console.smartglobalprivacy.com/dpo`
    - `https://eu-fr-south.console.smartglobalprivacy.com/platform`

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** vyberte odkaz **ke stažení** pro **certifikát (RAW)** a Stáhněte certifikát a uložte ho do počítače:

    ![Odkaz na stažení certifikátu](common/certificateraw.png)

1. V části **Nastavení inteligentního globálního řízení \ globální zásady správného řízení** zkopírujte příslušnou adresu URL nebo adresy URL na základě vašich požadavků:

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

V této části vytvoříte testovacího uživatele s názvem B. Simon ve Azure Portal.

1. V levém podokně Azure Portal vyberte možnost **Azure Active Directory**. Vyberte **Uživatelé**a pak vyberte **Všichni uživatelé**.
1. V horní části obrazovky vyberte **Nový uživatel** .
1. V části vlastnosti **uživatele** proveďte tyto kroky:
   1. Do pole **název** zadejte **B. Simon**.  
   1. Do pole **uživatelské jméno** zadejte \<username> @ \<companydomain> . \<extension> . Například, `B.Simon@contoso.com`.
   1. Vyberte možnost **Zobrazit heslo**a pak zapište hodnotu, která se zobrazí v poli **heslo** .
   1. Vyberte **Vytvořit**.

### <a name="grant-access-to-the-test-user"></a>Udělení přístupu testovacímu uživateli

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte tomuto uživateli přístup k inteligentnímu globálnímu řízení.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **inteligentní řízení globálního řízení**.
1. Na stránce Přehled aplikace v části **Spravovat** vyberte **Uživatelé a skupiny**:

   ![Vyberte Uživatelé a skupiny.](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** :

    ![Výběr možnosti Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu **Uživatelé** možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** vyberte **přiřadit**.

## <a name="configure-smart-global-governance-sso"></a>Konfigurace jednotného přihlašování Smart Global zásad správného řízení

Ke konfiguraci jednotného přihlašování na straně inteligentního globálního řízení se musí odeslat stažený nezpracovaný certifikát a příslušné adresy URL, které jste zkopírovali z Azure Portal do [týmu podpory inteligentního řízení](mailto:support.tech@smartglobal.com). Konfigurují připojení SAML pro jednotné přihlašování na obou stranách správně.

### <a name="create-a-smart-global-governance-test-user"></a>Vytvoření uživatele s automatickým automatickým testováním pro řízení

Spolupracujte s [týmem podpory inteligentního řízení globálního řízení](mailto:support.tech@smartglobal.com) a přidejte uživatele s názvem B. Simon do inteligentního řízení globálního řízení. Před použitím jednotného přihlašování je nutné vytvořit a aktivovat uživatele.

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci služby Azure AD SSO pomocí přístupového panelu.

Když na přístupovém panelu vyberete dlaždici Inteligentní zásady správného řízení, měli byste být automaticky přihlášení do globální instance zásad správného řízení, pro kterou jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Kurzy k integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si inteligentní řízení globálních věcí pomocí Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Ochrana inteligentního řízení globálního řízení pomocí pokročilých viditelností a ovládacích prvků](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)