---
title: Dv2 a Dsv2-Series – Azure Virtual Machines
description: Specifikace pro virtuální počítače s Dv2 a Dsv2-Series.
author: joelpelley
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: c5770a288ccf0ea540c4eef7277c4a8b70839ffc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87269474"
---
# <a name="dv2-and-dsv2-series"></a>Řada Dv2 a DSv2

Dv2 a Dsv2-Series, následná na originální D-Series, má výkonnější procesor a optimální konfiguraci procesoru na paměť, která je vhodná pro většinu produkčních úloh. Dv2-Series má přibližně 35% rychlejší než řada D-Series. Dv2-Series běží na Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1 GHz (Skylake), Intel® Xeon® E5-2673 V4 2,3 GHz (Broadwell), nebo procesory Intel® Xeon® E5-2673 V3 2,4 GHz (Haswell) s technologií Intel Turbo zvyšovat 2,0. Řada Dv2-series má stejnou konfiguraci paměti a disku jako řada D.

## <a name="dv2-series"></a>Dv2-series

Velikosti řady Dv2-Series běží na Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1 GHz (Skylake) nebo Intel® Xeon® E5-2673 V4 2,3 GHz (Broadwell) nebo procesory Intel® Xeon® E5-2673 V3 2,4 GHz (Haswell) s technologií Intel Turbo 2,0.

ACU: 210–250

Premium Storage: nepodporováno

Ukládání Premium Storage do mezipaměti: nepodporováno

Migrace za provozu: podporováno

Aktualizace pro zachování paměti: podporováno

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Maximální propustnost dočasného úložiště: IOPS/čtení MB/s/zápis MB/s | Max. datových disků | Propustnost: IOPS | Maximální počet síťových karet | Očekávaná šířka pásma sítě (MB/s) |
|---|---|---|---|---|---|---|---|---|
| Standard_D1_v2 | 1  | 3,5 | 50  | 3 000 / 46 / 23    | 4  | 4×500  | 2|750   |
| Standard_D2_v2 | 2  | 7   | 100 | 6 000 / 93 / 46    | 8  | 8×500  | 2|1 500  |
| Standard_D3_v2 | 4  | 14  | 200 | 12 000 / 187 / 93  | 16 | 16×500 | 4|3000  |
| Standard_D4_v2 | 8  | 28  | 400 | 24 000 / 375 / 187 | 32 | 32×500 | 8|6000  |
| Standard_D5_v2 | 16 | 56  | 800 | 48 000 / 750 / 375 | 64 | 64x500 | 8|12000 |

## <a name="dsv2-series"></a>DSv2-series

Velikosti řady DSv2-Series běží na Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1 GHz (Skylake) nebo Intel® Xeon® E5-2673 V4 2,3 GHz (Broadwell) nebo procesory Intel® Xeon® E5-2673 V3 2,4 GHz (Haswell) s technologií Intel Turbo pro zvýšení úrovně 2,0 a používat Premium Storage.

ACU: 210–250

Premium Storage: podporováno

Ukládání Premium Storage do mezipaměti: podporováno

Migrace za provozu: podporováno

Aktualizace pro zachování paměti: podporováno

| Velikost | Virtuální procesory | Paměť: GiB | Dočasné úložiště (SSD): GiB | Max. datových disků | Maximální propustnost úložiště v mezipaměti a dočasné úložiště: IOPS/MB/s (velikost mezipaměti v GiB) | Maximální propustnost disku neuloženého v mezipaměti: IOPS/MB/s | Maximální počet síťových karet|Očekávaná šířka pásma sítě (MB/s) |
|---|---|---|---|---|---|---|---|---|
| Standard_DS1_v2 | 1  | 3,5 | 7   | 4  | 4000/32 (43)    | 3200/48   | 2|750   |
| Standard_DS2_v2 | 2  | 7   | 14  | 8  | 8000/64 (86)    | 6400/96   | 2|1 500  |
| Standard_DS3_v2 | 4  | 14  | 28  | 16 | 16000/128 (172) | 12800/192 | 4|3000  |
| Standard_DS4_v2 | 8  | 28  | 56  | 32 | 32000/256 (344) | 25600/384 | 8|6000  |
| Standard_DS5_v2 | 16 | 56  | 112 | 64 | 64000/512 (688) | 51200/768 | 8|12000 |

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
