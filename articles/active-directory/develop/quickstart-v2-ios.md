---
title: Microsoft Identity Platform iOS a macOS – rychlý Start | Azure
description: Přečtěte si, jak se přihlašovat uživatelé a dotazy Microsoft Graph v aplikaci pro iOS nebo macOS.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/24/2019
ms.author: marsma
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:iOS
ms.openlocfilehash: bd17c20707c3bf1a153e9209cd7e2ec279de0501
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/30/2020
ms.locfileid: "85554048"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-or-macos-app"></a>Rychlý Start: přihlášení uživatelů a volání rozhraní Microsoft Graph API z aplikace pro iOS nebo macOS

Tento rychlý Start obsahuje ukázku kódu, která předvádí, jak může nativní aplikace pro iOS nebo macOS používat Microsoft Identity Platform k přihlašování osobních, pracovních a školních účtů, získání přístupového tokenu a volání rozhraní Microsoft Graph API.

Tento rychlý Start se týká aplikací pro iOS a macOS. Některé kroky jsou potřeba jenom pro aplikace pro iOS. Tyto kroky odvolají, že jsou pouze pro iOS.

![Ukazuje, jak ukázková aplikace vygenerovaná tímto rychlým startem funguje.](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Požadavky**
> * XCode 10 +
> * iOS 10 +
> * macOS 10.12 +

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registrace a stažení aplikace pro rychlý start
> Aplikaci pro rychlý start můžete spustit dvěma způsoby:
> * [Expresní] [Možnost 1: Registrace a automatická konfigurace aplikace a následné stažení vzorového kódu](#option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample)
> * [Ruční] [Možnost 2: Registrace a ruční konfigurace aplikace a vzorového kódu](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample"></a>Možnost 1: registrace a Automatická konfigurace aplikace a stažení ukázky kódu
> #### <a name="step-1-register-your-application"></a>Krok 1: Registrace aplikace
> K registraci aplikace
> 1. Přejít na nové podokno [Azure Portal-registrace aplikací](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) .
> 1. Zadejte název vaší aplikace a Vyberte **Zaregistrovat**.
> 1. Postupujte podle pokynů ke stažení a automatické konfiguraci nové aplikace jedním kliknutím.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Možnost 2: Registrace a ruční konfigurace aplikace a vzorového kódu
>
> #### <a name="step-1-register-your-application"></a>Krok 1: Registrace aplikace
> Pokud chcete zaregistrovat aplikaci a ručně přidat informace o registraci aplikace ke svému řešení, postupujte následovně:
>
> 1. Přejděte na stránku [Registrace aplikací](https://aka.ms/MobileAppReg) Microsoft Identity Platform for Developers.
> 1. Vyberte **Nová registrace**.
> 1. Když se zobrazí stránka **Registrace aplikace**, zadejte registrační informace vaší aplikace:
>      - V části **název** zadejte smysluplný název aplikace, který se uživatelům aplikace zobrazí při přihlášení nebo souhlasu vaší aplikace.
>      - Přeskočit další konfigurace na této stránce.
>      - Vyberte `Register`.
> 1. V části **Spravovat** vyberte `Authentication`  >  `Add Platform`  >  `iOS` .
>      - Zadejte ***identifikátor sady prostředků*** pro vaši aplikaci. Identifikátor sady prostředků je pouze jedinečný řetězec, který jedinečně identifikuje vaši aplikaci, například `com.<yourname>.identitysample.MSALMacOS` . Poznamenejte si hodnotu, kterou používáte.
>      - Všimněte si, že konfigurace iOS platí také pro aplikace macOS.
> 1. `Configure`V tomto rychlém startu vyberte a uložte podrobnosti ***Konfigurace MSAL*** pro později.
> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>Krok 1: Konfigurace aplikace
> Aby ukázka kódu pro tento rychlý Start fungovala, je nutné přidat identifikátor URI pro přesměrování, který je kompatibilní s zprostředkovatelem ověřování.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Udělat změnu za mě]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Už nakonfigurované](media/quickstart-v2-ios/green-check.png) Vaše aplikace je nakonfigurovaná s těmito atributy.
> 
> #### <a name="step-2-download-the-sample-project"></a>Krok 2: stažení ukázkového projektu
> > [!div id="autoupdate_ios" class="nextstepaction"]
> > [Stažení ukázky kódu pro iOS]()
> 
> > [!div id="autoupdate_macos" class="nextstepaction"]
> > [Stažení ukázky kódu pro macOS]()
> [!div renderon="docs"]
> #### <a name="step-2-download-the-sample-project"></a>Krok 2: stažení ukázkového projektu
> 
> - [Stažení ukázky kódu pro iOS](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
> - [Stažení ukázky kódu pro macOS](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

#### <a name="step-3-install-dependencies"></a>Krok 3: instalace závislostí

V okně terminálu přejděte do složky s ukázkou staženého kódu a spusťte `pod install` instalaci nejnovější knihovny MSAL.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-4-your-app-is-configured-and-ready-to-run"></a>Krok 4: vaše aplikace je nakonfigurovaná a připravená ke spuštění.
> Nakonfigurovali jsme projekt s hodnotami vlastností vaší aplikace a je připraven ke spuštění.
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
>
> [!div renderon="docs"]
>#### <a name="step-4-configure-your-project"></a>Krok 4: konfigurace projektu
> Pokud jste vybrali možnost 1 výše, můžete tento postup přeskočit.
> 1. Extrahujte soubor zip a otevřete projekt v XCode.
> 1. Upravte **soubor viewcontroller. SWIFT** a nahraďte řádek začínající řetězcem let kClientID s následujícím fragmentem kódu. Nezapomeňte aktualizovat hodnotu pro `kClientID` clientID, kterou jste uložili při registraci aplikace na portálu dříve v tomto rychlém startu:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. Pokud vytváříte aplikaci pro [národní cloudy Azure AD](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints), nahraďte řádek začínající řetězcem let kGraphEndpoint a let kAuthority se správnými koncovými body. Pro globální přístup použijte výchozí hodnoty:
>     ```swift
>     let kGraphEndpoint = "https://graph.microsoft.com/"
>     let kAuthority = "https://login.microsoftonline.com/common"
>     ```
> 1. Další koncové body jsou popsány [zde](https://docs.microsoft.com/graph/deployments#app-registration-and-token-service-root-endpoints). Pokud například chcete spustit rychlý Start s Azure AD Německo, použijte následující:
>     ```swift
>     let kGraphEndpoint = "https://graph.microsoft.de/"
>     let kAuthority = "https://login.microsoftonline.de/common"
>     ```
> 1. Otevřete nastavení projektu. V části **Identita** zadejte **identifikátor sady prostředků** , který jste zadali na portálu.
> 1. Klikněte pravým tlačítkem na **info. plist** a vyberte **Otevřít jako**  >  **zdrojový kód**.
> 1. V kořenovém uzlu dict – nahraďte `Enter_the_bundle_Id_Here` ***ID sady prostředků*** , které jste použili na portálu.
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
>
>    ```
> 1. Sestavte & spusťte aplikaci.

## <a name="more-information"></a>Další informace

Další informace o tomto rychlém startu najdete v následujících částech.

### <a name="get-msal"></a>Získat MSAL

MSAL ([MSAL. Framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) je knihovna používaná k přihlašování uživatelů a žádosti o tokeny používané pro přístup k rozhraní API chráněnému platformou Microsoft identity. Knihovnu MSAL můžete do své aplikace přidat následujícím způsobem:

```
$ vi Podfile

```
Do tohoto souboru podfile (s cílem projektu) přidejte následující:

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL'
end

```

Spusťte instalační příkaz CocoaPods:

```pod install```

### <a name="initialize-msal"></a>Inicializovat MSAL

Odkaz na knihovnu MSAL můžete přidat tak, že přidáte následující kód:

```swift
import MSAL
```

Potom inicializujte knihovnu MSAL pomocí následujícího kódu:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)

let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
```

> |Kde: | Popis |
> |---------|---------|
> | `clientId` | ID aplikace z aplikace zaregistrované na webu *portal.azure.com* |
> | `authority` | Koncový bod Microsoft Identity Platform. Ve většině případů to bude *https<span/>://login.microsoftonline.com/common* |
> | `redirectUri` | Identifikátor URI přesměrování aplikace Můžete předat hodnotu Nil a použít výchozí hodnotu nebo vlastní identifikátor URI přesměrování. |

### <a name="for-ios-only-additional-app-requirements"></a>Jenom pro iOS, požadavky na další aplikace

Vaše aplikace musí mít také následující v `AppDelegate` . To umožňuje, aby sada SDK MSAL při ověřování zpracovala odpověď tokenu z aplikace zprostředkovatele ověřování.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {

        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }

 ```

> [!NOTE]
> V systému iOS 13 +, pokud místo toho místo `UISceneDelegate` použijte `UIApplicationDelegate` Tento kód do `scene:openURLContexts:` zpětného volání (viz [Dokumentace společnosti Apple](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)).
> Pokud podporujete UISceneDelegate i UIApplicationDelegate pro zajištění kompatibility se staršími systémy iOS, je nutné MSAL zpětné volání umístit na obě místa.

 ```swift
 func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {

        guard let urlContext = URLContexts.first else {
            return
        }

        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication

        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
 ```

A konečně, vaše aplikace musí mít `LSApplicationQueriesSchemes` položku v souboru ***info. plist*** spolu s `CFBundleURLTypes` . Tato ukázka je obsažena v tomto příkladu.

   ```xml
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Přihlášení uživatelů & žádosti o tokeny

Knihovna MSAL používá k získání tokenů dvě metody: `acquireToken` a `acquireTokenSilent`.

#### <a name="acquiretoken-get-a-token-interactively"></a>acquireToken: interaktivní získání tokenu

Některé situace vyžadují, aby uživatelé mohli pracovat s platformou Microsoft identity. V těchto případech může být koncový uživatel muset vybrat svůj účet, zadat jejich přihlašovací údaje nebo vyjádřit souhlas s oprávněními vaší aplikace. Třeba

* Při prvním přihlášení uživatele k aplikaci
* Pokud uživatel resetuje heslo, bude muset zadat své přihlašovací údaje.
* Když vaše aplikace požaduje při prvním pokusu o přístup k prostředku
* Pokud jsou vyžadovány MFA nebo jiné zásady podmíněného přístupu

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: self.webViewParamaters!)
self.applicationContext!.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Kde:| Popis |
> |---------|---------|
> | `scopes` | Obsahuje požadované obory (tj. `[ "user.read" ]` pro Microsoft Graph nebo `[ "<Application ID URL>/scope" ]` pro vlastní webová rozhraní API ( `api://<Application ID>/access_as_user` ) |

#### <a name="acquiretokensilent-get-an-access-token-silently"></a>acquireTokenSilent: tiché získání přístupového tokenu

Aplikace by neměly vyžadovat, aby se uživatelé přihlásili pokaždé, když požadují token. Pokud se uživatel už přihlásil, tato metoda umožňuje aplikacím vyžádat tokeny v tichém režimu.

```swift
self.applicationContext!.getCurrentAccount(with: nil) { (currentAccount, previousAccount, error) in

   guard let account = currentAccount else {
      return
   }

   let silentParams = MSALSilentTokenParameters(scopes: self.kScopes, account: account)
   self.applicationContext!.acquireTokenSilent(with: silentParams) { (result, error) in /* Add your handling logic */}
}
```

> |Kde: | Popis |
> |---------|---------|
> | `scopes` | Obsahuje požadované obory (tj. `[ "user.read" ]` pro Microsoft Graph nebo `[ "<Application ID URL>/scope" ]` pro vlastní webová rozhraní API ( `api://<Application ID>/access_as_user` ) |
> | `account` | Účet, pro který se požaduje token. Tento rychlý Start se týká aplikace s jedním účtem. Pokud chcete vytvořit aplikaci s více účty, budete muset definovat logiku pro identifikaci, který účet se má použít pro žádosti o tokeny pomocí `accountsFromDeviceForParameters:completionBlock:` a předávání správných.`accountIdentifier` |

## <a name="next-steps"></a>Další kroky

Vyzkoušejte si kurz pro iOS a macOS, kde najdete kompletní podrobný průvodce vytvářením aplikací, včetně kompletního vysvětlení tohoto rychlého startu.

### <a name="learn-how-to-create-the-application-used-in-this-quickstart"></a>Naučte se, jak vytvořit aplikaci používanou v tomto rychlém startu.

> [!div class="nextstepaction"]
> [Kurz pro volání Graph API pro iOS a macOS](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
