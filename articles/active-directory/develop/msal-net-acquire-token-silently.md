---
title: Získání tokenu z mezipaměti (MSAL.NET)
titleSuffix: Microsoft identity platform
description: Přečtěte si, jak získat přístupový token v tichém režimu (z mezipaměti tokenu) pomocí knihovny Microsoft Authentication Library pro .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c4e60e7e6a16b3e526d2f1581bfa145b74e5da01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477494"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>Získání tokenu z mezipaměti tokenů pomocí MSAL.NET

Při získání přístupového tokenu pomocí knihovny Microsoft Authentication Library pro .NET (MSAL.NET) se token uloží do mezipaměti. Když aplikace potřebuje token, měla by nejdřív zavolat `AcquireTokenSilent` metodu pro ověření, jestli je přijatelný token v mezipaměti. V mnoha případech je možné získat další token s více rozsahy na základě tokenu v mezipaměti. Je také možné aktualizovat token, když se blíží vypršení platnosti (protože mezipaměť tokenů obsahuje také obnovovací token).

Doporučeným vzorem je nejprve zavolat `AcquireTokenSilent` metodu.  Pokud `AcquireTokenSilent` dojde k chybě, Získejte token pomocí jiných metod.

V následujícím příkladu se aplikace poprvé pokusí získat token z mezipaměti tokenu.  Pokud `MsalUiRequiredException` je vyvolána výjimka, aplikace získá token interaktivně. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
