---
title: Jak nainstalovat a spustit kontejnery pro použití rozhraní API detektoru anomálií
titleSuffix: Azure Cognitive Services
description: Pomocí pokročilých algoritmů rozhraní API pro detekci anomálií můžete identifikovat anomálie v datech časových řad.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: aahi
ms.openlocfilehash: 40906c97dc088687bbd960fecc91921a3eb888a6
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589954"
---
# <a name="install-and-run-anomaly-detector-containers-preview"></a>Instalace a spuštění kontejnerů detektoru anomálií (Preview)

Detektor anomálií má následující funkci kontejneru:

| Funkce | Funkce |
|--|--|
| Detektor anomálií | <li> Detekuje anomálie při jejich výskytu v reálném čase. <li> Detekuje anomálie v celé sadě dat jako dávku. <li> Odvodí očekávaný normální rozsah dat. <li> Podporuje úpravu citlivosti detekce anomálií, aby lépe vyhovovala vašim datům. |

Podrobné informace o rozhraních API najdete v těchto tématech:
* [Další informace o službě rozhraní API detektoru anomálií](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

## <a name="prerequisites"></a>Požadavky

Než začnete používat kontejnery detektoru anomálií, musíte splnit následující předpoklady:

|Vyžadováno|Účel|
|--|--|
|Docker Engine| Potřebujete modul Docker nainstalovaný na [hostitelském počítači](#the-host-computer). Docker poskytuje balíčky, které nakonfigurují prostředí Dockeru v systému [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) a [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Základní informace o Dockeru a kontejnerech najdete v článku [Docker Overview](https://docs.docker.com/engine/docker-overview/) (Přehled Dockeru).<br><br> Docker musí být nakonfigurovaný tak, aby umožňoval kontejnerům připojit se a odeslat fakturační data do Azure. <br><br> **V systému Windows**musí být Docker taky nakonfigurovaný tak, aby podporoval kontejnery Linux.<br><br>|
|Znalost pomocí Docker | Měli byste mít základní znalosti konceptů Docker, jako jsou registry, úložiště, kontejnery a image kontejnerů, a taky znalosti základních `docker` příkazů.| 
|Prostředek detektoru anomálií |Aby bylo možné tyto kontejnery použít, je nutné mít následující:<br><br>Prostředek _detektoru anomálií_ Azure, který získá přidružený klíč rozhraní API a identifikátor URI koncového bodu. Obě hodnoty jsou k dispozici na stránkách přehled **detektoru anomálií** Azure Portal a klíče a jsou požadovány ke spuštění kontejneru.<br><br>**{API_KEY}**: jeden ze dvou dostupných klíčů prostředků na stránce **klíče**<br><br>**{ENDPOINT_URI}**: koncový bod uvedený na stránce **Přehled**|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Hostitelský počítač

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Požadavky na kontejner a doporučení

V následující tabulce jsou popsány minimální a doporučené PROCESORové jádra a paměť pro přidělení kontejneru detektoru anomálií.

| QPS (dotazy za sekundu) | Minimální | Doporučené |
|-----------|---------|-------------|
| 10 QPS | 4 jádra, 1 GB paměti | 8 jader 2 – GB paměti |
| 20 QPS | 8 jader, 2 GB paměti | 16 jader 4 – GB paměti |

Každé jádro musí mít aspoň 2,6 GHz nebo rychlejší.

Základní a paměť odpovídají `--cpus` `--memory` nastavení a, která se používají jako součást `docker run` příkazu.

## <a name="get-the-container-image-with-docker-pull"></a>Získat image kontejneru pomocí`docker pull`

Pomocí [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) příkazu Stáhněte image kontejneru.

| Kontejner | Repository |
|-----------|------------|
| rozpoznávání – služby – anomálie-detektor | `mcr.microsoft.com/azure-cognitive-services/anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-anomaly-detector-container"></a>Pull Docker pro kontejner detektoru anomálií

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Jak používat kontejner

Jakmile je kontejner na [hostitelském počítači](#the-host-computer), použijte následující postup pro práci s kontejnerem.

1. [Spusťte kontejner](#run-the-container-with-docker-run)s požadovaným nastavením fakturace. [examples](anomaly-detector-container-configuration.md#example-docker-run-commands) `docker run` K dispozici jsou další příklady příkazu.
1. [Dotazování koncového bodu předpovědi kontejneru](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-with-docker-run"></a>Spusťte kontejner s`docker run`

Ke spuštění kontejneru použijte příkaz [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) . Podrobnosti o tom, jak získat hodnoty a, najdete v článku [shromáždění požadovaných parametrů](#gathering-required-parameters) `{ENDPOINT_URI}` `{API_KEY}` .

[Examples](anomaly-detector-container-configuration.md#example-docker-run-commands) `docker run` K dispozici jsou příklady příkazů.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/anomaly-detector:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Tento příkaz:

* Spustí kontejner detektoru anomálií z image kontejneru.
* Přiděluje jedno PROCESORové jádro a 4 gigabajty (GB) paměti.
* Zveřejňuje port TCP 5000 a přiděluje pro kontejner pseudo TTY.
* Po ukončení automaticky odstraní kontejner. Bitová kopie kontejneru je stále k dispozici na hostitelském počítači. 

> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Aby bylo možné spustit kontejner, musí být zadány možnosti, a. v opačném případě se kontejner nespustí.  Další informace najdete v tématu [fakturace](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Spuštění více kontejnerů na stejném hostiteli

Pokud máte v úmyslu spustit více kontejnerů s vystavenými porty, ujistěte se, že každý kontejner spustíte s jiným portem. Například spusťte první kontejner na portu 5000 a druhý kontejner na portu 5001.

Nahraďte `<container-registry>` a hodnotou `<container-name>` kontejnerů, které používáte. Nemusí se jednat o stejný kontejner. Můžete mít kontejner detektoru anomálií a kontejner LUIS běžící na hostiteli společně nebo může být spuštěno více kontejnerů detektoru anomálií. 

Spusťte první kontejner na portu 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Spusťte druhý kontejner na portu 5001.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Každý další kontejner by měl být na jiném portu. 

## <a name="query-the-containers-prediction-endpoint"></a>Dotazování koncového bodu předpovědi kontejneru

Kontejner poskytuje rozhraní API koncového bodu předpovědi založené na REST. 

http://localhost:5000Pro rozhraní API kontejneru použijte hostitele.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Zastavení kontejneru

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Řešení potíží

Pokud spouštíte kontejner s povoleným výstupním [připojením](anomaly-detector-container-configuration.md#mount-settings) a povolíte protokolování, kontejner generuje soubory protokolu, které jsou užitečné při řešení problémů, ke kterým dochází při spuštění nebo spuštění kontejneru.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Fakturace

Kontejnery detektoru anomálií odesílají informace o fakturaci do Azure pomocí prostředku _detektoru anomálií_ ve vašem účtu Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Další informace o těchto možnostech najdete v tématu [konfigurace kontejnerů](anomaly-detector-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Souhrn

V tomto článku jste zjistili koncepty a pracovní postupy pro stažení, instalaci a spuštění kontejnerů detektoru anomálií. Souhrn:

* Detekce anomálií poskytuje jeden kontejner pro Linux pro Docker, který zapouzdřuje detekci anomálií pomocí služby Batch vs streaming, očekávalo se odvození rozsahu a optimalizace citlivosti.
* Image kontejneru se stáhnou z privátní Azure Container Registry vyhrazené pro kontejnery ve verzi Preview.
* Image kontejneru se spouštějí v Docker.
* Můžete použít REST API nebo SDK pro volání operací v kontejnerech detektoru anomálií zadáním identifikátoru URI hostitele kontejneru.
* Při vytváření instance kontejneru je nutné zadat informace o fakturaci.

> [!IMPORTANT]
> Nemusíte spouštět kontejnery Cognitive Services bez připojení k Azure pro měření. Zákazníci musí povolit kontejnerům, aby ve všech časech komunikovaly informace o fakturaci. Kontejnery Cognitive Services neodesílají zákaznická data (např. Analyzovaná data časových řad) do Microsoftu.

## <a name="next-steps"></a>Další kroky

* Přečtěte si téma [konfigurace kontejnerů](anomaly-detector-container-configuration.md) pro nastavení konfigurace
* [Nasazení kontejneru detektoru anomálií pro Azure Container Instances](how-to/deploy-anomaly-detection-on-container-instances.md)
* [Další informace o službě rozhraní API detektoru anomálií](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
