---
title: 'Vzor: účinky definice zásady'
description: Tento model Azure Policy poskytuje příklad použití různých efektů definice zásady.
ms.date: 06/29/2020
ms.topic: sample
ms.openlocfilehash: 9c7e7689aa837a2dba0992112df3cd36ac9affe6
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565726"
---
# <a name="azure-policy-pattern-effects"></a>Azure Policy – vzor: efekty

Azure Policy má množství [efektů](../concepts/effects.md) , které určují, jak služba reaguje na prostředky, které nedodržují předpisy. Některé efekty jsou jednoduché a v definici zásad nevyžadují žádné další vlastnosti, zatímco jiné vyžadují několik vlastností.

## <a name="sample-1-simple-effect"></a>Ukázka 1: jednoduchý efekt

Tato definice zásady kontroluje, zda značka definovaná v parametru **TagName** existuje ve vyhodnoceném prostředku. Pokud značka ještě neexistuje, efekt [změny](../concepts/effects.md#modify) se aktivuje, aby se přidala značka s hodnotou v parametru **tagValue**.

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json":::

### <a name="sample-1-explanation"></a>Ukázka 1: vysvětlení

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-1.json" range="40-50":::

Efekt **úprav** vyžaduje blok **policyRule. then. Details** , který definuje **roleDefinitionIds** a **operace**. Tyto parametry informují Azure Policy, jaké role jsou potřeba k přidání značky a nápravě prostředku a kterou operaci **úprav** provést. V tomto příkladu je **operace** _přidána_ a parametry jsou použity k nastavení značky a její hodnoty.

## <a name="sample-2-complex-effect"></a>Ukázka 2: složitý efekt

Tato definice zásad Audituje každý virtuální počítač, pokud není k dispozici rozšíření definované v parametrech typu **Vydavatel** a **typ**. Používá [auditIfNotExists](../concepts/effects.md#auditifnotexists) ke kontrole prostředku, který souvisí s virtuálním počítačem, a zjistí, jestli existuje instance, která odpovídá definovaným parametrům. Tento příklad kontroluje typ **rozšíření** .

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json":::

### <a name="sample-2-explanation"></a>Ukázka 2: vysvětlení

:::code language="json" source="~/policy-templates/patterns/pattern-effect-details-2.json" range="45-58":::

**AuditIfNotExists** efekt vyžaduje blok **policyRule. then. Details** pro definování **typu** i **existenceCondition** pro hledání. **ExistenceCondition** používá prvky jazyka zásad, jako jsou například [logické operátory](../concepts/definition-structure.md#logical-operators), k určení, zda existuje odpovídající související prostředek. V tomto příkladu jsou v parametrech definovány hodnoty kontrolované proti jednotlivým [aliasům](../concepts/definition-structure.md#aliases) .

## <a name="next-steps"></a>Další kroky

- Zkontrolujte další [vzory a předdefinované definice](./index.md).
- Projděte si [strukturu definic Azure Policy](../concepts/definition-structure.md).
- Projděte si [Vysvětlení efektů zásad](../concepts/effects.md).