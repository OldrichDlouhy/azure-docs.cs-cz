---
title: CETAS v synapse SQL
description: Použití CETAS s synapse SQL
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: f3e53ac189e0d612b09c362e82ba5bc2fe5fec8d
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2020
ms.locfileid: "83696832"
---
# <a name="cetas-with-synapse-sql"></a>CETAS s synapse SQL

V buď ve fondu SQL nebo na vyžádání SQL (Preview), můžete použít možnost vytvořit externí tabulku jako SELECT (CETAS) a dokončit následující úlohy:  

- Vytvoření externí tabulky
- Export, paralelně, výsledky příkazu jazyka Transact-SQL SELECT do

  - Hadoop
  - Azure Storage Blob
  - Azure Data Lake Storage Gen2

## <a name="cetas-in-sql-pool"></a>CETAS ve fondu SQL

V případě fondu SQL, použití a syntaxe CETAS zaškrtněte políčko [vytvořit externí tabulku jako](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) článek. Další informace o pokynech k CTAS s využitím fondu SQL najdete v článku [CREATE TABLE AS Select](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) .

## <a name="cetas-in-sql-on-demand"></a>CETAS v SQL na vyžádání

Při použití prostředku SQL na vyžádání se CETAS používá k vytvoření externí tabulky a exportu výsledků dotazu pro Azure Storage Blob nebo Azure Data Lake Storage Gen2.

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Arguments

*[[ *database_name* . [ *schema_name* ]. ] | *schema_name* . ] *TABLE_NAME**

Název první ze tří částí tabulky, která se má vytvořit. V případě externí tabulky ukládá SQL na vyžádání pouze metadata tabulky. V SQL na vyžádání se nepřesunou ani neukládají žádná skutečná data.

LOCATION = *' path_to_folder '*

Určuje, kam se mají zapsat výsledky příkazu SELECT na externím zdroji dat. Kořenová složka je umístění dat zadané v externím zdroji dat. UMÍSTĚNÍ musí ukazovat na složku a mít koncovou/. Příklad: aggregated_data/

DATA_SOURCE = *external_data_source_name*

Určuje název objektu externího zdroje dat, který obsahuje umístění, kde budou externí data uložena. Chcete-li vytvořit externí zdroj dat, použijte příkaz [Create External data source (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source).

FILE_FORMAT = *external_file_format_name*

Určuje název objektu externího souboru formátu, který obsahuje formát pro externí datový soubor. Chcete-li vytvořit externí formát souboru, použijte příkaz [Create External File Format (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format). V současné době se podporují jenom formáty externích souborů s FORMAT = ' PARQUET '.

S *<common_table_expression>*

Určuje dočasnou pojmenovanou sadu výsledků, která se označuje jako výraz běžné tabulky (CTE). Další informace naleznete v tématu [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

Vyberte <select_criteria>

Naplní novou tabulku výsledky příkazu SELECT. *select_criteria* je tělo příkazu SELECT, který určuje, která data se mají zkopírovat do nové tabulky. Informace o příkazech SELECT naleznete v tématu [Select (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

> [!NOTE]
> Klauzule ORDER BY v příkazu SELECT part of CETAS není podporována.

## <a name="permissions"></a>Oprávnění

Aby CETAS fungovalo, musíte mít oprávnění k výpisu obsahu složky a zápisu do složky umístění.

## <a name="examples"></a>Příklady

Tyto příklady používají CETAS k uložení celkového počtu obyvatel agregovaného po rocích a State do složky aggregated_data, která je umístěná ve zdroji dat population_ds.

Tato ukázka spoléhá na dříve vytvořené přihlašovací údaje, zdroj dat a externí formát souboru. Přečtěte si dokument [externí tabulky](develop-tables-external-tables.md) . Chcete-li uložit výsledky dotazu do jiné složky ve stejném zdroji dat, změňte argument umístění. 

Pokud chcete výsledky Uložit do jiného účtu úložiště, vytvořte a použijte jiný zdroj dat pro DATA_SOURCE argument.

> [!NOTE]
> Níže uvedené příklady používají veřejný účet úložiště Azure Open data. Je jen pro čtení. Chcete-li spustit tyto dotazy, je nutné zadat zdroj dat, pro který máte oprávnění k zápisu.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

Následující příklad používá externí tabulku jako zdroj pro CETAS. Spoléhá se na přihlašovací údaje, zdroj dat, formát externích souborů a externí tabulku vytvořenou dříve. Přečtěte si dokument [externí tabulky](develop-tables-external-tables.md) .

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Podporované datové typy

CETAS lze použít k uložení sad výsledků s následujícími datovými typy SQL:

- binární
- varbinary
- char
- varchar
- date
- time
- datetime2
- decimal
- numerické
- float
- real
- bigint
- int
- smallint
- tinyint
- bit

> [!NOTE]
> Objekty LOBs s nelze použít s CETAS.

V rámci vybrané části CETAS nelze použít následující datové typy:

- nchar
- nvarchar
- datetime
- smalldatetime
- DateTimeOffset
- papír
- smallmoney
- uniqueidentifier

## <a name="next-steps"></a>Další kroky

Můžete zkusit dotazování [Apache Spark pro externí tabulky Azure synapse](develop-storage-files-spark-tables.md).
