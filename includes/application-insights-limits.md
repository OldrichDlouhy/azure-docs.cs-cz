---
title: zahrnout soubor
description: zahrnout soubor
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 08/06/2019
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 14e2bd4af2616e9dd33fe8267de132ab6c0f1cfe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "69982604"
---
K dispozici jsou určitá omezení počtu metrik a událostí na aplikaci, tj. na klíč instrumentace. Omezení závisí na zvoleném [cenovém plánu](https://azure.microsoft.com/pricing/details/application-insights/).

| Prostředek | Výchozí omezení | Poznámka
| --- | --- | --- |
| Celkem dat za den | 100 GB | Objem dat jde snížit nastavením limitu. Pokud potřebujete víc dat, můžete limit na portálu zvýšit, až 1 000 GB. Pro kapacitu větší než 1 000 GB odešlete e-mail na adresu AIDataCap@microsoft.com .
| Throttling | události 32 000 za sekundu | Omezení se měří se po minutách.
| Uchovávání dat | [30-730 dní](https://docs.microsoft.com/azure/azure-monitor/app/pricing#change-the-data-retention-period)  | Tento prostředek je pro funkce [Vyhledávání](../articles/azure-monitor/app/diagnostic-search.md), [Analýza](../articles/azure-monitor/app/analytics.md) a [Průzkumník metrik](../articles/azure-monitor/app/metrics-explorer.md).
| [Vícekrokový test dostupnosti](../articles/azure-monitor/app/availability-multistep.md) – uchování podrobných výsledků | 90 dnů | Tento prostředek poskytuje podrobné výsledky každého kroku.
| Maximální velikost položky telemetrie | 64 kB |
| Maximální počet položek telemetrie na dávku | 64 K |
| Délka názvu vlastnosti a metriky | 150 | Viz [schémata typů](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Délka řetězce hodnoty vlastnosti | 8 192  | Viz [schémata typů](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Délka zprávy trasování a výjimky | 32 768  | Viz [schémata typů](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| [Testy dostupnosti](../articles/azure-monitor/app/monitor-web-app-availability.md) – počet na aplikaci | 100 |
| Uchovávání dat [profileru](../articles/azure-monitor/app/profiler.md) | 5 dní |
| Data [profileru](../articles/azure-monitor/app/profiler.md) odesílaná za den | 10 GB |

Další informace najdete v tématu [Ceny a kvóty ve službě Application Insights](../articles/azure-monitor/app/pricing.md).