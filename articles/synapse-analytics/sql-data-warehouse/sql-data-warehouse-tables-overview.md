---
title: Navrhování tabulek
description: Seznámení s návrhem tabulek v synapse fondu SQL
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/15/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: f4cf9e2d02030021d3092629731fcd8b77566907
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213937"
---
# <a name="design-tables-in-synapse-sql-pool"></a>Návrh tabulek v synapse fondu SQL

Tento článek poskytuje klíčové úvodní koncepty pro navrhování tabulek ve fondu SQL.

## <a name="determine-table-category"></a>Určení kategorie tabulky

[Schéma hvězdičky](https://en.wikipedia.org/wiki/Star_schema) uspořádá data do tabulek faktů a dimenzí. Některé tabulky se používají pro integraci nebo přípravu dat předtím, než se přesunou do tabulky faktů nebo dimenzí. Při návrhu tabulky se rozhodněte, jestli data tabulky patří do tabulky faktů, dimenzí nebo integrace. Toto rozhodnutí informuje příslušnou strukturu a distribuci tabulek.

- **Tabulky faktů** obsahují kvantitativní data, která se běžně generují v transakčním systému, a pak se načtou do fondu SQL. Maloobchodní podnikání například vygeneruje transakce prodeje každý den a potom načte data do tabulky faktů ve fondu SQL pro účely analýzy.

- **Tabulky dimenzí** obsahují data atributů, která se mohou měnit, ale obvykle se mění často. Například jméno a adresa zákazníka jsou uloženy v tabulce dimenzí a aktualizovány pouze v případě, že se změní profil zákazníka. Aby se minimalizovala velikost velké tabulky faktů, musí být jméno a adresa zákazníka v každém řádku tabulky faktů. Místo toho může tabulka faktů a tabulka dimenzí sdílet ID zákazníka. Dotaz se může spojit s těmito dvěma tabulkami a přidružit k němu profil a transakce zákazníka.

- **Integrační tabulky** poskytují místo pro integraci nebo přípravu dat. Tabulku pro integraci můžete vytvořit jako normální tabulku, externí tabulku nebo dočasnou tabulku. Můžete například načíst data do pracovní tabulky, provést transformaci dat v pracovním prostředí a pak tato data vložit do provozní tabulky.

## <a name="schema-and-table-names"></a>Názvy schémat a tabulek

Schémata jsou dobrým způsobem, jak seskupovat tabulky, a to způsobem, který se používá podobně.  Pokud migrujete více databází z řešení Prem do fondu SQL, funguje nejlépe migraci všech tabulek fakt, Dimension a Integration do jednoho schématu ve fondu SQL.

Můžete například uložit všechny tabulky v [WideWorldImportersDWm](/sql/sample/world-wide-importers/database-catalog-wwi-olap?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) fondu SQL Sample v jednom schématu s názvem WWI. Následující kód vytvoří [uživatelsky definované schéma](/sql/t-sql/statements/create-schema-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) s názvem WWI.

```sql
CREATE SCHEMA wwi;
```

Chcete-li zobrazit organizaci tabulek ve fondu SQL, můžete použít fakt, Dim a int jako předpony názvů tabulek. V následující tabulce jsou uvedeny některé názvy schémat a tabulek pro WideWorldImportersDW.  

| Tabulka WideWorldImportersDW  | Typ tabulky | Fond SQL |
|:-----|:-----|:------|:-----|
| Město | Dimenze | WWI. DimCity |
| Objednání | Fact | WWI. FactOrder |

## <a name="table-persistence"></a>Trvalost tabulek

Tabulky ukládají data buď trvale v Azure Storage, dočasně v Azure Storage, nebo v úložišti dat mimo fond SQL.

### <a name="regular-table"></a>Běžná tabulka

Pravidelná tabulka ukládá data v Azure Storage jako součást fondu SQL. Tabulka a data jsou trvalá bez ohledu na to, jestli je relace otevřená.  Následující příklad vytvoří normální tabulku se dvěma sloupci.

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Dočasná tabulka

Dočasná tabulka existuje pouze po dobu trvání relace. Můžete použít dočasnou tabulku a zabránit tak ostatním uživatelům v prohlížení dočasných výsledků a také k omezení nutnosti vyčištění.  

Dočasné tabulky využívají místní úložiště, které nabízí rychlý výkon.  Další informace najdete v tématu [dočasné tabulky](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Externí tabulka

Externí tabulka odkazuje na data umístěná v Azure Storagem objektu BLOB nebo Azure Data Lake Store. Při použití ve spojení s příkazem CREATE TABLE jako SELECT, výběr z externí tabulky importuje data do fondu SQL.

V takovém případě jsou externí tabulky užitečné pro načítání dat. Kurz načítání najdete v tématu [použití základny k načítání dat z úložiště objektů BLOB v Azure](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="data-types"></a>Typy dat

Fond SQL podporuje nejběžněji používané datové typy. Seznam podporovaných datových typů najdete v tématu [datové typy v CREATE TABLE odkaz](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#DataTypes) v příkazu CREATE TABLE. Pokyny k používání datových typů najdete v tématu [datové typy](sql-data-warehouse-tables-data-types.md).

## <a name="distributed-tables"></a>Distribuované tabulky

Základní funkcí fondu SQL je způsob, jakým může ukládat a fungovat v tabulkách napříč [distribucí](massively-parallel-processing-mpp-architecture.md#distributions).  Fond SQL podporuje tři metody pro distribuci dat: kruhové dotazování (výchozí), algoritmus hash a replikovaný.

### <a name="hash-distributed-tables"></a>Distribuované zatřiďovací tabulky (distribuce hodnot hash)

Distribuovaná tabulka hash distribuuje řádky na základě hodnoty v distribučním sloupci. Distribuovaná tabulka s algoritmem hash je navržená tak, aby dosáhla vysokého výkonu dotazů na rozsáhlých tabulkách. Při volbě distribučního sloupce je třeba zvážit několik faktorů.

Další informace najdete v tématu [pokyny k návrhu distribuovaných tabulek](sql-data-warehouse-tables-distribute.md).

### <a name="replicated-tables"></a>Replikované tabulky

Replikovaná tabulka má úplnou kopii tabulky, která je k dispozici na každém výpočetním uzlu. Dotazy se v replikovaných tabulkách spouštějí rychleji, protože spojení v replikovaných tabulkách nevyžadují přesun dat. Replikace vyžaduje další úložiště, ale a není pro velké tabulky praktické.

Další informace najdete v tématu [pokyny k návrhu replikovaných tabulek](design-guidance-for-replicated-tables.md).

### <a name="round-robin-tables"></a>Tabulky kruhového dotazování

Tabulka kruhového dotazování rozděluje řádky tabulky rovnoměrně napříč všemi distribucí. Řádky jsou náhodně distribuovány. Načítání dat do tabulky kruhového dotazování je rychlé.  Pamatujte, že dotazy mohou vyžadovat větší přesun dat než jiné metody distribuce.

Další informace najdete v tématu [pokyny k návrhu distribuovaných tabulek](sql-data-warehouse-tables-distribute.md).

### <a name="common-distribution-methods-for-tables"></a>Běžné metody distribuce pro tabulky

Kategorie tabulka často určuje, která možnost se má zvolit pro distribuci tabulky.

| Kategorie tabulky | Možnost Doporučené distribuce |
|:---------------|:--------------------|
| Fact           | Použijte distribuci algoritmem hash s clusterovaným indexem columnstore. Zvýšení výkonu se zvyšuje, když se do stejného distribučního sloupce spojí dvě zatřiďovací tabulky. |
| Dimenze      | Použijte replikovaný pro menší tabulky. Pokud jsou tabulky příliš velké pro uložení na každém výpočetním uzlu, použijte distribuované pomocí algoritmu hash. |
| Příprava        | Pro pracovní tabulku použijte kruhové dotazování. Zatížení pomocí CTAS je rychlé. Jakmile jsou data v pracovní tabulce, použijte příkaz INSERT... Tuto možnost vyberte, pokud chcete přesunout data do provozních tabulek. |

## <a name="table-partitions"></a>Oddíly tabulky

Dělená tabulka ukládá a provádí operace na řádcích tabulky podle rozsahů dat. Například tabulka může být rozdělená podle dne, měsíce nebo roku. Můžete zlepšit výkon dotazů prostřednictvím eliminace oddílu, což omezuje vyhledávání dotazů na data v rámci oddílu. Data můžete také udržovat pomocí přepínání oddílů. Vzhledem k tomu, že jsou data v SQL Data Warehouse již distribuována, může být příliš mnoho oddílů pomalý výkon dotazů. Další informace najdete v tématu [pokyny k dělení](sql-data-warehouse-tables-partition.md).  Při přepínání oddílu do oddílů tabulky, které nejsou prázdné, zvažte použití možnosti TRUNCATE_TARGET v příkazu [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) , pokud se mají zkrátit existující data. Níže jsou uvedené přepínače kódu v podobě transformovaných denních dat do SalesFact, které Přepisuje všechna existující data.

```sql
ALTER TABLE SalesFact_DailyFinalLoad SWITCH PARTITION 256 TO SalesFact PARTITION 256 WITH (TRUNCATE_TARGET = ON);  
```

## <a name="columnstore-indexes"></a>Indexy Columnstore

Ve výchozím nastavení fond SQL ukládá tabulku jako clusterovaný index columnstore. Tato forma úložiště dat dosahuje vysoké komprese dat a výkonu dotazů ve velkých tabulkách.  

Clusterovaný index columnstore je obvykle nejlepší volbou, ale v některých případech je clusterový index nebo halda odpovídající strukturou úložiště.  

> [!TIP]
> Tabulka haldy může být obzvláště užitečná pro načítání přechodných dat, jako je například pracovní tabulka, která se transformuje na konečnou tabulku.

Seznam funkcí columnstore najdete v tématu [co je nového pro indexy columnstore](/sql/relational-databases/indexes/columnstore-indexes-what-s-new?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest). Pokud chcete zlepšit výkon indexu columnstore, přečtěte si téma [maximalizace kvality skupiny řádků pro indexy columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

## <a name="statistics"></a>Statistika

Optimalizátor dotazů používá při vytváření dotazu statistiku na úrovni sloupce.

Pro zlepšení výkonu dotazů je důležité mít statistiku pro jednotlivé sloupce, zejména sloupce používané v dotazech na dotazy. [Vytváření statistik](sql-data-warehouse-tables-statistics.md#automatic-creation-of-statistic) proběhne automaticky.  

Aktualizace statistiky se neprovádí automaticky. Aktualizuje statistiku po přidání nebo změně významného počtu řádků. Například aktualizujte statistiku po načtení. Další informace najdete v tématu věnovaném [statistickým pokynům](sql-data-warehouse-tables-statistics.md).

## <a name="primary-key-and-unique-key"></a>Primární klíč a jedinečný klíč

PRIMÁRNÍ klíč se podporuje jenom v případě, že se používají jenom neclusterované a nevynucované.  JEDINEČNÉ omezení se podporuje jenom s nevynucovaném využitím.  Ověřte [omezení tabulky fondu SQL](sql-data-warehouse-table-constraints.md).

## <a name="commands-for-creating-tables"></a>Příkazy pro vytváření tabulek

Tabulku můžete vytvořit jako novou prázdnou tabulku. Můžete také vytvořit a naplnit tabulku pomocí výsledků příkazu SELECT. Níže jsou uvedené příkazy T-SQL pro vytvoření tabulky.

| Příkaz T-SQL | Description |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) | Vytvoří prázdnou tabulku definováním všech sloupců a možností tabulky. |
| [VYTVOŘIT EXTERNÍ TABULKU](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) | Vytvoří externí tabulku. Definice tabulky je uložena ve fondu SQL. Data tabulky se ukládají do služby Azure Blob Storage nebo Azure Data Lake Store. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) | Naplní novou tabulku výsledky příkazu SELECT. Sloupce tabulky a datové typy jsou založené na výsledcích příkazu SELECT. K importu dat tento příkaz může vybrat z externí tabulky. |
| [VYTVOŘIT EXTERNÍ TABULKU JAKO SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) | Vytvoří novou externí tabulku exportováním výsledků příkazu SELECT do externího umístění.  Umístění je buď úložiště objektů BLOB v Azure, nebo Azure Data Lake Store. |

## <a name="aligning-source-data-with-the-sql-pool"></a>Zarovnávání zdrojových dat s fondem SQL

Tabulky fondu SQL jsou vyplněny načtením dat z jiného zdroje dat. Aby bylo možné provést úspěšné načtení, musí být počet a datové typy sloupců ve zdrojových datech v souladu s definicí tabulky ve fondu SQL. Získání dat k zarovnání může být nejzávažnější součástí návrhu tabulek.

Pokud data pocházejí z více úložišť dat, načtoute data do fondu SQL a uložíte je do integrační tabulky. Jakmile jsou data v tabulce integrace, můžete k provádění operací transformace použít sílu fondu SQL. Jakmile budou data připravena, můžete je vložit do provozních tabulek.

## <a name="unsupported-table-features"></a>Nepodporované funkce tabulky

Fond SQL podporuje mnoho, ale ne všechny, z funkcí tabulky nabízených jinými databázemi.  V následujícím seznamu jsou uvedeny některé funkce tabulky, které nejsou podporovány ve fondu SQL:

- Cizí klíč, [omezení CHECK Table](/sql/t-sql/statements/alter-table-table-constraint-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Vypočítané sloupce](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Indexovaná zobrazení](/sql/relational-databases/views/create-indexed-views?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Sequence](/sql/t-sql/statements/create-sequence-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Zhuštěné sloupce](/sql/relational-databases/tables/use-sparse-columns?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- Náhradní klíče. Implementovat s [identitou](sql-data-warehouse-tables-identity.md)
- [Synonyma](/sql/t-sql/statements/create-synonym-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Triggery](/sql/t-sql/statements/create-trigger-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Jedinečné indexy](/sql/t-sql/statements/create-index-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [Uživatelsky definované typy](/sql/relational-databases/native-client/features/using-user-defined-types?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="table-size-queries"></a>Dotazy na velikost tabulky

Jedním z jednoduchých způsobů, jak identifikovat prostor a řádky spotřebované v tabulce v každé z distribucí 60, je použití [příkazu DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Použití příkazů DBCC však může být poměrně omezené.  Zobrazení dynamické správy (zobrazení dynamické správy) zobrazují více podrobností než příkazy DBCC. Začněte tím, že vytvoříte toto zobrazení:

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count]
    + nps.[row_overflow_used_page_count]
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count]
 - (nps.[reserved_page_count] - nps.[used_page_count])
 - ([in_row_data_page_count]
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
WHERE pn.[type] = 'COMPUTE'
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT *
FROM size
;
```

### <a name="table-space-summary"></a>Souhrn prostoru tabulky

Tento dotaz vrátí řádky a prostor podle tabulky.  Umožňuje vám zjistit, které tabulky jsou vašimi největším tabulkou, a jestli jsou distribuované, replikované nebo mezivržené pomocí algoritmu hash.  V případě tabulek distribuovaných algoritmem hash dotaz zobrazí sloupec distribuce.  

```sql
SELECT
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM
    dbo.vTableSizes
GROUP BY
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Prostor tabulky podle typu distribuce

```sql
SELECT
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabulkový prostor podle typu indexu

```sql
SELECT
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Souhrn distribučního prostoru

```sql
SELECT
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Další kroky

Po vytvoření tabulek pro váš fond SQL je dalším krokem načtení dat do tabulky.  Kurz načítání najdete v tématu [načtení dat do fondu SQL](load-data-wideworldimportersdw.md).
