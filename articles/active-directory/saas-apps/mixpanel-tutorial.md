---
title: 'Kurz: Azure Active Directory integrace s Mixpanelu | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mixpanelu.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/28/2019
ms.author: jeedes
ms.openlocfilehash: 58074d02dfc437a1804784e73fa4e65086b53b9e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "73160470"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Kurz: Azure Active Directory integrace s Mixpanelu

V tomto kurzu se dozvíte, jak integrovat Mixpanelu s Azure Active Directory (Azure AD).
Integrace Mixpanelu s Azure AD poskytuje následující výhody:

* Můžete kontrolovat v Azure AD, kteří mají přístup k Mixpanelu.
* Můžete povolit, aby se vaši uživatelé automaticky přihlásili k Mixpanelu (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s Mixpanelu potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/) .
* Předplatné s povoleným mixpanelum jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Mixpanelu podporuje jednotné přihlašování iniciované v **SP**

## <a name="adding-mixpanel-from-the-gallery"></a>Přidání Mixpanelu z Galerie

Pokud chcete nakonfigurovat integraci Mixpanelu do služby Azure AD, musíte přidat Mixpanelu z Galerie do svého seznamu spravovaných aplikací SaaS.

**Pokud chcete přidat Mixpanelu z Galerie, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **mixpanelu**, vyberte **mixpanelu** z panelu výsledků a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![Mixpanelu v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí Mixpanelu na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, musí se zřídit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Mixpanelu.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Mixpanelu, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Nakonfigurujte jednotné přihlašování mixpanelu](#configure-mixpanel-single-sign-on)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvoření mixpanelu Test User](#create-mixpanel-test-user)** – pro Britta Simon v mixpanelu, který je propojený s reprezentací uživatele Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí Mixpanelu, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **mixpanelu** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V části **základní konfigurace SAML** proveďte následující kroky:

    ![Informace o jednotném přihlašování v doméně mixpanelu a adresách URL](common/sp-signonurl.png)

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:`https://mixpanel.com/login/`

    > [!NOTE]
    > Zaregistrujte [https://mixpanel.com/register/](https://mixpanel.com/register/) se prosím do Nastavení přihlašovacích údajů a obraťte se na [tým podpory mixpanelu](mailto:support@mixpanel.com) , který pro vašeho tenanta umožní nastavení jednotného přihlašování. V případě potřeby můžete v případě potřeby z týmu podpory Mixpanelu získat také hodnotu adresy URL pro přihlášení. 

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **certifikát (Base64)** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/certificatebase64.png)

6. V části **Nastavení mixpanelu** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-mixpanel-single-sign-on"></a>Konfigurace jednotného přihlašování Mixpanelu

1. V jiném okně prohlížeče se přihlaste k aplikaci Mixpanelu jako správce.

2. V dolní části stránky klikněte na ikonu malého **ozubeného kolečka** v levém rohu. 
   
    ![Jednotné přihlašování mixpanelu](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

3. Klikněte na kartu **zabezpečení přístupu** a pak klikněte na **změnit nastavení**.
   
    ![Nastavení mixpanelu](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

4. Na stránce **změnit certifikát** klikněte na **zvolit soubor** a nahrajte stažený certifikát a potom klikněte na **Další**.
   
    ![Nastavení mixpanelu](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

5.  Do textového pole Adresa URL pro ověřování na stránce **změnit adresu URL pro ověření** vložte hodnotu **přihlašovací adresy URL** , kterou jste zkopírovali z Azure Portal, a potom klikněte na tlačítko **Další**.
   
    ![Nastavení mixpanelu](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

6. Klikněte na **Done** (Hotovo).

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

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k Mixpanelu.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **mixpanelu**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **mixpanelu**.

    ![Odkaz Mixpanelu v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-mixpanel-test-user"></a>Vytvořit testovacího uživatele Mixpanelu

Cílem této části je vytvořit uživatele s názvem Britta Simon v Mixpanelu. 

1. Přihlaste se k webu Mixpanelu společnosti jako správce.

2. V dolní části stránky klikněte na tlačítko s malým ozubeným kolečkem v levém horním rohu a otevřete okno **Nastavení** .

3. Klikněte na kartu **tým** .

4. Do textového pole **člena týmu** zadejte e-mailovou adresu Britta v Azure.
   
    ![Nastavení mixpanelu](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. Klikněte na **pozvat**. 

> [!Note]
> Uživatel dostane e-mail, který nastaví profil.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Mixpanelu, měli byste se automaticky přihlásit k Mixpanelu, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

