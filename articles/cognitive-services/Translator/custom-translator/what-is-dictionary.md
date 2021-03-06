---
title: Co je slovník? – Vlastní Překladatel
titleSuffix: Azure Cognitive Services
description: Slovník je zarovnaný dokument, který určuje seznam frází nebo vět (a jejich překlady), který má aplikace Microsoft Translator vždycky přeložit stejným způsobem. Slovníky se někdy také označují jako Glossaries nebo pojem základ.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 826da5c3754ad03ac1fb62288f0b03ee2353d1f3
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2020
ms.locfileid: "85962258"
---
# <a name="what-is-a-dictionary"></a>Co je slovník?

Slovník je zarovnaný pár dokumentů, které určují seznam frází nebo vět a jejich odpovídající překlady. Použijte slovník ve školicím programu, pokud chcete, aby aplikace Translator vždy přeložila všechny instance zdrojové fráze nebo věty pomocí překladu, který jste zadali ve slovníku. Slovníky jsou někdy označovány jako Glossaries nebo pojem základ. Slovník si můžete představit jako hrubou silou "zkopírovat a nahradit" pro všechny uvedené výrazy. Vlastní funkce překladatele navíc sestaví a využívá vlastní slovníky pro obecné účely ke zlepšení kvality jeho překladu. Slovník poskytnutý zákazníkem má ale stejný předchůdce a bude prohledán jako první hledaná slova nebo věty.

Slovníky fungují jenom pro projekty ve dvojicích jazyků, které mají plně podporovaný model Microsoft General neuronové Network. [Zobrazte úplný seznam jazyků](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="phrase-dictionary"></a>Slovník frází
Když zahrnete Frázový slovník do školicího modelu, všechna slova nebo fráze uvedená v seznamu se budou přeložit způsobem, který jste zadali. Zbytek věty je přeložen běžným způsobem. Pomocí slovníku frází můžete určit fráze, které by se neměly překládat, zadáním stejné nepřeložené fráze ve zdrojovém a cílovém souboru ve slovníku.

## <a name="sentence-dictionary"></a>Slovník vět
Slovník vět umožňuje zadat přesný cílový překlad pro zdrojovou větu. Aby došlo ke shodě slovníku vět, musí celá odeslaná věta odpovídat zdrojové položce slovníku.  Pokud se shoduje jenom část věty, položka se neshoduje.  Při zjištění shody se vrátí cílová položka slovníku vět.

## <a name="dictionary-only-trainings"></a>Jenom kurzy pro slovník
Model můžete vytvořit pouze pomocí dat ze slovníku. Pokud to chcete provést, vyberte jenom dokument slovníku (nebo několik dokumentů slovníku), který chcete zahrnout, a klepněte na vytvořit model. Vzhledem k tomu, že se jedná o školení jenom pro slovník, není potřeba žádný minimální počet vět pro školení. Vaše modely obvykle dokončí školení mnohem rychleji než standardní školení.  Výsledné modely budou používat základní modely Microsoft pro překlad s přidáním slovníků, které jste přidali.  Nedostanete se k testovací sestavě.

>[!Note]
>Vlastní Překladatel nemění větu soubory slovníku, takže je důležité, aby se v dokumentech slovníku rovnal stejný počet zdrojových a cílových frází a vět a aby byly přesně zarovnané.

## <a name="recommendations"></a>Doporučení

- Slovníky nejsou náhradou za školení modelu pomocí školicích dat. Doporučuje se, abyste se vyhnuli a aby se systém dozvěděl od vašich školicích dat. Nicméně pokud je nutné vykreslit věty nebo složená podstatná jména, použijte slovník.
- Slovník frází by měl být používán zřídka. Počítejte s tím, že při nahrazení fráze ve větě je kontext v této větě ztracený nebo omezený pro překlad zbytku věty. Výsledkem je, že zatímco fráze nebo slovo v rámci věty budou přeloženy podle poskytnutého slovníku, bude celková kvalita překladu věty často zadarmo.
- Slovník frází dobře funguje pro složená podstatná jména, jako jsou názvy produktů ("Microsoft SQL Server"), správné názvy ("město Hamburg") nebo funkce produktu ("kontingenční tabulka"). Nefunguje stejně dobře pro příkazy nebo přídavné jména, protože jsou obvykle vysoce inflected ve zdroji nebo v cílovém jazyce. Osvědčenými postupy je vyhnout se záznamům slovníku frází pro cokoli, ale pro složená podstatná jména.
- Při použití slovníku frází jsou důležité kapitalizace a interpunkční znaménka. Položky slovníku budou odpovídat pouze slovům a frázím ve vstupní větě, které používají přesně stejnou velikost písmen a interpunkční znaménka, jak je uvedeno v souboru zdrojového slovníku. Také překlady budou odrážet velká a malá písmena, která jsou uvedena v souboru cílového slovníku. Například pokud jste si vyškole systém z angličtiny do španělštiny, který používá slovník frází, který určuje "US" ve zdrojovém souboru a "EE". UU. " v cílovém souboru. Když vyžádáte překlad věty, která obsahuje slovo "US" (bez velkých písmen), neshoduje se s slovníkem. Nicméně pokud požadujete překlad věty, která obsahuje slovo "US" (velkými písmeny), bude odpovídat slovníku a překlad by obsahoval "EE". UU. " Všimněte si, že velká a interpunkční znaménka v překladu se mohou lišit od zadání v souboru cílového slovníku a může se lišit od velkých a malých písmen ve zdroji. Řídí se pravidly cílového jazyka.
- Pokud se slovo v souboru slovníku vyskytuje více než jednou, bude systém vždycky používat poslední poskytnutou položku. Proto by váš slovník neměl obsahovat více překladů stejného slova.

## <a name="next-steps"></a>Další kroky

- Přečtěte si o [pokynech pro formáty dokumentů](document-formats-naming-convention.md).
