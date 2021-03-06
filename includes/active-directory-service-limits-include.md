---
title: zahrnout soubor
description: zahrnout soubor
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/28/2020
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 840357f51bbeeb877aba48fd8d04baad204cec1e
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87375185"
---
Tady jsou omezení využití a další omezení služby Azure Active Directory (Azure AD).

| Kategorie | Omezení |
| --- | --- |
| Adresáře | Jeden uživatel může jako člen nebo host patřit do maximálně 500 adresářů služby Azure AD.<br/>Jeden uživatel může vytvořit maximálně 200 adresářů. |
| Domény | Nemůžete přidat víc než 900 názvů spravovaných domén. Pokud pro všechny domény nastavíte federaci s využitím místní služby Active Directory, můžete v každém adresáři přidat maximálně 450 názvů domén. |
|Zdroje a prostředky |<ul><li>V jednom adresáři je možné vytvořit maximálně 50 000 prostředků Azure AD uživateli bezplatné edice Azure Active Directory ve výchozím nastavení. Pokud máte alespoň jednu ověřenou doménu, výchozí kvóta služby Azure AD pro vaši organizaci je rozšířena na 300 000 prostředků Azure AD. Toto omezení služby nesouvisí s limitem cenové úrovně 500 000 prostředků na stránce s cenami služby Azure AD. Pokud chcete přejít nad výchozí kvótu, musíte kontaktovat podpora Microsoftu.</li><li>Uživatel, který není správcem, může vytvořit maximálně 250 prostředků Azure AD. K této kvótě se počítá aktivní prostředky i odstraněné prostředky, které je možné obnovit. Pro obnovení jsou k dispozici pouze prostředky služby Azure AD, které byly odstraněny před méně než 30 dny. Odstranili jste prostředky služby Azure AD, které už nejsou dostupné pro obnovení tohoto počtu k této kvótě, a to v hodnotě jedna čtvrtina po dobu 30 dnů. Pokud máte vývojáři, kteří budou pravděpodobně opakovaně překračovat tuto kvótu v průběhu jejich běžných poplatků, můžete [vytvořit a přiřadit vlastní roli](../articles/active-directory/users-groups-roles/roles-quickstart-app-registration-limits.md) s oprávněním k vytvoření neomezeného počtu registrací aplikací.</li></ul> |
| Rozšíření schématu |<ul><li>Rozšíření řetězcového typu (String) mohou mít maximálně 256 znaků. </li><li>Rozšíření binárního typu (Binary) jsou omezena na 256 bajtů.</li><li>Do libovolného prostředku Azure AD se dají zapisovat jenom hodnoty rozšíření 100 napříč *všemi* typy a *všemi* aplikacemi.</li><li>Rozšířit pomocí atributů typu String nebo Binary s jednou hodnotou se dají jenom entity User, Group, TenantDetail, Device, Application a ServicePrincipal.</li><li>Rozšíření schématu jsou k dispozici jenom v rozhraní Graph API verze 1.21 Preview. Pro registraci rozšíření musí aplikace mít přístup k zápisu.</li></ul> |
| Aplikace |Vlastníky jedné aplikace může být maximálně 100 uživatelů. |
|Manifest aplikace |V manifestu aplikace lze přidat maximálně 1200 položek. |
| Skupiny |<ul><li>Uživatel může v organizaci Azure AD vytvořit maximálně 250 skupin.</li><li>Organizace Azure AD může mít maximálně 5000 dynamických skupin.<li>Vlastníky jedné skupiny může být maximálně 100 uživatelů.</li><li>Libovolný počet prostředků Azure AD může být členem jedné skupiny.</li><li>Uživatel může být členem libovolného počtu skupin.</li><li>Ve výchozím nastavení je počet členů ve skupině, které můžete synchronizovat z místní služby Active Directory, do Azure Active Directory pomocí Azure AD Connect omezený na 50 000 členů. Pokud potřebujete synchronizovat členství ve skupině, které překračuje tento limit, musíte připojit [rozhraní API koncového bodu Azure AD Connect Sync v2](../articles/active-directory/hybrid/how-to-connect-sync-endpoint-api-v2.md).</li><li>Vnořené skupiny v Azure AD se ve všech scénářích nepodporují.</li></ul><br/> V tomto okamžiku jsou podporovány následující scénáře s vnořenými skupinami.<ul><li> Jednu skupinu lze přidat jako člena jiné skupiny a můžete dosáhnout vnořování skupin.</li><li> Deklarace členství ve skupině (Pokud je aplikace nakonfigurovaná tak, aby přijímala deklarace členství ve skupině v tokenu, jsou zahrnuté vnořené skupiny, kterých je přihlášený uživatel členem.)</li><li>Podmíněný přístup (při určení oboru zásad podmíněného přístupu pro skupinu)</li><li>Omezení přístupu k samoobslužnému resetování hesla</li><li>Omezení, kteří uživatelé můžou provádět připojení k Azure AD a registraci zařízení</li></ul><br/>Následující scénáře nepodporují vnořené skupiny:<ul><li> Přiřazení role aplikace (přiřazování skupin k aplikaci se podporuje, ale skupiny vnořené v rámci přímo přiřazené skupiny nebudou mít přístup), a to jak pro přístup, tak pro zřizování.</li><li>Licencování na základě skupin (automatické přiřazení licence všem členům skupiny)</li><li>Skupiny sady Office 365.</li></ul> |
| Proxy aplikací | <ul><li>Maximálně 500 transakcí za sekundu na aplikaci proxy aplikace</li><li>Maximálně 750 transakcí za sekundu pro organizaci Azure AD</li></ul><br/>Transakce je definována jako jedna žádost HTTP a odpověď pro jedinečný prostředek. Když se omezí, klienti obdrží odpověď 429 (příliš mnoho požadavků). |
| Přístupový panel |Neexistuje žádné omezení počtu aplikací, které lze zobrazit na přístupovém panelu na uživatele bez ohledu na přiřazené licence.  |
| Sestavy | V jednotlivých sestavách je možné zobrazit nebo stáhnout maximálně 1 000 řádků. Veškerá další data se oříznou. |
| Jednotky pro správu | Prostředek služby Azure AD může být členem maximálně 30 jednotek pro správu. |
| Oprávnění a role správce | <ul><li>Skupinu nelze přidat jako [vlastníka](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership).</li><li>Skupině nelze přiřadit [roli](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).</li><li>Možnost uživatelů číst informace o adresáři jiných uživatelů nejde omezit mimo přepínač pro celou organizaci Azure AD a zakázat přístup všem uživatelům bez oprávnění správce ke všem informacím v adresáři (nedoporučuje se). Další informace o výchozích oprávněních [najdete tady](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users).</li><li>Může trvat až 15 minut nebo se odhlásit nebo přihlašovat před tím, než se projeví a odvolání členství v roli správce.</li></ul> |
