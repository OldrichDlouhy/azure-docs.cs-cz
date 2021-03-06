---
title: Statistika latence sítě Azure v-cestách | Microsoft Docs
description: Přečtěte si o statistikách latence odezvy mezi oblastmi Azure.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 07/07/2020
ms.author: kumud
ms.openlocfilehash: f0266205ea5ce618793022523ce167cbc6a615c0
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86114277"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Statistika latence sítě Azure v přenosu

Azure nepřetržitě monitoruje latenci (rychlost) základních oblastí své sítě pomocí nástrojů pro interní monitorování a také měření shromažďovaných službou [ThousandEyes](https://thousandeyes.com), což je služba syntetického monitorování třetí strany.

## <a name="how-are-the-measurements-collected"></a>Jak se shromažďují míry?

Měření latence se shromažďují od agentů ThousandEyes hostovaných v cloudových oblastech Azure po celém světě, které neustále odesílají testy sítě mezi sebou v intervalu 1 minut. Statistika měsíční latence se odvozuje z průměrného počtu shromážděných ukázek v měsíci.

## <a name="june-2020-round-trip-latency-figures"></a>Hodnoty latence odezvy v červnu 2020

Níže jsou uvedené měsíční průměrné doby odezvy mezi oblastmi Azure za posledních 30 dní (končící 30. června 2020). Následující měření jsou poháněná nástrojem [ThousandEyes](https://thousandeyes.com).

[![Statistika latence mezi oblastmi Azure](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Další kroky

Přečtěte si o [oblastech Azure](https://azure.microsoft.com/global-infrastructure/regions/).
