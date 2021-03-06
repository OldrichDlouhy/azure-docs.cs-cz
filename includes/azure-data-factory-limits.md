---
title: zahrnout soubor
description: zahrnout soubor
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 01/08/2020
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 995339f91be37b71d73b409f3eaeafb3dd1777ab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84754607"
---
Azure Data Factory je víceklientské služba, která má následující výchozí omezení, aby se zajistilo, že předplatná zákazníka budou chráněná před jednotlivými úlohami. Pokud chcete zvýšit limity až do maximálního počtu pro vaše předplatné, obraťte se na podporu.

### <a name="version-2"></a>Verze 2

| Prostředek | Výchozí omezení | Maximální omezení |
| -------- | ------------- | ------------- |
| Datové továrny v předplatném Azure | 800 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Celkový počet entit, jako jsou kanály, datové sady, triggery, propojené služby a prostředí Integration runtime v rámci datové továrny | 5 000 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Celkový počet jader procesoru pro prostředí Azure-SSIS Integration runtime v rámci jednoho předplatného | 256 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžné spouštění kanálů na objekt pro vytváření dat, která jsou sdílená mezi všemi kanály ve výrobě | 10 000  | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžná externí aktivita se spouští na základě předplatného na [Azure Integration runtime oblasti](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) .<br><small>Externí aktivity jsou spravované v prostředí Integration runtime, ale spouštějí se na propojených službách, včetně datacihly, uložených procedur, HDInsights, webů a dalších.</small> | 3000 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžná spuštění aktivity kanálu na předplatné na [Azure Integration runtime oblast](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location) <br><small>Aktivity kanálu se spouštějí v prostředí Integration runtime, včetně vyhledávání, GetMetadata a odstranění.</small>| 1000 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžné operace vytváření obsahu na předplatné na [Azure Integration runtime oblasti](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)<br><small>Včetně testovacího připojení, procházení seznamu složek a seznamu tabulek, náhled dat. | 200 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Počet současných jednotek integrace dat<sup>1</sup> na předplatné na [Azure Integration runtime oblast](../articles/data-factory/concepts-integration-runtime.md#integration-runtime-location)| Skupina oblastí 1<sup>2</sup>: 6000<br>Skupina oblastí 2<sup>2</sup>: 3000<br>Skupina oblastí 3<sup>2</sup>: 1500 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Maximální počet aktivit na kanál, které obsahují interní aktivity pro kontejnery | 40 | 40 |
| Maximální počet propojených prostředí Integration Integration, které se dají vytvořit na jednom místním prostředí Integration runtime | 100 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Maximální počet parametrů na kanál | 50 | 50 |
| Položky ForEach | 100 000 | 100 000 |
| Paralelismus ForEach | 20 | 50 |
| Maximální počet spuštění ve frontě na kanál | 100 | 100 |
| Počet znaků na výraz | 8 192 | 8 192 |
| Minimální interval spuštění bubnu okna | 15 min | 15 min |
| Maximální časový limit pro spuštění aktivit kanálu | 7 dní | 7 dní |
| Počet bajtů na objekt pro objekty kanálu<sup>3</sup> | 200 KB | 200 KB |
| Počet bajtů na objekt pro datovou sadu a objekty propojené služby<sup>3</sup> | 100 KB | 2 000 KB |
| Jednotky pro integraci dat<sup>1</sup> na spuštění aktivity kopírování | 256 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Zápis volání rozhraní API | 1200/h<br/><br/> Toto omezení je stanoveno Azure Resource Manager, nikoli Azure Data Factory. | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Číst volání rozhraní API | 12500/h<br/><br/> Toto omezení je stanoveno Azure Resource Manager, nikoli Azure Data Factory. | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Monitorování dotazů za minutu | 1 000 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Operace CRUD entity za minutu | 50 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Maximální doba relace ladění toku dat | 8 hodin | 8 hodin |
| Souběžný počet datových toků na prostředí Integration runtime | 50 | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžný počet relací ladění toku dat na uživatele na továrnu | 3 | 3 |
| Omezení hodnoty TTL Azure IR toku dat | 4 hodiny | [Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |

<sup>1</sup> v rámci operace kopírování z cloudu do cloudu se používá jednotka pro integraci dat (diú). Další informace najdete v části [Data Integration units (verze 2)](../articles/data-factory/copy-activity-performance.md#data-integration-units). Informace o fakturaci najdete v tématu [Azure Data Factory Price](https://azure.microsoft.com/pricing/details/data-factory/).

<sup>2</sup> [Azure Integration runtime](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) jsou [globálně dostupné](https://azure.microsoft.com/global-infrastructure/services/) , aby se zajistilo dodržování předpisů, efektivita a snížení nákladů na výstup sítě. 

| Skupina oblastí | Oblasti | 
| -------- | ------ |
| Skupina oblastí 1 | Střed USA, Východní USA, východní USA 2, Severní Evropa, Západní Evropa, Západní USA západní USA 2 |
| Skupina oblastí 2 | Austrálie – východ, Austrálie – jihovýchod, Brazílie – jih, Střed Indie, Japonsko – východ, USA – Northcentral, střed USA, jihovýchodní Asie, Středozápadní USA |
| Skupina oblastí 3 | Kanada – střed, Východní Asie, Francie – střed, Korea – střed, Velká Británie – jih |

<sup>3</sup> kanály, datové sady a propojené objekty služby reprezentují logické seskupení vašich úloh. Omezení pro tyto objekty se nevztahují na množství dat, která můžete přesunout a zpracovat pomocí Azure Data Factory. Data Factory je navržená tak, aby zpracovávala petabajty data.

### <a name="version-1"></a>Verze 1

| **Prostředek** | **Výchozí omezení** | **Maximální omezení** |
| --- | --- | --- |
| Kanály v rámci datové továrny |2,500 |[Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Datové sady v rámci datové továrny |5 000 |[Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Souběžné řezy na sadu dat |10 |10 |
| Počet bajtů na objekt pro objekty kanálu<sup>1</sup> |200 KB |200 KB |
| Počet bajtů na objekt pro datovou sadu a objekty propojené služby<sup>1</sup> |100 KB |2 000 KB |
| Jádra clusteru Azure HDInsight na vyžádání v rámci předplatného<sup>2</sup> |60 |[Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Počet jednotek pohybu dat v cloudu na běh aktivity kopírování<sup>3</sup> |32 |[Obraťte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)se na podporu. |
| Počet opakování pro spuštění aktivit kanálu |1 000 |MaxInt (32 bitů) |

<sup>1</sup> kanál, datová sada a objekty propojené služby reprezentují logické seskupení vašich úloh. Omezení pro tyto objekty se nevztahují na množství dat, která můžete přesunout a zpracovat pomocí Azure Data Factory. Data Factory je navržená tak, aby zpracovávala petabajty data.

<sup>2</sup> jádra HDInsight na vyžádání se přidělují mimo předplatné, které obsahuje datovou továrnu. V důsledku toho je předchozí limit Data Factory vyžádaného limitu pro jádro HDInsight na vyžádání. Liší se od limitu jader, který je přidružený k vašemu předplatnému Azure.

<sup>3</sup> jednotka pro pohyb dat v cloudu (DMU) pro verzi 1 se používá v operaci kopírování z cloudu do cloudu. Další informace najdete v části [jednotky přesunu dat v cloudu (verze 1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units). Informace o fakturaci najdete v tématu [Azure Data Factory Price](https://azure.microsoft.com/pricing/details/data-factory/).

| **Prostředek** | **Výchozí dolní limit** | **Minimální limit** |
| --- | --- | --- |
| Interval plánování |15 minut |15 minut |
| Interval mezi opakovanými pokusy |1 sekunda |1 sekunda |
| Hodnota časového limitu opakování |1 sekunda |1 sekunda |

#### <a name="web-service-call-limits"></a>Omezení volání webové služby
Azure Resource Manager má omezení pro volání rozhraní API. Volání rozhraní API můžete nastavit na sazbu v rámci [omezení Azure Resource Manager API](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits).
