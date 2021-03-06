---
title: Překlad řeči pomocí služby Speech
titleSuffix: Azure Cognitive Services
description: Služba Speech umožňuje přidat do aplikací, nástrojů a zařízení komplexní překlady řeči v reálném čase na více jazyků. Stejné rozhraní API lze použít pro překlad řeči na řeč i řeči na text.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: erhopf
ms.openlocfilehash: f51288da6af3580ba7592950cde4f17d7adad529
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80052622"
---
# <a name="what-is-speech-translation"></a>Co je překlad řeči?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Překlad řeči ze služby Speech umožňuje v reálném čase využívat překlady zvukových streamů a převodů řeči na text v reálném čase. Pomocí sady Speech SDK mají vaše aplikace, nástroje a zařízení přístup ke zdroji přepisů a výstupům překladu pro daný zvuk. Průběžné výsledky přepisu a překladu se vrátí, protože se detekuje řeč a konečné výsledky je možné převést na syntetizované řeč.

Překladatelské modul společnosti Microsoft používá dva různé přístupy: statistická strojové překlady (SMT) a neuronové Machine Translation (NMT). SMT používá pokročilou statistickou analýzu k odhadu nejlepší možné překlady s ohledem na kontext několika slov. Pomocí NMT se sítě neuronové používají k zajištění přesnější a přirozeného akustického překladu pomocí úplného kontextu vět k překladu slov.

Dnes Microsoft používá NMT k překladu do nejoblíbenějších jazyků. Všechny [jazyky, které jsou k dispozici pro překlad řeči na řeč](language-support.md#speech-translation) , jsou napájeny pomocí NMT. Překlad řeči na text může v závislosti na páru jazyků používat SMT nebo NMT. Pokud je cílový jazyk podporován nástrojem NMT, úplný překlad je NMT. Pokud NMT cílový jazyk nepodporuje, je překlad hybridem NMT a SMT, a to pomocí angličtiny jako "Pivot" mezi dvěma jazyky.

## <a name="core-features"></a>Základní funkce

Tady jsou funkce, které jsou k dispozici prostřednictvím sady Speech SDK a rozhraní REST API:

| Případ použití | Sada SDK | REST |
|----------|-----|------|
| Překlad řeči na text s výsledky rozpoznávání. | Ano | Ne |
| Překlad řeči na řeč. | Ano | Ne |
| Výsledky dočasnáho rozpoznávání a překladu. | Ano | Ne |

## <a name="get-started-with-speech-translation"></a>Začínáme s překladem řeči

Nabízíme rychlé starty, které jsou navržené tak, aby běžely kód za méně než 10 minut. Tato tabulka obsahuje seznam rychlých startů překladu řeči uspořádaných podle jazyka.

| Rychlý start | Platforma | API – referenční informace |
|------------|----------|---------------|
| [C#, .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnetcore) | Windows | [Procházet](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnet) | Windows | [Procházet](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp) | Windows | [Procházet](https://aka.ms/csspeech/csharpref) |
| [C++](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-cpp&tabs=windows) | Windows | [Procházet](https://aka.ms/csspeech/cppref)|
| [Java](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-java&tabs=jre) | Windows, Linux, macOS | [Procházet](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Ukázka kódu

Vzorový kód pro sadu Speech SDK je k dispozici na GitHubu. Tyto ukázky se týkají běžných scénářů, jako je čtení zvuku ze souboru nebo datového proudu, nepřetržitého a jednorázového rozpoznávání/překladu a práce s vlastními modely.

* [Ukázky pro převod řeči na text a překlady (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Průvodci migrací

Pokud vaše aplikace, nástroje nebo produkty používají [Translator Speech API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/overview), vytvořili jsme příručky, které vám pomůžou s migrací na službu Speech.

* [Migrace z Translator Speech API do služby pro rozpoznávání řeči](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Referenční dokumenty

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [REST API: Převod řeči na text](rest-speech-to-text.md)
* [REST API: převod textu na řeč](rest-text-to-speech.md)
* [REST API: přepis a přizpůsobení Batch](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Další kroky

* [Získejte zdarma klíč předplatného služby Speech](get-started.md)
* [Získat sadu Speech SDK](speech-sdk.md)
