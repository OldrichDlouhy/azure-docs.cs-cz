---
title: 'Kurz: Konfigurace Zscaler dvou pro Automatické zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: V tomto kurzu se dozvíte, jak nakonfigurovat Azure Active Directory pro automatické zřízení a zrušení zřízení uživatelských účtů, které Zscaler dva.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0a250fcd-6ca1-47c2-a780-7a6278186a69
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 364b106e7c1f01269ac02b0c2851f8824ea0f58c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77062689"
---
# <a name="tutorial-configure-zscaler-two-for-automatic-user-provisioning"></a>Kurz: Konfigurace Zscaler dvou pro Automatické zřizování uživatelů

V tomto kurzu se dozvíte, jak nakonfigurovat Azure Active Directory (Azure AD) k automatickému zřízení a zrušení zřízení uživatelů nebo skupin, které Zscaler dvě.

> [!NOTE]
> Tento kurz popisuje konektor, který je založený na službě zřizování uživatelů Azure AD. Důležité informace o tom, co tato služba dělá a jak funguje, a odpovědi na nejčastější dotazy najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů při SaaS aplikací pomocí Azure Active Directory](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Požadavky

K dokončení kroků popsaných v tomto kurzu budete potřebovat následující:

* Tenanta Azure AD.
* Zscalerého Dvaho tenanta.
* Uživatelský účet v Zscaler dvakrát s oprávněními správce.

> [!NOTE]
> Integrace zřizování Azure AD spoléhá na Zscaler SCIM rozhraní API, které je dostupné pro podnikové účty.

## <a name="add-zscaler-two-from-the-gallery"></a>Přidání Zscaler 2 z Galerie

Před konfigurací Zscaler dvou pro Automatické zřizování uživatelů pomocí Azure AD musíte přidat Zscaler dvě z Galerie aplikací Azure AD do svého seznamu spravovaných aplikací SaaS.

V [Azure Portal](https://portal.azure.com)v levém podokně vyberte **Azure Active Directory**:

![Vyberte Azure Active Directory.](common/select-azuread.png)

Přejít na **podnikové aplikace** a pak vyberte **všechny aplikace**:

![Podnikové aplikace](common/enterprise-applications.png)

Chcete-li přidat aplikaci, vyberte v horní části okna možnost **Nová aplikace** :

![Vybrat novou aplikaci](common/add-new-app.png)

Do vyhledávacího pole zadejte **Zscaler 2**. Ve výsledcích vyberte **Zscaler dvě** a pak vyberte **Přidat**.

![Seznam výsledků](common/search-new-app.png)

## <a name="assign-users-to-zscaler-two"></a>Přiřadit uživatele k Zscaler dvěma

Uživatelé Azure AD musí mít přiřazený přístup k vybraným aplikacím, aby je mohli používat. V kontextu automatického zřizování uživatelů se synchronizují jenom uživatelé nebo skupiny, kteří jsou přiřazeni k aplikaci v Azure AD.

Než nakonfigurujete a povolíte automatické zřizování uživatelů, měli byste rozhodnout, kteří uživatelé a skupiny ve službě Azure AD potřebují přístup k Zscaler dvěma. Až se rozhodnete, že tyto uživatele a skupiny můžete přiřadit Zscaler dvěma postupy v tématu [přiřazení uživatele nebo skupiny k podnikové aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-two"></a>Důležité tipy pro přiřazení uživatelů k Zscaler dvěma

* Doporučujeme, abyste nejdřív přiřadili jediného uživatele Azure AD, který Zscaler dvě k otestování automatické konfigurace zřizování uživatelů. Později můžete přiřadit více uživatelů a skupin.

* Když přiřadíte uživatele Zscaler dvě, musíte v dialogovém okně přiřazení vybrat jakoukoli platnou roli specifickou pro aplikaci (Pokud je dostupná). Uživatelé s **výchozí rolí přístupu** se z zřizování vylučují.

## <a name="set-up-automatic-user-provisioning"></a>Nastavení automatického zřizování uživatelů

V této části se seznámíte s postupem konfigurace služby zřizování Azure AD k vytváření, aktualizaci a zakázání uživatelů a skupin v Zscaler dvou na základě přiřazení uživatelů a skupin ve službě Azure AD.

> [!TIP]
> Pro Zscaler dvě možná budete chtít povolit jednotné přihlašování založené na SAML. Pokud to uděláte, postupujte podle pokynů v [Zscaler dvou jednotných přihlášeních](zscaler-two-tutorial.md). Jednotné přihlašování se dá nakonfigurovat nezávisle na automatickém zřizování uživatelů, ale tyto dvě funkce spolu doplňují.

1. Přihlaste se k [Azure Portal](https://portal.azure.com) a **Vyberte podnikové aplikace** > **všechny aplikace** > **Zscaler dvě**:

    ![Podnikové aplikace](common/enterprise-applications.png)

2. V seznamu aplikace vyberte **Zscaler dvě**:

    ![Seznam aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** :

    ![Zscaler dvou zřizování](./media/zscaler-two-provisioning-tutorial/provisioning-tab.png)

4. Nastavte **režim zřizování** na **automaticky**:

    ![Nastavení režimu zřizování](./media/zscaler-two-provisioning-tutorial/provisioning-credentials.png)

5. V části **přihlašovací údaje správce** zadejte **adresu URL tenanta** a **tajný token** účtu Zscaler, jak je popsáno v dalším kroku.

6. Pokud chcete získat **adresu URL tenanta** a **tajný token**, klikněte **na** > **nastavení ověřování** na portálu Zscaler dva a v části **typ ověřování**vyberte **SAML** :

    ![Zscaler – dvě nastavení ověřování](./media/zscaler-two-provisioning-tutorial/secret-token-1.png)

    Vyberte **Konfigurovat SAML** pro otevření okna **Konfigurovat okno SAML** :

    ![Konfigurovat okno SAML](./media/zscaler-two-provisioning-tutorial/secret-token-2.png)

    Vyberte **Povolit zřizování na základě SCIM** a zkopírujte **základní adresu URL** a **nosný token**a pak nastavení uložte. V Azure Portal vložte **základní adresu URL** do pole **Adresa URL tenanta** a **token nosiče** do pole **tajný token** .

7. Až zadáte hodnoty do polí **Adresa URL tenanta** a **tajný token** , vyberte **Test připojení** a ujistěte se, že se služba Azure AD může připojit k Zscaler dvěma. Pokud se připojení nepovede, ujistěte se, že váš Zscaler účet má oprávnění správce, a zkuste to znovu.

    ![Otestování připojení](./media/zscaler-two-provisioning-tutorial/test-connection.png)

8. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, které by měly dostávat oznámení o chybách zřizování. Vyberte **Odeslat e-mailové oznámení, když dojde k selhání**:

    ![Nastavení e-mailu s oznámením](./media/zscaler-two-provisioning-tutorial/notification.png)

9. Vyberte **Uložit**.

10. V části **mapování** vyberte **synchronizovat Azure Active Directory uživatelé ZscalerTwo**:

    ![Synchronizace uživatelů Azure AD](./media/zscaler-two-provisioning-tutorial/user-mappings.png)

11. Zkontrolujte atributy uživatele synchronizované z Azure AD a Zscaler dvě v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v Zscaler dvou pro operace aktualizace. Vyberte **Uložit** a potvrďte všechny změny.

    ![Mapování atributů](./media/zscaler-two-provisioning-tutorial/user-attribute-mappings.png)

12. V části **mapování** vyberte možnost **synchronizovat Azure Active Directory skupiny do ZscalerTwo**:

    ![Synchronizace skupin Azure AD](./media/zscaler-two-provisioning-tutorial/group-mappings.png)

13. Zkontrolujte atributy skupiny synchronizované z Azure AD a Zscaler dvě v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování skupin v Zscaler dvou pro operace aktualizace. Vyberte **Uložit** a potvrďte všechny změny.

    ![Mapování atributů](./media/zscaler-two-provisioning-tutorial/group-attribute-mappings.png)

14. Pokud chcete nakonfigurovat filtry oborů, přečtěte si pokyny v [kurzu filtr oboru](./../active-directory-saas-scoping-filters.md).

15. Pokud chcete povolit službu Azure AD Provisioning pro Zscaler dvě, změňte **stav zřizování** na **zapnuto** v části **Nastavení** :

    ![Stav zřizování](./media/zscaler-two-provisioning-tutorial/provisioning-status.png)

16. Určete uživatele nebo skupiny, které chcete zřídit pro Zscaler dvakrát výběrem požadovaných **hodnot v části** **Nastavení** :

    ![Hodnoty oboru](./media/zscaler-two-provisioning-tutorial/scoping.png)

17. Až budete připraveni zřídit, vyberte **Uložit**:

    ![Vyberte možnost Uložit.](./media/zscaler-two-provisioning-tutorial/save-provisioning.png)

Tato operace spustí počáteční synchronizaci všech uživatelů a skupin definovaných v části **obor** v části **Nastavení** . Počáteční synchronizace trvá déle než následující synchronizace, ke kterým dochází každých 40 minut, pokud je služba zřizování Azure AD spuštěná. Průběh můžete sledovat v části **Podrobnosti o synchronizaci** . Můžete také sledovat odkazy na sestavu aktivity zřizování, která popisuje všechny akce prováděné službou zřizování Azure AD na Zscaler dvou.

Informace o tom, jak číst protokoly zřizování služby Azure AD, najdete v tématu [vytváření sestav o automatickém zřizování uživatelských účtů](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Další materiály a zdroje informací

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* [Přečtěte si, jak zkontrolovat protokoly a získat sestavy pro aktivitu zřizování.](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-two-provisioning-tutorial/tutorial-general-03.png
