---
title: Řešení potíží s chybami Azure IoT Hub 404104 DeviceConnectionClosedRemotely
description: Vysvětlení, jak opravit chybu 404104 DeviceConnectionClosedRemotely
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom: mqtt
ms.openlocfilehash: c8cb91aa0c7ce1610320d4107db282d3c34407ba
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81758724"
---
# <a name="404104-deviceconnectionclosedremotely"></a>404104 DeviceConnectionClosedRemotely

Tento článek popisuje příčiny a řešení 404104 chyb **DeviceConnectionClosedRemotely** .

## <a name="symptoms"></a>Příznaky

### <a name="symptom-1"></a>Příznak 1

Zařízení se odpojí v pravidelných intervalech (například každých 65 minut) a v IoT Hub diagnostických protokolech vidíte **404104 DeviceConnectionClosedRemotely** . V některých případech se zobrazí také **401003 IoTHubUnauthorized** a událost úspěšného připojení zařízení je kratší než minuta později.

### <a name="symptom-2"></a>Příznak 2

Zařízení se náhodně odpojí a v IoT Hub diagnostických protokolech vidíte **404104 DeviceConnectionClosedRemotely** .

### <a name="symptom-3"></a>Příznak 3

Celá řada zařízení se současně odpojí. na [metrikě připojených zařízení](iot-hub-metrics.md)se zobrazí DIP a v diagnostických protokolech jsou k dispozici další [interní chyby](iot-hub-troubleshoot-error-500xxx-internal-errors.md) **404104 DeviceConnectionClosedRemotely** a 500xxx, než je obvyklé.

## <a name="causes"></a>Příčiny

### <a name="cause-1"></a>Příčina 1

Token SAS, který se [používá pro připojení k IoT Hub](iot-hub-devguide-security.md#security-tokens) vypršela, což způsobí, že IoT Hub odpojit zařízení. Připojení se znovu naváže při obnovení tokenu zařízením. Například [platnost tokenu SAS vyprší každou hodinu ve výchozím nastavení pro C SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-authentication), což může vést k pravidelnému odpojení.

Další informace najdete v článku [401003 IoTHubUnauthorized příčina](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#cause-1).

### <a name="cause-2"></a>Příčina 2

Mezi možnosti patří:

- Zařízení ztratilo základní síťové připojení déle než [MQTT Keep-Alive](iot-hub-mqtt-support.md#default-keep-alive-timeout), což má za následek časový limit vzdálené nečinnosti. Nastavení udržování otevřených připojení MQTT může být odlišné podle zařízení.

- Zařízení odeslalo resetování na úrovni protokolu TCP/IP, ale neposlalo na úrovni aplikace `MQTT DISCONNECT` . Zařízení v podstatě zavřelo základní připojení soketu. Tento problém je někdy způsoben chybami ve starších verzích sady Azure IoT SDK.

- Došlo k chybě aplikace na straně zařízení.

### <a name="cause-3"></a>Příčina 3

IoT Hub pravděpodobně dochází k přechodnému problému. Přečtěte si téma [Příčina chyby interního serveru IoT Hub](iot-hub-troubleshoot-error-500xxx-internal-errors.md#cause).

## <a name="solutions"></a>Řešení

### <a name="solution-1"></a>Řešení 1

Viz [401003 řešení IoTHubUnauthorized 1](iot-hub-troubleshoot-error-401003-iothubunauthorized.md#solution-1)

### <a name="solution-2"></a>Řešení 2

- Ověřte, že zařízení má dobré připojení k IoT Hub [otestováním připojení](tutorial-connectivity.md). Pokud je síť nespolehlivá nebo přerušovaná, nedoporučujeme zvyšovat hodnotu Keep-Alive, protože by mohla způsobit detekci (například pomocí Azure Monitor výstrah). 

- Použijte nejnovější verze [sad IoT SDK](iot-hub-devguide-sdks.md).

### <a name="solution-3"></a>Řešení 3

V tématu [řešení IoT Hub interních chybách serveru](iot-hub-troubleshoot-error-500xxx-internal-errors.md#solution).

## <a name="next-steps"></a>Další kroky

K spolehlivé správě připojení doporučujeme používat sady SDK pro zařízení Azure IoT. Další informace najdete v tématu [Správa připojení a spolehlivého zasílání zpráv pomocí sad SDK pro zařízení Azure IoT Hub](iot-hub-reliability-features-in-sdks.md) .