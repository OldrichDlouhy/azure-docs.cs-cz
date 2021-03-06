---
title: Ukázkové šablony Azure Resource Manageru – Event Grid | Microsoft Docs
description: Tento článek poskytuje seznam ukázek šablon Azure Resource Manager pro Azure Event Grid na GitHubu.
ms.topic: sample
ms.date: 07/07/2020
ms.openlocfilehash: 910012adf2dc930e6f1a26f1a7fc41f5ed0580c9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119051"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Šablony Azure Resource Manageru pro Event Grid

Informace o syntaxi a vlastnostech JSON pro použití v šabloně najdete v tématu [typy prostředků Microsoft. EventGrid](/azure/templates/microsoft.eventgrid/allversions). Následující tabulka obsahuje odkazy na šablony Azure Resource Manageru pro Event Grid.

## <a name="event-grid-subscriptions"></a>Předplatná Event Grid
- [Vlastní téma a předplatné s koncovým bodem Webhooku](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) – nasadí vlastní téma Event Grid. Vytvoří odběr tohoto vlastního tématu, který využívá koncový bod webhooku. 
- [Předplatné vlastního tématu s koncovým bodem EventHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler) – vytvoří Event Grid předplatné pro vlastní téma. Odběr jako koncový bod využívá centrum událostí. 
- Předplatné [Azure nebo předplatné skupiny prostředků](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) – přihlásí se k odběru událostí pro skupinu prostředků nebo předplatné Azure. Zdrojem událostí je skupina prostředků, kterou při nasazování zadáte jako cíl. Odběr jako koncový bod využívá Webhook. 
- [Účet úložiště BLOB a předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) – nasadí účet úložiště objektů BLOB v Azure a přihlásí se k odběru událostí pro tento účet úložiště. 

## <a name="next-steps"></a>Další kroky
Podívejte se na následující ukázky:

- [Ukázky pro PowerShell](powershell-samples.md)
- [Ukázky rozhraní příkazového řádku](cli-samples.md)
