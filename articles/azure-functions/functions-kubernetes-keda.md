---
title: Azure Functions v Kubernetes s KEDA
description: Pochopte, jak spouštět Azure Functions v Kubernetes v cloudu nebo v místním prostředí pomocí KEDA na základě Kubernetes automatického škálování založeného na událostech.
author: jeffhollan
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: jehollan
ms.openlocfilehash: eab0a54d30f2cd2829779dbfc6081445f5be0a71
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648842"
---
# <a name="azure-functions-on-kubernetes-with-keda"></a>Azure Functions v Kubernetes s KEDA

Azure Functions runtime poskytuje flexibilitu při hostování tam, kde a jak chcete.  [Keda](https://keda.sh) (automatické škálování založené na událostech založených na události Kubernetes) bez problémů s Azure Functions modulem runtime a nástroji pro zajištění škálování na základě událostí v Kubernetes.

## <a name="how-kubernetes-based-functions-work"></a>Jak fungují funkce založené na Kubernetes

Služba Azure Functions se skládá ze dvou klíčových součástí: modulu runtime a řadiče škálování.  Modul runtime Functions spustí a spustí váš kód.  Modul runtime obsahuje logiku, jak aktivovat, protokolovat a spravovat provádění funkcí.  Modul runtime Azure Functions může běžet *kdekoli*.  Druhá součást je kontroler škálování.  Kontroler škálování monitoruje míru událostí, které cílí na vaši funkci, a proaktivně škáluje počet instancí, na kterých běží vaše aplikace.  Další informace najdete v tématu [škálování Azure functions a hostování](functions-scale.md).

Funkce založené na Kubernetes poskytují modul runtime funkcí v [kontejneru Docker](functions-create-function-linux-custom-image.md) s škálováním řízeným událostmi prostřednictvím keda.  KEDA se může škálovat na 0 instancí (když k žádné události nedochází) a na *n* instancích. Vystavuje vlastní metriky pro Kubernetes AutoScale (vodorovně pod autoscaleer).  Pomocí kontejnerů Functions s KEDA je možné replikovat funkce bez serveru v jakémkoli clusteru Kubernetes.  Tyto funkce se dají nasadit taky pomocí funkce [virtuálních uzlů Azure Kubernetes Services (AKS)](../aks/virtual-nodes-cli.md) pro infrastrukturu bez serveru.

## <a name="managing-keda-and-functions-in-kubernetes"></a>Správa KEDA a funkcí v Kubernetes

Pokud chcete spouštět funkce v clusteru Kubernetes, musíte nainstalovat komponentu KEDA. Tuto součást můžete nainstalovat pomocí [Azure Functions Core Tools](functions-run-local.md).

### <a name="installing-with-helm"></a>Instalace pomocí Helm

Existují různé způsoby, jak nainstalovat KEDA do libovolného clusteru Kubernetes, včetně Helm.  Možnosti nasazení jsou zdokumentovány na [webu keda](https://keda.sh/docs/1.4/deploy/).

## <a name="deploying-a-function-app-to-kubernetes"></a>Nasazení aplikace Function App do Kubernetes

Do clusteru Kubernetes se systémem KEDA můžete nasadit jakoukoli aplikaci Function App.  Vzhledem k tomu, že se vaše funkce spouštějí v kontejneru Docker, projekt potřebuje `Dockerfile` .  Pokud ještě žádný nemáte, můžete přidat souboru Dockerfile spuštěním následujícího příkazu v kořenu projektu Functions:

```cli
func init --docker-only
```

Pokud chcete vytvořit image a nasadit své funkce do Kubernetes, spusťte následující příkaz:

> [!NOTE]
> Základní nástroje budou k sestavení a publikování image využívat rozhraní Docker CLI. Ujistěte se, že je Docker nainstalovaný a připojený k vašemu účtu pomocí `docker login` .

```cli
func kubernetes deploy --name <name-of-function-deployment> --registry <container-registry-username>
```

> Nahraďte `<name-of-function-deployment>` názvem vaší aplikace Function App.

Tím se vytvoří `Deployment` prostředek Kubernetes, `ScaledObject` prostředek a `Secrets` , což zahrnuje proměnné prostředí importované ze `local.settings.json` souboru.

### <a name="deploying-a-function-app-from-a-private-registry"></a>Nasazení aplikace Function App z privátního registru

Výše uvedený tok funguje i u privátních registrů.  Pokud nasazujete image kontejneru z privátního registru, zahrňte `--pull-secret` příznak, který odkazuje na tajný kód Kubernetes s přihlašovacími údaji soukromého registru, když je spuštěný `func kubernetes deploy` .

## <a name="removing-a-function-app-from-kubernetes"></a>Odebrání aplikace Function App z Kubernetes

Po nasazení můžete odebrat funkci odebráním přidruženého `Deployment` , `ScaledObject` `Secrets` vytvořeného.

```cli
kubectl delete deploy <name-of-function-deployment>
kubectl delete ScaledObject <name-of-function-deployment>
kubectl delete secret <name-of-function-deployment>
```

## <a name="uninstalling-keda-from-kubernetes"></a>Odinstalace KEDA z Kubernetes

Kroky k odinstalaci KEDA jsou zdokumentovány [na webu keda](https://keda.sh/docs/1.4/deploy/).

## <a name="supported-triggers-in-keda"></a>Podporované triggery v KEDA

KEDA podporuje následující aktivační události Azure Functions:

* [Fronty Azure Storage](functions-bindings-storage-queue.md)
* [Fronty Azure Service Bus](functions-bindings-service-bus.md)
* [Služby Azure Event/IoT Hub](functions-bindings-event-hubs.md)
* [Apache Kafka](https://github.com/azure/azure-functions-kafka-extension)
* [Fronta RabbitMQ](https://github.com/azure/azure-functions-rabbitmq-extension)

### <a name="http-trigger-support"></a>Podpora triggeru HTTP

Můžete použít Azure Functions, který zveřejňuje aktivační události HTTP, ale KEDA je přímo nespravuje.  Aktivační událost KEDA Prometheus můžete využít ke [škálování Azure FUNCTIONS http od 1 do *n* instancí](https://dev.to/anirudhgarg_99/scale-up-and-down-a-http-triggered-function-app-in-kubernetes-using-keda-4m42).

## <a name="next-steps"></a>Další kroky
Další informace najdete v následujících materiálech:

* [Vytvoření funkce s použitím vlastní image](functions-create-function-linux-custom-image.md)
* [Místní psaní kódu a testování funkcí Azure Functions](functions-develop-local.md)
* [Jak funguje plán spotřeby funkcí Azure](functions-scale.md)