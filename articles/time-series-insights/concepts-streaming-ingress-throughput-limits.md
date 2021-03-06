---
title: Omezení propustnosti příjmu streamování – Azure Time Series Insights Gen2 | Microsoft Docs
description: Přečtěte si o limitech propustnosti příchozích dat v Azure Time Series Insights Gen2.
author: lyrana
ms.author: lyhughes
manager: dpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: seodec18
ms.openlocfilehash: fb10effce8b94a6443e1daa8dadaa99111da0d4e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87097313"
---
# <a name="streaming-ingestion-throughput-limits"></a>Omezení propustnosti příjmu streamování

Omezení příchozího přenosu dat v Azure Time Series Insights Gen2 jsou popsána níže.

> [!TIP]
> Podrobný seznam všech omezení najdete v tématu [plánování prostředí Azure Time Series Insights Gen2](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits) .

## <a name="per-environment-limitations"></a>Omezení podle prostředí

Míry příchozího přenosu dat se zobrazují jako faktor počtu zařízení, která jsou ve vaší organizaci, četnosti emisí událostí a velikosti jednotlivých událostí:

*  **Počet zařízení** × **četnost měření událostí** × **Velikost každé události**.

Ve výchozím nastavení Azure Time Series Insights Gen2 může ingestovat příchozí data rychlostí **až 1 MB za sekundu (MB/s) na Azure Time Series Insights prostředí Gen2**. Existují další omezení [na oddíl centra](./concepts-streaming-ingress-throughput-limits.md#hub-partitions-and-per-partition-limits).

> [!TIP]
>
> * Podpora prostředí pro přijímání rychlostí až 16 MB/s může být zajištěna žádostí.
> * Pokud potřebujete vyšší propustnost odesláním lístku podpory prostřednictvím Azure Portal, kontaktujte nás.
 
* **Příklad 1:**

    Při expedici společnosti Contoso je 100 000 zařízení, která generují události třikrát za minutu. Velikost události je 200 bajtů. Používají IoT Hub se čtyřmi oddíly jako se zdrojem událostí Azure Time Series Insights Gen2.

    * Rychlost příjmu pro své Azure Time Series Insights prostředí Gen2 by byla: **100 000 zařízení * 200 bajtů/událost * (3/60 události/s) = 1 MB/s**.
    * Frekvence přijímání zpráv na oddíl by byla 0,25 MB/s.
    * Míra ingestování společnosti Contoso bude v rámci omezení škálování.

* **Příklad 2:**

    Analýza loďstva společnosti Contoso má 60 000 zařízení, která každou sekundu emitují událost. Používají centrum událostí s počtem oddílů 4, Azure Time Series Insights zdroj událostí Gen2. Velikost události je 200 bajtů.

    * Frekvence ingestování prostředí by byla: **60 000 zařízení × 200 bajtů/událost * 1 událost/s = 12 MB/** s.
    * Frekvence za oddíl by byla 3 MB/s.
    * Míra ingestování infrastruktury společnosti Contoso je nad limity prostředí a oddílů. Můžou odeslat žádost o Azure Time Series Insights Gen2 prostřednictvím Azure Portal, aby se zvýšila rychlost přijímání zpráv pro své prostředí, a vytvořit centrum událostí s dalšími oddíly, které budou v rámci omezení.

## <a name="hub-partitions-and-per-partition-limits"></a>Omezení oddílů centra a na oddíly

Při plánování Azure Time Series Insightsho prostředí Gen2 je důležité zvážit konfiguraci zdrojů událostí, ke kterým se budete připojovat Azure Time Series Insights Gen2. Azure IoT Hub i Event Hubs využívají oddíly k povolení horizontálního škálování pro zpracování událostí. 

*Oddíl* je seřazená posloupnost událostí, která je držena v centru. Počet oddílů se nastaví během fáze vytváření centra a nedá se změnit.

[Informace o tom, kolik oddílů](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need) potřebuji pro Event Hubs Doporučené postupy, najdete v tématu.

> [!NOTE]
> Většina Center IoT používá se Azure Time Series Insights Gen2 potřebuje jenom čtyři oddíly.

Bez ohledu na to, jestli vytváříte nové centrum pro prostředí Azure Time Series Insights Gen2 nebo použijete stávající, budete muset vypočítat sazbu ingestování na oddíly, abyste zjistili, jestli je v rámci omezení. 

Azure Time Series Insights Gen2 v současné době má **omezení na oddíly 0,5 MB/s**.

### <a name="iot-hub-specific-considerations"></a>Předpoklady týkající se IoT Hub

Při vytvoření zařízení v IoT Hub se trvale přiřadí k oddílu. V takovém případě IoT Hub moci zaručit řazení událostí (protože se přiřazení nikdy nemění).

Pevné přiřazení oddílu má vliv také na Azure Time Series Insights instance Gen2, které ingestují data odesílaná z IoT Hub pro příjem dat. Když se zprávy z více zařízení předají do centra pomocí stejného ID zařízení brány, můžou dorazit do stejného oddílu ve stejnou dobu, než je omezení škálování na oddíly.

**Dopad**:

* Pokud se v jednom oddílu udržuje nepřetržitá míra přijímání na limit, je možné, že Azure Time Series Insights Gen2 nebude synchronizovat všechny telemetrie zařízení před tím, než se překročila IoT Hub období uchovávání dat. V důsledku toho může dojít ke ztrátě odeslaných dat v případě, že dojde k trvalému překročení limitů pro přijímání.

Pro zmírnění této situace doporučujeme následující osvědčené postupy:

* Před nasazením řešení Vypočítejte sazby pro ingestování na jednotlivé prostředí a na oddíly.
* Ujistěte se, že vaše zařízení IoT Hub jsou vyvážená z hlediska zatížení co nejdáleního rozsahu.

> [!IMPORTANT]
> Pro prostředí, která používají IoT Hub jako zdroj události, vypočítejte rychlost příjmu pomocí počtu používaných zařízení centra, aby se zajistilo, že frekvence klesne pod omezení 0,5 MB/s na jeden oddíl.
>
> * I v případě, že několik událostí dorazí současně, limit nebude překročen.

  ![Diagram IoT Hubho oddílu](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Další informace o optimalizaci propustnosti a oddílů centra najdete v následujících zdrojích informací:

* [Škálování IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Škálování centra událostí](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Oddíly centra událostí](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

## <a name="next-steps"></a>Další kroky

* Přečtěte si o [úložišti](./concepts-storage.md) dat