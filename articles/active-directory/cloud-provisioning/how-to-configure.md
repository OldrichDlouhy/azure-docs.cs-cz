---
title: Konfigurace nového agenta Azure AD Connect zřízení cloudu
description: Tento článek popisuje, jak nainstalovat cloudové zřizování.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50f02ea42bb792320da6e2523b733f09afd412a0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85360958"
---
# <a name="create-a-new-configuration-for-azure-ad-connect-cloud-based-provisioning"></a>Vytvoření nové konfigurace pro Azure AD Connect cloudové zřizování

Po nainstalování agenta se budete muset přihlásit do Azure Portal a nakonfigurovat Azure Active Directory (Azure AD) Connect Cloud Provisioning. Chcete-li povolit agenta, postupujte podle těchto kroků.

## <a name="configure-provisioning"></a>Konfigurace zřizování
Pokud chcete nakonfigurovat zřizování, postupujte podle těchto kroků.

1.  V Azure Portal vyberte **Azure Active Directory**.
1.  Vyberte **Azure AD Connect**.
1.  Vyberte **Spravovat zřizování (Preview)**.

    ![Spravovat zřizování (Preview)](media/how-to-configure/manage1.png)

1.  Vyberte **Nová konfigurace**.
1.  Na obrazovce konfigurace se předběžně vyplní místní doména.
1.  Zadejte **e-mailové oznámení**. Tento e-mail se upozorní, když zřizování není v pořádku.
1.  Přesuňte selektor, který chcete **Povolit**, a vyberte **Uložit**.

    ![Zřizování Azure AD (Preview)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Zřizování oboru pro konkrétní uživatele a skupiny
Můžete určit, že má agent synchronizovat konkrétní uživatele a skupiny pomocí místních skupin služby Active Directory nebo organizačních jednotek. V rámci konfigurace nemůžete konfigurovat skupiny a organizační jednotky. 

1.  V Azure Portal vyberte **Azure Active Directory**.
1.  Vyberte **Azure AD Connect**.
1.  Vyberte **Spravovat zřizování (Preview)**.
1.  V části **Konfigurace**vyberte svou konfiguraci.

    ![Konfigurační oddíl](media/how-to-configure/scope1.png)

1.  V části **Konfigurovat**vyberte možnost **Všichni uživatelé** a změňte rozsah pravidla konfigurace.

    ![Možnost všichni uživatelé](media/how-to-configure/scope2.png)

1. Napravo můžete změnit obor tak, aby zahrnoval pouze skupiny zabezpečení. Zadejte rozlišující název skupiny a vyberte **Přidat**.

    ![Vybraná možnost skupin zabezpečení](media/how-to-configure/scope3.png)

1.  Můžete také změnit obor tak, aby zahrnoval pouze konkrétní organizační jednotky. Vyberte **Hotovo** a **Uložit**.  
2.  Po změně oboru byste měli [restartovat zřizování](#restart-provisioning) a zahájit okamžitou synchronizaci změn.

    ![Možnost vybraných organizačních jednotek](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Restartovat zřizování 
Pokud nechcete čekat na další naplánované spuštění, aktivujte zřizování spuštěním pomocí tlačítka pro **restartování** . 
1.  V Azure Portal vyberte **Azure Active Directory**.
1.  Vyberte **Azure AD Connect**.
1.  Vyberte **Spravovat zřizování (Preview)**.
1.  V části **Konfigurace**vyberte svou konfiguraci.

    ![Výběr konfigurace pro restartování zřizování](media/how-to-configure/scope1.png)

1.  V horní části vyberte **restartovat zřizování**.

## <a name="remove-a-configuration"></a>Odebrat konfiguraci
Pokud chcete konfiguraci odstranit, postupujte podle těchto kroků.

1.  V Azure Portal vyberte **Azure Active Directory**.
1.  Vyberte **Azure AD Connect**.
1.  Vyberte **Spravovat zřizování (Preview)**.
1.  V části **Konfigurace**vyberte svou konfiguraci.

    ![Výběr konfigurace pro odebrání konfigurace](media/how-to-configure/scope1.png)

1.  V horní části obrazovky konfigurace vyberte **Odstranit**.

    ![Tlačítko Odstranit](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Před odstraněním konfigurace se nejedná o žádné potvrzení. Ujistěte se, že se jedná o akci, kterou chcete provést před výběrem možnosti **Odstranit**.


## <a name="next-steps"></a>Další kroky 

- [Co je zřizování?](what-is-provisioning.md)
- [Co je zřízení cloudu Azure AD Connect?](what-is-cloud-provisioning.md)
