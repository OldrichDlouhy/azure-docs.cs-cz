---
title: Zásady pro označování prostředků
description: Popisuje zásady Azure, které můžete přiřadit k zajištění shody značek.
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e3eeb28ea23b18c3492f68d2fac294fc014420c5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "82147868"
---
# <a name="assign-policies-for-tag-compliance"></a>Přiřazení zásad pro požadavky na značky

Pomocí [Azure Policy](../../governance/policy/overview.md) vynutili pravidla a konvence označování. Vytvořením zásady se vyhnete scénáři nasazení prostředků ve vašem předplatném, které nemají očekávané značky pro vaši organizaci. Místo ručního použití značek nebo hledání prostředků, které nedodržují předpisy, vytvoříte zásadu, která při nasazení automaticky použije potřebné značky. Značky se teď dají použít u existujících prostředků s novým efektem [změny](../../governance/policy/concepts/effects.md#modify) a [úlohou nápravy](../../governance/policy/how-to/remediate-resources.md). V následující části jsou uvedeny příklady zásad pro značky.

## <a name="policies"></a>Zásady

[!INCLUDE [Tag policies](../../../includes/policy/samples/bycat/policies-tags.md)]

## <a name="next-steps"></a>Další kroky

* Další informace o označování prostředků najdete v tématu [použití značek k uspořádání prostředků Azure](tag-resources.md).
* Ne všechny typy prostředků podporují značky. Pokud chcete zjistit, jestli můžete pro typ prostředku použít značku, přečtěte si téma [Podpora značek pro prostředky Azure](tag-support.md).
