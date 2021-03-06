---
title: 'Kurz: Konfigurace základu OnDemand pro Automatické zřizování uživatelů pomocí Azure Active Directory | Microsoft Docs'
description: Naučte se, jak nakonfigurovat Azure Active Directory pro Automatické zřizování a rušení uživatelských účtů na základním kamenem OnDemand.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
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
ms.openlocfilehash: b6aaed8d56bb254e5a6385c770f97b5238a5747b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83647434"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Kurz: Konfigurace základu OnDemand pro Automatické zřizování uživatelů

V tomto kurzu se dozvíte, jak postupovat v kamenech OnDemand a Azure Active Directory (Azure AD) ke konfiguraci služby Azure AD, aby automaticky zřídila a zrušila zřizování uživatelů nebo skupin na základ OnDemand.

> [!NOTE]
> Tento kurz popisuje konektor, který je založený na službě zřizování uživatelů Azure AD. Informace o tom, jak tato služba funguje, jak funguje a nejčastější dotazy, najdete v tématu [Automatizace zřizování a rušení zřizování uživatelů pro aplikace typu software jako služba (SaaS) s Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Požadavky

Scénář popsaný v tomto kurzu předpokládá, že máte následující:

* Tenanta Azure AD.
* Tenant OnDemand.
* Uživatelský účet v obkameni OnDemand s oprávněními správce.

> [!NOTE]
> Integrace zřizování služby Azure AD spoléhá na [webovou službu OnDemand kamenů](https://www.cornerstoneondemand.com/). Tato služba je k dispozici pro OnDemand OnDemand Teams.

## <a name="add-cornerstone-ondemand-from-the-azure-marketplace"></a>Přidat základní kámen OnDemand z Azure Marketplace

Předtím, než nakonfigurujete základ pro Automatické zřizování uživatelů pomocí Azure AD, přidejte do seznamu spravovaných aplikací SaaS základ OnDemand na webu Marketplace.

Pokud chcete přidat nadberné rozhraní OnDemand z webu Marketplace, postupujte podle těchto kroků.

1. V [Azure Portal](https://portal.azure.com)v navigačním podokně na levé straně vyberte **Azure Active Directory**.

    ![Ikona Azure Active Directory](common/select-azuread.png)

2. Vyberte možnost **podnikové aplikace**a pak vyberte **všechny aplikace**.

    ![Okno podnikové aplikace](common/enterprise-applications.png)

3. Chcete-li přidat novou aplikaci, vyberte v horní části dialogového okna možnost **Nová aplikace** .

    ![Tlačítko Nová aplikace](common/add-new-app.png)

4. Do vyhledávacího pole zadejte základní text **OnDemand** a z panelu výsledek vyberte základní možnost **OnDemand** . Chcete-li přidat aplikaci, vyberte možnost **Přidat**.

    ![OnDemandský základ – v seznamu výsledků](common/search-new-app.png)

## <a name="assign-users-to-cornerstone-ondemand"></a>Přiřazení uživatelů k základu OnDemand

Azure Active Directory používá koncept nazvaný *přiřazení* k určení uživatelů, kteří mají získat přístup k vybraným aplikacím. V kontextu automatického zřizování uživatelů se synchronizují jenom uživatelé nebo skupiny, které jsou přiřazené k aplikaci v Azure AD.

Než nakonfigurujete a povolíte automatické zřizování uživatelů, rozhodněte se, kteří uživatelé nebo skupiny ve službě Azure AD potřebují přístup k získání kamene OnDemand. Pokud chcete přiřadit tyto uživatele nebo skupiny k OnDemand OnDemand, postupujte podle pokynů v tématu [přiřazení uživatele nebo skupiny k podnikové aplikaci](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Důležité tipy pro přiřazení uživatelů k základu OnDemand

* Pro otestování automatické konfigurace zřizování uživatelů Doporučujeme, abyste jednomu uživateli služby Azure AD přiřadili OnDemand OnDemand. Později můžete přiřadit další uživatele nebo skupiny.

* Když přiřadíte uživatele k obmoci OnDemand OnDemand, vyberte libovolnou platnou roli specifickou pro aplikaci, pokud je k dispozici v dialogovém okně přiřazení. Uživatelé s **výchozí rolí přístupu** se z zřizování vylučují.

## <a name="configure-automatic-user-provisioning-to-cornerstone-ondemand"></a>Konfigurace automatického zřizování uživatelů u základů OnDemand

Tato část vás provede postupem konfigurace služby zřizování Azure AD. Slouží k vytváření, aktualizaci a zakázání uživatelů nebo skupin v základní nabídce OnDemand na základě přiřazení uživatelů nebo skupin ve službě Azure AD.

Pokud chcete nakonfigurovat automatické zřizování uživatelů pro OnDemand OnDemand ve službě Azure AD, postupujte podle těchto kroků.

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com). Vyberte možnost **podnikové aplikace**  >  **všechny aplikace**v rámci  >  **OnDemand**.

    ![Okno podnikových aplikací](common/enterprise-applications.png)

2. V seznamu aplikace vyberte možnost **základ OnDemand**.

    ![Odkaz na OnDemand OnDemand v seznamu aplikací](common/all-applications.png)

3. Vyberte kartu **zřizování** .

    ![Zřizování OnDemand OnDemand v základu](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Nastavte **režim zřizování** na **automaticky**.

    ![Režim zřizování OnDemand OnDemand na více kamenů](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. V části **přihlašovací údaje správce** zadejte uživatelské jméno správce, heslo správce a doménu účtu OnDemand v rámci svého nadvýrobního prvku:

    * V poli **uživatelské jméno správce** vyplňte v části doména nebo uživatelské jméno účtu správce v tenantovi OnDemand. Příkladem je Contoso\admin.

    * Do pole **heslo správce** zadejte heslo, které odpovídá uživatelskému jménu správce.

    * V poli **doména** vyplňte adresu URL webové služby pro OnDemand tenant. Například služba je umístěna na adrese `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx` a pro společnost Contoso je doména `https://ws-contoso.csod.com/feed30/clientdataservice.asmx` . Další informace o tom, jak načíst adresu URL webové služby, najdete v [tomto dokumentu PDF](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Po vyplnění polí zobrazených v kroku 5 vyberte **Test připojení** , abyste se ujistili, že se služba Azure AD může připojit k základnímu základu OnDemand. Pokud se připojení nepovede, ujistěte se, že má váš účet OnDemand na základech oprávnění správce, a zkuste to znovu.

    ![Připojení k OnDemand test se základem](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. V poli **e-mail s oznámením** zadejte e-mailovou adresu osoby nebo skupiny, ve které se budou zobrazovat oznámení o chybách zřizování. Zaškrtněte políčko **Odeslat e-mailové oznámení, když dojde k chybě** .

    ![E-mail s oznámením nadzákladního základu](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Vyberte **Uložit**.

9. V části **mapování** vyberte možnost **synchronizovat Azure Active Directory uživatele na základě OnDemand OnDemand**.

    ![Synchronizace základů OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Zkontrolujte atributy uživatele, které se synchronizují z Azure AD až do základu OnDemand v oddílu **mapování atributů** . Atributy vybrané jako **odpovídající** vlastnosti se používají ke spárování uživatelských účtů v rámci OnDemand OnDemand pro operace aktualizace. Chcete-li uložit všechny změny, vyberte možnost **Uložit**.

    ![Mapování atributů na základ OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Pokud chcete nakonfigurovat filtry oborů, postupujte podle pokynů v [kurzu filtru oborů](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Pokud chcete povolit službu zřizování Azure AD pro základní platformu OnDemand, v části **Nastavení** změňte **stav zřizování** na **zapnuto**.

    ![Stav zřizování OnDemand OnDemand na více kamenů](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Definujte uživatele nebo skupiny, které chcete zřídit pro základní přístup k OnDemand. V části **Nastavení** vyberte hodnoty, které chcete v **oboru**.

    ![Obor s OnDemandm základem OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Až budete připraveni zřídit, vyberte **Uložit**.

    ![Úspora – OnDemand – úspora](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Tato operace spustí počáteční synchronizaci všech uživatelů nebo skupin definovaných v **oboru** v části **Nastavení** . Počáteční synchronizace trvá déle než pozdější synchronizace. Vyskytují se přibližně každých 40 minut, pokud se služba zřizování Azure AD spouští. 

V části **Podrobnosti o synchronizaci** můžete sledovat průběh a postupovat podle odkazů na sestavu aktivity zřizování. Tato sestava popisuje všechny akce, které provádí služba zřizování služby Azure AD, na základě základu OnDemand.

Informace o tom, jak číst protokoly zřizování služby Azure AD, najdete v tématu [vytváření sestav o automatickém zřizování uživatelských účtů](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Omezení konektoru

Atribut **pozice** OnDemand nadmnožiny očekává hodnotu odpovídající rolím na portálu OnDemand na základě základů. Seznam platných hodnot **pozice** získáte tak, že přejdete na **upravit uživatelský záznam > organizační struktura > pozice** na portálu OnDemand OnDemand.

![Provisioning – upravit záznam uživatele v základním základu OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png)

![Pozice zřizování OnDemand na základech](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png)

![Seznam pozic pro zřizování základů OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Další zdroje

* [Správa zřizování uživatelských účtů pro podnikové aplikace](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Další kroky

* [Přečtěte si, jak zkontrolovat protokoly a získat sestavy pro aktivitu zřizování.](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
