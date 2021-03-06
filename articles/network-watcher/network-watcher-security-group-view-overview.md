---
title: Úvod do zobrazení efektivních pravidel zabezpečení v Azure Network Watcher | Microsoft Docs
description: Tato stránka poskytuje přehled možností zobrazení Network Watcher efektivních pravidel zabezpečení.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: damendo
ms.openlocfilehash: f4c601184a060c3dfc4f033bcf983bf773f7167f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87022646"
---
# <a name="introduction-to-effective-security-rules-view-in-azure-network-watcher"></a>Úvod do zobrazení efektivních pravidel zabezpečení v Azure Network Watcher

Skupiny zabezpečení sítě jsou přidružené na úrovni podsítě nebo na úrovni síťové karty. V případě, že se přidruží na úrovni podsítě, platí pro všechny instance virtuálních počítačů v podsíti. Zobrazení platná pravidla zabezpečení vrací všechna nakonfigurovaná skupin zabezpečení sítě a pravidla, která jsou přidružená na síťové kartě a na úrovni podsítě pro virtuální počítač, který poskytuje přehled o konfiguraci. Kromě toho jsou pro každou ze síťových adaptérů ve virtuálním počítači vracena platná pravidla zabezpečení. Pomocí zobrazení efektivních pravidel zabezpečení můžete vyhodnotit virtuální počítač pro ohrožení zabezpečení sítě, například otevřít porty. Můžete také ověřit, jestli vaše skupina zabezpečení sítě funguje podle očekávání, na základě [porovnání mezi nakonfigurovanými a schválenými pravidly zabezpečení](network-watcher-nsg-auditing-powershell.md).

Rozšířený případ použití je v souladu se zabezpečením a auditováním. Můžete definovat sadu pravidel zabezpečení jako model pro řízení zabezpečení ve vaší organizaci. Pravidelný audit dodržování předpisů se dá implementovat programovým způsobem tím, že se porovnávají pravidla pro všechny virtuální počítače ve vaší síti s platnými pravidly.

V pravidlech portálu se zobrazují pro každé síťové rozhraní a seskupují podle příchozího vs odchozího. To poskytuje jednoduché zobrazení do pravidel použitých pro virtuální počítač. K dispozici je tlačítko Stáhnout pro snadné stažení všech pravidel zabezpečení bez ohledu na kartu na soubor CSV.

![zobrazení skupiny zabezpečení][1]

Můžete vybrat pravidla a otevře se nové okno, ve kterém se zobrazí skupina zabezpečení sítě a předpony zdroje a cíle. Z tohoto okna můžete přejít přímo k prostředku skupiny zabezpečení sítě.

![drobná][2]

### <a name="next-steps"></a>Další kroky

Funkci *efektivních skupin zabezpečení* můžete použít také prostřednictvím jiných metod uvedených níže:
* [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/NetworkInterfaces/ListEffectiveNetworkSecurityGroups)
* [PowerShell](https://docs.microsoft.com/powershell/module/az.network/get-azeffectivenetworksecuritygroup?view=azps-4.4.0)
* [Azure CLI](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-list-effective-nsg)

Informace o tom, jak auditovat nastavení skupiny zabezpečení sítě, najdete v tématu [audit nastavení skupiny zabezpečení sítě pomocí PowerShellu](network-watcher-nsg-auditing-powershell.md) .

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









