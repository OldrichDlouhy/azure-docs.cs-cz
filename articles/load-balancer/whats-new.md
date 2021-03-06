---
title: Co je nového v Azure Load Balancer
description: Přečtěte si, co je nového v Azure Load Balancer, jako je například nejnovější zpráva k vydání verze, známé problémy, opravy chyb, zastaralé funkce a nadcházející změny.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: overview
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: 8b44dc230dbee1b29b9889a1b81e35ebe25f6b97
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078690"
---
# <a name="whats-new-in-azure-load-balancer"></a>Co je nového v Azure Load Balancer?

Azure Load Balancer se pravidelně aktualizují. Udržujte si přehled o nejnovějších oznámeních. V tomto článku najdete informace o:

- Nejnovější verze
- Známé problémy
- Opravy chyb
- Zastaralé funkce (Pokud je k dispozici)

Nejnovější Azure Load Balancer aktualizace a přihlásit se k odběru informačního kanálu RSS můžete také najít [zde](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer).

## <a name="recent-releases"></a>Poslední verze

| Typ |Název |Popis  |Datum přidání  |
| ------ |---------|---------|---------|
| Funkce | Podpora správy fondů back-endu založených na protokolu IP (verze Preview) | Azure Load Balancer podporuje přidávání a odebírání prostředků z back-endového fondu prostřednictvím adres IPv4 nebo IPv6. To umožňuje snadnou správu kontejnerů, virtuálních počítačů a sady škálování virtuálních počítačů přidružených k Load Balancer. Umožní taky, aby se IP adresy rezervovaly jako součást fondu back-endu před vytvořením přidružených prostředků. Další informace najdete [tady](backend-pool-management.md).|Červenec 2020 |
| Funkce| Azure Load Balancer přehledy pomocí Azure Monitor | Jako součást Azure Monitor pro sítě mají zákazníci nyní topologické mapy pro všechny své Load Balancer konfigurace a řídicí panely stavu pro své standardní nástroje pro vyrovnávání zatížení, které jsou předem nakonfigurované s metrikami v Azure Portal. [Začněte a získejte další informace](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Červen 2020 |
| Ověřování | Přidání ověření pro porty HA | Bylo přidáno ověření, které zajistí, že pravidla portů HA a pravidla portů bez vysoké dostupnosti jsou konfigurovatelná pouze v případě, že je povolena plovoucí IP adresa. Dříve tato konfigurace procházela, ale nefunguje tak, jak je zamýšlená. Nebyla provedena žádná změna funkčnosti. Další informace najdete [tady](load-balancer-ha-ports-overview.md#limitations) .| Červen 2020 |
| Funkce| Podpora protokolu IPv6 pro Azure Load Balancer (všeobecně dostupná) | Pro vaše front-end služby Vyrovnávání zatížení Azure můžete mít IPv6 adresy. Další informace o tom, jak [vytvořit aplikaci s duálním zásobníkem](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) |Duben 2020|
| Funkce| Resetování TCP při nečinnosti časového limitu (všeobecně dostupné)| Použijte resety TCP k vytvoření předvídatelného chování aplikace. [Další informace](load-balancer-tcp-reset.md)| Únor 2020 |

## <a name="next-steps"></a>Další kroky

Další informace o Azure Load Balancer najdete v tématu [co je Azure Load Balancer?](load-balancer-overview.md) a [Nejčastější dotazy](load-balancer-faqs.md).
