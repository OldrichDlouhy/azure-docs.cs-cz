---
title: Převod dat – LUIS
titleSuffix: Azure Cognitive Services
description: Přečtěte si, jak se dá projevy změnit před předpovědi v Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b2455df87c8eae1a48cb6c8b1381dad85d304bf4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82099236"
---
# <a name="convert-data-format-of-utterances"></a>Převést formát dat projevy
LUIS poskytuje následující převody uživatele utterance před předpovědi "

* Převod řeči na text pomocí služby [Cognitive Services Speech](../Speech-Service/overview.md)

## <a name="speech-to-text"></a>Řeč na text

Převod řeči na text je k dispozici jako integrace s LUIS.

### <a name="intent-conversion-concepts"></a>Koncepce převodu záměru
Převod řeči na text v LUIS umožňuje odeslat mluvené projevy na koncový bod a získat odpověď předpovědi LUIS. Proces je integrací služby pro [rozpoznávání řeči](https://docs.microsoft.com/azure/cognitive-services/Speech) pomocí Luis. Přečtěte si další informace o rozpoznávání řeči k záměru pomocí [kurzu](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Klíčové požadavky
Pro tuto integraci nemusíte vytvářet **rozhraní API pro zpracování řeči Bingu** klíč. **Language Understanding** klíč vytvořený v Azure Portal funguje pro tuto integraci. Nepoužívejte počáteční klíč LUIS.

### <a name="pricing-tier"></a>Cenová úroveň
Tato integrace používá jiný [cenový](luis-limits.md#key-limits) model než obvyklé Language Understanding cenové úrovně.

### <a name="quota-usage"></a>Využití kvóty
Informace najdete v tématu [omezení klíčů](luis-limits.md#key-limits) .

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Extrakce dat](luis-concept-data-extraction.md)

