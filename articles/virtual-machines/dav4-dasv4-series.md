---
title: Řada Dav4 a Dasv4
description: Specifikace pro virtuální počítače s Dav4 a Dasv4-Series.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 06fe0cf14346b9a1a5a1f3c093abeec1d1be159a
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292489"
---
# <a name="dav4-and-dasv4-series"></a>Řada Dav4 a Dasv4

Řady Dav4-Series a Dasv4-Series jsou novými velikostmi, které využívají procesor AMD 2.35 EPYC<sup>TM</sup> 7452 v konfiguraci s více vlákny s více než 256 MB mezipaměti L3, která vynásobí 8 MB této mezipaměti L3 každých 8 jader a zvyšuje možnosti zákazníků na spouštění jejich obecných úloh pro účely. Řady Dav4-Series a Dasv4-Series mají stejnou konfiguraci paměti a disku jako řada D & Dsv3-Series.

## <a name="dav4-series"></a>Dav4-Series

ACU: 230-260

Premium Storage: nepodporováno

Ukládání Premium Storage do mezipaměti: nepodporováno

Migrace za provozu: podporováno

Aktualizace pro zachování paměti: podporováno

Velikosti řady Dav4-Series jsou založené na procesoru AMD EPYC<sup>TM</sup> 7452 v 2.35 GHz, který může dosáhnout zvýšení maximální frekvence 3.35 GHz. Velikosti řady Dav4-Series nabízejí kombinaci vCPU, paměti a dočasného úložiště pro většinu produkčních úloh. Úložiště datových disků se účtuje nezávisle na virtuálních počítačích. Pokud chcete použít disk SSD úrovně Premium, použijte velikosti Dasv4. Měřiče cen a účtování pro velikosti Dasv4 jsou stejné jako pro Dav4-Series.

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště: IOPS / čtení v MB/s / zápis v MB/s | Maximální počet síťových karet | Očekávaná šířka pásma sítě (MB/s) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000 / 46 / 23   | 2 | 1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000 / 93 / 46   | 2 | 2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000 / 187 / 93 | 4 | 4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000 / 375 / 187 |8 | 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000 / 750 / 375 |8 | 16000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000/1000/500 | 8 | 24000 |
| Standard_D64a_v4| 64 | 256 | 1600 | 32 | 96000/1000/500 | 8 | 30000 |
| Standard_D96a_v4| 96 | 384 | 2400 | 32 | 96000/1000/500 | 8 | 30000 |

## <a name="dasv4-series"></a>Dasv4-Series

ACU: 230-260

Premium Storage: podporováno

Ukládání Premium Storage do mezipaměti: podporováno

Migrace za provozu: podporováno

Aktualizace pro zachování paměti: podporováno

Velikosti řady Dasv4-Series jsou založené na procesoru AMD EPYC<sup>TM</sup> 7452 v 2.35 GHz, který může dosáhnout zvýšení maximální frekvence 3.35 GHz a používání jednotky SSD úrovně Premium. Velikosti řady Dasv4-Series nabízejí kombinaci vCPU, paměti a dočasného úložiště pro většinu produkčních úloh.

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost dočasného úložiště a úložiště v mezipaměti: IOPS / MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku bez mezipaměti: IOPS / MB/s | Maximální počet síťových karet | Očekávaná šířka pásma sítě (MB/s) |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000/32 (50)|3200/48|2 | 1000 |
| Standard_D4as_v4|4|16|32|8|8000/64 (100)|6400/96|2 | 2000 |
| Standard_D8as_v4|8|32|64|16|16000/128 (200)|12800/192|4 | 4000 |
| Standard_D16as_v4|16|64|128|32|32000/255 (400)|25600/384|8 | 8000 |
| Standard_D32as_v4|32|128|256|32|64000/510 (800)|51200/768|8 | 16000 |
| Standard_D48as_v4|48|192|384|32|96000/1020 (1200)|76800/1148|8 | 24000 |
| Standard_D64as_v4|64|256|512|32|128000/1020 (1600)|80000/1200|8 | 30000 | 
| Standard_D96as_v4|96|384|768|32|192000/1020 (2400)|80000/1200|8 | 30000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Další velikosti a informace

- [Obecné účely](sizes-general.md)
- [Optimalizované pro paměť](sizes-memory.md)
- [Optimalizované pro úložiště](sizes-storage.md)
- [Optimalizované z hlediska GPU](sizes-gpu.md)
- [Vysokovýkonné výpočetní prostředí](sizes-hpc.md)
- [Předchozí generace](sizes-previous-gen.md)

Cenová kalkulačka: [Cenová Kalkulačka](https://azure.microsoft.com/pricing/calculator/)

Další informace o typech disků: [typy disků](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types#ultra-ssd-preview/)

## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o tom, jak [výpočetní jednotky Azure (ACU)](acu.md) vám pomůžou porovnat výpočetní výkon napříč SKU Azure.
