---
title: 'Kurz: Azure Active Directory integrace s konzolou pro správu Mimecast | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a konzolou pro správu Mimecast.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 276a1acb5735e3490f331000799d57c329e7fca0
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848404"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mimecast-admin-console"></a>Kurz: Azure Active Directory integraci jednotného přihlašování s konzolou správce Mimecast

V tomto kurzu se dozvíte, jak integrovat konzolu pro správu Mimecast s Azure Active Directory (Azure AD). Když integrujete konzolu správce Mimecast s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup ke konzole správce Mimecast.
* Umožněte, aby se vaši uživatelé automaticky přihlásili k Mimecast konzole pro správu pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Předplatné Mimecast pro správu jednotného přihlašování (SSO) s povoleným jednotným přihlašováním.

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Konzola pro správu Mimecast podporuje **aktualizace SP a IDP,** iniciované jednotné přihlašování
* Po nakonfigurování konzoly správce Mimecast můžete vynutili řízení relace, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Řízení relace se rozšiřuje z podmíněného přístupu. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Přidání konzoly správce Mimecast z Galerie

Pokud chcete nakonfigurovat integraci konzoly správce Mimecast do služby Azure AD, musíte přidat konzolu správce Mimecast z Galerie do svého seznamu spravovaných aplikací SaaS.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** zadejte do vyhledávacího pole **Mimecast Console admin** .
1. Z panelu výsledků vyberte **Konzola pro správu Mimecast** a pak aplikaci přidejte. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.

## <a name="configure-and-test-azure-ad-single-sign-on-for-mimecast-admin-console"></a>Konfigurace a testování jednotného přihlašování Azure AD pro konzolu pro správu Mimecast

Nakonfigurujte a otestujte jednotné přihlašování Azure AD pomocí konzoly pro správu Mimecast pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte v konzole pro správu Mimecast vytvořit vztah propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD pomocí konzoly pro správu Mimecast, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    1. **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    1. **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[Nakonfigurujte jednotné přihlašování konzoly pro správu Mimecast](#configure-mimecast-admin-console-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
    1. **[Vytvořte testovacího uživatele konzoly pro správu Mimecast](#create-mimecast-admin-console-test-user)** , abyste měli protějšek B. Simon v konzole pro správu Mimecast, která je propojená s reprezentací uživatele v Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace **konzolových aplikací Správce Mimecast** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

1. Pokud chcete nakonfigurovat aplikaci v režimu iniciované IDP, proveďte v **základní části Konfigurace SAML** následující kroky:

    a. Do textového pole **identifikátor** zadejte adresu URL pomocí následujícího vzoru:

    | Oblast  |  Hodnota | 
    | --------------- | --------------- |
    | Evropa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | USA   | `https://us-api.mimecast.com/sso/<accountcode>`|
    | Jižní Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Austrálie       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Prací        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    > [!NOTE]
    > Tuto `accountcode` hodnotu najdete v konzole pro správu Mimecast v části **Account**  >  **Settings**  >  **Kód účtu**nastavení účtu. Připojí `accountcode` k identifikátoru.

    b. Do textového pole **Adresa URL odpovědi** zadejte adresu URL: 

    | Oblast  |  Hodnota | 
    | --------------- | --------------- | 
    | Evropa          | `https://eu-api.mimecast.com/login/saml`|
    | USA   | `https://us-api.mimecast.com/login/saml`|
    | Jižní Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Austrálie       | `https://au-api.mimecast.com/login/saml`|
    | Prací        | `https://jer-api.mimecast.com/login/saml`|

1. Pokud chcete nakonfigurovat aplikaci v režimu iniciované **SP** :

    Do textového pole **přihlašovací adresa URL** zadejte adresu URL: 

    | Oblast  |  Hodnota | 
    | --------------- | --------------- | 
    | Evropa          | `https://login-eu.mimecast.com/administration/app/#/administration-dashboard`|
    | USA   | `https://login-us.mimecast.com/administration/app/#/administration-dashboard`|
    | Jižní Afrika    | `https://login-za.mimecast.com/administration/app/#/administration-dashboard`|
    | Austrálie       | `https://login-au.mimecast.com/administration/app/#/administration-dashboard`|
    | Prací        | `https://login-jer.mimecast.com/administration/app/#/administration-dashboard`|

1. Klikněte na **Uložit**.

1. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** kliknutím na tlačítko Kopírovat zkopírujte **adresu URL federačních metadat aplikace** a uložte ji do svého počítače.

    ![Odkaz na stažení certifikátu](common/copy-metadataurl.png)

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

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup ke konzole pro správu Mimecast.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte možnost **Konzola pro správu Mimecast**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-mimecast-admin-console-sso"></a>Konfigurace jednotného přihlašování konzoly pro správu Mimecast

1. V jiném okně webového prohlížeče se přihlaste ke konzole pro správu Mimecast.

1. Přejděte na **aplikace pro správu**  >  **Services**  >  **Applications**.

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/services.png)

1. Klikněte na kartu **profily ověřování** .
    
    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/authentication-profiles.png)

1. Klikněte na **Nová karta profil ověřování** .

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/new-authenticatio-profile.png)

1. Do textového pole **Popis** zadejte platný popis a zaškrtněte políčko **vyhovět ověřování SAML pro konzolu pro správu** .

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/selecting-admin-consle.png)

1. Na stránce **konzoly Konfigurace SAML pro správu** proveďte následující kroky:

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/sso-settings.png)

    a. V rozevíracím seznamu **poskytovatel**vyberte **Azure Active Directory** .

    b. Do textového pole **Adresa URL metadat** vložte hodnotu **adresy URL federačních metadat aplikace** , kterou jste zkopírovali z Azure Portal.

    c. Klikněte na **importovat**. Po importu adresy URL metadat budou pole naplněna automaticky, takže v těchto polích není nutné provádět žádné akce.

    d. Ujistěte se, že jste zaškrtli políčko **použít kontext chráněný heslem** a **použijete kontexty integrovaného ověřování** .

    e. Klikněte na **Uložit**.

### <a name="create-mimecast-admin-console-test-user"></a>Vytvořit testovacího uživatele konzoly pro správu Mimecast

1. V jiném okně webového prohlížeče se přihlaste ke konzole pro správu Mimecast.

1. Přejděte do **složky pro správu**  >  **Directories**  >  **interní adresáře**.

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/internal-directories.png)

1. Pokud je doména uvedená níže, vyberte v doméně. v opačném případě vytvořte novou doménu kliknutím na **novou doménu**.

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/domain-name.png)

1. Klikněte na kartu **Nová adresa** .

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/new-address.png)

1. Zadejte požadované informace o uživateli na následující stránce:

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/user-information.png)

    a. Do textového pole **e-mailová adresa** zadejte e-mailovou adresu uživatele, jako je `B.Simon@yourdomainname.com` .

    b. Do textového pole **globální název** zadejte jméno a **příjmení** uživatele.

    c. Do textových polí **heslo** a **potvrzení hesla** zadejte heslo uživatele.

    d. Zaškrtněte políčko **Vynutit změnu při přihlášení** .

    e. Klikněte na **Uložit**.

    f. Pokud chcete přiřadit role uživateli, klikněte na **Upravit roli** a přiřaďte požadovanou roli uživateli podle požadavků vaší organizace.

    ![Konfigurace konzoly správce Mimecast](./media/mimecast-admin-console-tutorial/assign-role.png)

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Po kliknutí na dlaždici konzoly pro správu Mimecast na přístupovém panelu byste měli být automaticky přihlášeni ke konzole pro správu Mimecast, pro kterou jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další zdroje

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si konzolu pro správu Mimecast s Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chránit konzolu správce Mimecast s pokročilými viditelnostmi a ovládacími prvky](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
