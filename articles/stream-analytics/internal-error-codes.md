---
title: Řešení potíží s kódy chyb Azure Stream Analytics
description: Řešení potíží s Azure Stream Analytics problémy s kódy interních chyb.
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 9dc3d873ddfef9a729f583cd914ca4700d562ff3
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045225"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Azure Stream Analytics kódy interních chyb

Protokoly aktivit a protokoly prostředků můžete použít k ladění neočekávaného chování z vaší Azure Stream Analytics úlohy. V tomto článku je uveden popis každého interního kódu chyby. Vnitřní chyby jsou obecné chyby, které jsou vyvolány v rámci Stream Analyticsé platformy, pokud Stream Analytics nedokáže rozlišovat, pokud je chyba vnitřní dostupnost nebo chyba v systému.

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **Příčina**: velikost dávky použitá k zápisu do Cosmos DB je příliš velká. 
* **Doporučení**: zkuste zopakovat s menší velikostí dávky.

## <a name="next-steps"></a>Další kroky

* [Řešení potíží se vstupními připojeními](stream-analytics-troubleshoot-input.md)
* [Řešení potíží s výstupy Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Řešení potíží s Azure Stream Analytics dotazy](stream-analytics-troubleshoot-query.md)
* [Řešení potíží s Azure Stream Analytics pomocí protokolů prostředků](stream-analytics-job-diagnostic-logs.md)
* [Chyby Azure Stream Analytics dat](data-errors.md)