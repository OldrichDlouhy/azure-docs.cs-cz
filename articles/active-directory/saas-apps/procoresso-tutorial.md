---
title: 'Kurz: Integrace Azure Active Directory s Procore SSO | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Procore SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca6863a6b02e867afd732ce1662136051b8afec8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67093665"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Kurz: Azure Active Directory integrace s jednotným přihlašováním

V tomto kurzu se dozvíte, jak integrovat základní jednotné přihlašování pomocí Azure Active Directory (Azure AD).
Integrování hlavního jednotného přihlašování s Azure AD poskytuje následující výhody:

* Můžete řídit v Azure AD, který má přístup k hlavnímu přihlášení SSO.
* Uživatelům můžete povolit, aby se automaticky přihlásili k základnímu přihlašování (jednotné přihlašování) k účtům Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s využitím Procore SSO budete potřebovat následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat [bezplatný účet](https://azure.microsoft.com/free/) .
* Předplatné Procore jednotného přihlašování s jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Základní jednotné přihlašování podporuje **IDP** .

## <a name="adding-procore-sso-from-the-gallery"></a>Přidání hlavního jednotného přihlašování z Galerie

Pokud chcete nakonfigurovat integraci Procore jednotného přihlašování do Azure AD, musíte do seznamu spravovaných aplikací SaaS přidat hlavní přihlášení SSO z galerie.

**Pokud chcete přidat hlavní přihlášení SSO z Galerie, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **jednotné přihlašování (SSO**), vyberte **jednotné přihlašování** z panelu výsledků a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

    ![Základní jednotné přihlašování v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD s jednotným přihlašováním (SSO) na základě testovacího uživatele s názvem **Britta Simon**.
Aby se jednotné přihlašování fungovalo, je potřeba zřídit odkazový vztah mezi uživatelem služby Azure AD a souvisejícím uživatelem v rámci hlavního přihlašování.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí jednotného přihlašování (SSO), musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Konfigurace jednotného přihlašování jednotného](#configure-procore-sso-single-sign-on)** přihlašování – pro konfiguraci nastavení jednotného přihlašování na straně aplikace
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvořte si uživatele se zkušebním základem jednotného přihlašování (SSO](#create-procore-sso-test-user)** ), který bude mít protějšek Britta Simon v proHlavním jednotném přihlašování, které je propojené s reprezentací uživatele v Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí jednotného přihlašování (SSO), proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce pro integraci aplikace **jednotného přihlašování** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V **základní části Konfigurace SAML** nemusí uživatel provádět žádný krok, protože aplikace už je předem integrovaná s Azure.

    ![Základní informace o jednotném přihlašování k doméně a adresám URL jednotného přihlašování](common/preintegrated.png)

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

6. V části **Nastavení Procore jednotného přihlašování** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-procore-sso-single-sign-on"></a>Konfigurace jednotného přihlašování jednotného přihlašování (SSO)

1. Pokud chcete nakonfigurovat jednotné přihlašování na straně **jednotného přihlašování (SSO** ), přihlaste se k místnímu webu společnosti jako správce.

2. V rozevíracím seznamu nástrojů klikněte na **správce** a otevřete stránku nastavení jednotného přihlašování.

    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/procore_tool_admin.png)

3. Vložte hodnoty do polí, jak je popsáno níže.

    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. Do textového pole **Adresa URL vystavitele jednotného přihlašování** vložte hodnotu **identifikátoru služby Azure AD** , který jste zkopírovali z Azure Portal.

    b. Do pole **Adresa URL pro přihlášení k poskytovateli SAML** vložte hodnotu **adresy URL pro přihlášení** , kterou jste zkopírovali z Azure Portal.

    c. Teď otevřete **soubor XML federačních metadat** stažený výše z Azure Portal a zkopírujte certifikát do značky s názvem **certifikátu x509**. Vložte zkopírovanou hodnotu do pole **certifikát x509 jednotného přihlašování** .

4. Klikněte na **Save Changes** (Uložit změny).

5. Po těchto nastaveních musíte odeslat **název domény** (třeba **contoso.com**), přes který se přihlašujete do základu [týmu podpory](https://support.procore.com/) , a aktivovat pro tuto doménu federované přihlašování.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD 

Cílem této části je vytvořit testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky vyberte **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. Ve vlastnostech uživatele proveďte následující kroky.

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole **uživatelské jméno** zadejte `brittasimon@yourcompanydomain.extension`. Například BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k hlavnímu přihlášení SSO.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **základní jednotné přihlašování**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **Procore SSO**.

    ![Základní odkaz SSO v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-procore-sso-test-user"></a>Vytvořit uživatele se zkušebním základem jednotného přihlašování

Postupujte podle následujících kroků a vytvořte na straně jednotného přihlašování základní test uživatele.

1. Přihlaste se k hlavnímu serveru společnosti jako správce.    

2. V rozevírací nabídce panelu nástrojů klikněte na **adresář** a otevřete stránku adresáře společnosti.

    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Klikněte na možnost **Přidat osobu** a otevřete formulář a zadejte příkaz provést následující možnosti –

    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/Procore_user_add.png)

    a. Do textového pole **jméno** zadejte jméno uživatele (například **Britta**).

    b. Do textového pole **příjmení** zadejte příjmení uživatele, například **Simon**.

    c. Do textového pole **e-mailová adresa** zadejte e-mailovou adresu uživatele jako BrittaSimon@contoso.com.

    d. Vyberte **šablonu oprávnění** **použít šablonu oprávnění později**.

    e. Klikněte na **Vytvořit**.

4. Zkontroluje a aktualizuje podrobnosti pro nově přidaný kontakt.

    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/Procore_user_check.png)

5. Klikněte na **Uložit a poslat pozvánku** (Pokud se vyžaduje Pozvánka prostřednictvím e-mailu) nebo **uložte** (uložit přímo), abyste dokončili registraci uživatele.
    
    ![Konfigurace jednotného přihlašování](./media/procoresso-tutorial/Procore_user_save.png)

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když kliknete na dlaždici Procore SSO na přístupovém panelu, měli byste se automaticky přihlásit k hlavnímu přihlášení SSO, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

