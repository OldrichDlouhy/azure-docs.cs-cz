---
title: Předem vytvořená entita person – LUIS
titleSuffix: Azure Cognitive Services
description: Tento článek obsahuje předem připravené informace o entitách v Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 768c719211e8a8f2133d3798343d076e795a3da0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273421"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Předem sestavená entita person pro aplikaci LUIS
Entita s předdefinovaným jménem detekuje osoby. Vzhledem k tomu, že je tato entita již vyškolená, nemusíte do záměrů aplikace přidat příklad projevy obsahující jméno osoby. entita person je podporovaná v anglické a čínské [jazykové verzi](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>Překlad pro entitu Person

Pro dotaz se vrátí následující objekty entity:

`Is Jill Jones in Cairo?`


#### <a name="v3-response"></a>[Odpověď V3](#tab/V3)


Následující kód JSON je s `verbose` parametrem nastaveným `false`na:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-response"></a>[Podrobná odpověď V3](#tab/V3-verbose)
Následující kód JSON je s `verbose` parametrem nastaveným `true`na:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ],
    "$instance": {
        "personName": [
            {
                "type": "builtin.personName",
                "text": "Jill Jones",
                "startIndex": 3,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
    }
}
```
#### <a name="v2-response"></a>[Odpověď v2](#tab/V2)

Následující příklad ukazuje řešení entity **Builtin. person** .

```json
"entities": [
{
    "entity": "Jill Jones",
    "type": "builtin.personName",
    "startIndex": 3,
    "endIndex": 12
}
]
```
* * *

## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [koncovém bodu předpovědi V3](luis-migration-api-v3.md).

Přečtěte si informace o [e-mailu](luis-reference-prebuilt-email.md), [číslu](luis-reference-prebuilt-number.md)a [řadových](luis-reference-prebuilt-ordinal.md) entitách.
