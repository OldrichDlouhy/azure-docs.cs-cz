---
title: Povolení služby profilů uživatelů SharePointu pomocí Azure služba AD DS | Microsoft Docs
description: Informace o tom, jak nakonfigurovat Azure Active Directory Domain Services spravovanou doménu pro podporu synchronizace profilů pro SharePoint Server
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/09/2020
ms.author: iainfou
ms.openlocfilehash: 9a65065a6f3cbc7264a8efb9bcf128b06897aacf
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86220265"
---
# <a name="configure-azure-active-directory-domain-services-to-support-user-profile-synchronization-for-sharepoint-server"></a>Konfigurace Azure Active Directory Domain Services pro podporu synchronizace profilů uživatelů pro server SharePoint

SharePoint Server obsahuje službu pro synchronizaci profilů uživatelů. Tato funkce umožňuje, aby byly profily uživatelů uloženy v centrálním umístění a přístupné napříč několika weby a farmami služby SharePoint. Chcete-li nakonfigurovat službu profilů uživatelů serveru SharePoint, musí být udělena příslušná oprávnění ve spravované doméně služby Azure Active Directory Domain Services (Azure služba AD DS). Další informace najdete v tématu [synchronizace profilů uživatelů na serveru SharePoint](https://technet.microsoft.com/library/hh296982.aspx).

V tomto článku se dozvíte, jak nakonfigurovat službu Azure služba AD DS tak, aby umožňovala službu synchronizace profilů uživatelů serveru SharePoint.

## <a name="before-you-begin"></a>Než začnete

K dokončení tohoto článku potřebujete následující prostředky a oprávnění:

* Musíte mít aktivní předplatné Azure.
    * Pokud nemáte předplatné Azure, [vytvořte účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Tenant Azure Active Directory přidružený k vašemu předplatnému, buď synchronizovaný s místním adresářem, nebo jenom s cloudovým adresářem.
    * V případě potřeby [vytvořte tenanta Azure Active Directory][create-azure-ad-tenant] nebo [přidružte předplatné Azure k vašemu účtu][associate-azure-ad-tenant].
* Ve vašem tenantovi Azure AD je povolená a nakonfigurovaná spravovaná doména Azure Active Directory Domain Services.
    * V případě potřeby dokončete kurz a [vytvořte a nakonfigurujte Azure Active Directory Domain Services spravovanou doménu][create-azure-ad-ds-instance].
* Virtuální počítač pro správu Windows serveru, který je připojený k spravované doméně Azure služba AD DS.
    * V případě potřeby dokončete kurz a [vytvořte virtuální počítač pro správu][tutorial-create-management-vm].
* Uživatelský účet, který je členem skupiny *správců řadičů domény Azure AD* ve vašem TENANTOVI Azure AD.
* Účet služby SharePoint pro službu synchronizace profilů uživatelů.
    * V případě potřeby si přečtěte téma [plánování účtů pro správu a účty služby na serveru SharePoint][sharepoint-service-account].

## <a name="service-accounts-overview"></a>Přehled účtů služeb

Ve spravované doméně existuje skupina zabezpečení s názvem *účty služby AAD DC* jako součást organizační jednotky *uživatelů* (OU). Členové této skupiny zabezpečení mají delegovaná tato oprávnění:

- **Replikace oprávnění ke změnám adresáře** u KOŘENOVÉho DSEu.
- **Replikace oprávnění ke změnám adresáře** v názvovém kontextu *Konfigurace* ( `cn=configuration` kontejneru).

Skupina zabezpečení *účty služby AAD DC* je zároveň členem předdefinované skupiny *Pre-Windows 2000 Compatible Access*.

Po přidání do této skupiny zabezpečení má účet služby pro synchronizační službu profilů uživatelů serveru SharePoint udělená potřebná oprávnění k správnému fungování.

## <a name="enable-support-for-sharepoint-server-user-profile-sync"></a>Povolit podporu pro synchronizaci profilů uživatelů serveru SharePoint

Účet služby pro SharePoint Server potřebuje odpovídající oprávnění k replikaci změn do adresáře a správné fungování synchronizace profilů uživatelů serveru SharePoint. Chcete-li poskytnout tato oprávnění, přidejte účet služby, který se používá pro synchronizaci profilů uživatelů služby SharePoint, do skupiny *účty služby AAD DC* .

Z virtuálního počítače pro správu Azure služba AD DS proveďte následující kroky:

> [!NOTE]
> Chcete-li upravit členství ve skupině ve spravované doméně, musíte být přihlášeni k uživatelskému účtu, který je členem skupiny *Správci AAD řadiče domény* .

1. Z obrazovky Start vyberte **Nástroje pro správu**. Zobrazí se seznam dostupných nástrojů pro správu, které byly nainstalovány v tomto kurzu, aby bylo možné [vytvořit virtuální počítač pro správu][tutorial-create-management-vm].
1. Pokud chcete spravovat členství ve skupině, vyberte **Centrum správy služby Active Directory** ze seznamu nástrojů pro správu.
1. V levém podokně vyberte spravovanou doménu, například *aaddscontoso.com*. Zobrazí se seznam existujících organizačních jednotek a prostředků.
1. Vyberte organizační jednotku **uživatelů** a pak zvolte skupinu zabezpečení *AAD DC Service Accounts* .
1. Vyberte **Členové**a pak zvolte **Přidat...**.
1. Zadejte název účtu služby SharePoint a pak vyberte **OK**. V následujícím příkladu má účet služby SharePoint název *SPAdmin*:

    ![Přidejte účet služby SharePoint do skupiny zabezpečení účty služby AAD DC.](./media/deploy-sp-profile-sync/add-member-to-aad-dc-service-accounts-group.png)

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [udělení oprávnění Active Directory Domain Services pro synchronizaci profilů na serveru SharePoint](https://technet.microsoft.com/library/hh296982.aspx) .

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md

<!-- EXTERNAL LINKS -->
[sharepoint-service-account]: /sharepoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts
