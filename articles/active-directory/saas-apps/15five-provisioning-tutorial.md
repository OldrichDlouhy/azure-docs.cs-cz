---
title: 'Kurz: Konfigurace 15Five pro Automatické zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: Naučte se konfigurovat Azure Active Directory pro automatické zřízení a zrušení zřízení uživatelských účtů pro 15Five.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a276c004-9f71-4efc-8cca-1f615760249f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 5752715b447965c2aad99e170217bc0adce94a78
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85367627"
---
# <a name="tutorial-configure-15five-for-automatic-user-provisioning"></a>Kurz: Konfigurace 15Five pro Automatické zřizování uživatelů

Cílem tohoto kurzu je předvést kroky, které je třeba provést v 15Five a Azure Active Directory (Azure AD) ke konfiguraci služby Azure AD pro Automatické zřizování a zrušení zřizování uživatelů nebo skupin pro [15Five](https://www.15five.com/pricing/). Důležité informace o tom, co tato služba dělá, jak funguje a nejčastější dotazy, najdete v tématu Automatizace zřizování a rušení zřizování uživatelů pro SaaS aplikací pomocí Azure Active Directory.

> [!NOTE]
> Tento konektor je aktuálně ve Public Preview. Další informace o obecných Microsoft Azure podmínek použití pro funkce ve verzi Preview najdete v tématu [doplňujících podmínek použití pro Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)náhledy.


## <a name="capabilities-supported"></a>Podporované funkce
> [!div class="checklist"]
> * Vytváření uživatelů v 15Five
> * Odebrat uživatele v 15Five, když už nevyžadují přístup
> * Udržování uživatelských atributů synchronizovaných mezi Azure AD a 15Five
> * Zřizování skupin a členství ve skupinách v 15Five
> * [Jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/saas-apps/15five-tutorial) k 15Five (doporučeno)

## <a name="prerequisites"></a>Požadavky

Scénář popsaný v tomto kurzu předpokládá, že už máte následující požadavky:

* [Tenant služby Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) .
* Uživatelský účet ve službě Azure AD s [oprávněním](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) ke konfiguraci zřizování (například správce aplikace, správce cloudové aplikace, vlastník aplikace nebo globální správce).
* [Tenant 15Five](https://www.15five.com/pricing/)
* Uživatelský účet v 15Five s oprávněními správce.

## <a name="step-1-plan-your-provisioning-deployment"></a>Krok 1. Plánování nasazení zřizování
1. Přečtěte si [, jak služba zřizování funguje](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Určete, kdo bude v [oboru pro zřizování](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Určete, jaká data se mají [mapovat mezi Azure AD a 15Five](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-15five-to-support-provisioning-with-azure-ad"></a>Krok 2. Konfigurace 15Five pro podporu zřizování pomocí Azure AD

Před konfigurací 15Five pro Automatické zřizování uživatelů pomocí Azure AD budete muset povolit SCIM zřizování na 15Five.

1. Přihlaste se ke [konzole pro správu 15Five](https://my.15five.com/). Přejděte na **funkce > integrace**.

    ![Konzola pro správu 15Five](media/15five-provisioning-tutorial/integration.png)

2.  Klikněte na **SCIM 2,0**.

    ![Konzola pro správu 15Five](media/15five-provisioning-tutorial/image00.png)

3.  Přejděte na **SCIM integration > vygenerujte token OAuth**.

    ![15Five přidat SCIM](media/15five-provisioning-tutorial/image02.png)

4.  Zkopírujte hodnoty **základní adresy URL** a **přístupového tokenu**pro SCIM 2,0. Tato hodnota se zadá do pole **Adresa URL tenanta** a **tajný token** na kartě zřizování vaší aplikace 15Five ve Azure Portal.
    
    ![15Five přidat SCIM](media/15five-provisioning-tutorial/image03.png)

## <a name="step-3-add-15five-from-the-azure-ad-application-gallery"></a>Krok 3. Přidání 15Five z Galerie aplikací Azure AD

Přidejte 15Five z Galerie aplikací Azure AD a začněte spravovat zřizování pro 15Five. Pokud jste dříve nastavili 15Five pro jednotné přihlašování, můžete použít stejnou aplikaci. Doporučuje se ale při počátečním testování integrace vytvořit samostatnou aplikaci. Další informace o přidání aplikace z Galerie [najdete tady](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Krok 4. Definujte, kdo bude v oboru pro zřizování. 

Služba zřizování Azure AD umožňuje obor, který se zřídí na základě přiřazení do aplikace, nebo na základě atributů uživatele nebo skupiny. Pokud se rozhodnete určit rozsah, který se zřídí pro vaši aplikaci na základě přiřazení, můžete k přiřazení uživatelů a skupin k aplikaci použít následující [postup](../manage-apps/assign-user-or-group-access-portal.md) . Pokud se rozhodnete obor, který se zřídí výhradně na základě atributů uživatele nebo skupiny, můžete použít filtr oboru, jak je popsáno [zde](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Při přiřazování uživatelů a skupin k 15Five je nutné vybrat jinou roli než **výchozí přístup**. Uživatelé s výchozí rolí přístupu se z zřizování vylučují a v protokolech zřizování se označí jako neefektivně. Pokud je jedinou rolí dostupnou v aplikaci výchozí role přístupu, můžete [aktualizovat manifest aplikace](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) a přidat další role. 

* Začněte malým. Než se pustíte do všech uživatelů, testujte je s malou sadou uživatelů a skupin. Pokud je obor pro zřizování nastavený na přiřazené uživatele a skupiny, můžete to řídit přiřazením jednoho nebo dvou uživatelů nebo skupin k aplikaci. Pokud je obor nastavený na všechny uživatele a skupiny, můžete zadat [Filtr oboru založený na atributech](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).

## <a name="step-5-configure-automatic-user-provisioning-to-15five"></a>Krok 5. Konfigurace automatického zřizování uživatelů na 15Five 

V této části se seznámíte s postupem konfigurace služby zřizování Azure AD k vytváření, aktualizaci a zakázání uživatelů nebo skupin v 15Five na základě přiřazení uživatelů nebo skupin ve službě Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-15five-in-azure-ad"></a>Konfigurace automatického zřizování uživatelů pro 15Five ve službě Azure AD:

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com). Vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **15Five**.

    ![Odkaz 15Five v seznamu aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** .

    ![Karta zřizování](common/provisioning.png)

4. Nastavte **režim zřizování** na **automaticky**.

    ![Karta zřizování](common/provisioning-automatic.png)

5.  V části přihlašovací údaje správce zadejte **základní adresu URL SCIM 2,0 a hodnoty přístupového tokenu** načtené dříve v polích **Adresa URL tenanta** a **tajného tokenu** . Klikněte na **Test připojení** a ujistěte se, že se služba Azure AD může připojit k 15Five. Pokud se připojení nepovede, ujistěte se, že má váš účet 15Five oprávnění správce, a zkuste to znovu.

    ![Adresa URL tenanta + token](common/provisioning-testconnection-tenanturltoken.png)

6. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, které by měly dostávat oznámení o chybách zřizování, a zaškrtněte políčko – **pošle e-mailové oznámení, když dojde k chybě**.

    ![E-mail s oznámením](common/provisioning-notification-email.png)

7. Klikněte na **Uložit**.

8. V části **mapování** vyberte **synchronizovat Azure Active Directory uživatelé 15Five**.

9. Zkontrolujte atributy uživatele synchronizované z Azure AD do 15Five v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v 15Five pro operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.


   |Atribut|Typ|
   |---|---|
   |aktivně|Logická hodnota|
   |title|Řetězec|
   |e-maily [typ EQ "Work"]. Value|Řetězec|
   |userName|Řetězec|
   |název. křestní jméno|Řetězec|
   |název. rodina|Řetězec|
   |externalId|Řetězec|
   |urn: IETF: parametry: SCIM: schémata: rozšíření: Enterprise: 2.0: User: Manager|Referenční informace|
   |urn: IETF: parametry: SCIM: schémata: rozšíření: Enterprise: 2.0: uživatel: employeeNumber|Řetězec|
   |urn: IETF: parametry: SCIM: schémata: rozšíření: 15Five: 2.0: uživatel: umístění|Řetězec|
   |urn: IETF: parametry: SCIM: schémata: rozšíření: 15Five: 2.0: uživatel: startDate|Řetězec|

10. V části **mapování** vyberte **synchronizovat Azure Active Directory skupiny do 15Five**.

11. Zkontrolujte atributy skupiny synchronizované z Azure AD do 15Five v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování skupin v 15Five pro operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.

      |Atribut|Typ|
      |---|---|
      |externalId|Řetězec|
      |displayName|Řetězec|
      |členy|Referenční informace|

12. Pokud chcete nakonfigurovat filtry oborů, přečtěte si následující pokyny uvedené v [kurzu filtr oboru](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Pokud chcete povolit službu Azure AD Provisioning pro 15Five, změňte **stav zřizování** na **zapnuto** v části **Nastavení** .

    ![Zapnutý stav zřizování](common/provisioning-toggle-on.png)

14. Definujte uživatele nebo skupiny, které chcete zřídit pro 15Five, výběrem požadovaných hodnot v **oboru** v části **Nastavení** .

    ![Rozsah zřizování](common/provisioning-scope.png)

15. Až budete připraveni zřídit, klikněte na **Uložit**.

    ![Ukládá se konfigurace zřizování.](common/provisioning-configuration-save.png)

    Tato operace spustí počáteční synchronizaci všech uživatelů nebo skupin definovaných v **oboru** v části **Nastavení** . Počáteční synchronizace trvá déle než další synchronizace, ke kterým dochází přibližně každých 40 minut, pokud je služba zřizování Azure AD spuštěná.

## <a name="step-6-monitor-your-deployment"></a>Krok 6. Monitorování nasazení
Jakmile nakonfigurujete zřizování, použijte k monitorování nasazení tyto prostředky:

1. Pomocí [protokolů zřizování](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) určete, kteří uživatelé se úspěšně zřídili nebo neúspěšně nastavili.
2. Podívejte se na [indikátor průběhu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) , kde se zobrazí stav cyklu zřizování a jak se má dokončit.
3. Pokud se zdá, že konfigurace zřizování je ve stavu není v pořádku, bude aplikace přejít do karantény. Další informace o stavech karantény najdete [tady](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  
    
## <a name="connector-limitations"></a>Omezení konektoru

* 15Five nepodporuje pro uživatele obnovitelné odstranění.

## <a name="change-log"></a>Protokol změn

* 06/16/2020 – Přidali jsme podporu pro atribut Enterprise Extension "Manager" a vlastní atributy "umístění" a "datum zahájení" pro uživatele.

## <a name="additional-resources"></a>Další zdroje

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* Přečtěte si, [Jak zkontrolovat protokoly a získat sestavy o aktivitě zřizování](../app-provisioning/check-status-user-account-provisioning.md).
