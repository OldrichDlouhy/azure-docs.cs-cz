---
title: Ukázka podrobného plánu Základy CAF – přehled
description: Přehled a architektura přechodu na cloud pro Azure (CAF) pro ukázkový podrobný plán Základy CAF.
ms.date: 04/15/2020
ms.topic: sample
ms.openlocfilehash: 3291fdc299652d2b22bff89f5b1dadbdc064e561
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2020
ms.locfileid: "82901306"
---
# <a name="overview-of-the-microsoft-cloud-adoption-framework-for-azure-foundation-blueprint-sample"></a>Přehled architektury přechodu na cloud pro Azure od Microsoftu pro ukázkový podrobný plán Základy

Ukázkový podrobný plán Základy architektury přechodu na cloud pro Azure od Microsoftu (CAF) nasadí sadu základních prostředků infrastruktury a ovládacích prvků zásad, které budete potřebovat pro vaši první aplikaci Azure na produkční úrovni. Tento podrobný plán je založený na osvědčeném vzoru zjištěném v CAF.

## <a name="architecture"></a>Architektura

Ukázkový podrobný plán Základy CAF nasadí do Azure doporučené prostředky infrastruktury, které organizace můžou využít k zavedení základních kontrol nutných ke správě jejich cloudových aktiv. Tato ukázka nasadí a vynutí prostředky, zásady a šablony, které organizacím umožní, aby s Azure mohli bez obav začít.

:::image type="content" source="../../media/caf-blueprints/caf-foundation-architecture.png" alt-text="Základy CAF: Obrázek popisuje, co se nainstaluje v rámci pokynů CAF pro vytvoření základu pro zahájení práce v Azure." border="false":::

Tato implementace je tvořená několika službami Azure, které se využívají k zajištění zabezpečeného a plně monitorovaného základu na podnikové úrovni. Toto prostředí tvoří:

- Instance služby [Azure Key Vault](../../../../key-vault/general/overview.md), která slouží k hostování tajných kódů používaných pro virtuální počítače nasazené v prostředí sdílených služeb
- Nasazení služby [Log Analytics](../../../../azure-monitor/overview.md), aby se zajistilo, že se všechny akce a služby připojují k centrálnímu umístění, a to od okamžiku, kdy vaše zabezpečené nasazení spustíte v [účtech úložiště](../../../../storage/common/storage-introduction.md) pro protokolování diagnostiky
- Nasazení služby [Azure Security Center](../../../../security-center/security-center-intro.md) (standardní verze) zajišťující ochranu před hrozbami pro vaše migrované úlohy
- Podrobný plán také definuje a nasazuje [zásady Azure](../../../policy/overview.md): 
  - Pro označování (CostCenter) použité pro skupiny prostředků
  - Pro připojení prostředků do skupiny prostředků se značkou CostCenter
  - Pro povolení oblasti Azure pro prostředky a skupiny prostředků
  - Pro povolení skladových položek účtu úložiště (volí se během nasazování)
  - Pro povolení skladových položek virtuálních počítačů Azure (volí se během nasazování)
  - Vyžadování nasazení Network Watcheru 
  - Pro vyžadování šifrování zabezpečeného přenosu pro účet Azure Storage
  - Pro zamítnutí typů prostředků (volí se během nasazování)  
- Iniciativy
  - Povolení monitorování ve službě Azure Security Center (89 zásad)

Všechny tyto prvky dodržují prověřené postupy publikované v článku zaměřeném na [Centrum architektury Azure – referenční architektury](/azure/architecture/reference-architectures/).

> [!NOTE]
> Podrobný plán Základy CAF vymezuje základní architekturu pro vaše úlohy.
> V rámci této základní architektury je ale pořád potřeba úlohy nasadit.

Další informace najdete v tématu [Architektura přechodu na cloud pro Azure od Microsoftu – připraveno](/azure/cloud-adoption-framework/ready/).

## <a name="next-steps"></a>Další kroky

Prošli jste si přehled a architekturu ukázky podrobného plánu Základy CAF.

> [!div class="nextstepaction"]
> [Podrobný plán Základy CAF – kroky nasazení](./deploy.md)

Další články věnované podrobným plánům a postupu jejich využití:

- Další informace o [životním cyklu podrobného plánu](../../concepts/lifecycle.md)
- Principy použití [statických a dynamických parametrů](../../concepts/parameters.md)
- Další informace o přizpůsobení [pořadí podrobných plánů](../../concepts/sequencing-order.md)
- Použití [zamykání prostředků podrobného plánu](../../concepts/resource-locking.md)
- Další informace o [aktualizaci existujících přiřazení](../../how-to/update-existing-assignments.md)
