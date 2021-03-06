---
title: Microsoft Graph API
description: Rozhraní Microsoft Graph API je webové rozhraní API RESTful, které umožňuje přístup k prostředkům služby Microsoft Cloud.
author: davidmu1
services: active-directory
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 02/13/2020
ms.author: davidmu
ms.custom: aaddev
ms.openlocfilehash: 0cdcb5287434c72bf54337611d67de8d6f65d8d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85479508"
---
# <a name="microsoft-graph-api"></a>Microsoft Graph API

Rozhraní Microsoft Graph API je webové rozhraní API RESTful, které umožňuje přístup k prostředkům služby Microsoft Cloud. Po registraci aplikace a získání ověřovacích tokenů pro uživatele nebo službu můžete vytvořit požadavky na rozhraní Microsoft Graph API. Další informace najdete v tématu [přehled Microsoft Graph](https://docs.microsoft.com/graph/overview).

Microsoft Graph zpřístupňuje rozhraní REST API a klientské knihovny pro přístup k datům v následujících Microsoft 365 službách:
- Služby Office 365: Delvu, Excel, Microsoft Books, Microsoft teams, OneDrive, OneNote, Outlook/Exchange, Planner a SharePoint
- Enterprise mobility and Security Services: Advanced Threat Analytics, Rozšířená ochrana před internetovými útoky, Azure Active Directory, Identity Manager a Intune
- Služby Windows 10: aktivity, zařízení, oznámení
- Dynamics 365 Business Central

## <a name="versions"></a>Verze

Microsoft Graph aktuálně podporuje dvě verze: v 1.0 a beta. Verze v 1.0 zahrnuje všeobecně dostupná rozhraní API. Pro všechny produkční aplikace použijte verzi v 1.0. Beta verze obsahuje rozhraní API, která jsou momentálně ve verzi Preview. Vzhledem k tomu, že můžeme zavést zásadní změny v našich rozhraních API beta verze, doporučujeme, abyste používali beta verzi pouze pro testovací aplikace, které jsou ve vývoji. Nepoužívejte beta rozhraní API v produkčních aplikacích. Další informace najdete v tématu [Správa verzí, podpora a přerušující zásady změny pro Microsoft Graph](https://docs.microsoft.com/graph/versioning-and-support).

Pokud chcete začít používat beta rozhraní API, přečtěte si téma [referenční informace o Microsoft Graph beta verzi](https://docs.microsoft.com/graph/api/overview?view=graph-rest-beta)

Pokud chcete začít používat rozhraní API v 1.0, přečtěte si referenční informace o [Microsoft Graph REST API v 1.0](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0) .

## <a name="get-started"></a>Začínáme

Chcete-li číst nebo zapisovat do prostředku, jako je například uživatel nebo e-mailová zpráva, vytvoříte požadavek, který bude vypadat nějak takto:

`{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}`

Další informace o prvcích vytvořené žádosti najdete v tématu [použití rozhraní Microsoft Graph API](https://docs.microsoft.com/graph/use-the-api) .

Ukázky pro rychlý Start jsou k dispozici, abyste viděli, jak získat přístup k výkonu rozhraní Microsoft Graph API. Příklady, které jsou k dispozici, mají přístup ke dvěma službám s jedním ověřováním: účet Microsoft a Outlook. Každý rychlý Start má přístup k informacím z profilů účet Microsoft uživatelů a zobrazuje události ze svého kalendáře.
Rychlé starty obsahují čtyři kroky:
- Výběr platformy
- Získat ID aplikace (ID klienta)
- Sestavení ukázky
- Přihlaste se a Prohlédněte si události v kalendáři.

Po dokončení rychlého startu máte aplikaci, která je připravená ke spuštění. Další informace najdete v [nejčastějších dotazech k rychlému startu Microsoft Graph](https://docs.microsoft.com/graph/quick-start-faq). Pokud chcete začít s ukázkami, přečtěte si téma [rychlý start Microsoft Graph](https://developer.microsoft.com/graph/quick-start).

## <a name="tools"></a>nástroje

Microsoft Graph Explorer je webový nástroj, který můžete použít k sestavování a testování požadavků pomocí rozhraní Microsoft Graph API. K aplikaci Microsoft Graph Explorer máte přístup v umístění: `https://developer.microsoft.com/graph/graph-explorer` .

Metoda post je nástroj, který můžete použít také k sestavení a testování požadavků pomocí Microsoft Graph rozhraní API. Můžete si stáhnout tento příspěvek v: `https://www.getpostman.com/` . Pokud chcete pracovat s Microsoft Graph v nástroji post, použijete Microsoft Graph kolekci v poli post. Další informace najdete v tématu [použití metody post s rozhraním Microsoft Graph API](/graph/use-postman?context=graph%2Fapi%2Fbeta&view=graph-rest-beta).
