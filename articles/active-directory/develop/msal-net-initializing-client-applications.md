---
title: Inicializovat klientské aplikace MSAL.NET | Azure
titleSuffix: Microsoft identity platform
description: Seznamte se s inicializací veřejných klientských a důvěrných klientských aplikací pomocí knihovny Microsoft Authentication Library pro .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/12/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 57ce6ab31421cd4016f7e204eeabce82f2f7e6a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77083982"
---
# <a name="initialize-client-applications-using-msalnet"></a>Inicializace klientských aplikací pomocí MSAL.NET
Tento článek popisuje inicializaci veřejného klienta a důvěrných klientských aplikací pomocí knihovny Microsoft Authentication Library pro .NET (MSAL.NET).  Další informace o typech klientských aplikací a možnostech konfigurace aplikací najdete v [přehledu](msal-client-applications.md).

Pomocí MSAL.NET 3. x je doporučený způsob vytvoření instance aplikace pomocí tvůrců aplikací: `PublicClientApplicationBuilder` a `ConfidentialClientApplicationBuilder` . Nabízí účinný mechanismus pro konfiguraci aplikace buď z kódu, nebo z konfiguračního souboru, nebo i smícháním obou přístupů.

## <a name="prerequisites"></a>Požadavky
Před inicializací aplikace je nejprve nutné [ji zaregistrovat](quickstart-register-app.md) , aby bylo možné aplikaci integrovat s platformou Microsoft identity.  Po registraci možná budete potřebovat následující informace (které najdete v Azure Portal):

- ID klienta (řetězec představující GUID)
- Adresa URL zprostředkovatele identity (pojmenovaná instance) a cílová skupina pro přihlášení k vaší aplikaci. Tyto dva parametry jsou souhrnně známé jako autorita.
- ID tenanta, pokud píšete obchodní aplikaci výhradně pro vaši organizaci (nazývá se jenom jediná aplikace tenanta).
- Tajný klíč aplikace (tajný řetězec klienta) nebo certifikát (typu X509Certificate2), pokud se jedná o důvěrnou klientskou aplikaci.
- Pro webové aplikace a někdy pro veřejné klientské aplikace (zejména v případě, že vaše aplikace potřebuje použít zprostředkovatele) nastavíte také redirectUri, kde bude poskytovatel identity kontaktovat zpět vaší aplikaci pomocí tokenů zabezpečení.

## <a name="ways-to-initialize-applications"></a>Způsoby inicializace aplikací
Existuje mnoho různých způsobů, jak vytvořit instanci klientských aplikací.

### <a name="initializing-a-public-client-application-from-code"></a>Inicializace veřejné klientské aplikace z kódu

Následující kód vytvoří instanci veřejné klientské aplikace, přihlašuje uživatele ve veřejném cloudu Microsoft Azure, se svými pracovními a školními účty nebo jejich osobními účty Microsoft.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-code"></a>Inicializace důvěrné klientské aplikace z kódu

Stejným způsobem následující kód vytvoří instanci důvěrné aplikace (webová aplikace, která se nachází v rámci `https://myapp.azurewebsites.net` ) zpracování tokenů od uživatelů ve veřejném cloudu Microsoft Azure, s pracovními a školními účty nebo jejich osobními účty Microsoft. Aplikace je identifikována poskytovatelem identity sdílením tajného klíče klienta:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

Jak můžete v produkčním prostředí znát místo používání tajného klíče klienta, možná budete chtít sdílet s Azure AD a certifikátem. Kód by pak byl následující:

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithCertificate(certificate)
    .WithRedirectUri(redirectUri )
    .Build();
```

### <a name="initializing-a-public-client-application-from-configuration-options"></a>Inicializace veřejné klientské aplikace z možností konfigurace

Následující kód vytvoří instanci veřejné klientské aplikace z konfiguračního objektu, který může být vyplněn programově nebo načten z konfiguračního souboru:

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-configuration-options"></a>Inicializace důvěrné klientské aplikace z možností konfigurace

Stejný druh vzoru se vztahuje na důvěrné klientské aplikace. Můžete také přidat další parametry pomocí `.WithXXX` modifikátorů (tady je certifikát).

```csharp
ConfidentialClientApplicationOptions options = GetOptions(); // your own method
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(options)
    .WithCertificate(certificate)
    .Build();
```

## <a name="builder-modifiers"></a>Tvůrce – modifikátory

V fragmentech kódu pomocí tvůrců aplikací `.With` lze použít několik metod jako modifikátory (například `.WithCertificate` a `.WithRedirectUri` ). 

### <a name="modifiers-common-to-public-and-confidential-client-applications"></a>Modifikátory společné pro veřejné a důvěrné klientské aplikace

Modifikátory, které můžete nastavit ve veřejném klientovi nebo v nástroji pro vytváření důvěrných klientských aplikací, jsou tyto:

|Modifikátor | Description|
|--------- | --------- |
|`.WithAuthority()`7 přepsání | Nastaví výchozí autoritu aplikace na autoritu Azure AD s možností výběru cloudu Azure, cílové skupiny, tenanta (ID tenanta nebo názvu domény) nebo poskytnutí přímo identifikátoru URI autority.|
|`.WithAdfsAuthority(string)` | Nastaví výchozí autoritu aplikace jako autoritu služby ADFS.|
|`.WithB2CAuthority(string)` | Nastaví výchozí autoritu aplikace jako autoritu Azure AD B2C.|
|`.WithClientId(string)` | Přepíše ID klienta.|
|`.WithComponent(string)` | Nastaví název knihovny pomocí MSAL.NET (z důvodů telemetrie). |
|`.WithDebugLoggingCallback()` | Při volání aplikace bude volána `Debug.Write` jednoduše povolení trasování ladění. Další informace najdete v tématu [protokolování](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) .|
|`.WithExtraQueryParameters(IDictionary<string,string> eqp)` | Nastavte dodatečné parametry dotazu na úrovni aplikace, které budou odeslány ve všech žádostech o ověření. To je přepsatelné na každé úrovni metody získání tokenu (se stejným `.WithExtraQueryParameters pattern` ).|
|`.WithHttpClientFactory(IMsalHttpClientFactory httpClientFactory)` | Povoluje pokročilé scénáře, jako je například konfigurace proxy serveru HTTP, nebo vynucení MSAL používání konkrétního HttpClient (například v ASP.NET Core Web Apps/rozhraní API).|
|`.WithLogging()` | Při volání aplikace bude volat zpětné volání s trasováním ladění. Další informace najdete v tématu [protokolování](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) .|
|`.WithRedirectUri(string redirectUri)` | Přepíše výchozí identifikátor URI přesměrování. V případě veřejných klientských aplikací to bude užitečné pro scénáře týkající se zprostředkovatele.|
|`.WithTelemetry(TelemetryCallback telemetryCallback)` | Nastaví delegáta použitý k odeslání telemetrie.|
|`.WithTenantId(string tenantId)` | Přepíše ID tenanta nebo popis tenanta.|

### <a name="modifiers-specific-to-xamarinios-applications"></a>Modifikátory specifické pro aplikace Xamarin. iOS

Modifikátory, které můžete nastavit na tvůrci veřejné klientské aplikace v Xamarin. iOS, jsou:

|Modifikátor | Description|
|--------- | --------- |
|`.WithIosKeychainSecurityGroup()` | **Jenom Xamarin. iOS**: nastaví skupinu zabezpečení řetězu klíčů pro iOS (pro trvalost mezipaměti).|

### <a name="modifiers-specific-to-confidential-client-applications"></a>Modifikátory specifické pro důvěrné klientské aplikace

V Tvůrci důvěrných klientských aplikací můžete nastavit Modifikátory:

|Modifikátor | Description|
|--------- | --------- |
|`.WithCertificate(X509Certificate2 certificate)` | Nastaví certifikát identifikující aplikaci pomocí Azure AD.|
|`.WithClientSecret(string clientSecret)` | Nastaví tajný klíč klienta (heslo aplikace) identifikující aplikaci pomocí Azure AD.|

Tyto modifikátory se vzájemně vylučují. Pokud zadáte obě, MSAL vyvolá smysluplnou výjimku.

### <a name="example-of-usage-of-modifiers"></a>Příklad použití modifikátorů

Předpokládejme, že vaše aplikace je obchodní aplikace, která je určena pouze pro vaši organizaci.  Pak můžete napsat:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzurePublic, tenantId)
        .Build();
```

Tam, kde se to bude zajímavé, je teď zjednodušené programování pro národní cloudy. Pokud chcete, aby vaše aplikace byla víceklientské aplikace v národním cloudu, můžete napsat například:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment, AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

K dispozici je také přepsání služby ADFS (ADFS 2019 není aktuálně podporováno):
```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Nakonec, pokud jste vývojář Azure AD B2C, můžete určit svého tenanta takto:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```
