---
title: Parametrizovat propojené služby v Azure Data Factory
description: Naučte se, jak parametrizovat propojené služby v Azure Data Factory a v době běhu předávat dynamické hodnoty.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 85689661e7f0d170cd88edde8985f46285e679c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987772"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parametrizovat propojené služby v Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Nyní můžete parametrizovat propojenou službu a v době běhu předat dynamické hodnoty. Pokud se například chcete připojit k různým databázím na stejném logickém serveru SQL, můžete v definici propojené služby nyní parametrizovat název databáze. Tím zabráníte tomu, abyste vytvořili propojenou službu pro každou databázi na logickém SQL serveru. V definici propojené služby můžete také parametrizovat jiné vlastnosti, například *uživatelské jméno.*

K parametrizovat propojených služeb můžete použít uživatelské rozhraní Data Factory v Azure Portal nebo programovacím rozhraní.

> [!TIP]
> Doporučujeme, abyste neparametrizovati hesla ani tajné klíče. Místo toho uložte všechny připojovací řetězce v Azure Key Vault a parametrizovat *název tajného klíče*.

Pokud chcete tuto funkci seznámit a předvedení této funkce, podívejte se na následující video:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Podporované zdroje dat

V tuto chvíli je propojená služba Parametrizace podporovaná v uživatelském rozhraní Data Factory pro následující úložiště dat. U všech ostatních úložišť dat můžete propojenou službu parametrizovat tak, že na kartě **připojení** vyberete ikonu **kódu** a použijete Editor JSON.

- Amazon Redshift
- Azure Cosmos DB (SQL API)
- Azure Database for MySQL
- Azure SQL Database
- Azure Synapse Analytics (dříve SQL DW)
- MySQL
- Oracle
- SQL Server

## <a name="data-factory-ui"></a>Uživatelské rozhraní Data Factory

![Přidat dynamický obsah do definice propojené služby](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Vytvoření nového parametru](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
