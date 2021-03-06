---
title: 'Vzor: definice zásad skupiny s iniciativami'
description: Tento model Azure Policy poskytuje příklad definice zásad skupiny v iniciativě.
ms.date: 06/29/2020
ms.topic: sample
ms.openlocfilehash: 065e7632d76feccdd6d2d5a411ba7cdf50aeaf68
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565701"
---
# <a name="azure-policy-pattern-group-policy-definitions"></a>Vzor Azure Policy: definice zásad skupiny

Iniciativa je skupina definic zásad. Seskupením souvisejících definic zásad do jednoho objektu můžete vytvořit jedno přiřazení, které by bylo více přiřazení.

## <a name="sample-initiative-definition"></a>Definice ukázkové iniciativy

V této iniciativě se nasadí dvě definice zásad, z nichž každý vezme parametry **TagName** a **tagValue** . Vlastní iniciativa má dva parametry: **costCenterValue** a **productNameValue**.
Každý z těchto parametrů iniciativy je uvedený u každé ze seskupených definic zásad. Tento návrh maximalizuje opětovné použití stávajících definic zásad a přitom omezuje počet přiřazení, která jsou podle potřeby implementovaná.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json":::

### <a name="explanation"></a>Vysvětlení

#### <a name="initiative-parameters"></a>Parametry iniciativ

Iniciativa může definovat vlastní parametry, které jsou pak předány do seskupených definic zásad.
V tomto příkladu jsou **costCenterValue** i **productNameValue** definovány jako parametry iniciativy. Hodnoty jsou k dispozici v případě, že je iniciativa přiřazena.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="5-18":::

#### <a name="includes-policy-definitions"></a>Zahrnuje definice zásad

Každá zahrnutá definice zásad musí poskytovat **policyDefinitionId** a pole **parametrů** , pokud definice zásady akceptuje parametry. V následujícím fragmentu kódu zahrnutá definice zásad má dva parametry: **TagName** a **tagValue**. parametr **TagName** je definován s literálem, ale **TagValue** používá parametr **costCenterValue** definovaný iniciativou. Tento průchozí hodnoty zlepšuje opakované použití.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="30-40":::

## <a name="next-steps"></a>Další kroky

- Zkontrolujte další [vzory a předdefinované definice](./index.md).
- Projděte si [strukturu definic Azure Policy](../concepts/definition-structure.md).
- Projděte si [Vysvětlení efektů zásad](../concepts/effects.md).