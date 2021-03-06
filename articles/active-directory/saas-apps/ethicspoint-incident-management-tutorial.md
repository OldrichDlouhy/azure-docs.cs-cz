---
title: 'Kurz: Azure Active Directory integrace se správou incidentů EthicsPoint (EPIM) | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a EthicsPoint (EPIM).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01ad3a1fb23aac9badefcef7414521e014476eef
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/01/2020
ms.locfileid: "85799560"
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Kurz: Azure Active Directory integrace se správou incidentů EthicsPoint (EPIM)

V tomto kurzu se dozvíte, jak integrovat správu EthicsPoint incidentů (EPIM) s Azure Active Directory (Azure AD).
Integrace správy incidentů EthicsPoint (EPIM) se službou Azure AD poskytuje následující výhody:

* Můžete řídit v Azure AD, kteří mají přístup ke správě incidentů EthicsPoint (EPIM).
* Uživatelům můžete povolit, aby se automaticky přihlásili ke správě EthicsPoint incidentů (EPIM) (jednotné přihlašování) se svými účty Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD se správou incidentů EthicsPoint (EPIM) potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/) .
* Předplatné s povoleným jednotným přihlašováním pro EthicsPoint Incidenting (EPIM)

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* EthicsPoint Incidenting (EPIM) podporuje jednotné přihlašování (SSO) iniciované v **SP**

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>Přidání správy incidentů EthicsPoint (EPIM) z Galerie

Pokud chcete nakonfigurovat integraci EthicsPoint správy incidentů (EPIM) do služby Azure AD, musíte do seznamu spravovaných aplikací SaaS přidat z Galerie EthicsPoint správu incidentů (EPIM).

**Pokud chcete do galerie přidat správu incidentů EthicsPoint (EPIM), proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **EthicsPoint Incident Management (EPIM)**, na panelu výsledků vyberte **ETHICSPOINT Management (EPIM)** a pak kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![EthicsPoint (EPIM) – Správa incidentů () v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD se správou EthicsPoint incidentů (EPIM) na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, musí být navázán odkaz na odkaz mezi uživatelem služby Azure AD a souvisejícím uživatelem ve správě incidentů EthicsPoint (EPIM).

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí EthicsPoint (EPIM) pro správu incidentů, je potřeba, abyste dokončili tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Konfigurace jednotného přihlašování EPIM (EthicsPoint Incident Management)](#configure-ethicspoint-incident-management-epim-single-sign-on)** – pro konfiguraci nastavení jednotného přihlašování na straně aplikace
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvořte testovacího uživatele EPIM (EthicsPoint Incident Management)](#create-ethicspoint-incident-management-epim-test-user)** – abyste měli protějšek Britta Simon ve EthicsPoint Incident Management (EPIM), která je propojená s reprezentací uživatele v Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí správy incidentů EthicsPoint (EPIM), proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikace **pro správu incidentů EthicsPoint (EPIM)** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V části **základní konfigurace SAML** proveďte následující kroky:

    ![Informace o jednotném přihlašování EthicsPoint Incident Management (EPIM) a adresy URL](common/sp-identifier-reply.png)

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:
    
    ```http
    https://<companyname>.navexglobal.com
    https://<companyname>.ethicspointvp.com
    ```

    b. Do pole **identifikátor** zadejte adresu URL pomocí následujícího vzoru:`https://<companyname>.navexglobal.com/adfs/services/trust`

    c. Do textového pole **Adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzoru:`https://<servername>.navexglobal.com/adfs/ls/`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty pomocí skutečné přihlašovací adresy URL, identifikátoru a adresy URL odpovědi. Chcete-li získat tyto hodnoty, obraťte se na [tým podpory správy incidentů EthicsPoint (EPIM)](https://www.navexglobal.com/company/contact-us) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

6. V části **Nastavení EPIM (EthicsPoint Incident Management)** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-ethicspoint-incident-management-epim-single-sign-on"></a>Konfigurace jednotného přihlašování pro správu incidentů EthicsPoint (EPIM)

Chcete-li nakonfigurovat jednotné přihlašování na straně **EthicsPoint (EPIM)** , je třeba odeslat stažené **federační metadata XML** a příslušné zkopírované adresy URL z Azure Portal do [týmu podpory správy incidentů EthicsPoint (EPIM)](https://www.navexglobal.com/company/contact-us). Toto nastavení nastaví, aby bylo správně nastaveno připojení SAML SSO na obou stranách.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD 

Cílem této části je vytvořit testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky vyberte **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. Ve vlastnostech uživatele proveďte následující kroky.

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole **uživatelské jméno** zadejte **brittasimon \@ yourcompanydomain. extension.**  
    Například BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup ke správě incidentů EthicsPoint (EPIM).

1. V Azure Portal vyberte možnost **podnikové aplikace**, vyberte možnost **všechny aplikace**a pak vyberte možnost **Správa incidentů EthicsPoint (EPIM)**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **EthicsPoint Incident Management (EPIM)**.

    ![Odkaz EPIM (EthicsPoint Incidenting Management) v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-ethicspoint-incident-management-epim-test-user"></a>Vytvořit testovacího uživatele pro správu incidentů EthicsPoint (EPIM)

V této části vytvoříte uživatele s názvem Britta Simon ve správě incidentů EthicsPoint (EPIM). Pokud chcete přidat uživatele na platformě EthicsPoint správy incidentů (EPIM), pracujte s [týmem podpory správy incidentů EthicsPoint (EPIM)](https://www.navexglobal.com/company/contact-us) . Před použitím jednotného přihlašování je nutné vytvořit a aktivovat uživatele.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Správa incidentů EthicsPoint (EPIM), měli byste být automaticky přihlášeni ke správě incidentů EthicsPoint (EPIM), pro kterou jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

