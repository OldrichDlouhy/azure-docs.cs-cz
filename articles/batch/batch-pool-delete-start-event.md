---
title: Událost zahájení odstranění fondu Azure Batch
description: Referenční informace pro událost zahájení odstranění fondu Batch Tato událost je generována při spuštění operace odstranění fondu.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: d4fac833b6c77258e48b38838f6b7b133738a6e8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83723931"
---
# <a name="pool-delete-start-event"></a>Událost zahájení odstranění fondu

 Tato událost je generována při spuštění operace odstranění fondu. Vzhledem k tomu, že odstranění fondu představuje asynchronní událost, můžete očekávat, že událost odstranění fondu bude po dokončení operace odstranění vygenerována.

 Následující příklad ukazuje tělo události spuštění odstranění fondu.

```
{
    "id": "myPool1"
}
```

|Prvek|Typ|Poznámky|
|-------------|----------|-----------|
|`id`|Řetězec|ID fondu|
