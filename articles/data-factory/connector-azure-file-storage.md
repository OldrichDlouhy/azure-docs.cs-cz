---
title: Kopírování dat z/do Azure File Storage
description: Naučte se, jak kopírovat data z Azure File Storage do podporovaných úložišť dat jímky (nebo) z podporovaných zdrojových úložišť dat do Azure File Storage pomocí Azure Data Factory.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/12/2020
ms.openlocfilehash: 43d5b787152c3c14ef11af513907fc5f41fe7cf0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84737749"
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Kopírování dat z nebo do Azure File Storage pomocí služby Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tento článek popisuje, jak kopírovat data do a z Azure File Storage. Pokud se chcete dozvědět o Azure Data Factory, přečtěte si [úvodní článek](introduction.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Tento konektor Azure File Storage se podporuje pro následující činnosti:

- [Aktivita kopírování](copy-activity-overview.md) s [podporovanou maticí zdroje/jímky](copy-activity-overview.md)
- [Aktivita vyhledávání](control-flow-lookup-activity.md)
- [Aktivita GetMetadata](control-flow-get-metadata-activity.md)
- [Odstranit aktivitu](delete-activity.md)

Konkrétně tento konektor služby Azure File Storage podporuje kopírování souborů tak, jak jsou, nebo pro analýzu a generování souborů s [podporovanými formáty souborů a kompresními kodeky](supported-file-formats-and-compression-codecs.md).

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Následující části obsahují podrobné informace o vlastnostech, které slouží k definování Data Factory entit specifických pro Azure File Storage.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro propojenou službu Azure File Storage se podporují tyto vlastnosti:

| Vlastnost | Popis | Vyžadováno |
|:--- |:--- |:--- |
| typ | Vlastnost Type musí být nastavená na: **AzureFileStorage**. | Yes |
| host | Určuje koncový bod Azure File Storage jako: <br/>-Using UI: zadejte`\\<storage name>.file.core.windows.net\<file service name>`<br/>-Používá se JSON: `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"` . | Yes |
| UserID | Zadejte uživatele pro přístup do Azure File Storage jako: <br/>-Using UI: zadejte`AZURE\<storage name>`<br/>-Používá se JSON: `"userid": "AZURE\\<storage name>"` . | Yes |
| heslo | Zadejte přístupový klíč k úložišti. Označte toto pole jako SecureString, abyste ho bezpečně ukládali do Data Factory nebo [odkazovali na tajný kód uložený v Azure Key Vault](store-credentials-in-key-vault.md). | Yes |
| connectVia | [Integration runtime](concepts-integration-runtime.md) , která se má použít pro připojení k úložišti dat Můžete použít Azure Integration Runtime nebo místní Integration Runtime (Pokud je úložiště dat umístěné v privátní síti). Pokud není zadaný, použije se výchozí Azure Integration Runtime. |Ne pro zdroj, Ano pro jímku |

>[!IMPORTANT]
> - Pokud chcete kopírovat data do Azure File Storage pomocí Azure Integration Runtime, explicitně [vytvořte Azure IR](create-azure-integration-runtime.md#create-azure-ir) s umístěním vašeho File Storage a přidružte ji v propojené službě jako následující příklad.
> - Pokud chcete kopírovat data z/do Azure File Storage pomocí samoobslužného Integration Runtime mimo Azure, nezapomeňte otevřít odchozí port TCP 445 ve vaší místní síti.

>[!TIP]
>Při použití uživatelského rozhraní ADF pro vytváření obsahu můžete pro vytváření propojených služeb najít konkrétní položku "Azure File Storage", která pod vygeneruje `FileServer` objekt typu.

**Příklad:**

```json
{
    "name": "AzureFileStorageLinkedService",
    "properties": {
        "type": "AzureFileStorage",
        "typeProperties": {
            "host": "\\\\<storage name>.file.core.windows.net\\<file service name>",
            "userid": "AZURE\\<storage name>",
            "password": {
                "type": "SecureString",
                "value": "<storage access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastností, které jsou k dispozici pro definování datových sad, naleznete v článku [datové sady](concepts-datasets-linked-services.md) . 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Pro Azure File Storage jsou podporovány následující vlastnosti v části `location` nastavení v datové sadě založené na formátu:

| Vlastnost   | Popis                                                  | Vyžadováno |
| ---------- | ------------------------------------------------------------ | -------- |
| typ       | Vlastnost Type v rámci `location` datové sady musí být nastavená na **FileServerLocation**. | Yes      |
| folderPath | Cesta ke složce Pokud chcete použít zástupný znak k filtrování složky, toto nastavení nechejte a zadejte v nastavení zdroje aktivity. | No       |
| fileName   | Název souboru pod daným folderPath. Pokud chcete použít zástupný znak k filtrování souborů, přeskočte toto nastavení a zadejte v nastavení zdroje aktivity. | No       |

**Příklad:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure File Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FileServerLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastností, které jsou k dispozici pro definování aktivit, najdete v článku [kanály](concepts-pipelines-activities.md) . V této části najdete seznam vlastností, které služba Azure File Storage source a jímka podporuje.

### <a name="azure-file-storage-as-source"></a>Azure File Storage jako zdroj

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Následující vlastnosti jsou podporovány pro Azure File Storage v části `storeSettings` nastavení ve zdroji kopírování založeném na formátu:

| Vlastnost                 | Popis                                                  | Vyžadováno                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| typ                     | Vlastnost Type v poli `storeSettings` musí být nastavená na **FileServerReadSettings**. | Yes                                           |
| ***Vyhledejte soubory ke zkopírování:*** |  |  |
| MOŽNOST 1: statická cesta<br> | Kopírovat ze zadané cesty ke složce nebo souboru v datové sadě. Pokud chcete zkopírovat všechny soubory ze složky, zadejte také `wildcardFileName` jako `*` . |  |
| MOŽNOST 2: zástupný znak<br>- wildcardFolderPath | Cesta ke složce se zástupnými znaky pro filtrování zdrojových složek. <br>Povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku); `^` Pokud vlastní název složky obsahuje zástupný znak nebo tento řídicí znak v rámci, použijte k Escape. <br>Další příklady najdete v [příkladech složky a filtru souborů](#folder-and-file-filter-examples). | No                                            |
| MOŽNOST 2: zástupný znak<br>- wildcardFileName | Název souboru se zástupnými znaky v rámci daného folderPath/wildcardFolderPath pro filtrování zdrojových souborů. <br>Povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku); `^` Pokud vlastní název složky obsahuje zástupný znak nebo tento řídicí znak v rámci, použijte k Escape.  Další příklady najdete v [příkladech složky a filtru souborů](#folder-and-file-filter-examples). | Yes |
| MOŽNOST 3: seznam souborů<br>- fileListPath | Určuje, že se má zkopírovat daná sada souborů. Najeďte na textový soubor, který obsahuje seznam souborů, které chcete zkopírovat, jeden soubor na řádek, který je relativní cestou k cestě nakonfigurované v datové sadě.<br/>Při použití této možnosti nezadávejte název souboru v datové sadě. Další příklady najdete v [příkladech seznamu souborů](#file-list-examples). |No |
| ***Další nastavení:*** |  | |
| zahrnout | Určuje, zda mají být data rekurzivně čtena z podsložek nebo pouze ze zadané složky. Všimněte si, že pokud je rekurzivní nastavení nastaveno na hodnotu true a jímka je úložiště založené na souborech, prázdná složka nebo podsložka není kopírována ani vytvořena v jímky. <br>Povolené hodnoty jsou **true** (výchozí) a **false**.<br>Tato vlastnost se při konfiguraci nepoužívá `fileListPath` . |No |
| deleteFilesAfterCompletion | Uvádí, zda budou binární soubory po úspěšném přesunutí do cílového úložiště odstraněny ze zdrojového úložiště. Odstranění souboru je vázané na soubor, takže když aktivita kopírování selže, uvidíte, že některé soubory se už zkopírovaly do cílového umístění a odstranily ze zdroje, zatímco ostatní jsou pořád ve zdrojovém úložišti. <br/>Tato vlastnost je platná jenom ve scénáři binárního kopírování, kde zdroje dat ukládají objekty blob, ADLS Gen1, ADLS Gen2, S3, Google Cloud Storage, File, Azure File, SFTP nebo FTP. Výchozí hodnota: false. |No |
| modifiedDatetimeStart    | Filtr souborů na základě atributu: Naposledy změněno <br>Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br> Vlastnosti mohou mít hodnotu NULL, což znamená, že pro datovou sadu nebude použit filtr atributů souborů.  Pokud `modifiedDatetimeStart` má hodnota DateTime `modifiedDatetimeEnd` , ale je null, znamená to, že budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Pokud `modifiedDatetimeEnd` má hodnota DateTime `modifiedDatetimeStart` , ale je null, znamená to, že jsou soubory, jejichž naposledy upravený atribut je menší než hodnota DateTime, bude vybrána.<br/>Tato vlastnost se při konfiguraci nepoužívá `fileListPath` . | No                                            |
| modifiedDatetimeEnd      | Stejné jako výše.                                               | No                                            |
| maxConcurrentConnections | Počet připojení, která se mají souběžně připojit k úložišti úložiště Určete pouze v případě, že chcete omezit souběžné připojení k úložišti dat. | No                                            |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "FileServerReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-file-storage-as-sink"></a>Azure File Storage jako jímka

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Pro Azure File Storage v části `storeSettings` nastavení v jímky kopírování na základě formátu jsou podporovány následující vlastnosti:

| Vlastnost                 | Popis                                                  | Vyžadováno |
| ------------------------ | ------------------------------------------------------------ | -------- |
| typ                     | Vlastnost Type v poli `storeSettings` musí být nastavená na **FileServerWriteSettings**. | Yes      |
| copyBehavior             | Definuje chování kopírování, pokud je zdrojem soubory z úložiště dat založeného na souborech.<br/><br/>Povolené hodnoty jsou následující:<br/><b>-PreserveHierarchy (výchozí)</b>: zachovává hierarchii souborů v cílové složce. Relativní cesta ke zdrojovému souboru se zdrojovou složkou je shodná s relativní cestou cílového souboru do cílové složky.<br/><b>-FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úrovni cílové složky. Cílové soubory mají automaticky generované názvy. <br/><b>-MergeFiles</b>: sloučí všechny soubory ze zdrojové složky do jednoho souboru. Je-li zadán název souboru, Název sloučeného souboru je zadaný název. V opačném případě se jedná o automaticky vygenerovaný název souboru. | No       |
| maxConcurrentConnections | Počet připojení, která mají být souběžně propojena s úložištěm dat. Určete pouze v případě, že chcete omezit souběžné připojení k úložišti dat. | No       |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "FileServerWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Příklady filtru složek a souborů

Tato část popisuje výsledné chování cesty ke složce a názvu souboru s filtry zástupných znaků.

| folderPath | fileName | zahrnout | Struktura zdrojové složky a výsledek filtru (jsou načteny soubory **tučně** )|
|:--- |:--- |:--- |:--- |
| `Folder*` | (prázdné, použít výchozí) | false (nepravda) | Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (prázdné, použít výchozí) | true | Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false (nepravda) | Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="file-list-examples"></a>Příklady seznamů souborů

Tato část popisuje výsledné chování při použití cesty seznamu souborů ve zdroji aktivity kopírování.

Za předpokladu, že máte následující strukturu zdrojové složky a chcete soubory zkopírovat tučně:

| Ukázka zdrojové struktury                                      | Obsah v FileListToCopy.txt                             | Konfigurace ADF                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| kořen<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mezipaměť<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | File1.csv<br>Subfolder1/File3.csv<br>Subfolder1/File5.csv | **V datové sadě:**<br>– Cesta ke složce:`root/FolderA`<br><br>**Ve zdroji aktivity kopírování:**<br>– Cesta k seznamu souborů:`root/Metadata/FileListToCopy.txt` <br><br>Cesta k seznamu souborů odkazuje na textový soubor ve stejném úložišti dat, který obsahuje seznam souborů, které chcete zkopírovat, jeden soubor na řádek s relativní cestou k cestě, která je nakonfigurovaná v datové sadě. |

### <a name="recursive-and-copybehavior-examples"></a>Příklady rekurzivních a copyBehavior

Tato část popisuje výsledné chování operace kopírování pro různé kombinace rekurzivních a copyBehavior hodnot.

| zahrnout | copyBehavior | Struktura zdrojové složky | Výsledný cíl |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílová složka Složku1 se vytvoří se stejnou strukturou jako zdroj:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílový Složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro soubor2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro file3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro file4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro File5 |
| true |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílový Složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Obsah soubor1 + soubor2 + file3 + file4 + soubor 5 se sloučí do jednoho souboru s automaticky generovaným názvem souboru. |
| false (nepravda) |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílová složka Složku1 se vytvoří s následující strukturou.<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 s file3, file4 a File5 se neúčtují. |
| false (nepravda) |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílová složka Složku1 se vytvoří s následující strukturou.<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky vygenerovaný název pro soubor2<br/><br/>Subfolder1 s file3, file4 a File5 se neúčtují. |
| false (nepravda) |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Soubor1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Cílová složka Složku1 se vytvoří s následující strukturou.<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Obsah soubor1 + soubor2 se sloučí do jednoho souboru s automaticky generovaným názvem souboru. automaticky vygenerovaný název pro Soubor1<br/><br/>Subfolder1 s file3, file4 a File5 se neúčtují. |

## <a name="lookup-activity-properties"></a>Vlastnosti aktivity vyhledávání

Chcete-li získat informace o vlastnostech, ověřte [aktivitu vyhledávání](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Vlastnosti aktivity GetMetadata

Pokud se chcete dozvědět víc o vlastnostech, podívejte se na [aktivitu GetMetadata](control-flow-get-metadata-activity.md) . 

## <a name="delete-activity-properties"></a>Odstranit vlastnosti aktivity

Další informace o vlastnostech najdete v části [Odstranění aktivity](delete-activity.md) .

## <a name="legacy-models"></a>Starší modely

>[!NOTE]
>Následující modely jsou stále podporovány, protože jsou z důvodu zpětné kompatibility. Navrhnete použití nového modelu uvedeného výše v předchozích částech a uživatelské rozhraní pro vytváření ADF bylo přepnuto na generování nového modelu.

### <a name="legacy-dataset-model"></a>Model zastaralé sady dat

| Vlastnost | Popis | Vyžadováno |
|:--- |:--- |:--- |
| typ | Vlastnost Type datové sady musí být nastavená na: **Shared** . |Yes |
| folderPath | Cesta ke složce <br/><br/>Filtr zástupných znaků je podporovaný, povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku); `^` Pokud vlastní název složky obsahuje zástupný znak nebo tento řídicí znak v rámci, použijte příkaz. <br/><br/>Příklady: RootFolder/podsložce/, další příklady najdete v [příkladech složky a filtru souborů](#folder-and-file-filter-examples). |Yes |
| fileName | **Název nebo zástupný filtr** pro soubory v rámci zadaného "FolderPath". Pokud nezadáte hodnotu pro tuto vlastnost, datová sada bude ukazovat na všechny soubory ve složce. <br/><br/>V případě filtru jsou povoleny zástupné znaky: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku).<br/>-Příklad 1:`"fileName": "*.csv"`<br/>-Příklad 2:`"fileName": "???20180427.txt"`<br/>Použijte `^` k ukončení, pokud skutečný název souboru obsahuje zástupný znak nebo tento řídicí znak v.<br/><br/>Když není zadaný název souboru pro výstupní datovou sadu a v jímky aktivity není zadaný **preserveHierarchy** , aktivita kopírování automaticky vygeneruje název souboru s následujícím vzorem: "*data. [ identifikátor GUID ID běhu aktivity]. [GUID if FlattenHierarchy]. [formát, pokud je nakonfigurován]. [komprese, je-li nakonfigurována]*", například" Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. gz "; Pokud kopírujete z tabulkového zdroje pomocí názvu tabulky místo dotazu, vzor názvu je "*[název tabulky]. [ formát]. [komprese, pokud je nakonfigurována]*", například" MyTable.csv ". |No |
| modifiedDatetimeStart | Filtr souborů na základě atributu: Naposledy změněno Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br/><br/> Počítejte s tím, že bude ovlivněn celkový výkon přesunu dat tím, že toto nastavení povolíte, pokud chcete provádět filtr souborů z obrovských objemů souborů. <br/><br/> Vlastnosti mohou mít hodnotu NULL, což znamená, že pro datovou sadu nebude použit filtr atributů souboru.  Pokud `modifiedDatetimeStart` má hodnota DateTime `modifiedDatetimeEnd` , ale je null, znamená to, že budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Pokud `modifiedDatetimeEnd` má hodnota DateTime `modifiedDatetimeStart` , ale je null, znamená to, že jsou soubory, jejichž naposledy upravený atribut je menší než hodnota DateTime, bude vybrána.| No |
| modifiedDatetimeEnd | Filtr souborů na základě atributu: Naposledy změněno Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br/><br/> Počítejte s tím, že bude ovlivněn celkový výkon přesunu dat tím, že toto nastavení povolíte, pokud chcete provádět filtr souborů z obrovských objemů souborů. <br/><br/> Vlastnosti mohou mít hodnotu NULL, což znamená, že pro datovou sadu nebude použit filtr atributů souboru.  Pokud `modifiedDatetimeStart` má hodnota DateTime `modifiedDatetimeEnd` , ale je null, znamená to, že budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Pokud `modifiedDatetimeEnd` má hodnota DateTime `modifiedDatetimeStart` , ale je null, znamená to, že jsou soubory, jejichž naposledy upravený atribut je menší než hodnota DateTime, bude vybrána.| No |
| formát | Pokud chcete **Kopírovat soubory** mezi úložišti na základě souborů (binární kopie), přeskočte oddíl formát v definicích vstupní i výstupní datové sady.<br/><br/>Pokud chcete analyzovat nebo generovat soubory s konkrétním formátem, podporují se tyto typy formátů souborů: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. V části formát nastavte vlastnost **typ** na jednu z těchto hodnot. Další informace najdete v částech [Formát textu](supported-file-formats-and-compression-codecs-legacy.md#text-format), [formát JSON](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Formát Avro](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [Formát ORC](supported-file-formats-and-compression-codecs-legacy.md#orc-format)a formátování [Parquet](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) . |Ne (jenom pro binární scénář kopírování) |
| komprese | Zadejte typ a úroveň komprese dat. Další informace najdete v tématu [podporované formáty souborů a kompresní kodeky](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Podporované typy jsou: **gzip**, **Deflate**, **bzip2**a **ZipDeflate**.<br/>Podporované úrovně: **optimální** a **nejrychlejší**. |No |

>[!TIP]
>Chcete-li zkopírovat všechny soubory ve složce, zadejte pouze **FolderPath** .<br>Chcete-li zkopírovat jeden soubor se zadaným názvem, zadejte **FolderPath** s částí **složky a názvem souboru s** názvem.<br>Chcete-li zkopírovat podmnožinu souborů ve složce, zadejte **FolderPath** s částí složky a **názvem souboru** s filtrem zástupných znaků.

>[!NOTE]
>Pokud jste pro filtr souborů používali vlastnost "FileFilter", je stále podporovaná tak, jak je, a když jste se rozhodli použít nové funkce filtru přidané do souboru "fileName".

**Příklad:**

```json
{
    "name": "AzureFileStorageDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<Azure File Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Starší zdrojový model aktivity kopírování

| Vlastnost | Popis | Vyžadováno |
|:--- |:--- |:--- |
| typ | Vlastnost Type zdroje aktivity kopírování musí být nastavená na: **FileSystemSource** . |Yes |
| zahrnout | Určuje, zda mají být data rekurzivně čtena z dílčích složek nebo pouze ze zadané složky. Poznámka: Pokud je rekurzivní nastavení nastaveno na hodnotu true a jímka je úložiště založené na souborech, prázdná složka/podsložka se nekopíruje/nevytvoří při jímky.<br/>Povolené hodnoty jsou: **true** (výchozí), **false** | No |
| maxConcurrentConnections | Počet připojení, která se mají souběžně připojit k úložišti úložiště Určete pouze v případě, že chcete omezit souběžné připojení k úložišti dat. | No |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure File Storage input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Starší model jímky aktivity kopírování

| Vlastnost | Popis | Vyžadováno |
|:--- |:--- |:--- |
| typ | Vlastnost Type jímky aktivity kopírování musí být nastavená na: **FileSystemSink** . |Yes |
| copyBehavior | Definuje chování kopírování, pokud je zdrojem soubory z úložiště dat založeného na souborech.<br/><br/>Povolené hodnoty jsou následující:<br/><b>-PreserveHierarchy (výchozí)</b>: zachovává hierarchii souborů v cílové složce. Relativní cesta ke zdrojovému souboru se zdrojovou složkou je shodná s relativní cestou cílového souboru do cílové složky.<br/><b>-FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úrovni cílové složky. Cílové soubory mají automaticky vygenerovaný název. <br/><b>-MergeFiles</b>: sloučí všechny soubory ze zdrojové složky do jednoho souboru. Je-li zadán název souboru, bude název sloučeného souboru uveden jako název. jinak by byl automaticky vygenerován název souboru. | No |
| maxConcurrentConnections | Počet připojení, která se mají souběžně připojit k úložišti úložiště Určete pouze v případě, že chcete omezit souběžné připojení k úložišti dat. | No |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure File Storage output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat podporovaných jako zdroje a jímky aktivity kopírování v Azure Data Factory najdete v části [podporovaná úložiště dat](copy-activity-overview.md#supported-data-stores-and-formats).
