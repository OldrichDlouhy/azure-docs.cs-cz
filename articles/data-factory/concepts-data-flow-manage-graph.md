---
title: Grafy toku dat
description: Jak pracovat s grafy toku dat Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/04/2019
ms.openlocfilehash: 0d357c4c671070a5c5e9d4587e2f90b6628996f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81605362"
---
# <a name="mapping-data-flow-graphs"></a>Mapování grafů toku dat

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Návrhová plocha mapování toků dat je "stavební" plocha, kde můžete vytvářet toky dat shora dolů, zleva doprava. Pro každou transformaci se symbolem plus (+) je připojena sada nástrojů. Soustřeďte se na obchodní logiku místo propojení uzlů pomocí okrajů v DAG prostředí s bezplatným formulářem.

Níže jsou uvedené předdefinované mechanismy pro správu grafu toku dat.

## <a name="move-nodes"></a>Přesun uzlů

![Možnosti agregované transformace](media/data-flow/agghead.png "záhlaví Agregátoru")

Bez možnosti přetahování přetahováním je možné změnit příchozí datový proud tak, aby "přesunul" transformační uzel. Místo toho převedete transformované transformace změnou "příchozího datového proudu".

## <a name="streams-of-data-inside-of-data-flow"></a>Datové proudy dat v toku dat

V Azure Data Factory tok dat prezentují toky dat. V podokně nastavení transformace se zobrazí pole příchozí Stream. Tím se dozvíte, který příchozí datový proud dodává tuto transformaci. Fyzické umístění svého transformačního uzlu můžete změnit v grafu kliknutím na název příchozího datového proudu a výběrem jiného datového proudu. Aktuální transformace spolu se všemi následnými transformacemi v tomto datovém proudu se pak přesune na nové místo.

Pokud přesouváte transformaci s jednou nebo více transformacemi po ní, bude nové umístění v toku dat připojeno prostřednictvím nové větve.

Pokud po zvoleném uzlu nemáte žádné následné transformace, pak se pouze tato transformace přesune do nového umístění.

## <a name="hide-graph-and-show-graph"></a>Skrýt graf a zobrazit graf

Na pravé straně dolního podokna konfigurace je tlačítko, kde při práci na konfiguracích transformace můžete rozbalit dolní podokno na celou obrazovku. To vám umožní procházet konfigurace grafu pomocí tlačítek předchozí a další. Chcete-li přejít zpět do zobrazení grafu, klikněte na tlačítko dolů a vraťte se k rozdělené obrazovce.

## <a name="search-graph"></a>Vyhledat graf

Graf můžete vyhledat pomocí tlačítka Hledat na návrhové ploše.

![Vyhledávání](media/data-flow/search001.png "Vyhledat graf")

## <a name="next-steps"></a>Další kroky

Po dokončení návrhu toku dat zapněte tlačítko ladění a otestujte ho v režimu ladění buď přímo v [Návrháři toku dat](concepts-data-flow-debug-mode.md) , nebo v [ladění kanálu](control-flow-execute-data-flow-activity.md).
