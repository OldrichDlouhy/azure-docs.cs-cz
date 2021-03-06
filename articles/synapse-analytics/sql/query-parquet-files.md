---
title: Dotazování souborů Parquet pomocí SQL na vyžádání (Preview)
description: V tomto článku se naučíte, jak zadávat dotazy na soubory Parquet pomocí SQL na vyžádání (Preview).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 4bab1ef4588a705f0dd6cdb34be8272868f826e9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207562"
---
# <a name="query-parquet-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Dotazování souborů Parquet pomocí SQL na vyžádání (Preview) ve službě Azure synapse Analytics

V tomto článku se dozvíte, jak napsat dotaz pomocí SQL na vyžádání (Preview), který načte soubory Parquet.

## <a name="prerequisites"></a>Požadavky

Prvním krokem je **Vytvoření databáze** se zdrojem dat, který odkazuje na účet úložiště [NYC Yellow taxislužby](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) . Pak inicializujte objekty spuštěním [instalačního skriptu](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) v této databázi. Tento instalační skript vytvoří zdroje dat, přihlašovací údaje v oboru databáze a formáty externích souborů, které jsou použity v těchto ukázkách.

## <a name="dataset"></a>Datová sada

V této ukázce se používá [NYC Yellow taxislužby](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) DataSet. Soubory Parquet můžete dotazovat stejným způsobem jako [soubory CSV](query-parquet-files.md). Jediným rozdílem je, že `FILEFORMAT` parametr by měl být nastaven na hodnotu `PARQUET` . Příklady v tomto článku znázorňují konkrétní soubory pro čtení Parquet.

## <a name="query-set-of-parquet-files"></a>Dotaz sady souborů Parquet

Při dotazování souborů Parquet můžete zadat pouze sloupce s oznámením.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) WITH (
        tpepPickupDateTime DATETIME2,
        passengerCount INT
    ) AS nyc
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
```

## <a name="automatic-schema-inference"></a>Automatické odvození schématu

Při čtení souborů Parquet není nutné používat klauzuli OPENROWSET WITH. Názvy sloupců a datové typy se automaticky čtou ze souborů Parquet.

Následující ukázka ukazuje možnosti automatického odvození schématu pro soubory Parquet. Vrátí počet řádků v září 2017 bez zadání schématu.

> [!NOTE]
> Při čtení souborů Parquet není nutné zadávat sloupce v klauzuli OPENROWSET WITH. V takovém případě dotazovací služba SQL na vyžádání bude používat metadata v souboru Parquet a sváže sloupce podle názvu.

```sql
SELECT TOP 10 *
FROM  
    OPENROWSET(
        BULK 'puYear=2018/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) AS nyc
```

### <a name="query-partitioned-data"></a>Dotazování na dělená data

Datová sada uvedená v této ukázce je rozdělená (rozdělená do oddílů) na samostatné podsložky. Pomocí funkce FilePath můžete cílit na konkrétní oddíly. V tomto příkladu se zobrazuje množství tarifů podle roku, měsíce a payment_type v prvních třech měsících od 2017.

> [!NOTE]
> Dotaz SQL na vyžádání je kompatibilní se schématem dělení na oddíly/Hadoop.

```sql
SELECT
        YEAR(tpepPickupDateTime),
        passengerCount,
        COUNT(*) AS cnt
FROM  
    OPENROWSET(
        BULK 'puYear=*/puMonth=*/*.snappy.parquet',
        DATA_SOURCE = 'YellowTaxi',
        FORMAT='PARQUET'
    ) nyc
WHERE
    nyc.filepath(1) = 2017
    AND nyc.filepath(2) IN (1, 2, 3)
    AND tpepPickupDateTime BETWEEN CAST('1/1/2017' AS datetime) AND CAST('3/31/2017' AS datetime)
GROUP BY
    passengerCount,
    YEAR(tpepPickupDateTime)
ORDER BY
    YEAR(tpepPickupDateTime),
    passengerCount;
```

## <a name="type-mapping"></a>Mapování typů

Soubory Parquet obsahují popisy typů pro každý sloupec. Následující tabulka popisuje, jak jsou typy Parquet mapovány na nativní typy SQL.

| Typ Parquet | Logický typ Parquet (anotace) | Datový typ SQL |
| --- | --- | --- |
| DATOVÉHO | | bit |
| BINÁRNÍ/BYTE_ARRAY | | varbinary |
| KLEPAT | | float |
| Plovák | | real |
| UVEDENA | | int |
| INT64 | | bigint |
| INT96 | |datetime2 |
| FIXED_LEN_BYTE_ARRAY | |binární |
| TVARU |UTF |varchar \* (řazení UTF8) |
| TVARU |ŘETEZCE |varchar \* (řazení UTF8) |
| TVARU |VYTVÁŘENÍ|varchar \* (řazení UTF8) |
| TVARU |IDENTIFIKÁTOR |uniqueidentifier |
| TVARU |NOTACI |decimal |
| TVARU |JSON |varchar (max) \* (kolace UTF8) |
| TVARU |BSON |varbinary (max) |
| FIXED_LEN_BYTE_ARRAY |NOTACI |decimal |
| BYTE_ARRAY |DOBA |varchar (max), serializováno do standardizovaného formátu |
| UVEDENA |INT (8, true) |smallint |
| UVEDENA |INT (16, true) |smallint |
| UVEDENA |INT (32, true) |int |
| UVEDENA |INT (8, false) |tinyint |
| UVEDENA |INT (16, false) |int |
| UVEDENA |INT (32, false) |bigint |
| UVEDENA |DATE (Datum) |date |
| UVEDENA |NOTACI |decimal |
| UVEDENA |ČAS (LISOVNY)|time |
| INT64 |INT (64; true) |bigint |
| INT64 |INT (64, false) |desetinné číslo (20, 0) |
| INT64 |NOTACI |decimal |
| INT64 |ČAS (MIKROČASU A NANO) |time |
|INT64 |ČASOVÉ RAZÍTKO (LISOVNY//NANO) |datetime2 |
|[Komplexní typ](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#lists) |SEZNAMU |varchar (max), serializováno do formátu JSON |
|[Komplexní typ](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#maps)|MAPY|varchar (max), serializováno do formátu JSON |

## <a name="next-steps"></a>Další kroky

Přejděte k dalšímu článku a Naučte se, jak [zadávat dotazy na vnořené typy Parquet](query-parquet-nested-types.md).
