---
title: Operátory dotazů SQL pro Azure Cosmos DB
description: Přečtěte si o operátorech SQL, jako jsou rovnost, porovnání a logické operátory podporované Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: tisande
ms.openlocfilehash: 8ef41edb687a5df39243880c897d12e83c008ec9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80063562"
---
# <a name="operators-in-azure-cosmos-db"></a>Operátory v Azure Cosmos DB

Tento článek podrobně popisuje různé operátory podporované nástrojem Azure Cosmos DB.

## <a name="equality-and-comparison-operators"></a>Operátory rovnosti a porovnání

Následující tabulka ukazuje výsledek porovnání rovnosti v rozhraní SQL API mezi libovolnými dvěma typy JSON.

| **Evřít** | **Nedefinované** | **Null** | **Logická hodnota** | **Číselná** | **Řetězec** | **Předmětů** | **Pole** |
|---|---|---|---|---|---|---|---|
| **Nedefinované** | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované |
| **Null** | Nedefinované | **Ok** | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované |
| **Logická hodnota** | Nedefinované | Nedefinované | **Ok** | Nedefinované | Nedefinované | Nedefinované | Nedefinované |
| **Číselná** | Nedefinované | Nedefinované | Nedefinované | **Ok** | Nedefinované | Nedefinované | Nedefinované |
| **Řetězec** | Nedefinované | Nedefinované | Nedefinované | Nedefinované | **Ok** | Nedefinované | Nedefinované |
| **Předmětů** | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované | **Ok** | Nedefinované |
| **Pole** | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované | Nedefinované | **Ok** |

Pro operátory porovnání, jako `>` například,, `>=` `!=` , `<` , a `<=` , porovnání mezi typy nebo mezi dvěma objekty nebo poli generuje `Undefined` .  

Pokud je výsledek skalárního výrazu `Undefined` , položka není obsažena ve výsledku, protože se `Undefined` nerovná `true` .

## <a name="logical-and-or-and-not-operators"></a>Operátory logických operátorů (AND, OR a NOT)

Logické operátory pracují s logickými hodnotami. V následujících tabulkách jsou uvedeny logické tabulky pravdy pro tyto operátory:

**OR – operátor**

Vrátí `true` , pokud je jedna z podmínek `true` .

|  | **Podmínka** | **Chybné** | **Nedefinované** |
| --- | --- | --- | --- |
| **Podmínka** |True |True |True |
| **Chybné** |True |False |Nedefinované |
| **Nedefinované** |True |Nedefinované |Nedefinované |

**AND – operátor**

Vrátí `true` , pokud jsou oba výrazy `true` .

|  | **Podmínka** | **Chybné** | **Nedefinované** |
| --- | --- | --- | --- |
| **Podmínka** |True |False |Nedefinované |
| **Chybné** |False |False |False |
| **Nedefinované** |Nedefinované |False |Nedefinované |

**NOT – operátor**

Obrátí hodnotu libovolného logického výrazu.

|  | **MĚNÍ** |
| --- | --- |
| **Podmínka** |False |
| **Chybné** |True |
| **Nedefinované** |Nedefinované |

**Priorita operátorů**

Logické operátory `OR` , `AND` a `NOT` mají úroveň priority zobrazenou níže:

| **Operátor** | **Priorita** |
| --- | --- |
| **MĚNÍ** |1 |
| **ANI** |2 |
| **ANI** |3 |

## <a name="-operator"></a>* – operátor

Speciální operátor * projektuje celou položku tak, jak je. Při použití musí být jediným projektovým polem. Dotaz, jako `SELECT * FROM Families f` je platný, ale je neplatný `SELECT VALUE * FROM Families f` `SELECT *, f.id FROM Families f` .

## <a name="-and--operators"></a>? a?? operátory

Můžete použít operátory Ternární (?) a COALESCE (??) k sestavení podmíněných výrazů, jako v programovacích jazycích, jako je C# nebo JavaScript.

Můžete použít? operátor pro vytvoření nových vlastností JSON za běhu. Například následující dotaz klasifikuje úrovně stupňů do `elementary` nebo `other` :

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Můžete také vnořit volání do? – operátor, jak je uvedeno v následujícím dotazu: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Stejně jako u jiných operátorů dotazů? operátor vyloučí položky, pokud nebyly nalezeny odkazované vlastnosti nebo se porovnávané typy liší.

Chcete použít? operátor pro efektivní kontrolu vlastnosti v položce při dotazování na částečně strukturovaných nebo smíšených dat typu. Například následující dotaz vrátí `lastName` , pokud je k dispozici, nebo `surname` Pokud `lastName` není k dispozici.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Další kroky

- [Ukázky Azure Cosmos DB .NET](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Klíčová slova](sql-query-keywords.md)
- [Klauzule SELECT](sql-query-select.md)
