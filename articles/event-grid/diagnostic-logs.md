---
title: Azure Event Grid – protokoly diagnostiky pro témata nebo domény
description: Tento článek obsahuje koncepční informace o diagnostických protokolech k tématu a doméně služby Azure Event Grid.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: c8f6734d9d43887d0eb0bb90bb08f727732feac3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116739"
---
#  <a name="diagnostic-logs-for-azure-event-grid-topicsdomains"></a>Diagnostické protokoly pro Azure Event Grid témata/domény
Nastavení diagnostiky umožňuje Event Grid uživatelům zachytit a zobrazit protokoly **selhání publikování a doručení** v účtu úložiště, v centru událostí nebo v pracovním prostoru Log Analytics. Tento článek poskytuje schéma pro protokoly a příklad položky protokolu.


## <a name="schema-for-publishdelivery-failure-logs"></a>Schéma pro protokoly selhání publikování a doručení

| Název vlastnosti | Datový typ | Description |
| ------------- | --------- | ----------- | 
| Čas | DateTime | Čas, kdy byla položka protokolu vygenerována <p>**Příklad hodnoty:** 01-29-2020 09:52:02.700</p> |
| EventSubscriptionName | Řetězec | Název odběru události <p>**Ukázková hodnota:** "EVENTSUB1"</p> <p>Tato vlastnost existuje pouze pro protokoly selhání doručení.</p>  |
| Kategorie | Řetězec | Název kategorie protokolu <p>**Příklady hodnot:** "DeliveryFailures" nebo "PublishFailures" | 
| OperationName | Řetězec | Název operace provedené při výskytu chyby.<p>**Příklady hodnot:** "Doručení" pro chyby doručení. |
| Zpráva | Řetězec | Zpráva protokolu pro uživatele s vysvětlením důvodu selhání a dalších dalších podrobností. |
| ResourceId | Řetězec | ID prostředku pro téma nebo prostředek domény<p>**Příklady hodnot:**`/SUBSCRIPTIONS/SAMPLE-SUBSCRIPTION-ID/RESOURCEGROUPS/SAMPLE-RESOURCEGROUP/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/TOPIC1` |

## <a name="example"></a>Příklad

```json
{
    "time": "2019-11-01T00:17:13.4389048Z",
    "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
    "eventSubscriptionName": "SAMPLEDESTINATION",
    "category": "DeliveryFailures",
    "operationName": "Deliver",
    "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
}
```

## <a name="next-steps"></a>Další kroky
Informace o tom, jak povolit diagnostické protokoly pro témata nebo domény, najdete v tématu [Povolení diagnostických protokolů](enable-diagnostic-logs-topic.md).
