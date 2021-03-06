---
title: 'Kurz: Konfigurace automatického zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: Naučte se, jak nakonfigurovat Azure Active Directory pro automatické zřízení a zrušení zřízení uživatelských účtů pro dotazování na napájení.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 178ca5b3-4155-43ab-84f5-650d25c5a74a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2019
ms.author: Zhchia
ms.openlocfilehash: fabd8a1953bedf6c3db6da443903a6dbd965b01e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77060928"
---
# <a name="tutorial-configure-robin-for-automatic-user-provisioning"></a>Kurz: Konfigurace dotazování pro Automatické zřizování uživatelů

Cílem tohoto kurzu je Ukázat kroky, které je třeba provést v dotazech a Azure Active Directory (Azure AD) ke konfiguraci služby Azure AD pro Automatické zřizování a zrušení zřizování uživatelů a skupin pro dotazování.

> [!NOTE]
> Tento kurz popisuje konektor založený na službě zřizování uživatelů Azure AD. Důležité informace o tom, co tato služba dělá, jak funguje a nejčastější dotazy, najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů pro SaaS aplikací pomocí Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Tento konektor je aktuálně ve Public Preview. Další informace o obecných Microsoft Azure podmínek použití pro funkce ve verzi Preview najdete v tématu [doplňujících podmínek použití pro Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)náhledy.

## <a name="prerequisites"></a>Požadavky

Scénář popsaný v tomto kurzu předpokládá, že už máte následující požadavky:

* Tenant Azure AD
* [Tenant pro dotazování](https://robinpowered.com/pricing/)
* Uživatelský účet v dotazování s oprávněními správce.

## <a name="assigning-users-to-robin"></a>Přiřazení uživatelů k dotazování

Azure Active Directory používá koncept nazvaný *přiřazení* k určení uživatelů, kteří mají získat přístup k vybraným aplikacím. V kontextu automatického zřizování uživatelů se synchronizují jenom uživatelé a skupiny, které jsou přiřazené k aplikaci v Azure AD.

Před konfigurací a povolením automatického zřizování uživatelů byste se měli rozhodnout, kteří uživatelé a skupiny ve službě Azure AD potřebují přístup k dotazování. Po rozhodnutí můžete přiřadit tyto uživatele a skupiny do podrobného dotazování podle pokynů uvedených tady:
* [Přiřazení uživatele nebo skupiny k podnikové aplikaci](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-robin"></a>Důležité tipy pro přiřazení uživatelů k dotazování

* Doporučuje se, aby se k dotazování na automatickou konfiguraci zřizování uživatelů přiřadil jeden uživatel Azure AD. Další uživatele a skupiny můžete přiřadit později.

* Při přiřazování uživatele k dotazování musíte v dialogovém okně přiřazení vybrat jakoukoli platnou roli specifickou pro aplikaci (Pokud je dostupná). Uživatelé s **výchozí rolí přístupu** se z zřizování vylučují.

## <a name="set-up-robin-for-provisioning"></a>Nastavení podrobného dotazování pro zřizování

1. Přihlaste se ke [konzole pro správu pro dotazování](https://dashboard.robinpowered.com/login). Přejděte ke **správě > integrace > SCIM > spravovat**.

    ![Konzola pro správu s dotazem](media/robin-provisioning-tutorial/robin-admin.png)

2.  Vygenerujte nový token organizace. Pokud tento token ztratíte, můžete vždy vytvořit nové, aniž by to ovlivnilo stávající uživatele.

    ![dotazování na technologii Add SCIM](media/robin-provisioning-tutorial/robin-token.png)

3.  Zkopírujte **token ověřování SCIM**. Tato hodnota se zadá do pole token tajného klíče na kartě zřizování vaší aplikace pro dotazování v Azure Portal.



## <a name="add-robin-from-the-gallery"></a>Přidat dotaz z Galerie

Před konfigurací dotazování pro Automatické zřizování uživatelů pomocí Azure AD musíte do svého seznamu spravovaných aplikací pro SaaS přidat dotaz z Galerie aplikací Azure AD.

**Pokud chcete přidat dotaz z Galerie aplikací Azure AD, proveďte následující kroky:**

1. V **[Azure Portal](https://portal.azure.com)** v levém navigačním panelu vyberte možnost **Azure Active Directory**.

    ![Tlačítko Azure Active Directory](common/select-azuread.png)

2. Vyberte možnost **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, vyberte tlačítko **Nová aplikace** v horní části podokna.

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte **dotazování**, v panelu výsledků vyberte **dotazování** a potom kliknutím na tlačítko **Přidat** přidejte aplikaci.

    ![Dotazování v seznamu výsledků](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-robin"></a>Konfigurace automatického zřizování uživatelů pro dotazování 

V této části se seznámíte s postupem konfigurace služby zřizování Azure AD k vytváření, aktualizaci a zakázání uživatelů a skupin v rámci dotazování na základě přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Můžete se také rozhodnout povolit pro dotazování jednotné přihlašování založené na SAML podle pokynů uvedených v [kurzu jednotného přihlašování pro dotazování](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatickém zřizování uživatelů, i když se tyto dvě funkce vzájemně přidávají.

### <a name="to-configure-automatic-user-provisioning-for-robin-in-azure-ad"></a>Konfigurace automatického zřizování uživatelů pro dotazování ve službě Azure AD:

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com). Vyberte **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte možnost **Robin**.

    ![Odkaz na technologii pro dotazování v seznamu aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** .

    ![Karta zřizování](common/provisioning.png)

4. Nastavte **režim zřizování** na **automaticky**.

    ![Karta zřizování](common/provisioning-automatic.png)

5. V části **přihlašovací údaje správce** zadejte `https://api.robinpowered.com/v1.0/scim-2` **adresu URL tenanta**. Zadejte hodnotu **SCIM tokenu ověřování** získanou dříve v **tajném tokenu**. Klikněte na **Test připojení** a ujistěte se, že se Azure AD může připojit k dotazování. Pokud se připojení nepovede, ujistěte se, že váš účet pro dotazování má oprávnění správce, a zkuste to znovu.

    ![Adresa URL tenanta + token](common/provisioning-testconnection-tenanturltoken.png)

6. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, které by měly dostávat oznámení o chybách zřizování, a zaškrtněte políčko – **pošle e-mailové oznámení, když dojde k chybě**.

    ![E-mail s oznámením](common/provisioning-notification-email.png)

7. Klikněte na **Uložit**.

8. V části **mapování** vyberte **synchronizovat Azure Active Directory uživatelů k dotazování**.

    ![Uživatelsky řízené mapování na technologii](media/robin-provisioning-tutorial/robin-user-mapping.png)

9. Zkontrolujte atributy uživatelů synchronizované z Azure AD s dotazování v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v dotazech na operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.

    ![Uživatelské atributy s dotazem na technologii](media/robin-provisioning-tutorial/robin-user-attribute-mapping.png)

10. V části **mapování** vyberte možnost **synchronizovat Azure Active Directory skupiny a dotazování**.

    ![Mapování skupin napájení s dotazem](media/robin-provisioning-tutorial/robin-group-mapping.png)

11. Zkontrolujte atributy skupiny, které jsou synchronizované z Azure AD, s dotazování v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování skupin v dotazech na operace aktualizace. Kliknutím na tlačítko **Uložit** potvrďte změny.

    ![Atributy skupiny napájení s dotazem](media/robin-provisioning-tutorial/robin-group-attribute-mapping.png)

12. Pokud chcete nakonfigurovat filtry oborů, přečtěte si následující pokyny uvedené v [kurzu filtr oboru](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Pokud chcete povolit službu Azure AD Provisioning pro dotazování, změňte **stav zřizování** na **zapnuto** v části **Nastavení** .

    ![Zapnutý stav zřizování](common/provisioning-toggle-on.png)

14. V části **Nastavení** definujte uživatele nebo skupiny, které chcete zřídit pro dotazování, a vyberte požadované hodnoty v **oboru** .

    ![Rozsah zřizování](common/provisioning-scope.png)

15. Až budete připraveni zřídit, klikněte na **Uložit**.

    ![Ukládá se konfigurace zřizování.](common/provisioning-configuration-save.png)

Tato operace spustí počáteční synchronizaci všech uživatelů nebo skupin definovaných v **oboru** v části **Nastavení** . Počáteční synchronizace trvá déle než další synchronizace, ke kterým dochází přibližně každých 40 minut, pokud je služba zřizování Azure AD spuštěná. Část **Podrobnosti o synchronizaci** můžete použít ke sledování průběhu a následného odkazu na sestavu aktivity zřizování, která popisuje všechny akce prováděné službou zřizování Azure AD při dotazování.

Další informace o tom, jak číst protokoly zřizování Azure AD, najdete v tématu [vytváření sestav o automatickém zřizování uživatelských účtů](../app-provisioning/check-status-user-account-provisioning.md).



## <a name="additional-resources"></a>Další zdroje

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* [Přečtěte si, jak zkontrolovat protokoly a získat sestavy pro aktivitu zřizování.](../app-provisioning/check-status-user-account-provisioning.md)

