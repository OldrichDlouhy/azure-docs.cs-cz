---
title: Vzor. libovolný typ entity – LUIS
titleSuffix: Azure Cognitive Services
description: Pattern. any je zástupný symbol s proměnlivou délkou, který se používá jenom v šabloně vzoru utterance k označení, kde začíná a končí entita.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 5164bf55ef8233cf34a470524da3bc852678d79a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "75979165"
---
# <a name="patternany-entity"></a>Entita Pattern.any

Pattern. any je zástupný symbol s proměnlivou délkou, který se používá jenom v šabloně vzoru utterance k označení, kde začíná a končí entita.  

Vzor. všechny entity musí být označeny v příkladech šablony [vzoru](luis-how-to-model-intent-pattern.md) , nikoli v příkladech uživatele záměr.

**Entita je vhodná v případě, že:**

* Koncová entita může být zaměněna se zbývajícím textem utterance.

## <a name="usage"></a>Využití

Pro klientskou aplikaci, která vyhledává knihy na základě názvu, vzoru. vše extrahuje úplný název. Šablona utterance pomocí vzoru. Any pro toto hledání v knihách `Was {BookTitle} written by an American this year[?]`je.

V následující tabulce má každý řádek dvě verze utterance. Horní utterance je to, jak LUIS zpočátku vidí utterance. Není jasné, kde začíná a končí nadpis knihy. Dolní utterance používá vzor. kterákoli entita označuje začátek a konec entity.

|Utterance s entitou tučně|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>Jednalo **se o člověka, který v tomto roce nedokázal své manželky na Hat a další klinické úkony** zapsané americkým?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>Byl tento rok v roce Pajamas zapsaný na **polovinu režimu spánku** ?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>Jednalo **se o konkrétní smuteký dort: nové** zapsané do USA tento rok?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>**Ve svém kapesním počítači je Wocket!** Napsali jste tento rok American?|
||



## <a name="example-json"></a>Ukázkový kód JSON

Zamyslete se nad následujícím dotazem:

`where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

S názvem vloženého formuláře, který se má extrahovat jako vzor. any:

`Understand your responsibilities as a member of the community`

#### <a name="v2-prediction-endpoint-response"></a>[Předpověď odezvy koncového bodu v2](#tab/V2)

```JSON
"entities": [
  {
    "entity": "understand your responsibilities as a member of the community",
    "type": "FormName",
    "startIndex": 18,
    "endIndex": 78,
    "role": ""
  }
```


#### <a name="v3-prediction-endpoint-response"></a>[Prediktivní odezva koncového bodu V3](#tab/V3)

Toto je kód JSON, `verbose=false` Pokud je nastaven v řetězci dotazu:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ]
}
```

Toto je kód JSON, `verbose=true` Pokud je nastaven v řetězci dotazu:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ],
    "$instance": {
        "FormName": [
            {
                "type": "FormName",
                "text": "Understand your responsibilities as a member of the community",
                "startIndex": 18,
                "length": 61,
                "modelTypeId": 7,
                "modelType": "Pattern.Any Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

## <a name="next-steps"></a>Další kroky

V tomto [kurzu](luis-tutorial-pattern.md)použijte **vzor. libovolnou** entitu pro extrahování dat z projevy, kde jsou projevy ve správném formátu a kde se konce dat můžou snadno zaměňovat se zbývajícími slovy utterance.
