---
title: Ukázka podrobného plánu úlohy ASE/SQL ISO 27001 – přehled
description: Přehled a architektura ukázky podrobného plánu úlohy ISO 27001 App Service Environment/SQL Database
ms.date: 07/13/2020
ms.topic: sample
ms.openlocfilehash: 71044ae1754cf4bdfc42be7b6585a1905dce426c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536829"
---
# <a name="overview-of-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>Přehled ukázky podrobného plánu úlohy ISO 27001 App Service Environment/SQL Database

Ukázka podrobného plánu úlohy ISO 27001 App Service Environment/SQL Database poskytuje další informace k ukázce podrobného plánu [ISO 27001: Sdílené služby](../iso27001-shared/index.md).
Tento podrobný plán pomáhá zákazníkům nasazovat cloudové architektury, které nabízejí řešení pro scénáře s požadavky na dodržování předpisů nebo akreditaci.

K dispozici jsou dvě ukázky podrobného plánu ISO 27001, tato ukázka a ukázka podrobného plánu [ISO 27001: Sdílené služby](../iso27001-shared/index.md).

> [!IMPORTANT]
> Tato ukázka je závislá na infrastruktuře nasazené ukázkou podrobného plánu [ISO 27001: Sdílené služby](../iso27001-shared/index.md). Musí být nasazená jako první.

## <a name="architecture"></a>Architektura

Ukázka podrobného plánu úlohy ISO 27001 App Service Environment/SQL Database nasazuje webové prostředí založené na platformě jako službě. Toto prostředí se dá využít k hostování několika webových aplikací, webových rozhraní API a instancí SQL Database, které dodržují standardy ISO 27001. Tato ukázka podrobného plánu závisí na ukázce podrobného plánu [ISO 27001: Sdílené služby](../iso27001-shared/index.md).

:::image type="content" source="../../media/sample-iso27001-ase-sql-workload/iso27001-ase-sql-workload-blueprint-sample-design.png" alt-text="Návrh ukázky podrobného plánu úlohy ASE/SQL ISO 27001" border="false":::

Toto prostředí je tvořené několika službami Azure, které se využívají k zajištění zabezpečené a plně monitorované infrastruktury úloh na podnikové úrovni, která je založená na standardech ISO 27001. Toto prostředí tvoří:

- Role [řízení přístupu na základě role](../../../../role-based-access-control/overview.md) (RBAC) nazvaná DevOps, která má oprávnění nasazovat a spravovat prostředky v p[prostředích Azure App Service Environment](../../../../app-service/environment/intro.md) nasazených ukázkou podrobného plánu
- [Zásady Azure](../../../policy/overview.md) k pevnému určení služeb, které je možné v tomto prostředí nasadit, a zamítnutí vytváření libovolných prostředků veřejných IP adres (PIP)
- Virtuální síť obsahující jednu podsíť, která je propojená k předem připravenému prostředí [sdílených služeb](../iso27001-shared/index.md) a vynucuje průchod veškerého provozu bránou firewall [sdílených služeb](../iso27001-shared/index.md). Tato virtuální síť je hostitelem pro tyto prostředky:
  - [Prostředí Azure App Service Environment](../../../../app-service/environment/intro.md), která se dají využít k hostování jedné nebo několika webových aplikací, webových rozhraní API nebo služeb
  - Instance služby [Azure Key Vault](../../../../key-vault/general/overview.md) využívající koncový bod služby virtuální sítě, a to pro ukládání tajných kódů využívaných aplikacemi, které jsou spuštěné v prostředí příslušné úlohy
  - Instance serveru [Azure SQL Database](../../../../azure-sql/database/sql-database-paas-overview.md) využívající koncový bod služby virtuální sítě, a to pro hostování databází využívaných pro aplikace v prostředí příslušné úlohy

## <a name="next-steps"></a>Další kroky

Prošli jste si přehled a architekturu ukázky podrobného plánu úlohy ISO 27001 App Service Environment/SQL Database. Jako další si projděte následující články, ve kterých se dozvíte víc o mapování kontrol a o postupu nasazení této ukázky:

> [!div class="nextstepaction"]
> [Podrobný plán úlohy ISO 27001 App Service Environment/SQL Database – mapování kontrol](./control-mapping.md)
> [Podrobný plán úlohy ISO 27001 App Service Environment/SQL Database – kroky pro nasazení](./deploy.md)

Další články věnované podrobným plánům a postupu jejich využití:

- Další informace o [životním cyklu podrobného plánu](../../concepts/lifecycle.md)
- Principy použití [statických a dynamických parametrů](../../concepts/parameters.md)
- Další informace o přizpůsobení [pořadí podrobných plánů](../../concepts/sequencing-order.md)
- Použití [zamykání prostředků podrobného plánu](../../concepts/resource-locking.md)
- Další informace o [aktualizaci existujících přiřazení](../../how-to/update-existing-assignments.md)
