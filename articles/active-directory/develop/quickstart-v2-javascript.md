---
title: Přihlaste se k uživatelům v aplikacích JavaScript Single-Page | Azure
titleSuffix: Microsoft identity platform
description: Zjistěte, jak může aplikace JavaScriptu volat rozhraní API, které vyžaduje přístupové tokeny pomocí platformy Microsoft Identity Platform.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: 787f30302d163dc0097cde1be31e745d7f29bb64
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129776"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa"></a>Rychlý Start: přihlášení uživatelů a získání přístupového tokenu v ZABEZPEČENÉm kódu JavaScript

V tomto rychlém startu pomocí ukázky kódu zjistíte, jak se jednostránkové aplikace v JavaScriptu (SPA) můžou přihlašovat uživatelům osobních účtů, pracovních účtů a školních účtů. K volání rozhraní API pro Microsoft Graph nebo libovolné webové rozhraní API může získat přístupový token, a to prostřednictvím JavaScriptu. (Podívejte [se, jak ukázka funguje](#how-the-sample-works) pro ilustraci.)

## <a name="prerequisites"></a>Předpoklady

* Předplatné Azure – [vytvoření předplatného Azure zdarma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) (pro úpravy souborů projektu)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registrace a stažení aplikace pro rychlý Start
> Chcete-li spustit aplikaci pro rychlý Start, použijte kteroukoli z následujících možností.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Možnost 1 (Express): registrace a Automatická konfigurace aplikace a stažení ukázky kódu
>
> 1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
> 1. Pokud vám váš účet poskytne přístup k více než jednomu klientovi, vyberte účet v pravém horním rohu a nastavte relaci portálu na tenanta Azure Active Directory (Azure AD), který chcete použít.
> 1. Přejít na nové podokno [Azure Portal-registrace aplikací](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) .
> 1. Zadejte název své aplikace.
> 1. V části **podporované typy účtů**vyberte **účty v libovolném organizačním adresáři a osobní účty Microsoft**.
> 1. Vyberte **Zaregistrovat**.
> 1. Podle pokynů stáhněte a automaticky nakonfigurujte novou aplikaci.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Možnost 2 (ruční): registrace a ruční konfigurace aplikace a ukázky kódu
>
> #### <a name="step-1-register-your-application"></a>Krok 1: Registrace aplikace
>
> 1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí pracovního nebo školního účtu nebo osobního účet Microsoft.
>
> 1. Pokud vám váš účet poskytne přístup k více než jednomu klientovi, vyberte svůj účet v pravém horním rohu a pak nastavte relaci portálu na tenanta Azure AD, kterého chcete použít.
> 1. Přejít na stránku Microsoft Identity Platform for Developers [Registrace aplikací](https://go.microsoft.com/fwlink/?linkid=2083908) .
> 1. Vyberte **Nová registrace**.
> 1. Když se zobrazí stránka **Zaregistrovat aplikaci**, zadejte název pro vaši aplikaci.
> 1. V části **podporované typy účtů**vyberte **účty v libovolném organizačním adresáři a osobní účty Microsoft**.
> 1. Vyberte **Zaregistrovat**. Na stránce **Přehled** aplikace si poznamenejte hodnotu **ID aplikace (klienta)** pro pozdější použití.
> 1. Tento rychlý Start vyžaduje, aby byl povolený [tok implicitního udělení](v2-oauth2-implicit-grant-flow.md) . V levém podokně registrované aplikace vyberte **ověřování**.
> 1. V části **konfigurace platformy**vyberte **Přidat platformu**. Panel se otevře vlevo. Vyberte oblast **webové aplikace** .
> 1. Pořád na levé straně nastavte hodnotu **identifikátoru URI přesměrování** na `http://localhost:3000/` . Pak vyberte **přístupový token** a **token ID**.
> 1. Vyberte **Konfigurovat**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Krok 1: Konfigurace aplikace v Azure Portal
> Chcete-li vytvořit ukázku kódu v tomto rychlém startu, je nutné přidat `redirectUri` jako `http://localhost:3000/` a povolit **implicitní udělení**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Provést tyto změny pro mě]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Už nakonfigurované](media/quickstart-v2-javascript/green-check.png) Vaše aplikace je nakonfigurovaná s těmito atributy.

#### <a name="step-2-download-the-project"></a>Krok 2: Stažení projektu

> [!div renderon="docs"]
> Chcete-li spustit projekt s webovým serverem pomocí Node.js, [Stáhněte si základní soubory projektu](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).

> [!div renderon="portal"]
> Spustit projekt s webovým serverem pomocí Node.js

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Stažení ukázky kódu](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Krok 3: Konfigurace aplikace JavaScriptu
>
> Ve složce *JavaScriptSPA* upravte *authConfig.js*a nastavte `clientID` `authority` `redirectUri` hodnoty a v části `msalConfig` .
>
> ```javascript
>
>  // Config object to be passed to Msal on creation
>  const msalConfig = {
>    auth: {
>      clientId: "Enter_the_Application_Id_Here",
>      authority: "Enter_the_Cloud_Instance_Id_Here/Enter_the_Tenant_Info_Here",
>      redirectUri: "Enter_the_Redirect_Uri_Here",
>    },
>    cache: {
>      cacheLocation: "sessionStorage", // This configures where your cache will be stored
>      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
>    }
>  };
>
>```

> [!div renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
>
> Kde:
> - *\<Enter_the_Application_Id_Here>* je **ID aplikace (klienta)** pro aplikaci, kterou jste zaregistrovali.
> - *\<Enter_the_Cloud_Instance_Id_Here>* je instancí cloudu Azure. V případě hlavního nebo globálního cloudu Azure stačí zadat *https://login.microsoftonline.com* . Pro **národní** cloudy (například Čína) si přečtěte téma [národní cloudy](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud).
> - *\<Enter_the_Tenant_info_here>* je nastavená na jednu z následujících možností:
>    - Pokud vaše aplikace podporuje *účty v tomto organizačním adresáři*, nahraďte tuto hodnotu **ID tenanta** nebo **názvem tenanta** (například *contoso.Microsoft.com*).
>    - Pokud vaše aplikace podporuje *účty v jakémkoli organizačním adresáři*, nahraďte tuto hodnotu **organizacemi**.
>    - Pokud vaše aplikace podporuje *účty v libovolném organizačním adresáři a osobních účtech Microsoft*, nahraďte tuto hodnotu **běžnými**. Pokud chcete omezit podporu *jenom na osobní účty Microsoft*, nahraďte tuto hodnotu **příjemci**.
>
> > [!TIP]
> > Hodnoty **ID aplikace (klienta)**, **ID adresáře (tenanta)** a **Podporované typy účtu** najdete na stránce **Přehled** aplikace na webu Azure Portal.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Krok 3: vaše aplikace je nakonfigurovaná a připravená ke spuštění.
> Nakonfigurovali jsme projekt s hodnotami vlastností vaší aplikace.

> [!div renderon="docs"]
>
> Potom stále ve stejné složce upravte soubor *graphConfig.js* pro nastavení `graphMeEndpoint` a `graphMeEndpoint` pro `apiConfig` objekt.
> ```javascript
>   // Add here the endpoints for MS Graph API services you would like to use.
>   const graphConfig = {
>     graphMeEndpoint: "Enter_the_Graph_Endpoint_Here/v1.0/me",
>     graphMailEndpoint: "Enter_the_Graph_Endpoint_Here/v1.0/me/messages"
>   };
>
>   // Add here scopes for access token to be used at MS Graph API endpoints.
>   const tokenRequest = {
>       scopes: ["Mail.Read"]
>   };
> ```
>

> [!div renderon="docs"]
>
> Kde:
> - *\<Enter_the_Graph_Endpoint_Here>* je koncový bod, na který se bude volat volání rozhraní API. V případě hlavní nebo globální služby Microsoft Graph API stačí zadat `https://graph.microsoft.com` . Další informace najdete v tématu věnovaném [národním cloudovým nasazením](https://docs.microsoft.com/graph/deployments) .
>
> #### <a name="step-4-run-the-project"></a>Krok 4: spuštění projektu

Spusťte projekt s webovým serverem pomocí [Node.js](https://nodejs.org/en/download/):

1. Chcete-li spustit server, spusťte následující příkaz z adresáře projektu:
    ```batch
    npm install
    npm start
    ```
1. Otevřete webový prohlížeč a pokračujte na `http://localhost:3000/` .

1. Vyberte **Přihlásit** se a spusťte přihlášení a pak zavolejte Microsoft Graph API.

Poté, co prohlížeč načte aplikaci, vyberte možnost **Přihlásit**se. Při prvním přihlášení se zobrazí výzva k zadání vašeho souhlasu, který aplikaci umožní přístup k vašemu profilu a přihlášení. Po úspěšném přihlášení by se na stránce měly zobrazit informace o vašem profilu uživatele.

## <a name="more-information"></a>Další informace

### <a name="how-the-sample-works"></a>Jak ukázka funguje

![Jak ukázka ZABEZPEČENÉho kódu JavaScript funguje: 1. SPA inicializuje přihlášení. 2. SPA získá token ID z platformy Microsoft identity. 3. Zabezpečené ověřování hesla volá token získání. 4. Platforma Microsoft Identity vrací přístupový token do zabezpečeného hesla. 5. Protokol SPA vytvoří požadavek a HTTP GET s tokenem ACE pro rozhraní Microsoft Graph API. 6. Graph API vrátí odpověď protokolu HTTP do zabezpečeného hesla.](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal.js

Knihovna MSAL se přihlásí uživatelům a požádá o tokeny, které se používají pro přístup k rozhraní API, které je chráněné platformou Microsoft identity. Soubor rychlý Start *index.html* obsahuje odkaz na knihovnu:

```html
<script type="text/javascript" src="https://alcdn.msftauth.net/lib/1.2.1/js/msal.js" integrity="sha384-9TV1245fz+BaI+VvCjMYL0YDMElLBwNS84v3mY57pXNOt6xcUYch2QLImaTahcOP" crossorigin="anonymous"></script>
```
> [!TIP]
> Předchozí verzi můžete nahradit nejnovější vydanou verzí v části [MSAL.js releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).

Případně, pokud máte Node.js nainstalované, můžete nejnovější verzi stáhnout prostřednictvím Správce balíčků Node.js (npm):

```batch
npm install msal
```

### <a name="msal-initialization"></a>Inicializace knihovny MSAL

Kód pro rychlý Start také ukazuje, jak inicializovat knihovnu MSAL:

```javascript
  // Config object to be passed to Msal on creation
  const msalConfig = {
    auth: {
      clientId: "75d84e7a-40bx-f0a2-91b9-0c82d4c556aa", // this is a fake id
      authority: "https://login.microsoftonline.com/common",
      redirectUri: "http://localhost:3000/",
    },
    cache: {
      cacheLocation: "sessionStorage", // This configures where your cache will be stored
      storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
    }
  };

const myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Kde  | Popis |
> |---------|---------|
> |`clientId`     | ID aplikace, která je zaregistrována v Azure Portal.|
> |`authority`    | Volitelné Adresa URL autority, která podporuje typy účtů, jak je popsáno výše v části konfigurace. Výchozí autorita je `https://login.microsoftonline.com/common` . |
> |`redirectUri`     | Nakonfigurovaná odpověď/redirectUri registrace aplikace V tomto případě `http://localhost:3000/` . |
> |`cacheLocation`  | Volitelné Nastaví úložiště prohlížeče pro stav ověřování. Výchozí hodnota je sessionStorage.   |
> |`storeAuthStateInCookie`  | Volitelné Knihovna, ve které je uložen stav žádosti o ověření, který je požadován pro ověření toků ověřování v souborech cookie prohlížeče. Tento soubor cookie je nastaven pro prohlížeče IE a Edge, aby se zmírnily určité [známé problémy](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues). |

Další informace o dostupných konfigurovatelných možnostech najdete v tématu [inicializace klientských aplikací](msal-js-initializing-client-applications.md).

### <a name="sign-in-users"></a>Přihlášení uživatelů

Následující fragment kódu ukazuje, jak přihlašovat uživatele:

```javascript
// Add scopes for the id token to be used at Microsoft identity platform endpoints.
const loginRequest = {
    scopes: ["openid", "profile", "User.Read"],
};

myMSALObj.loginPopup(loginRequest)
    .then((loginResponse) => {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Kde  | Popis |
> |---------|---------|
> | `scopes`   | Volitelné Obsahuje obory, které jsou požadovány pro souhlas uživatele v době přihlášení. Například `[ "user.read" ]` pro Microsoft Graph nebo `[ "<Application ID URL>/scope" ]` pro vlastní webová rozhraní API (tj `api://<Application ID>/access_as_user` .). |

> [!TIP]
> Alternativně můžete chtít použít `loginRedirect` metodu pro přesměrování aktuální stránky na přihlašovací stránku místo na místní okno.

### <a name="request-tokens"></a>Žádosti o tokeny

MSAL používá tři metody k získání tokenů: `acquireTokenRedirect` , `acquireTokenPopup` a.`acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění

`acquireTokenSilent`Metoda zpracovává získání a obnovení tokenu bez zásahu uživatele. Po `loginRedirect` `loginPopup` prvním spuštění metody nebo `acquireTokenSilent` se jedná o metodu, která se běžně používá k získání tokenů používaných k přístupu k chráněným prostředkům pro následná volání. Volání požadavků na požadavky nebo obnovení tokenů se provádí tiše.

```javascript

const tokenRequest = {
    scopes: ["Mail.Read"]
};

myMSALObj.acquireTokenSilent(tokenRequest)
    .then((tokenResponse) => {
        // Callback code here
        console.log(tokenResponse.accessToken);
    }).catch((error) => {
        console.log(error);
    });
```

> |Kde  | Popis |
> |---------|---------|
> | `scopes`   | Obsahuje požadované obory, které mají být vráceny v přístupovém tokenu pro rozhraní API. Například `[ "mail.read" ]` pro Microsoft Graph nebo `[ "<Application ID URL>/scope" ]` pro vlastní webová rozhraní API (tj `api://<Application ID>/access_as_user` .).|

#### <a name="get-a-user-token-interactively"></a>Interaktivní získání tokenu uživatele

Existují situace, kdy potřebujete vynutit, aby uživatelé mohli pracovat s koncovým bodem Microsoft Identity Platform. Příklad:
* Uživatelé možná budou muset znovu zadat svoje přihlašovací údaje, protože vypršela platnost hesla.
* Vaše aplikace požaduje přístup k dalším oborům prostředků, ke kterým uživatel musí vyjádřit souhlas.
* Je vyžadováno dvojúrovňové ověřování.

Obvyklý doporučený vzor pro většinu aplikací je zavolat `acquireTokenSilent` nejprve a pak zachytit výjimku a pak voláním `acquireTokenPopup` (nebo `acquireTokenRedirect` ) spustit interaktivní požadavek.

Volání `acquireTokenPopup` výsledků v překryvném okně pro přihlášení. (Nebo `acquireTokenRedirect` má za následek přesměrování uživatelů na koncový bod Microsoft Identity Platform.) V tomto okně musí uživatelé interaktivně pracovat tím, že potvrdí své přihlašovací údaje, udělí souhlas požadovanému prostředku nebo dokončí ověřování pomocí dvou faktorů.

```javascript
// Add here scopes for access token to be used at MS Graph API endpoints.
const tokenRequest = {
    scopes: ["Mail.Read"]
};

myMSALObj.acquireTokenPopup(requestObj)
    .then((tokenResponse) => {
        // Callback code here
        console.log(tokenResponse.accessToken);
    }).catch((error) => {
        console.log(error);
    });
```

> [!NOTE]
> V tomto rychlém `loginRedirect` startu `acquireTokenRedirect` se používá metoda a v aplikaci Microsoft Internet Explorer, protože se jedná o [známý problém](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) související se zpracováním překryvných oken aplikací Internet Explorer.

## <a name="next-steps"></a>Další kroky

Podrobnější návod k sestavování aplikace pro tento rychlý Start najdete v těchto tématech:

> [!div class="nextstepaction"]
> [Kurz pro přihlášení a volání MS graphu](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

K procházení úložiště MSAL pro dokumentaci, nejčastější dotazy, problémy a další informace najdete v těchto tématech:

> [!div class="nextstepaction"]
> [MSAL.js úložiště GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js)
