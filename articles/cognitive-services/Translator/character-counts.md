---
title: Počty znaků – Překladatel
titleSuffix: Azure Cognitive Services
description: Tento článek vysvětluje, jak Azure Cognitive Services Translator počítá znaky, abyste mohli porozumět tomu, jak ingestuje obsah.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 7fad90cf47591b5ffab4232c2be3180138738f01
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2020
ms.locfileid: "83992825"
---
# <a name="how-the-translator-counts-characters"></a>Způsob, jakým Překladatel počítá znaky

Překladatel počítá každý bod kódu Unicode vstupního textu jako znak. Každý překlad textu na jazyk se počítá jako samostatný překlad, i když se požadavek provedl v jediném volání rozhraní API, které se překládá do několika jazyků. Délka odpovědi nezáleží.

Jaké počty jsou:

* Text předaný překladateli v těle žádosti
   * `Text`Při použití metod překladu, přepisu a slovníku vyhledávání
   * `Text`a `Translation` při použití metody Dictionary Examples
* Všechny značky: HTML, XML tagy atd. v textovém poli textu žádosti. Zápis JSON použitý k vytvoření žádosti (například text:) se nepočítá.
* Jednotlivá písmena
* Interpunkční znaménka
* Mezera, tabulátor, značka a libovolný druh prázdného znaku
* Každý bod kódu definovaný v kódování Unicode
* Opakovaný překlad, i když jste dříve přeložili stejný text

Pro skripty založené na ideograms, jako je čínština a japonština kanji, služba Translator stále počítá počet kódových bodů Unicode, jeden znak na ideogram. Výjimka: náhrada Unicode se počítá jako dva znaky.

Počet požadavků, slov, bajtů nebo vět není v počtu znaků podstatný.

Volání metod detekce a BreakSentence se ve spotřebě znaků nezapočítávají. Očekáváme ale, že volání metod detekce a BreakSentence jsou úměrná k používání dalších funkcí, které se počítají. Pokud počet vydaných volání detekce nebo BreakSentence překročí počet dalších výpočetních metod o 100 krát, společnost Microsoft si vyhrazuje právo omezit použití metod detekce a BreakSentence.

Další informace o počtu znaků najdete v [nejčastějších dotazech k překladatelům](https://www.microsoft.com/en-us/translator/faq.aspx).
