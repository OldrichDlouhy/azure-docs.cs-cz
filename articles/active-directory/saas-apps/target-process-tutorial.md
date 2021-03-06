---
title: 'Kurz: Azure Active Directory integrace s TargetProcess | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TargetProcess.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 60cd043c34ba2aee365c59feb38cc9dd02d45746
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67089045"
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Kurz: Azure Active Directory integrace s TargetProcess

V tomto kurzu se dozvíte, jak integrovat TargetProcess s Azure Active Directory (Azure AD).
Integrace TargetProcess s Azure AD poskytuje následující výhody:

* Můžete kontrolovat v Azure AD, kteří mají přístup k TargetProcess.
* Můžete povolit, aby se vaši uživatelé automaticky přihlásili k TargetProcess (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s TargetProcess potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/) .
* Předplatné s povoleným TargetProcessm jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* TargetProcess podporuje jednotné přihlašování iniciované v **SP**
* TargetProcess podporuje zřizování uživatelů **jenom v čase** .

## <a name="adding-targetprocess-from-the-gallery"></a>Přidání TargetProcess z Galerie

Pokud chcete nakonfigurovat integraci TargetProcess do služby Azure AD, musíte přidat TargetProcess z Galerie do svého seznamu spravovaných aplikací SaaS.

**Pokud chcete přidat TargetProcess z Galerie, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **TargetProcess**, vyberte **TargetProcess** z panelu výsledků a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![TargetProcess v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí TargetProcess na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, musí se zřídit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v TargetProcess.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí TargetProcess, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Nakonfigurujte jednotné přihlašování TargetProcess](#configure-targetprocess-single-sign-on)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvoření TargetProcess Test User](#create-targetprocess-test-user)** – pro Britta Simon v TargetProcess, který je propojený s reprezentací uživatele Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí TargetProcess, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **TargetProcess** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V části **základní konfigurace SAML** proveďte následující kroky:

    ![Informace o jednotném přihlašování v doméně TargetProcess a adresách URL](common/sp-identifier.png)

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:`https://<subdomain>.tpondemand.com/`

    b. Do textového pole **identifikátor (ID entity)** zadejte adresu URL pomocí následujícího vzoru:`https://<subdomain>.tpondemand.com/`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty skutečným přihlašovacím jménem a identifikátorem URL. Pokud chcete získat tyto hodnoty, obraťte se na [tým podpory klienta TargetProcess](mailto:support@targetprocess.com) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **certifikát (Base64)** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/certificatebase64.png)

6. V části **Nastavení TargetProcess** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-targetprocess-single-sign-on"></a>Konfigurace jednotného přihlašování TargetProcess

1. Pokud chcete automatizovat konfiguraci v rámci **TargetProcess**, je potřeba nainstalovat rozšíření **prohlížeče zabezpečeného přihlašování aplikace** kliknutím na **instalovat rozšíření**.

    ![image](./media/target-process-tutorial/install_extension.png)

2. Po přidání rozšíření do prohlížeče klikněte na **TargetProcess nastavení** a nasměrujte vás na aplikaci TargetProcess. Odtud zadejte přihlašovací údaje správce, které se přihlásí k TargetProcess. Rozšíření prohlížeče automaticky provede konfiguraci aplikace za vás a automatizujte kroky 3-7.

    **Pokud chcete aplikaci nakonfigurovat ručně, proveďte následující kroky:**

3. Přihlaste se k aplikaci TargetProcess jako správce.

4. V nabídce v horní části klikněte na **Nastavení**.

    ![Nastavení](./media/target-process-tutorial/tutorial_target_process_05.png)

5. Klikněte na kartu **Nastavení** .

    ![Nastavení](./media/target-process-tutorial/tutorial_target_process_06.png)

6. Klikněte na kartu **jednotné přihlašování** .

    ![klikněte na jednotné přihlašování.](./media/target-process-tutorial/tutorial_target_process_07.png)

7. V dialogovém okně nastavení jednotného přihlašování proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/target-process-tutorial/tutorial_target_process_08.png)

    a. Klikněte na **Povolit jednotné přihlašování**.

    b. Do textového pole **Adresa URL pro přihlášení** vložte hodnotu **adresy URL pro přihlášení** , kterou jste zkopírovali z Azure Portal.

    c. Otevřete stažený certifikát v programu Poznámkový blok, zkopírujte obsah a vložte ho do textového pole **certifikátu** .

    d. klikněte na **Povolit zřizování JIT**.

    e. Klikněte na **Uložit**.

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

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k TargetProcess.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **TargetProcess**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **TargetProcess**.

    ![Odkaz TargetProcess v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-targetprocess-test-user"></a>Vytvořit testovacího uživatele TargetProcess

V této části se v TargetProcess vytvoří uživatel s názvem Britta Simon. TargetProcess podporuje zřizování uživatelů za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel ještě v TargetProcess neexistuje, vytvoří se po ověření nový.

> [!Note]
> Pokud potřebujete ručně vytvořit uživatele, obraťte se na [tým podpory TargetProcess](mailto:support@targetprocess.com).

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici TargetProcess, měli byste se automaticky přihlásit k TargetProcess, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)