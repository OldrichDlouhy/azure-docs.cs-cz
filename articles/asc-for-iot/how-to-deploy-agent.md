---
title: Výběr a nasazení agentů zabezpečení
description: Přečtěte si, jak vybírat a nasazovat Azure Security Center pro agenty zabezpečení IoT na zařízeních IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 82103da3f2547baab62d0d95cf9a6084ef099cba
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87033763"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Výběr a nasazení agenta zabezpečení na zařízení IoT

Azure Security Center pro IoT poskytuje referenční architektury pro agenty zabezpečení, kteří monitorují a shromažďují data ze zařízení IoT.
Další informace najdete v tématu [Referenční architektura agenta zabezpečení](security-agent-architecture.md).

Agenti se vyvíjí jako open source projekty a jsou k dispozici ve dvou charakterech: <br> [C](https://aka.ms/iot-security-github-c)a [C#](https://aka.ms/iot-security-github-cs).

V tomto článku získáte informace o těchto tématech:

> [!div class="checklist"]
> * Porovnání charakteru agenta zabezpečení
> * Zjistit podporované platformy agenta
> * Volba správného charakteru agenta pro vaše řešení

## <a name="understand-security-agent-options"></a>Pochopení možností agenta zabezpečení

Každý Azure Security Center pro charakter služby IoT Security Agent nabízí stejnou sadu funkcí a podporuje podobné možnosti konfigurace.

Agent zabezpečení založený na jazyce C má nižší nároky na paměť a je ideální volbou pro zařízení s menšími dostupnými prostředky.

|     | Agent zabezpečení založený na jazyce C | Agent zabezpečení založený na jazyce C# |
| --- | ----------- | --------- |
| **Open Source** | K dispozici v rámci [licence MIT](https://en.wikipedia.org/wiki/MIT_License) na [GitHubu](https://aka.ms/iot-security-github-c) | K dispozici v rámci [licence MIT](https://en.wikipedia.org/wiki/MIT_License) na [GitHubu](https://aka.ms/iot-security-github-cs) |
| **Vývojový jazyk**    | C | C# |
| **Podporované platformy Windows?** | No | Yes |
| **Požadavky Windows** | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| **Podporované platformy Linux?** | Ano, x64 a x86 | Ano, pouze x64 |
| **Předpoklady pro Linux** | libunwind8, libcurl3, UUID – modul runtime, audit, audispd – moduly plug-in | libunwind8, libcurl3, UUID-runtime, audited, audispd-plugins, sudo, netstat, softwaru iptables |
| **Nároky na disk** | 10,5 MB | 90 MB |
| **Nároky na paměť (v průměru)** | 5,5 MB | 33 MB |
| **[Ověřování](concept-security-agent-authentication-methods.md) pro IoT Hub** | Ano | Ano |
| **[Shromažďování](how-to-agent-configuration.md#supported-security-events) dat zabezpečení** | Ano | Ano |
| **Agregace událostí** | Ano | Ano |
| **Vzdálená konfigurace prostřednictvím [modulu zabezpečení je dvojitá](concept-security-module.md) .** | Ano | Ano |

## <a name="security-agent-installation-guidelines"></a>Pokyny k instalaci agenta zabezpečení

Pro **Windows**: skript Install SecurityAgent.ps1 se musí spustit z okna PowerShellu pro správu.

Pro **Linux**: InstallSecurityAgent.sh musí být spuštěný jako uživatel. Doporučujeme použít předponu instalačního příkazu s názvem "sudo".

## <a name="choose-an-agent-flavor"></a>Volba charakteru agenta

Odpovězte na následující otázky týkající se zařízení IoT a vyberte správného agenta:

- Používáte _Windows Server_ nebo _Windows IoT Core_?

    [Nasazení agenta zabezpečení založeného na jazyce C# pro systém Windows](how-to-deploy-windows-cs.md).

- Používáte distribuci Linux s architekturou x86?

    [Nasazení agenta zabezpečení založeného na jazyce C pro Linux](how-to-deploy-linux-c.md).

- Používáte distribuci systému Linux s architekturou x64?

    Lze použít jak charakter agenta. <br>
    [Nasazení agenta zabezpečení založeného na jazyce C pro Linux](how-to-deploy-linux-c.md) nebo [nasazení agenta zabezpečení založeného na jazyce C# pro Linux](how-to-deploy-linux-cs.md).

Charakter obou agentů nabízí stejnou sadu funkcí a podporují podobné možnosti konfigurace.
Další informace najdete v tématu [porovnání agenta zabezpečení](how-to-deploy-agent.md#understand-security-agent-options) .

## <a name="supported-platforms"></a>Podporované platformy

Následující seznam obsahuje všechny aktuálně podporované platformy.

|Azure Security Center pro agenta IoT |Operační systém |Architektura |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |    x64|
|C|Ubuntu 18.04 |    x64, ARMv7|
|C|Debian 9 |    x64, x86|
|C#|Ubuntu 16.04     |x64|
|C#|Ubuntu 18.04    |x64, ARMv7|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    x64|
|C#|Windows 10 IoT Core, Build 17763    |x64|
|

## <a name="next-steps"></a>Další kroky

Pokud chcete získat další informace o možnostech konfigurace, přejděte k příručce Průvodce konfigurací agenta.
> [!div class="nextstepaction"]
> [Průvodce konfigurací agenta](./how-to-agent-configuration.md)
