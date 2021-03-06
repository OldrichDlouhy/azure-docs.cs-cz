---
title: Detekce značky – Počítačové zpracování obrazu
titleSuffix: Azure Cognitive Services
description: Tento článek popisuje specializovaný režim detekce objektů; rozpoznání značky nebo loga pomocí rozhraní API pro počítačové zpracování obrazu.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: 50e4fe1e2573c8566bbdf5697bb81b025a00935c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80131742"
---
# <a name="detect-popular-brands-in-images"></a>Detekce oblíbených značek na obrázcích

Rozpoznávání značek je specializovaný režim [detekce objektů](concept-object-detection.md) , který používá databázi tisíců globálních log k identifikaci komerčních značek na obrázcích nebo videu. Tuto funkci můžete použít například k tomu, abyste zjistili, které značky jsou nejoblíbenější na sociálních médiích nebo ve většině druhů v mediálním umístění.

Služba Počítačové zpracování obrazu zjišťuje, zda v dané imagi existují logo značky; Pokud ano, vrátí název značky, hodnocení spolehlivosti a souřadnice ohraničujícího rámečku kolem loga.

Integrovaná databáze loga zahrnuje oblíbené značky v zákaznické elektronikě, ošacení a dalších. Pokud zjistíte, že služba Počítačové zpracování obrazu nedetekuje značku, kterou hledáte, je možné, že bude lepší pracovat s vytvářením a školením vlastního detektoru loga pomocí služby [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) .

## <a name="brand-detection-example"></a>Příklad detekce značky

Následující odpovědi JSON znázorňují, co Počítačové zpracování obrazu vrací při detekci značek v ukázkových obrázcích.

![Červená košile s popiskem a logem Microsoftu](./Images/red-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":20,
         "y":97,
         "w":62,
         "h":52
      }
   }
]
```

V některých případech značka detektoru zachová jak obrázek loga, tak název stylizované značky jako dvě samostatná loga.

![Šedý Sweatshirt s popiskem a logem Microsoftu](./Images/gray-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":106,
         "w":55,
         "h":46
      }
   },
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":86,
         "w":202,
         "h":63
      }
   }
]
```

## <a name="use-the-api"></a>Použití rozhraní API

Funkce detekce značky je součástí rozhraní API pro [analýzu obrazu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) . Toto rozhraní API můžete zavolat prostřednictvím nativní sady SDK nebo prostřednictvím volání REST. Do `Brands` parametru dotazu **visualFeatures** zahrňte. Až získáte úplnou odpověď ve formátu JSON, stačí analyzovat řetězec pro obsah `"brands"` oddílu.

* [Rychlý Start: Počítačové zpracování obrazu .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Rychlý Start: analýza obrázku (REST API)](./quickstarts/csharp-analyze.md)
