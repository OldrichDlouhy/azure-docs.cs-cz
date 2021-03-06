---
title: 'Kurz: integrace s Azure Active Directory s předpokládanou sadou CX | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a předpokládanou sadou CX.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: c78fa8ec470f04bcd88ab403249f77a65120c707
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67550392"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Kurz: Azure Active Directory integrací s předpokládanou sadou CX

V tomto kurzu se dozvíte, jak integrovat sadu předsady CX s Azure Active Directory (Azure AD).
Integrací předvídatelné sady CX pomocí Azure AD získáte následující výhody:

* Můžete řídit v Azure AD, kteří mají přístup k předvídat sadě CX.
* Uživatelům můžete umožnit, aby se automaticky přihlásili k předplatnému systému CX Suite (jednotné přihlašování) pomocí svých účtů Azure AD.
* Účty můžete spravovat v jednom centrálním umístění – Azure Portal.

Pokud chcete získat další podrobnosti o integraci aplikace SaaS s Azure AD, přečtěte si téma [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD s využitím předvídatelné sady CX budete potřebovat následující položky:

* Předplatné služby Azure AD. Pokud nemáte prostředí Azure AD, můžete získat [bezplatný účet](https://azure.microsoft.com/free/) .
* Předvídat předplatné sady CX s povoleným jednotným přihlašováním

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Předpokládaná sada CX Suite podporuje jednotné přihlašování se spuštěnou **SP**

* Předvídatelná sada CX Suite podporuje zřizování uživatelů **jenom v čase**

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>Přidání sady předvídatelná CX Suite z Galerie

Pokud chcete nakonfigurovat integraci sady předsady CX do Azure AD, musíte do seznamu spravovaných aplikací SaaS přidat z Galerie předpokládanou sadu CX.

**Pokud chcete přidat z Galerie předpokládanou sadu CX, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** na levém navigačním panelu klikněte na ikonu **Azure Active Directory** .

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Přejděte na **podnikové aplikace** a vyberte možnost **všechny aplikace** .

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **Nová aplikace** v horní části dialogového okna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte příkaz **předvídatelná CX Suite**, vyberte možnost **předvídatelná CX Suite** z panelu výsledků a kliknutím na tlačítko **Přidat** přidejte aplikaci.

     ![Předvídat sadu CX v seznamu výsledků](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

V této části nakonfigurujete a otestujete jednotné přihlašování Azure AD s předplatným sady CX na základě testovacího uživatele s názvem **Britta Simon**.
Aby jednotné přihlašování fungovalo, je potřeba zřídit odkaz na propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v sadě předvídatelná CX Suite.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí předvídatelné sady CX, musíte dokončit tyto stavební bloky:

1. **[Nakonfigurujte jednotné přihlašování Azure AD](#configure-azure-ad-single-sign-on)** a Umožněte uživatelům používat tuto funkci.
2. Pokud chcete nakonfigurovat jednotné přihlašování na straně aplikace, **[nakonfigurujte pro jednotné přihlašování možnost předpokládaná sada CX Suite](#configure-foresee-cx-suite-single-sign-on)** .
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvořením Předpředvídatelného testovacího uživatele sady CX Suite](#create-foresee-cx-suite-test-user)** – můžete mít protějšek Britta Simon v předprodukční sadě CX, který je propojený s reprezentací uživatele v Azure AD.
6. **[Otestujte jednotné přihlašování](#test-single-sign-on)** – ověřte, jestli konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace jednotného přihlašování Azure AD

V této části povolíte jednotné přihlašování Azure AD v Azure Portal.

Pokud chcete nakonfigurovat jednotné přihlašování pomocí Azure AD s využitím předvídatelné sady CX, proveďte následující kroky:

1. V [Azure Portal](https://portal.azure.com/)na stránce **předpokládaná integrace aplikací sady CX Suite** vyberte **jednotné přihlašování**.

    ![Konfigurovat odkaz jednotného přihlašování](common/select-sso.png)

2. V dialogovém okně **Vyberte metodu jednotného přihlašování** vyberte možnost režim **SAML/WS** , čímž povolíte jednotné přihlašování.

    ![Režim výběru jednotného přihlašování](common/select-saml-option.png)

3. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na **Upravit** ikona a otevře se základní dialogové okno **Konfigurace SAML** .

    ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. Pokud máte **soubor metadat poskytovatele služeb**v **základní části Konfigurace SAML** , proveďte následující kroky:

    a. Klikněte na **nahrát soubor metadat**.

    ![Nahrát soubor metadat](common/upload-metadata.png)

    b. Kliknutím na **logo složky** vyberte soubor metadat a klikněte na **nahrát**.

    ![zvolit soubor metadat](common/browse-upload-metadata.png)

    c. Po úspěšném nahrání souboru metadat se hodnota **identifikátoru** automaticky naplní v základní části Konfigurace SAML.

    ![Předvídatelné informace o jednotném přihlašování k doméně a adresám URL sady CX Suite](common/sp-identifier.png)

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL:`https://cxsuite.foresee.com/`

    b. Do textového pole **identifikátor** zadejte adresu URL pomocí následujícího vzoru: https:\//www.okta.com/Saml2/Service-Provider/\<UniqueID>

    > [!Note]
    > Pokud hodnota **identifikátoru** nezíská auto polulated, zadejte hodnotu ručně podle vzoru výše. Hodnota identifikátoru není reálné číslo. Aktualizuje tuto hodnotu skutečným identifikátorem. Pokud chcete získat tuto hodnotu, obraťte se na [tým podpory pro klienta podpory sady CX Suite](mailto:support@foresee.com) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a Stáhněte si **XML federačních metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

6. V části **Nastavení Předpředvídatelné sady CX** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

### <a name="configure-foresee-cx-suite-single-sign-on"></a>Konfigurovat jednotné přihlašování sady # CX Suite

Pokud chcete nakonfigurovat jednotné přihlašování na straně **předvídatelné sady CX Suite** , je potřeba odeslat stažený **soubor XML federačních metadat** a příslušné zkopírované adresy URL z Azure Portal k [Předočekávanému týmu podpory sady CX Suite](mailto:support@foresee.com). Toto nastavení nastaví, aby bylo správně nastaveno připojení SAML SSO na obou stranách.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD 

Cílem této části je vytvořit testovacího uživatele v Azure Portal s názvem Britta Simon.

1. V Azure Portal v levém podokně vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.

    ![Odkazy "uživatelé a skupiny" a "Všichni uživatelé"](common/users.png)

2. V horní části obrazovky vyberte **Nový uživatel** .

    ![Tlačítko pro nového uživatele](common/new-user.png)

3. Ve vlastnostech uživatele proveďte následující kroky.

    ![Uživatelský dialog](common/user-properties.png)

    a. Do pole **název** zadejte **BrittaSimon**.
  
    b. Do pole **uživatelské jméno** zadejte brittasimon@yourcompanydomain.extension. Například BrittaSimon@contoso.com.

    c. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli heslo.

    d. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon pro použití jednotného přihlašování pomocí Azure tím, že udělíte přístup k předprodukční sadě CX.

1. V Azure Portal vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **předpokládaná sada CX Suite**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte možnost **předvídatelná sada CX Suite**.

    ![Odkaz na sadu MorphX sady CX v seznamu aplikací](common/all-applications.png)

3. V nabídce na levé straně vyberte **Uživatelé a skupiny**.

    ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

4. Klikněte na tlačítko **Přidat uživatele** a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Podokno přidat přiřazení](common/add-assign-user.png)

5. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **Britta Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

6. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, pak v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.

7. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-foresee-cx-suite-test-user"></a>Vytvoření předvídatelné sady testů CX Suite Test User

V této části vytvoříte uživatele s názvem Britta Simon v tématu předvídatelná sada CX Suite. Pokud chcete přidat uživatele nebo doménu, která musí být přidána do seznamu povolených pro platformu předvídatelné sada CX Suite, pracujte s [předpokládaným týmem podpory sady CX Suite](mailto:support@foresee.com) . Pokud je doména přidána týmem, uživatelé se automaticky zřídí k platformě předvídatelné sady CX Suite. Před použitím jednotného přihlašování je nutné vytvořit a aktivovat uživatele.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici předvídatelné sady pro sadu CX, měli byste být automaticky přihlášení do sady předvídatelná CX Suite, pro kterou jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

