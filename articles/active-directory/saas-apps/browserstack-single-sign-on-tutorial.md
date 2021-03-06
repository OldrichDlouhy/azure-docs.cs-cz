---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování s jednotným přihlašováním Browserstackem | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Browserstackem jednotného přihlašování.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 122f2095-07cb-4e8e-9f87-d059271380c8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/18/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 545fead0d80b204225c79ee7f365660a55cfbfef
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2020
ms.locfileid: "85486982"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-browserstack-single-sign-on"></a>Kurz: Azure Active Directory integraci jednotného přihlašování s jednotným přihlašováním Browserstackem

V tomto kurzu se dozvíte, jak integrovat jednotné přihlašování Browserstackem pomocí Azure Active Directory (Azure AD). Když integrujete jednotné přihlašování Browserstackem s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k jednotnému přihlašování Browserstackem
* Umožněte, aby se vaši uživatelé automaticky přihlásili k Browserstackem jednotnému přihlašování pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Browserstackem odběr jednotného přihlašování s povoleným jednotným přihlašováním (SSO).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Browserstackem jednotné přihlašování podporuje **aktualizace SP a IDP, které** iniciovaly jednotné přihlašování.

* Po nakonfigurování jednotného přihlašování Browserstackem můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Řízení relace se rozšiřuje z podmíněného přístupu. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-browserstack-single-sign-on-from-the-gallery"></a>Přidání jednotného přihlašování Browserstackem z Galerie

Pokud chcete nakonfigurovat integraci jednotného přihlašování Browserstackem do služby Azure AD, musíte do seznamu spravovaných aplikací pro SaaS přidat Browserstackem jednotné přihlašování z galerie.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** zadejte do vyhledávacího pole **Browserstackem jednotné přihlašování** .
1. Na panelu výsledků vyberte **Browserstackem jednotné přihlašování** a pak aplikaci přidejte. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.


## <a name="configure-and-test-azure-ad-single-sign-on-for-browserstack-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD pro jednotné přihlašování Browserstackem

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí jednotného přihlašování Browserstackem pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte v rámci jednotného přihlašování Browserstackem vytvořit vztah propojení mezi uživatelem Azure AD a souvisejícím uživatelem.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí jednotného přihlašování Browserstackem, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. Nakonfigurujte jednotné přihlašování **[browserstackem jednotného přihlašování](#configure-browserstack-single-sign-on-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte testovacího uživatele s jednotným přihlašováním browserstackem](#create-browserstack-single-sign-on-test-user)** , abyste měli protějšek B. Simon v browserstackem jednotném přihlašování, která je propojená s reprezentací uživatele v Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikace **jednotného přihlašování Browserstackem** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V **základní části Konfigurace SAML** nemusí uživatel provádět žádný krok, protože aplikace už je předem integrovaná s Azure.

1. Pokud chcete nakonfigurovat aplikaci v režimu iniciované **IDP** , zadejte v **základní části Konfigurace SAML** hodnoty následujících polí:

    a. Do textového pole **identifikátor (ID entity)** zadejte adresu URL pomocí následujícího vzoru:`https://login.browserstack.com/auth/realms/<REALM_ID>`

    b. Do textového pole **Adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzoru:`https://login.browserstack.com/auth/realms/<REALM_ID>/broker/<BROKER_ID>/endpoint`

1. Klikněte na **nastavit další adresy URL** a proveďte následující krok, pokud chcete nakonfigurovat aplikaci v režimu iniciované **SP** :

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL:`https://browserstack.com/users/sign_in`

    > [!Note]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty skutečným identifikátorem a adresou URL odpovědi. Pokud chcete získat tyto hodnoty, obraťte se na [tým podpory browserstackem pro jednotné přihlašování](mailto:support@browserstack.com) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

1. Klikněte na **Uložit**.

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** Najděte **XML metadata federace** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

1. V části **nastavení jednotného přihlašování browserstackem** zkopírujte příslušné adresy URL na základě vašeho požadavku.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

V této části vytvoříte testovacího uživatele ve Azure Portal s názvem B. Simon.

1. V levém podokně Azure Portal vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.
1. V horní části obrazovky vyberte **Nový uživatel** .
1. Ve vlastnostech **uživatele** proveďte následující kroky:
   1. Do pole **Název** zadejte `B.Simon`.  
   1. Do pole **uživatelské jméno** zadejte username@companydomain.extension . Například, `B.Simon@contoso.com`.
   1. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli **heslo** .
   1. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup k jednotnému přihlašování Browserstackem.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **Browserstackem jednotné přihlašování**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-browserstack-single-sign-on-sso"></a>Konfigurace jednotného přihlašování Browserstackem jednotného přihlašování

Ke konfiguraci jednotného přihlašování na straně **jednotného přihlašování browserstackem** je potřeba odeslat stažený **soubor XML federačních metadat** a příslušné zkopírované adresy URL z Azure Portal do [týmu podpory browserstackem jednotného přihlašování](mailto:support@browserstack.com). Toto nastavení nastaví, aby bylo správně nastaveno připojení SAML SSO na obou stranách.

### <a name="create-browserstack-single-sign-on-test-user"></a>Vytvořit testovacího uživatele jednotného přihlašování Browserstackem

V této části vytvoříte uživatele s názvem B. Simon v rámci Browserstackem jednotného přihlašování. Pokud chcete přidat uživatele na platformě jednotného přihlašování Browserstackem, pracujte s [browserstackem týmem podpory jednotného přihlašování](mailto:support@browserstack.com) . Před použitím jednotného přihlašování je nutné vytvořit a aktivovat uživatele.

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici jednotného přihlašování Browserstackem, měli byste se automaticky přihlásit k Browserstackem jednotnému přihlašování, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si Browserstackem jednotné přihlašování s Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chránit jednotné přihlašování Browserstackem pomocí pokročilých viditelností a ovládacích prvků](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)