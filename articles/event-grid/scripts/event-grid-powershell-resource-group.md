---
title: Ukázkový skript Azure PowerShellu – Přihlášení k odběru skupiny prostředků | Microsoft Docs
description: Tento článek obsahuje ukázkový skript Azure PowerShell, který ukazuje, jak se přihlásit k odběru Event Grid událostí pro skupinu prostředků.
ms.devlang: powershell
ms.topic: sample
ms.date: 07/08/2020
ms.openlocfilehash: 82f40b9971d89d96500887e2927a7b86246ce74d
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171189"
---
# <a name="subscribe-to-events-for-a-resource-group-with-powershell"></a>Přihlášení k odběru událostí skupiny prostředků pomocí PowerShellu

Tento skript vytvoří odběr Event Gridu pro události skupiny prostředků.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Vzorový skript ve verzi Preview vyžaduje modul Event Grid. Chcete-li nainstalovat, spusťte příkaz`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

## <a name="sample-script---stable"></a>Ukázkový skript – stabilní

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.ps1 "Subscribe to resource group")]

## <a name="sample-script---preview-module"></a>Vzorový modul Script-Preview

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.ps1 "Subscribe to resource group")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k vytvoření odběru událostí používá následující příkaz. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Vytvoří odběr Event Gridu. |

## <a name="next-steps"></a>Další kroky

* Úvod ke spravovaným aplikacím najdete v [přehledu spravovaných aplikací Azure](../overview.md).
* Další informace o PowerShellu najdete v [dokumentaci k Azure PowerShellu](https://docs.microsoft.com/powershell/azure/get-started-azureps).
