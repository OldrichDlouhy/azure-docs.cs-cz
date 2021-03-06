---
title: Nainstalovat & nasazení agenta pro Linux C#
description: Naučte se, jak nainstalovat Azure Security Center pro agenta IoT na 32 i 64.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2019
ms.author: mlottner
ms.openlocfilehash: 40c6ea91fd84a0f088ed770cd7c4c3ea7b8b1c91
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311150"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Nasazení agenta zabezpečení Azure Security Center pro IoT založeného na C# pro Linux

Tato příručka vysvětluje, jak nainstalovat a nasadit Azure Security Center pro agenta zabezpečení založeného na službě IoT C# na platformě Linux.

V této příručce se naučíte:

> [!div class="checklist"]
> * Instalace
> * Ověření nasazení
> * Odinstalace agenta
> * Řešení potíží

## <a name="prerequisites"></a>Požadavky

Další typy platforem a agentů najdete v tématu [Volba správného agenta zabezpečení](how-to-deploy-agent.md).

1. Chcete-li nasadit agenta zabezpečení, jsou na počítači, na kterém chcete nainstalovat, požadovány oprávnění místního správce.

1. [Vytvořte modul zabezpečení](quickstart-create-security-twin.md) pro zařízení.

## <a name="installation"></a>Instalace

Chcete-li nasadit agenta zabezpečení, použijte následující postup:

1. Stáhněte si nejnovější verzi do počítače z [GitHubu](https://aka.ms/iot-security-github-cs).

1. Extrahujte obsah balíčku a přejděte do složky _/install_ .

1. Do **skriptu InstallSecurityAgent** přidejte spuštěná oprávnění spuštěním`chmod +x InstallSecurityAgent.sh`

1. Dále spusťte následující příkaz s **kořenovými oprávněními**:

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```

   Další informace o parametrech ověřování najdete v tématu [Postup konfigurace ověřování](concept-security-agent-authentication-methods.md).

Tento skript provede následující akce:

- Nainstaluje požadavky.

- Přidá uživatele služby (s vypnutým interaktivním přihlášením).

- Nainstaluje agenta jako **démona** – předpokládá, že zařízení používá **systém** pro klasický model nasazení.

- Konfiguruje **sudoers** , aby mohl agentovi provádět určité úlohy jako kořen.

- Nakonfiguruje agenta pomocí zadaných parametrů ověřování.

Další nápovědu získáte spuštěním skriptu s parametrem – Help:`./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Odinstalace agenta

Chcete-li odinstalovat agenta, spusťte skript s parametrem-u: `./InstallSecurityAgent.sh -u` .

> [!NOTE]
> Odinstalace neodebere žádné chybějící součásti, které byly nainstalovány během instalace.

## <a name="troubleshooting"></a>Řešení potíží

1. Stav nasazení ověřte spuštěním:

    `systemctl status ASCIoTAgent.service`

1. Povolit protokolování.
   Pokud se agent nespustí, zapněte protokolování a získejte další informace.

   Zapnout protokolování podle:

   1. Otevřete konfigurační soubor pro úpravy v libovolném editoru Linux:

        `vi /var/ASCIoTAgent/General.config`

   1. Upravte následující hodnoty:

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" />
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```

       Hodnota **LogFilePath** se dá nakonfigurovat.

       > [!NOTE]
       > Doporučujeme, abyste po dokončení řešení potíží **vypnuli** protokolování. Zanechávání **protokolování zvyšuje velikost** souboru protokolu a využití dat.

   1. Restartujte agenta spuštěním:

       `systemctl restart ASCIoTAgent.service`

   1. Další informace o selhání najdete v souboru protokolu.

       Umístění souboru protokolu:`/var/ASCIoTAgent/IotAgentLog.log`

       Změňte cestu umístění souboru podle názvu, který jste zvolili pro **LogFilePath** v kroku 2.

## <a name="next-steps"></a>Další kroky

- Přečtěte si [Přehled](overview.md) služby Azure Security Center for IoT.
- Další informace o [architektuře](architecture.md) Azure Security Center pro IoT
- Povolení [služby](quickstart-onboard-iot-hub.md)
- Přečtěte si [Nejčastější dotazy](resources-frequently-asked-questions.md) .
- Vysvětlení [výstrah](concept-security-alerts.md)
