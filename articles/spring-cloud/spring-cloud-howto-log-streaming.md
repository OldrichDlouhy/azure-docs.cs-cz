---
title: Streamování protokolů aplikace Azure Spring Cloudu v reálném čase
description: Jak používat streamování protokolů k okamžitému zobrazení protokolů aplikací
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.custom: devx-track-java
ms.openlocfilehash: 82d820e676cb241198e7b412bad9602b5eb8109b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037334"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>Streamování protokolů aplikace Azure Spring Cloudu v reálném čase
Azure jaře Cloud umožňuje streamování protokolů v Azure CLI a získat tak řešení potíží v protokolech konzoly aplikací v reálném čase. Můžete také [analyzovat protokoly a metriky pomocí nastavení diagnostiky](./diagnostic-services.md).

## <a name="prerequisites"></a>Předpoklady

* Nainstalujte [rozšíření Azure CLI](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) pro jarní Cloud a minimální verzi 0.2.0.
* Instance **jarního cloudu Azure** se spuštěnou aplikací, například [jarní cloudová aplikace](./spring-cloud-quickstart-launch-app-cli.md).

> [!NOTE]
>  Rozšíření ASC CLI je aktualizované z verze 0.2.0 na 0.2.1. Tato změna má vliv na syntaxi příkazu pro streamování protokolů: `az spring-cloud app log tail` , který je nahrazen: `az spring-cloud app logs` . Příkaz: `az spring-cloud app log tail` bude v budoucí verzi zastaralá. Pokud jste používali verzi 0.2.0, můžete upgradovat na 0.2.1. Nejdřív odeberte starou verzi pomocí příkazu: `az extension remove -n spring-cloud` .  Pak nainstalujte 0.2.1 pomocí příkazu: `az extension add -n spring-cloud` .

## <a name="use-cli-to-tail-logs"></a>Použít CLI pro protokoly Tail

Abyste se vyhnuli opakovanému zadání vaší skupiny prostředků a názvu instance služby, nastavte výchozí název skupiny prostředků a název clusteru.
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
V následujících příkladech bude v příkazech vynechána skupina prostředků a název služby.

### <a name="tail-log-for-app-with-single-instance"></a>Protokol Tail pro aplikaci s jednou instancí
Pokud má aplikace s názvem auth-Service pouze jednu instanci, můžete zobrazit protokol instance aplikace pomocí následujícího příkazu:
```
az spring-cloud app logs -n auth-service
```
Tato akce vrátí protokoly:
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>Protokol Tail pro aplikaci s více instancemi
Pokud existuje pro aplikaci s názvem více instancí `auth-service` , můžete si Zobrazit protokol instance pomocí `-i/--instance` Možnosti. 

Nejprve můžete získat názvy instancí aplikace pomocí následujícího příkazu.

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
S výsledky:

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
Pak můžete zasílat protokoly instance aplikace s `-i/--instance` možností možnosti:

```
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

Můžete také získat podrobnosti o instancích aplikace z Azure Portal.  Po výběru možnosti **aplikace** v levém navigačním podokně vaší jarní cloudové služby Azure vyberte **instance aplikací**.

### <a name="continuously-stream-new-logs"></a>Průběžné streamování nových protokolů
Ve výchozím nastavení `az spring-cloud ap log tail` vytiskne jenom existující protokoly streamované do konzoly App Console a pak se ukončí. Pokud chcete streamovat nové protokoly, přidejte-f (--Sledujte):  

```
az spring-cloud app logs -n auth-service -f
``` 
Pro kontrolu všech podporovaných možností protokolování:
``` 
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>Další kroky

* [Analýza protokolů a metrik pomocí nastavení diagnostiky](./diagnostic-services.md)

 





