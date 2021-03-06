---
title: Přehled konektoru Azure Data Factory
description: Seznamte se s podporovanými konektory v Data Factory.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 334d5b5113dba17c5abc2b4f2520bde0d16e4c06
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87007435"
---
# <a name="azure-data-factory-connector-overview"></a>Přehled konektoru Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory podporuje následující úložiště a formáty dat prostřednictvím kopírování, toku dat, vyhledávání, získávání metadat a odstraňování aktivit. Klikněte na jednotlivá úložiště dat, abyste se seznámili s podporovanými funkcemi a odpovídajícími konfiguracemi v podrobnostech.

## <a name="supported-data-stores"></a>Podporované zdroje dat

[!INCLUDE [Connector overview](../../includes/data-factory-v2-connector-overview.md)]

## <a name="supported-file-formats"></a>Podporované formáty souborů

Azure Data Factory podporuje následující formáty souborů. Nastavení založená na formátu najdete v každém článku.

- [Formát Avro](format-avro.md)
- [Binární formát](format-binary.md)
- [Formát Common Data Modelu](format-common-data-model.md)
- [Formát textu s oddělovači](format-delimited-text.md)
- [Rozdílový formát](format-delta.md)
- [Excelový formát](format-excel.md)
- [Formát JSON](format-json.md)
- [Formát ORC](format-orc.md)
- [Formát Parquet](format-parquet.md)
- [Formát XML](format-xml.md)

## <a name="next-steps"></a>Další kroky

- [Aktivita kopírování](copy-activity-overview.md)
- [Mapování toku dat](concepts-data-flow-overview.md)
- [Aktivita vyhledávání](control-flow-lookup-activity.md)
- [Aktivita získání metadat](control-flow-get-metadata-activity.md)
- [Odstranit aktivitu](delete-activity.md)
