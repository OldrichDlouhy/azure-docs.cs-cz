---
title: Co je privátní propojení Azure?
description: Přehled funkcí, architektury a implementace privátních odkazů Azure Přečtěte si, jak fungují privátní koncové body Azure a služba privátního propojení Azure a jak je používat.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 06/18/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: 19e7a7972af55173f19d5784b2a6ac78ae14a0f0
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87124965"
---
# <a name="what-is-azure-private-link"></a>Co je privátní propojení Azure? 
Privátní odkaz Azure vám umožňuje přístup ke službám Azure PaaS (například Azure Storage a SQL Database) a službám, které hostuje zákaznická/Partnerská služba Azure, prostřednictvím [privátního koncového bodu](private-endpoint-overview.md) ve vaší virtuální síti.

Provoz mezi vaší virtuální sítí a službou přenáší páteřní síť Microsoftu. Vystavení vaší služby pro veřejný Internet již není nutné. Ve virtuální síti můžete vytvořit vlastní [službu privátního propojení](private-link-service-overview.md) a předat ji zákazníkům. Nastavení a spotřeba pomocí privátního propojení Azure jsou konzistentní v rámci služeb Azure PaaS, Customer a Shared partnered.

> [!IMPORTANT]
> Privátní propojení Azure je teď všeobecně dostupné. K dispozici jsou všeobecně dostupné služby privátního i privátního koncového bodu (služba za službu Load Balancer úrovně Standard). Různé služby Azure PaaS se budou připojovat k privátnímu propojení Azure v různých plánech. Níže uvedený oddíl [dostupnosti](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) najdete pro přesný stav služby Azure PaaS na privátním odkazu. Známá omezení najdete v tématu [privátní koncový bod](private-endpoint-overview.md#limitations) a [Služba privátního propojení](private-link-service-overview.md#limitations). 

![Přehled privátního koncového bodu](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Klíčové výhody
Privátní propojení Azure přináší následující výhody:  
- **Služby soukromého přístupu na platformě Azure**: Připojte svoji virtuální síť ke službám v Azure bez veřejné IP adresy ve zdroji nebo cíli. Poskytovatelé služeb mohou vykreslovat své služby ve své vlastní virtuální síti a příjemci mají přístup k těmto službám ve své místní virtuální síti. Platforma privátního propojení bude zpracovávat připojení mezi příjemcem a službami přes páteřní síť Azure. 
 
- **Místní a partnerské sítě**: přístup ke službám, které běží v Azure, prostřednictvím privátního partnerského vztahu ExpressRoute, tunelových propojení VPN a partnerských virtuálních sítí s použitím privátních koncových bodů. Pro přístup ke službě není nutné nastavovat veřejný partnerský vztah ani procházet internetem. Privátní odkaz poskytuje zabezpečený způsob migrace úloh do Azure.
 
- **Ochrana před únikem dat**: privátní koncový bod je namapován na instanci prostředku PaaS namísto celé služby. Příjemci se můžou připojit jenom ke konkrétnímu prostředku. Přístup k jakémukoli jinému prostředku ve službě je blokovaný. Tento mechanismus zajišťuje ochranu před riziky úniku dat. 
 
- **Globální dosah**: Připojte soukromě ke službám běžícím v jiných oblastech. Virtuální síť příjemce může být v oblasti A a může se připojit ke službám za soukromým odkazem v oblasti B.  
 
- **Rozšiřování na vlastní služby**: umožňuje stejné prostředí a funkce pro vlastní vygenerování služby pro uživatele v Azure. Umístěním služby za standardní Azure Load Balancer můžete povolit pro privátní propojení. Příjemce se pak může připojit přímo k vaší službě pomocí privátního koncového bodu ve své vlastní virtuální síti. Žádosti o připojení můžete spravovat pomocí toku volání schválení. Privátní propojení Azure funguje pro zákazníky a služby patřící různým klientům Azure Active Directory. 

## <a name="availability"></a>Dostupnost 
 V následující tabulce jsou uvedeny služby privátních odkazů a oblasti, kde jsou k dispozici. 

|Podporované služby  |Dostupné oblasti | Další aspekty | Status  |
|:-------------------|:-----------------|:----------------|:--------|
|Služby privátního propojení za standardním Azure Load Balancer | Všechny veřejné oblasti<br/> Všechny oblasti státní správy  | Podporováno v Standard Load Balancer | GA <br/> [Další informace](https://docs.microsoft.com/azure/private-link/private-link-service-overview) |
| Azure Storage       |  Všechny veřejné oblasti       |  Podporováno na druhu účtu Pro obecné účely v2 | GA <br/> [Další informace](/azure/storage/common/storage-private-endpoints)  |
| Azure Data Lake Storage Gen2        |  Všechny veřejné oblasti      |  Podporováno na druhu účtu Pro obecné účely v2 | GA <br/> [Další informace](/azure/storage/common/storage-private-endpoints)  |
|  Azure SQL Database         | Všechny veřejné oblasti <br/> Všechny oblasti státní správy      |  Podporováno pro [zásady připojení](https://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policyhttps://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policy) proxy | GA <br/> [Další informace](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)      |
|Azure synapse Analytics (SQL Data Warehouse)| Všechny veřejné oblasti <br/> Všechny oblasti státní správy |  Podporováno pro [zásady připojení](https://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policyhttps://docs.microsoft.com/azure/azure-sql/database/connectivity-architecture#connection-policy) proxy |GA <br/> [Další informace](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)|
|Azure Cosmos DB|  Všechny veřejné oblasti<br/> Všechny oblasti státní správy | |GA <br/> [Další informace](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints)|
|  Azure Database for PostgreSQL – jeden server         | Všechny veřejné oblasti <br/> Všechny oblasti státní správy     | Podporováno pro Pro obecné účely a paměťově optimalizované cenové úrovně | GA <br/> [Další informace](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)      |
|  Azure Database for MySQL         | Všechny veřejné oblasti<br/> Všechny oblasti státní správy      |  | GA <br/> [Další informace](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)     |
|  Azure Database for MariaDB         | Všechny veřejné oblasti<br/> Všechny oblasti státní správy     |  | GA <br/> [Další informace](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)      |
|  Azure Key Vault         | Všechny veřejné oblasti<br/> Všechny oblasti státní správy      |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/key-vault/private-link-service)   |
|Azure Kubernetes Service – rozhraní API pro Kubernetes | Všechny veřejné oblasti      |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/aks/private-clusters)   |
|Azure Search | Všechny veřejné oblasti <br/> Všechny oblasti státní správy | Podporováno u služby v privátním režimu | GA   <br/> [Další informace](https://docs.microsoft.com/azure/search/search-security-overview#endpoint-access)    |
|Azure Container Registry | Všechny veřejné oblasti<br/> Všechny oblasti státní správy    | Podporováno s úrovní Premium registru kontejneru. [Kliknutí pro úrovně](https://docs.microsoft.com/azure/container-registry/container-registry-skus)| GA   <br/> [Další informace](https://docs.microsoft.com/azure/container-registry/container-registry-private-link)   |
|Azure App Configuration | Všechny veřejné oblasti      |  | Preview   |
|Azure Backup | Všechny veřejné oblasti<br/> Všechny oblasti státní správy   |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/backup/private-endpoints)   |
|Azure Event Hub | Všechny veřejné oblasti<br/>Všechny oblasti státní správy      |   | GA   <br/> [Další informace](https://docs.microsoft.com/azure/event-hubs/private-link-service)  |
|Azure Service Bus | Všechny veřejné oblasti<br/>Všechny oblasti státní správy  | Podporováno s úrovní Premium Azure Service Bus. [Kliknutí pro úrovně](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-premium-messaging) | GA   <br/> [Další informace](https://docs.microsoft.com/azure/service-bus-messaging/private-link-service)    |
|Azure Relay | Všechny veřejné oblasti      |  | Preview <br/> [Další informace](https://docs.microsoft.com/azure/service-bus-relay/private-link-service)  |
|Azure Event Grid| Všechny veřejné oblasti<br/> Všechny oblasti státní správy       |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/event-grid/network-security) |
|Azure Web Apps | Všechny veřejné oblasti      | Podporováno pro PremiumV2 Windows a Linux a elastické funkce Premium  | Preview   <br/> [Další informace](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)   |
|Azure Machine Learning | VÝCHODNÍ USA, ZÁPADNÍ USA 2, STŘED USA – JIH      |  | Preview   <br/> [Další informace](https://docs.microsoft.com/azure/machine-learning/how-to-configure-private-link)   |
| Azure Automation  | Všechny veřejné oblasti |  | Preview | |
| Azure IoT Hub | Všechny veřejné oblasti    |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/iot-hub/virtual-network-support ) |
| Azure SignalR | VÝCHODNÍ USA, ZÁPADNÍ USA 2, STŘED USA – JIH      |  | Preview   <br/> [Další informace](https://aka.ms/asrs/privatelink)   |
| Azure Monitor <br/>(Log Analytics & Application Insights) | Všechny veřejné oblasti      |  | GA   <br/> [Další informace](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security)   | 
| Synchronizace souborů Azure | Všechny veřejné oblasti      | |   GA   <br/> [Další informace](https://docs.microsoft.com/azure/storage/files/storage-sync-files-networking-endpoints?tabs=azure-portal)   |

Nejaktuálnější oznámení najdete na [stránce s aktualizacemi privátních odkazů Azure](https://azure.microsoft.com/updates/?product=private-link).

## <a name="logging-and-monitoring"></a>Protokolování a monitorování

Privátní propojení Azure má integraci s Azure Monitor. Tato kombinace umožňuje:

 - Archivace protokolů do účtu úložiště.
 - Streamování událostí do centra událostí.
 - Azure Monitor protokolování.

Na Azure Monitor můžete získat přístup k následujícím informacím: 
- **Privátní koncový bod**: 
    - Data zpracovaná privátním koncovým bodem (v/v)
 
- **Služba privátního propojení**:
    - Data zpracovaná službou privátního propojení (v/v)
    - Dostupnost portu NAT  
 
## <a name="pricing"></a>Ceny   
Podrobnosti o cenách najdete v tématu [ceny za privátní propojení Azure](https://azure.microsoft.com/pricing/details/private-link/).
 
## <a name="faqs"></a>Nejčastější dotazy  
Nejčastější dotazy najdete v tématu [Nejčastější dotazy k privátním linkám Azure](private-link-faq.md).
 
## <a name="limits"></a>Omezení  
Omezení najdete v tématu [omezení privátních propojení Azure](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits).

## <a name="service-level-agreement"></a>smlouva SLA
Pro smlouvu SLA si přečtěte téma věnované [smlouvě SLA pro privátní propojení Azure](https://azure.microsoft.com/support/legal/sla/private-link/v1_0/).

## <a name="next-steps"></a>Další kroky

- [Rychlý Start: Vytvoření privátního koncového bodu pomocí Azure Portal](create-private-endpoint-portal.md)
- [Rychlý Start: vytvoření služby privátního propojení pomocí Azure Portal](create-private-link-service-portal.md)


 
