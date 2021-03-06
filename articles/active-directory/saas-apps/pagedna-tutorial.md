---
title: 'Kurz: Azure Active Directory integrace s PageDNA | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PageDNA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c8765864-45f4-48c2-9d86-986a4aa431e4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34e496ea9d2a89894951856a19854bff18f20a8b
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/01/2020
ms.locfileid: "85800870"
---
# <a name="tutorial-azure-active-directory-integration-with-pagedna"></a>Kurz: Azure Active Directory integrace s PageDNA

V tomto kurzu se dozvíte, jak integrovat PageDNA s Azure Active Directory (Azure AD).

Integrace PageDNA s Azure AD poskytuje následující výhody:

* V Azure AD můžete řídit, kdo má přístup k PageDNA.
* Uživatelům můžete povolit, aby se automaticky přihlásili k PageDNA (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění: Azure Portal.

Podrobnosti o integraci aplikací SaaS (software jako služba) se službou Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s PageDNA potřebujete následující položky:

* Předplatné služby Azure AD. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
* Předplatné PageDNA s povoleným jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí a integrujete PageDNA se službou Azure AD.

PageDNA podporuje následující funkce:

* V případě spuštění jednotného přihlašování (SSO) bylo inicializováno SP.

* Zřizování uživatelů za běhu.

## <a name="add-pagedna-from-the-azure-marketplace"></a>Přidat PageDNA z Azure Marketplace

Pokud chcete nakonfigurovat integraci PageDNA do služby Azure AD, musíte přidat PageDNA z Azure Marketplace do seznamu spravovaných aplikací SaaS:

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com?azure-portal=true).
1. V levém podokně vyberte **Azure Active Directory**.

    ![Možnost Azure Active Directory](common/select-azuread.png)

1. Vyberte možnost **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Podokno podnikové aplikace](common/enterprise-applications.png)

1. Chcete-li přidat novou aplikaci, v horní části podokna vyberte **+ Nová aplikace** .

    ![Možnost nové aplikace](common/add-new-app.png)

1. Do vyhledávacího pole zadejte **PageDNA**. Ve výsledcích hledání vyberte **PageDNA**a pak vyberte **Přidat** , aby se aplikace přidala.

    ![PageDNA v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD pomocí PageDNA na základě testovacího uživatele s názvem **Britta Simon**. Aby jednotné přihlašování fungovalo, musíte vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v PageDNA.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí PageDNA, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** , aby mohli vaši uživatelé používat tuto funkci.
1. **[Nakonfigurujte jednotné přihlašování PageDNA](#configure-pagedna-single-sign-on)** ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** pro testování jednotného přihlašování Azure AD pomocí Britta Simon.
1. **[Přiřaďte testovacímu uživateli Azure AD](#assign-the-azure-ad-test-user)** , aby mohl Britta Simon používat jednotné přihlašování Azure AD.
1. **[Vytvořte testovacího uživatele PageDNA](#create-a-pagedna-test-user)** , aby byl uživatel s názvem Britta Simon v PageDNA, který je propojený s uživatelem služby Azure AD s názvem Britta Simon.
1. **[Otestujte jednotné přihlašování](#test-single-sign-on)** a ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování Azure AD pomocí PageDNA, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **PageDNA** vyberte **jednotné přihlašování**.

    ![Konfigurovat možnost jednotného přihlašování](common/select-sso.png)

1. V podokně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

1. V podokně **nastavit jednotné přihlašování pomocí SAML** vyberte **Upravit** (ikona tužky) a otevřete **základní podokno konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V podokně **základní konfigurace SAML** proveďte následující kroky:

    ![Informace o jednotném přihlašování v doméně PageDNA a adresách URL](common/sp-identifier.png)

    1. Do pole **přihlašovací adresa URL** zadejte adresu URL pomocí jednoho z následujících vzorů:

        ```https
        https://stores.pagedna.com/<your site>
        https://<your domain>
        https://<your domain>/<your site>
        https://www.nationsprint.com/<your site>
        ```

    1. V poli **identifikátor (ID entity)** zadejte adresu URL pomocí jednoho z následujících vzorů:

        ```https
        https://stores.pagedna.com/<your site>/saml2ep.cgi
        https://www.nationsprint.com/<your site>/saml2ep.cgi
        ```

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty pomocí skutečné přihlašovací adresy URL a identifikátoru. Chcete-li získat tyto hodnoty, obraťte se na [tým podpory PageDNA](mailto:success@pagedna.com). Můžete také odkazovat na vzory zobrazené v podokně **základní konfigurace SAML** v Azure Portal.

1. V podokně **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** vyberte **Stáhnout** a ze daných možností stáhněte **certifikát (RAW)** a uložte ho do svého počítače.

    ![Možnost stažení certifikátu (RAW)](common/certificateraw.png)

1. V části **Nastavení PageDNA** zkopírujte adresy URL nebo adresy URL, které potřebujete:

   * **Přihlašovací adresa URL**
   * **Identifikátor Azure AD**
   * **Odhlašovací adresa URL**

    ![Kopírovat konfigurační adresy URL](common/copy-configuration-urls.png)

### <a name="configure-pagedna-single-sign-on"></a>Konfigurace jednotného přihlašování PageDNA

Ke konfiguraci jednotného přihlašování na straně PageDNA odešlete stažený certifikát (RAW) a příslušné zkopírované adresy URL z Azure Portal do [týmu podpory PageDNA](mailto:success@pagedna.com). Tým PageDNA zajistí, že připojení SAML SSO je na obou stranách správně nastavené.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

V této části vytvoříte testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte **Azure Active Directory**    >  **Uživatelé**  >  **Všichni uživatelé**.

    ![Možnosti uživatelé a všichni uživatelé](common/users.png)

1. V horní části obrazovky vyberte **+ Nový uživatel**.

    ![Možnost Nový uživatel](common/new-user.png)

1. V podokně **uživatel** proveďte následující kroky:

    ![Podokno uživatele](common/user-properties.png)

    1. Do pole **název** zadejte **BrittaSimon**.
  
    1. Do pole **uživatelské jméno** zadejte **BrittaSimon \@ \<yourcompanydomain> . \<extension> **. Například **BrittaSimon \@ contoso.com**.

    1. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli **heslo** .

    1. Vyberte **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte, aby uživatel Britta Simon používat jednotné přihlašování pomocí Azure tím, že udělí uživateli přístup k PageDNA.

1. V Azure Portal vyberte možnost **podnikové aplikace**  >  **všechny aplikace**  >  **PageDNA**.

    ![Podokno podnikové aplikace](common/enterprise-applications.png)

1. V seznamu aplikace vyberte **PageDNA**.

    ![PageDNA v seznamu aplikací](common/all-applications.png)

1. V levém podokně v části **Spravovat**vyberte **Uživatelé a skupiny**.

    ![Možnost Uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **+ Přidat uživatele**a pak v podokně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

1. V podokně **Uživatelé a skupiny** vyberte v seznamu **uživatelů** položku **Britta Simon** a pak zvolte **Vybrat** v dolní části podokna.

1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v podokně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele. V dolní části podokna zvolte **možnost vybrat**.

1. V podokně **Přidat přiřazení** vyberte **přiřadit**.

### <a name="create-a-pagedna-test-user"></a>Vytvořit testovacího uživatele v PageDNA

V PageDNA se teď vytvoří uživatel s názvem Britta Simon. K vytvoření tohoto uživatele nemusíte nic dělat. PageDNA podporuje zřizování uživatelů za běhu, což je ve výchozím nastavení povolené. Pokud uživatel s názvem Britta Simon v PageDNA ještě neexistuje, vytvoří se po ověření nový.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí portálu moje aplikace.

Když vyberete **PageDNA** na portálu moje aplikace, měli byste být automaticky přihlášeni k předplatnému PageDNA, pro které jste nastavili jednotné přihlašování. Další informace o portálu moje aplikace najdete v tématu věnovaném [přístupu a používání aplikací na portálu moje aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Jednotné přihlašování k aplikacím v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

