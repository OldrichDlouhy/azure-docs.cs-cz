---
title: Co je zřizování identit pomocí Azure AD? | Dokumentace Microsoftu
description: Popisuje přehled zřizování identit.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28513c57101af67695d10056b3dc8e6537dcddb2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "76712550"
---
# <a name="what-is-identity-provisioning"></a>Co je zřizování identit?

V současné době se podniky a společnosti stávají více a více než v různých místních i cloudových aplikacích.  Uživatelé vyžadují přístup k aplikacím místně i v cloudu. V rámci těchto různých aplikací je potřeba mít jedinou identitu (místní i Cloud).

Zřizování je proces vytváření objektu na základě určitých podmínek, udržování objektu v aktuálním stavu a odstranění objektu, pokud podmínky již nejsou splněny. Když se například do vaší organizace připojí nový uživatel, tento uživatel se zadává do systému HR.  V tomto okamžiku zřizování může vytvořit odpovídající uživatelský účet v cloudu, ve službě Active Directory a v různých aplikacích, ke kterým uživatel potřebuje mít přístup.  Díky tomu může uživatel začít pracovat a mít přístup k aplikacím a systémům, které potřebují k jednomu dni. 

![zřizování cloudu](media/what-is-provisioning/cloud1.png)

Pokud jde o Azure Active Directory, zřizování se dá rozdělit do následujících klíčových scénářů.  

- **[Zřizování na základě lidských zdrojů](#hr-driven-provisioning)**  
- **[Zřizování aplikací](#app-provisioning)**  
- **[Zřizování adresáře](#directory-provisioning)** 

## <a name="hr-driven-provisioning"></a>Zřizování na základě lidských zdrojů

![zřizování cloudu](media/what-is-provisioning/cloud2.png)

Zřizování z HR do cloudu zahrnuje vytváření objektů (uživatelů, rolí, skupin atd.) na základě informací, které jsou v systému HR.  

Nejběžnějším scénářem je, že když se do vaší společnosti připojí nový zaměstnanec, vstoupí do systému HR.  Jakmile k tomu dojde, budou zřízeny do cloudu.  V tomto případě Azure AD.  Zřizování z HR může pokrývat následující scénáře. 

- Připravují se **noví zaměstnanci** – když se do cloudového HR přidá nový zaměstnanec, automaticky se vytvoří uživatelský účet ve službě Active Directory, Azure Active Directory a volitelně na Office 365 a další aplikace SaaS podporované službou Azure AD, a to s zpětným zápisem e-mailové adresy do cloudového hr.
- **Aktualizace atributů a profilů zaměstnanců** – když se v cloudovém HR aktualizuje záznam zaměstnance (například jeho jméno, název nebo manažer), automaticky se aktualizuje jeho uživatelský účet ve službě Active Directory, Azure Active Directory a volitelně v Office 365 a dalších aplikacích SaaS podporovaných službou Azure AD.
- **Ukončení zaměstnanců** – když se zaměstnanec v cloudovém HR ukončí, je jejich uživatelský účet automaticky zakázaný ve službě Active Directory, Azure Active Directory a volitelně Office 365 a další aplikace SaaS podporované službou Azure AD.
- **Pracovní zařazení zaměstnanců** – Pokud se zaměstnanec znovu přiřadí do CLOUDového hru, jeho starý účet se dá automaticky znovu aktivovat nebo znovu zřídit (v závislosti na vaší preferenci) se službou Active Directory, Azure Active Directory a volitelně Office 365 a dalšími aplikacemi SaaS, které Azure AD podporuje.


## <a name="app-provisioning"></a>Zřizování aplikací

![zřizování cloudu](media/what-is-provisioning/cloud3.png)

V Azure Active Directory (Azure AD) pojem **[zřizování aplikací](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)** označuje automatické vytváření identit uživatelů a rolí v cloudových aplikacích, ke kterým uživatelé potřebují přístup. Kromě vytváření identit uživatelů zahrnuje Automatické zřizování také údržbu a odebírání identit uživatelů při změně stavu nebo rolí. Mezi běžné scénáře patří zřizování uživatelů Azure AD v aplikacích, jako jsou [Dropbox](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial), [Salesforce](https://docs.microsoft.com/azure/active-directory/saas-apps/salesforce-provisioning-tutorial), [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial)a další.

## <a name="directory-provisioning"></a>Zřizování adresáře

![zřizování cloudu](media/what-is-provisioning/cloud4.png)

Místní zřizování zahrnuje zřizování z místních zdrojů (jako je služba Active Directory) do Azure AD.  

Nejběžnějším scénářem je situace, kdy se uživatel ve službě Active Directory (AD) zřídí do služby Azure AD.

To bylo dosaženo Azure AD Connect synchronizace Azure AD Connect zřizování cloudu a Microsoft Identity Manager. 
 
## <a name="next-steps"></a>Další kroky 

- [Co je zřízení cloudu Azure AD Connect?](what-is-cloud-provisioning.md)
- [Instalace zřizování cloudu](how-to-install.md)
