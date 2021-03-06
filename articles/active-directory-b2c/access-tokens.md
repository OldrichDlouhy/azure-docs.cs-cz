---
title: Vyžádání přístupového tokenu – Azure Active Directory B2C | Microsoft Docs
description: Přečtěte si, jak požádat o přístupový token z Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/12/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: be43b74e7128f9b250d25f8bdb2642c6f7b41d2a
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87115538"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Vyžádání přístupového tokenu v Azure Active Directory B2C

*Přístupový token* obsahuje deklarace identity, které můžete použít v Azure Active Directory B2C (Azure AD B2C) k identifikaci udělených oprávnění k vašim rozhraním API. Při volání serveru prostředků musí být v požadavku HTTP přítomen přístupový token. Přístupový token je označený jako **access_token** v odpovědích od Azure AD B2C.

V tomto článku se dozvíte, jak požádat o přístupový token pro webovou aplikaci a webové rozhraní API. Další informace o tokenech v Azure AD B2C najdete v tématu [Přehled tokenů v Azure Active Directory B2C](tokens-overview.md).

> [!NOTE]
> **Služba Azure AD B2C nepodporuje řetězy webového rozhraní API (za běhu).** – Mnoho architektur zahrnuje webové rozhraní API, které potřebuje volat jiné webové rozhraní API pro příjem dat, jak je zabezpečené Azure AD B2C. Tento scénář je běžný u klientů, které mají back-end webového rozhraní API, který zase volá jinou službu. Tento scénář zřetězeného webového rozhraní API můžete podporovat pomocí udělení přihlašovacích údajů nosiče OAuth 2,0 JWT, jinak označovaného jako tok za běhu. Tok prováděný jménem se ale v Azure AD B2C v tuto chvíli neimplementuje.

## <a name="prerequisites"></a>Předpoklady

- [Vytvořte uživatelský tok](tutorial-create-user-flows.md) , který uživatelům umožní přihlásit se k aplikaci a přihlásit se k ní.
- Pokud jste to ještě neudělali, [přidejte aplikaci webového rozhraní API do tenanta Azure Active Directory B2C](add-web-api-application.md).

## <a name="scopes"></a>Obory

Obory poskytují způsob, jak spravovat oprávnění k chráněným prostředkům. Po vyžádání přístupového tokenu musí klientská aplikace zadat požadovaná oprávnění v parametru **Scope** žádosti. Pokud například chcete zadat **hodnotu oboru** `read` pro rozhraní API s **identifikátorem URI ID aplikace** `https://contoso.onmicrosoft.com/api` , bude obor `https://contoso.onmicrosoft.com/api/read` .

Webové rozhraní API používá obory k implementaci řízení přístupu na základě oboru. Například uživatelé webového rozhraní API můžou mít přístup ke čtení i zápisu nebo přístup pouze ke čtení. Chcete-li získat více oprávnění v rámci jedné žádosti, můžete přidat více položek v parametru s jedním **oborem** žádosti oddělené mezerami.

Následující příklad ukazuje obory Dekódovatelné v adrese URL:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

Následující příklad ukazuje obory kódované v adrese URL:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

Pokud požadujete více oborů, než je uděleno pro klientskou aplikaci, bude volání úspěšné, pokud je uděleno alespoň jedno oprávnění. Deklarace **spojovacího bodu** služby ve výsledném přístupovém tokenu se naplní pouze oprávněními, která byla úspěšně udělena. Standard OpenID Connect určuje několik speciálních hodnot oboru. Následující rozsahy reprezentují oprávnění pro přístup k profilu uživatele:

- **OpenID** – vyžádá token ID.
- **offline_access** – vyžádá obnovovací token pomocí [toků kódu ověřování](authorization-code-flow.md).

Pokud parametr **response_type** v `/authorize` požadavku obsahuje `token` , musí parametr **Scope** obsahovat alespoň jeden obor prostředků, než je `openid` a `offline_access` který bude udělen. V opačném případě se `/authorize` požadavek nezdařil.

## <a name="request-a-token"></a>Požádat o token

K vyžádání přístupového tokenu potřebujete autorizační kód. Níže je příklad požadavku na `/authorize` koncový bod pro autorizační kód. Vlastní domény se nepodporují pro použití s přístupovými tokeny. V adrese URL žádosti použijte svoji tenant-name.onmicrosoft.com doménu.

V následujícím příkladu nahradíte tyto hodnoty:

- `<tenant-name>`– Název vašeho tenanta Azure AD B2C.
- `<policy-name>`– Název vlastní zásady nebo tok uživatele.
- `<application-ID>`– Identifikátor aplikace webové aplikace, kterou jste zaregistrovali pro podporu toku uživatele.
- `<redirect-uri>`– **Identifikátor URI přesměrování** , který jste zadali při registraci klientské aplikace.

```http
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&response_type=code
```

Odpověď s autorizačním kódem by měla být podobná tomuto příkladu:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Po úspěšném přijetí autorizačního kódu ho můžete použít k vyžádání přístupového tokenu:

```http
POST <tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/token HTTP/1.1
Host: <tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id=<application-ID>
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
&redirect_uri=https://jwt.ms
&client_secret=2hMG2-_:y12n10vwH...
```

Mělo by se zobrazit něco podobného jako u následující odpovědi:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrN...",
    "token_type": "Bearer",
    "not_before": 1549647431,
    "expires_in": 3600,
    "expires_on": 1549651031,
    "resource": "f2a76e08-93f2-4350-833c-965c02483b11",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiJjNjRhNGY3ZC0zMDkxLTRjNzMtYTcyMi1hM2YwNjk0Z..."
}
```

Při použití nástroje https://jwt.ms k prohlédnutí vráceného přístupového tokenu by se měla zobrazit podobný příklad jako v následujícím příkladu:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dl..."
}.{
  "iss": "https://contoso0926tenant.b2clogin.com/c64a4f7d-3091-4c73-a7.../v2.0/",
  "exp": 1549651031,
  "nbf": 1549647431,
  "aud": "f2a76e08-93f2-4350-833c-965...",
  "oid": "1558f87f-452b-4757-bcd1-883...",
  "sub": "1558f87f-452b-4757-bcd1-883...",
  "name": "David",
  "tfp": "B2C_1_signupsignin1",
  "nonce": "anyRandomValue",
  "scp": "read",
  "azp": "38307aee-303c-4fff-8087-d8d2...",
  "ver": "1.0",
  "iat": 1549647431
}.[Signature]
```

## <a name="next-steps"></a>Další kroky

- Přečtěte si, jak [nakonfigurovat tokeny v Azure AD B2C](configure-tokens.md)
