---
title: Řešení chyb AMQP v Azure Service Bus | Microsoft Docs
description: Poskytuje seznam AMQPch chyb, které se mohou zobrazit při použití Azure Service Bus, a příčině těchto chyb.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 9680e930dd8c1cb8cbd062f029af9d674d62c0e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85337825"
---
# <a name="amqp-errors-in-azure-service-bus"></a>AMQP chyby v Azure Service Bus
Tento článek popisuje některé chyby, které obdržíte při použití AMQP s Azure Service Bus. Jsou to všechna standardní chování služby. Můžete se jim vyhnout tím, že v připojení nebo propojení vytvoříte volání pro odesílání a přijímání, které automaticky znovu vytvoří připojení nebo propojení.

## <a name="link-is-closed"></a>Odkaz je uzavřený. 
Při aktivním připojení AMQP a propojení se zobrazí následující chyba, ale pomocí odkazu na 10 minut se neuskuteční žádná volání (například odeslat nebo přijmout). Proto je odkaz uzavřený. Připojení je stále otevřené.

```
amqp:link:detach-forced:The link 'G2:7223832:user.tenant0.cud_00000000000-0000-0000-0000-00000000000000' is force detached by the broker due to errors occurred in publisher(link164614). Detach origin: AmqpMessagePublisher.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:00000000000000000000000000000000000000_G2_B3, SystemTracker:mynamespace:Topic:MyTopic, Timestamp:2/16/2018 11:10:40 PM
```

## <a name="connection-is-closed"></a>Připojení je zavřené.
Na připojení AMQP se zobrazí následující chyba, když jsou všechna propojení v připojení zavřena, protože nebyla žádná aktivita (nečinný) a nové propojení nebylo vytvořeno za 5 minut.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 300000 milliseconds and is closed by container 'LinkTracker'. TrackingId:00000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T17:32:00', info=null}
```

## <a name="link-is-not-created"></a>Odkaz není vytvořen. 
Tato chyba se zobrazí, když se vytvoří nové připojení AMQP, ale odkaz se nevytvoří během 1 minuty od vytvoření připojení AMQP.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 60000 milliseconds and is closed by container 'LinkTracker'. TrackingId:0000000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T18:41:51', info=null}
```

## <a name="next-steps"></a>Další kroky

Další informace o AMQP a Service Bus najdete na následujících odkazech:

* [Přehled Service Bus AMQP]
* [Průvodce protokolem AMQP 1.0]
* [AMQP v Service Bus pro Windows Server]

[Přehled Service Bus AMQP]: service-bus-amqp-overview.md
[Průvodce protokolem AMQP 1.0]: service-bus-amqp-protocol-guide.md
[AMQP v Service Bus pro Windows Server]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
