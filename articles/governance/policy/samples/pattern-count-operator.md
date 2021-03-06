---
title: 'Vzor: operátor Count v definici zásady'
description: Tento model Azure Policy poskytuje příklad použití operátoru Count v definici zásady.
ms.date: 06/29/2020
ms.topic: sample
ms.openlocfilehash: 807890b7fb08d790deff6e0be9e08ad91c4ec44d
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/30/2020
ms.locfileid: "85565742"
---
# <a name="azure-policy-pattern-the-count-operator"></a>Azure Policy vzor: operátor Count

Operátor [Count](../concepts/definition-structure.md#count) vyhodnocuje členy \[ \* \] aliasu.

## <a name="sample-policy-definition"></a>Definice ukázkové zásady

Tato definice zásad [Audituje](../concepts/effects.md#audit) skupiny zabezpečení sítě, které jsou nakonfigurované tak, aby umožňovaly provoz příchozího protokol RDP (Remote Desktop Protocol) (RDP)

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json":::

### <a name="explanation"></a>Vysvětlení

Základní komponenty operátoru **Count** jsou _pole_, _kde_a podmínka. Každá je zvýrazněna v následujícím fragmentu kódu.

- _pole_ udává počet, který [alias](../concepts/definition-structure.md#aliases) vyhodnocuje členy. Tady se díváte na _pole_ aliasu **securityRules \[ \* \] ** skupiny zabezpečení sítě.
- _kde_ nástroj používá jazyk zásad k definování, které členy _pole_ splňují kritéria. V tomto příkladu skupiny logických operátorů **allOf** seskupují tři odlišná vyhodnocení podmínky vlastností _pole_ aliasu: _Direction_, _Access_a _destinationPortRange_.
- Podmínka Count v tomto příkladu je **větší**. Počet se vyhodnotí jako true, pokud jeden nebo více členů _pole_ aliasu odpovídá klauzuli _WHERE_ .

:::code language="json" source="~/policy-templates/patterns/pattern-count-operator.json" range="12-32" highlight="3,4,20":::

## <a name="next-steps"></a>Další kroky

- Zkontrolujte další [vzory a předdefinované definice](./index.md).
- Projděte si [strukturu definic Azure Policy](../concepts/definition-structure.md).
- Projděte si [Vysvětlení efektů zásad](../concepts/effects.md).