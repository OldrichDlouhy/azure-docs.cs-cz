---
title: Azure Load Balancer SKU
description: Přehled Azure Load Balancer SKU
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/2020
ms.author: allensu
ms.openlocfilehash: d08d7a81fddfe70593c31ac3ebd2191679ea1220
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206365"
---
# <a name="azure-load-balancer-skus"></a>Azure Load Balancer SKU

Azure Load Balancer má dva typy nebo SKU.

## <a name="sku-comparison"></a><a name="skus"></a>Porovnání skladové položky

Nástroj pro vyrovnávání zatížení podporuje skladové položky Basic i Standard. Tyto SKU se liší ve scénáři škálování, funkce a ceny. Pomocí nástroje Load Balancer úrovně Basic lze vytvořit libovolný scénář, který je možné použít pro nástroj pro vyrovnávání zatížení.

Porovnání a vysvětlení rozdílů najdete v následující tabulce. Další informace najdete v tématu [Přehled služby Azure Standard Load Balancer](load-balancer-standard-overview.md).

>[!NOTE]
> Microsoft doporučuje službu Load Balancer úrovně Standard.
Samostatné virtuální počítače, skupiny dostupnosti a škálovací sady virtuálních počítačů je možné připojit pouze k jedné skladové položce, nikdy k oběma. Nástroj pro vyrovnávání zatížení a SKU veřejné IP adresy se musí shodovat, když je používáte s veřejnými IP adresami. Služba Vyrovnávání zatížení a SKU veřejné IP adresy nejsou proměnlivé.

| | Standard Load Balancer | Základní Load Balancer |
| --- | --- | --- |
| **[Velikost fondu back-endu](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer)** | Podporuje až 1000 instancí. | Podporuje až 300 instancí. |
| **Koncové body fondu back-endu** | Všechny virtuální počítače nebo sady škálování virtuálních počítačů v jedné virtuální síti. | Virtuální počítače v jedné skupině dostupnosti nebo v sadě škálování virtuálních počítačů. |
| **[Sondy stavu](./load-balancer-custom-probe-overview.md#types)** | TCP, HTTP, HTTPS | TCP, HTTP |
| **[Chování sondy stavu](./load-balancer-custom-probe-overview.md#probedown)** | Připojení TCP zůstávají v provozu při testování instance dolů __a__ na všech sondách. | Připojení TCP zůstávají v provozu při testování instance. Všechna připojení TCP se ukončí, když jsou všechny sondy mimo provoz. |
| **Zóny dostupnosti** | Zóna – redundantní a oblasti front-endu pro příchozí a odchozí provoz. | Není k dispozici |
| **Diagnostika** | [Azure Monitor multidimenzionální metriky](./load-balancer-standard-diagnostics.md) | [Protokoly Azure Monitor](./load-balancer-monitor-log.md) |
| **Porty HA** | [K dispozici pro interní Load Balancer](./load-balancer-ha-ports-overview.md) | Není k dispozici |
| **Zabezpečení ve výchozím nastavení** | Uzavřeno do příchozích toků, pokud to není povoleno skupinou zabezpečení sítě. Upozorňujeme, že interní provoz z virtuální sítě do interního nástroje pro vyrovnávání zatížení je povolený. | Ve výchozím nastavení otevřete. Skupina zabezpečení sítě je volitelná. |
| **Odchozí pravidla** | [Konfigurace deklarativního odchozího překladu adres (NAT)](./load-balancer-outbound-rules-overview.md) | Není k dispozici |
| **Resetování protokolu TCP při nečinnosti** | [K dispozici u libovolného pravidla](./load-balancer-tcp-reset.md) | Není k dispozici |
| **[Více front-endy](./load-balancer-multivip-overview.md)** | Příchozí a [odchozí](./load-balancer-outbound-connections.md) | Pouze příchozí |
| **Operace správy** | Většina operací < 30 sekund | typických 60 až 90 sekund |
| **SLA** | [99,99 %](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | Není k dispozici | 

Další informace najdete v tématu [omezení nástroje pro vyrovnávání zatížení](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer). Podrobnosti o Load Balanceru úrovně Standard najdete v [přehledu](load-balancer-standard-overview.md), na stránce s [cenami](https://aka.ms/lbpricing) a ve [smlouvě SLA](https://aka.ms/lbsla).

## <a name="limitations"></a>Omezení

- SKU nejsou proměnlivé. SKU existujícího prostředku nemůžete změnit.
- Samostatný prostředek virtuálního počítače, prostředek sady dostupnosti nebo prostředek sady škálování virtuálního počítače můžou odkazovat na jednu SKU, nikdy ne obojí.
- [Operace přesunu předplatného](../azure-resource-manager/management/move-resource-group-and-subscription.md) nejsou podporované pro standard Load Balancer a standardní prostředky veřejné IP adresy.

## <a name="next-steps"></a>Další kroky

- Pokud chcete začít s používáním Load Balancer, přečtěte si téma [Vytvoření veřejné Standard Load Balancer](quickstart-load-balancer-standard-public-portal.md) .
- Přečtěte si o používání [Standard Load Balancer a zóny dostupnosti](load-balancer-standard-availability-zones.md).
- Seznamte se s [sondami stavu](load-balancer-custom-probe-overview.md).
- Přečtěte si o použití [Load Balancer pro odchozí připojení](load-balancer-outbound-connections.md).
- Přečtěte si o [Standard Load Balancer s pravidly pro vyrovnávání zatížení portů vysoké dostupnosti](load-balancer-ha-ports-overview.md).
- Přečtěte si další informace o [skupinách zabezpečení sítě](../virtual-network/security-overview.md).
