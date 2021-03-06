---
title: 'Kurz: Azure Active Directory integrace s Mitel Connect | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mitel Connect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: 26a761708f56ff7aba8daf86d2991579e60291cb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "81870195"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect"></a>Kurz: integrace s Azure Active Directory Mitel MiCloud Connect

V tomto kurzu se dozvíte, jak integrovat Mitel MiCloud Connect s Azure Active Directory (Azure AD). Integrace MiCloud Connect s Azure AD poskytuje následující výhody:

* Můžete řídit v Azure AD, kteří mají přístup k aplikacím MiCloud Connect pomocí svých podnikových přihlašovacích údajů.
* Můžete povolit, aby se uživatelé na vašem účtu automaticky přihlásili k MiCloud připojení (jednotné přihlašování) ke svým účtům Azure AD.


Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s MiCloud Connect budete potřebovat následující položky:

* Předplatné Azure AD

  Pokud nemáte prostředí Azure AD, můžete získat [bezplatný účet](https://azure.microsoft.com/free/) .
* Účet Mitel MiCloud Connect

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování (SSO) Azure AD.

* Mitel Connect podporuje jednotné přihlašování iniciované v **SP**

## <a name="adding-mitel-connect-from-the-gallery"></a>Přidání Mitel Connect z Galerie

Pokud chcete nakonfigurovat integraci Mitel připojení ke službě Azure AD, musíte přidat Mitel připojit z Galerie do seznamu spravovaných aplikací pro SaaS v Azure Portal.

**Pokud chcete přidat Mitel Connect z Galerie, proveďte následující kroky:**

1. Na webu **[Azure Portal](https://portal.azure.com)** klikněte na levém navigačním panelu na **Azure Active Directory**.

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Klikněte na **podnikové aplikace** a pak klikněte na **všechny aplikace**.

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Klikněte na **Nová aplikace**.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **Mitel připojit** , klikněte na **Mitel připojit** z panelu výsledků a pak klikněte na **Přidat**.

     ![Mitel Connect v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí MiCloud Connect na základě testovacího uživatele s názvem **Britta Simon**. Aby jednotné přihlašování fungovalo, musí být navázán odkaz na odkaz mezi uživatelem služby Azure AD a souvisejícím uživatelem v MiCloud Connect.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí MiCloud Connect, musíte provést následující kroky:

1. **[Nakonfigurujte MiCloud Connect pro jednotné přihlašování pomocí Azure AD](#configure-micloud-connect-for-sso-with-azure-ad)** – umožníte uživatelům používat tuto funkci a nakonfigurovat nastavení jednotného přihlašování na straně aplikace.
2. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
3. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
4. **[Vytvořte testovacího uživatele Mitel MiCloud Connect](#create-a-mitel-micloud-connect-test-user)** – abyste měli protějšek Britta Simon na svém účtu MiCloud Connect, který je propojený s reprezentací uživatele v Azure AD.
5. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>Konfigurace MiCloud Connect pro jednotné přihlašování pomocí Azure AD

V této části povolíte jednotné přihlašování Azure AD pro MiCloud Connect v Azure Portal a nakonfigurujete účet MiCloud Connect tak, aby povoloval jednotné přihlašování pomocí Azure AD.

Pokud chcete nakonfigurovat MiCloud Connect s SSO pro Azure AD, je nejjednodušší otevřít Azure Portal a portál účtu Mitel vedle sebe. Na portálu účtů Mitel budete muset zkopírovat některé informace z Azure Portal na portál účtu Mitel a některé z portálu účtů na Azure Portal.


1. Stránku konfigurace v [Azure Portal](https://portal.azure.com/)otevřete následujícím způsobem:

    a. Na stránce integrace aplikace **Mitel Connect** klikněte na **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

    b. V dialogovém okně **Vyberte metodu jednotného přihlašování** klikněte na **SAML**.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)
    
    Zobrazí se přihlašovací stránka založená na SAML.

2. Chcete-li otevřít dialogové okno Konfigurace na portálu účtů Mitel, postupujte takto:

    a. V nabídce **systém pro telefon** klikněte na **Doplňky funkce**.

    b. Napravo od **jednotného přihlašování**klikněte na **aktivovat** nebo **Nastavení**.
    
    Zobrazí se dialogové okno připojit nastavení jednotného přihlašování.
    
3. Zaškrtněte políčko **Povolit jednotné přihlašování** .
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_Enable.png)


4. V Azure Portal klikněte na ikonu **Upravit** v části **základní konfigurace SAML** .
    ![image](common/edit-urls.png)

    Zobrazí se základní dialogové okno Konfigurace SAML.

5.  Zkopírujte adresu URL z pole **identifikátor Mitel (ID entity)** na portálu účtů Mitel a vložte ho do pole **identifikátor (ID entity)** v Azure Portal.

6. Zkopírujte adresu URL z pole **Adresa URL odpovědi (adresa URL služby potvrzení)** na portálu účtu Mitel a vložte ji do pole **Adresa URL odpovědi (adresa URL služby potvrzení)** v Azure Portal.  
   ![image](./media/mitel-connect-tutorial/Mitel_Azure_BasicConfig.png)

7. Do textového pole **přihlašovací adresa URL** zadejte jednu z těchto adres URL:

    * **https://portal.shoretelsky.com**– použití portálu účtu Mitel jako výchozí aplikace Mitel
    * **https://teamwork.shoretel.com**– použití týmové práce jako výchozí aplikace Mitel

    **Poznámka**: výchozí aplikace Mitel je aplikace, ke které se přistupuje, když uživatel klikne na dlaždici Mitel Connect na přístupovém panelu. To je také aplikace, ke které se přistupoval při instalaci testu z Azure AD.

8. V Azure Portal základní dialogové okno **Konfigurace SAML** klikněte na **Uložit** .

9. V části **podpisový certifikát SAML** na **přihlašovací stránce založené na SAML** v Azure Portal klikněte na **Stáhnout** další **certifikát (Base64)** a stáhněte **podpisový certifikát** a uložte ho do svého počítače.
    ![image](./media/mitel-connect-tutorial/Azure_SigningCert.png)

10. Otevřete soubor podpisového certifikátu v textovém editoru, zkopírujte všechna data v souboru a vložte data do pole **podpisový certifikát** na portálu účtů Mitel. 
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_SigningCert.png)

11. V části **Nastavení Mitel Connect** na přihlašovací stránce na **bázi SAML** v Azure Portal postupujte takto:

    a. Zkopírujte adresu URL z pole **Adresa URL pro přihlášení** a vložte ji do pole **Adresa URL pro přihlášení** na portálu účtu Mitel.

    b. Zkopírujte adresu URL z pole **identifikátoru Azure AD** a vložte ji do pole **ID entity** na portálu účtu Mitel.
    ![image](./media/mitel-connect-tutorial/Mitel_Azure_SetupConnect.png)

12. V dialogovém okně **připojit nastavení jednotného přihlašování** na portálu účtů Mitel klikněte na **Uložit** .

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD 

V této části vytvoříte testovacího uživatele s názvem Britta Simon v Azure Portal.

1. V Azure Portal v levém podokně klikněte na **Azure Active Directory**, klikněte na **Uživatelé**a potom klikněte na **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky klikněte na **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. V dialogovém okně Vlastnosti uživatele proveďte následující kroky:

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole **uživatelské jméno** zadejte brittasimon@\<yourcompanydomain\>. \<přípona\>.  
Například, BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a potom zapište hodnotu, která se zobrazí v poli **heslo** .

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k Mitel připojení.

1. V Azure Portal klikněte na **podnikové aplikace**a pak klikněte na **všechny aplikace**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace klikněte na **Mitel připojit**.

    ![Odkaz Mitel Connect v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně klikněte na **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** klikněte na **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu **Uživatelé** položku **Britta Simon** a pak klikněte na **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, vyberte příslušnou roli pro uživatele ze seznamu v dialogovém okně **Vybrat roli** a pak klikněte na **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na **přiřadit**.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Vytvoření testovacího uživatele Mitel MiCloud Connect

V této části vytvoříte uživatele s názvem Britta Simon na účtu MiCloud Connect. Před použitím jednotného přihlašování je nutné vytvořit a aktivovat uživatele.

Podrobnosti o přidávání uživatelů na portálu účtů Mitel najdete v článku věnovaném [Přidání uživatele](https://oneview.mitel.com/s/article/Adding-a-User-092815) ve znalostní bázi Mitel.

Na účtu MiCloud Connect vytvořte uživatele s následujícími podrobnostmi:

  * **Název:** Britta Simon

* **Pracovní e-mailová adresa:**`brittasimon@<yourcompanydomain>.<extension>`   
(Příklad: [brittasimon@contoso.com](mailto:brittasimon@contoso.com))

* **Uživatelské jméno:**`brittasimon@<yourcompanydomain>.<extension>`  
(Příklad: [brittasimon@contoso.com](mailto:brittasimon@contoso.com); uživatelské jméno uživatele je obvykle stejné jako pracovní e-mailová adresa uživatele.)

**Poznámka:** Uživatelské jméno MiCloud připojení uživatele musí být stejné jako e-mailová adresa uživatele v Azure.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když kliknete na dlaždici Mitel Connect na přístupovém panelu, měli byste se automaticky přesměrovat tak, aby se přihlásili do aplikace MiCloud Connect, kterou jste nakonfigurovali jako výchozí v poli **Adresa URL pro přihlášení** . Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
