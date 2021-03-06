---
title: zahrnout soubor
description: zahrnout soubor
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/26/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a67ad4c5010cf93ff55123013a35c697ce5971f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77597786"
---
Sdílené složky Azure se nasazují do *účtů úložiště*, což jsou objekty nejvyšší úrovně, které představují sdílený fond úložiště. Tento fond úložiště se dá použít k nasazení několika sdílených složek a dalších prostředků úložiště, jako jsou kontejnery objektů blob, fronty nebo tabulky. Všechny prostředky úložiště, které jsou nasazené do účtu úložiště, sdílejí omezení vztahující se na tento účet úložiště. Aktuální omezení pro účet úložiště najdete v tématu [škálovatelnost a cíle výkonnosti souborů Azure](../articles/storage/files/storage-files-scale-targets.md).

Existují dva hlavní typy účtů úložiště, které budete používat pro nasazení souborů Azure: 
- **Účty úložiště pro obecné účely verze 2 (GPv2)**: účty úložiště GPv2 umožňují nasadit sdílené složky Azure na hardwaru založeném na standardu a na pevných discích (HDD). Kromě ukládání sdílených složek Azure můžou účty úložiště GPv2 ukládat i další prostředky úložiště, jako jsou kontejnery objektů blob, fronty nebo tabulky. 
- **Účty úložiště**úložiště: účty úložiště úložiště umožňují nasadit sdílené složky Azure na hardware Premium/Solid-State (SSD) na disku (SSD). Účty úložiště souborů se dají použít jenom k ukládání sdílených složek Azure. v účtu úložiště úložiště se nedají nasadit žádné další prostředky úložiště (kontejnery objektů blob, fronty, tabulky atd.).

V Azure Portal, PowerShellu nebo rozhraní příkazového řádku se může nacházet několik dalších typů účtů úložiště. Dva typy účtů úložiště, účty úložiště BlockBlobStorage a BlobStorage, nemůžou obsahovat sdílené složky Azure. Další dva typy účtů úložiště, které se můžou zobrazovat, jsou obecné účely verze 1 (GPv1) a klasické účty úložiště, z nichž obě můžou obsahovat sdílené složky Azure. I když účty úložiště GPv1 a Classic můžou obsahovat sdílené složky Azure, jsou většina nových funkcí služby Azure Files k dispozici pouze v účtech úložiště GPv2 a úložiště souborů. Proto doporučujeme, abyste pro nová nasazení používali jenom účty úložiště GPv2 a Storage a k upgradu GPv1 a klasických účtů úložiště, pokud už ve svém prostředí existují.  