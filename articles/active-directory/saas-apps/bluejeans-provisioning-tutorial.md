---
title: 'Kurz: Konfigurace BlueJeans pro Automatické zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: Naučte se konfigurovat Azure Active Directory pro automatické zřízení a zrušení zřízení uživatelských účtů pro BlueJeans.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcb3fe009a6482c8e512899a952694beaed361a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77058973"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Kurz: Konfigurace BlueJeans pro Automatické zřizování uživatelů

Cílem tohoto kurzu je předvést kroky, které je třeba provést v BlueJeans a Azure Active Directory (Azure AD) ke konfiguraci služby Azure AD pro Automatické zřizování a zrušení zřizování uživatelů nebo skupin pro BlueJeans.

> [!NOTE]
> Tento kurz popisuje konektor založený na službě zřizování uživatelů Azure AD. Důležité informace o tom, co tato služba dělá, jak funguje a nejčastější dotazy, najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů pro SaaS aplikací pomocí Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Požadavky

Scénář popsaný v tomto kurzu předpokládá, že už máte následující:

* Tenant Azure AD
* Tenant BlueJeans s plánem [Moje společnost](https://www.BlueJeans.com/pricing) nebo lepší povolený
* Uživatelský účet v BlueJeans s oprávněními správce

> [!NOTE]
> Integrace zřizování Azure AD spoléhá na [rozhraní BlueJeans API](https://BlueJeans.github.io/developer), které je dostupné pro BlueJeans týmy na standardním plánu nebo lepší.

## <a name="adding-bluejeans-from-the-gallery"></a>Přidání BlueJeans z Galerie

Před konfigurací BlueJeans pro Automatické zřizování uživatelů se službou Azure AD je nutné přidat BlueJeans z Galerie aplikací Azure AD do svého seznamu spravovaných aplikací SaaS.

**Pokud chcete přidat BlueJeans z Galerie aplikací Azure AD, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** v levém navigačním panelu vyberte možnost **Azure Active Directory**.

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Vyberte možnost **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, vyberte tlačítko **Nová aplikace** v horní části podokna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **BlueJeans**, na panelu výsledků vyberte **BlueJeans** a pak kliknutím na tlačítko **Přidat** přidejte aplikaci.

    ![BlueJeans v seznamu výsledků](common/search-new-app.png)

## <a name="assigning-users-to-bluejeans"></a>Přiřazování uživatelů k BlueJeans

Azure Active Directory používá koncept nazvaný "přiřazení" k určení uživatelů, kteří mají získat přístup k vybraným aplikacím. V kontextu automatického zřizování uživatelů se synchronizují jenom uživatelé a skupiny, které jsou přiřazené k aplikaci ve službě Azure AD.

Před konfigurací a povolením automatického zřizování uživatelů byste se měli rozhodnout, kteří uživatelé a skupiny ve službě Azure AD potřebují přístup k BlueJeans. Po rozhodnutí můžete přiřadit tyto uživatele nebo skupiny k BlueJeans podle pokynů uvedených tady:

* [Přiřazení uživatele nebo skupiny k podnikové aplikaci](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Důležité tipy pro přiřazení uživatelů k BlueJeans

* Doporučuje se, aby se k BlueJeans k testování automatické konfigurace zřizování uživatelů přiřadil jeden uživatel Azure AD. Další uživatele a skupiny můžete přiřadit později.

* Při přiřazování uživatele k BlueJeans musíte v dialogovém okně přiřazení vybrat jakoukoli platnou roli specifickou pro aplikaci (Pokud je dostupná). Uživatelé s **výchozí rolí přístupu** se z zřizování vylučují.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Konfigurace automatického zřizování uživatelů na BlueJeans

V této části se seznámíte s postupem konfigurace služby zřizování Azure AD k vytváření, aktualizaci a zakázání uživatelů nebo skupin v BlueJeans na základě přiřazení uživatelů nebo skupin ve službě Azure AD.

> [!TIP]
> Můžete se také rozhodnout povolit jednotné přihlašování založené na SAML pro BlueJeans podle pokynů uvedených v [kurzu BlueJeans jednotného přihlašování](bluejeans-tutorial.md). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatickém zřizování uživatelů, i když se tyto dvě funkce navzájem doplňují.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Konfigurace automatického zřizování uživatelů pro BlueJeans ve službě Azure AD:

1. Přihlaste se k [Azure Portal](https://portal.azure.com) a vyberte **podnikové aplikace**, vyberte **všechny aplikace**a pak vyberte **BlueJeans**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **BlueJeans**.

    ![Odkaz BlueJeans v seznamu aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** .

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Nastavte **režim zřizování** na **automaticky**.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/Bluejeans1.png)

5. V části **přihlašovací údaje správce** zadejte **uživatelské jméno správce**a **heslo správce** účtu BlueJeans. Příklady těchto hodnot:

   * V poli **uživatelské jméno správce** naplňte uživatelské jméno účtu správce v tenantovi BlueJeans. Příklad: admin@contoso.com.

   * V poli **heslo správce** vyplňte heslo odpovídající uživatelskému jménu správce.

6. Po vyplnění polí zobrazených v kroku 5 klikněte na **Test připojení** , aby se služba Azure AD mohla připojit k BlueJeans. Pokud se připojení nepovede, ujistěte se, že má váš účet BlueJeans oprávnění správce, a zkuste to znovu.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, které by měly dostávat oznámení o chybách zřizování, a zaškrtněte políčko – **pošle e-mailové oznámení, když dojde k chybě**.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. Klikněte na **Uložit**.

9. V části **mapování** vyberte **synchronizovat Azure Active Directory uživatelé BlueJeans**.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. Zkontrolujte atributy uživatele synchronizované z Azure AD do BlueJeans v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v BlueJeans pro operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Pokud chcete nakonfigurovat filtry oborů, přečtěte si následující pokyny uvedené v [kurzu filtr oboru](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Pokud chcete povolit službu Azure AD Provisioning pro BlueJeans, změňte **stav zřizování** na **zapnuto** v části **Nastavení** .

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. Definujte uživatele nebo skupiny, které chcete zřídit pro BlueJeans, výběrem požadovaných hodnot v **oboru** v části **Nastavení** .

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Až budete připraveni zřídit, klikněte na **Uložit**.

    ![Zřizování BlueJeans](./media/bluejeans-provisioning-tutorial/SaveProvisioning.png)

Tato operace spustí počáteční synchronizaci všech uživatelů nebo skupin definovaných v **oboru** v části **Nastavení** . Počáteční synchronizace trvá déle než další synchronizace, ke kterým dochází přibližně každých 40 minut, pokud je služba zřizování Azure AD spuštěná. V části **Podrobnosti o synchronizaci** můžete sledovat průběh a postupovat podle odkazů na sestavu aktivity zřizování, která popisuje všechny akce prováděné službou zřizování Azure AD v BlueJeans.

Další informace o tom, jak číst protokoly zřizování Azure AD, najdete v tématu [vytváření sestav o automatickém zřizování uživatelských účtů](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Omezení konektoru

* BlueJeans nepovoluje uživatelská jména, která překračují 30 znaků.

## <a name="additional-resources"></a>Další zdroje

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* [Přečtěte si, jak zkontrolovat protokoly a získat sestavy pro aktivitu zřizování.](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->

[1]: ./media/bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
