---
title: Úvod do IoT technologie Plug and Play Preview | Microsoft Docs
description: Přečtěte si o IoT technologie Plug and Play ve verzi Preview. IoT technologie Plug and Play vychází z otevřeného modelovacího jazyka, který umožňuje inteligentním zařízením IoT deklarovat své schopnosti. Zařízení IoT prezentují tuto deklaraci, která se označuje jako model zařízení, když se připojují ke cloudovým řešením. Cloudové řešení pak může automaticky pochopit zařízení a začít s ním pracovat, a to vše bez psaní kódu.
author: rido-min
ms.author: rmpablos
ms.date: 07/06/2020
ms.topic: overview
ms.service: iot-pnp
services: iot-pnp
manager: eliotgra
ms.custom: references_regions
ms.openlocfilehash: 7f889d6730012a11ebf82a78583ef420b7621075
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337649"
---
# <a name="what-is-iot-plug-and-play-preview"></a>Co je IoT technologie Plug and Play Preview?

IoT technologie Plug and Play Preview umožňuje tvůrcům řešení integrovat inteligentní zařízení s jejich řešeními bez jakékoli ruční konfigurace. V jádru IoT technologie Plug and Play je _model_ zařízení, který zařízení používá k inzerování svých schopností k aplikaci s podporou technologie Plug and Play IoT. Tento model je strukturovaný jako sada elementů, které definují:

- _Vlastnosti_ , které reprezentují stav jen pro čtení nebo zapisovatelného stavu zařízení nebo jiné entity. Například sériové číslo zařízení může být vlastnost jen pro čtení a cílová teplota na termostatovi může být vlastnost s možností zápisu.
- _Telemetrii_ , která je daty vysílaná zařízením, ať už data jsou pravidelným proudem čtení senzorů, příležitostné chyby nebo informační zprávou.
- _Příkazy_ , které popisují funkci nebo operaci, které lze provést na zařízení. Například příkaz může restartovat bránu nebo pořídit obrázek pomocí vzdálené kamery.

Tyto prvky můžete seskupit v rozhraních k opakovanému použití napříč modely, aby bylo snazší spolupráce a urychlila vývoj.

Pokud chcete, aby IoT technologie Plug and Play spolupracuje s [digitálními úkoly Azure](../digital-twins/about-digital-twins.md), definujete modely a rozhraní pomocí [jazyka DTDL (Digital Working Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl). IoT technologie Plug and Play a DTDL jsou otevřené komunitě a Microsoft vítá spolupráci se zákazníky, partnery a odvětvím. Obě jsou založené na otevřených standardech W3C, jako jsou JSON-LD a RDF, což umožňuje snazší přijímání mezi službami a nástroji.

Pro použití IoT technologie Plug and Play a DTDL se neúčtují žádné další poplatky. Standardní sazby za [azure IoT Hub](../iot-hub/about-iot-hub.md) a další služby Azure zůstávají stejné.

Tento článek popisuje:

- Typické role přidružené k projektu, který používá IoT technologie Plug and Play.
- Jak používat zařízení IoT technologie Plug and Play ve vaší aplikaci.
- Jak vyvíjet aplikace zařízení IoT, která podporuje IoT technologie Plug and Play.

## <a name="user-roles"></a>Role uživatelů

IoT technologie Plug and Play je užitečné pro dva typy vývojářů:

- _Tvůrce řešení_ zodpovídá za vývoj řešení IoT pomocí Azure IoT Hub a dalších prostředků Azure a pro identifikaci zařízení IoT, která se mají integrovat.
- _Tvůrce zařízení_ vytvoří kód, který běží na zařízení připojeném k vašemu řešení.

## <a name="use-iot-plug-and-play-devices"></a>Použití zařízení IoT technologie Plug and Play

Jako tvůrce řešení můžete vyvíjet řešení IoT hostované v cloudu, které používá zařízení IoT technologie Plug and Play. Použijte [IoT Hub](../iot-hub/about-iot-hub.md) – spravovanou cloudovou službu, která funguje jako centrum zpráv pro bezpečnou obousměrnou komunikaci mezi vaší aplikací IoT a vašimi zařízeními.

Když připojíte zařízení IoT technologie Plug and Play ke službě IoT Hub, můžete použít nástroj [Azure IoT Explorer](./howto-use-iot-explorer.md) k zobrazení telemetrie, vlastností a příkazů definovaných v rozhraních, která model tvoří.

## <a name="develop-an-iot-device-application"></a>Vývoj aplikace zařízení IoT

Jako tvůrce zařízení můžete vyvíjet hardwarový produkt IoT, který podporuje technologie Plug and Play IoT. Tento proces zahrnuje tři klíčové kroky:

1. Definujte model zařízení. Vytvoříte sadu souborů JSON, které definují možnosti zařízení pomocí nástroje [DTDL](https://github.com/Azure/opendigitaltwins-dtdl). Model popisuje kompletní entitu, jako je třeba fyzický produkt, a definuje sadu rozhraní implementovaných touto entitou. Rozhraní jsou sdílené kontrakty, které jednoznačně identifikují telemetrii, vlastnosti a příkazy, které zařízení podporuje. Rozhraní lze znovu použít napříč různými modely.

1. Vytvářejte software nebo firmware zařízení tak, aby jejich telemetrie, vlastnosti a příkazy dodržovaly konvence technologie Plug and Play IoT.

1. Zařízení oznamuje ID modelu v rámci připojení MQTT. Sada Azure IoT SDK obsahuje nové konstrukce, které v době připojení poskytují ID modelu.

> [!Important]
> Zařízení IoT technologie Plug and Play musí používat MQTT nebo MQTT přes objekty WebSockets. Jiné protokoly jako AMQP nebo HTTP nejsou platné k implementaci zařízení IoT technologie Plug and Play.

## <a name="regional-availability"></a>Regionální dostupnost

Tato aktualizace IoT technologie Plug and Play Preview je dostupná na rozbočovačích IoT vytvořených v oblastech Střed USA, Severní Evropa a Východní Japonsko.

## <a name="next-steps"></a>Další kroky

Teď, když máte přehled o IoT technologie Plug and Play, navržený další krok si vyzkoušíme jedno z rychlých startů:

- [Připojení zařízení k IoT Hub (C)](./quickstart-connect-device-c.md)
- [Interakce se zařízením z vašeho řešení (Node.js)](./quickstart-service-node.md)

