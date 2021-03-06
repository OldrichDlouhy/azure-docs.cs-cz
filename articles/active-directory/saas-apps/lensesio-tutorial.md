---
title: 'Kurz: Azure Active Directory integraci jednotného přihlašování s Lenses.io | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lenses.io.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2a0d4a7c-a171-48c6-b1c1-f2bd728fb37f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/02/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2b630111261be8e3615ab45e95633040e799551
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050985"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-the-lensesio-dataops-portal"></a>Kurz: Azure Active Directory integraci jednotného přihlašování (SSO) k portálu DataOps pro Lenses.io.

V tomto kurzu se dozvíte, jak integrovat portál [lenses.IO](https://lenses.io/) DataOps s Azure Active Directory (Azure AD). Když integrujete Lenses.io s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k portálu Lenses.io.
* Umožněte uživatelům, aby se automaticky přihlásili k objektivu pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Předpoklady

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Instance portálu čočky. Portál pro rozptylová skla můžete nasadit [různými způsoby](https://lenses.io/product/deployment/).
* [Licence](https://lenses.io/product/pricing/) lenses.IO, která podporuje jednotné přihlašování (SSO).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Lenses.io podporuje jednotné přihlašování iniciované v **SP**

* Po nakonfigurování Lenses.io můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Řízení relace se rozšiřuje z podmíněného přístupu. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-lensesio-from-the-gallery"></a>Přidání Lenses.io z Galerie

Pokud chcete nakonfigurovat integraci Lenses.io do služby Azure AD, musíte přidat Lenses.io z Galerie do svého seznamu spravovaných aplikací SaaS.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** do vyhledávacího pole zadejte **lenses.IO** .
1. Na panelu výsledků vyberte **lenses.IO** a pak aplikaci přidejte. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.


## <a name="configure-and-test-azure-ad-sso-for-lensesio"></a>Konfigurace a testování jednotného přihlašování Azure AD pro Lenses.io

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí portálu Lenses.io pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, je potřeba vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v Lenses.io.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí Lenses.io, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele a skupinu Azure AD](#create-an-azure-ad-test-user-and-group)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[Nakonfigurujte LENSES.IO SSO](#configure-lensesio-sso)** – pro konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte lenses.IO oprávnění skupiny testu](#create-lensesio-test-group-permissions)** – abyste mohli řídit, co B. Simon má mít přístup v lenses.IO (autorizaci).
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikací **lenses.IO** Najděte oddíl **Spravovat** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. V části **základní konfigurace SAML** zadejte hodnoty pro následující pole:

    a. Do textového pole **přihlašovací adresa URL** zadejte adresu URL pomocí následujícího vzoru: `https://<CUSTOMER_LENSES_BASE_URL>` např.`https://lenses.my.company.com`

    b. Do textového pole **identifikátor (ID entity)** zadejte adresu URL pomocí následujícího vzoru: `https://<CUSTOMER_LENSES_BASE_URL>` např.`https://lenses.my.company.com`

    c. Do textového pole **Adresa URL odpovědi** zadejte adresu URL pomocí následujícího vzoru:`https://<CUSTOMER_LENSES_BASE_URL>/api/v2/auth/saml/callback?client_name=SAML2Client`
    například.`https://lenses.my.company.com/api/v2/auth/saml/callback?client_name=SAML2Client`

    > [!NOTE]
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty pomocí skutečné přihlašovací adresy URL, adresy URL odpovědi a identifikátoru na základě základní adresy URL vaší instance portálu čočky. Další informace najdete v [dokumentaci k LENSES.IO SSO](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0).

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** Najděte **XML metadata federace** a vyberte **Stáhnout** a Stáhněte certifikát a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

1. V části **nastavení lenses.IO** použijte soubor XML výše a nakonfigurujete rozptylová skla v rámci služby Azure SSO.

### <a name="create-an-azure-ad-test-user-and-group"></a>Vytvoření testovacího uživatele a skupiny Azure AD

V této části vytvoříte testovacího uživatele ve Azure Portal s názvem B. Simon. Také vytvoříte testovací skupinu pro B. Simon, která bude sloužit k řízení toho, jaký přístup B. Simon má v čočky.
Můžete zjistit, jak rozptylová skla používá mapování členství ve skupině pro autorizaci v [dokumentaci k jednotnému přihlašování pomocí čočky](https://docs.lenses.io/install_setup/configuration/security.html#id3) .

1. V levém podokně Azure Portal vyberte možnost **Azure Active Directory**, vyberte možnost **Uživatelé**a potom vyberte možnost **Všichni uživatelé**.
1. V horní části obrazovky vyberte **Nový uživatel** .
1. Ve vlastnostech **uživatele** proveďte následující kroky:
   1. Do pole **Název** zadejte `B.Simon`.  
   1. Do pole **uživatelské jméno** zadejte username@companydomain.extension . Například, `B.Simon@contoso.com`.
   1. Zaškrtněte políčko **Zobrazit heslo** a pak zapište hodnotu, která se zobrazí v poli **heslo** .
   1. Klikněte na **Vytvořit**.

Vytvoření skupiny:
1. Zpět na **Azure Active Directory**vyberte **skupiny** .
1. V horní části obrazovky vyberte **Nová skupina** .
1. Ve **vlastnostech skupiny**proveďte tyto kroky:
   1. V poli **typ skupiny** vyberte `Security` .
   1. Do pole **název skupiny** zadejte`LensesUsers`
   1. Klikněte na **Vytvořit**.
1. Vyberte skupinu `LensesUsers` a poznamenejte si **ID objektu** (např. `f8b5c1ec-45de-4abd-af5c-e874091fb5f7` ). Toto ID se použije v čočky k mapování uživatelů této skupiny na [správná oprávnění](https://docs.lenses.io/install_setup/configuration/security.html#id3).  
   
Přiřazení skupiny k testovacímu uživateli: 
1. Vraťte se zpět na **Azure Active Directory**vyberte **Uživatelé**.
1. Vyberte testovacího uživatele `B.Simon` .
1. Vyberte **skupiny**.
1. V horní části obrazovky vyberte **Přidat členství** .
1. Vyhledejte `LensesUsers` a vyberte ji.
1. Klikněte na **Vybrat**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup k Lenses.io.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **lenses.IO**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-lensesio-sso"></a>Konfigurace jednotného přihlašování Lenses.io

Ke konfiguraci jednotného přihlašování na portálu **lenses.IO** nainstalujete stažený **soubor XML federačních metadat** do instance čočky a [nakonfigurujete rozptylová skla pro povolení jednotného přihlašování](https://docs.lenses.io/install_setup/configuration/security.html#configure-lenses). 

### <a name="create-lensesio-test-group-permissions"></a>Vytvořit oprávnění skupiny testů Lenses.io

V této části vytvoříte skupinu v objektivech pomocí **ID objektu** skupiny, kterou jsme si `LensesUsers` poznamenali v [části Vytvoření](#create-an-azure-ad-test-user-and-group)uživatele.
Přiřadíte požadovaná oprávnění, která `B.Simon` by měla mít v čočky.
Další informace najdete v části [mapování skupin Azure-čočky](https://docs.lenses.io/install_setup/configuration/security.html#azure-groups).

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Když na přístupovém panelu kliknete na dlaždici Lenses.io, měli byste se automaticky přihlásit k portálu Lenses.io, pro který jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje informací

- [Nastavení jednotného přihlašování v instanci Lenses.io](https://docs.lenses.io/install_setup/configuration/security.html#single-sign-on-sso-saml-2-0)

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si Lenses.io s Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chránit Lenses.io pomocí pokročilých viditelností a ovládacích prvků](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
