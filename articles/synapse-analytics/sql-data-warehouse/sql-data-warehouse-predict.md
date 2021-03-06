---
title: Skóre modelů strojového učení s předpovídat
description: Naučte se pomocí funkce PREDIKTIVNÍho jazyka T-SQL v synapse SQL určit skóre modelů strojového učení.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/21/2020
ms.author: anjangsh
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d28f37e6020e3a3165b012548868a3ec798651c2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294150"
---
# <a name="score-machine-learning-models-with-predict"></a>Skóre modelů strojového učení s předpovídat

Synapse SQL nabízí možnost skóre modelů strojového učení pomocí známého jazyka T-SQL. Pomocí jazyka T- [SQL můžete](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)přenést stávající modely strojového učení s historickými daty a určit jejich skóre v rámci zabezpečených hranic datového skladu. Funkce PREDIKTIVNÍho přebírá model [ONNX (Open neuronové Network Exchange)](https://onnx.ai/) a data jako vstupy. Tato funkce eliminuje krok přesunu cenných dat mimo datový sklad pro účely bodování. Cílem je umožnit odborníkům v oblasti dat snadno nasadit modely strojového učení pomocí známého rozhraní T-SQL a zároveň bez problémů spolupracovat s odborníky přes data, kteří pracují se správnou architekturou pro jejich úkoly.

> [!NOTE]
> Tato funkce se v tuto chvíli v SQL na vyžádání nepodporuje.

Tato funkce vyžaduje, aby byl model vyškolený mimo synapse SQL. Jakmile model sestavíte, načtěte ho do datového skladu a Zastavte ho pomocí syntaxe pro předpověď T-SQL, abyste získali přehledy z dat.

![predictoverview](./media/sql-data-warehouse-predict/datawarehouse-overview.png)

## <a name="training-the-model"></a>Školení modelu

Synapse SQL očekává předem vyškolený model. Při výuce modelu strojového učení, který se používá k provádění předpovědi v synapse SQL, mějte na paměti následující faktory.

- Synapse SQL podporuje pouze modely formátu ONNX. ONNX je open source formát modelu, který umožňuje výměnu modelů mezi různými architekturami, aby bylo možné vzájemnou funkční spolupráci. Stávající modely můžete převést do formátu ONNX pomocí platforem, které ji buď nativně podporují, nebo převádění dostupných balíčků. Například balíček [skriptu sklearn-Onnx](https://github.com/onnx/sklearn-onnx) převádí modely scikit-učení na Onnx. [Úložiště GitHub ONNX](https://github.com/onnx/tutorials#converting-to-onnx-format) nabízí seznam podporovaných rozhraní a příkladů.

   Pokud používáte [automatizované ml](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) pro školení, nezapomeňte nastavit parametr *enable_onnx_compatible_models* na hodnotu true, aby se vytvořil model formátu Onnx. [Automatizovaný Machine Learning Poznámkový blok](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) ukazuje příklad použití AutoML k vytvoření modelu strojového učení ve formátu ONNX.

- Pro vstupní data jsou podporovány následující datové typy:
    - int, bigint, Real, float
    - char, varchar, nvarchar

- Data bodování musí být ve stejném formátu jako školicí data. PŘEDPOVĚDI nepodporují komplexní datové typy, jako například multidimenzionální pole. Takže pro účely školení se ujistěte, že každý vstup modelu odpovídá jednomu sloupci tabulky bodování místo předání jednoho pole obsahujícího všechny vstupy.

- Ujistěte se, že názvy a datové typy vzorových vstupů odpovídají názvům sloupců a datovým typům nových dat předpovědi. Vizualizace modelu ONNX pomocí různých open-source nástrojů dostupných online může další pomoc s laděním.

## <a name="loading-the-model"></a>Načítání modelu

Model je uložený v uživatelské tabulce synapse SQL jako šestnáctkový řetězec. Další sloupce, například ID a popis, mohou být přidány v tabulce modelů pro identifikaci modelu. Jako datový typ sloupce modelu použijte varbinary (max). Zde je příklad kódu pro tabulku, která se dá použít pro ukládání modelů:

```sql
-- Sample table schema for storing a model and related data
CREATE TABLE [dbo].[Models]
(
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Model] [varbinary](max) NULL,
    [Description] [varchar](200) NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    HEAP
)
GO

```

Jakmile je model převeden na šestnáctkový řetězec a na zadanou definici tabulky, použijte k načtení modelu v tabulce SQL synapse [příkaz Kopírovat](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) nebo základ. Následující ukázka kódu používá k načtení modelu příkaz Copy.

```sql
-- Copy command to load hexadecimal string of the model from Azure Data Lake storage location
COPY INTO [Models] (Model)
FROM '<enter your storage location>'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<enter your storage key here>')
)
```

## <a name="scoring-the-model"></a>Bodování modelu

Jakmile se model a data načtou do datového skladu, použijte k vyhodnocení modelu funkci **předpověď T-SQL** . Ujistěte se, že nová vstupní data jsou ve stejném formátu jako školicí data použitá k vytvoření modelu. Předpověď T-SQL přebírá dva vstupy: model a nové vstupní data bodování a generuje nové sloupce pro výstup. Model lze zadat jako proměnnou, literál nebo skalární sub_query. Použijte [s common_table_expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver15) k určení pojmenované sady výsledků pro datový parametr.

Následující příklad ukazuje vzorový dotaz pomocí funkce předpovědi. Vytvoří se další sloupec s názvem *skóre* a datovým typem *float* , který obsahuje výsledky předpovědi. Všechny sloupce vstupních dat a výstupní prediktivní sloupce jsou k dispozici pro zobrazení pomocí příkazu SELECT. Další podrobnosti najdete v tématu [předpověď (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).

```sql
-- Query for ML predictions
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT Model FROM Models WHERE Id = 1),
DATA = dbo.mytable AS d) WITH (Score float) AS p;
```

## <a name="next-steps"></a>Další kroky

Další informace o funkci předpověď naleznete v tématu [předpověď (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).
