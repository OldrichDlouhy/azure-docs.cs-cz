---
title: Prostředky pro vývoj synapse fondu SQL ve službě Azure synapse Analytics
description: Koncepce vývoje, rozhodování o návrhu, doporučení a techniky kódování pro SQL Data Warehouse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: a3c0d7924fb550761d050c9c404b1065c7d3cf72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85211489"
---
# <a name="design-decisions-and-coding-techniques-for-a-synapse-sql-pool-in-azure-synapse-analytics"></a>Rozhodnutí o návrhu a techniky kódování pro synapse fond SQL ve službě Azure synapse Analytics 
 V tomto článku najdete další materiály, které vám pomůžou lépe pochopit klíčová rozhodnutí pro návrh, doporučení a techniky kódování pro fond SQL ve službě Azure synapse.

## <a name="key-design-decisions"></a>Klíčová rozhodnutí pro návrh
Následující články zvýrazňují koncepty a rozhodnutí o návrhu pro vývoj distribuovaného datového skladu pomocí funkce fondu SQL ve službě Azure synapse:

* [připojení](../sql/connect-overview.md)
* [Concurrency](resource-classes-for-workload-management.md)
* [převody](sql-data-warehouse-develop-transactions.md)
* [uživatelsky definovaná schémata](sql-data-warehouse-develop-user-defined-schemas.md)
* [distribuce tabulky](sql-data-warehouse-tables-distribute.md)
* [indexy tabulek](sql-data-warehouse-tables-index.md)
* [oddíly tabulky](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [týkají](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Doporučení pro vývoj a techniky kódování
Následující články obsahují konkrétní techniky kódování, tipy a doporučení pro vývoj fondu SQL:

* [uložené procedury](sql-data-warehouse-develop-stored-procedures.md)
* [popisky](sql-data-warehouse-develop-label.md)
* [Náhled](sql-data-warehouse-develop-views.md)
* [dočasné tabulky](sql-data-warehouse-tables-temporary.md)
* [dynamické SQL](sql-data-warehouse-develop-dynamic-sql.md)
* [opakování](sql-data-warehouse-develop-loops.md)
* [možnosti pro seskupení](sql-data-warehouse-develop-group-by-options.md)
* [přiřazení proměnné](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Další kroky
Další referenční informace najdete v tématu [příkazy jazyka T-SQL](sql-data-warehouse-reference-tsql-statements.md).
