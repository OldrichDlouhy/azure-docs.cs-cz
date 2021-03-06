---
title: 'Kurz: Azure Active Directory integrace s Vtiger CRM (SAML) | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Vtiger CRM (SAML).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f14e34a6-f51f-4cd1-a6ad-f04df551303d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ccd1304ae43a2f424dab94cf67e7cc41357579fa
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86167024"
---
# <a name="tutorial-integrate-vtiger-crm-saml-with-azure-active-directory"></a>Kurz: integrace Vtiger CRM (SAML) pomocí Azure Active Directory

V tomto kurzu se dozvíte, jak integrovat Vtiger CRM (SAML) pomocí Azure Active Directory (Azure AD). Když integrujete Vtiger CRM (SAML) s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k Vtiger CRM (SAML).
* Umožněte uživatelům, aby se automaticky přihlásili k Vtiger CRM (SAML) pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat zdarma [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* Předplatné Vtiger CRM (SAML) jednotného přihlašování (SSO) s povoleným jednotným přihlašováním.

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí. 

* Vtiger CRM (SAML) podporuje jednotné přihlašování (SSO) iniciované v **SP**
* Vtiger CRM (SAML) podporuje zřizování uživatelů **jenom v čase**

## <a name="adding-vtiger-crm-saml-from-the-gallery"></a>Přidání aplikace Vtiger CRM (SAML) z Galerie

Pokud chcete nakonfigurovat integraci Vtiger CRM (SAML) do Azure AD, musíte do seznamu spravovaných aplikací pro SaaS přidat Vtiger CRM (SAML) z galerie.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** do vyhledávacího pole zadejte **VTIGER CRM (SAML)** .
1. Z panelu výsledků vyberte **VTIGER CRM (SAML)** a pak přidejte aplikaci. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a testování jednotného přihlašování Azure AD

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí Vtiger CRM (SAML) pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte v Vtiger CRM (SAML) vytvořit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Vtiger CRM (SAML), dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
2. **[NAKONFIGURUJTE jednotné přihlašování VTIGER CRM (SAML)](#configure-vtiger-crm-saml-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
3. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí Britta Simon.
4. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – pro povolení Britta Simon pro použití jednotného přihlašování Azure AD.
5. **[Vytvoření testovacího uživatele VTIGER CRM (SAML)](#create-vtiger-crm-saml-test-user)** – Pokud chcete mít protějšek Britta Simon v Vtiger CRM (SAML), který je propojený s reprezentací uživatele v Azure AD.
6. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

### <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikace **VTIGER CRM (SAML)** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. Na stránce **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru:

   - `https://<customer_instance>.od1.vtiger.com`
   - `https://<customer_instance>.od2.vtiger.com`
   - `https://<customer_instance>.od1.vtiger.ws`

    b. Do textového pole **identifikátor (ID entity)** zadejte adresu URL pomocí následujícího vzoru:`https://<customer_instance>.od1.vtiger.com/sso/saml?acs`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty skutečným přihlašovacím jménem a identifikátorem URL. K získání těchto hodnot se obraťte na [tým podpory klienta podpory VTIGER CRM (SAML)](mailto:support@vtiger.com) . Můžete se také podívat na vzory uvedené v části **základní konfigurace SAML** v Azure Portal.

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** vyhledejte **certifikát (Base64)** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do počítače.

   ![Odkaz na stažení certifikátu](common/certificatebase64.png)

1. V části **Nastavení VTIGER CRM (SAML)** zkopírujte příslušné adresy URL na základě vašeho požadavku.

   ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

### <a name="configure-vtiger-crm-saml-sso"></a>Konfigurace jednotného přihlašování Vtiger CRM (SAML)

Ke konfiguraci jednotného přihlašování na straně **VTIGER CRM (SAML)** je potřeba odeslat stažený **certifikát (Base64)** a příslušné zkopírované adresy URL z Azure Portal do [týmu podpory Vtiger CRM (SAML)](mailto:support@vtiger.com). Toto nastavení nastaví, aby bylo správně nastaveno připojení SAML SSO na obou stranách.

### <a name="create-an-azure-ad-test-user"></a>Vytvoření testovacího uživatele Azure AD

V této části vytvoříte testovacího uživatele ve Azure Portal s názvem B. Simon.

1. V levém podokně Azure Portal vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.
1. V horní části obrazovky vyberte **Nový uživatel** .
1. Ve vlastnostech **uživatele** proveďte následující kroky:
   1. Do pole **Název** zadejte `B.Simon`.  
   1. Do pole **uživatelské jméno** zadejte username@companydomain.extension . Například, `B.Simon@contoso.com`.
   1. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli **heslo** .
   1. Klikněte na **Vytvořit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte B. Simon používat jednotné přihlašování pomocí Azure tím, že udělíte přístup k Vtiger CRM (SAML).

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **VTIGER CRM (SAML)**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

### <a name="create-vtiger-crm-saml-test-user"></a>Vytvoření testovacího uživatele Vtiger CRM (SAML)

V této části se v Vtiger CRM (SAML) vytvoří uživatel s názvem Britta Simon. Vtiger CRM (SAML) podporuje zřizování uživatelů za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel v Vtiger CRM (SAML) ještě neexistuje, vytvoří se po ověření nový.

### <a name="test-sso"></a>Test SSO

Když vyberete dlaždici Vtiger CRM (SAML) na přístupovém panelu, měli byste se automaticky přihlásit k Vtiger CRM (SAML), pro který jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)