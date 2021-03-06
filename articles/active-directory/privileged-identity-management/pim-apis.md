---
title: Rozhraní API pro Microsoft Graph pro PIM (Preview) – Azure AD | Microsoft Docs
description: Poskytuje informace o použití rozhraní API Microsoft Graph pro Azure AD Privileged Identity Management (PIM) (Preview).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: how-to
ms.date: 01/02/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea3e307df95fe4cb18986ed8e68a7f3381c5fbe7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742891"
---
# <a name="microsoft-graph-apis-for-privileged-identity-management-preview"></a>Rozhraní API pro Microsoft Graph pro Privileged Identity Management (Preview)

Privileged Identity Management úlohy můžete provádět pomocí [rozhraní api Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/overview) pro Azure Active Directory. Tento článek popisuje důležité koncepty pro používání rozhraní Microsoft Graph API pro Privileged Identity Management.

Podrobnosti o rozhraních API Microsoft Graph najdete v referenčních informacích k [rozhraní api Azure AD Privileged Identity Management](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root).

> [!IMPORTANT]
> Rozhraní API pod verzí/beta v Microsoft Graph jsou ve verzi Preview a mohou se měnit. Použití těchto rozhraní API v produkčních aplikacích není podporováno.

## <a name="required-permissions"></a>Požadovaná oprávnění

Chcete-li volat rozhraní API Microsoft Graph pro Privileged Identity Management, je nutné mít **minimálně jedno** z následujících oprávnění:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Nastavení oprávnění

Aby mohly aplikace volat rozhraní API Microsoft Graph pro Privileged Identity Management, musí mít požadovaná oprávnění. Nejjednodušším způsobem, jak zadat požadovaná oprávnění, je použít [rozhraní pro vyjádření souhlasu Azure AD](../develop/consent-framework.md).

### <a name="set-permissions-in-graph-explorer"></a>Nastavení oprávnění v Graph Exploreru

Pokud k otestování volání používáte Průzkumníka grafů, můžete zadat oprávnění v nástroji.

1. Přihlaste se k [aplikaci Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) jako globální správce.

1. Klikněte na **Upravit oprávnění**.

    ![Průzkumník grafů – Změna oprávnění](./media/pim-apis/graph-explorer.png)

1. Zaškrtněte políčka vedle oprávnění, která chcete zahrnout. `PrivilegedAccess.ReadWrite.AzureAD`není zatím k dispozici v Graph Exploreru.

    ![Průzkumník grafů – Změna oprávnění](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klikněte na **Upravit oprávnění** , aby se změny oprávnění projevily.

## <a name="next-steps"></a>Další kroky

- [Reference k rozhraní API Azure AD Privileged Identity Management](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
