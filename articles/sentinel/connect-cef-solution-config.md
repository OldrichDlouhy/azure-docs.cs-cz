---
title: Konfigurace řešení zabezpečení pro připojení CEF dat ke službě Azure Sentinel Preview | Microsoft Docs
description: Naučte se konfigurovat řešení zabezpečení pro připojení dat CEF k ověřovací konfiguraci Azure.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 1c25e48bd46f0d37330f693cb4d6538e7bc29c4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85367236"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>Krok 2: konfigurace řešení zabezpečení pro posílání zpráv CEF

V tomto kroku provedete potřebné změny konfigurace v samotném řešení zabezpečení k odesílání protokolů agentu CEF.

## <a name="configure-a-solution-with-a-connector"></a>Konfigurace řešení pomocí konektoru

Pokud vaše řešení zabezpečení již obsahuje existující konektor, použijte pokyny specifické pro konektor následujícím způsobem:

- [AI Vectra Detect](connect-ai-vectra-detect.md)
- [Check Point](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [ExtraHop Reveal(x)](connect-extrahop.md)
- [F5 ASM](connect-f5.md)  
- [Fortinet](connect-fortinet.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)   

## <a name="configure-any-other-solution"></a>Konfigurovat jiné řešení

Pokud pro konkrétní řešení zabezpečení neexistuje konektor, použijte následující obecné pokyny pro přesměrování protokolů do agenta CEF.

1. V článku o konkrétní konfiguraci najdete postup konfigurace řešení pro posílání zpráv CEF. Pokud vaše řešení není uvedené, na zařízení, které potřebujete k nastavení těchto hodnot, tak, aby zařízení odesílalo potřebné protokoly v potřebném formátu do agenta Azure Sentinel syslog na základě agenta Log Analytics. Tyto parametry můžete upravit v zařízení, pokud je také upravíte v procesu démona syslog na agentu služby Azure Sentinel.
    - Protokol = TCP
    - Port = 514
    - Format = CEF
    - IP adresa – zajistěte odeslání zpráv CEF na IP adresu virtuálního počítače, který jste si pro tento účel vyhradi.

   > [!NOTE]
   > Toto řešení podporuje syslog RFC 3164 nebo RFC 5424.

1. Pokud chcete použít příslušné schéma v Log Analytics pro události CEF, vyhledejte `CommonSecurityLog` .

1. Pokračujte krokem 3: [ověření připojení](connect-cef-verify.md).

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste zjistili, jak připojit zařízení CEF ke službě Azure Sentinel. Další informace o Sentinel Azure najdete v následujících článcích:
- Naučte se [, jak získat přehled o vašich datech a potenciálních hrozbách](quickstart-get-visibility.md).
- Začněte [s detekcí hrozeb pomocí služby Azure Sentinel](tutorial-detect-threats.md).
