---
title: Výkon virtuálního počítače s více řadami – Azure Virtual Machines | Microsoft Docs
description: Přečtěte si o výsledcích testování výkonu pro velikosti virtuálních počítačů s velmi řadou v Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: 4745fda5417ea937c367a116f999070bd62cf8cd
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077416"
---
# <a name="hb-series-virtual-machine-sizes"></a>Velikosti virtuálních počítačů s nejvyšší řadou

Několik testů výkonu bylo spuštěno na velikostech s velmi řadou. Níže jsou uvedeny některé výsledky tohoto testování výkonu.


| Úloha                                        | HB                    |
|-------------------------------------------------|-----------------------|
| Triad STREAMu                                    | ~ 260 GB/s (32-33 GB/s za CCX)  |
| Vysoce výkonné Linpack (HPL)                  | ~ 1 000 GigaFLOPS (Rpeak), ~ 860 GigaFLOPS (Rmax) |
| Latence & šířky pásma RDMA                        | 2.35 USEC, 96,5 GB/s   |
| FIO na místní disk SSD NVMe                           | čtení z ~ 1,7 GB/s, ~ 1,0 GB/s zápisy      |  
| IOR na 4 * Azure SSD úrovně Premium (P30 Managed Disks, RAID0) * *  | × 725 MB/s čtení, ~ 780 MB/zápisy   |



## <a name="infiniband-send-latency"></a>Latence odesílání InfiniBand
Mellanox Perftest.

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```


|  #bytes         | #iterations     | t_min [mikrosekunda]     | t_max [mikrosekunda]     | t_typical [mikrosekunda] | t_avg [mikrosekunda]     | t_stdev [mikrosekunda]   |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| 2               | 1000            | 2,35            | 12,63           | 2,38            | 2,42            | 0,33            |
| 4               | 1000            | 2,35            | 18,53           | 2,38            | 2,4             | 0,21            |
| 8               | 1000            | 2,36            | 6,06            | 2,39            | 2,41            | 0.22            |
| 16              | 1000            | 2,36            | 6,05            | 2,39            | 2,41            | 0,21            |
| 32              | 1000            | 2,37            | 18,93           | 2,4             | 2,42            | 0,25            |
| 64              | 1000            | 2,39            | 17,98           | 2,43            | 2.45            | 0.18            |
| 128             | 1000            | 2,44            | 19,4            | 2,76            | 2,65            | 0,29            |
| 256             | 1000            | 3,06            | 18,31           | 3.1             | 3,12            | 0,27            |
| 512             | 1000            | 3.15            | 7,89            | 3.2             | 3.23            | 0,31            |
| 1024            | 1000            | 3,27            | 17,62           | 3,31            | 3,33            | 0.22            |
| 2 048            | 1000            | 3,48            | 7,94            | 3,52            | 3,55            | 0.26            |
| 4 096            | 1000            | 3,91            | 7.7             | 3,96            | 3,98            | 0,21            |


## <a name="osu-mpi-latency-test"></a>Test latence OSU MPI

OSU MPI latence testu v 5.4.3.

```azure-cli
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```


| #bytes  | Latence [mikrosekunda] (MPICH 3,3 + CH4) | Latence [mikrosekunda] (OpenMPs 4.0.0) | Latence [mikrosekunda] (MVAPICH2 2,3) | Latence [mikrosekunda] (Intel MPI 2019) |
|------|----------|----------|----------|----------|
| 2    | 2,44     | 2.52     | 2,84     | 2,76     |
| 4    | 2,44     | 2,53     | 2,84     | 2,76     |
| 8    | 2,44     | 2,53     | 2,83     | 2,76     |
| 16   | 2.45     | 2,53     | 2,87     | 2,77     |
| 32   | 2,62     | 2,69     | 2,89     | 2,78     |
| 64   | 2,72     | 2,79     | 2.93     | 2,85     |
| 128  | 2,76     | 2,88     | 3,06     | 2,91     |
| 256  | 3,53     | 3,65     | 3.73     | 3,57     |
| 512  | 3,68     | 3,78     | 3,81     | 3,70     |
| 1024 | 3,86     | 3,97     | 3,95     | 3,93     |
| 2 048 | 4,12     | 4.5      | 4,24     | 4,22     |
| 4 096 | 4,79     | 5,28     | 6,33     | 4,91     |


## <a name="mpi-bandwidth"></a>MPI šířka pásma

OSU MPI Bandwidth test šířky pásma v 5.4.3.

```azure-cli
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

| #Size            | Šířka pásma (MB/s) | Šířka pásma (GB/s) |
|------------------|------------------|------------------|
| 2                | 4,03             | 0,03             |
| 4                | 8.2              | 0,07             |
| 8                | 16,15            | 0.13             |
| 16               | 32,33            | 0.26             |
| 32               | 64,36            | 0,51             |
| 64               | 126,29           | 1.01             |
| 128              | 234,14           | 1,87             |
| 256              | 486,89           | 3,90             |
| 512              | 874,24           | 6,99             |
| 1024             | 1538,47          | 12,31            |
| 2 048             | 2743,98          | 21,95            |
| 4 096             | 4194,69          | 33,56            |
| 8192             | 5657,67          | 45,26            |
| 16384            | 7618,96          | 60,95            |
| 32768            | 10333,76         | 82,67            |
| 65536            | 11171,06         | 89,37            |
| 131072           | 11539,64         | 92,32            |
| 262144           | 11768,43         | 94,15            |
| 524288           | 11908,59         | 95,27            |
| 1048576          | 12012,8          | 96,10            |
| 2097152          | 12049,38         | 96,40            |
| 4194304          | 12061,33         | 96,49            |


## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [vysoce výkonném výpočetním](/azure/architecture/topics/high-performance-computing/) prostředí v Azure.
