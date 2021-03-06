---
title: 'Kurz: Azure Active Directory integrace s PolicyStat | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PolicyStat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 739e4fa45987f33b3a32503eaedf9aae74a7e000
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86499964"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Kurz: Azure Active Directory integrace s PolicyStat

V tomto kurzu se dozvíte, jak integrovat PolicyStat s Azure Active Directory (Azure AD).
Integrace PolicyStat s Azure AD poskytuje následující výhody:

* Můžete kontrolovat v Azure AD, kteří mají přístup k PolicyStat.
* Můžete povolit, aby se vaši uživatelé automaticky přihlásili k PolicyStat (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud předplatné Azure ještě nemáte, napřed si [vytvořte bezplatný účet](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Předpoklady

Ke konfiguraci integrace služby Azure AD s PolicyStat potřebujete následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat měsíční zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/) .
* Předplatné s povoleným PolicyStatm jednotným přihlašováním

> [!NOTE]
> Tato integrace je taky dostupná pro použití z cloudového prostředí Azure AD USA. Tuto aplikaci můžete najít v galerii cloudových aplikací pro státní správu Azure AD USA a nakonfigurovat ji stejným způsobem jako ve veřejném cloudu.

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* PolicyStat podporuje jednotné přihlašování iniciované v **SP**

* PolicyStat podporuje zřizování uživatelů **jenom v čase** .

## <a name="adding-policystat-from-the-gallery"></a>Přidání PolicyStat z Galerie

Pokud chcete nakonfigurovat integraci PolicyStat do služby Azure AD, musíte přidat PolicyStat z Galerie do svého seznamu spravovaných aplikací SaaS.

**Pokud chcete přidat PolicyStat z Galerie, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **PolicyStat**, vyberte **PolicyStat** z panelu výsledků a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![PolicyStat v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí PolicyStat na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, musí se zřídit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v PolicyStat.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí PolicyStat, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. **[Nakonfigurujte jednotné přihlašování PolicyStat](#configure-policystat-single-sign-on)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
4. **[Vytvoření PolicyStat Test User](#create-policystat-test-user)** – pro Britta Simon v PolicyStat, který je propojený s reprezentací uživatele Azure AD.
5. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí PolicyStat, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **PolicyStat** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. V části **základní konfigurace SAML** proveďte následující kroky:

    ![Informace o jednotném přihlašování v doméně PolicyStat a adresách URL](common/sp-identifier.png)

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:`https://<companyname>.policystat.com`

    b. Do textového pole **identifikátor (ID entity)** zadejte adresu URL pomocí následujícího vzoru:`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty skutečným přihlašovacím jménem a identifikátorem URL. Pokud chcete získat tyto hodnoty, obraťte se na [tým podpory klienta PolicyStat](http://www.policystat.com/support/) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

4. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

5. Vaše aplikace PolicyStat očekává kontrolní výrazy SAML v určitém formátu, což vyžaduje přidání mapování vlastních atributů do konfigurace atributů tokenu SAML. Následující snímek obrazovky ukazuje seznam výchozích atributů. Kliknutím na tlačítko **Upravit** ikonu otevřete dialogové okno **atributy uživatele** .

    ![image](common/edit-attribute.png)

6. Kromě výše očekává aplikace PolicyStat několik dalších atributů, které se vrátí zpátky v odpovědi SAML. V části **deklarace identity uživatelů** v dialogovém okně **atributy uživatele** proveďte následující kroky pro přidání atributu tokenu SAML, jak je znázorněno v následující tabulce:

    | Název | Zdrojový atribut |
    |------------------- | -------------------- |
    | UID | ExtractMailPrefix ([pošta]) |

    a. Kliknutím na **Přidat novou deklaraci identity** otevřete dialogové okno **Spravovat deklarace identity uživatelů** .
    
    ![image](common/new-save-attribute.png)

    ![image](./media/policystat-tutorial/attribute01.png)

    b. Do textového pole **název** zadejte název atributu zobrazeného pro tento řádek.

    c. Ponechte **obor názvů** prázdný.

    d. Jako **transformaci**vyberte zdroj.

    e. V seznamu **transformace** zadejte hodnotu atributu zobrazenou pro tento řádek.
    
    f. V seznamu **parametrů 1** zadejte hodnotu atributu zobrazenou pro tento řádek.

    například Klikněte na **Uložit**.

7. V části **Nastavení PolicyStat** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-policystat-single-sign-on"></a>Konfigurace jednotného přihlašování PolicyStat

1. V jiném okně webového prohlížeče se přihlaste k webu PolicyStat společnosti jako správce.

2. Klikněte na kartu **správce** a potom v levém navigačním podokně klikněte na **Konfigurace jednotného přihlašování** .
   
    ![Nabídka správce](./media/policystat-tutorial/ic808633.png "Nabídka správce")

3. Klikněte na **vaše metadata IDP**a potom v části **metadata IDP** proveďte následující kroky:
   
    ![Konfigurace jednotného přihlašování](./media/policystat-tutorial/ic808636.png "Konfigurace jednotného přihlašování")
   
    a. Otevřete stažený soubor metadat, zkopírujte obsah a vložte ho do textového pole **metadata poskytovatele identity** .

    b. Klikněte na **Save Changes** (Uložit změny).

4. Klikněte na **Konfigurovat atributy**a potom v části **konfigurace atributů** proveďte následující kroky:
   
    a. Do textového pole **atributu username** zadejte **UID**.

    b. Do textového pole **název atributu first name** (křestní jméno) zadejte název deklarace identity z Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`** .

    c. Do textového pole **atributu Last Name (příjmení** ) zadejte název deklarace identity z Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`** .

    d. Do textového pole **atribut e-mailu** zadejte název deklarace identity e-mailu z Azure **`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`** .

    e. Klikněte na **Save Changes** (Uložit změny).

5. V části **Nastavení** vyberte **Povolit integraci jednotného přihlašování**.
   
    ![Konfigurace jednotného přihlašování](./media/policystat-tutorial/ic808634.png "Konfigurace jednotného přihlašování")


### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte použití jednotného přihlašování v Azure pomocí jednotného přihlašování, a to tím, že udělíte přístup k PolicyStat.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **PolicyStat**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **PolicyStat**.

    ![Odkaz PolicyStat v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé svůj účet a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-policystat-test-user"></a>Vytvořit testovacího uživatele PolicyStat

V této části se v PolicyStat vytvoří uživatel s názvem Britta Simon. PolicyStat podporuje zřizování uživatelů za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel ještě v PolicyStat neexistuje, vytvoří se po ověření nový.

>[!NOTE]
>K zřizování uživatelských účtů Azure AD můžete použít jiné nástroje pro vytváření uživatelských účtů PolicyStat nebo rozhraní API poskytovaná PolicyStat.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici PolicyStat, měli byste se automaticky přihlásit k PolicyStat, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další materiály

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
