---
title: Odvozená transformace sloupce v toku mapování dat
description: Naučte se, jak transformovat data ve velkém měřítku v Azure Data Factory pomocí transformace sloupce s odvozeným datovým tokem.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 38ec2d4619f47bf9fc4d1815cb6e9990cef72dcf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606507"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Odvozená transformace sloupce v toku mapování dat

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Pomocí transformace odvozeného sloupce můžete vygenerovat nové sloupce v toku dat nebo upravit existující pole.

## <a name="derived-column-settings"></a>Nastavení odvozeného sloupce

Pokud chcete přepsat existující sloupec, vyberte ho přes rozevírací seznam sloupec. V opačném případě použijte pole výběru sloupce jako textové pole a zadejte název nového sloupce. Chcete-li vytvořit výraz odvozeného sloupce, klikněte na pole zadejte výraz pro otevření [Tvůrce výrazů toku dat](concepts-data-flow-expression-builder.md).

![Nastavení odvozeného sloupce](media/data-flow/dc1.png "Nastavení odvozeného sloupce")

Chcete-li přidat další odvozené sloupce, najeďte myší na existující odvozený sloupec a klikněte na ikonu se symbolem plus. Vyberte možnost **Přidat sloupec** nebo **Přidat vzor sloupce**. Vzorce sloupce mohou být užitečné, pokud jsou názvy sloupců z vašich zdrojů proměnné. Další informace najdete v tématu [vzory sloupců](concepts-data-flow-column-pattern.md).

![Nový odvozený výběr sloupce](media/data-flow/columnpattern.png "Nový odvozený výběr sloupce")

## <a name="build-schemas-in-output-schema-pane"></a>Schémata sestavení v podokně výstupní schéma

Sloupce, které upravujete a přidáváte do schématu, jsou uvedeny v podokně výstupní schéma. Tady můžete interaktivně vytvářet jednoduché a komplexní datové struktury. Chcete-li přidat další pole, vyberte možnost **Přidat sloupec**. Chcete-li vytvořit hierarchie, vyberte možnost **Přidat dílčí sloupec**.

![Přidat Podsloupec](media/data-flow/addsubcolumn.png "Přidat Podsloupec")

Další informace o zpracování složitých typů v toku dat naleznete v tématu [zpracování JSON při mapování toku dat](format-json.md#mapping-data-flow-properties).

![Přidat složitý sloupec](media/data-flow/complexcolumn.png "Přidání sloupců")

## <a name="data-flow-script"></a>Skript toku dat

### <a name="syntax"></a>Syntax

```
<incomingStream>
    derive(
           <columnName1> = <expression1>,
           <columnName2> = <expression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <deriveTransformationName>
```

### <a name="example"></a>Příklad

Níže uvedený příklad je odvozený sloupec s názvem `CleanData` , který přijímá příchozí datový proud `MoviesYear` a vytvoří dva odvozené sloupce. První odvozený sloupec nahrazuje sloupec `Rating` hodnotou hodnocení jako typ Integer. Druhý odvozený sloupec je vzor, který se shoduje se všemi sloupci, jejichž název začíná řetězcem "filmy". Pro každý odpovídající sloupec vytvoří sloupec `movie` , který je roven hodnotě odpovídajícího sloupce s předponou ' movie_ '. 

V uživatelském prostředí Data Factory Tato transformace vypadá jako na následujícím obrázku:

![Odvodit příklad](media/data-flow/derive-script1.png "Odvodit příklad")

Skript toku dat pro tuto transformaci je v následujícím fragmentu kódu:

```
MoviesYear derive(
                Rating = toInteger(Rating),
                each(
                    match(startsWith(name,'movies')),
                    'movie' = 'movie_' + toString($$)
                )
            ) ~> CleanData
```

## <a name="next-steps"></a>Další kroky

- Přečtěte si další informace o [jazyce výrazu Mapping data Flow](data-flow-expression-functions.md).
