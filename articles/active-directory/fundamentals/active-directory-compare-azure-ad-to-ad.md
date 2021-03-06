---
title: Porovnat službu Active Directory s Azure Active Directory
description: Tento dokument porovnává Active Directory Domain Services (přidá) do Azure Active Directory (AD). Popisuje klíčové koncepty v řešeních identit a vysvětluje, jak se liší nebo které je podobné.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 02/26/2020
ms.author: martinco
ms.openlocfilehash: 5075ae57df6a7306f0c860690931c846e52c2a89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "78926888"
---
# <a name="compare-active-directory-to-azure-active-directory"></a>Porovnat službu Active Directory s Azure Active Directory

Azure Active Directory je další vývoj řešení pro správu identit a přístupů pro Cloud. Společnost Microsoft představila Active Directory Domain Services ve Windows 2000, aby organizacím umožnila spravovat více místních komponent a systémů infrastruktury s použitím jediné identity na uživatele.

Azure AD tento přístup přináší na další úroveň tím, že poskytuje organizacím řešení identity jako služby (IDaaS) pro všechny své aplikace v cloudu i v místním prostředí.

Většina správců IT je obeznámená s Active Directory Domain Services koncepty. Následující tabulka popisuje rozdíly a podobnosti mezi koncepty a Azure Active Directory služby Active Directory.

|Koncepce|Služba Active Directory (AD)|Azure Active Directory |
|:-|:-|:-|
|**Uživatelé**|||
|Zřizování: uživatelé | Organizace vytváří interní uživatele ručně nebo používá integrovaný nebo automatizovaný systém zřizování, jako je například Microsoft Identity Manager, pro integraci se systémem HR.|Stávající organizace služby AD používají [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) k synchronizaci identit do cloudu.</br> Azure AD přidává podporu pro automatické vytváření uživatelů ze [systémů cloudového HR](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial). </br>Azure AD může zřídit identity v aplikacích SaaS s [povoleným SCIM](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups) , aby automaticky poskytovaly aplikace s potřebnými podrobnostmi pro povolení přístupu pro uživatele. |
|Zřizování: externí identity| Organizace vytváří externí uživatele ručně jako běžné uživatele ve vyhrazené externí doménové struktuře služby AD, což má za následek režijní náklady na správu životního cyklu externích identit (uživatelů typu Host).| Azure AD poskytuje speciální třídu identity pro podporu externích identit. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) bude spravovat odkaz na identitu externího uživatele, aby se ujistili, že jsou platné. |
| Správa nároků a skupin| Správci provádějí uživatelské členy skupin. Vlastníci aplikací a prostředků pak poskytnou skupinám přístup k aplikacím nebo prostředkům.| [Skupiny](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) jsou také dostupné ve službě Azure AD a správci můžou k udělení oprávnění k prostředkům použít taky skupiny. Ve službě Azure AD můžou správci přiřadit členství do skupin ručně nebo použít dotaz k dynamickému zahrnutí uživatelů do skupiny. </br> Správci můžou použít [správu nároků](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) ve službě Azure AD, aby měli uživatelé přístup ke kolekci aplikací a prostředků pomocí pracovních postupů a v případě potřeby i podle časových kritérií. |
| Správa správců|Organizace budou používat kombinaci domén, organizačních jednotek a skupin ve službě AD k delegování oprávnění správce ke správě adresáře a prostředků, které ovládá.| Azure AD poskytuje [předdefinované role](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) se systémem řízení přístupu na základě role (RBAC) s omezeným podporou pro [vytváření vlastních rolí](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview) pro delegování privilegovaného přístupu k systému identit, aplikacím a prostředkům, které ovládá.</br>Správa rolí se dá rozšířit pomocí [Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) a poskytovat přístup k privilegovaným rolím za běhu, s omezeným časem nebo pomocí pracovního postupu. |
| Správa přihlašovacích údajů| Přihlašovací údaje ve službě Active Directory jsou založené na heslech, ověřování certifikátů a ověřování pomocí čipové karty. Hesla se spravují pomocí zásad hesel, které jsou založené na délce hesla, vypršení platnosti a složitosti.|Azure AD používá inteligentní [ochranu heslem](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad) pro Cloud a místní prostředí. Ochrana zahrnuje chytré zamykání a blokuje společná a vlastní hesla a náhrady. </br>Azure AD významně zvyšuje zabezpečení [prostřednictvím služby Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) a technologií bez [hesla](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless) , jako je FIDO2. </br>Služba Azure AD snižuje náklady na podporu tím, že poskytuje uživatelům [Samoobslužný systém pro resetování hesla](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) . |
| **Aplikace**|||
| Aplikace infrastruktury|Služba Active Directory je základem pro mnoho místních komponent infrastruktury, například DNS, DHCP, IPSec, Wi-Fi, NPS a VPN Access.|V novém cloudovém světě je Azure AD novou rovinou ovládacích prvků pro přístup k aplikacím, které se spoléhají na řízení sítě. Když se uživatelé ověřují[, podmíněný přístup (CA)](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)bude řídit, kteří uživatelé budou mít přístup k aplikacím, které jsou v požadovaných podmínkách.|
| Tradiční a starší aplikace| Většina místních aplikací používá protokol LDAP, integrované ověřování systému Windows (NTLM a Kerberos) nebo ověřování na základě hlaviček k řízení přístupu uživatelům.| Azure AD může poskytovat přístup k těmto typům místních aplikací pomocí agentů [proxy aplikací služby Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) místně běžících v místním prostředí. Pomocí této metody může Azure AD ověřovat uživatele Active Directory místně pomocí protokolu Kerberos při migraci nebo nutnosti koexistovat se staršími aplikacemi. |
| Aplikace SaaS|Služba Active Directory nepodporuje aplikace SaaS nativně a vyžaduje federační systém, například AD FS.|Aplikace SaaS podporující OAuth2, SAML a WS- \* Authentication lze integrovat pro ověřování pomocí služby Azure AD. |
| Obchodní aplikace (LOB) s moderním ověřováním|Organizace můžou použít AD FS se službou Active Directory k podpoře obchodních aplikací vyžadujících moderní ověřování.| Obchodní aplikace vyžadující moderní ověřování se dají nakonfigurovat tak, aby pro ověřování používaly službu Azure AD. |
| Služby střední vrstvy/démona|Služby běžící v místních prostředích obvykle používají účty služby AD nebo skupinové účty spravované služby (gMSA) ke spuštění. Tyto aplikace pak zdědí oprávnění k účtu služby.| Azure AD poskytuje [spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) pro spouštění dalších úloh v cloudu. Životní cyklus těchto identit spravuje Azure AD a je vázaný na poskytovatele prostředků se nedá použít k získání přístupu zadní vrátka k jiným účelům.|
| **Zařízení**|||
| Mobilní|Služba Active Directory nativně nepodporuje mobilní zařízení bez řešení třetích stran.| Řešení správy mobilních zařízení od Microsoftu, Microsoft Intune, je integrováno se službou Azure AD. Microsoft Intune poskytuje systému identit informace o stavu zařízení k vyhodnocení během ověřování. |
| Stolní počítače se systémem Windows|Služba Active Directory umožňuje připojit zařízení s Windows k doméně a spravovat je pomocí Zásady skupiny, System Center Configuration Manager nebo jiných řešení třetích stran.|Zařízení s Windows je možné [připojit ke službě Azure AD](https://docs.microsoft.com/azure/active-directory/devices/). Podmíněný přístup může v rámci procesu ověřování zjistit, jestli je zařízení připojené k Azure AD. Zařízení s Windows je taky možné spravovat pomocí [Microsoft Intune](https://docs.microsoft.com/intune/what-is-intune). V takovém případě podmíněný přístup před povolením přístupu k aplikacím bere v úvahu, jestli má zařízení stížnost (třeba aktualizované opravy zabezpečení a signatury virů).|
| Windows servery| Služba Active Directory poskytuje silné možnosti správy místních serverů Windows pomocí Zásady skupiny nebo jiných řešení pro správu.| Virtuální počítače s Windows serverem v Azure je možné spravovat pomocí [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/). [Spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) se dají použít, když virtuální počítače potřebují přístup k prostředkům nebo adresářům systému identit.|
| Úlohy Linux/UNIX|Služba Active Directory není nativně podporována bez řešení jiných výrobců, i když počítače se systémem Linux lze konfigurovat pro ověřování pomocí služby Active Directory jako sféru protokolu Kerberos.|Virtuální počítače se systémem Linux/UNIX můžou k přístupu k systémům identit a prostředkům používat [spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/index) . Některé organizace migrují tyto úlohy do cloudových kontejnerových technologií, které můžou používat i spravované identity.|

## <a name="next-steps"></a>Další kroky

- [Představení služby Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
- [Porovnání samoobslužně spravovaných Active Directory Domain Services, Azure Active Directory a spravovaných Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions)
- [Nejčastější dotazy týkající se Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-faq)
- [Co je nového v Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/whats-new)
