---
title: Co je IP adresa 168.63.129.16? | Dokumentace Microsoftu
description: Přečtěte si o 168.63.129.16 IP adres, konkrétně o tom, jak se používá k usnadnění komunikačního kanálu k prostředkům platformy Azure.
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: genli
ms.openlocfilehash: 0f0bfa693086a3a097df219132d696a1d04e6f56
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286032"
---
# <a name="what-is-ip-address-1686312916"></a>Co je IP adresa 168.63.129.16?

IP adresa 168.63.129.16 je virtuální veřejná IP adresa, která se používá k usnadnění komunikačního kanálu k prostředkům platformy Azure. Zákazníci můžou v Azure definovat libovolný adresní prostor pro svou privátní virtuální síť. Proto se prostředky platformy Azure musí prezentovat jako jedinečná veřejná IP adresa. Tato virtuální veřejná IP adresa usnadňuje následující akce:

- Povolí agentovi virtuálního počítače komunikovat s platformou Azure k signalizaci, že je ve stavu "připraveno".
- Umožňuje komunikaci s virtuálním serverem DNS poskytovat filtrované rozlišení názvů k prostředkům (například k virtuálnímu počítači), které nemají vlastní server DNS. Toto filtrování zajistí, že zákazníci můžou přeložit jenom názvy hostitelů svých prostředků.
- Povolí [sondy stavu z Azure Load Balancer](../load-balancer/load-balancer-custom-probe-overview.md) a určí stav virtuálních počítačů.
- Umožňuje virtuálnímu počítači získat dynamickou IP adresu ze služby DHCP v Azure.
- Povoluje zprávy prezenčního signálu agenta hosta pro roli PaaS.

## <a name="scope-of-ip-address-1686312916"></a>Rozsah IP adres 168.63.129.16

Veřejná IP adresa 168.63.129.16 se používá ve všech oblastech a v národních cloudech. Tato speciální veřejná IP adresa je vlastněná Microsoftem a nebude se měnit. Tuto IP adresu doporučujeme v jakýchkoli místních zásadách brány firewall (ve virtuálním počítači) Povolte (odchozí směr). Komunikace mezi touto zvláštní IP adresou a prostředky je bezpečná, protože z této IP adresy může zdroj zprávy vytvořit jenom interní platforma Azure. Pokud je tato adresa zablokovaná, může dojít k neočekávanému chování v nejrůznějších scénářích. 168.63.129.16 je [virtuální IP adresa uzlu hostitele](../virtual-network/security-overview.md#azure-platform-considerations) , protože nepodléhá uživatelsky definovaným trasám.

- Agent virtuálního počítače vyžaduje odchozí komunikaci přes porty 80, 443, 32526 s WireServer (168.63.129.16). Ty by měly být otevřené v místní bráně firewall na virtuálním počítači. Komunikace na těchto portech s 168.63.129.16 nepodléhá nakonfigurovaným skupinám zabezpečení sítě.
- 168.63.129.16 může virtuálnímu počítači poskytnout služby DNS. Pokud to není žádoucí, můžete tento provoz zablokovat v místní bráně firewall na virtuálním počítači. Ve výchozím nastavení se komunikace DNS neřídí nakonfigurovanými skupinami zabezpečení sítě, pokud není konkrétně cílena využití značky služby [AzurePlatformDNS](../virtual-network/service-tags-overview.md#available-service-tags) . Pokud chcete zablokovat provoz DNS Azure DNS prostřednictvím NSG, vytvořte odchozí pravidlo pro odepření provozu do [AzurePlatformDNS](../virtual-network/service-tags-overview.md#available-service-tags)a jako protokol zadejte "*" jako "rozsahy cílových portů" a "any".
- Pokud je virtuální počítač součástí fondu back-endu nástroje pro vyrovnávání zatížení, musí být komunikace [sondy stavu](../load-balancer/load-balancer-custom-probe-overview.md) povolena z 168.63.129.16. Výchozí konfigurace skupiny zabezpečení sítě má pravidlo, které tuto komunikaci povoluje. Toto pravidlo využívá značku služby [AzureLoadBalancer](../virtual-network/service-tags-overview.md#available-service-tags) . Pokud chcete tento provoz zablokovat tak, že nakonfigurujete skupinu zabezpečení sítě, bude to mít za následek selhání sond.

Ve scénáři nevirtuální sítě (Classic) se sonda stavu nachází z privátní IP adresy a 168.63.129.16 se nepoužije.

## <a name="next-steps"></a>Další kroky

- [Skupiny zabezpečení](security-overview.md)
- [Vytvoření, změna nebo odstranění skupiny zabezpečení sítě](manage-network-security-group.md)
