---
title: Zdroje událostí ingestování streamování – Azure Time Series Insights Gen2 | Microsoft Docs
description: Přečtěte si o streamování dat do Azure Time Series Insights Gen2.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: c2a25632942c0c39a20fa0c7f51a1e8937bdd873
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87059384"
---
# <a name="azure-time-series-insights-gen2-event-sources"></a>Azure Time Series Insights zdroje událostí Gen2

 Vaše Azure Time Series Insights Gen2 prostředí může mít až dva streamování zdrojů událostí. Jako vstupy se podporují dva typy prostředků Azure:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Události se musí odesílat jako JSON kódovaný ve formátu UTF-8.

## <a name="create-or-edit-event-sources"></a>Vytvoření nebo úprava zdrojů událostí

Prostředky zdroje událostí můžou být živé ve stejném předplatném Azure jako vaše prostředí Azure Time Series Insights Gen2 nebo jiné předplatné. K vytváření, úpravám a odstraňování zdrojů událostí vašeho prostředí můžete použít [Azure Portal](time-series-insights-update-create-environment.md#create-a-preview-payg-environment), [Azure CLI](https://github.com/Azure/azure-cli-extensions/tree/master/src/timeseriesinsights), [šablony ARM](time-series-insights-manage-resources-using-azure-resource-manager-template.md)a [REST API](https://docs.microsoft.com/rest/api/time-series-insights/management/eventsources) .

Po připojení zdroje událostí vaše prostředí Azure Time Series Insights Gen2 přečte všechny události, které jsou aktuálně uložené ve službě IoT nebo v centru událostí, počínaje nejstarší událostí.

> [!IMPORTANT]
>
> * Při připojování zdroje událostí k prostředí Azure Time Series Insights Gen2 může docházet k vysoké počáteční latenci.
> Latence zdroje událostí závisí na počtu událostí, které jsou aktuálně ve IoT Hub nebo v centru událostí.
> * Po prvním ingestování zdrojových dat událostí se tato vysoká latence bude nacházet. Pokud se setkáte s probíhající vysokou latencí, odešlete lístek podpory prostřednictvím Azure Portal.

## <a name="streaming-ingestion-best-practices"></a>Osvědčené postupy příjmu streamování

* Vždy vytvořte jedinečnou skupinu uživatelů pro prostředí Azure Time Series Insights Gen2, abyste mohli využívat data ze zdroje událostí. Opětovné použití skupin uživatelů může způsobit náhodné odpojení a může dojít ke ztrátě dat.

* Nakonfigurujte prostředí Azure Time Series Insights Gen2 a IoT Hub a/nebo Event Hubs ve stejné oblasti Azure. I když je možné nakonfigurovat zdroje událostí v samostatné oblasti, tento scénář se nepodporuje a nemůžeme zaručit vysokou dostupnost.

* Nepřekračuje [limit četnosti propustnosti](./concepts-streaming-ingress-throughput-limits.md) vašeho prostředí ani limit počtu na oddíly.

* Nakonfigurujte [Upozornění](https://review.docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency?branch=pr-en-us-117938#monitor-latency-and-throttling-with-alerts) na prodlevu, které bude informovat, pokud vaše prostředí má problémy zpracovávající data.

* Ingestování streamování se dá použít jenom pro téměř v reálném čase i pro poslední data. data streamovaná v historických datech se nepodporují.

* Pochopte, jak budou vlastnosti uvozeny řídicími znaky a data JSON se [sloučí a uloží.](./concepts-json-flattening-escaping-rules.md)

* Při poskytování připojovacích řetězců zdrojové události postupujte podle principu minimálního oprávnění. V případě Event Hubs nakonfigurujte zásady sdíleného přístupu pouze s deklarací *Odeslat* a pro IoT Hub použijte pouze oprávnění *k připojení služby* .

### <a name="historical-data-ingestion"></a>Ingestování historických dat

Použití kanálu streamování k importu historických dat se v současnosti v Azure Time Series Insights Gen2 nepodporuje. Pokud potřebujete importovat minulá data do svého prostředí, postupujte podle následujících pokynů:

* Nepoužívejte paralelní streamování živých a historických dat. Ingestování dat z pořadí bude mít za následek snížení výkonu dotazů.
* Ingestovat historická data v časově uspořádaném čase pro dosažení nejlepšího výkonu.
* Zajistěte si omezení rychlosti propustnosti příjmu níže.
* Pokud jsou data starší než doba uchovávání teplého obchodu, zakažte úložiště.

## <a name="event-source-timestamp"></a>Časové razítko zdroje události

Při konfiguraci zdroje událostí budete požádáni o zadání vlastnosti ID časového razítka. Vlastnost timestamp se používá ke sledování událostí v průběhu času, což je čas, který se použije jako $event. $ts v [rozhraních API pro dotazy](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute) a pro vykreslování řad v Azure Time Series Insights Gen2 Exploreru. Pokud není v době vytvoření k dispozici žádná vlastnost nebo pokud v události chybí vlastnost timestamp, bude jako výchozí použita událost IoT Hub nebo centra událostí. Hodnoty vlastnosti časového razítka jsou uloženy ve formátu UTC.

Obecně platí, že uživatelé budou chtít přizpůsobit vlastnost časového razítka a použít čas, kdy senzor nebo značka vygenerovala čtení místo použití výchozího centra ve frontě. To je vhodné zejména v případě, že zařízení mají přerušovanou ztrátu připojení a dávka zpožděných zpráv se předává Azure Time Series Insights Gen2.

Pokud je vlastní časové razítko v rámci vnořeného objektu JSON nebo pole, budete muset zadat správný název vlastnosti za naše [sloučené a uvozovací konvence pojmenování](concepts-json-flattening-escaping-rules.md). Například zdrojové časové razítko pro datovou část JSON, které vidíte [tady](concepts-json-flattening-escaping-rules.md#example-a) , by mělo být zadáno jako `"values.time"` .

### <a name="time-zone-offsets"></a>Posuny časového pásma

Časová razítka musí být odesílána ve formátu ISO 8601 a budou uložena v UTC. Pokud je k dispozici posun časového pásma, použije se posun a pak se čas uloží a vrátí ve formátu UTC. Pokud je posun nesprávně formátovaný, bude ignorován. V situacích, kdy vaše řešení nemusí mít kontext původního posunutí, můžete odeslat posunutá data v další samostatné vlastnosti události, abyste zajistili, že jsou zachovaná a že vaše aplikace může odkazovat na odpověď na dotaz.

Posun časového pásma by měl být naformátovaný jako jeden z následujících:

± HHMMZ</br>
± HH: MM</br>
± HH: MMZ</br>

## <a name="next-steps"></a>Další kroky

* Přečtěte si [pravidla pro sloučení a uvozovací znaky JSON](./concepts-json-flattening-escaping-rules.md) , abyste pochopili, jak budou události uložené. 

* Princip [omezení propustnosti](./concepts-streaming-ingress-throughput-limits.md) vašeho prostředí




