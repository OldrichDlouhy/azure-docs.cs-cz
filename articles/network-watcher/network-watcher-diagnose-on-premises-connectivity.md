---
title: Diagnostika místního připojení prostřednictvím brány VPN
titleSuffix: Azure Network Watcher
description: Tento článek popisuje, jak diagnostikovat místní připojení prostřednictvím brány VPN pomocí řešení potíží s prostředky v Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 632a1eb7b7ac53bd3d7df3f2722d6e53277c7926
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738749"
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>Diagnostika místního připojení prostřednictvím bran VPN

Azure VPN Gateway umožňuje vytvářet hybridní řešení, které řeší nutnost zabezpečeného připojení mezi vaší místní sítí a virtuální sítí Azure. Vzhledem k tomu, že vaše požadavky jsou jedinečné, je to volba místního zařízení VPN. Azure v současné době podporuje [několik zařízení VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) , která se neustále ověřují ve spolupráci s dodavateli zařízení. Před konfigurací místního zařízení VPN zkontrolujte nastavení konfigurace specifické pro konkrétní zařízení. Podobně je Azure VPN Gateway nakonfigurovaný se sadou [podporovaných parametrů protokolu IPSec](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) , které se používají pro vytváření připojení. V současné době neexistuje způsob, jak zadat nebo vybrat konkrétní kombinaci parametrů protokolu IPsec z VPN Gateway Azure. Pro navázání úspěšného připojení mezi místními a Azure musí být nastavení místního zařízení VPN v souladu s parametry protokolu IPsec, které jsou předepsané v Azure VPN Gateway. Pokud je nastavení nesprávné, dojde ke ztrátě připojení a dokud tyto problémy nevyřešíte sami, nedošlo k jejich odhalení a obvykle trvalo hodiny k identifikaci a vyřešení problému.

Funkce řešení potíží s Azure Network Watcher vám umožní diagnostikovat všechny problémy s bránou a připojeními a během několika minut má dostatek informací, aby bylo možné problém opravit.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Scénář

Chcete nakonfigurovat připojení typu Site-to-site mezi Azure a místním prostředím pomocí FortiGate jako místní VPN Gateway. Chcete-li dosáhnout tohoto scénáře, budete potřebovat následující nastavení:

1. Brána Virtual Network – VPN Gateway v Azure
1. Brána místní sítě – místní [(Fortigate) VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) reprezentaci v cloudu Azure
1. Připojení Site-to-Site (založené na směrování) – [připojení mezi VPN Gateway a místním směrovačem](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#CreateConnection)
1. [Konfigurace FortiGate](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Podrobný návod pro konfiguraci konfigurace site-to-site najdete v tématu [vytvoření virtuální sítě s připojením typu Site-to-site pomocí Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Jedním z důležitých kroků konfigurace je konfigurace komunikačních parametrů protokolu IPsec, jakákoli Chybná konfigurace vede ke ztrátě připojení mezi místní sítí a Azure. V současné době jsou brány VPN Azure nakonfigurované tak, aby podporovaly následující parametry protokolu IPsec pro fázi 1. Jak vidíte v následující tabulce, šifrovací algoritmy podporované službou Azure VPN Gateway jsou AES256, AES128 a 3DES.

### <a name="ike-phase-1-setup"></a>Nastavení protokolu IKE fáze 1

| **Vlastnost** | **PolicyBased** | **RouteBased a standardní nebo vysoce výkonná Brána VPN Gateway** |
| --- | --- | --- |
| Verze IKE |IKEv1 |IKEv2 |
| Skupina Diffie-Hellman |Skupina 2 (1 024 bitů) |Skupina 2 (1 024 bitů) |
| Metoda ověřování |Předsdílený klíč |Předsdílený klíč |
| Algoritmy šifrování |AES256 AES128 3DES |AES256 3DES |
| Algoritmus hash |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Životnost přidružení zabezpečení (SA) Fáze 1 (čas) |28 800 sekund |28 800 sekund |

Jako uživatel byste se museli FortiGate nakonfigurovat, ukázkovou konfiguraci najdete na [GitHubu](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). Nevědomě jste si nakonfigurovali FortiGate, aby jako algoritmus hash používala SHA-512. Vzhledem k tomu, že tento algoritmus není podporovaným algoritmem pro připojení založená na zásadách, vaše připojení k síti VPN funguje.

Tyto problémy je obtížné řešit a hlavní příčiny jsou často neintuitivní. V takovém případě můžete otevřít lístek podpory a získat pomoc při řešení tohoto problému. Ale u Azure Network Watcher řešení potíží s rozhraním API můžete tyto problémy identifikovat sami.

## <a name="troubleshooting-using-azure-network-watcher"></a>Řešení potíží s používáním Azure Network Watcher

Chcete-li diagnostikovat připojení, připojte se k Azure PowerShell a spusťte `Start-AzNetworkWatcherResourceTroubleshooting` rutinu. Podrobnosti o používání této rutiny najdete v tématu [řešení potíží s Virtual Network bránou a připojením – PowerShell](network-watcher-troubleshoot-manage-powershell.md). Dokončení této rutiny může trvat až několik minut.

Po dokončení rutiny můžete přejít do umístění úložiště určeného v rutině a získat podrobné informace o problémech a protokolech. Azure Network Watcher vytvoří složku zip, která obsahuje následující soubory protokolu:

![1][1]

Otevřete soubor s názvem IKEErrors.txt a zobrazí se následující chyba s informacemi o potížích s nastavením místní IKE na chybné konfiguraci.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Můžete získat podrobné informace z Scrubbed-wfpdiag.txt o chybě, protože v tomto případě uvádí, že v takovém případě by to `ERROR_IPSEC_IKE_POLICY_MATCH` vedlo k nesprávnému fungování připojení.

Další běžnou chybnou konfigurací je zadání nesprávných sdílených klíčů. Pokud v předchozím příkladu jste určili jiné sdílené klíče, IKEErrors.txt zobrazí následující chybu: `Error: Authentication failed. Check shared key` .

Funkce řešení potíží s Azure Network Watcher umožňuje diagnostikovat a řešit potíže s VPN Gateway a připojení s jednoduchou rutinou PowerShellu. V současné době podporujeme diagnostiku následujících podmínek a pracujeme na přidávání dalších podmínek.

### <a name="gateway"></a>brána

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Pokud není zjištěna žádná chyba. |Yes|
| GatewayNotFound | Nejde najít bránu nebo bránu není zřízená. |No|
| PlannedMaintenance |  V instanci brány probíhá údržba.  |No|
| UserDrivenUpdate | V případě, že probíhá aktualizace uživatele. Může se jednat o operaci změny velikosti. | No |
| VipUnResponsive | Nelze se připojit k primární instanci brány. K tomu dojde, když sonda stavu neproběhne úspěšně. | No |
| PlatformInActive | Došlo k problému s platformou. | No|
| ServiceNotRunning | Podkladová služba není spuštěná. | No|
| NoConnectionsFoundForGateway | V bráně neexistují žádná připojení. Toto je pouze upozornění.| No|
| ConnectionsNotConnected | Žádná připojení nejsou připojená. Toto je pouze upozornění.| Yes|
| GatewayCPUUsageExceeded | Aktuální využití procesoru využití brány je > 95%. | Yes |

### <a name="connection"></a>Připojení

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Pokud není zjištěna žádná chyba. |Yes|
| GatewayNotFound | Nejde najít bránu nebo bránu není zřízená. |No|
| PlannedMaintenance | V instanci brány probíhá údržba.  |No|
| UserDrivenUpdate | V případě, že probíhá aktualizace uživatele. Může se jednat o operaci změny velikosti.  | No |
| VipUnResponsive | Nelze se připojit k primární instanci brány. K tomu dojde, když sonda stavu neproběhne úspěšně. | No |
| ConnectionEntityNotFound | Chybí konfigurace připojení. | No |
| ConnectionIsMarkedDisconnected | Připojení je označeno jako odpojeno. |No|
| ConnectionNotConfiguredOnGateway | V podkladové službě není nakonfigurované připojení. | Yes |
| ConnectionMarkedStandby | Podkladová služba je označena jako pohotovostní.| Yes|
| Authentication | Neshoda s předsdíleným klíčem. | Yes|
| PeerReachability | Partnerská brána není dostupná. | Yes|
| IkePolicyMismatch | Partnerská brána má zásady IKE, které Azure nepodporuje. | Yes|
| Chyba WfpParse | Při analýze protokolu WFP došlo k chybě. |Ano|

## <a name="next-steps"></a>Další kroky

Naučte se VPN Gateway kontrolovat možnosti připojení k prostředí PowerShell a Azure Automation v tématu [monitorování bran VPN pomocí Azure Network Watcher řešení potíží](network-watcher-monitor-with-azure-automation.md) .

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
