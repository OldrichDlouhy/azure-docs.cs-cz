---
title: Místní konfigurace agenta zabezpečení (C)
description: Přečtěte si o Azure Security Center pro místní konfiguraci agentů pro C.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 842a69c27ceb0d56df5a7b49eb9922b88d8d4b32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85206933"
---
# <a name="understanding-the-localconfigurationjson-file---c-agent"></a>Vysvětlení souboru LocalConfiguration.json – agent v jazyce C

Azure Security Center pro agenta zabezpečení IoT používá konfigurace z místního konfiguračního souboru.
Agent zabezpečení čte konfiguraci jednou při spuštění agenta.
Konfigurace nalezená v místním konfiguračním souboru obsahuje konfiguraci ověřování a další konfigurace související s agenty.
Soubor obsahuje konfigurace párů klíč-hodnota v zápisu JSON a konfigurace se naplní při instalaci agenta.

Ve výchozím nastavení se soubor nachází v umístění:/var/ASCIoTAgent/LocalConfiguration.json

Změny konfiguračního souboru se provádějí při restartování agenta.

## <a name="security-agent-configurations-for-c"></a>Konfigurace agenta zabezpečení pro jazyk C

| Název konfigurace | Možné hodnoty | Podrobnosti |
|:-----------|:---------------|:--------|
| ID agenta | Identifikátor GUID | Jedinečný identifikátor agenta |
| TriggerdEventsInterval | ISO8601 řetězec | Interval Scheduleru pro kolekci aktivovaných událostí |
| ConnectionTimeout | ISO8601 řetězec | Doba, po jejímž uplynutí vypršel časový limit připojení k IoThub |
| Authentication | JsonObject | Konfigurace ověřování. Tento objekt obsahuje všechny informace potřebné pro ověřování proti IoTHub |
| Identita | "DPS", "SecurityModule", "Device" | Ověřování identity – DPS Pokud se provádí ověření prostřednictvím DPS, SecurityModule Pokud se provádí ověřování prostřednictvím přihlašovacích údajů modulu zabezpečení nebo zařízení, pokud se provede ověřování pomocí přihlašovacích údajů k zařízení |
| Parametr | "SasToken", "SelfSignedCertificate" | uživatelský tajný klíč pro ověřování – vyberte SasToken, pokud je klíč use symetrický, vyberte certifikát podepsaný svým držitelem, pokud je tajný kód certifikát podepsaný svým držitelem.  |
| FilePath | Cesta k souboru (řetězec) | Cesta k souboru, který obsahuje tajný klíč ověřování |
| HostName | řetězec | Název hostitele služby Azure IoT Hub. obvykle <>. azure-devices.net |
| DeviceId | řetězec | ID zařízení (registrované ve službě Azure IoT Hub) |
| DPS | JsonObject | Konfigurace související s DPS |
| IDScope | řetězec | Rozsah ID v DPS |
| RegistrationId | řetězec  | ID registrace zařízení DPS |
| protokolování | JsonObject | Konfigurace související s protokolovacím agentem |
| SystemLoggerMinimumSeverity | 0 <= číslo <= 4 | zprávy protokolu rovnající se této závažnosti se budou protokolovat do/var/log/syslog (0 je nejnižší závažnost). |
| DiagnosticEventMinimumSeverity | 0 <= číslo <= 4 | zprávy protokolu rovnající se této závažnosti se budou posílat jako diagnostické události (0 je nejnižší závažnost). |

## <a name="security-agent-configurations-code-example"></a>Příklad kódu konfigurace agenta zabezpečení

```json
{
    "Configuration" : {
        "AgentId" : "b97faf0a-0f57-471f-9dab-46a8e1764946",
        "TriggerdEventsInterval" : "PT2M",
        "ConnectionTimeout" : "PT30S",
        "Authentication" : {
            "Identity" : "Device",
            "AuthenticationMethod" : "SasToken",
            "FilePath" : "/path/to/my/SymmetricKey",
            "DeviceId" : "my-device",
            "HostName" : "my-iothub.azure-devices.net",
            "DPS" : {
                "IDScope" : "",
                "RegistrationId" : ""
            }
        },
        "Logging": {
            "SystemLoggerMinimumSeverity": 0,
            "DiagnoticEventMinimumSeverity": 2
        }
    }
}
```

## <a name="next-steps"></a>Další kroky

- Přečtěte si [Přehled](overview.md) služby Azure Security Center for IoT.
- Další informace o [architektuře](architecture.md) Azure Security Center pro IoT
- Povolení služby Azure Security Center pro [službu](quickstart-onboard-iot-hub.md) IoT
- Přečtěte si [Nejčastější dotazy](resources-frequently-asked-questions.md) ke službě Azure Security Center for IoT
- Přečtěte si, jak získat přístup k [nezpracovaným datům zabezpečení](how-to-security-data-access.md)
- Vysvětlení [doporučení](concept-recommendations.md)
- Vysvětlení [výstrah](concept-security-alerts.md) zabezpečení
