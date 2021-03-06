---
title: StringToArray v jazyce pro dotaz na Azure Cosmos DB
description: Přečtěte si o StringToArray funkcí SQL systému v Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 18acbd94fa3d717fc20b9e1020b9bf7c6db7744d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302912"
---
# <a name="stringtoarray-azure-cosmos-db"></a>StringToArray (Azure Cosmos DB)
 Vrátí výraz přeložený do pole. Pokud výraz nelze přeložit, vrátí nedefinované funkce.  
  
## <a name="syntax"></a>Syntaxe
  
```sql  
StringToArray(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumenty
  
*str_expr*  
   Je řetězcový výraz, který se má analyzovat jako výraz pole JSON. 
  
## <a name="return-types"></a>Návratové typy
  
  Vrátí výraz pole nebo nedefinovaný. 
  
## <a name="remarks"></a>Poznámky
  Vnořené řetězcové hodnoty musí být zapsány pomocí dvojitých uvozovek, aby byly platné JSON. Podrobnosti o formátu JSON najdete v tématu [JSON.org](https://json.org/) .
  
## <a name="examples"></a>Příklady
  
  Následující příklad ukazuje, jak se `StringToArray` chová v různých typech. 
  
 Níže jsou uvedeny příklady s platným vstupem.

```sql
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Zde je sada výsledků.

```json
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Následuje příklad neplatného vstupu. 
   
 Jednoduché uvozovky v rámci pole nejsou platné JSON.
I když jsou v rámci dotazu platné, nebudou analyzovány na platná pole. Řetězce v řetězci pole musí být buď uvozeny řídicím znakem "[ \\ " \\ "]", nebo okolní uvozovka musí být jednoduché "[" "]".

```sql
SELECT
    StringToArray("['5','6','7']")
```

Zde je sada výsledků.

```json
[{}]
```

Níže jsou uvedeny příklady neplatného vstupu.
   
 Předaný výraz se analyzuje jako pole JSON; Následující pole nejsou vyhodnocena pro typ Array a proto vracejí nedefinované.
   
```sql
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

Zde je sada výsledků.

```json
[{}]
```

## <a name="remarks"></a>Poznámky

Tato systémová funkce nebude index využívat.

## <a name="next-steps"></a>Další kroky

- [Azure Cosmos DB funkce řetězce](sql-query-string-functions.md)
- [Systémové funkce Azure Cosmos DB](sql-query-system-functions.md)
- [Úvod do Azure Cosmos DB](introduction.md)
