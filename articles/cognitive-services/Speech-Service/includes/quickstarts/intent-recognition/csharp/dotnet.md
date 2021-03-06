---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 142a78dbb994a28d267294ce3b3d86e32f52bb45
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298856"
---
## <a name="prerequisites"></a>Požadavky

Než začnete:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp" target="_blank">Nainstalujte sadu Speech SDK pro vývojové prostředí. ukázkový projekt <span class="docon docon-navigate-external x-hidden-focus"></span> pro vytvoření a vyprázdnění</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Vytvoření aplikace v LUIS pro rozpoznávání záměrů

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Otevřete projekt v aplikaci Visual Studio

Pak otevřete projekt v aplikaci Visual Studio.

1. Spusťte Visual Studio 2019.
2. Načtěte projekt a otevřete `Program.cs` .

## <a name="start-with-some-boilerplate-code"></a>Začínáme s některým často používaným kódem

Pojďme přidat kód, který funguje jako kostra pro náš projekt. Nezapomeňte, že jste vytvořili asynchronní metodu s názvem `RecognizeIntentAsync()` .
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-17,77-86)]

## <a name="create-a-speech-configuration"></a>Vytvoření konfigurace řeči

Předtím, než budete moci inicializovat `IntentRecognizer` objekt, je nutné vytvořit konfiguraci, která používá klíč a umístění prostředku předpovědi Luis.

> [!IMPORTANT]
> Spouštěcí klíč a klíč pro vytváření obsahu nebudou fungovat. Je nutné použít klíč předpovědi a umístění, které jste vytvořili dříve. Další informace najdete v tématu [Vytvoření aplikace Luis pro rozpoznávání záměrů](#create-a-luis-app-for-intent-recognition).

Vložte tento kód do `RecognizeIntentAsync()` metody. Ujistěte se, že tyto hodnoty aktualizujete:

* Nahraďte `"YourLanguageUnderstandingSubscriptionKey"` klíčem předpovědi Luis.
* Nahraďte `"YourLanguageUnderstandingServiceRegion"` umístěním Luis. Použijte **identifikátor oblasti** z [oblasti](https://aka.ms/speech/sdkregion).

>[!TIP]
> Pokud potřebujete nápovědu k nalezení těchto hodnot, přečtěte si téma [Vytvoření aplikace v Luis pro rozpoznávání záměrů](#create-a-luis-app-for-intent-recognition).

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=26)]

Tato ukázka používá `FromSubscription()` metodu pro sestavení `SpeechConfig` . Úplný seznam dostupných metod naleznete v tématu [Třída SpeechConfig](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

Sada Speech SDK bude standardně rozpoznána pomocí en-US pro daný jazyk. informace o výběru zdrojového jazyka najdete v tématu [určení zdrojového jazyka pro převod řeči na text](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Inicializovat IntentRecognizer

Teď vytvoříme `IntentRecognizer` . Tento objekt je vytvořen v rámci příkazu Using, aby bylo zajištěno správné vydání nespravovaných prostředků. Vložte tento kód do `RecognizeIntentAsync()` metody hned pod konfigurací řeči.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=29-30,76)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Přidat LanguageUnderstandingModel a záměry

Je potřeba přidružit k `LanguageUnderstandingModel` nástroji pro rozpoznávání záměrů a přidat záměry, které chcete rozpoznat. Budeme používat záměry z předem připravené domény pro automatizaci domů. Vložte tento kód do příkazu Using z předchozí části. Ujistěte se, že jste nahradili `"YourLanguageUnderstandingAppId"` ID aplikace Luis.

>[!TIP]
> Pokud potřebujete nápovědu najít tuto hodnotu, přečtěte si téma [Vytvoření aplikace v Luis pro rozpoznávání záměrů](#create-a-luis-app-for-intent-recognition).

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=33-35)]

Tento příklad používá `AddIntent()` funkci k samostatnému přidání záměrů. Pokud chcete přidat všechny záměry z modelu, použijte `AddAllIntents(model)` a předejte model. 

## <a name="recognize-an-intent"></a>Rozpoznávání záměru

Z `IntentRecognizer` objektu budete volat `RecognizeOnceAsync()` metodu. Tato metoda umožňuje službě rozpoznávání řeči zjistit, že posíláte jednoduchou frázi pro rozpoznávání, a že po identifikaci fráze zastavit rozpoznávání řeči.

V příkazu Using přidejte tento kód pod model.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=46)]

## <a name="display-recognition-results-or-errors"></a>Zobrazit výsledky rozpoznávání (nebo chyby)

Když Služba rozpoznávání řeči vrátí výsledek rozpoznávání, budete s ním chtít něco dělat. Nebudeme tak jednoduché a vytisknou výsledky do konzoly.

V příkazu Using níže `RecognizeOnceAsync()` přidejte tento kód:

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=49-75)]

## <a name="check-your-code"></a>Kontrolovat kód

V tomto okamžiku váš kód by měl vypadat takto:

> [!NOTE]
> Do této verze jsme přidali nějaké komentáře.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-86)]

## <a name="build-and-run-your-app"></a>Sestavení a spuštění aplikace

Nyní jste připraveni sestavit aplikaci a otestovat rozpoznávání řeči pomocí služby Speech.

1. **Zkompilujte kód** -z panelu nabídek v aplikaci Visual Studio, vyberte **sestavení**  >  **řešení**sestavení.
2. **Spusťte aplikaci** – z řádku nabídek zvolte **ladění**  >  **Spustit ladění** nebo stiskněte klávesu <kbd>F5</kbd>.
3. **Spustit rozpoznávání** – zobrazí výzvu k vymluvenému vynechání fráze v angličtině. Váš hlas se odešle službě Speech, přepisu jako text a vykreslí se v konzole nástroje.

## <a name="next-steps"></a>Další kroky

[!INCLUDE [footer](./footer.md)]
