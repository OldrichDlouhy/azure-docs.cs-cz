---
title: Vytvoření tenanta Azure pro aplikaci s více klienty
description: Pokyny pro nezávislé výrobce softwaru při integraci s Azure Active Directory
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: kenwith
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a0b63c130d7d1e72bd3320e40213ae3cb1069a6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763240"
---
# <a name="create-an-azure-tenant-for-a-multi-tenant-application"></a>Vytvoření tenanta Azure pro aplikaci s více klienty  

Aby bylo možné zajistit přístup k aplikaci s více klienty, je nutné vytvořit klienta Azure Active Directory pro registraci aplikace a povolení federace identit zákazníka. Podívejte [se na téma Volba správného federačního protokolu pro vaši aplikaci s více klienty](isv-choose-multi-tenant-federation.md). Tento tenant vám umožní testovat vaši aplikaci a federaci v prostředí, které je podobné vašim zákazníkům v prostředích Azure AD.

## <a name="costs-of-hosting-a-multi-tenant-application"></a>Náklady na hostování aplikace s více klienty

Azure Active Directory je k dispozici v několika edicích. [Podrobné porovnání funkcí najdete v tématu](https://azure.microsoft.com/pricing/details/active-directory/).

Můžete si vytvořit předplatné Azure a službu Azure Active Directory zdarma a používat základní funkce.

## <a name="create-your-tenant"></a>Vytvoření tenanta

1. Vytvořte svého tenanta. Viz [Nastavení vývojového prostředí](../develop/quickstart-create-new-tenant.md).

2. Povolení a testování přístupu jednotného přihlašování k aplikaci,

   a. V **případě aplikací OIDC nebo Oath** [Zaregistrujte svoji aplikaci](../develop/quickstart-register-app.md) jako víceklientské aplikace. V části Podporované typy účtů vyberte účty v možnosti organizační adresář a osobní účet Microsoft.

   b. **Pro aplikace založené na SAML a WS-based**můžete [nakonfigurovat jednotné přihlašování založené na SAML](configure-single-sign-on-non-gallery-applications.md) pomocí obecné šablony SAML v Azure AD.

V případě potřeby můžete také [převést aplikaci s jedním klientem na více tenantů](../develop/howto-convert-app-to-be-multi-tenant.md) .

## <a name="next-steps"></a>Další kroky

[Integrace jednotného přihlašování do aplikace](isv-sso-content.md)
