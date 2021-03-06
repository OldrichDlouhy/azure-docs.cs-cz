---
title: Pomocí Azure Portal můžete vytvářet Service Bus témata a předplatná.
description: 'Rychlý Start: v tomto rychlém startu se dozvíte, jak vytvořit Service Bus téma a odběry k tomuto tématu pomocí Azure Portal.'
author: spelluru
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: fc84b9ec9cc71d35e31a5c11ae0dd0d2228621da
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/24/2020
ms.locfileid: "85341061"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Rychlý Start: pomocí Azure Portal vytvořit Service Bus téma a odběry tématu
V tomto rychlém startu použijete Azure Portal k vytvoření Service Bus tématu a pak vytvoříte odběry k tomuto tématu. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co jsou témata a předplatné služby Service Bus?
Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*. Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Na rozdíl od Service Busch front, ve kterých každá zpráva zpracovává jeden uživatel, témata a odběry poskytují formu komunikace 1: n pomocí vzoru pro publikování a odběr. K jednomu tématu můžete zaregistrovat několik předplatných. Při odeslání zprávy do tématu bude zpráva zpřístupněná všem předplatným, aby ji nezávisle zpracovala. Předplatné tématu se podobá virtuální frontě, která obdrží kopii zpráv, které byly odeslány do tématu. Volitelně můžete zaregistrovat pravidla filtru pro téma na základě jednotlivých předplatných, což umožňuje filtrovat nebo omezit, které zprávy na téma jsou přijímány v rámci předplatných tématu.

Service Bus témata a předplatná umožňují škálovat na zpracování velkého počtu zpráv napříč velkým počtem uživatelů a aplikací.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]

> [!NOTE]
> Prostředky Service Bus můžete spravovat pomocí [Service Bus Exploreru](https://github.com/paolosalvatori/ServiceBusExplorer/). Service Bus Explorer umožňuje uživatelům připojit se k oboru názvů Service Bus a snadno spravovat entity zasílání zpráv. Tento nástroj poskytuje pokročilé funkce, jako jsou funkce importu a exportu, nebo možnost testovat témata, fronty, odběry, služby Relay, centra oznámení a centra událostí. 

## <a name="next-steps"></a>Další kroky
Informace o tom, jak odesílat zprávy do tématu a přijímat tyto zprávy prostřednictvím předplatného, najdete v následujícím článku: vyberte programovací jazyk v obsahu. 

> [!div class="nextstepaction"]
> [Publikování a přihlášení k odběru zpráv](service-bus-dotnet-how-to-use-topics-subscriptions.md)
