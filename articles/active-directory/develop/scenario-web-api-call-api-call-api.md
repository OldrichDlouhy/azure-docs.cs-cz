---
title: Webové rozhraní API, které volá webová rozhraní API – Microsoft Identity Platform | Azure
description: Naučte se vytvářet webové rozhraní API, které volá webová rozhraní API.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b756d7df03bd3c06b703617dbf84a194d716f1e3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026369"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Webové rozhraní API, které volá webová rozhraní API: volá rozhraní API.

Po vytvoření tokenu můžete zavolat chráněné webové rozhraní API. Provedete to z kontroleru vašeho webového rozhraní API.

## <a name="controller-code"></a>Kód kontroleru

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Následující kód pokračuje v ukázkovém kódu, který je zobrazen ve [webovém rozhraní API, které volá webová rozhraní API: získání tokenu pro aplikaci](scenario-web-api-call-api-acquire-token.md). Kód se volá v akcích řadičů rozhraní API. Volá rozhraní API pro příjem dat s názvem *ToDoList*.

Po získání tokenu ho použijte jako nosný token pro volání rozhraní API pro příjem dat.

```csharp
private async Task CallTodoListService(string accessToken)
{

// After the token has been returned by Microsoft Identity Web, add it to the HTTP authorization header before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

# <a name="java"></a>[Java](#tab/java)

Následující kód pokračuje v ukázkovém kódu, který je zobrazen ve [webovém rozhraní API, které volá webová rozhraní API: získání tokenu pro aplikaci](scenario-web-api-call-api-acquire-token.md). Kód se volá v akcích řadičů rozhraní API. Volá API MS Graph pro příjem dat.

Po získání tokenu ho použijte jako nosný token pro volání rozhraní API pro příjem dat.

```Java
private String callMicrosoftGraphMeEndpoint(String accessToken){
    RestTemplate restTemplate = new RestTemplate();

    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);

    headers.set("Authorization", "Bearer " + accessToken);

    HttpEntity<String> entity = new HttpEntity<>(null, headers);

    String result = restTemplate.exchange("https://graph.microsoft.com/v1.0/me", HttpMethod.GET,
            entity, String.class).getBody();

    return result;
}
```

# <a name="python"></a>[Python](#tab/python)
Ukázka demonstrující tento tok pomocí MSAL Python ještě není k dispozici.

---

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Webové rozhraní API, které volá webová rozhraní API: Přesun do produkčního prostředí](scenario-web-api-call-api-production.md)
