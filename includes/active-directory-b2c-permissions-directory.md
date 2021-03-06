---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: f8c972bdb9195008c2983d3993e8d9369749b284
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85200172"
---
#### <a name="app-registrations"></a>[Registrace aplikací](#tab/app-reg-ga/) 

1. V části **Spravovat**vyberte **oprávnění rozhraní API**.
1. V části **konfigurovaná oprávnění**vyberte **Přidat oprávnění**.
1. Vyberte kartu **rozhraní Microsoft API** a pak vyberte **Microsoft Graph**.
1. Vyberte **oprávnění aplikace**.
1. Rozbalte příslušnou skupinu oprávnění a zaškrtněte políčko oprávnění pro udělení vaší aplikaci pro správu. Příklad:
    * **AuditLog**  >  **AuditLog. Read. All**: pro čtení protokolů auditu adresáře.
    * **Adresář**  >  **Directory.** prokládání. All: pro scénáře migrace uživatelů nebo správy uživatelů.
    * **Zásada**  >  **Policy. TrustFramework**: pro scénáře průběžné integrace a průběžného doručování (CI/CD). Například nasazení vlastních zásad pomocí Azure Pipelines.
1. Vyberte **Přidat oprávnění**. Jak je směrované, počkejte několik minut, než budete pokračovat k dalšímu kroku.
1. Vyberte **udělit souhlas správce pro (název vašeho tenanta)**.
1. Pokud aktuálně nejste přihlášeni pomocí účtu globálního správce, přihlaste se pomocí účtu v Azure AD B2C klientovi, kterému byla přiřazena alespoň role *správce cloudové aplikace* , a pak vyberte **udělit souhlas správce pro (název tenanta)**.
1. Vyberte **aktualizovat**a pak ověřte, že "uděleno pro..." zobrazí se pod položkou **stav**. Rozšíření oprávnění může trvat několik minut.

#### <a name="applications-legacy"></a>[Aplikace (starší verze)](#tab/applications-legacy/)

1. Na stránce Přehled **zaregistrovaných aplikací** vyberte **Nastavení**.
1. V části **přístup přes rozhraní API**vyberte **požadovaná oprávnění**.
1. Vyberte **Microsoft Graph**.
1. V části **oprávnění aplikace**zaškrtněte políčko oprávnění pro udělení vaší aplikaci pro správu. Příklad:
    * **Číst všechna data protokolu auditu**: vyberte toto oprávnění pro čtení protokolů auditu adresáře.
    * **Čtení a zápis dat adresáře**: Toto oprávnění vyberte pro scénáře migrace uživatelů nebo správy uživatelů.
    * **Čtení a zápis zásad pro vztahy důvěryhodnosti vaší organizace**: Toto oprávnění vyberte pro scénáře průběžné integrace/průběžného doručování (CI/CD). Například nasazení vlastních zásad pomocí Azure Pipelines.
1. Vyberte **Uložit**.
1. Vyberte **udělit oprávnění**a pak vyberte **Ano**. Aby bylo možné plně šířit oprávnění, může trvat několik minut.
