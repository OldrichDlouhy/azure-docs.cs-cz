---
title: 'Kurz: Azure Active Directory integrace se službou SAML SSO pro Bitbucket podle rezoluce GmbH | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAML SSO pro Bitbucket pomocí rezoluce GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57ae83aee563d4893f767331fff2289a4595cc60
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "73157640"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Kurz: Azure Active Directory integrace se službou SAML SSO pro Bitbucket podle rezoluce GmbH

V tomto kurzu se naučíte integrovat jednotné přihlašování SAML pro Bitbucket pomocí rezoluce GmbH s Azure Active Directory (Azure AD).
Integrace jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH s Azure AD poskytuje následující výhody:

* Můžete řídit v Azure AD, který má přístup k SAML SSO pro Bitbucket podle rezoluce GmbH.
* Můžete povolit, aby se vaši uživatelé automaticky přihlásili k SAML SSO pro Bitbucket pomocí rezoluce GmbH (jednotné přihlašování) se svými účty Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

K nakonfigurování integrace služby Azure AD pomocí jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH budete potřebovat následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/) .
* JEDNOTNÉ přihlašování SAML pro Bitbucket podle řešení GmbH s povoleným jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH podporuje **aktualizace SP a IDP,** které iniciovaly jednotné přihlašování
* Jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH podporuje **jenom při** zřizování uživatelů.


## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Přidání jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH z Galerie

Pokud chcete nakonfigurovat integraci SAML SSO pro Bitbucket podle rezoluce GmbH na Azure AD, musíte do svého seznamu spravovaných aplikací pro SaaS přidat jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH z galerie.

**K přidání jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH z Galerie proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **SAML SSO pro Bitbucket podle rezoluce GmbH**, vyberte položku **SAML SSO pro Bitbucket podle rezoluce GmbH** z panelu výsledek a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![Jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování služby Azure AD pomocí jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, je potřeba zřídit odkaz na propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v rámci SAML SSO pro Bitbucket podle rezoluce GmbH.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí jednotného přihlašování SAML pro Bitbucket pomocí nástroje Solution GmbH, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. Nakonfigurujte jednotné přihlašování **[SAML pro Bitbucket podle rezoluce GmbH jednotného přihlašování](#configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on)** – pro konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvořte jednotné přihlašování SAML pro Bitbucket podle řešení GmbH Test User](#create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)** – Pokud chcete mít protějšek Britta Simon v jednotném přihlašování SAML pro Bitbucket podle rezoluce GmbH, která je propojená s reprezentací uživatele v Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí jednotného přihlašování SAML pro Bitbucket podle rezoluce GmbH, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce pro integraci aplikace **SAML pro Bitbucket podle rezoluce GmbH** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V části **základní konfigurace SAML** proveďte následující kroky, pokud chcete nakonfigurovat aplikaci v režimu iniciované **IDP** :

    ![Jednotné přihlašování SAML pro Bitbucket podle rozlišení domény a adres URL v protokolu GmbH](common/idp-intiated.png)

    a. Do textového pole **identifikátor** zadejte adresu URL pomocí následujícího vzoru:`https://<server-base-url>/plugins/servlet/samlsso`

    b. Do textového pole **Adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzoru:`https://<server-base-url>/plugins/servlet/samlsso`

    c. Klikněte na **nastavit další adresy URL** a proveďte následující krok, pokud chcete nakonfigurovat aplikaci v režimu iniciované **SP** :

    ![Jednotné přihlašování SAML pro Bitbucket podle rozlišení domény a adres URL v protokolu GmbH](common/metadata-upload-additional-signon.png)

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty skutečným identifikátorem, adresou URL odpovědi a přihlašovací adresou URL. Chcete-li získat tyto hodnoty, obraťte se na [podporu SAML SSO pro Bitbucket podle týmu podpory pro klienty](https://marketplace.atlassian.com/apps/1217045/saml-single-sign-on-sso-bitbucket?hosting=server&tab=support) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

### <a name="configure-saml-sso-for-bitbucket-by-resolution-gmbh-single-sign-on"></a>Konfigurace jednotného přihlašování SAML pro Bitbucket podle rozlišení GmbH

1. Přihlaste se ke svému jednotnému přihlašování SAML pro Bitbucket pomocí webu usnesení GmbH jako správce.

2. Na pravé straně hlavního panelu nástrojů klikněte na **Nastavení**.

3. Přejděte na část účty a na řádku nabídek klikněte na **SingleSignon SAML** .

    ![Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

4. Na **stránce konfigurace modulu plug-in SAML SIngleSignOn**klikněte na **Přidat IDP**. 

    ![Přidat IDP](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

5. Na stránce **Vyberte poskytovatele identity SAML** proveďte následující kroky:

    ![Zprostředkovatel identity](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Jako **Azure AD**vyberte **typ IDP** .

    b. Do textového pole **název** zadejte název.

    c. Do textového pole **Popis** zadejte popis.

    d. Klikněte na **Další**.

6. Na **stránce Konfigurace zprostředkovatele identity**klikněte na **Další**.

    ![Konfigurace identity](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

7.  Na stránce **importovat metadata IDP SAML** klikněte na **načíst soubor** a nahrajte soubor **XML s metadaty** , který jste stáhli z Azure Portal.

    ![Idpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
8. Klikněte na **Další**.

9. Klikněte na **Uložit nastavení**.

    ![Uložit](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD 

Cílem této části je vytvořit testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky vyberte **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. Ve vlastnostech uživatele proveďte následující kroky.

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole **uživatelské jméno** zadejte **brittasimon\@yourcompanydomain. extension.**  
    Například BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k JEDNOTNÉmu přihlašování SAML pro Bitbucket podle rezoluce GmbH.

1. V Azure Portal vyberte možnost **podnikové aplikace**, vyberte možnost **všechny aplikace**a pak vyberte položku **SAML SSO pro Bitbucket podle rezoluce GmbH**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace zadejte a vyberte **jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH**.

    ![Odkaz na rozhraní SAML SSO pro Bitbucket podle rezoluce GmbH v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Vytvoření jednotného přihlašování SAML pro Bitbucket podle řešení GmbH Test User

Cílem této části je vytvořit uživatele s názvem Britta Simon v jednotném přihlašování SAML pro Bitbucket podle rezoluce GmbH. Jednotné přihlašování SAML pro Bitbucket podle rezoluce GmbH podporuje zřizování za běhu a také uživatele je možné vytvořit ručně, obraťte se na [tým podpory SAML pro Bitbucket podle](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) vašeho požadavku.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici SSO SSO pro Bitbucket podle rezoluce GmbH, měli byste se automaticky přihlásit k rozhraní SAML SSO pro BitBucket, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

