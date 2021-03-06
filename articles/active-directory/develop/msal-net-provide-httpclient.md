---
title: Zadejte proxy server HttpClient & (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Přečtěte si, jak poskytnout vlastní HttpClient a proxy server pro připojení k Azure AD pomocí knihovny Microsoft Authentication Library pro .NET (MSAL.NET).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: e13c4ed472535d2c02734949bf72a5ed42108e7d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477324"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>Poskytování vlastních HttpClient a proxy serveru pomocí MSAL.NET
Při [inicializaci veřejné klientské aplikace](msal-net-initializing-client-applications.md)můžete použít `.WithHttpClientFactory method` k poskytnutí vlastní HttpClient.  Poskytování vlastní HttpClient umožňuje pokročilým scénářům, jako je jemně odstupňovaná kontrola proxy serveru HTTP, přizpůsobení hlaviček uživatelských agentů nebo vynucení MSAL používání konkrétního HttpClient (například v ASP.NET Core Web Apps/rozhraní API).

## <a name="initialize-with-httpclientfactory"></a>Inicializovat pomocí HttpClientFactory
Následující příklad ukazuje vytvoření `HttpClientFactory` a následné inicializaci klientské aplikace s ním:

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient a Xamarin iOS
Při použití Xamarin iOS se doporučuje vytvořit `HttpClient` , který explicitně používá `NSURLSession` obslužnou rutinu založenou na systému iOS 7 a novějším. MSAL.NET automaticky vytvoří `HttpClient` , který používá systém `NSURLSessionHandler` iOS 7 a novější. Další informace najdete v dokumentaci k [Xamarin iOS pro HttpClient](/xamarin/cross-platform/macios/http-stack).