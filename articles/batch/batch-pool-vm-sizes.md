---
title: Volba velikostí virtuálních počítačů pro fondy
description: Jak vybrat z dostupných velikostí virtuálních počítačů pro výpočetní uzly ve fondech Azure Batch
ms.topic: conceptual
ms.date: 06/10/2020
ms.custom: seodec18
ms.openlocfilehash: e56632ce66cb25bf023813f2b98be6141f651465
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86143529"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Vyberte velikost virtuálního počítače pro výpočetní uzly ve fondu Azure Batch.

Když vyberete velikost uzlu pro fond Azure Batch, můžete si vybrat z téměř všech velikostí virtuálních počítačů dostupných v Azure. Azure nabízí řadu velikostí pro virtuální počítače s operačním systémem Linux a Windows pro různé úlohy.

Pro výběr velikosti virtuálního počítače je k dispozici několik výjimek a omezení:

* Některé řady virtuálních počítačů nebo velikosti virtuálních počítačů se v dávce nepodporují.
* Některé velikosti virtuálních počítačů jsou omezené a je potřeba je nejdřív povolit, než je budete moct přidělit.

## <a name="supported-vm-series-and-sizes"></a>Podporované řady a velikosti virtuálních počítačů

### <a name="pools-in-virtual-machine-configuration"></a>Fondy v konfiguraci virtuálního počítače

Fondy dávek v konfiguraci virtuálního počítače podporují skoro všechny velikosti virtuálních počítačů ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Další informace o podporovaných velikostech a omezeních najdete v následující tabulce.

| Řada virtuálních počítačů  | Podporované velikosti |
|------------|---------|
| Basic A | Všechny velikosti *kromě* Basic_A0 (a0) |
| A | Všechny velikosti *kromě* Standard_A0 |
| Av2 | Všechny velikosti |
| B | Žádná |
| DC | Žádná |
| Dv2, DSv2 | Všechny velikosti |
| Dv3, Dsv3 | Všechny velikosti |
| Dav4<sup>1</sup> | Žádný – zatím není k dispozici |
| Dasv4<sup>1</sup> | Všechny velikosti kromě Standard_D48as_v4 Standard_D64as_v4 Standard_D96as_v4 |
| Ddv4, Ddsv4 |  Žádný – zatím není k dispozici |
| Ev3, Esv3 | Všechny velikosti kromě E64is_v3 a E64i_v3 |
| Eav4<sup>1</sup> | Všechny velikosti kromě Standard_E48a_v4 Standard_E64a_v4 Standard_E96a_v4 |
| Easv4<sup>1</sup> | Všechny velikosti kromě Standard_E48as_v4 Standard_E64as_v4 Standard_E96as_v4 |
| Edv4, Edsv4 |  Žádný – zatím není k dispozici |
| F, FS | Všechny velikosti |
| Fsv2 | Všechny velikosti |
| G, GS | Všechny velikosti |
| H | Všechny velikosti |
| Nejenom<sup>1</sup> | Všechny velikosti |
| HBv2<sup>1</sup> | Všechny velikosti |
| HC<sup>1</sup> | Všechny velikosti |
| Ls | Všechny velikosti |
| Lsv2<sup>1</sup> | Všechny velikosti |
| M<sup>1</sup> | Všechny velikosti |
| Mv2 | Žádný – zatím není k dispozici |
| NC | Všechny velikosti |
| NCv2<sup>1</sup> | Všechny velikosti |
| NCv3<sup>1</sup> | Všechny velikosti |
| ND<sup>1</sup> | Všechny velikosti |
| NDv2<sup>1</sup> | Žádný – zatím není k dispozici |
| NV | Všechny velikosti |
| NVv3<sup>1</sup> | Všechny velikosti |
| NVv4 | Žádná |
| SAP HANA | Žádná |

<sup>1</sup> tyto velikosti virtuálních počítačů je možné přidělit ve fondech Batch v konfiguraci virtuálního počítače, musíte ale vytvořit nový účet Batch a požádat o konkrétní [zvýšení kvóty](batch-quota-limit.md#increase-a-quota). Toto omezení se odebere, až se vCPU kvóta pro každou řadu virtuálních počítačů plně podporuje pro účty Batch.

### <a name="pools-in-cloud-service-configuration"></a>Fondy v konfiguraci cloudové služby

Fondy dávek v konfiguraci cloudové služby podporují všechny [velikosti virtuálních počítačů pro Cloud Services](../cloud-services/cloud-services-sizes-specs.md) **s výjimkou** následujících:

| Řada virtuálních počítačů  | Nepodporované velikosti |
|------------|-------------------|
| A-Series   | Velmi malý       |
| Av2-series | Standard_A1_v2, Standard_A2_v2 Standard_A2m_v2 |

## <a name="size-considerations"></a>Informace o velikosti

* **Požadavky na aplikaci** – zvažte charakteristiky a požadavky aplikace, které budete spouštět na uzlech. To, jestli je aplikace vícevláknová, kolik paměti používá, a další aspekty vám mohou pomoci při určování nejvhodnější a cenově nejefektivnější velikosti uzlu. Pro [úlohy MPI](batch-mpi.md) nebo CUDA aplikace v několika instancích zvažte specializované velikosti virtuálních počítačů s podporou [HPC](../virtual-machines/sizes-hpc.md) nebo [GPU](../virtual-machines/sizes-gpu.md) . (Viz [použití instancí podporujících technologii RDMA nebo GPU ve fondech Batch](batch-pool-compute-intensive-sizes.md).)

* **Úkoly na uzel** – typický výběr velikosti uzlů za předpokladu, že jedna úloha běží na uzlu v jednom okamžiku. Může však být výhodné mít více úloh (a proto více instancí aplikace) [paralelně](batch-parallel-node-tasks.md) na výpočetních uzlech během provádění úlohy. V tomto případě je běžné zvolit velikost vícejádrových uzlů, která bude vyhovovat zvýšené poptávce při provádění paralelních úkolů.

* **Úrovně zatížení pro různé úlohy** – všechny uzly ve fondu mají stejnou velikost. Pokud máte v úmyslu spouštět aplikace s různými požadavky na systém nebo úrovně zatížení, doporučujeme vám používat oddělené fondy.

* **Dostupnost oblasti** – řada nebo velikost virtuálních počítačů nemusí být k dispozici v oblastech, kde vytvoříte účty Batch. Chcete-li ověřit, zda je velikost k dispozici, přečtěte si téma [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/).

* **Kvóty** – [kvóty jader](batch-quota-limit.md#resource-quotas) v účtu Batch můžou omezit počet uzlů dané velikosti, které můžete přidat do fondu služby Batch. Chcete-li požádat o zvýšení kvóty, přečtěte si [Tento článek](batch-quota-limit.md#increase-a-quota). 

* **Konfigurace fondu** – obecně platí více možností velikosti virtuálních počítačů při vytváření fondu v konfiguraci virtuálního počítače v porovnání s konfigurací cloudové služby.

## <a name="next-steps"></a>Další kroky

* Přečtěte si o [pracovních postupech služby Batch a primárních prostředcích](batch-service-workflow-features.md) , jako jsou fondy, uzly, úlohy a úkoly.
* Informace o použití velikosti virtuálních počítačů náročných na výpočetní výkon najdete [v tématu použití instancí s podporou RDMA nebo GPU ve fondech služby Batch](batch-pool-compute-intensive-sizes.md).
