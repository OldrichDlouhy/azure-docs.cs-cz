---
title: Microsoft Identity Platform Android – rychlý Start | Azure
description: Přečtěte si, jak můžou aplikace pro Android volat rozhraní API, které vyžaduje přístupové tokeny pomocí koncového bodu Microsoft Identity Platform.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/15/2019
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:Android
ms.openlocfilehash: 9afb5b7602b220c25d919f8fe0773d5cfa143d89
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80991190"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Rychlý start: Přihlášení uživatelů a volání rozhraní Microsoft Graph API z aplikace pro Android

V tomto rychlém startu se používá ukázka kódu, která předvádí, jak se aplikace pro Android může přihlašovat pomocí platformy Microsoft identity na osobní, pracovní nebo školní účty a získat přístupový token a volat rozhraní Microsoft Graph API. (Podívejte [se, jak ukázka funguje](#how-the-sample-works) pro ilustraci.)

Aplikace musí být reprezentovány objektem aplikace v Azure Active Directory tak, aby platforma identity Microsoftu mohla poskytovat tokeny vaší aplikaci.

> [!div renderon="docs"]
> V zájmu usnadnění je ukázka kódu dodávána s výchozí `redirect_uri` předem nakonfigurovanou v `AndroidManifest.xml` souboru, takže nemusíte nejdřív registrovat vlastní objekt aplikace. A `redirect_uri` je částečně založený na podpisovém klíči vaší aplikace. Vzorový projekt je předem nakonfigurovaný pomocí podpisového klíče, aby zadaný `redirect_uri` bude fungovat. Chcete-li získat další informace o registraci objektu aplikace a jeho integraci s vaší aplikací, přečtěte si téma [přihlášení uživatelů a zavolejte Microsoft Graph z kurzu aplikace pro Android](tutorial-v2-android.md) .


> [!NOTE]
> **Požadavky**
> * Android Studio 
> * Android 16 +

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Krok 1: Konfigurace aplikace v Azure Portal 
>  Aby ukázka kódu pro tento rychlý Start fungovala, je nutné přidat identifikátor URI pro přesměrování, který je kompatibilní s zprostředkovatelem ověřování.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Provést tyto změny pro mě]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Už nakonfigurované](media/quickstart-v2-android/green-check.png) Vaše aplikace je nakonfigurovaná s těmito atributy.
>
> ### <a name="step-2-download-the-project"></a>Krok 2: Stažení projektu 
> [!div class="sxs-lookup" renderon="portal"]
> Spusťte projekt pomocí Android Studio.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Stažení ukázky kódu](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip)
>
> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Krok 3: vaše aplikace je nakonfigurovaná a připravená ke spuštění.
> Nakonfigurovali jsme projekt s hodnotami vlastností vaší aplikace a je připraven ke spuštění. 
> Ukázková aplikace se spustí na obrazovce **režimu jednoho účtu** . Ve výchozím nastavení je k dispozici výchozí obor ( **User. Read**), který se používá při čtení dat profilu během volání rozhraní API Microsoft Graph. Ve výchozím nastavení je k dispozici adresa URL pro volání rozhraní API Microsoft Graph. Oba tyto typy můžete změnit, pokud chcete.
>
> ![Ukázková aplikace MSAL ukazující použití jednoho a více účtů](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Pomocí nabídky aplikace můžete změnit mezi jedním a více režimy účtu.
>
> V režimu jednoho účtu se přihlaste pomocí pracovního nebo domácího účtu:
>
> 1. Vyberte možnost **získat data grafu interaktivně** , aby se uživateli zobrazila výzva k zadání přihlašovacích údajů. V dolní části obrazovky uvidíte výstup volání rozhraní Microsoft Graph API.
> 2. Po přihlášení vyberte možnost **získat data grafu v tichém režimu** , aby bylo možné volat rozhraní API pro Microsoft Graph bez výzvy k zadání přihlašovacích údajů uživatele. V dolní části obrazovky uvidíte výstup volání rozhraní Microsoft Graph API.
>
> V režimu více účtů můžete opakovat stejný postup.  Kromě toho můžete odebrat přihlášený účet, který také odebere tokeny uložené v mezipaměti pro tento účet.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> ## <a name="step-1-get-the-sample-app"></a>Krok 1: získání ukázkové aplikace
>
> [Stáhněte si kód](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip).
>
> ## <a name="step-2-run-the-sample-app"></a>Krok 2: spuštění ukázkové aplikace
>
> Vyberte emulátor nebo fyzické zařízení, v rozevíracím seznamu **dostupná zařízení** Android Studio a spusťte aplikaci.
>
> Ukázková aplikace se spustí na obrazovce **režimu jednoho účtu** . Ve výchozím nastavení je k dispozici výchozí obor ( **User. Read**), který se používá při čtení dat profilu během volání rozhraní API Microsoft Graph. Ve výchozím nastavení je k dispozici adresa URL pro volání rozhraní API Microsoft Graph. Oba tyto typy můžete změnit, pokud chcete.
>
> ![Ukázková aplikace MSAL ukazující použití jednoho a více účtů](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Pomocí nabídky aplikace můžete změnit mezi jedním a více režimy účtu.
>
> V režimu jednoho účtu se přihlaste pomocí pracovního nebo domácího účtu:
>
> 1. Vyberte možnost **získat data grafu interaktivně** , aby se uživateli zobrazila výzva k zadání přihlašovacích údajů. V dolní části obrazovky uvidíte výstup volání rozhraní Microsoft Graph API.
> 2. Po přihlášení vyberte možnost **získat data grafu v tichém režimu** , aby bylo možné volat rozhraní API pro Microsoft Graph bez výzvy k zadání přihlašovacích údajů uživatele. V dolní části obrazovky uvidíte výstup volání rozhraní Microsoft Graph API.
>
> V režimu více účtů můžete opakovat stejný postup.  Kromě toho můžete odebrat přihlášený účet, který také odebere tokeny uložené v mezipaměti pro tento účet.

## <a name="how-the-sample-works"></a>Jak ukázka funguje
![Snímek obrazovky ukázkové aplikace](media/quickstart-v2-android/android-intro.svg)


Kód je uspořádaný na fragmenty, které ukazují, jak napsat jednu a více účtů MSAL aplikaci. Soubory s kódem jsou uspořádány takto:

| File  | Demonstruje  |
|---------|---------|
| MainActivity | Spravuje uživatelské rozhraní. |
| MSGraphRequestWrapper  | Volá rozhraní API Microsoft Graph pomocí tokenu poskytnutého funkcí MSAL. |
| MultipleAccountModeFragment  | Inicializuje aplikaci s více účty, načte uživatelský účet a získá token pro volání rozhraní API Microsoft Graph. |
| SingleAccountModeFragment | Inicializuje aplikaci s jedním účtem, načte uživatelský účet a získá token pro volání rozhraní API Microsoft Graph. |
| res/auth_config_multiple_account. JSON  | Konfigurační soubor s více účty |
| res/auth_config_single_account. JSON  | Konfigurační soubor s jedním účtem |
| Gradle skripty/Build. Grad (Module: App) | Tady se přidají závislosti knihovny MSAL. |

Nyní se podrobněji podíváme na tyto soubory a v každém z nich budete volat kód specifický pro MSAL.

### <a name="adding-msal-to-the-app"></a>Přidání MSAL do aplikace

MSAL ([com. Microsoft. identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) je knihovna používaná k přihlašování uživatelů a žádosti o tokeny používané pro přístup k rozhraní API chráněnému platformou Microsoft identity. Gradle 3.0 + nainstaluje knihovnu, když přidáte následující do **skriptu** > Gradle**Build. Gradle (Module: App)** v části **závislosti**:

```gradle  
implementation 'com.microsoft.identity.client:msal:1.+'
```

Můžete to vidět v ukázkovém projektu v sestavení. Gradle (modul: aplikace):

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.+'
    ...
}
```

Tím se dá Gradle stáhnout a sestavit MSAL z Maven Central.

### <a name="msal-imports"></a>MSAL importy

Importy, které jsou relevantní pro knihovnu MSAL, `com.microsoft.identity.client.*`jsou.  Například uvidíte `import com.microsoft.identity.client.PublicClientApplication;` , což je obor názvů pro `PublicClientApplication` třídu, který představuje vaši veřejnou klientskou aplikaci.

### <a name="singleaccountmodefragmentjava"></a>SingleAccountModeFragment. Java

Tento soubor ukazuje, jak vytvořit MSAL aplikaci s jedním účtem a zavolat rozhraní API Microsoft Graph.

Aplikace s jedním účtem používá jenom jeden uživatel.  Například můžete mít jenom jeden účet, který se přihlašujete do aplikace mapování pomocí.

#### <a name="single-account-msal-initialization"></a>MSAL inicializace jednoho účtu

V `auth_config_single_account.json`systému je `onCreateView()`v nástroji vytvořen jediný `PublicClientApplication` účet s použitím konfiguračních informací uložených v `auth_config_single_account.json` souboru.  Tímto způsobem inicializujete knihovnu MSAL pro použití v aplikaci MSAL s jedním účtem:

```java
...
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createSingleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_single_account,
        new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(ISingleAccountPublicClientApplication application) {
                /**
                 * This test app assumes that the app is only going to support one account.
                 * This requires "account_mode" : "SINGLE" in the config json file.
                 **/
                mSingleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                displayError(exception);
            }
        });
```

#### <a name="sign-in-a-user"></a>Přihlášení uživatele

V `SingleAccountModeFragment.java`kódu, který se má přihlašovat uživateli `initializeUI()`, se nachází v obslužné `signInButton` rutině Click.

Před `signIn()` pokusem o získání tokenů volejte volání. `signIn()`se chová stejně, `acquireToken()` jako by byl volán, což vede k interaktivní výzvě pro uživatele, aby se přihlásili.

Přihlášení uživatele je asynchronní operace. Zpětné volání, které volá rozhraní Microsoft Graph API a aktualizuje uživatelské rozhraní po přihlášení uživatele:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Odhlásit uživatele

V `SingleAccountModeFragment.java`nástroji je kód pro odhlášení uživatele v `initializeUI()`v obslužné rutině `signOutButton` Click.  Podepsání uživatele je asynchronní operace. Podepsáním uživatele se taky vymaže mezipaměť tokenu pro tento účet. Po odhlášení uživatelského účtu se vytvoří zpětné volání, které aktualizuje uživatelské rozhraní:

```java
mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
    @Override
    public void onSignOut() {
        updateUI(null);
        performOperationOnSignOut();
    }

    @Override
    public void onError(@NonNull MsalException exception) {
        displayError(exception);
    }
});
```

#### <a name="get-a-token-interactively-or-silently"></a>Interaktivní nebo tiché získání tokenu

K získání nejmenšího počtu výzev uživateli obvykle obdržíte token v tichém režimu. Pokud dojde k chybě, pokusí se získat interaktivní token. Při prvním volání `signIn()`aplikace bude efektivně fungovat jako volání `acquireToken()`, které uživateli vyzve k zadání přihlašovacích údajů.

Některé situace, kdy se uživatel může zobrazit výzva k výběru účtu, zadání přihlašovacích údajů nebo vyjádření souhlasu s oprávněními, které vaše aplikace požaduje:

* Když se uživatel poprvé přihlásí k aplikaci
* Pokud uživatel resetuje heslo, bude muset zadat své přihlašovací údaje.
* Pokud je souhlas odvolán
* Pokud vaše aplikace explicitně vyžaduje souhlas
* Když vaše aplikace požaduje při prvním pokusu o přístup k prostředku
* Pokud jsou vyžadovány MFA nebo jiné zásady podmíněného přístupu

Kód pro interaktivní získání tokenu, který je s uživatelským ROZHRANÍm, které bude zahrnovat uživatele, je v `SingleAccountModeFragment.java`, v `initializeUI()`, v obslužné `callGraphApiInteractiveButton` rutině Click:

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 **/
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Pokud se uživatel už přihlásil, `acquireTokenSilentAsync()` umožňuje aplikacím, aby požádaly o tokeny v tichém režimu, jak je znázorněno v `initializeUI()`, v obslužné rutině `callGraphApiSilentButton` Click:

```java
/**
 * Once you've signed the user in,
 * you can perform acquireTokenSilent to obtain resources without interrupting the user.
 **/
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Načtení účtu

Kód pro načtení účtu je `SingleAccountModeFragment.java` v. `loadAccount()`  Načtení účtu uživatele je asynchronní operace, takže zpětná volání, která mají být zpracována, když se účet načte, změní nebo dojde k chybě, se předává do MSAL.  Následující kód také zpracovává `onAccountChanged()`, který nastane při odebrání účtu, uživatel se změní na jiný účet atd.

```java
private void loadAccount() {
    ...

    mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback() {
        @Override
        public void onAccountLoaded(@Nullable IAccount activeAccount) {
            // You can use the account data to update your UI or your app database.
            updateUI(activeAccount);
        }

        @Override
        public void onAccountChanged(@Nullable IAccount priorAccount, @Nullable IAccount currentAccount) {
            if (currentAccount == null) {
                // Perform a cleanup task as the signed-in account changed.
                performOperationOnSignOut();
            }
        }

        @Override
        public void onError(@NonNull MsalException exception) {
            displayError(exception);
        }
    });
```

#### <a name="call-microsoft-graph"></a>Microsoft Graph volání

Když je uživatel přihlášený, volání Microsoft Graph je provedeno prostřednictvím požadavku HTTP, `callGraphAPI()` který je definován v. `SingleAccountModeFragment.java` Tato funkce je obálkou, která zjednodušuje vzorek provedením některých úkolů, jako je získání přístupového tokenu `authenticationResult` z a balení volání MSGraphRequestWrapper, a zobrazení výsledků volání.

```java
private void callGraphAPI(final IAuthenticationResult authenticationResult) {
    MSGraphRequestWrapper.callGraphAPIUsingVolley(
            getContext(),
            graphResourceTextView.getText().toString(),
            authenticationResult.getAccessToken(),
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    /* Successfully called graph, process data and send to UI */
                    ...
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    ...
                }
            });
}
```

### <a name="auth_config_single_accountjson"></a>auth_config_single_account. JSON

Toto je konfigurační soubor pro aplikaci MSAL, která používá jediný účet.

Vysvětlení těchto polí najdete v tématu Vysvětlení [konfiguračního souboru pro Android MSAL](msal-configuration.md) .

Všimněte si přítomnosti `"account_mode" : "SINGLE"`, která nakonfiguruje tuto aplikaci tak, aby používala jediný účet.

`"client_id"`je předem nakonfigurovaný tak, aby používal registraci objektu aplikace, kterou Microsoft udržuje.
`"redirect_uri"`je předem nakonfigurovaný tak, aby používal podpisový klíč, který je k dispozici v ukázce kódu.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

### <a name="multipleaccountmodefragmentjava"></a>MultipleAccountModeFragment. Java

Tento soubor ukazuje, jak vytvořit MSAL aplikaci s více účty a volat rozhraní API pro Microsoft Graph.

Příkladem aplikace s více účty je e-mailová aplikace, která umožňuje pracovat s několika uživatelskými účty, jako je například pracovní účet a osobní účet.

#### <a name="multiple-account-msal-initialization"></a>Vícenásobná inicializace MSAL účtu

V `MultipleAccountModeFragment.java` souboru je v `onCreateView()`nástroji vytvořen objekt aplikace s více účty (`IMultipleAccountPublicClientApplication`) s použitím konfiguračních informací uložených v: `auth_config_multiple_account.json file`

```java
// Creates a PublicClientApplication object with res/raw/auth_config_multiple_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccounts();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

Vytvořený `MultipleAccountPublicClientApplication` objekt je uložen v proměnné členu třídy, aby jej bylo možné použít k interakci s knihovnou MSAL k získání tokenů a načtení a odebrání uživatelského účtu.

#### <a name="load-an-account"></a>Načtení účtu

Více aplikací účtu obvykle volá `getAccounts()` výběr účtu, který se má použít pro operace MSAL. Kód pro načtení účtu je v `MultipleAccountModeFragment.java` souboru v. `loadAccounts()`  Načtení účtu uživatele je asynchronní operace. Proto zpětné volání zpracovává situace při načtení účtu, změny nebo dojde k chybě.

```java
/**
 * Load currently signed-in accounts, if there's any.
 **/
private void loadAccounts() {
    if (mMultipleAccountApp == null) {
        return;
    }

    mMultipleAccountApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            // You can use the account data to update your UI or your app database.
            accountList = result;
            updateUI(accountList);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

#### <a name="get-a-token-interactively-or-silently"></a>Interaktivní nebo tiché získání tokenu

Některé situace, kdy se uživatel může zobrazit výzva k výběru účtu, zadání přihlašovacích údajů nebo vyjádření souhlasu s oprávněními, které vaše aplikace požaduje:

* Při prvním přihlášení uživatele k aplikaci
* Pokud uživatel resetuje heslo, bude muset zadat své přihlašovací údaje. 
* Pokud je souhlas odvolán 
* Pokud vaše aplikace explicitně vyžaduje souhlas 
* Když vaše aplikace požaduje při prvním pokusu o přístup k prostředku
* Pokud jsou vyžadovány MFA nebo jiné zásady podmíněného přístupu

K `acquireToken()`interaktivnímu získávání tokenů by měly být obvykle vyvolány aplikace s více účty, což je uživatelské rozhraní, které zahrnuje uživatele s voláním.  Kód pro interaktivní získání tokenu je v `MultipleAccountModeFragment.java` souboru v `initializeUI()`, v obslužné rutině `callGraphApiInteractiveButton` Click:

```java
/**
 * Acquire token interactively. It will also create an account object for the silent call as a result (to be obtained by getAccount()).
 *
 * If acquireTokenSilent() returns an error that requires an interaction,
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your SSO refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 **/
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Aplikace by neměly vyžadovat, aby se uživatelé přihlásili pokaždé, když požadují token. Pokud se `acquireTokenSilentAsync()` uživatel už přihlásil, umožňuje aplikacím žádat o tokeny bez zobrazení výzvy uživateli, jak je znázorněno v `MultipleAccountModeFragment.java` souboru`initializeUI()` v obslužné rutině `callGraphApiSilentButton` Click:

```java
/**
 * Performs acquireToken without interrupting the user.
 *
 * This requires an account object of the account you're obtaining a token for.
 * (can be obtained via getAccount()).
 */
mMultipleAccountApp.acquireTokenSilentAsync(getScopes(),
    accountList.get(accountListSpinner.getSelectedItemPosition()),
    AUTHORITY,
    getAuthSilentCallback());
```

#### <a name="remove-an-account"></a>Odebrat účet

Kód pro odebrání účtu a všechny tokeny uložené v mezipaměti pro účet jsou v `MultipleAccountModeFragment.java` souboru `initializeUI()` v obslužné rutině pro tlačítko Odebrat účet. Než budete moct účet odebrat, potřebujete objekt účtu, který získáte z metod MSAL jako `getAccounts()` a. `acquireToken()` Vzhledem k tomu, že odebrání účtu je asynchronní operace `onRemoved` , je k dispozici zpětné volání pro aktualizaci uživatelského rozhraní.

```java
/**
 * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
 **/
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccounts();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>auth_config_multiple_account. JSON

Toto je konfigurační soubor pro aplikaci MSAL, která používá více účtů.

Vysvětlení různých polí najdete v tématu Vysvětlení [konfiguračního souboru pro Android MSAL](msal-configuration.md) .

Na rozdíl od konfiguračního souboru [auth_config_single_account. JSON](#auth_config_single_accountjson) má `"account_mode" : "MULTIPLE"` tento konfigurační soubor místo, `"account_mode" : "SINGLE"` protože se jedná o aplikaci s více účty.

`"client_id"`je předem nakonfigurovaný tak, aby používal registraci objektu aplikace, kterou Microsoft udržuje.
`"redirect_uri"`je předem nakonfigurovaný tak, aby používal podpisový klíč, který je k dispozici v ukázce kódu.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "account_mode" : "MULTIPLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Další kroky

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Zjistěte, jak vytvořit aplikaci použitou v tomto rychlém startu

Vyzkoušejte si [uživatele přihlášení a zavolejte Microsoft Graph z kurzu aplikace pro Android](tutorial-v2-android.md) , kde najdete podrobný Průvodce vytvořením aplikace pro Android, která získá přístupový token a používá ho k volání rozhraní Microsoft Graph API.

> [!div class="nextstepaction"]
> [Kurz volání Graph API pro Android](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Wiki knihovny MSAL pro Android

Přečtěte si další informace o knihovně MSAL pro Android:

> [!div class="nextstepaction"]
> [Wiki knihovny MSAL pro Android](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
