---
title: Velikosti virtuálních počítačů Azure – HPC | Microsoft Docs
description: Zobrazuje seznam různých velikostí dostupných pro vysoce výkonné výpočetní virtuální počítače v Azure. Uvádí informace o počtu vCPU, datových discích a síťových rozhraních a propustnosti úložiště a šířce pásma sítě pro velikosti v této sérii.
author: vermagit
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: c347f637083d8dfdf39cbd032df97bc52973465f
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2020
ms.locfileid: "87372565"
---
# <a name="high-performance-computing-vm-sizes"></a>Vysoce výkonné výpočetní velikosti virtuálních počítačů

Virtuální počítače Azure H-Series jsou navržené tak, aby poskytovaly výkon výkonné třídy, škálovatelnost MPI a cenovou efektivitu pro celou řadu úloh HPC ve skutečném světě.

[HBv2-Series](hbv2-series.md) Funkce virtuálních počítačů 200 GB/s Mellanox HDR InfiniBand, zatímco virtuální počítače s funkcí geti HC-Series 100 GB/s Mellanox EDR InfiniBand. Každý z těchto typů virtuálních počítačů je připojen v neblokujícím stromu FAT pro optimalizaci a konzistentní výkon RDMA. Virtuální počítače s HBv2 podporují adaptivní směrování a dynamický propojený přenos (DCT, ve více než standard RC a UD Transports). Tyto funkce zvyšují výkon, škálovatelnost a konzistenci aplikací a jejich využití se důrazně doporučuje.

[Řady s více procesory](hb-series.md) Virtuální počítače jsou optimalizované pro aplikace, které jsou založené na šířce pásma, jako je například kapalinová dynamika, explicitní konečná analýza elementu a modelování počasí. Virtuální počítače s funkcí 60 AMD EPYC 7551 procesory, 4 GB paměti RAM na jádro procesoru a žádné podprocesy. Platforma AMD EPYC poskytuje šířku pásma větší než 260 GB/s.

[Řada HC-Series](hc-series.md) Virtuální počítače jsou optimalizované pro aplikace, které jsou založené na hustém výpočtu, jako je například implicitní nekonečná analýza elementu, molekulová dynamika a výpočetní chemie. Virtuální počítače HC – funkce 44 Intel Xeon Platinum 8168, 8 GB paměti RAM na jádro procesoru a žádné podprocesy. Platforma Intel Xeon Platinum podporuje bohatě bohatý ekosystém softwarových nástrojů od společnosti Intel, jako je například knihovna Intel Math kernel.

[Řada H-Series](h-series.md) Virtuální počítače jsou optimalizované pro aplikace řízené vysokými kmitočty procesoru nebo velkým množstvím paměti podle základních požadavků. Virtuální počítače řady H-Series funkce 8 nebo 16 Intel Xeon E5 2667 V3 procesory, 7 nebo 14 GB paměti RAM na jádro procesoru a žádné podprocesy. Funkce H-Series 56 GB/s Mellanox FDR InfiniBand v neblokované konfiguraci stromu FAT pro zajištění konzistentního výkonu RDMA. Virtuální počítače H-series podporují Intel MPI 5. x a MS-MPI.

> [!NOTE]
> Virtuální počítače A8 – A11 jsou plánovány k vyřazení na 3/2021. Další informace najdete v tématu [Průvodce migrací HPC](https://azure.microsoft.com/resources/hpc-migration-guide/).

## <a name="rdma-capable-instances"></a>Instance s podporou RDMA

Většina velikostí virtuálních počítačů HPC (HBv2, Get, HC, H16r, H16mr, A8 a c) funguje jako síťové rozhraní pro připojení vzdáleného přímého přístupu do paměti (RDMA). Vybrané velikosti [řady N-Series](./nc-series.md) určené pomocí r (ND40rs_v2, ND24rs, NC24rs_v3, NC24rs_v2 a NC24r) jsou taky podporující RDMA. Toto rozhraní je navíc ke standardním síťovým rozhraním Azure, které je dostupné v dalších velikostech virtuálních počítačů.

Toto rozhraní umožňuje, aby instance s podporou RDMA komunikovaly přes síť InfiniBand (IB), která pracuje s sazbami HDR pro HBv2, EDR sazbami pro NDv2, FDR sazby pro H16r, H16mr a další virtuální počítače řady N-Series s podporou RDMA a QDR sazby pro virtuální počítače A8 a c. Tyto možnosti RDMA můžou zvýšit škálovatelnost a výkon určitých aplikací MPI (Message Passing Interface). Další informace o rychlosti najdete v podrobnostech v tabulkách na této stránce.

> [!NOTE]
> V prostředí Azure HPC existují dvě třídy virtuálních počítačů v závislosti na tom, jestli mají rozhraní SR-IOV povolené pro InfiniBand. V současné době je rozhraní SR-IOV pro virtuální počítače s povolenou InfiniBand: HBv2,, HC, NCv3 a NDv2. Na ostatních virtuálních počítačích s povolenou InfiniBand nejsou aktuálně povoleny SR-IOV.
> U všech virtuálních počítačů podporujících RDMA je podpora RDMA přes IB podporovaná.
> IP přes IB se podporuje jenom na virtuálních počítačích s povolenou SR-IOV.

- **Operační systém** – Linux je velmi dobře podporovaný pro virtuální počítače HPC; distribuce, jako je CentOS, RHEL, Ubuntu, SUSE, se běžně používají. Týkající se podpory Windows je podpora Windows serveru 2016 a novějších verzí na všech virtuálních počítačích řady HPC. Windows Server 2012 R2, Windows Server 2012 se podporuje taky na virtuálních počítačích s povolenou instancí nevyužívajících SR-IOV (H16r, H16mr, A8 a c \ c). Upozorňujeme, že [systém Windows Server 2012 R2 není podporován na HBv2 a dalších virtuálních počítačích s více než 64 (virtuálními nebo fyzickými) jádry](/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

- **Ovladače InfiniBand a RDMA** – na virtuálních počítačích s povolenou InfiniBand jsou k povolení RDMA potřeba příslušné ovladače. V systému Linux jsou image virtuálních počítačů CentOS-HPC na webu Marketplace předem nakonfigurované s příslušnými ovladači. Image virtuálních počítačů s Ubuntu se dají nakonfigurovat pomocí správných ovladačů podle [pokynů uvedených tady](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351). U virtuálních počítačů s podporou SR-IOV a N-Series je možné pomocí [rozšíření INFINIBANDDRIVERLINUX VM](./extensions/hpc-compute-infiniband-linux.md) nainstalovat ovladače Mellanox OFED a povolit InfiniBand. Přečtěte si další informace o povolení InfiniBand pro úlohy prostředí [HPC](./workloads/hpc/overview.md)s podporou RDMA.

V systému Windows [rozšíření virtuálního počítače InfiniBandDriverWindows](./extensions/hpc-compute-infiniband-windows.md) nainstaluje ovladače síťové technologie Windows (na virtuálních počítačích bez SR-IOV) nebo ovladače Mellanox OFED (na virtuálních počítačích s SR-IOV) pro připojení RDMA. V některých nasazeních instancí A8 a A8 se rozšíření HpcVmDrivers přidá automaticky. Všimněte si, že rozšíření virtuálního počítače HpcVmDrivers je zastaralé; nebude aktualizován.

Pokud chcete přidat rozšíření virtuálního počítače do virtuálního počítače, můžete použít rutiny [Azure PowerShell](/powershell/azure/) . Další informace najdete v tématu [rozšíření a funkce virtuálních počítačů](./extensions/overview.md). Můžete také pracovat s rozšířeními pro virtuální počítače nasazené v [modelu nasazení Classic](/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic).

- **MPI** – velikosti virtuálních počítačů s povoleným rozhraním SR-IOV v Azure (HBv2,, HC, NCv3, NDv2) umožňují téměř jakýkoli charakter MPI pro použití s Mellanox OFED.
V případě virtuálních počítačů s podporou SR-IOV podporované implementace MPI používají ke komunikaci mezi virtuálními počítači rozhraní Microsoft Network Direct (ND). Proto jsou podporovány pouze verze Microsoft MPI (MS-MPI) 2012 R2 nebo novější a Intel MPI 5. x. Novější verze (2017, 2018) běhové knihovny Intel MPI mohou nebo nemusí být kompatibilní s ovladači Azure RDMA.

- **Adresní prostor síťového adres RDMA** – síť RDMA v Azure rezervuje adresní prostor 172.16.0.0/16. Pokud chcete spouštět aplikace MPI na instancích nasazených ve službě Azure Virtual Network, ujistěte se, že adresní prostor virtuální sítě nepřekrývá síť RDMA.

## <a name="cluster-configuration-options"></a>Možnosti konfigurace clusteru

Azure poskytuje několik možností pro vytváření clusterů virtuálních počítačů Windows HPC, které mohou komunikovat pomocí sítě RDMA, včetně těchto: 

- **Virtuální počítače** – nasazení virtuálních počítačů HPC podporujících RDMA ve stejné sadě škálování nebo skupině dostupnosti (při použití modelu nasazení Azure Resource Manager). Pokud používáte model nasazení Classic, nasaďte virtuální počítače do stejné cloudové služby.

- **Virtual Machine Scale Sets** – v sadě škálování virtuálního počítače Nezapomeňte toto nasazení omezit na jednu skupinu umístění pro InfiniBand komunikaci v rámci sady škálování. Například v šabloně Správce prostředků nastavte `singlePlacementGroup` vlastnost na `true` . Všimněte si, že maximální velikost sady škálování, kterou je možné pomocí vlastnosti vystavit, `singlePlacementGroup` `true` je omezené na 100 virtuálních počítačů ve výchozím nastavení. Pokud vaše požadavky na škálování úlohy HPC jsou vyšší než 100 virtuálních počítačů v jednom tenantovi, můžete požádat o zvýšení, [otevřít Online žádost o zákaznickou podporu](../azure-portal/supportability/how-to-create-azure-support-request.md) bez poplatků. Omezení počtu virtuálních počítačů v jedné sadě škálování se dá zvýšit na 300. Všimněte si, že při nasazování virtuálních počítačů pomocí skupin dostupnosti je maximální limit na 200 virtuálních počítačů na skupinu dostupnosti.

- **MPI mezi virtuálními počítači** – Pokud se pro virtuální počítače (VM) vyžaduje RDMA (např. použití komunikace MPI), ujistěte se, že jsou virtuální počítače ve stejné sadě nebo skupině dostupnosti virtuálních počítačů.

- **Azure CycleCloud** – vytvoření clusteru HPC v [Azure CycleCloud](/azure/cyclecloud/) pro spouštění úloh MPI

- **Azure Batch** – vytvořte fond [Azure Batch](../batch/index.yml) pro spouštění úloh MPI. Pokud chcete používat instance náročné na výpočetní výkon při spouštění aplikací MPI s Azure Batch, přečtěte si téma [použití úloh s více instancemi ke spouštění aplikací MPI (Message Passing Interface) v Azure Batch](../batch/batch-mpi.md).

- **Sada Microsoft HPC Pack**  -  [HPC Pack](/powershell/high-performance-computing/overview) zahrnuje běhové prostředí pro MS-MPI, které používá síť Azure RDMA při nasazení na virtuální počítače Linux s podporou RDMA. Například nasazení najdete v tématu [Nastavení clusteru Linux RDMA se sadou HPC Pack pro spouštění aplikací MPI](/powershell/high-performance-computing/hpcpack-linux-openfoam).

## <a name="deployment-considerations"></a>Aspekty nasazování

- **Předplatné Azure** – Chcete-li nasadit více než několik několika instancí náročných na výpočetní výkon, vezměte v úvahu předplatné s průběžnými platbami nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

- **Ceny a dostupnost** – tyto velikosti virtuálních počítačů se nabízejí jenom v cenové úrovni Standard. Podívejte se na [produkty dostupné v oblasti a](https://azure.microsoft.com/global-infrastructure/services/) dostupnost v oblastech Azure.

- **Kvóty jader** – možná bude potřeba zvýšit kvótu jader v předplatném Azure z výchozí hodnoty. Vaše předplatné může také omezit počet jader, které můžete nasadit v určitých rodinách velikostí virtuálních počítačů, včetně řady H-Series. Chcete-li požádat o zvýšení kvóty, [otevřete online žádost o zákaznickou podporu](../azure-portal/supportability/how-to-create-azure-support-request.md) zdarma. (Výchozí omezení se můžou lišit v závislosti na vaší kategorii předplatného.)

  > [!NOTE]
  > Pokud máte velké nároky na kapacitu, obraťte se na podporu Azure. Kvóty Azure jsou úvěrovými limity, které nezaručují kapacitu. Bez ohledu na vaši kvótu se účtují jenom ty jádra, které používáte.
  
- **Virtual Network** – [virtuální síť](https://azure.microsoft.com/documentation/services/virtual-network/) Azure není nutná k používání instancí náročných na výpočetní výkon. Pro mnoho nasazení ale potřebujete alespoň cloudovou virtuální síť Azure nebo připojení typu Site-to-site, pokud potřebujete přístup k místním prostředkům. V případě potřeby vytvořte novou virtuální síť pro nasazení instancí. Přidání virtuálních počítačů náročných na výpočetní výkon do virtuální sítě ve skupině vztahů se nepodporuje.

- **Změna velikosti** – z důvodu jejich specializovaného hardwaru můžete měnit velikost jenom pro instance náročné na výpočetní výkon v rámci stejné řady velikostí (H-Series nebo N-Series). Například můžete změnit velikost virtuálního počítače H-Series jenom z jedné velikosti řady H-Series na jinou. Další okolnosti týkající se podpory ovladačů InfiniBand a disků NVMe může být potřeba vzít v úvahu pro některé virtuální počítače.


## <a name="other-sizes"></a>Jiné velikosti

- [Obecné účely](sizes-general.md)
- [Optimalizované pro výpočty](sizes-compute.md)
- [Optimalizované pro paměť](sizes-memory.md)
- [Optimalizované pro úložiště](sizes-storage.md)
- [Optimalizované z hlediska GPU](sizes-gpu.md)
- [Předchozí generace](sizes-previous-gen.md)

## <a name="next-steps"></a>Další kroky

- Další informace o optimalizaci aplikace HPC pro Azure a některých příkladů v [úlohách HPC](./workloads/hpc/overview.md) 

- Přečtěte si další informace o tom, jak [výpočetní jednotky Azure (ACU)](acu.md) vám pomůžou porovnat výpočetní výkon napříč SKU Azure.
