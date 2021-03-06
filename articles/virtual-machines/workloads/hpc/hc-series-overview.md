---
title: Přehled virtuálních počítačů s funkcí HC-Series – Azure Virtual Machines | Microsoft Docs
description: Přečtěte si o podpoře verze Preview pro velikost virtuálního počítače s rozhraním HC-Series v Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 7110f3417937b623260983a9d94e9e6834fc8fc9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077382"
---
# <a name="hc-series-virtual-machine-overview"></a>Základní informace o virtuálním počítači řady HC-Series

Maximalizace výkonu aplikace HPC u škálovatelných procesorů Intel Xeon vyžaduje důkladné přístup pro zpracování umístění v této nové architektuře. Tady si vydáme naši implementaci IT na virtuálních počítačích Azure HC-Series pro aplikace HPC. K odkazování na fyzickou doménu NUMA a "vNUMA" použijeme termín "pNUMA", který odkazuje na virtualizované domény NUMA. Obdobně budeme používat pojem "pCore", který odkazuje na fyzické jádra procesoru a "vCore", který odkazuje na virtualizované jádra procesoru.

Fyzický, server HC je 2 × 24 jader Intel Xeon Platinum 8168 procesorů, celkem 48 fyzických jader. Každý procesor je jedinou doménou pNUMA a má jednotný přístup k šesti kanálům DRAM. Procesory Intel Xeon Platinum mají velikost 4x větší vyrovnávací paměti L2 než v předchozích generacích (256 KB/s-> 1 MB/jader), a zároveň snižují hodnotu mezipaměti L3 v porovnání s předchozími procesory Intel (2,5 MB/jádro-> 1,375 MB/jádro).

Výše uvedená topologie přináší také konfiguraci hypervisoru HC-Series. Vyhrazujeme si pCores 0-1 a 24-25 (tj. první 2 pCores na každém soketu), abyste zajistili, že Azure hypervisor funguje bez rušivého provozu virtuálního počítače. Následně přiřadíme domény pNUMA všechny zbývající jádra k virtuálnímu počítači. Proto se virtuální počítač zobrazí:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44`jader na virtuální počítač

Virtuální počítač nemá žádné znalosti o tom, že se mu pCores 0-1 a 24-25. Proto zpřístupňuje jednotlivé vNUMA, jako kdyby nativně měly 22 jader.

Procesory Intel Xeon Platinum, Gold a stříbrné také představují síť 2D sítě na více kostcích pro komunikaci v rámci a externě pro PROCESORy. Důrazně doporučujeme připnutí procesů pro zajištění optimálního výkonu a konzistence. Připnutí procesů bude fungovat na virtuálních počítačích s procesorem HC – Series, protože základní Silicon je vystavený virtuálnímu počítači hosta. Další informace najdete v tématu [architektura Intel Xeon SP](https://bit.ly/2RCYkiE).

Následující diagram znázorňuje oddělení jader rezervovaných pro Azure hypervisor a virtuální počítač s rozhraním HC-Series.

![Oddělení jader rezervovaných pro virtuální počítač hypervisoru a sady HC-Series](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Specifikace hardwaru

| Specifikace hardwaru          | Virtuální počítač řady HC-Series                     |
|----------------------------------|----------------------------------|
| Cores                            | 44 (HT-zakázáno)                 |
| Procesor                              | Intel Xeon Platinum 8168 *        |
| Frekvence procesoru (ne AVX)          | 3,7 GHz (Single core), 2.7-3.4 GHz (všechny jádra) |
| Paměť                           | 8 GB/jádro (352 celkem)            |
| Místní disk                       | 700 GB NVMe                      |
| InfiniBand                       | 100 GB EDR Mellanox ConnectX-5 * * |
| Síť                          | 50 GB Ethernet (40 GB použitelné) Azure Second gen SmartNIC * * * |

## <a name="software-specifications"></a>Specifikace softwaru

| Specifikace softwaru     | Virtuální počítač řady HC-Series          |
|-----------------------------|-----------------------|
| Maximální velikost úlohy MPI            | 13200 jader (300 virtuálních počítačů v jednom VMSS s singlePlacementGroup = true) |
| Podpora MPI                 | MVAPICH2, OpenMPs, MPICH, Platform MPI, Intel MPI  |
| Další architektury       | Unified Communications X, libfabric, PGAS |
| Podpora Azure Storage       | STD + Premium (max. 4 disky) |
| Podpora operačního systému pro SRIOV RDMA   | CentOS/RHEL 7.6 +, SLES 12 SP4 +, WinServer 2016 + |
| Podpora Azure CycleCloud    | Yes                         |
| Podpora Azure Batch         | Ano                         |

## <a name="next-steps"></a>Další kroky

* Přečtěte si další informace o velikostech virtuálních počítačů HPC pro [Linux](../../sizes-hpc.md) a [Windows](../../sizes-hpc.md) v Azure.

* Přečtěte si další informace o [HPC](/azure/architecture/topics/high-performance-computing/) v Azure.
