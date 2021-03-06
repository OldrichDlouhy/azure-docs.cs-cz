---
title: Výpočty s vysokým výkonem – Azure Virtual Machines | Microsoft Docs
description: Seznamte se s vysokým výpočetním výkonem v Azure.
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
ms.openlocfilehash: 723419b97dc024a700d860dd3fe61ff48073a587
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019994"
---
# <a name="optimization-for-linux"></a>Optimalizace pro Linux

Tento článek ukazuje několik klíčových postupů pro optimalizaci image operačního systému. Přečtěte si další informace o [Povolení InfiniBand](enable-infiniband.md) a optimalizaci imagí operačního systému.

## <a name="update-lis"></a>Aktualizace LIS

Pokud nasazujete pomocí vlastní image (například starší operační systém, například CentOS/RHEL 7,4 nebo 7,5), aktualizujte v aplikaci LIS na virtuálním počítači.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Uvolnit paměť

Vylepšete efektivitu tím, že automaticky uvolní paměť, aby nedocházelo k vzdálenému přístupu do paměti.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

Chcete-li toto zachovat po restartování virtuálního počítače:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Zakázat bránu firewall a SELinux

Zakáže bránu firewall a SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Zakázat cpupower

Zakáže cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Další kroky

* Přečtěte si další informace o [Povolení InfiniBand](enable-infiniband.md) a optimalizaci imagí operačního systému.

* Přečtěte si další informace o [HPC](/azure/architecture/topics/high-performance-computing/) v Azure.
