---
title: Ověřit obory a chráněné webové rozhraní API pro obory a role aplikace | Azure
titleSuffix: Microsoft identity platform
description: Naučte se vytvářet chráněné webové rozhraní API a konfigurovat kód vaší aplikace.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 218c0bebee6ed1e36da747802ea5e94bcebf9d62
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026522"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>Chráněné webové rozhraní API: ověření oborů a rolí aplikací

Tento článek popisuje, jak můžete přidat autorizaci do webového rozhraní API. Tato ochrana zajišťuje, že rozhraní API je voláno pouze pomocí:

- Aplikace jménem uživatelů, kteří mají správné obory
- Démon aplikace, které mají správnou aplikační role

> [!NOTE]
> Fragmenty kódu v tomto článku se extrahují z následujících ukázek kódu na GitHubu:
>
> - [Přírůstkový kurz ASP.NET Core webového rozhraní API](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/master/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs)
> - [Ukázka webového rozhraní API v ASP.NET](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/master/TodoListService/Controllers/TodoListController.cs)

Chcete-li chránit ASP.NET nebo ASP.NET Core webového rozhraní API, je nutné přidat `[Authorize]` atribut do jedné z následujících položek:

- Kontroler, pokud chcete, aby všechny akce kontroleru byly chráněné
- Akce jednotlivého kontroleru pro vaše rozhraní API

```csharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Tato ochrana ale není dostatečná. Garantuje jenom ASP.NET a ASP.NET Core ověřit token. Vaše rozhraní API musí ověřit, že token používaný pro volání rozhraní API je požadován s očekávanými deklaracemi. Tyto deklarace identity vyžadují konkrétně ověření:

- *Rozsahy* , pokud je rozhraní API voláno jménem uživatele.
- *Role aplikace* , pokud je možné rozhraní API volat z aplikace typu démon.

## <a name="verify-scopes-in-apis-called-on-behalf-of-users"></a>Ověřit obory rozhraní API volaných jménem uživatelů

Pokud klientská aplikace volá vaše rozhraní API jménem uživatele, musí rozhraní API požádat o token nosiče, který má konkrétní obory pro rozhraní API. Další informace najdete v tématu [Konfigurace kódu | Nosný token](scenario-protected-web-api-app-configuration.md#bearer-token)

### <a name="net-core"></a>.NET Core

#### <a name="verify-the-scopes-on-each-controller-action"></a>Ověřte obory u každé akce kontroleru.

```csharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
         HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi)
        // Do the work and return the result.
        // ...
    }
...
}
```

`VerifyUserHasAnyAcceptedScope`Metoda dělá podobný postup jako u následujících kroků:

- Ověřte, jestli existuje deklarace identity s názvem `http://schemas.microsoft.com/identity/claims/scope` nebo `scp` .
- Ověřte, že deklarace identity má hodnotu, která obsahuje obor očekávaný rozhraním API.


#### <a name="verify-the-scopes-more-globally"></a>Ověřování oborů efektivněji

Definování podrobných oborů pro vaše webové rozhraní API a ověření oborů v každé akci kontroleru je doporučený postup. Je ale také možné ověřit obory na úrovni aplikace nebo řadiče pomocí ASP.NET Core. Podrobnosti najdete v dokumentaci k ASP.NET Core v dokumentaci k [ověřování na základě deklarací identity](https://docs.microsoft.com/aspnet/core/security/authorization/claims) .

### <a name="net-mvc"></a>.NET MVC

V případě ASP.NET jenom nahraďte `HttpContext.User` `ClaimsPrincipal.Current` argumentem a nahraďte typ deklarace `"http://schemas.microsoft.com/identity/claims/scope"` pomocí `"scp"` . Viz také fragment kódu dále v tomto článku.

## <a name="verify-app-roles-in-apis-called-by-daemon-apps"></a>Ověření aplikačních rolí v rozhraní API volaných aplikacemi démona

Pokud je vaše webové rozhraní API voláno [aplikací démona](scenario-daemon-overview.md), měla by tato aplikace vyžadovat oprávnění aplikace pro vaše webové rozhraní API. Jak je znázorněno v vystavování [oprávnění aplikace (aplikační role)](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles), vaše rozhraní API zpřístupňuje taková oprávnění. Jedním z příkladů je `access_as_application` aplikační role.

Teď musíte mít rozhraní API, abyste ověřili, že token, který obdrží, obsahuje `roles` deklaraci identity a že tato deklarace má očekávanou hodnotu. Ověřovací kód je podobný kódu, který ověřuje delegovaná oprávnění s tím rozdílem, že vaše akce kontroleru testuje role namísto oborů:

### <a name="aspnet-core"></a>Výsledek akce

```csharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

`ValidateAppRole`Metoda je definována v Microsoft. identity. Web v [RolesRequiredHttpContextExtensions.cs](https://github.com/AzureAD/microsoft-identity-web/blob/d2ad0f5f830391a34175d48621a2c56011a45082/src/Microsoft.Identity.Web/Resource/RolesRequiredHttpContextExtensions.cs#L28).

### <a name="aspnet-mvc"></a>ASP.NET MVC

```csharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage
        { StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found"
        });
    }
}
}
```

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Přijímají se tokeny jenom pro aplikace, pokud by webové rozhraní API mělo volat jenom aplikace typu démon.

Uživatelé můžou ve vzorcích přiřazení uživatelů také používat deklarace identity, jak je znázorněno v tématu [Postupy: Přidání rolí aplikace do aplikace a jejich přijetí v tokenu](howto-add-app-roles-in-azure-ad-apps.md). Pokud se role přiřazují oběma uživatelům, kontrola rolí umožní aplikacím přihlásit se jako uživatelé a uživatelé, aby se přihlásili jako aplikace. Doporučujeme, abyste pro uživatele a aplikace deklarovali různé role, aby nedocházelo k nejasnostem.

Pokud chcete, aby vaše webové rozhraní API volaly jenom aplikace typu démon, přidejte podmínku, která je tokenem jenom pro aplikace při ověřování role aplikace.

```csharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Kontrola inverzní podmínky umožňuje pouze aplikacím, které přihlásí uživatele k volání rozhraní API.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přesun do produkčního prostředí](scenario-protected-web-api-production.md)
