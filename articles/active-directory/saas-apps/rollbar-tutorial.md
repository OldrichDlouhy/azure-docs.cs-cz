---
title: 'Kurz: Azure Active Directory integrace s Rollbar | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Rollbar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 57537e54-9388-4272-a610-805ce45a451f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/15/2019
ms.author: jeedes
ms.openlocfilehash: d76f4e9d61d8fd210fe9332084f9f44d19e54eed
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67092680"
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Kurz: Azure Active Directory integrace s Rollbar

V tomto kurzu se dozvíte, jak integrovat Rollbar s Azure Active Directory (Azure AD).
Integrace Rollbar s Azure AD poskytuje následující výhody:

* Můžete kontrolovat v Azure AD, kteří mají přístup k Rollbar.
* Můžete povolit, aby se vaši uživatelé automaticky přihlásili k Rollbar (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s Rollbar potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat [bezplatný účet](https://azure.microsoft.com/free/) .
* Předplatné s povoleným Rollbarm jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Rollbar podporuje jednotné přihlašování (SSO) **a IDP** .

## <a name="adding-rollbar-from-the-gallery"></a>Přidání Rollbar z Galerie

Pokud chcete nakonfigurovat integraci Rollbar do služby Azure AD, musíte přidat Rollbar z Galerie do svého seznamu spravovaných aplikací SaaS.

**Pokud chcete přidat Rollbar z Galerie, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **Rollbar**, vyberte **Rollbar** z panelu výsledků a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![Rollbar v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí Rollbar na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, musí se zřídit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Rollbar.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Rollbar, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Nakonfigurujte jednotné přihlašování Rollbar](#configure-rollbar-single-sign-on)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvoření Rollbar Test User](#create-rollbar-test-user)** – pro Britta Simon v Rollbar, který je propojený s reprezentací uživatele Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí Rollbar, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **Rollbar** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. Pokud chcete nakonfigurovat aplikaci v režimu iniciované **IDP** , proveďte v **základní části Konfigurace SAML** následující kroky:

    ![Informace o jednotném přihlašování v doméně Rollbar a adresách URL](common/idp-intiated.png)

    a. Do textového pole **identifikátor** zadejte adresu URL:`https://saml.rollbar.com`

    b. Do textového pole **Adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzoru:`https://rollbar.com/<accountname>/saml/sso/azure/`

5. Klikněte na **nastavit další adresy URL** a proveďte následující krok, pokud chcete nakonfigurovat aplikaci v režimu iniciované **SP** :

    ![Informace o jednotném přihlašování v doméně Rollbar a adresách URL](common/metadata-upload-additional-signon.png)

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:`https://rollbar.com/<accountname>/saml/login/azure/`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty pomocí skutečné adresy URL odpovědi a přihlašovací adresy URL. Pokud chcete získat tyto hodnoty, obraťte se na [tým podpory klienta Rollbar](mailto:support@rollbar.com) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

6. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

7. V části **Nastavení Rollbar** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-rollbar-single-sign-on"></a>Konfigurace jednotného přihlašování Rollbar

1. V jiném okně webového prohlížeče se přihlaste k webu Rollbar společnosti jako správce.

1. Klikněte na **nastavení profilu** v pravém horním rohu a pak klikněte na **Nastavení názvu účtu**.

    ![Konfigurace](./media/rollbar-tutorial/general.png)

1. V části zabezpečení klikněte na **zprostředkovatel identity** .

    ![Konfigurace](./media/rollbar-tutorial/configure1.png)

1. V části **zprostředkovatel identity SAML** proveďte následující kroky:

    ![Konfigurace](./media/rollbar-tutorial/configure2.png)

    a. V rozevíracím seznamu **zprostředkovatel identity SAML** vyberte **Azure** .

    b. Otevřete soubor metadat v programu Poznámkový blok, zkopírujte jeho obsah do schránky a vložte ho do textového pole **metadat SAML** .

    c. Klikněte na **Uložit**.

1. Po kliknutí na tlačítko Uložit bude obrazovka vypadat takto:

    ![Konfigurace](./media/rollbar-tutorial/configure3.png)

    > [!NOTE]
    > Aby bylo možné provést následující krok, je třeba nejprve přidat sami sebe jako uživatele do aplikace Rollbar v Azure.
    >

    a. Pokud chcete, aby všichni uživatelé ověřili přes Azure, klikněte na **Přihlásit se přes poskytovatele identity** a znovu proveďte ověření prostřednictvím Azure.  

    b.  Po návratu na obrazovku zaškrtněte políčko **požadovat přihlášení prostřednictvím poskytovatele identity SAML** .

    b. Klikněte na **Uložit**.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

Cílem této části je vytvořit testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky vyberte **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. Ve vlastnostech uživatele proveďte následující kroky.

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole typ **uživatelského jména**`brittasimon@yourcompanydomain.extension`  
    Například BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k Rollbar.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **Rollbar**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **Rollbar**.

    ![Odkaz Rollbar v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-rollbar-test-user"></a>Vytvořit testovacího uživatele Rollbar

Aby se uživatelé Azure AD mohli přihlašovat k Rollbar, musí se zřídit v Rollbar. V případě Rollbar je zřizování ručním úkolem.

**Chcete-li zřídit uživatelský účet, proveďte následující kroky:**

1. Přihlaste se k webu Rollbar společnosti jako správce.

1. Klikněte na **nastavení profilu** v pravém horním rohu a pak klikněte na **Nastavení názvu účtu**.

    ![Uživatel](./media/rollbar-tutorial/general.png)

1. Klikněte na **Uživatelé**.

    ![Přidat zaměstnance](./media/rollbar-tutorial/user1.png)

1. Klikněte na **pozvat členy týmu**.

    ![Pozvat lidi](./media/rollbar-tutorial/user2.png)

1. Do textového pole zadejte jméno uživatele jako **brittasimon\@contoso.com** a klikněte na **Přidat nebo pozvat**.

    ![Pozvat lidi](./media/rollbar-tutorial/user3.png)

1. Uživatel obdrží pozvánku a po přijetí ji vytvoří v systému.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Rollbar, měli byste se automaticky přihlásit k Rollbar, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

