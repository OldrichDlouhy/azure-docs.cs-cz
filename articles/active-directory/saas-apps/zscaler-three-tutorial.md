---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování (SSO) s Zscaler tři | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Zscaler tři.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b23abcf9a39ce7f6d77bc40e7143505bc68e8b72
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "72554986"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zscaler-three"></a>Kurz: Azure Active Directory integraci jednotného přihlašování (SSO) s Zscaler tři

V tomto kurzu se dozvíte, jak integrovat Zscaler tři s Azure Active Directory (Azure AD). Když integrujete Zscaler tři s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k Zscaler třem.
* Umožněte, aby se vaši uživatelé automaticky přihlásili k Zscaler třem pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Zscaler tři odběry s povoleným jednotným přihlašováním (SSO).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Zscaler tři podporují jednotné přihlašování se spuštěnou **aktualizací SP**

* Zscaler tři podporují zřizování uživatelů **jenom v čase**

> [!NOTE]
> Identifikátorem této aplikace je pevná řetězcová hodnota, takže v jednom tenantovi může být nakonfigurovaná jenom jedna instance.

## <a name="adding-zscaler-three-from-the-gallery"></a>Přidání Zscaler tři z Galerie

Pokud chcete nakonfigurovat integraci Zscaler tří do Azure AD, musíte do seznamu spravovaných aplikací pro SaaS přidat Zscaler tři z galerie.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** do vyhledávacího pole zadejte **Zscaler tři** .
1. Z panelu výsledků vyberte **Zscaler tři** a pak aplikaci přidejte. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zscaler-three"></a>Konfigurace a testování jednotného přihlašování Azure AD pro Zscaler tři

Nakonfigurujte a otestujte jednotné přihlašování Azure AD s Zscaler tři pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, je potřeba vytvořit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Zscaler tři.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Zscaler tří, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[Nakonfigurujte Zscaler tři jednotné přihlašování](#configure-zscaler-three-sso)** – pro konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte Zscaler tři testovací uživatele](#create-zscaler-three-test-user)** – abyste měli protějšek B. Simon v Zscaler třech, který je propojený s reprezentací uživatele v Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace **tří aplikací Zscaler** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V části **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL:`https://login.zscalerthree.net/sfc_sso`

1. Vaše Zscaler tři aplikace očekává kontrolní výrazy SAML v určitém formátu, což vyžaduje přidání mapování vlastních atributů do konfigurace atributů tokenu SAML. Následující snímek obrazovky ukazuje seznam výchozích atributů.

    ![image](common/edit-attribute.png)

6. Kromě toho Zscaler tři aplikace očekává, že se v odpovědi SAML vrátí další atributy, které jsou uvedené dál. Tyto atributy se také předem naplní, ale můžete je zkontrolovat podle vašich požadavků.
    
    | Název | Zdrojový atribut |
    | ---------| ------------ |
    | memberOf     | User. assignedroles |

    > [!NOTE]
    > Chcete-li zjistit, jak nakonfigurovat roli v Azure AD, klikněte prosím [sem](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) .

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** vyhledejte **certifikát (Base64)** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do počítače.

    ![Odkaz na stažení certifikátu](common/certificatebase64.png)

1. V části **nastavit tři Zscaler** zkopírujte příslušné adresy URL na základě vašeho požadavku.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

V této části vytvoříte testovacího uživatele ve Azure Portal s názvem B. Simon.

1. V levém podokně Azure Portal vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.
1. V horní části obrazovky vyberte **Nový uživatel** .
1. Ve vlastnostech **uživatele** proveďte následující kroky:
   1. Do pole **Název** zadejte `B.Simon`.  
   1. Do pole **uživatelské jméno** zadejte username@companydomain.extension. Například, `B.Simon@contoso.com`.
   1. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli **heslo** .
   1. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte B. Simon používat jednotné přihlašování pomocí Azure tím, že udělíte přístup k Zscaler tři.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **Zscaler tři**.
1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatele, jako je **Britta Simon** , a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_users.png)

1. V dialogu **Vybrat roli** vyberte příslušnou roli uživatele v seznamu a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_roles.png)

1. V dialogovém okně **Přidat přiřazení** vyberte tlačítko **přiřadit** .

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_assign.png)

## <a name="configure-zscaler-three-sso"></a>Konfigurace Zscaler tři jednotné přihlašování

1. Pokud chcete automatizovat konfiguraci v rámci Zscaler tří, je potřeba nainstalovat rozšíření **prohlížeče zabezpečeného přihlašování aplikace** kliknutím na **instalovat rozšíření**.

    ![Rozšíření moje aplikace](common/install-myappssecure-extension.png)

2. Po přidání rozšíření do prohlížeče klikněte na **Nastavení Zscaler tři** a nasměrujte vás na Zscalerovou aplikaci. Odtud zadejte přihlašovací údaje správce, abyste se přihlásili k Zscaler třem. Rozšíření prohlížeče automaticky provede konfiguraci aplikace za vás a automatizujte kroky 3-6.

    ![Nastavení](common/setup-sso.png)

3. Pokud chcete nastavit Zscaler tři ručně, otevřete nové okno webového prohlížeče a přihlaste se ke svému Zscaler webu společnosti jako správce a proveďte následující kroky:

4. V části **správa > ověřování > nastavení ověřování** a proveďte následující kroky:
   
    ![Správa](./media/zscaler-three-tutorial/ic800206.png "Správa")

    a. V části typ ověřování vyberte **SAML**.

    b. Klikněte na **Konfigurovat SAML**.

5. V okně **Upravit SAML** proveďte následující kroky: a klikněte na Uložit.  
            
    ![Správa uživatelů & ověřování](./media/zscaler-three-tutorial/ic800208.png "Správa uživatelů & ověřování")
    
    a. Do textového pole **Adresa URL portálu SAML** vložte **přihlašovací adresu URL** , kterou jste zkopírovali z Azure Portal.

    b. Do textového pole **atributu přihlašovací jméno** zadejte **NameId**.

    c. Klikněte na **nahrát**a nahrajte podpisový certifikát Azure SAML, který jste stáhli z Azure Portal ve **veřejném certifikátu SSL**.

    d. Přepněte **možnost povolit Automatické zřizování SAML**.

    e. Do textového pole **atribut zobrazovaného jména uživatele** zadejte **DisplayName** , pokud chcete povolit Automatické zřizování SAML pro atributy DisplayName.

    f. Do textového pole **atributu název skupiny** zadejte **memberOf** , pokud chcete povolit Automatické zřizování SAML pro atributy memberOf.

    g. V **atributu název oddělení** zadejte **oddělení** , pokud chcete povolit Automatické zřizování SAML pro atributy oddělení.

    h. Klikněte na **Uložit**.

6. Na stránce **Konfigurovat ověření uživatele** proveďte následující kroky:

    ![Správa](./media/zscaler-three-tutorial/ic800207.png)

    a. Najeďte myší na nabídku **Aktivace** v blízkosti levého dolního rohu.

    b. Klikněte na tlačítko **aktivovat**.

## <a name="configuring-proxy-settings"></a>Konfigurace nastavení proxy serveru
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Konfigurace nastavení proxy serveru v Internet Exploreru

1. Spusťte **aplikaci Internet Explorer**.

2. V nabídce **nástroje** vyberte **Možnosti Internetu** . otevře se dialogové okno **Možnosti Internetu** .   
    
     ![Možnosti Internetu](./media/zscaler-three-tutorial/ic769492.png "Možnosti Internetu")

3. Klikněte na kartu **připojení** .   
  
     ![Připojení](./media/zscaler-three-tutorial/ic769493.png "Připojení")

4. Kliknutím na **Nastavení místní sítě** otevřete dialogové okno **nastavení sítě LAN** .

5. V části proxy server proveďte následující kroky:   
   
    ![Proxy server](./media/zscaler-three-tutorial/ic769494.png "Proxy server")

    a. Vyberte **použít proxy server pro vaši síť LAN**.

    b. Do textového pole Adresa zadejte **Gateway. Zscaler Three.net**.

    c. Do textového pole Port zadejte **80**.

    d. Vyberte možnost **obejít proxy server pro místní adresy**.

    e. Kliknutím na tlačítko **OK** zavřete dialogové okno **Nastavení místní sítě (LAN)** .

6. Kliknutím na tlačítko **OK** zavřete dialogové okno **Možnosti Internetu** .

### <a name="create-zscaler-three-test-user"></a>Vytvořit Zscaler tři testovací uživatele

V této části je uživatel nazvaný B. Simon vytvořen v Zscaler 3. Zscaler tři podporují zřizování za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel ještě v Zscaler tři neexistuje, vytvoří se nový, když se pokusíte o přístup k Zscaler tři.

>[!Note]
>Pokud potřebujete ručně vytvořit uživatele, kontaktujte [Zscaler tři tým podpory](https://www.zscaler.com/company/contact).

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Zscaler, měli byste se automaticky přihlásit k Zscaler tři, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další materiály a zdroje informací

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si Zscaler tři s Azure AD](https://aad.portal.azure.com/)

