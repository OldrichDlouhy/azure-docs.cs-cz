---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování s využitím Azure AD SAML Toolkit | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a službou Azure AD SAML Toolkit.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3f4348e7-c34e-43c7-926e-f1b26ffacf6d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/24/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d2681c09030ff0f36938d7a09e1d1b2e9aa645c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82166306"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-azure-ad-saml-toolkit"></a>Kurz: Azure Active Directory integraci jednotného přihlašování pomocí sady nástrojů Azure AD SAML Toolkit

V tomto kurzu se dozvíte, jak integrovat sadu nástrojů Azure AD SAML pomocí Azure Active Directory (Azure AD). Když integrujete sadu nástrojů SAML sady Azure AD s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup ke službě Azure AD SAML Toolkit.
* Umožněte, aby se vaši uživatelé automaticky přihlásili ke službě Azure AD SAML Toolkit pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Předplatné Azure AD SAML Toolkit jednotného přihlašování (SSO) s povoleným jednotným přihlašováním.

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Sada nástrojů SAML pro Azure AD podporuje jednotné přihlašování na více **aktualizacích**
* Po konfiguraci sady Azure AD SAML Toolkit můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Řízení relace se rozšiřuje z podmíněného přístupu. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-azure-ad-saml-toolkit-from-the-gallery"></a>Přidání sady nástrojů SAML pro Azure AD z Galerie

Ke konfiguraci integrace služby Azure AD SAML Toolkit do služby Azure AD je nutné přidat sadu nástrojů Azure AD SAML z Galerie do seznamu spravovaných aplikací SaaS.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** zadejte do vyhledávacího pole **službu Azure AD SAML Toolkit** .
1. Vyberte **Azure AD SAML Toolkit** z panelu výsledků a pak přidejte aplikaci. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-single-sign-on-for-azure-ad-saml-toolkit"></a>Konfigurace a testování jednotného přihlašování Azure AD pro Azure AD SAML Toolkit

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí služby Azure AD SAML Toolkit pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v sadě nástrojů Azure AD SAML Toolkit.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí sady nástrojů SAML pro Azure AD, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[NAKONFIGURUJTE jednotné přihlašování sady SAML sady Azure AD](#configure-azure-ad-saml-toolkit-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte testovacího uživatele pro Azure AD SAML Toolkit](#create-azure-ad-saml-toolkit-test-user)** – můžete mít protějšek B. Simon ve službě Azure AD SAML Toolkit, který je propojený s reprezentací uživatele v Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikace **SAML sady nástrojů Azure AD** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. Na stránce **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL:`https://samltoolkit.azurewebsites.net/`

    b. Do textového pole **identifikátor (ID entity)** zadejte adresu URL:`https://samltoolkit.azurewebsites.net`

    c. Do textového pole **Adresa URL odpovědi** zadejte adresu URL:`https://samltoolkit.azurewebsites.net/SAML/Consume`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné hodnoty. Aktualizujte tyto hodnoty pomocí vlastního přihlašovací adresy URL, identifikátoru a adresy URL odpovědi, které jsou vysvětleny dále v tomto kurzu.

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** Najděte **certifikát (RAW)** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do počítače.

    ![Odkaz na stažení certifikátu](common/certificateraw.png)

1. V části **nastavení sady Azure AD SAML Toolkit** zkopírujte příslušné adresy URL na základě vašeho požadavku.

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

V této části povolíte B. Simon používat jednotné přihlašování pomocí Azure udělením přístupu ke službě Azure AD SAML Toolkit.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **Azure AD SAML Toolkit**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-azure-ad-saml-toolkit-sso"></a>Konfigurace jednotného přihlašování sady Azure AD SAML Toolkit

1. Otevřete nové okno webového prohlížeče (Pokud jste nezaregistrovali na webu Azure AD SAML Toolkit), nejprve se zaregistrujte kliknutím na **registr**. Pokud jste se už zaregistrovali, přihlaste se k webu služby Azure AD SAML Toolkit pomocí zaregistrovaných přihlašovacích údajů pro přihlášení.

    ![Registr sady nástrojů SAML pro Azure AD](./media/saml-toolkit-tutorial/register.png)

1. Klikněte na **konfiguraci SAML**.

    ![Konfigurace SAML sady Azure AD SAML Toolkit](./media/saml-toolkit-tutorial/saml-configure.png)

1. Klikněte na **Vytvořit**.

    ![Služba Azure AD SAML Toolkit vytvořit jednotné přihlašování](./media/saml-toolkit-tutorial/createsso.png)

1. Na stránce **Konfigurace jednotného přihlašování SAML** proveďte následující kroky:

    ![Služba Azure AD SAML Toolkit vytvořit jednotné přihlašování](./media/saml-toolkit-tutorial/fill-details.png)

    1. Do textového pole **Adresa URL pro přihlášení** vložte hodnotu **URL pro přihlášení** , kterou jste zkopírovali z Azure Portal.

    1. Do textového pole **identifikátoru Azure AD** vložte hodnotu **identifikátoru Azure AD** , kterou jste zkopírovali z Azure Portal.

    1. Do textového pole **Adresa URL pro odhlášení** vložte hodnotu **URL pro odhlášení** , kterou jste zkopírovali z Azure Portal.

    1. Klikněte na **zvolit soubor** a nahrajte soubor **certifikátu (RAW)** , který jste stáhli z Azure Portal.

    1. Klikněte na **Vytvořit**.

    1. Kopírování přihlašovacích adres URL, identifikátorů a hodnot adresy URL služby ACS na stránce konfigurace jednotného přihlašování SAML Toolkit a vložení do v **části základní konfigurační** pole v Azure Portal.

### <a name="create-azure-ad-saml-toolkit-test-user"></a>Vytvořit testovacího uživatele pro Azure AD SAML Toolkit

V této části se ve službě Azure AD SAML Toolkit vytvoří uživatel s názvem B. Simon. Azure AD SAML Toolkit podporuje zřizování uživatelů za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel už v sadě Azure AD SAML Toolkit neexistuje, vytvoří se po ověření nový.

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Po kliknutí na dlaždici sady Azure AD SAML Toolkit na přístupovém panelu byste měli být automaticky přihlášeni ke službě Azure AD SAML Toolkit, pro kterou jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další materiály a zdroje informací

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte Azure AD SAML Toolkit s Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chránit sadu Azure AD SAML Toolkit pomocí pokročilých viditelností a ovládacích prvků](https://docs.microsoft.com/cloud-app-security/protect-azure)