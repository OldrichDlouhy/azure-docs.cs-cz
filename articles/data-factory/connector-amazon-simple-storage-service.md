---
title: Kopírovat data ze služby Amazon Simple Storage Service (S3)
description: Přečtěte si, jak kopírovat data ze služby Amazon Simple Storage Service (S3) do podporovaných úložišť dat jímky pomocí Azure Data Factory.
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
ms.openlocfilehash: 023d6734195dabefff12210c2e63a0a4f4f9ac93
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87007652"
---
# <a name="copy-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Kopírování dat ze služby Amazon Simple Storage pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Vyberte verzi Data Factory služby, kterou používáte:"]
>
> * [Verze 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Aktuální verze](connector-amazon-simple-storage-service.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tento článek popisuje, jak kopírovat data ze služby Amazon Simple Storage Service (Amazon S3). Pokud se chcete dozvědět o Azure Data Factory, přečtěte si [úvodní článek](introduction.md).

>[!TIP]
>Další informace o scénáři migrace dat ze služby Amazon S3 do Azure Storage najdete v tématu [použití Azure Data Factory k migraci dat ze služby Amazon S3 na Azure Storage](data-migration-guidance-s3-azure-storage.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Tento konektor Amazon S3 se podporuje pro následující činnosti:

- [Aktivita kopírování](copy-activity-overview.md) s [podporovanou maticí zdroje/jímky](copy-activity-overview.md)
- [Aktivita vyhledávání](control-flow-lookup-activity.md)
- [Aktivita GetMetadata](control-flow-get-metadata-activity.md)
- [Odstranit aktivitu](delete-activity.md)

Konkrétně tato konektor Amazon S3 podporuje kopírování souborů, jako je, nebo k analýze souborů s [podporovanými formáty souborů a kompresními kodeky](supported-file-formats-and-compression-codecs.md). [Při kopírování můžete také zachovat metadata souboru](#preserve-metadata-during-copy). Konektor používá [AWS Signature verze 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) k ověřování požadavků na S3.

>[!TIP]
>Tento konektor Amazon S3 můžete použít ke kopírování dat z *jakéhokoli poskytovatele úložiště kompatibilního se systémem S3*, jako je [Google Cloud Storage](connector-google-cloud-storage.md). Zadejte odpovídající adresu URL služby v konfiguraci propojené služby.

## <a name="required-permissions"></a>Požadovaná oprávnění

Pokud chcete kopírovat data ze služby Amazon S3, ujistěte se, že máte udělena následující oprávnění:

- **Pro provádění aktivit kopírování**: `s3:GetObject` a `s3:GetObjectVersion` pro operace s objekty Amazon S3.
- **Pro Data Factory vytváření grafického uživatelského rozhraní**: `s3:ListAllMyBuckets` a `s3:ListBucket` / `s3:GetBucketLocation` pro operace s kontejnerem Amazon S3. Pro operace, jako je testování připojení a procházení cest k souborům, se vyžadují i oprávnění. Pokud tato oprávnění nechcete udělit, přeskočte testovací připojení na stránce pro vytvoření propojené služby a zadejte cestu přímo v nastavení datové sady.

Úplný seznam oprávnění Amazon S3 najdete v tématu [určení oprávnění v zásadách](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) na webu AWS.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Následující části obsahují podrobné informace o vlastnostech, které se používají k definování Data Factory entit specifických pro službu Amazon S3.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro propojenou službu Amazon S3 jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Povinné |
|:--- |:--- |:--- |
| typ | Vlastnost **Type** musí být nastavená na **AmazonS3**. | Yes |
| accessKeyId | ID tajného přístupového klíče |Yes |
| secretAccessKey | Tajný přístupový klíč sám o sobě. Označte toto pole jako **SecureString** , abyste ho bezpečně ukládali do Data Factory nebo [odkazovali na tajný kód uložený v Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| serviceUrl | Pokud kopírujete data z jiného poskytovatele úložiště kompatibilního se systémem S3, než je oficiální služba Amazon S3, určete vlastní koncový bod S3. Pokud například chcete kopírovat data ze služby Google Cloud Storage, zadejte `https://storage.googleapis.com` . | No |
| connectVia | [Prostředí Integration runtime](concepts-integration-runtime.md) , které se má použít pro připojení k úložišti dat. Můžete použít prostředí Azure Integration runtime nebo místní prostředí Integration runtime (Pokud je vaše úložiště dat v privátní síti). Pokud tato vlastnost není zadaná, služba použije výchozí prostředí Azure Integration runtime. |No |

Tento konektor vyžaduje ke kopírování dat z Amazon S3 přístupové klíče pro účet AWS identity and Access Management (IAM). [Dočasné zabezpečovací přihlašovací údaje](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) se teď nepodporují.

>[!TIP]
>Pokud kopírujete data z úložiště kompatibilního se systémem S3, než je oficiální služba Amazon S3, zadejte adresu URL vlastní služby S3.

Tady je příklad:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
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

V části `location` nastavení v datové sadě založené na formátu jsou podporovány následující vlastnosti služby Amazon S3:

| Vlastnost   | Popis                                                  | Povinné |
| ---------- | ------------------------------------------------------------ | -------- |
| typ       | Vlastnost **Type** v rámci `location` datové sady musí být nastavená na **AmazonS3Location**. | Yes      |
| interval intervalu | Název intervalu S3.                                          | Yes      |
| folderPath | Cesta ke složce v daném kontejneru. Pokud chcete použít zástupný znak k filtrování složky, toto nastavení nechejte a zadejte v nastavení zdroje aktivity. | No       |
| fileName   | Název souboru v daném intervalu a cestě ke složce. Pokud chcete k filtrování souborů použít zástupný znak, toto nastavení nechte a určete v nastavení zdroje aktivity. | No       |
| verze | Verze objektu S3, pokud je povolená Správa verzí S3 Pokud není zadán, bude načtena nejnovější verze. |No |

**Případě**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
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

Úplný seznam oddílů a vlastností, které jsou k dispozici pro definování aktivit, najdete v článku [kanály](concepts-pipelines-activities.md) . V této části najdete seznam vlastností, které podporuje zdroj Amazon S3.

### <a name="amazon-s3-as-a-source-type"></a>Amazon S3 jako typ zdroje

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

V části `storeSettings` nastavení ve zdroji kopírování založené na formátu jsou podporovány následující vlastnosti služby Amazon S3:

| Vlastnost                 | Popis                                                  | Povinné                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| typ                     | Vlastnost **Type** v poli `storeSettings` musí být nastavená na **AmazonS3ReadSettings**. | Yes                                                         |
| ***Vyhledejte soubory ke zkopírování:*** |  |  |
| MOŽNOST 1: statická cesta<br> | Kopírovat z daného kontejneru nebo složky/cesty k souboru, které jsou zadány v datové sadě. Pokud chcete zkopírovat všechny soubory z intervalu nebo složky, zadejte také `wildcardFileName` jako `*` . |  |
| MOŽNOST 2: Předpona S3<br>-prefix | Předpona názvu klíče S3 v rámci daného intervalu nakonfigurovaného v datové sadě pro filtrování zdrojových souborů S3 Jsou vybrané klíče S3, jejichž názvy začínají na `bucket_in_dataset/this_prefix` . Využívá filtr na straně služby S3's, který poskytuje lepší výkon než filtr zástupných znaků. | No |
| MOŽNOST 3: zástupný znak<br>- wildcardFolderPath | Cesta ke složce se zástupnými znaky v daném kontejneru nakonfigurovaném pro filtrování zdrojových složek. <br>Povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku). Použijte `^` k ukončení, jestli má název složky zástupný znak nebo tento řídicí znak uvnitř. <br>Další příklady najdete v [příkladech složky a filtru souborů](#folder-and-file-filter-examples). | No                                            |
| MOŽNOST 3: zástupný znak<br>- wildcardFileName | Název souboru se zástupnými znaky v daném intervalu a cestě ke složce (nebo cesta ke složce se zástupnými znaky) pro filtrování zdrojových souborů. <br>Povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku). Použijte `^` k ukončení, jestli má název složky zástupný znak nebo tento řídicí znak uvnitř.  Další příklady najdete v [příkladech složky a filtru souborů](#folder-and-file-filter-examples). | Yes |
| MOŽNOST 4: seznam souborů<br>- fileListPath | Určuje, že se má zkopírovat daná sada souborů. Najeďte na textový soubor, který obsahuje seznam souborů, které chcete zkopírovat, jeden soubor na řádek, což je relativní cesta k cestě nakonfigurované v datové sadě.<br/>Při použití této možnosti nezadávejte název souboru v datové sadě. Další příklady najdete v [příkladech seznamu souborů](#file-list-examples). |No |
| ***Další nastavení:*** |  | |
| zahrnout | Určuje, zda mají být data rekurzivně čtena z podsložek nebo pouze ze zadané složky. Všimněte si, že pokud je **rekurzivní** nastavení nastaveno na **hodnotu true** a jímka je úložiště založené na souborech, prázdná složka nebo podsložka není kopírována ani vytvořena v jímky. <br>Povolené hodnoty jsou **true** (výchozí) a **false**.<br>Tato vlastnost se při konfiguraci nepoužívá `fileListPath` . |No |
| deleteFilesAfterCompletion | Uvádí, zda budou binární soubory po úspěšném přesunutí do cílového úložiště odstraněny ze zdrojového úložiště. Odstranění souboru je vázané na soubor, takže když aktivita kopírování selže, uvidíte, že některé soubory se už zkopírovaly do cílového umístění a odstranily ze zdroje, zatímco ostatní jsou pořád ve zdrojovém úložišti. <br/>Tato vlastnost je platná jenom ve scénáři binárního kopírování, kde zdroje dat ukládají objekty blob, ADLS Gen1, ADLS Gen2, S3, Google Cloud Storage, File, Azure File, SFTP nebo FTP. Výchozí hodnota: false. |No |
| modifiedDatetimeStart    | Soubory jsou filtrovány na základě atributu: Naposledy změněno. <br>Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br> Vlastnosti mohou mít **hodnotu null**, což znamená, že pro datovou sadu nebude použit filtr atributů souborů.  Když `modifiedDatetimeStart` má hodnotu DateTime, ale `modifiedDatetimeEnd` je **null**, budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Když `modifiedDatetimeEnd` má hodnotu DateTime, ale `modifiedDatetimeStart` je **null**, soubory, jejichž atribut Last Modified je menší než hodnota DateTime, se vybere.<br/>Tato vlastnost se při konfiguraci nepoužívá `fileListPath` . | No                                            |
| modifiedDatetimeEnd      | Stejné jako výše.                                               | No                                                          |
| maxConcurrentConnections | Počet souběžných připojení k úložišti dat. Určete pouze v případě, že chcete omezit souběžná připojení k úložišti dat. | No                                                          |

**Případě**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
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
                    "type": "AmazonS3ReadSettings",
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

### <a name="folder-and-file-filter-examples"></a>Příklady filtru složek a souborů

Tato část popisuje výsledné chování cesty ke složce a názvu souboru s filtry zástupných znaků.

| blocích | key | zahrnout | Struktura zdrojové složky a výsledek filtru (jsou načteny soubory tučně)|
|:--- |:--- |:--- |:--- |
| blocích | `Folder*/*` | false (nepravda) | blocích<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| blocích | `Folder*/*` | true | blocích<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.jsna**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| blocích | `Folder*/*.csv` | false (nepravda) | blocích<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| blocích | `Folder*/*.csv` | true | blocích<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="file-list-examples"></a>Příklady seznamů souborů

Tato část popisuje výsledné chování při použití cesty seznamu souborů ve zdroji aktivity kopírování.

Předpokládejme, že máte následující strukturu zdrojové složky a chcete soubory zkopírovat tučně:

| Ukázka zdrojové struktury                                      | Obsah v FileListToCopy.txt                             | Konfigurace Data Factory                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| blocích<br/>&nbsp;&nbsp;&nbsp;&nbsp;Složka<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.jsna<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mezipaměť<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | File1.csv<br>Subfolder1/File3.csv<br>Subfolder1/File5.csv | **V datové sadě:**<br>Blocích`bucket`<br>– Cesta ke složce:`FolderA`<br><br>**Ve zdroji aktivity kopírování:**<br>– Cesta k seznamu souborů:`bucket/Metadata/FileListToCopy.txt` <br><br>Cesta k seznamu souborů odkazuje na textový soubor ve stejném úložišti dat, který obsahuje seznam souborů, které chcete zkopírovat, jeden soubor na řádek s relativní cestou k cestě, která je nakonfigurovaná v datové sadě. |

## <a name="preserve-metadata-during-copy"></a>Zachovat metadata během kopírování

Když kopírujete soubory ze služby Amazon S3 do Azure Data Lake Storage Gen2 nebo úložiště objektů BLOB v Azure, můžete se rozhodnout zachovat metadata souboru spolu s daty. Další informace o [zachování metadat](copy-activity-preserve-metadata.md#preserve-metadata)

## <a name="lookup-activity-properties"></a>Vlastnosti aktivity vyhledávání

Chcete-li získat informace o vlastnostech, ověřte [aktivitu vyhledávání](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Vlastnosti aktivity GetMetadata

Chcete-li získat informace o vlastnostech, ověřte [aktivitu GetMetadata](control-flow-get-metadata-activity.md). 

## <a name="delete-activity-properties"></a>Odstranit vlastnosti aktivity

Chcete-li získat informace o vlastnostech, ověřte [aktivitu odstranit](delete-activity.md).

## <a name="legacy-models"></a>Starší modely

>[!NOTE]
>Následující modely jsou stále podporovány, protože jsou z důvodu zpětné kompatibility. Doporučujeme použít nový model uvedený výše. Uživatelské rozhraní pro vytváření Data Factory bylo přepnuto na generování nového modelu.

### <a name="legacy-dataset-model"></a>Model zastaralé sady dat

| Vlastnost | Popis | Povinné |
|:--- |:--- |:--- |
| typ | Vlastnost **Type** datové sady musí být nastavená na **AmazonS3Object**. |Yes |
| interval intervalu | Název intervalu S3. Filtr zástupných znaků se nepodporuje. |Ano pro aktivitu kopírování nebo vyhledávání, ne pro aktivitu GetMetadata |
| key | Název nebo zástupný filtr klíče objektu S3 v zadaném intervalu. Platí pouze v případě, že není zadána vlastnost **prefix** . <br/><br/>Filtr zástupných znaků je podporován pro část složky i pro název souboru. Povolené zástupné znaky jsou: `*` (odpovídá žádnému nebo více znakům) a `?` (odpovídá žádnému nebo jednomu znaku).<br/>-Příklad 1:`"key": "rootfolder/subfolder/*.csv"`<br/>-Příklad 2:`"key": "rootfolder/subfolder/???20180427.txt"`<br/>Další příklady najdete v příkladech [složky a filtru souborů](#folder-and-file-filter-examples). Použijte `^` k Escape, jestli má vaše skutečná složka nebo název souboru zástupný znak nebo tento řídicí znak uvnitř. |No |
| směr | Prefix pro klíč objektu S3 Jsou vybrány objekty, jejichž klíče začínají touto předponou. Platí pouze v případě, že není zadána vlastnost **klíče** . |No |
| verze | Verze objektu S3, pokud je povolená Správa verzí S3 Pokud není zadána verze, bude načtena nejnovější verze. |No |
| modifiedDatetimeStart | Soubory jsou filtrovány na základě atributu: Naposledy změněno. Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br/><br/> Mějte na paměti, že povolení tohoto nastavení bude mít vliv na celkový výkon přesunu dat, pokud chcete filtrovat velké objemy souborů. <br/><br/> Vlastnosti mohou mít **hodnotu null**, což znamená, že pro datovou sadu nebude použit filtr atributů souborů.  Když `modifiedDatetimeStart` má hodnotu DateTime, ale `modifiedDatetimeEnd` je **null**, budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Když `modifiedDatetimeEnd` má hodnotu DateTime, ale `modifiedDatetimeStart` je null, soubory, jejichž atribut Last Modified je menší než hodnota DateTime, se vybere.| No |
| modifiedDatetimeEnd | Soubory jsou filtrovány na základě atributu: Naposledy změněno. Soubory budou vybrány, pokud čas poslední změny spadá do časového rozsahu mezi `modifiedDatetimeStart` a `modifiedDatetimeEnd` . Čas se použije na časové pásmo UTC ve formátu "2018-12-01T05:00:00Z". <br/><br/> Mějte na paměti, že povolení tohoto nastavení bude mít vliv na celkový výkon přesunu dat, pokud chcete filtrovat velké objemy souborů. <br/><br/> Vlastnosti mohou mít **hodnotu null**, což znamená, že pro datovou sadu nebude použit filtr atributů souborů.  Když `modifiedDatetimeStart` má hodnotu DateTime, ale `modifiedDatetimeEnd` je **null**, budou vybrány soubory, jejichž atribut Last Modified je větší nebo roven hodnotě DateTime.  Když `modifiedDatetimeEnd` má hodnotu DateTime, ale `modifiedDatetimeStart` je **null**, soubory, jejichž atribut Last Modified je menší než hodnota DateTime, se vybere.| No |
| formát | Pokud chcete kopírovat soubory mezi úložišti na základě souborů (binární kopie), přeskočte oddíl formát v definicích vstupní i výstupní datové sady.<br/><br/>Pokud chcete analyzovat nebo generovat soubory s konkrétním formátem, podporují se tyto typy formátů souborů: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. V části **Formát** nastavte vlastnost **typ** na jednu z těchto hodnot. Další informace najdete v oddílech [Formát textu](supported-file-formats-and-compression-codecs-legacy.md#text-format), [formát JSON](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Formát Avro](supported-file-formats-and-compression-codecs-legacy.md#avro-format), formát [ORC](supported-file-formats-and-compression-codecs-legacy.md#orc-format)a formát [Parquet](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) . |Ne (jenom pro binární scénář kopírování) |
| komprese | Zadejte typ a úroveň komprese dat. Další informace najdete v tématu [podporované formáty souborů a kompresní kodeky](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Podporované typy jsou **gzip**, **Deflate**, **bzip2**a **ZipDeflate**.<br/>Podporované úrovně jsou **optimální** a **nejrychlejší**. |No |

>[!TIP]
>Chcete-li zkopírovat všechny soubory ve složce, zadejte hodnotu vlastnosti **intervalu** a **prefix** pro část složky.
>
>Chcete-li zkopírovat jeden soubor se zadaným názvem, zadejte název **intervalu** pro interval a **klíč** pro část složky plus název souboru.
>
>Chcete-li zkopírovat podmnožinu souborů ve složce, zadejte **interval** pro interval a **klíč** pro část složky a filtr zástupných znaků.

**Příklad: použití předpony**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
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

**Příklad: použití klíče a verze (volitelné)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
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

### <a name="legacy-source-model-for-the-copy-activity"></a>Starší zdrojový model pro aktivitu kopírování

| Vlastnost | Popis | Povinné |
|:--- |:--- |:--- |
| typ | Vlastnost **Type** zdroje aktivity kopírování musí být nastavená na **FileSystemSource**. |Yes |
| zahrnout | Určuje, zda mají být data rekurzivně čtena z podsložek nebo pouze ze zadané složky. Všimněte si, že pokud je **rekurzivní** nastavení nastaveno na **hodnotu true** a jímka je úložiště založené na souborech, prázdná složka nebo podsložka nebude kopírována ani vytvořena v jímky.<br/>Povolené hodnoty jsou **true** (výchozí) a **false**. | No |
| maxConcurrentConnections | Počet připojení, která mají být souběžně propojena s úložištěm dat. Určete pouze v případě, že chcete omezit souběžná připojení k úložišti dat. | No |

**Případě**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
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

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat, která aktivita kopírování v Azure Data Factory podporuje jako zdroje a jímky, najdete v tématu [podporovaná úložiště dat](copy-activity-overview.md#supported-data-stores-and-formats).
