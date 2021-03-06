---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 9c130fb2e032a24384f52f11957632319093e1e3
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226382"
---
| Resource | Standardní sdílené složky | Soubory ke sdílení souborů úrovně Premium |
|----------|---------------|------------------------------------------|
| Minimální velikost sdílené složky | Bez minima; průběžné platby | 100 GiB; zřízené |
| Maximální velikost sdílené složky | 100 TiB *, 5 TiB | 100 TiB |
| Maximální velikost souboru ve sdílené složce | 1 TiB | 1 TiB |
| Maximální počet souborů ve sdílené složce | Bez omezení | Bez omezení |
| Maximální počet IOPS na sdílenou složku | 10 000 IOPS *, 1 000 IOPS | 100 000 IOPS |
| Maximální počet uložených zásad přístupu na sdílení souborů | 5 | 5 |
| Cílová propustnost pro jednu sdílenou složku | až 300 MiB/s *, až 60 MiB/s.  | Viz hodnoty příchozího a odchozího sdílení souborů Premium|
| Maximální počet odchozích dat pro jednu sdílenou složku | Viz standardní propustnost cíle sdílení souborů | Až 6 204 MiB/s |
| Maximální příchozí přenos dat pro jednu sdílenou složku | Viz standardní propustnost cíle sdílení souborů | Až 4 136 MiB/s |
| Maximální počet otevřených popisovačů na soubor nebo adresář | 2 000 otevřených popisovačů | 2 000 otevřených popisovačů |
| Maximální počet snímků sdílené složky | 200 snímky sdílené složky | 200 snímky sdílené složky |
| Maximální délka názvu objektu (adresářů a souborů) | 2 048 znaků | 2 048 znaků |
| Maximální součást cesty (v cestě \A\B\C\D je každé písmeno součásti) | 255 znaků | 255 znaků |

\*Výchozí nastavení pro standardní sdílené složky je 5 TiB. podrobné informace o tom, jak zvýšit úroveň standardních 100 sdílených složek, najdete v tématu [povolení a vytváření velkých sdílených složek](../articles/storage/files/storage-files-how-to-create-large-file-share.md) .
