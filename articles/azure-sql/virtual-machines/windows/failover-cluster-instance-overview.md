---
title: Instance clusteru převzetí služeb při selhání
description: Přečtěte si o instancích clusteru s podporou převzetí služeb při selhání (FCIs SQL Server) ve službě Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: 00c9482eab74003f6a667d52440d4cb6dd21fcfc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287366"
---
# <a name="failover-cluster-instances-with-sql-server-on-azure-virtual-machines"></a>Instance clusteru s podporou převzetí služeb při selhání s SQL Server v Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Tento článek přináší rozdíly ve funkcích při práci s instancemi clusteru s podporou převzetí služeb při selhání (FCI) pro SQL Server v Azure Virtual Machines (VM). 

## <a name="overview"></a>Přehled

SQL Server na virtuálních počítačích Azure používá funkci služby Windows Server Failover Clustering (WSFC) k zajištění místní vysoké dostupnosti prostřednictvím redundance na úrovni serveru: instance clusteru s podporou převzetí služeb při selhání. FCI je jediná instance SQL Server, která je nainstalovaná v rámci služby WSFC (nebo jednoduše cluster) uzlů a může být v několika podsítích. V síti se FCI jeví jako instance SQL Server běžící v jednom počítači. Ale FCI poskytuje převzetí služeb při selhání z jednoho uzlu WSFC na jiný, pokud aktuální uzel nebude dostupný.

Zbytek článku se zaměřuje na rozdíly v instancích clusteru s podporou převzetí služeb při selhání, když se používají s SQL Server na virtuálních počítačích Azure. Další informace o technologii clusteringu s podporou převzetí služeb při selhání najdete v těchto tématech: 

- [Technologie clusterů Windows](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server instancí clusteru s podporou převzetí služeb při selhání](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

## <a name="quorum"></a>Umožněn

Instance clusteru s podporou převzetí služeb při selhání s SQL Server v Azure Virtual Machines podporují použití určujícího disku, sdílené složky v cloudu nebo určující sdílené složky pro kvorum clusteru.

Další informace najdete v tématu [osvědčené postupy pro kvorum s SQL Servermi virtuálními počítači v Azure](hadr-cluster-best-practices.md#quorum). 


## <a name="storage"></a>Storage

V tradičních místních clusterovaných prostředích používá cluster s podporou převzetí služeb při selhání systému Windows síť SAN (Storage Area Network), která je přístupná v obou uzlech jako sdílené úložiště. SQL Server soubory jsou hostované na sdíleném úložišti a přístup k souborům je možné jenom v aktivním uzlu. 

SQL Server na virtuálních počítačích Azure nabízí různé možnosti jako řešení sdíleného úložiště pro nasazení SQL Server instancí clusteru s podporou převzetí služeb při selhání: 

||[Sdílené disky Azure](../../../virtual-machines/windows/disks-shared.md)|[Soubory ke sdílení souborů úrovně Premium](../../../storage/files/storage-how-to-create-premium-fileshare.md) |[Prostory úložiště s přímým přístupem (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)|
|---------|---------|---------|---------|
|**Minimální verze operačního systému**| Vše |Windows Server 2012|Windows Server 2016|
|**Minimální verze SQL Server**|Vše|SQL Server 2012|SQL Server 2016|
|**Podporovaná dostupnost virtuálního počítače** |Skupiny dostupnosti se skupinami umístění blízkých souborů |Skupiny dostupnosti a zóny dostupnosti|Skupiny dostupnosti |
|**Podporuje FileStream**|Ano|Ne|Ano |
|**Mezipaměť objektů BLOB v Azure**|Ne|Ne|Ano|

Zbytek této části obsahuje seznam výhod a omezení jednotlivých možností úložiště, které jsou dostupné pro SQL Server na virtuálních počítačích Azure. 

### <a name="azure-shared-disks"></a>Sdílené disky Azure

[Sdílené disky Azure](../../../virtual-machines/windows/disks-shared.md) jsou funkcí služby [Azure Managed disks](../../../virtual-machines/windows/managed-disks-overview.md). Clustering s podporou převzetí služeb při selhání ve Windows serveru podporuje použití sdílených disků Azure s instancí clusteru s podporou převzetí 

**Podporovaný operační systém**: vše   
**Podporovaná verze SQL**: vše     

**Výhody**: 
- Užitečné pro aplikace, které se chtějí migrovat do Azure a současně zachovat jejich architekturu s vysokou dostupností a zotavení po havárii (HADR). 
- Může migrovat clusterové aplikace do Azure, protože je to kvůli podpoře trvalých rezervací SCSI (SCSI PR). 
- Podporuje sdílené SSD úrovně Premium Azure pro všechny verze SQL Server a sdílené Azure Ultra Disk Storage pro SQL Server 2019. 
- Může použít jeden sdílený disk nebo rozkládat více sdílených disků k vytvoření sdíleného fondu úložiště. 
- Podporuje FileStream.


**Omezení**: 
- Virtuální počítače musí být umístěné ve stejné skupině dostupnosti a skupině umístění blízkosti.
- Zóny dostupnosti se nepodporují.
- Mezipaměť SSD úrovně Premium disku není podporována.
 
Pokud chcete začít, přečtěte si téma [SQL Server instance clusteru s podporou převzetí služeb při selhání se sdílenými disky](failover-cluster-instance-azure-shared-disks-manually-configure.md) 

### <a name="storage-spaces-direct"></a>Prostory úložiště – přímé

[Prostory úložiště s přímým přístupem](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) je funkce Windows serveru, která je podporovaná s clusteringem s podporou převzetí služeb při selhání ve službě Azure Virtual Machines. Poskytuje softwarovou virtuální síť SAN.

**Podporovaný operační systém**: Windows Server 2016 a novější   
**Podporovaná verze SQL**: SQL Server 2016 a novější   


**Výhodnější** 
- Dostatečná šířka pásma sítě umožňuje robustní a vysoce výkonné řešení sdíleného úložiště. 
- Podporuje mezipaměť objektů BLOB v Azure, takže je možné je zpracovat místně z mezipaměti. (Aktualizace se replikují současně do obou uzlů.) 
- Podporuje FileStream. 

**Určitá**
- Dostupné jenom pro Windows Server 2016 a novější. 
- Zóny dostupnosti se nepodporují.
- Vyžaduje stejnou diskovou kapacitu připojenou k oběma virtuálním počítačům. 
- Vysoká šířka pásma sítě je nutná k dosažení vysokého výkonu kvůli probíhající replikaci disku. 
- Vyžaduje větší velikost virtuálního počítače a dvojí platbu za úložiště, protože úložiště je připojené ke každému virtuálnímu počítači. 

Chcete-li začít, přečtěte si téma [SQL Server prostory úložiště s přímým přístupem instance clusteru s podporou převzetí služeb při selhání](failover-cluster-instance-azure-shared-disks-manually-configure.md) 

### <a name="premium-file-share"></a>Zvýhodněná sdílení souborů

[Sdílené složky Premium](../../../storage/files/storage-how-to-create-premium-fileshare.md) jsou funkcí služby [soubory Azure](../../../storage/files/index.yml). Soubory úrovně Premium jsou back-SSD a mají konzistentně nízkou latenci. Jsou plně podporované pro použití s instancemi clusteru s podporou převzetí služeb při selhání pro SQL Server 2012 nebo novější v systému Windows Server 2012 nebo novějším. Prémiové sdílené složky poskytují větší flexibilitu, protože je možné změnit velikost sdílené složky a škálovat ji bez výpadků.

**Podporovaný operační systém**: Windows Server 2012 a novější   
**Podporovaná verze SQL**: SQL Server 2012 a novější   

**Výhodnější** 
- Jenom sdílené řešení úložiště pro virtuální počítače se šíří přes několik zón dostupnosti. 
- Plně spravovaný systém souborů s latencí s jedním číslem a výkonem vstupně-výstupních operací. 

**Určitá**
- Dostupné jenom pro Windows Server 2012 a novější. 
- FileStream není podporován. 


Pokud chcete začít, přečtěte si téma [SQL Server instance clusteru s podporou převzetí služeb při selhání se službou Premium](failover-cluster-instance-premium-file-share-manually-configure.md). 

### <a name="partner"></a>Partner

Existují řešení partnerských clusterů s podporovaným úložištěm. 

**Podporovaný operační systém**: vše   
**Podporovaná verze SQL**: vše   

V jednom příkladu se jako úložiště používá s datakeep. Další informace najdete v tématu [Clustering s podporou převzetí služeb při selhání](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/)v záznamech blogu a s datakeeping.

### <a name="iscsi-and-expressroute"></a>iSCSI a ExpressRoute

Pomocí Azure ExpressRoute můžete také zveřejnit úložiště sdíleného bloku cíle iSCSI. 

**Podporovaný operační systém**: vše   
**Podporovaná verze SQL**: vše   

Například NetApp Private Storage (NPS) zveřejňuje cíl iSCSI prostřednictvím ExpressRoute s Equinix do virtuálních počítačů Azure.

Pro řešení sdíleného úložiště a replikace dat od partnerů Microsoftu se obraťte na dodavatele s případnými problémy souvisejícími s přístupem k datům při převzetí služeb při selhání.

## <a name="connectivity"></a>Připojení

Instance clusterů s podporou převzetí služeb při selhání s SQL Server v Azure Virtual Machines použít [název distribuované sítě (DNN)](hadr-distributed-network-name-dnn-configure.md) nebo [název virtuální sítě (VNN) s Azure Load Balancer](hadr-vnn-azure-load-balancer-configure.md) ke směrování provozu do SQL Server instance bez ohledu na to, který uzel aktuálně vlastní clusterované prostředky. Při použití určitých funkcí a DNN s SQL Server FCI existují další okolnosti. Další informace najdete v tématu [interoperabilita DNN s SQL Server FCI](failover-cluster-instance-dnn-interoperability.md) . 

Další podrobnosti o možnostech připojení clusteru najdete v tématu [Směrování hadr připojení k SQL Server na virtuálních počítačích Azure](hadr-cluster-best-practices.md#connectivity). 

## <a name="limitations"></a>Omezení

Vezměte v úvahu následující omezení pro instance clusteru s podporou převzetí služeb při selhání s SQL Server v Azure Virtual Machines. 

### <a name="lightweight-resource-provider"></a>Zjednodušený poskytovatel prostředků   
V současné době se SQL Server instance clusterů s podporou převzetí služeb při selhání na virtuálních počítačích Azure podporují jenom s [režimem zjednodušené správy](sql-vm-resource-provider-register.md#management-modes) [rozšíření agenta SQL Server IaaS](sql-server-iaas-agent-extension-automate-management.md). Pokud chcete přejít z režimu úplného rozšíření na odlehčený, odstraňte prostředek **virtuálního počítače SQL** pro odpovídající virtuální počítače a pak je zaregistrujte u poskytovatele prostředků virtuálního počítače SQL ve zjednodušeném režimu. Při odstraňování prostředku **virtuálního počítače SQL** pomocí Azure Portal zrušte zaškrtnutí políčka u správného virtuálního počítače. 

Úplné rozšíření podporuje funkce, jako je automatické zálohování, opravy a Správa portálu. Tyto funkce nebudou fungovat pro SQL Server virtuální počítače po přeinstalaci agenta v režimu zjednodušené správy.

### <a name="msdtc"></a>NÁSTROJE   
Azure Virtual Machines podporuje službu MSDTC v systému Windows Server 2019 s úložištěm na sdílených svazcích clusteru (CSV) a [Azure Standard Load Balancer](../../../load-balancer/load-balancer-standard-overview.md).

V Azure Virtual Machines není služba MSDTC podporovaná pro Windows Server 2016 nebo starší, protože:

- Clusterový prostředek MSDTC nejde nakonfigurovat tak, aby používal sdílené úložiště. Pokud v systému Windows Server 2016 vytvoříte prostředek MSDTC, nezobrazí se žádné sdílené úložiště dostupné pro použití, a to i v případě, že je úložiště k dispozici. Tento problém byl opravený v systému Windows Server 2019.
- Nástroj pro vyrovnávání zatížení úrovně Basic nezpracovává porty RPC.


## <a name="next-steps"></a>Další kroky

Projděte si [osvědčené postupy konfigurace clusteru](hadr-cluster-best-practices.md)a potom můžete [připravit SQL Server virtuální počítač pro FCI](failover-cluster-instance-prepare-vm.md). 

Další informace naleznete v tématu: 

- [Technologie clusterů Windows](/windows-server/failover-clustering/failover-clustering-overview)   
- [SQL Server instancí clusteru s podporou převzetí služeb při selhání](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

