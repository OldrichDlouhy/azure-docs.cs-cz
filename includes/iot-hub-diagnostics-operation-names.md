---
title: zahrnout soubor
description: zahrnout soubor
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 362c13771e7382ead1ba5aebd99a69fd86cd718c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84793314"
---
<!-- operation names for the diag logs for IoT Hub -->

|Název operace|Úroveň|Description|
|------------- |-----|-----------|
|UndefinedRouteEvaluation|Informace|Zprávu nelze vyhodnotit pomocí podmínky předání. Například pokud ve zprávě chybí vlastnost v podmínce dotaz trasy. Přečtěte si další informace o [syntaxi dotazů směrování](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).|
|RouteEvaluationError|Chyba|Při vyhodnocování zprávy došlo k chybě z důvodu problému s formátem zprávy. Tato chyba se zaznamená například v případě, že kódování obsahu není zadáno nebo typ obsahu není ve zprávě platný. Ty musí být nastavené ve [vlastnostech systému](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties).|
|DroppedMessage|Chyba|Zpráva byla vyřazena a nebyla směrována. To může být způsobeno tím, že se zpráva neshodovala s žádným dotazem na směrování nebo koncový bod nebyl doručen a zpráva nemohla být doručena po několika opakovaných pokusech. K získání dalších podrobností o koncovém bodu doporučujeme použít REST API [získat stav koncového bodu](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointUnhealthy|Chyba|Koncový bod nepřijal zprávy od IoT Hub a IoT Hub se pokouší znovu odeslat zprávy. Doporučujeme, abyste zobrazili poslední známou chybu prostřednictvím REST API [získat stav koncového bodu](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointDead|Chyba|Koncový bod nepřijímal zprávy od IoT Hub po dobu delší než hodinu. Doporučujeme, abyste zobrazili poslední známou chybu prostřednictvím REST API [získat stav koncového bodu](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointHealthy|Informace|Koncový bod je v pořádku a přijímá zprávy z IoT Hub. Tato zpráva se neprotokoluje průběžně, ale přihlásila se jenom v případě, že se koncový bod znovu obnoví. Tato zpráva znamená, že IoT Hub nebylo možné odeslat zprávy do koncového bodu, ale koncový bod je nyní v pořádku.|
|OrphanedMessage|Informace|Zpráva neodpovídá žádné trase.|
|InvalidMessage|Chyba|Zpráva je neplatná kvůli nekompatibilitě s koncovým bodem. Doporučujeme, abyste zkontrolovali konfigurace koncového bodu.|


Operace *UndefinedRouteEvaluation*, *RouteEvaluationError* a *OrphanedMessage* jsou omezené a protokolují se nejenom jednou za minutu na IoT Hub.