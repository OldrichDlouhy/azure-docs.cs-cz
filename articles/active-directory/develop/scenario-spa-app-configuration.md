---
title: Konfigurace jednostránkové aplikace – Microsoft Identity Platform | Azure
description: Naučte se vytvářet jednostránkové aplikace (konfigurace kódu aplikace).
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f159105046231ba5fb4e458cdd70d930a411a920
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80882331"
---
# <a name="single-page-application-code-configuration"></a>Jednostránkové aplikace: Konfigurace kódu

Naučte se konfigurovat kód pro jednostránkové aplikace (SPA).

## <a name="msal-libraries-that-support-implicit-flow"></a>MSAL knihovny podporující implicitní tok

Platforma Microsoft Identity poskytuje následující knihovny Microsoft Authentication Library (MSAL) pro podporu implicitního toku pomocí doporučených postupů zabezpečení v oboru:

| Knihovna MSAL | Description |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Jednoduchá knihovna JavaScriptu pro použití v jakékoli webové aplikaci na straně klienta, která je sestavena prostřednictvím rozhraní JavaScript nebo SPA, jako je například úhlová, Vue.jsa a React.js. |
| ![MSALý úhlový](media/sample-v2-code/logo_angular.png) <br/> [MSALý úhlový](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Obálka základní knihovny MSAL.js pro zjednodušení použití v jednostránkovéch aplikacích, které jsou sestaveny prostřednictvím úhlové architektury. |

## <a name="application-code-configuration"></a>Konfigurace kódu aplikace

V knihovně MSAL jsou informace o registraci aplikace předány jako konfigurace při inicializaci knihovny.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Další informace o konfigurovatelných možnostech naleznete v tématu [inicializace aplikace pomocí MSAL.js](msal-js-initializing-client-applications.md).

# <a name="angular"></a>[Angular](#tab/angular)

```javascript
// App.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id'
            }
        })
    ]
})

export class AppModule { }
```

---

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přihlášení a odhlášení](scenario-spa-sign-in.md)
