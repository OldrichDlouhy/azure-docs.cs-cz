---
title: Mapování sjednocení toku dat
description: Azure Data Factory transformace toku dat při mapování nové větve
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 04/27/2020
ms.openlocfilehash: 1874e3536a3dda123c7fff9726c02e5d92d88804
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "82204334"
---
# <a name="azure-data-factory-mapping-data-flow-union-transformation"></a>Azure Data Factory transformace sjednocení toku dat

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Sjednocení sloučí několik datových proudů do jednoho s sjednocením SQL těchto datových proudů jako nový výstup z transformace Union. Všechna schémata z každého vstupního datového proudu budou kombinována v toku dat, aniž by bylo nutné mít klávesu JOIN.

N-Number datových proudů můžete v tabulce nastavení kombinovat tak, že vyberete ikonu "+" vedle každého konfigurovaného řádku, včetně zdrojových dat i datových proudů z existujících transformací v toku dat.

Tady je krátký návod k videu transformace sjednocení v toku dat mapování ADF:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4vngz]

![Transformace sjednocení](media/data-flow/union.png "Sjednocení")

V takovém případě můžete zkombinovat různá metadata z více zdrojů (v tomto příkladu tři různé zdrojové soubory) a kombinovat je do jednoho datového proudu:

![Přehled transformace sjednocení](media/data-flow/union111.png "Sjednocení 1")

Chcete-li toho dosáhnout, přidejte další řádky do nastavení sjednocení zahrnutím veškerého zdroje, který chcete přidat. Nemusíte mít společné vyhledávání ani klíč JOIN:

![Nastavení transformace sjednocení](media/data-flow/unionsettings.png "Nastavení sjednocení")

Pokud po sjednocení nastavíte transformaci SELECT, budete moct překrývat překrývající se pole nebo pole, která se nejmenují ze zdrojů bez záhlaví. Kliknutím na zkontrolovat zobrazíte kombinaci metadat s 132 celkovými sloupci v tomto příkladu ze tří různých zdrojů:

![Poslední transformace sjednocení](media/data-flow/union333.png "Sjednocení 3")

## <a name="name-and-position"></a>Název a umístění

Pokud zvolíte "sjednotit podle názvu", každá hodnota sloupce se přetáhne do odpovídajícího sloupce z každého zdroje a vytvoří se nové zřetězené schéma metadat.

Pokud zvolíte "sjednotit podle pozice", každá hodnota sloupce se přetáhne do původní pozice z každého odpovídajícího zdroje, což vede k vytvoření nového kombinovaného datového proudu, kde jsou data z jednotlivých zdrojů přidána do stejného datového proudu:

![Výstup sjednocení](media/data-flow/unionoutput.png "Výstup sjednocení")

## <a name="next-steps"></a>Další kroky

Prozkoumejte podobné transformace, včetně [spojení](data-flow-join.md) a [existence](data-flow-exists.md).
