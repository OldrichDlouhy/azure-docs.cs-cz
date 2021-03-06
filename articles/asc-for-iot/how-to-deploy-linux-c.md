---
title: Nainstalovat & nasazení agenta pro Linux C
description: Naučte se, jak nainstalovat Azure Security Center pro agenta IoT na 32 i 64.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 3ccf2aec-106a-4d2c-8079-5f3e8f2afdcb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: d9f9602a19a266c70b17422e90566f72de2978f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311191"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Nasazení agenta zabezpečení Azure Security Center pro IoT založeného na C pro Linux

Tato příručka vysvětluje, jak nainstalovat a nasadit Azure Security Center pro agenta zabezpečení založeného na službě IoT C v systému Linux.

V této příručce se naučíte:

> [!div class="checklist"]
> * Instalace
> * Ověření nasazení
> * Odinstalace agenta
> * Řešení potíží

## <a name="prerequisites"></a>Požadavky

Další typy platforem a agentů najdete v tématu [Volba správného agenta zabezpečení](how-to-deploy-agent.md).

1. Chcete-li nasadit agenta zabezpečení, jsou na počítači, na kterém chcete nainstalovat nástroj, požadovány oprávnění místního správce (sudo).

1. [Vytvořte modul zabezpečení](quickstart-create-security-twin.md) pro zařízení.

## <a name="installation"></a>Instalace

Chcete-li nainstalovat a nasadit agenta zabezpečení, použijte následující pracovní postup:

1. Stáhněte si nejnovější verzi do počítače z [GitHubu](https://aka.ms/iot-security-github-c).

1. Extrahujte obsah balíčku a přejděte do složky _/Src/Installation_ .

1. Spuštěním následujícího příkazu přidejte do **skriptu InstallSecurityAgent** oprávnění ke spuštění:

   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Dále spusťte:

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```

   Další informace o parametrech ověřování najdete v tématu [Postup konfigurace ověřování](concept-security-agent-authentication-methods.md) .

Tento skript provede následující funkci:

1. Nainstaluje požadavky.

1. Přidá uživatele služby (s vypnutým interaktivním přihlášením).

1. Nainstaluje agenta jako **démona** – předpokládá, že zařízení používá **systém** pro správu služeb.

1. Nakonfiguruje agenta pomocí zadaných parametrů ověřování.

Další nápovědu získáte spuštěním skriptu s parametrem – Help:

```./InstallSecurityAgent.sh --help```

### <a name="uninstall-the-agent"></a>Odinstalace agenta

Chcete-li odinstalovat agenta, spusťte skript s parametrem –-Uninstall:

```./InstallSecurityAgent.sh -–uninstall```

## <a name="troubleshooting"></a>Řešení potíží

Stav nasazení ověřte spuštěním:

```systemctl status ASCIoTAgent.service```

## <a name="next-steps"></a>Další kroky

- Přečtěte si [Přehled](overview.md) služby Azure Security Center for IoT.
- Další informace o [architektuře](architecture.md) Azure Security Center pro IoT
- Povolení [služby](quickstart-onboard-iot-hub.md)
- Přečtěte si [Nejčastější dotazy](resources-frequently-asked-questions.md) .
- Vysvětlení [výstrah zabezpečení](concept-security-alerts.md)
