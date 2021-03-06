---
title: 'Kurz: Azure Active Directory integrace s izolovaným prostorem Salesforce | Microsoft Docs'
description: Přečtěte si, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9d6d11b6b56483f954049fdc1858db31f35c14a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "76289999"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integraci jednotného přihlašování s izolovaným prostorem Salesforce

V tomto kurzu se dozvíte, jak integrovat izolovaný prostor Salesforce s Azure Active Directory (Azure AD). Když integrujete izolovaný prostor Salesforce s Azure AD, můžete:

* Řízení ve službě Azure AD, která má přístup k izolovanému prostoru Salesforce
* Umožněte uživatelům, aby se automaticky přihlásili do izolovaného prostoru Salesforce pomocí svých účtů Azure AD.
* Spravujte svoje účty v jednom centrálním umístění – Azure Portal.

Další informace o integraci aplikací SaaS s Azure AD najdete v tématu [co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Požadavky

Chcete-li začít, potřebujete následující položky:

* Předplatné služby Azure AD. Pokud předplatné nemáte, můžete získat [bezplatný účet](https://azure.microsoft.com/free/).
* Odběr služby Salesforce Sandbox jednotného přihlašování (SSO).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu nakonfigurujete a otestujete jednotné přihlašování Azure AD v testovacím prostředí.

* Izolovaný prostor Salesforce podporuje **aktualizace SP a IDP** .
* Izolovaný prostor Salesforce podporuje zřizování uživatelů **jenom v čase**
* Izolovaný prostor Salesforce podporuje [ **automatizované** zřizování uživatelů](salesforce-sandbox-provisioning-tutorial.md)
* Jakmile nakonfigurujete izolovaný prostor Salesforce, můžete vymáhat ovládací prvky relací, které chrání exfiltrace a infiltraci citlivých dat vaší organizace v reálném čase. Ovládací prvky relace přesahují podmíněný přístup. [Přečtěte si, jak vynutili řízení relace pomocí Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Přidání izolovaného prostoru Salesforce z Galerie

Pokud chcete nakonfigurovat integraci izolovaného prostoru Salesforce do Azure AD, musíte do seznamu spravovaných aplikací SaaS přidat izolovaný prostor Salesforce z galerie.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
1. V levém navigačním podokně vyberte službu **Azure Active Directory** .
1. Přejděte na **podnikové aplikace** a pak vyberte **všechny aplikace**.
1. Chcete-li přidat novou aplikaci, vyberte možnost **Nová aplikace**.
1. V části **Přidat z Galerie** zadejte do vyhledávacího pole **Sandbox typu Salesforce** .
1. Vyberte **izolovaný prostor Salesforce** z panelu výsledků a pak přidejte aplikaci. Počkejte několik sekund, než se aplikace přidá do vašeho tenanta.


## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce-sandbox"></a>Konfigurace a testování jednotného přihlašování Azure AD pro izolovaný prostor Salesforce

Nakonfigurujte a otestujte jednotné přihlašování Azure AD s izolovaným prostorem Salesforce pomocí testovacího uživatele s názvem **B. Simon**. Aby jednotné přihlašování fungovalo, musíte vytvořit propojení mezi uživatelem služby Azure AD a souvisejícím uživatelem v izolovaném prostoru Salesforce.

Pokud chcete nakonfigurovat a otestovat jednotné přihlašování Azure AD s izolovaným prostorem Salesforce, dokončete následující stavební bloky:

1. **[NAKONFIGURUJTE jednotné přihlašování Azure AD](#configure-azure-ad-sso)** – umožníte uživatelům používat tuto funkci.
    * **[Vytvořte testovacího uživatele Azure AD](#create-an-azure-ad-test-user)** – k otestování jednotného přihlašování Azure AD pomocí B. Simon.
    * **[Přiřaďte testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)** – Pokud chcete povolit B. Simon používat jednotné přihlašování Azure AD.
1. **[NAKONFIGURUJTE jednotné přihlašování k izolovanému prostoru Salesforce](#configure-salesforce-sandbox-sso)** – ke konfiguraci nastavení jednotného přihlašování na straně aplikace.
    * **[Vytvoření testovacího uživatele izolovaného prostoru Salesforce](#create-salesforce-sandbox-test-user)** – Chcete-li mít protějšek B. Simon v izolovaném prostoru (sandbox) služby Salesforce, který je propojený s reprezentací uživatele Azure AD.
1. **[Test SSO](#test-sso)** – ověřte, zda konfigurace funguje.

## <a name="configure-azure-ad-sso"></a>Konfigurace jednotného přihlašování v Azure AD

Pomocí těchto kroků povolíte jednotné přihlašování služby Azure AD v Azure Portal.

1. V [Azure Portal](https://portal.azure.com/)na stránce integrace aplikace **izolovaného prostoru Salesforce** najděte část **Správa** a vyberte **jednotné přihlašování**.
1. Na stránce **Vyberte metodu jednotného přihlašování** vyberte **SAML**.
1. Na stránce **nastavit jednotné přihlašování pomocí SAML** klikněte na ikonu Upravit/pero pro **základní konfiguraci SAML** a upravte nastavení.

   ![Upravit základní konfiguraci SAML](common/edit-urls.png)

4. Pokud v **základním oddílu konfigurace SAML** máte **soubor metadat Service Provider** a chcete ho nakonfigurovat v režimu iniciované **IDP** , proveďte následující kroky:

    a. Klikněte na **nahrát soubor metadat**.

    ![Nahrát soubor metadat](common/upload-metadata.png)

    b. Kliknutím na **logo složky** vyberte soubor metadat a klikněte na **nahrát**.

    ![zvolit soubor metadat](common/browse-upload-metadata.png)

    > [!NOTE]
    > Soubor metadat poskytovatele služeb získáte na portálu pro správu izolovaného prostoru Salesforce, který je vysvětlen dále v tomto kurzu.

    c. Po úspěšném nahrání souboru metadat se hodnota **adresy URL odpovědi** získá automaticky v textovém poli **adresy URL odpovědi** .

    ![image](common/both-replyurl.png)

    > [!Note]
    > Pokud hodnota **adresy URL odpovědi** nezíská auto polulated, pak tuto hodnotu vyplňte ručně podle vašeho požadavku.

5. Na stránce **nastavit jednotné přihlašování pomocí SAML** v části **podpisový certifikát SAML** klikněte na **Stáhnout** a stáhněte **XML metadat** z daných možností podle vašich požadavků a uložte ho do svého počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

6. V části **Nastavení izolovaného prostoru Salesforce** zkopírujte příslušné adresy URL podle vašich požadavků.

    ![Kopírovat adresy URL konfigurace](common/copy-configuration-urls.png)

    a. Přihlašovací adresa URL

    b. Identifikátor Azure AD

    c. Odhlašovací adresa URL

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

V této části povolíte B. Simon pro použití jednotného přihlašování Azure tím, že udělíte přístup k izolovanému prostoru Salesforce.

1. V Azure Portal vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.
1. V seznamu aplikace vyberte **izolovaný prostor Salesforce**.
1. Na stránce Přehled aplikace najděte část **Správa** a vyberte **Uživatelé a skupiny**.

   ![Odkaz uživatelé a skupiny](common/users-groups-blade.png)

1. Vyberte **Přidat uživatele**a pak v dialogovém okně **Přidat přiřazení** vyberte **Uživatelé a skupiny** .

    ![Odkaz Přidat uživatele](common/add-assign-user.png)

1. V dialogovém okně **Uživatelé a skupiny** vyberte v seznamu uživatelé možnost **B. Simon** a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. Pokud očekáváte hodnotu role v kontrolním výrazu SAML, v dialogovém okně **Vybrat roli** vyberte v seznamu příslušnou roli pro uživatele a pak klikněte na tlačítko **Vybrat** v dolní části obrazovky.
1. V dialogovém okně **Přidat přiřazení** klikněte na tlačítko **přiřadit** .

## <a name="configure-salesforce-sandbox-sso"></a>Konfigurace jednotného přihlašování pro Salesforce Sandbox

1. V prohlížeči otevřete novou kartu a přihlaste se ke svému účtu správce izolovaného prostoru (Salesforce).

2. Klikněte na **ikonu nastavení** **v pravém** horním rohu stránky.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure1.png)

3. Posuňte se dolů k **Nastavení** v levém navigačním podokně a kliknutím na **Identita** rozbalte související část. Pak klikněte na **nastavení jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. Na stránce **nastavení jednotného přihlašování** klikněte na tlačítko **Upravit** .

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure3.png)

5. Vyberte možnost **SAML povolena**a pak klikněte na tlačítko **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. Pokud chcete nakonfigurovat nastavení jednotného přihlašování SAML, klikněte na **Nový ze souboru metadat**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Kliknutím na **zvolit soubor** odešlete soubor XML s metadaty, který jste stáhli z Azure Portal, a kliknete na **vytvořit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. Na stránce **nastavení jednotného přihlašování SAML** se automaticky naplní pole a klikněte na Uložit.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. Na stránce **nastavení jednotného přihlašování** klikněte na tlačítko **Stáhnout metadata** a Stáhněte soubor metadat poskytovatele služby. Tento soubor použijte v části **základní konfigurace SAML** v Azure Portal ke konfiguraci nezbytných adres URL, jak je vysvětleno výše.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure4.png)

10. Pokud chcete nakonfigurovat aplikaci v režimu **SP** iniciované, je nutné splnit následující požadavky:

    a. Měli byste mít ověřenou doménu.

    b. Je nutné nakonfigurovat a povolit doménu v izolovaném prostoru služby Salesforce. postup je vysvětlen dále v tomto kurzu.

    c. V Azure Portal v části **základní konfigurace SAML** klikněte na **nastavit další adresy URL** a proveďte následující kroky:
  
    ![Informace o jednotném přihlašování k doméně a adresám URL izolovaného prostoru Salesforce](common/both-signonurl.png)

    Do textového pole **přihlašovací adresa URL** zadejte hodnotu pomocí následujícího vzoru:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Tato hodnota by se měla zkopírovat z portálu izolovaného prostoru Salesforce, jakmile jste tuto doménu povolili.

11. V části **podpisový certifikát SAML** klikněte na **metadata federace XML** a uložte soubor XML do počítače.

    ![Odkaz na stažení certifikátu](common/metadataxml.png)

12. V prohlížeči otevřete novou kartu a přihlaste se ke svému účtu správce izolovaného prostoru (Salesforce).

13. Klikněte na **ikonu nastavení** **v pravém** horním rohu stránky.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure1.png)

14. Posuňte se dolů k **Nastavení** v levém navigačním podokně a kliknutím na **Identita** rozbalte související část. Pak klikněte na **nastavení jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. Na stránce **nastavení jednotného přihlašování** klikněte na tlačítko **Upravit** .

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure3.png)

16. Vyberte možnost **SAML povolena**a pak klikněte na tlačítko **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. Pokud chcete nakonfigurovat nastavení jednotného přihlašování SAML, klikněte na **Nový ze souboru metadat**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Kliknutím na **zvolit soubor** odešlete soubor XML s metadaty a kliknete na **vytvořit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. Na stránce **nastavení jednotného přihlašování SAML** se pole naplní automaticky, do textového pole **název** zadejte název konfigurace (například *SPSSOWAAD_Test*) a klikněte na Uložit.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Pokud chcete povolit doménu v izolovaném prostoru služby Salesforce, proveďte následující kroky:

    > [!NOTE]
    > Než povolíte doménu, musíte ji vytvořit v izolovaném prostoru (sandbox) Salesforce. Další informace najdete v tématu [Definování názvu domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Po vytvoření domény se ujistěte, že je správně nakonfigurovaná.

21. V levém navigačním podokně v izolovaném prostoru Salesforce klikněte na **nastavení společnosti** a rozbalte související část a potom klikněte na **moje doména**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. V části **Konfigurace ověřování** klikněte na **Upravit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. V části **Konfigurace ověřování** jako **ověřovací služba**vyberte název nastavení jednotného přihlašování SAML, které jste nastavili během konfigurace jednotného přihlašování v izolovaném prostoru Salesforce, a klikněte na **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-salesforce-sandbox-test-user"></a>Vytvořit testovacího uživatele izolovaného prostoru Salesforce

V této části se v izolovaném prostoru Salesforce vytvoří uživatel s názvem Britta Simon. Izolovaný prostor Salesforce podporuje zřizování za běhu, což je ve výchozím nastavení povolené. V této části není žádná položka akce. Pokud uživatel ještě v izolovaném prostoru Salesforce neexistuje, vytvoří se nový, když se pokusíte o přístup k izolovanému prostoru Salesforce. Izolovaný prostor Salesforce taky podporuje automatické zřizování uživatelů. Další podrobnosti najdete [tady](salesforce-sandbox-provisioning-tutorial.md) , jak nakonfigurovat automatické zřizování uživatelů.

## <a name="test-sso"></a>Test SSO 

V této části otestujete konfiguraci jednotného přihlašování Azure AD pomocí přístupového panelu.

Po kliknutí na dlaždici izolovaného prostoru (sandbox) na přístupovém panelu byste měli být automaticky přihlášení do izolovaného prostoru Salesforce, pro který jste nastavili jednotné přihlašování. Další informace o přístupovém panelu najdete v tématu [Úvod do přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Další materiály a zdroje informací

- [Seznam kurzů pro integraci aplikací SaaS s Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co je přístup k aplikacím a jednotné přihlašování pomocí Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co je podmíněný přístup v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Vyzkoušejte si Sandbox Salesforce s Azure AD](https://aad.portal.azure.com/)

- [Co je řízení relace v Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/protect-salesforce)

- [Konfigurace zřizování uživatelů](salesforce-sandbox-provisioning-tutorial.md)

- [Postup ochrany izolovaného prostoru Salesforce s pokročilými viditelnostmi a ovládacími prvky](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
