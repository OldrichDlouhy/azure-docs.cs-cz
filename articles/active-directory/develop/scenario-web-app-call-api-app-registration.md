---
title: Registrace webové aplikace, která volá webová rozhraní API – Microsoft Identity Platform | Azure
description: Zjistěte, jak zaregistrovat webovou aplikaci, která volá webová rozhraní API.
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
ms.openlocfilehash: 8cb7d86bd419563363779c499962c81f0c59e3b6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80881872"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Webová aplikace, která volá webová rozhraní API: registrace aplikace

Webová aplikace, která volá webová rozhraní API, má stejnou registraci jako webová aplikace, která uživatele podepisuje. Postupujte proto podle pokynů ve [webové aplikaci, která se přihlásí uživatelům: registrace aplikace](scenario-web-app-sign-user-app-registration.md).

Vzhledem k tomu, že webová aplikace teď také volá webová rozhraní API, se z nich stala důvěrná klientská aplikace. To je důvod, proč je nutné provést další registraci. Aplikace musí sdílet přihlašovací údaje klienta nebo *tajné klíče*s platformou Microsoft identity.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Oprávnění rozhraní API

Webové aplikace volají rozhraní API jménem přihlášeného uživatele. Aby to bylo možné, musí požádat o *delegovaná oprávnění*. Podrobnosti najdete v tématu [Přidání oprávnění pro přístup k webovým rozhraním API](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Webová aplikace, která volá webová rozhraní API: Konfigurace kódu](scenario-web-app-call-api-app-configuration.md)
