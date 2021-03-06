---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4f40a821a0a639fa117dc0844146f28d887166e1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102554"
---
Přírůstkové snímky jsou zálohy v čase pro spravované disky, které při pořízení sestávají pouze změny od posledního snímku. Při obnovení disku z přírůstkového snímku systém rekonstruuje celý disk, který představuje bod v čase zálohování disku při pořízení přírůstkového snímku. Tato nová funkce pro snímky spravovaných disků potenciálně umožňuje získat cenově výhodnější, protože pokud se nerozhodnete, nebudete muset celý disk ukládat na jednotlivé snímky. Stejně jako úplné snímky lze přírůstkové snímky použít buď k vytvoření úplného spravovaného disku, nebo úplného snímku.

Existuje několik rozdílů mezi přírůstkovým snímkem a úplným snímkem. Přírůstkové snímky budou vždy používat standardní úložiště HDD bez ohledu na typ úložiště disku, zatímco úplné snímky mohou používat Premium SSD. Pokud k horizontálnímu navýšení kapacity nasazení virtuálních počítačů používáte úplné snímky Premium Storage, doporučujeme v [galerii sdílených imagí](../articles/virtual-machines/linux/shared-image-galleries.md)používat vlastní image ve službě Storage úrovně Standard. Pomůže vám dosáhnout větší obrovské škály s nižšími náklady. Navíc přírůstkové snímky potenciálně nabízejí lepší spolehlivost pomocí [redundantního úložiště](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) zóny. Pokud je ve vybrané oblasti k dispozici ZRS, přírůstkový snímek bude automaticky používat ZRS. Pokud ZRS není v této oblasti k dispozici, bude mít tento snímek výchozí [místně redundantní úložiště](../articles/storage/common/storage-redundancy-lrs.md) (LRS). Toto chování můžete přepsat a vybrat ho ručně, ale nedoporučujeme to.

Přírůstkové snímky také nabízí rozdílovou možnost, která je k dispozici pouze pro spravované disky. Umožňují získat změny dvou přírůstkových snímků stejných spravovaných disků na úrovni bloku. Tuto možnost můžete použít ke snížení objemu dat při kopírování snímků napříč oblastmi.  Například můžete stáhnout první přírůstkový snímek jako základní objekt BLOB v jiné oblasti. Pro následné přírůstkové snímky můžete kopírovat pouze změny od posledního snímku do základního objektu BLOB. Po zkopírování změn můžete pořizovat snímky základního objektu blob, který představuje bod v čase zálohování disku v jiné oblasti. Disk můžete obnovit buď ze základního objektu blob, nebo z snímku na základním objektu BLOB v jiné oblasti.

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="Diagram znázorňující přírůstkové snímky zkopírované do různých oblastí Snímky přivedou různá volání rozhraní API, dokud se nakonec nevytvoří objekty blob stránky pro každý snímek.":::

Přírůstkové snímky se účtují jenom za použitou velikost. Velikost snímků můžete najít tak, že si prohlížíte [sestavu využití Azure](https://docs.microsoft.com/azure/billing/billing-understand-your-bill). Pokud je například použitá velikost snímku 10 GiB, zobrazí se **denní** sestava o využití 10 GIB/(31 dní) = 0,3226 jako spotřebované množství.
