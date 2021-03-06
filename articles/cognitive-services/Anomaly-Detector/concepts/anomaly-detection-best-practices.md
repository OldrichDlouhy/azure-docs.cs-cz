---
title: Osvědčené postupy při používání rozhraní API Detektoru anomálií
titleSuffix: Azure Cognitive Services
description: Seznamte se s osvědčenými postupy při detekci anomálií pomocí rozhraní API detektoru anomálií.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 9407f2fc9375765efb6eb9688b3ebfeef24ba90a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67721619"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Osvědčené postupy pro používání rozhraní API detektoru anomálií

Rozhraní API pro detekci anomálií je Bezstavová služba pro detekci anomálií. Přesnost a výkon jejich výsledků může mít vliv na:

* Jak se připravují data časové řady.
* Použité parametry rozhraní API detektoru anomálií.
* Počet datových bodů v žádosti rozhraní API. 

V tomto článku se seznámíte s osvědčenými postupy pro používání rozhraní API, které získává nejlepší výsledky pro vaše data. 

## <a name="when-to-use-batch-entire-or-latest-last-point-anomaly-detection"></a>Kdy použít detekci anomálií (celý) nebo nejnovější (poslední) bod dávky

Koncový bod rozhraní API pro detekci anomálií umožňuje detekovat anomálie prostřednictvím celých dat řady času. V tomto režimu zjišťování se vytvoří jeden statistický model a použije se na každý bod v sadě dat. Pokud vaše časová řada obsahuje níže uvedené charakteristiky, doporučujeme vám pomocí zjišťování služby Batch zobrazit náhled vašich dat v jednom volání rozhraní API.

* Sezónní časová řada s příležitostnými anomáliemi.
* Paušální časová řada trendů s příležitostnými špičkami a neshodou. 

Nedoporučujeme používat detekci anomálií služby Batch pro monitorování dat v reálném čase nebo ho používat v datech časových řad, která neobsahují výše uvedené charakteristiky. 

* Při detekci dávky se vytvoří a použije jenom jeden model. zjišťování jednotlivých bodů se provádí v kontextu celé řady. Pokud se data časové řady pohybují nahoru a dolů bez sezónnost, může model nějaký bod změny (DIP a špičky v datech) chybět. Podobně některé body změny, které jsou méně významné než v sadě dat, se nemusí počítat, protože jsou dostatečně významné pro začlenění do modelu.

* Zjišťování dávky je pomalejší než zjišťování stavu anomálií posledního bodu při monitorování dat v reálném čase, z důvodu počtu analyzovaných bodů.

Pro monitorování dat v reálném čase doporučujeme zjistit stav anomálií jenom pro poslední datový bod. Po neustálém použití nejnovější detekce bodů je možné monitorování streamování dat dělat efektivněji a přesně.

Následující příklad popisuje vliv těchto režimů detekce na výkon. První obrázek ukazuje výsledek nepřetržitého zjišťování stavu anomálií nejnovější bod v 28 dříve zjištěných datových bodech. Červené body jsou anomálie.

![Obrázek znázorňující detekci anomálií s využitím posledního bodu](../media/last.png)

Níže je stejná datová sada používající detekci anomálií v dávce. Model sestavený pro operaci ignoroval několik anomálií označených obdélníky.

![Obrázek znázorňující detekci anomálií pomocí metody Batch](../media/entire.png)

## <a name="data-preparation"></a>Příprava dat

Rozhraní API detektoru anomálií akceptuje data časové řady formátovaná do objektu žádosti JSON. Časová řada může být jakákoli číselná data zaznamenaná v průběhu času v sekvenčním pořadí. Můžete odesílat okna dat časových řad do koncového bodu rozhraní API detektoru anomálií, aby se zlepšil výkon rozhraní API. Minimální počet datových bodů, které můžete odeslat, je 12 a maximum je 8640 bodů. [Členitost](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) je definována jako sazba, na kterou jsou data Navzorkovaná. 

Datové body odesílané do rozhraní API detektoru anomálií musí mít platný koordinovaný světový čas (UTC) a číselnou hodnotu. 

```json
{
    "granularity": "daily",
    "series": [
      {
        "timestamp": "2018-03-01T00:00:00Z",
        "value": 32858923
      },
      {
        "timestamp": "2018-03-02T00:00:00Z",
        "value": 29615278
      },
    ]
}
```

Pokud jsou vaše data Navzorkovaná v nestandardním časovém intervalu, můžete ji zadat přidáním `customInterval` atributu do své žádosti. Například pokud je vaše série Vzorkovat každých 5 minut, můžete do žádosti JSON přidat následující:

```json
{
    "granularity" : "minutely", 
    "customInterval" : 5
}
```

### <a name="missing-data-points"></a>Chybějící datové body

Chybějící datové body jsou společné rovnoměrně distribuovaným datovým sadám časových řad, zejména s jemnou členitosti (malý interval vzorkování. Například data jsou ve vzorku každých několik minut. Chybějící méně než 10% očekávaného počtu bodů v datech by neměl mít negativní dopad na výsledky detekce. Zvažte vyplnění mezer ve vašich datech na základě jejich vlastností, jako je nahrazování datových bodů z dřívějšího období, lineární interpolace nebo klouzavý průměr.

### <a name="aggregate-distributed-data"></a>Agregovaná distribuovaná data

Rozhraní API pro detekci anomálií funguje nejlépe u rovnoměrně distribuovaných časových řad. Pokud se data náhodně distribuují, měli byste je agregovat podle jednotky času, například za minutu, každou hodinu nebo každý den.

## <a name="anomaly-detection-on-data-with-seasonal-patterns"></a>Detekce anomálií u dat pomocí sezónních vzorů

Pokud víte, že vaše data časové řady mají sezónní vzor (k tomu dochází v pravidelných intervalech), můžete zlepšit přesnost a dobu odezvy rozhraní API. 

Určení `period` při vytváření požadavku JSON může snížit latenci detekce anomálií až o 50%. `period` Je celé číslo, které určuje zhruba počet datových bodů, které časová řada potřebuje k opakování vzoru. Například časová řada s jedním datovým bodem za den bude `period` mít jako `7`a časová řada s jedním bodem za hodinu (se stejným týdenním vzorem) by `period` měla. `7*24` Pokud si nejste jisti vzorem vašich dat, nemusíte tento parametr zadávat.

Nejlepších výsledků dosáhnete, když `period`zadáte 4 pro datový bod a navíc ještě další. Například hodinová data s týdenním vzorem, jak je popsáno výše, by měla v textu žádosti (`7 * 24 * 4 + 1`) poskytnout 673 datových bodů.

### <a name="sampling-data-for-real-time-monitoring"></a>Vzorkování dat pro sledování v reálném čase

Pokud jsou vaše streamovaná data vzorkovat v krátkém intervalu (například sekund nebo minut), může odeslání doporučeného počtu datových bodů překročit maximální povolený počet (8640 datových bodů) rozhraní API detektoru anomálií. Pokud vaše data zobrazují stabilní sezónní vzor, zvažte odeslání ukázky dat časových řad v delším časovém intervalu, například hodiny. Vzorkování dat tímto způsobem může také znamenat zvýšení doby odezvy rozhraní API. 

## <a name="next-steps"></a>Další kroky

* [Co je rozhraní API Detektoru anomálií?](../overview.md)
* [Rychlý Start: zjištění anomálií v datech časových řad pomocí REST API detektoru anomálií](../quickstarts/detect-data-anomalies-csharp.md)
