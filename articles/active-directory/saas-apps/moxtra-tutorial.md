---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování s Moxtra | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3e53ba11744b0e78287ffc46c4aac7b99b16b23
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "74889545"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-moxtra"></a>Kurz: Azure Active Directory integraci jednotného přihlašování (SSO) s Moxtra

V tomto kurzu se dozvíte, jak integrovat Moxtra s Azure Active Directory (Azure AD). Když integrujete Moxtra s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k Moxtra.
* Umožněte, aby se vaši uživatelé automaticky přihlásili k Moxtra svým účtům Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Moxtra odběr s povoleným jednotným přihlašováním (SSO).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Moxtra podporuje jednotné přihlašování iniciované v **SP**

> [!NOTE]
> Identifikátorem této aplikace je pevná řetězcová hodnota, takže v jednom tenantovi může být nakonfigurovaná jenom jedna instance.

## <a name="adding-moxtra-from-the-gallery"></a>Přidání Moxtra z Galerie

Pokud chcete nakonfigurovat integraci Moxtra do služby Azure AD, musíte přidat Moxtra z Galerie do svého seznamu spravovaných aplikací SaaS.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** do vyhledávacího pole zadejte **Moxtra** .
1. Na panelu výsledků vyberte **Moxtra** a pak aplikaci přidejte. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.


## <a name="configure-and-test-azure-ad-single-sign-on-for-moxtra"></a>Konfigurace a testování jednotného přihlašování Azure AD pro Moxtra

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí Moxtra pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, je potřeba vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Moxtra.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Moxtra, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[Nakonfigurujte MOXTRA SSO](#configure-moxtra-sso)** – pro konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte Moxtra Test User](#create-moxtra-test-user)** -to, abyste měli protějšek B. Simon v Moxtra, která je propojená s reprezentací uživatele v Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **Moxtra** Najděte oddíl **Spravovat** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V části **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL:`https://www.moxtra.com/service/#login`

1. Moxtra aplikace očekává kontrolní výrazy SAML v určitém formátu, což vyžaduje přidání mapování vlastních atributů do konfigurace atributů tokenu SAML. Následující snímek obrazovky ukazuje seznam výchozích atributů. Kliknutím na tlačítko **Upravit** ikonu otevřete dialogové okno atributy uživatele.

    ![image](common/edit-attribute.png)

1. Kromě výše očekává aplikace Moxtra několik dalších atributů, které se vrátí zpátky v odpovědi SAML. V části deklarace identity uživatelů v dialogovém okně atributy uživatele proveďte následující kroky pro přidání atributu tokenu SAML, jak je znázorněno v následující tabulce:

    | Název | Zdrojový atribut|
    | ------------------- | -------------------- |    
    | FirstName | User. křestní jméno |
    | polím | User. příjmení |
    | idpid    | < > identifikátoru Azure AD

    > [!Note]
    > Hodnota atributu **idpid** není reálné číslo. Aktuální hodnotu můžete získat z části **Nastavení Moxtra** z kroku č. 8. 

    1. Kliknutím na **Přidat novou deklaraci identity** otevřete dialogové okno **Spravovat deklarace identity uživatelů** .

    1. Do textového pole **název** zadejte název atributu zobrazeného pro tento řádek.

    1. Ponechte **obor názvů** prázdný.

    1. Jako **atribut**vyberte zdroj.

    1. V seznamu **zdrojový atribut** zadejte hodnotu atributu zobrazenou pro tento řádek.

    1. Klikněte na **OK** .

    1. Klikněte na **Uložit**.

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** vyhledejte **certifikát (Base64)** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do počítače.

    ![Odkaz na stažení certifikátu](common/certificatebase64.png)

1. V části **Nastavení Moxtra** zkopírujte na základě vašeho požadavku příslušné adresy URL.

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

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup k Moxtra.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **Moxtra**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-moxtra-sso"></a>Konfigurace jednotného přihlašování Moxtra

1. V jiném okně prohlížeče se přihlaste k webu Moxtra společnosti jako správce.

2. Na panelu nástrojů na levé straně klikněte na **Konzola pro správu > jednotné přihlašování SAML**a potom klikněte na **Nový**.
   
    ![Konfigurace jednotného přihlašování](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

3. Na stránce **SAML** proveďte následující kroky:
   
    ![Konfigurace jednotného přihlašování](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. Do textového pole **název** zadejte název vaší konfigurace (např.: *SAML*). 
  
    b. Do textového pole **ID entity IDP** vložte hodnotu **identifikátoru služby Azure AD** , který jste zkopírovali z Azure Portal. 
 
    c. Do textového pole **Adresa URL pro přihlášení** vložte hodnotu **adresy URL pro přihlášení** , kterou jste zkopírovali z Azure Portal. 
 
    d. Do textového pole **AuthnContextClassRef** zadejte **urn: Oasis: names: TC: SAML: 2.0: AC: Classes: Password**. 
 
    e. Do textového pole **Formát NameId** zadejte **urn: Oasis: names: TC: SAML: 1.1: NameId-Format: EmailAddress**. 
 
    f. Otevřete certifikát, který jste si stáhli z Azure Portal v programu Poznámkový blok, zkopírujte obsah a vložte ho do textového pole **certifikátu** .    
 
    g. Do textového pole e-mailová doména SAML zadejte svou e-mailovou doménu SAML.    
  
    >[!NOTE]
    >Chcete-li zobrazit kroky pro ověření domény, klikněte na tlačítko "**i**" níže.

    h. Klikněte na **Aktualizovat**.

### <a name="create-moxtra-test-user"></a>Vytvořit testovacího uživatele Moxtra

Cílem této části je vytvořit uživatele s názvem B. Simon v Moxtra.

**Chcete-li vytvořit uživatele s názvem B. Simon v Moxtra, proveďte následující kroky:**

1. Přihlaste se k webu Moxtra společnosti jako správce.

1. Na panelu nástrojů na levé straně klikněte na **Konzola pro správu > Správa uživatelů**a pak na **Přidat uživatele**.
   
    ![Konfigurace jednotného přihlašování](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

1. V dialogovém okně **Přidat uživatele** proveďte následující kroky:
  
    a. Do textového pole **jméno v prvním** poli zadejte **B**.
  
    b. Do textového pole **příjmení** zadejte **Simon**.
  
    c. Do textového pole **e-mail** zadejte B. Simon e-mailová adresa stejné jako na Azure Portal.
  
    d. Do textového pole **dělení** zadejte **dev**.
  
    e. Do textového pole **oddělení** **Zadejte.**
  
    f. Vyberte **správce**.
  
    g. Klikněte na tlačítko **Add** (Přidat).

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Moxtra, měli byste se automaticky přihlásit k Moxtra, pro které jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další materiály a zdroje informací

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si Moxtra s Azure AD](https://aad.portal.azure.com/)

