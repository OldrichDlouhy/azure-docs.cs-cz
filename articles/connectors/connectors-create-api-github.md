---
title: Přístup, sledování a správa úložiště GitHub
description: Umožňuje monitorovat události GitHubu a spravovat úložiště GitHub vytvořením automatizovaných pracovních postupů pomocí Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: b67d2e860e8c69f6f2f43441144ef2f60ed3a104
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75378445"
---
# <a name="monitor-and-manage-your-github-repo-by-using-azure-logic-apps"></a>Monitorování a správa úložiště GitHub pomocí Azure Logic Apps

GitHub je webová služba hostující úložiště Git, která nabízí všechny funkce distribuovaných revizí a správy zdrojového kódu (SCM) v Gitu a další funkce.

Pokud chcete začít s konektorem GitHubu, [vytvořte nejdřív aplikaci logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Vytvoření připojení k GitHubu

Pokud chcete použít konektor GitHubu v aplikaci logiky, musíte nejdřív vytvořit *připojení* a pak zadat podrobnosti těchto vlastností: 

| Vlastnost | Povinné | Popis | 
| -------- | -------- | ----------- | 
| Token | Yes | Zadejte svoje přihlašovací údaje GitHubu. |

Po vytvoření připojení můžete provést akce a naslouchat aktivačním událostem popsaným v tomto článku.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Referenční informace ke konektorům

Technické podrobnosti o aktivačních událostech, akcích a omezeních, které jsou popsány v popisu OpenAPI konektoru (dříve Swagger), najdete na [referenční stránce konektoru](/connectors/github/).

## <a name="next-steps"></a>Další kroky

* Další informace o dalších [konektorech Logic Apps](../connectors/apis-list.md)