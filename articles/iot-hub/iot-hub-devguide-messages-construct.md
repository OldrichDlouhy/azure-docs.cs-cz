---
title: Vysvětlení formátu zprávy Azure IoT Hub | Microsoft Docs
description: Příručka pro vývojáře – popisuje formát a očekávaný obsah IoT Hubch zpráv.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: asrastog
ms.custom:
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: dd2b88d923d0398dc42362242b94b978ccd24252
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87336714"
---
# <a name="create-and-read-iot-hub-messages"></a>Vytvoření a čtení zpráv IoT Hubu

Pro zajištění bezproblémové interoperability mezi protokoly IoT Hub definuje společný formát zpráv pro všechny protokoly pro zařízení. Tento formát zprávy se používá pro směrování mezi zařízeními [a cloudem i pro zprávy ze](iot-hub-devguide-messages-c2d.md) zařízení do [cloudu](iot-hub-devguide-messages-d2c.md) . 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub implementuje zprávy typu zařízení-Cloud pomocí vzoru zasílání zpráv streamování. Zprávy typu zařízení-Cloud IoT Hub jsou větší, jako [Event Hubs](/azure/event-hubs/) *události* , než [Service Bus](/azure/service-bus-messaging/) *zprávy* v tom, že existuje velký objem událostí, které je možné předávat prostřednictvím služby, kterou může číst více čtenářů.

IoT Hub zpráva se skládá z těchto:

* Předem stanovená sada *vlastností systému* , jak je uvedeno níže.

* Sada *vlastností aplikace* Slovník vlastností řetězce, které může aplikace definovat a přistupovat, aniž by bylo nutné deserializovat tělo zprávy. IoT Hub tyto vlastnosti nikdy nezmění.

* Neprůhledný binární text

Názvy vlastností a hodnoty mohou obsahovat pouze alfanumerické znaky ASCII a také ``{'!', '#', '$', '%, '&', ''', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` při posílání zpráv ze zařízení do cloudu pomocí protokolu HTTPS nebo posílání zpráv z cloudu na zařízení.

Zasílání zpráv ze zařízení do cloudu s IoT Hub má následující vlastnosti:

* Zprávy ze zařízení do cloudu jsou odolné a uchovávané ve výchozím koncovém bodě **zpráv/událostí** služby IoT Hub po dobu až sedmi dnů.

* Zprávy ze zařízení do cloudu můžou být maximálně 256 KB a můžou se seskupovat v dávkách pro optimalizaci odesílání. Dávky mohou být nejvýše 256 KB.

* IoT Hub nepovoluje libovolné dělení. Zprávy ze zařízení do cloudu jsou rozdělené podle původního **deviceId**.

* Jak je vysvětleno v tématu [řízení přístupu k IoT Hub](iot-hub-devguide-security.md), IoT Hub umožňuje ověřování na základě zařízení a řízení přístupu.

* Zprávy lze porazítkovat informacemi, které jsou součástí vlastností aplikace. Další informace najdete v tématu věnovaném [rozšířením zpráv](iot-hub-message-enrichments-overview.md).

Další informace o tom, jak zakódovat a dekódovat zprávy odeslané pomocí různých protokolů, najdete v tématu sady [SDK služby Azure IoT](iot-hub-devguide-sdks.md).

## <a name="system-properties-of-d2c-iot-hub-messages"></a>Systémové vlastnosti zpráv **D2C** IoT Hub

| Vlastnost | Popis  |Nastavit uživatele?|Klíčové slovo pro </br>dotaz směrování|
| --- | --- | --- | --- |
| ID zprávy |Uživatelsky nastavitelný identifikátor zprávy, která se používá ke vzorům požadavků a odpovědí. Format: řetězec s rozlišováním velkých a malých písmen (maximálně 128 znaků dlouhý) znaků ASCII 7 bitů `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}` .  | Ano | Parametr |
| iothub – enqueuedtime |Datum a čas přijetí zprávy ze [zařízení do cloudu](iot-hub-devguide-d2c-guidance.md) IoT Hub. | Ne | enqueuedTime |
| user-id |ID, které slouží k určení původu zpráv. Když jsou zprávy generovány IoT Hub, je nastavena na `{iot hub name}` . | Ano | userId |
| iothub-ID zařízení-připojení |ID nastavené IoT Hub u zpráv ze zařízení do cloudu. Obsahuje **deviceId** zařízení, které zprávu odeslalo. | Ne | connectionDeviceId |
| iothub-Connection-Module-ID |ID nastavené IoT Hub u zpráv ze zařízení do cloudu. Obsahuje **moduleId** zařízení, které zprávu odeslalo. | Ne | connectionModuleId |
| iothub-Connection-auth-Generation-ID |ID nastavené IoT Hub u zpráv ze zařízení do cloudu. Obsahuje **connectionDeviceGenerationId** (podle [vlastností identity jednotlivých zařízení](iot-hub-devguide-identity-registry.md#device-identity-properties)) zařízení, které zprávu odeslalo. | Ne |connectionDeviceGenerationId |
| iothub připojení-auth-Method |Metoda ověřování nastavená IoT Hub na zprávy ze zařízení do cloudu. Tato vlastnost obsahuje informace o metodě ověřování, která se používá k ověření zařízení odesílajícího zprávu.| Ne | connectionAuthMethod |
| DT-DataSchema | Tuto hodnotu nastavuje služba IoT Hub pro zprávy ze zařízení do cloudu. Obsahuje ID modelu zařízení nastavené v připojení zařízení. Tato funkce je k dispozici jako součást [IoT technologie Plug and Play Public Preview](../iot-pnp/overview-iot-plug-and-play.md). | Ne | – |
| DT – předmět | Název součásti odesílající zprávy typu zařízení-Cloud. Tato funkce je k dispozici jako součást [IoT technologie Plug and Play Public Preview](../iot-pnp/overview-iot-plug-and-play.md). | Ano | – |

## <a name="system-properties-of-c2d-iot-hub-messages"></a>Systémové vlastnosti zpráv **C2D** IoT Hub

| Vlastnost | Popis  |Nastavit uživatele?|
| --- | --- | --- |
| ID zprávy |Uživatelsky nastavitelný identifikátor zprávy, která se používá ke vzorům požadavků a odpovědí. Format: řetězec s rozlišováním velkých a malých písmen (maximálně 128 znaků dlouhý) znaků ASCII 7 bitů `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}` .  |Ano|
| pořadové číslo |Číslo (jedinečné pro každou frontu zařízení) přiřazené IoT Hub ke každé zprávě typu cloud-zařízení. |Ne|
| na |Cíl určený ve zprávách typu [Cloud-zařízení](iot-hub-devguide-c2d-guidance.md) . |Ne|
| absolutní – doba vypršení platnosti |Datum a čas vypršení platnosti zprávy |Ne|   |
| correlation-id |Řetězcová vlastnost ve zprávě odpovědi, která obvykle obsahuje parametr MessageId žádosti ve vzorech požadavek-odpověď. |Ano|
| user-id |ID, které slouží k určení původu zpráv. Když jsou zprávy generovány IoT Hub, je nastavena na `{iot hub name}` . |Ano|
| iothub – ACK |Generátor zpráv zpětné vazby. Tato vlastnost se používá ve zprávách typu cloud-zařízení k vyžádání IoT Hub k vygenerování zpráv zpětné vazby v důsledku spotřeby zprávy ze zařízení. Možné hodnoty: **none** (výchozí): není generována žádná zpětná vazba, **kladná**zpráva o zpětné vazbě v případě, že zpráva byla dokončena, **záporná**: doručení zprávy zpětné vazby, pokud vypršela platnost zprávy (nebo byl dosažen maximální počet doručení), aniž by bylo zařízení dokončeno nebo **úplné**: kladné i záporné. |Ano|

### <a name="system-property-names"></a>Názvy systémových vlastností

Názvy systémových vlastností se liší v závislosti na koncovém bodu, na který jsou směrovány zprávy. Podrobnosti o těchto názvech naleznete v následující tabulce.

|Název systémové vlastnosti|Event Hubs|Azure Storage|Service Bus|Event Grid|
|--------------------|----------|-------------|-----------|----------|
|ID zprávy|ID zprávy|Parametr|Parametr|ID zprávy|
|Id uživatele|user-id|userId|UserId|user-id|
|ID zařízení připojení|iothub-ID zařízení-připojení| connectionDeviceId|iothub-ID zařízení-připojení|iothub-ID zařízení-připojení|
|ID modulu připojení|iothub-Connection-Module-ID|connectionModuleId|iothub-Connection-Module-ID|iothub-Connection-Module-ID|
|ID generování ověřování připojení|iothub-Connection-auth-Generation-ID|connectionDeviceGenerationId| iothub-Connection-auth-Generation-ID|iothub-Connection-auth-Generation-ID|
|Metoda auth připojení|iothub připojení-auth-Method|connectionAuthMethod|iothub připojení-auth-Method|iothub připojení-auth-Method|
|Třída|typ obsahu|Třída|Třída|iothub-Content-Type|
|contentEncoding|kódování obsahu|contentEncoding|ContentEncoding|iothub – kódování obsahu|
|iothub – enqueuedtime|iothub – enqueuedtime|enqueuedTime| – |iothub – enqueuedtime|
|CorrelationId|correlation-id|correlationId|CorrelationId|correlation-id|
|DT-DataSchema|DT-DataSchema|DT-DataSchema|DT-DataSchema|DT-DataSchema|
|DT – předmět|DT – předmět|DT – předmět|DT – předmět|DT – předmět|

## <a name="message-size"></a>Velikost zpráv

IoT Hub měří velikost zpráv v nezávislá způsobech, přičemž zvažuje pouze skutečnou datovou část. Velikost v bajtech se počítá jako součet z následujících hodnot:

* Velikost těla v bajtech
* Velikost v bajtech všech hodnot vlastností systému zprávy.
* Velikost všech názvů a hodnot vlastností uživatele v bajtech.

Názvy vlastností a hodnoty jsou omezeny na znaky ASCII, takže délka řetězce se rovná velikosti v bajtech.

## <a name="anti-spoofing-properties"></a>Vlastnosti ochrany proti falšování identity

Aby se zabránilo falšování zařízení v rámci zpráv ze zařízení do cloudu, IoT Hub razítka všech zpráv s následujícími vlastnostmi:

* **iothub-ID zařízení-připojení**
* **iothub-Connection-auth-Generation-ID**
* **iothub připojení-auth-Method**

První dva obsahují **deviceId** a **generationId** původního zařízení, jako jsou [Vlastnosti identity jednotlivých zařízení](iot-hub-devguide-identity-registry.md#device-identity-properties).

Vlastnost **iothub-Connection-auth-Method** obsahuje serializovaný objekt JSON s následujícími vlastnostmi:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Další kroky

* Informace o omezení velikosti zpráv v IoT Hub najdete v tématu [IoT Hub kvóty a omezování](iot-hub-devguide-quotas-throttling.md).

* Informace o tom, jak vytvářet a číst IoT Hub zprávy v různých programovacích jazycích, najdete v tématu [rychlé starty](quickstart-send-telemetry-node.md).
