---
title: 'Kurz: Konfigurace G Suite pro Automatické zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: Přečtěte si, jak automaticky zřídit a zrušit zřízení uživatelských účtů z Azure AD do G Suite.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2020
ms.author: Zhchia
ms.openlocfilehash: 5ecf8ca6e6790b218216aed9a6ff82c8a7ac9dd2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85250141"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Kurz: Konfigurace G Suite pro Automatické zřizování uživatelů

Tento kurz popisuje kroky, které je třeba provést v G Suite a Azure Active Directory (Azure AD) ke konfiguraci automatického zřizování uživatelů. Po nakonfigurování Azure AD automaticky zřídí a odzřídí uživatele a skupiny se [sadou G Suite](https://gsuite.google.com/) pomocí služby zřizování Azure AD. Důležité informace o tom, co tato služba dělá, jak funguje a nejčastější dotazy, najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů pro SaaS aplikací pomocí Azure Active Directory](../manage-apps/user-provisioning.md). 

> [!NOTE]
> Tento kurz popisuje konektor založený na službě zřizování uživatelů Azure AD. Důležité informace o tom, co tato služba dělá, jak funguje a nejčastější dotazy, najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů pro SaaS aplikací pomocí Azure Active Directory](../app-provisioning/user-provisioning.md).

> [!NOTE]
> Konektor G Suite se nedávno aktualizoval v říjnu 2019. Mezi změny provedené v konektoru G Suite patří:
>
> * Byla přidána podpora dalších atributů uživatelů a skupin G Suite.
> * Aktualizované názvy atributů cíle G Suite tak, aby odpovídaly, co se [tady](https://developers.google.com/admin-sdk/directory)definuje.
> * Mapování výchozích atributů se aktualizovala.

## <a name="capabilities-supported"></a>Podporované funkce
> [!div class="checklist"]
> * Vytváření uživatelů v G Suite
> * Odebrat uživatele v G Suite, když už nevyžadují přístup
> * Udržování uživatelských atributů synchronizovaných mezi Azure AD a G Suite
> * Zřizování skupin a členství ve skupinách v G Suite
> * [Jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial) do G Suite (doporučeno)

## <a name="prerequisites"></a>Požadavky

Scénář popsaný v tomto kurzu předpokládá, že už máte následující požadavky:

* [Tenant Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Uživatelský účet ve službě Azure AD s [oprávněním](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) ke konfiguraci zřizování (například správce aplikace, správce cloudové aplikace, vlastník aplikace nebo globální správce). 
* [Tenant G Suite](https://gsuite.google.com/pricing.html)
* Uživatelský účet v G Suite s oprávněními správce.

## <a name="step-1-plan-your-provisioning-deployment"></a>Krok 1. Plánování nasazení zřizování
1. Přečtěte si [, jak služba zřizování funguje](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Určete, kdo bude v [oboru pro zřizování](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Určete, jaká data se mají [mapovat mezi Azure AD a G Suite](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-g-suite-to-support-provisioning-with-azure-ad"></a>Krok 2. Konfigurace G Suite pro podporu zřizování s Azure AD

Před konfigurací G Suite pro Automatické zřizování uživatelů pomocí Azure AD budete muset povolit zřizování SCIM v G Suite.

1. Přihlaste se ke [konzole pro správu G Suite](https://admin.google.com/) pomocí účtu správce a pak vyberte **zabezpečení**. Pokud odkaz nevidíte, může být skrytý v nabídce **Další ovládací prvky** v dolní části obrazovky.

    ![Zabezpečení G Suite](./media/google-apps-provisioning-tutorial/gapps-security.png)

2. Na stránce **zabezpečení** vyberte **odkaz rozhraní API**.

    ![Rozhraní API G Suite](./media/google-apps-provisioning-tutorial/gapps-api.png)

3. Vyberte **Povolit přístup přes rozhraní API**.

    ![Rozhraní API G Suite povoleno](./media/google-apps-provisioning-tutorial/gapps-api-enabled.png)

    > [!IMPORTANT]
   > Pro každého uživatele, kterého máte v úmyslu zřídit sadu G Suite, **musí** být jeho uživatelské jméno ve službě Azure AD vázané na vlastní doménu. Například uživatelská jména, která vypadají jako, bob@contoso.onmicrosoft.com nepřijímá sada G Suite. Na druhé straně bob@contoso.com se akceptuje. Stávající doménu uživatele můžete změnit podle pokynů uvedených [tady](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).

4. Jakmile přidáte a ověříte vaše požadované vlastní domény pomocí Azure AD, musíte je znovu ověřit pomocí G Suite. Pokud chcete ověřit domény v G Suite, přečtěte si následující postup:

    a. V [konzole pro správu G Suite](https://admin.google.com/)vyberte **domény**.

    ![Domény G Suite](./media/google-apps-provisioning-tutorial/gapps-domains.png)

    b. Vyberte **Přidat doménu nebo alias domény**.

    ![Sada G Suite – přidat doménu](./media/google-apps-provisioning-tutorial/gapps-add-domain.png)

    c. Vyberte **Přidat další doménu**a pak zadejte název domény, kterou chcete přidat.

    ![G Suite přidat další](./media/google-apps-provisioning-tutorial/gapps-add-another.png)

    d. Vyberte **pokračovat a ověřte vlastnictví domény**. Pak postupujte podle pokynů a ověřte, že název domény vlastníte. Podrobné pokyny, jak ověřit vaši doménu pomocí Google, najdete v tématu [ověření vlastnictví webu](https://support.google.com/webmasters/answer/35179).

    e. Předchozí kroky opakujte pro všechny další domény, které máte v úmyslu přidat do G Suite.

5. Dále určete, který účet správce chcete použít ke správě zřizování uživatelů v G Suite. Přejděte na **role správce**.

    ![Správce G Suite](./media/google-apps-provisioning-tutorial/gapps-admin.png)

6. Pro **roli správce** tohoto účtu upravte **oprávnění** pro tuto roli. Ujistěte se, že jste povolili všechna **oprávnění rozhraní API pro správu** , aby se tento účet mohl použít k zřizování.

    ![Oprávnění správce G Suite](./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png)

## <a name="step-3-add-g-suite-from-the-azure-ad-application-gallery"></a>Krok 3. Přidání G Suite z Galerie aplikací Azure AD

Přidejte G Suite z Galerie aplikací Azure AD a začněte spravovat zřizování ke G Suite. Pokud jste již dříve nastavili G Suite pro jednotné přihlašování, můžete použít stejnou aplikaci. Doporučuje se ale při počátečním testování integrace vytvořit samostatnou aplikaci. Další informace o přidání aplikace z Galerie [najdete tady](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Krok 4. Definujte, kdo bude v oboru pro zřizování. 

Služba zřizování Azure AD umožňuje obor, který se zřídí na základě přiřazení do aplikace, nebo na základě atributů uživatele nebo skupiny. Pokud se rozhodnete určit rozsah, který se zřídí pro vaši aplikaci na základě přiřazení, můžete k přiřazení uživatelů a skupin k aplikaci použít následující [postup](../manage-apps/assign-user-or-group-access-portal.md) . Pokud se rozhodnete obor, který se zřídí výhradně na základě atributů uživatele nebo skupiny, můžete použít filtr oboru, jak je popsáno [zde](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Při přiřazování uživatelů a skupin k G Suite je nutné vybrat jinou roli než **výchozí přístup**. Uživatelé s výchozí rolí přístupu se z zřizování vylučují a v protokolech zřizování se označí jako neefektivně. Pokud je jedinou rolí dostupnou v aplikaci výchozí role přístupu, můžete [aktualizovat manifest aplikace](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) a přidat další role. 

* Začněte malým. Než se pustíte do všech uživatelů, testujte je s malou sadou uživatelů a skupin. Pokud je obor pro zřizování nastavený na přiřazené uživatele a skupiny, můžete to řídit přiřazením jednoho nebo dvou uživatelů nebo skupin k aplikaci. Pokud je obor nastavený na všechny uživatele a skupiny, můžete zadat [Filtr oboru založený na atributech](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-g-suite"></a>Krok 5. Konfigurace automatického zřizování uživatelů na G Suite 

V této části se seznámíte s postupem konfigurace služby zřizování Azure AD k vytváření, aktualizaci a zakázání uživatelů nebo skupin v TestApp na základě přiřazení uživatelů nebo skupin ve službě Azure AD.

> [!NOTE]
> Další informace o koncovém bodu rozhraní API adresářových sad G Suite najdete v tématu věnovaném [rozhraní Directory API](https://developers.google.com/admin-sdk/directory).

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>Konfigurace automatického zřizování uživatelů pro G Suite ve službě Azure AD:

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com). Vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**. Uživatelé se budou muset přihlásit k portal.azure.com a nebudou moct používat aad.portal.azure.com.

    ![Okno podnikových aplikací](./media/google-apps-provisioning-tutorial/enterprise-applications.png)

    ![Okno Všechny aplikace](./media/google-apps-provisioning-tutorial/all-applications.png)

2. V seznamu aplikace vyberte **G Suite**.

    ![Odkaz G Suite v seznamu aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** . klikněte na **Začínáme**.

    ![Karta zřizování](common/provisioning.png)

      ![Okno Začínáme](./media/google-apps-provisioning-tutorial/get-started.png)

4. Nastavte **režim zřizování** na **automaticky**.

    ![Karta zřizování](common/provisioning-automatic.png)

5. V části **přihlašovací údaje správce** klikněte na **autorizovat**. V novém okně prohlížeče budete přesměrováni do dialogového okna pro autorizaci Google.

      ![Autorizace G Suite](./media/google-apps-provisioning-tutorial/authorize-1.png)

6. Potvrďte, že chcete udělit oprávnění služby Azure AD, aby bylo možné provádět změny v tenantovi G Suite. Vyberte **Přijmout**.

     ![Ověřování tenanta G Suite](./media/google-apps-provisioning-tutorial/gapps-auth.png)

7. V Azure Portal klikněte na **Test připojení** a ujistěte se, že se služba Azure AD může připojit k G Suite. Pokud se připojení nepovede, ujistěte se, že váš účet G Suite má oprávnění správce, a zkuste to znovu. Pak zkuste znovu spustit krok **autorizace** .

6. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, které by měly dostávat oznámení o chybách zřizování a zaškrtněte políčko **Odeslat e-mailové oznámení, když dojde k chybě** .

    ![E-mail s oznámením](common/provisioning-notification-email.png)

7. Vyberte **Uložit**.

8. V části **mapování** vyberte možnost **zřídit Azure Active Directory uživatelé**.

9. Zkontrolujte atributy uživatele synchronizované z Azure AD do G Suite v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v G Suite pro operace aktualizace. Pokud se rozhodnete změnit [odpovídající cílový atribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), bude nutné zajistit, aby rozhraní API G Suite podporovalo filtrování uživatelů na základě tohoto atributu. Kliknutím na tlačítko **Uložit** potvrďte změny.

   |Atribut|Typ|
   |---|---|
   |primaryEmail|Řetězec|
   |vzájemné. [Type EQ "správce"]. hodnota|Řetězec|
   |název. rodina|Řetězec|
   |název. křestní jméno|Řetězec|
   |rozpuštěn|Řetězec|
   |externalIds. [Type EQ "vlastní"]. Value|Řetězec|
   |externalIds. [Type EQ "organizace"]. hodnota|Řetězec|
   |Adresa. [Type EQ "Work"]. Country|Řetězec|
   |Adresa. [Type EQ "Work"]. streetAddress|Řetězec|
   |Adresa. [Type EQ "Work"]. region|Řetězec|
   |Adresa. [Type EQ "Work"].|Řetězec|
   |Adresa. [Type EQ "Work"]. postalCode|Řetězec|
   |zpráv. [Type EQ "Work"]. Address|Řetězec|
   |subjekt. [Type EQ "Work"]. oddělení|Řetězec|
   |subjekt. [Type EQ "Work"]. title|Řetězec|
   |phoneNumbers. [Type EQ "Work"]. Value|Řetězec|
   |phoneNumbers. [typ EQ "mobilní"]. hodnota|Řetězec|
   |phoneNumbers. [Type EQ "work_fax"]. hodnota|Řetězec|
   |zpráv. [Type EQ "Work"]. Address|Řetězec|
   |subjekt. [Type EQ "Work"]. oddělení|Řetězec|
   |subjekt. [Type EQ "Work"]. title|Řetězec|
   |phoneNumbers. [Type EQ "Work"]. Value|Řetězec|
   |phoneNumbers. [typ EQ "mobilní"]. hodnota|Řetězec|
   |phoneNumbers. [Type EQ "work_fax"]. hodnota|Řetězec|
   |Adresa. [typ EQ "domů"]. Country|Řetězec|
   |Adresa. [Type EQ "Home"]. formátovaný|Řetězec|
   |Adresa. [typ EQ "Home"]. umístění|Řetězec|
   |Adresa. [Type EQ "domů"]. postalCode|Řetězec|
   |Adresa. [Type EQ "domů"]. region|Řetězec|
   |Adresa. [typ EQ "Home"]. streetAddress|Řetězec|
   |Adresa. [Type EQ "ostatní"]. Country|Řetězec|
   |Adresa. [Type EQ "ostatní"]. formátovaný|Řetězec|
   |Adresa. [Type EQ "other"].|Řetězec|
   |Adresa. [Type EQ "other"]. postalCode|Řetězec|
   |Adresa. [Type EQ "ostatní"]. region|Řetězec|
   |Adresa. [Type EQ "other"]. streetAddress|Řetězec|
   |Adresa. [Type EQ "Work"]. formátovaný|Řetězec|
   |changePasswordAtNextLogin|Řetězec|
   |zpráv. [typ EQ "domů"]. adresa|Řetězec|
   |zpráv. [Type EQ "ostatní"]. adresa|Řetězec|
   |externalIds. [Type EQ "účet"]. hodnota|Řetězec|
   |externalIds. [Type EQ "Custom"]. customType|Řetězec|
   |externalIds. [Type EQ "Customer"]. Value|Řetězec|
   |externalIds. [Type EQ "login_id"]. hodnota|Řetězec|
   |externalIds. [Type EQ "síť"]. hodnota|Řetězec|
   |pohlaví. typ|Řetězec|
   |GeneratedImmutableId|Řetězec|
   |Identifikátor|Řetězec|
   |IMS. [Type EQ "Home"]. Protocol|Řetězec|
   |IMS. [Type EQ "jiný"]. protokol|Řetězec|
   |IMS. [Type EQ "Work"]. Protocol|Řetězec|
   |includeInGlobalAddressList|Řetězec|
   |ipWhitelisted|Řetězec|
   |subjekt. [Type EQ "School"]. costCenter|Řetězec|
   |subjekt. [Type EQ "škola"]. oddělení|Řetězec|
   |subjekt. [Type EQ "škola"]. doména|Řetězec|
   |subjekt. [Type EQ "School"]. fullTimeEquivalent|Řetězec|
   |subjekt. [Type EQ "School"]. Location|Řetězec|
   |subjekt. [Type EQ "School"]. Name|Řetězec|
   |subjekt. [Type EQ "School"]. symbol|Řetězec|
   |subjekt. [Type EQ "škola"]. title|Řetězec|
   |subjekt. [Type EQ "Work"]. costCenter|Řetězec|
   |subjekt. [Type EQ "Work"]. Domain|Řetězec|
   |subjekt. [Type EQ "Work"]. fullTimeEquivalent|Řetězec|
   |subjekt. [Type EQ "Work"]. Location|Řetězec|
   |subjekt. [Type EQ "Work"]. Name|Řetězec|
   |subjekt. [Type EQ "Work"]. symbol|Řetězec|
   |OrgUnitPath|Řetězec|
   |phoneNumbers. [typ EQ "domů"]. hodnota|Řetězec|
   |phoneNumbers. [Type EQ "jiné"]. hodnota|Řetězec|
   |websites. [typ EQ "domů"]. hodnota|Řetězec|
   |websites. [Type EQ "jiné"]. hodnota|Řetězec|
   |websites. [Type EQ "Work"]. Value|Řetězec|
   

10. V části **mapování** vyberte možnost **zřídit Azure Active Directory skupiny**.

11. Zkontrolujte atributy skupiny synchronizované z Azure AD do G Suite v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování skupin v G Suite pro operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.

      |Atribut|Typ|
      |---|---|
      |e-mail|Řetězec|
      |Členové|Řetězec|
      |name|Řetězec|
      |description|Řetězec|

12. Pokud chcete nakonfigurovat filtry oborů, přečtěte si následující pokyny uvedené v [kurzu filtr oboru](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Pokud chcete povolit službu Azure AD Provisioning pro G Suite, změňte **stav zřizování** na **zapnuto** v části **Nastavení** .

    ![Zapnutý stav zřizování](common/provisioning-toggle-on.png)

14. Definujte uživatele nebo skupiny, které chcete zřídit pro sadu G Suite výběrem požadovaných hodnot v **oboru** v části **Nastavení** .

    ![Rozsah zřizování](common/provisioning-scope.png)

15. Až budete připraveni zřídit, klikněte na **Uložit**.

    ![Ukládá se konfigurace zřizování.](common/provisioning-configuration-save.png)

Tato operace spustí počáteční cyklus synchronizace všech uživatelů a skupin definovaných v **oboru** v části **Nastavení** . Počáteční cyklus trvá déle než u dalších cyklů, ke kterým dojde přibližně každých 40 minut, pokud je služba zřizování Azure AD spuštěná.

> [!NOTE]
> Pokud už uživatelé mají pomocí e-mailové adresy uživatele služby Azure AD existující osobní/uživatelský účet, může dojít k nějakému problému, který by se mohl vyřešit pomocí nástroje pro přenos Google před provedením synchronizace adresáře.

## <a name="step-6-monitor-your-deployment"></a>Krok 6. Monitorování nasazení
Jakmile nakonfigurujete zřizování, použijte k monitorování nasazení tyto prostředky:

1. Pomocí [protokolů zřizování](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) určete, kteří uživatelé se úspěšně zřídili nebo neúspěšně nastavili.
2. Podívejte se na [indikátor průběhu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) , kde se zobrazí stav cyklu zřizování a jak se má dokončit.
3. Pokud se zdá, že konfigurace zřizování je ve stavu není v pořádku, bude aplikace přejít do karantény. Další informace o stavech karantény najdete [tady](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Další zdroje

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* [Přečtěte si, jak zkontrolovat protokoly a získat sestavy pro aktivitu zřizování.](../manage-apps/check-status-user-account-provisioning.md)
