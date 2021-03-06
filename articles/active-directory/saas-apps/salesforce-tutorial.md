---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování s Salesforce | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f66a5ba491a5afed4007f73b88a38719ec71fb63
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849157"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce"></a>Kurz: Azure Active Directory integraci jednotného přihlašování se službou Salesforce

V tomto kurzu se dozvíte, jak integrovat Salesforce s Azure Active Directory (Azure AD). Když integrujete Salesforce s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k Salesforce.
* Umožněte uživatelům, aby se automaticky přihlásili k Salesforce pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Odběr s povoleným jednotným přihlašováním (SSO) Salesforce.

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Salesforce podporuje jednotné přihlašování (SSO) iniciované **SP**

* Salesforce podporuje [ **automatizované** zřizování a rušení zřizování uživatelů](salesforce-provisioning-tutorial.md) (doporučeno)

* Salesforce podporuje zřizování uživatelů **jenom v čase** .

* Mobilní aplikace Salesforce se teď dá nakonfigurovat s Azure AD pro povolení jednotného přihlašování. V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.
* Jakmile nakonfigurujete Salesforce, můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Řízení relace se rozšiřuje z podmíněného přístupu. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-from-the-gallery"></a>Přidání Salesforce z Galerie

Pokud chcete nakonfigurovat integraci Salesforce do Azure AD, musíte přidat Salesforce z Galerie do svého seznamu spravovaných aplikací SaaS.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** do vyhledávacího pole zadejte **Salesforce** .
1. Vyberte **Salesforce** z panelu výsledků a pak přidejte aplikaci. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce"></a>Konfigurace a testování jednotného přihlašování Azure AD pro Salesforce

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí Salesforce pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Salesforce.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Salesforce, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    * **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    * **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[NAKONFIGURUJTE jednotné přihlašování Salesforce](#configure-salesforce-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
    * **[Vytvořte testovacího uživatele Salesforce](#create-salesforce-test-user)** , abyste měli protějšek B. Simon v Salesforce, který je propojený s reprezentací uživatele Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **Salesforce** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V části **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    a. Do textového pole **přihlašovací adresa URL** zadejte hodnotu pomocí následujícího vzoru:

    Podnikový účet:`https://<subdomain>.my.salesforce.com`

    Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`
    
    b. Do textového pole **Adresa URL odpovědi** zadejte hodnotu pomocí následujícího vzoru:

    Podnikový účet:`https://<subdomain>.my.salesforce.com`

    Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`

    c. Do textového pole **identifikátor** zadejte hodnotu pomocí následujícího vzoru:

    Podnikový účet:`https://<subdomain>.my.salesforce.com`

    Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty pomocí skutečné přihlašovací adresy URL a identifikátoru. Pokud chcete získat tyto hodnoty, obraťte se na [tým podpory klienta Salesforce](https://help.salesforce.com/support) .

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** Najděte **XML metadata federace** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

1. V části **Nastavení Salesforce** zkopírujte příslušné adresy URL na základě vašeho požadavku.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

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

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup k Salesforce.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **Salesforce**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-salesforce-sso"></a>Konfigurace jednotného přihlašování Salesforce

1. K automatizaci konfigurace v rámci Salesforce je potřeba nainstalovat rozšíření **prohlížeče pro zabezpečené přihlašování k aplikacím** kliknutím na **instalovat rozšíření**.

    ![Rozšíření moje aplikace](common/install-myappssecure-extension.png)

1. Po přidání rozšíření do prohlížeče klikněte na **nastavit Salesforce** . budete přesměrováni na aplikaci jednotného přihlašování Salesforce. Odtud zadejte přihlašovací údaje správce pro přihlášení do jednotného přihlašování Salesforce. Rozšíření prohlížeče automaticky provede konfiguraci aplikace za vás a automatizujte kroky 3-13.

    ![Konfigurace instalace](common/setup-sso.png)

1. Pokud chcete nastavit Salesforce ručně, otevřete nové okno webového prohlížeče a přihlaste se k webu Salesforce společnosti jako správce a proveďte následující kroky:

1. Klikněte na **ikonu nastavení** **v pravém** horním rohu stránky.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/configure1.png)

1. Posuňte se dolů k **Nastavení** v navigačním podokně a kliknutím na **Identita** rozbalte související část. Pak klikněte na **nastavení jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-admin-sso.png)

1. Na stránce **nastavení jednotného přihlašování** klikněte na tlačítko **Upravit** .

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Pokud pro svůj účet Salesforce nemůžete povolit nastavení jednotného přihlašování, možná budete muset kontaktovat [tým podpory klienta Salesforce](https://help.salesforce.com/support).

1. Vyberte možnost **SAML povolena**a pak klikněte na tlačítko **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-enable-saml.png)

1. Pokud chcete nakonfigurovat nastavení jednotného přihlašování SAML, klikněte na **Nový ze souboru metadat**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-admin-sso-new.png)

1. Kliknutím na **zvolit soubor** odešlete soubor XML s metadaty, který jste stáhli z Azure Portal, a kliknete na **vytvořit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/xmlchoose.png)

1. Na stránce **nastavení jednotného přihlašování SAML** se automaticky naplní pole, vyberte **povolené zřizování uživatelů** a pak klikněte na **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/salesforcexml.png)

1. V levém navigačním podokně v Salesforce klikněte na **nastavení společnosti** a rozbalte související část a potom klikněte na **moje doména**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-my-domain.png)

1. Přejděte dolů k části **Konfigurace ověřování** a klikněte na tlačítko **Upravit** .

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-edit-auth-config.png)

1. V části **Konfigurace ověřování** Zkontrolujte službu **AzureSSO** as **Authentication služby** konfigurace jednotného přihlašování SAML a pak klikněte na **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Pokud je vybraná víc než jedna ověřovací služba, zobrazí se uživatelům výzva k výběru ověřovací služby, se kterou se chtějí přihlásit, a současně se zahájí jednotné přihlašování k vašemu prostředí Salesforce. Pokud nechcete, aby k tomu docházelo, měli byste **všechny ostatní ověřovací služby nechat nezaškrtnuté**.

### <a name="create-salesforce-test-user"></a>Vytvořit testovacího uživatele Salesforce

V této části se v Salesforce vytvoří uživatel s názvem B. Simon. Salesforce podporuje zřizování za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel v Salesforce ještě neexistuje, vytvoří se nový, když se pokusíte o přístup k Salesforce. Salesforce podporuje taky Automatické zřizování uživatelů. Další podrobnosti najdete [tady](salesforce-provisioning-tutorial.md) , jak nakonfigurovat automatické zřizování uživatelů.

## <a name="test-sso"></a>Test SSO

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když kliknete na dlaždici Salesforce na přístupovém panelu, měli byste být automaticky přihlášeni k Salesforce, pro který jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="test-sso-for-salesforce-mobile"></a>Testování jednotného přihlašování pro Salesforce (mobilní zařízení)

1. Otevřete mobilní aplikaci Salesforce. Na přihlašovací stránce klikněte na **použít vlastní doménu**.

    ![Mobilní aplikace Salesforce](media/salesforce-tutorial/mobile-app1.png)

1. Do textového pole **vlastní doména** zadejte registrovaný název vlastní domény a klikněte na **pokračovat**.

    ![Mobilní aplikace Salesforce](media/salesforce-tutorial/mobile-app2.png)

1. Zadejte svoje přihlašovací údaje služby Azure AD, abyste se přihlásili do aplikace Salesforce, a klikněte na **Další**.

    ![Mobilní aplikace Salesforce](media/salesforce-tutorial/mobile-app3.png)

1. Na stránce **Povolit přístup** , jak je uvedeno níže, klikněte na **Povolit** a umožněte přístup k aplikaci Salesforce.

    ![Mobilní aplikace Salesforce](media/salesforce-tutorial/mobile-app4.png)

1. Nakonec po úspěšném přihlášení se zobrazí domovská stránka aplikace.

    ![](media/salesforce-tutorial/mobile-app5.png) ![ Mobilní aplikace Salesforce mobilní aplikace Salesforce](media/salesforce-tutorial/mobile-app6.png)

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Konfigurace zřizování uživatelů](salesforce-provisioning-tutorial.md)

- [Vyzkoušejte si Salesforce s Azure AD](https://aad.portal.azure.com)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chránit Salesforce pomocí pokročilých viditelností a ovládacích prvků](https://docs.microsoft.com/cloud-app-security/protect-salesforce)
