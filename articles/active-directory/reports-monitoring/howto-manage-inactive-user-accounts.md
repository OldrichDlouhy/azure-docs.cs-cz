---
title: Správa neaktivních uživatelských účtů v Azure AD | Microsoft Docs
description: Přečtěte si, jak zjistit a zpracovat uživatelské účty ve službě Azure AD, které se staly zastaralými.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/07/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92f6f32298dcccca4eba08fd25de0504416e5560
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608139"
---
# <a name="how-to-manage-inactive-user-accounts-in-azure-ad"></a>Postupy: Správa neaktivních uživatelských účtů v Azure AD

Ve velkých prostředích nebudou uživatelské účty vždycky odstraněny, když zaměstnanci odejdou z organizace. Jako správce IT chcete zjistit a zpracovat tyto zastaralé uživatelské účty, protože představují bezpečnostní riziko.

Tento článek vysvětluje způsob zpracování zastaralých uživatelských účtů ve službě Azure AD. 

## <a name="what-are-inactive-user-accounts"></a>Co jsou neaktivní uživatelské účty?

Neaktivní účty jsou uživatelské účty, které už členové vaší organizace nevyžadují k získání přístupu k vašim prostředkům. Jeden identifikátor klíče pro neaktivní účty znamená, že se *při* přihlášení k vašemu prostředí zatím nepoužívaly. Vzhledem k tomu, že neaktivní účty jsou svázané s aktivitou přihlašování, můžete použít časové razítko posledního přihlášení, které bylo úspěšně rozpoznáno. 

Výzvou k této metodě je definování toho, co *pro chvíli* znamená v případě vašeho prostředí. Například uživatelé se nemusí k prostředí *během chvilky*přihlašovat, protože jsou na dovolené. Při definování rozdílů pro neaktivní uživatelské účty musíte zvážit všechny oprávněné důvody, proč se přihlašujete k vašemu prostředí. V mnoha organizacích je rozdíl mezi neaktivními uživatelskými účty mezi 90 a 180 dny. 

Poslední úspěšné přihlášení nabízí potenciálním přehledům, které uživatel potřebuje k přístupu k prostředkům.  Může pomáhat s určením, jestli je členství ve skupině nebo aplikace stále potřeba, nebo odebrat. Pro správu externích uživatelů můžete pochopit, jestli je externí uživatel pořád aktivní v rámci tenanta, nebo by se měl vyčistit. 

    
## <a name="how-to-detect-inactive-user-accounts"></a>Zjišťování neaktivních uživatelských účtů

Neaktivní účty zjistíte tak, že vyhodnocujete vlastnost **lastSignInDateTime** zveřejněnou typem prostředku **signInActivity** rozhraní API pro **Microsoft Graph** . Pomocí této vlastnosti můžete implementovat řešení pro následující scénáře:

- **Uživatelé podle jména**: v tomto scénáři vyhledáte konkrétního uživatele podle názvu, který vám umožní vyhodnotit lastSignInDateTime:`https://graph.microsoft.com/beta/users?$filter=startswith(displayName,'markvi')&$select=displayName,signInActivity`

- **Uživatelé podle data**: v tomto scénáři si vyžádáte seznam uživatelů s lastSignInDateTime před zadaným datem:`https://graph.microsoft.com/beta/users?filter=signInActivity/lastSignInDateTime le 2019-06-01T00:00:00Z`






## <a name="what-you-need-to-know"></a>Co je potřeba vědět

V této části jsou uvedeny informace o tom, co potřebujete znát o vlastnosti lastSignInDateTime.

### <a name="how-can-i-access-this-property"></a>Jak se dá získat přístup k této vlastnosti?

Vlastnost **lastSignInDateTime** je vystavena [typem prostředku signInActivity](https://docs.microsoft.com/graph/api/resources/signinactivity?view=graph-rest-beta) [REST API Microsoft Graph](https://docs.microsoft.com/graph/overview?view=graph-rest-beta#whats-in-microsoft-graph).   

### <a name="is-the-lastsignindatetime-property-available-through-the-get-azureaduser-cmdlet"></a>Je k dispozici vlastnost lastSignInDateTime prostřednictvím rutiny Get-AzureAdUser?

Ne.

### <a name="what-edition-of-azure-ad-do-i-need-to-access-the-property"></a>Jakou edici služby Azure AD potřebuji pro přístup k této vlastnosti?

K této vlastnosti můžete přistupovat ve všech edicích služby Azure AD.

### <a name="what-permission-do-i-need-to-read-the-property"></a>Jaká oprávnění potřebuji ke čtení vlastnosti?

Chcete-li tuto vlastnost číst, je třeba udělit následující oprávnění: 

- AuditLogs. Read. All
- Organizace. Read. All  


### <a name="when-does-azure-ad-update-the-property"></a>Kdy služba Azure AD aktualizuje vlastnost?

Každé interaktivní přihlášení, které bylo úspěšné, má za následek aktualizaci základního úložiště dat. Úspěšná přihlášení se obvykle zobrazují v související sestavě přihlášení do 10 minut.
 

### <a name="what-does-a-blank-property-value-mean"></a>Co znamená prázdná hodnota vlastnosti?

Pokud chcete vygenerovat lastSignInDateTime časové razítko, budete potřebovat úspěšné přihlášení. Vzhledem k tomu, že vlastnost lastSignInDateTime je nová funkce, hodnota vlastnosti lastSignInDateTime může být prázdná, pokud:

- Poslední úspěšné přihlášení uživatele proběhlo před vydáním této funkce (1. prosince 2019).
- Ovlivněný uživatelský účet nebyl nikdy použit k úspěšnému přihlášení.

## <a name="next-steps"></a>Další kroky

* [Získání dat pomocí rozhraní API pro generování sestav Azure Active Directory s certifikáty](tutorial-access-api-with-certificates.md)
* [Reference k rozhraní API auditu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Reference k rozhraní API sestav aktivit přihlašování](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
