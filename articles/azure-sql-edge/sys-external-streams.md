---
title: sys. external_streams (Transact-SQL) – Azure SQL Edge (Preview)
description: Další informace o použití sys. external_streams ve službě Azure SQL Edge (Preview)
keywords: sys. external_streams, Edge SQL
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 8200d1814537a76db357704d6baf3bf482c587e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235110"
---
# <a name="sysexternal_streams-transact-sql"></a>sys.external_streams (Transact-SQL)

Vrátí řádek pro každý objekt externího datového proudu vytvořený v rámci oboru databáze.

|Název sloupce|Datový typ|Description|  
|-----------------|---------------|-----------------|
|**Jméno**|**musí**|Název datového proudu. Je v rámci databáze jedinečný.|
|**object_id**|**int**|identifikační číslo objektu pro objekt datového proudu. Je v rámci databáze jedinečný.|
|**principal_id**|**int**|ID objektu zabezpečení, který vlastní toto sestavení|
|**schema_id**|**int**| ID schématu, které obsahuje objekt|
|**parent_object_id**|**účet**| identifikační číslo objektu pro nadřazený objekt tohoto datového proudu. V aktuální implementaci je tato hodnota vždycky null.|
|**textový**|**char (2)**|Typ objektu. U objektů Stream je typ vždycky "ES".|
|**type_desc**|**nvarchar (60)**| Popis typu objektu U objektů Stream je typ vždycky EXTERNAL_STREAM.|
|**create_date**|**datetime**| Datum, kdy byl objekt vytvořen.|
|**modify_date**|**datetime**| Datum, kdy byl objekt naposledy změněn pomocí příkazu ALTER.|
|**is_ms_shipped**|**bit**| Objekt vytvořený interní komponentou.|  
|**is_published**|**bit**|Objekt je publikovaný.|  
|**is_schema_published**|**bit**|Je Publikováno pouze schéma objektu.|
|**max_column_id_used**|**bit**| Tento sloupec se používá pro interní účely a v budoucnu se odebere.|  
|**uses_ansi_nulls**|**bit**| Objekt Stream byl vytvořen pomocí možnosti SET ANSI_NULLS Database ON.|
|**data_source_id**|**int**| ID objektu pro externí zdroj dat reprezentované objektem Stream |  
|**file_format_id**|**int**| ID objektu pro formát externího souboru, který používá objekt Stream. Aby bylo možné určit skutečné rozložení dat, na které odkazuje externí datový proud, je nutné použít formát externího souboru.| 
|**oblasti**|**varchar(max)**| Cíl pro objekt externího datového proudu. Další informace najdete v tématu [Vytvoření externího streamu](overview.md) . |
|**input_option**|**varchar(max)**| Možnosti vstupu používané při vytváření externího datového proudu. Další informace najdete v tématu [Vytvoření externího streamu](overview.md) . |
|**output_option**|**varchar(max)**| Možnosti výstupu používané při vytváření externího datového proudu. Další informace najdete v tématu [Vytvoření externího streamu](overview.md) . | 

## <a name="permissions"></a>Oprávnění

Viditelnost metadat v zobrazeních katalogu je omezená na to, že uživatel buď vlastní, nebo ke kterému má uživatel udělené oprávnění. Další informace najdete v tématu [Konfigurace viditelnosti metadat](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Viz také

- [Zobrazení katalogu (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Systémová zobrazení (Transact-SQL)](/sql/t-sql/language-reference/)
