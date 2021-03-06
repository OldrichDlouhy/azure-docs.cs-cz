---
title: Přizpůsobení překladu – Překladatel
titleSuffix: Azure Cognitive Services
description: Pomocí centra Microsoft Translator můžete vytvořit vlastní systém překladu počítačů s využitím preferované terminologie a stylu.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: swmachan
ms.openlocfilehash: 8d49d9b9d29116d95173c1daf5133622c3694de6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523991"
---
# <a name="customize-your-text-translations"></a>Přizpůsobení překladů textu

Vlastní Překladatel je funkcí služby Translator, která umožňuje uživatelům přizpůsobit pokročilý neuronové strojového překladu Microsoft translatoru při překladu textu pomocí překladatele (jenom verze 3).

Tuto funkci můžete také použít k přizpůsobení překladu řeči při použití s [Cognitive Services Speech](https://docs.microsoft.com/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Custom Translator

Pomocí vlastního překladatele můžete vytvářet neuronové překladatelské systémy, které rozumí terminologii používané ve vašem podniku a v průmyslu. Přizpůsobený systém překladu pak bude integrován do stávajících aplikací, pracovních postupů a webů.

### <a name="how-does-it-work"></a>Jak funguje?

Pomocí dříve přeložených dokumentů (letáků, webových stránek, dokumentace atd.) sestavíte systém překladu, který odráží vaši terminologii a styl specifický pro doménu, a lepší než standardní systém překladu. Uživatelé můžou ukládat dokumenty TMX, XLIFF, TXT, DOCX a XLSX.  

Systém také přijímá data, která jsou paralelní na úrovni dokumentu, ale ještě není zarovnána na úrovni věty. Pokud mají uživatelé přístup k verzím stejného obsahu v různých jazycích, ale v samostatných dokumentech vlastní Překladatel bude moci automaticky rozlišovat věty mezi dokumenty.  Systém může také použít monolingual data v jednom nebo obou jazycích k doplnění dat paralelního školení pro zlepšení překladu.

Přizpůsobený systém je pak k dispozici prostřednictvím pravidelného volání překladatele pomocí parametru Category.

Vzhledem k odpovídajícímu typu a množství školicích dat není běžné očekávat zisky mezi 5 a 10 nebo ještě více BLEUch bodů v kvalitě překladu pomocí vlastního překladatele.

Další podrobnosti o různých úrovních přizpůsobení na základě dostupných dat najdete v [uživatelské příručce pro vlastní překladatele](https://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Centrum Microsoft Translator

> [!NOTE]
> Starší verze centra Microsoft Translator se vyřadí do 17. května 2019. [Zobrazení důležitých informací a dat migrace](https://www.microsoft.com/translator/business/hub/).  

## <a name="custom-translator-versus-hub"></a>Vlastní Překladatel versus centrum

| Funkce | Rozbočovač | Custom Translator |
| ------- | :-: | :---------------: |
|Stav funkce přizpůsobení    | Obecná dostupnost    | Obecná dostupnost |
| Verze textového rozhraní API    | Pouze v2    | Jenom V3 |
| Přizpůsobení SMT    | Ano    | No |
| Přizpůsobení NMT    | No    | Ano |
| Nové přizpůsobení sjednocené služby pro rozpoznávání řeči    | No    | Ano |
| [Žádné trasování](https://www.aka.ms/notrace) | Ano    | Ano |

## <a name="collaborative-translations-framework"></a>Architektura pro spolupráci s překlady

> [!NOTE]
> Od 1. února 2018, AddTranslation () a AddTranslationArray () již nejsou k dispozici pro použití s překladačem v 2.0. Tyto metody selžou a nic se nebudou zapisovat. Překladatel v 3.0 tyto metody nepodporuje.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Nastavení přizpůsobeného jazykového systému pomocí vlastního překladatele](https://aka.ms/CustomTranslatorDocs)
