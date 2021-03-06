---
title: Začínáme se službou tahání data flow v Azure Data Factory
description: Kurz o přípravě dat v Azure Data Factory pomocí toku dat tahání
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: f9b5380fa219d768651703eeb9fe445fcd215332
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921763"
---
# <a name="prepare-data-with-wrangling-data-flow"></a>Příprava dat pomocí toku dat tahání

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

> [!NOTE]
> Tok dat tahání je aktuálně dostupných ve verzi Public Preview.

## <a name="create-a-wrangling-data-flow"></a>Vytvoření toku dat tahání

Existují dva způsoby, jak vytvořit tok dat tahání v Azure Data Factory. Jedním ze způsobů, jak kliknout na ikonu Plus a vybrat **tok dat** v podokně prostředky továrny.

![Tahání](media/wrangling-data-flow/tutorial7.png)

Druhá metoda je v podokně aktivity plátna kanálu. Otevřete možnost **přesunutí a transformace** a přetáhněte aktivitu **toku dat** na plátno.

V obou metodách se v zobrazeném podokně vyberte **vytvořit nový tok dat** a zvolte **tahání tok dat**. Klikněte na tlačítko OK.

![Tahání](media/wrangling-data-flow/tutorial1.png)

## <a name="author-a-wrangling-data-flow"></a>Vytváření tahání toku dat

Přidejte **zdrojovou datovou sadu** pro tok dat tahání. Můžete buď zvolit existující datovou sadu, nebo vytvořit novou. Můžete také vybrat datovou sadu jímky. Můžete zvolit jednu nebo více zdrojových datových sad, ale v tuto chvíli je povolena pouze jedna jímka. Výběr datové sady jímky je nepovinný, ale vyžaduje se aspoň jedna zdrojová datová sada.

> [!NOTE]
> U omezené verze Preview se podporuje jenom text s oddělovači ADLS Gen 2. 

![Tahání](media/wrangling-data-flow/tutorial4.png)

Kliknutím na **vytvořit** otevřete Power Query Editor hybridních webových aplikací v online režimu.

![Tahání](media/wrangling-data-flow/tutorial5.png)

Vytvářejte tahání data Flow pomocí přípravy dat bez kódu. Seznam dostupných funkcí najdete v tématu [transformační funkce](wrangling-data-flow-functions.md) ./

![Tahání](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-wrangling-data-flow"></a>Spuštění a monitorování toku dat tahání

Pokud chcete spustit ladění kanálu pro tahání tok dat, klikněte na **ladit** na plátně kanálu. Po publikování toku dat **teď aktivační událost** spustí spuštění posledního publikovaného kanálu na vyžádání. Toky dat tahání se dají naplánovat se všemi existujícími triggery Azure Data Factory.

![Tahání](media/wrangling-data-flow/tutorial3.png)

Přechod na kartu **monitorování** a vizualizujte výstup spuštění aktivity toku dat aktivovaného tahání.

![Tahání](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Další kroky

Naučte se [vytvořit tok dat mapování](tutorial-data-flow.md).