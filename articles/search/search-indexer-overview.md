---
title: Indexery pro procházení dat během importu
titleSuffix: Azure Cognitive Search
description: Procházení Azure SQL Database, spravované instance SQL, Azure Cosmos DB nebo Azure Storage k extrakci prohledávatelných dat a naplnění indexu služby Azure Kognitivní hledání.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/12/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: d73782d9de7da2c5daacbff5397d9a365ff9ae03
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87038405"
---
# <a name="indexers-in-azure-cognitive-search"></a>Indexery ve službě Azure Cognitive Search

*Indexer* v Azure kognitivní hledání je prohledávací modul, který extrahuje hledaná data a metadata z externího zdroje dat Azure a naplní index založený na mapování polí mezi indexem a zdrojem dat. Tento přístup se někdy označuje jako "pull model", protože služba získává data v, aniž byste museli psát kód, který do indexu přidává data.

Indexery jsou založené na typech nebo platformách zdrojů dat a jednotlivé indexery pro SQL Server v Azure, Cosmos DB, Azure Table Storage a Blob Storage. Indexery úložiště objektů BLOB mají další vlastnosti specifické pro typy obsahu objektů BLOB.

Můžete použít indexer jako jediný prostředek přijímání dat, nebo můžete použít kombinaci postupů, kdy se indexer použije k načtení jenom některých polí v indexu.

Indexery můžete spouštět na vyžádání nebo podle plánu opakované aktualizace dat, který se spouští tak často, jak každých pět minut. Častější aktualizace vyžadují model nabízených oznámení, který současně aktualizuje data v Kognitivní hledání Azure i v externím zdroji dat.

## <a name="approaches-for-creating-and-managing-indexers"></a>Přístupy k vytváření a správě indexerů

Indexery můžete vytvářet a spravovat pomocí těchto přístupů:

* [Průvodce importem dat > portálu](search-import-data-portal.md)
* [Rozhraní API služby REST](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Nový indexer je zpočátku oznámen jako funkce ve verzi Preview. Funkce ve verzi Preview se zavádějí v rozhraních API (REST a .NET) a po přechodu do všeobecné dostupnosti se integrují do portálu. Pokud vyhodnocujete nový indexer, měli byste počítat s psaním kódu.

## <a name="permissions"></a>Oprávnění

Všechny operace související s indexery, včetně požadavků GET pro stav nebo definice, vyžadují [klíč rozhraní API pro správu](search-security-api-keys.md). 

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Podporované zdroje dat

Úložiště dat procházení indexerů v Azure

* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2](search-howto-index-azure-data-lake-storage.md) (ve verzi Preview)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure SQL Database a SQL Managed instance](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [SQL Server na Azure Virtual Machines](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)
* [Spravovaná instance SQL](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md)

## <a name="basic-configuration-steps"></a>Postup základní konfigurace
Indexery můžou nabízet funkce, které jsou jedinečné pro daný zdroj dat. Z toho důvodu se budou některé aspekty konfigurace indexeru nebo zdroje dat lišit podle typu indexeru. Všechny indexery ale sdílejí stejné základní složení a požadavky. Níže najdete popis kroků společných pro všechny indexery.

### <a name="step-1-create-a-data-source"></a>Krok 1: Vytvoření zdroje dat
Indexer získá připojení ke zdroji dat z objektu *zdroje dat* . Definice zdroje dat poskytuje připojovací řetězec a případně přihlašovací údaje. Chcete-li vytvořit prostředek, zavolejte třídu [Create datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source) REST API nebo [DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) .

Zdroje dat se konfigurují a spravují nezávisle na indexerech, které je používají, což znamená, že několik indexerů může používat zdroj dat k načtení více indexů současně.

### <a name="step-2-create-an-index"></a>Krok 2: Vytvoření indexu
Indexer automatizuje některé úkoly související s příjmem dat, ale vytváření indexu k nim obvykle nepatří. K základním požadavkům patří předdefinovaný index s poli, která odpovídají polím v externím zdroji dat. Pole musí odpovídat názvu a datovému typu. Další informace o strukturování indexu najdete v tématu [vytvoření indexu (Azure Kognitivní hledání REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) nebo [třídy indexu](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Nápovědu k přidružení polí najdete v tématu [mapování polí v indexerech Azure kognitivní hledání](search-indexer-field-mappings.md).

> [!Tip]
> Přestože indexery nedokážou vygenerovat index za vás, může vám pomoct průvodce **importem dat** na portálu. Ve většině případů dokáže průvodce odvodit schéma indexu ze stávajících metadat ve zdroji a zobrazit předběžné schéma indexu, které můžete upravit přímo v aktivním průvodci. Po vytvoření indexu ve službě jsou další úpravy na portálu omezené hlavně na přidávání nových polí. K vytvoření, ale ne revidování, indexu zvažte použití průvodce. Praktickou výuku najdete v [průvodci portálem](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Krok 3: Vytvoření a naplánování indexeru
Definice indexeru je konstrukce, která spojuje všechny prvky související s přijímáním dat. Mezi požadované prvky patří zdroj dat a index. Mezi volitelné prvky patří mapování plánu a polí. Mapování polí je nepovinné pouze v případě, že zdrojová pole a indexová pole jasně odpovídají. Další informace o strukturování indexeru najdete v tématu [Vytvoření indexeru (Azure Kognitivní hledání REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Spustit indexery na vyžádání

I když je běžné indexování běžné, lze indexer vyvolat také na vyžádání pomocí [příkazu Run](https://docs.microsoft.com/rest/api/searchservice/run-indexer):

```http
POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2020-06-30
api-key: [Search service admin key]
```

> [!NOTE]
> Když se rozhraní API spustí úspěšně, vyvolání indexeru se naplánuje, ale vlastní zpracování proběhne asynchronně. 

Stav indexeru můžete monitorovat na portálu nebo prostřednictvím rozhraní API pro získání stavu indexeru. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Získat stav indexeru

Můžete načíst stav a historii spouštění indexeru pomocí [příkazu Get indexer status](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):

```http
GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2020-06-30
api-key: [Search service admin key]
```

Odpověď obsahuje celkový stav indexeru, vyvolání posledního (nebo probíhajícího) indexeru a historii posledních vyvolání indexeru.

```output
{
    "status":"running",
    "lastResult": {
        "status":"success",
        "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
     },
    "executionHistory":[ {
        "status":"success",
         "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
    }]
}
```

Historie spouštění obsahuje až 50 posledních dokončených spuštění, která jsou seřazena v opačném chronologickém pořadí (takže poslední spuštění nastane jako první v odpovědi).

## <a name="next-steps"></a>Další kroky
Teď jste získali základní představu. V dalším kroku se zaměříme na požadavky a úlohy specifické pro různé typy zdrojů dat.

* [Azure SQL Database, spravovaná instance SQL nebo SQL Server na virtuálním počítači Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexování objektů BLOB CSV pomocí indexeru Azure Kognitivní hledání BLOB](search-howto-index-csv-blobs.md)
* [Indexování objektů BLOB JSON pomocí indexeru Azure Kognitivní hledání BLOB](search-howto-index-json-blobs.md)
