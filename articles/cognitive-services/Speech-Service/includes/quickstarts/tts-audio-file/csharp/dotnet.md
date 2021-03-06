---
title: 'Rychlý Start: syntetizace řeči do zvukového souboru, C# (.NET) – služba Speech'
titleSuffix: Azure Cognitive Services
description: Bude doplněno
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: erhopf
ms.openlocfilehash: d3d0a807e8742419ee321f38daebf6f70d6b0be1
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "81274883"
---
## <a name="prerequisites"></a>Požadavky

Než začnete, nezapomeňte:

> [!div class="checklist"]
> * [Vytvoření prostředku Azure Speech](../../../../get-started.md)
> * [Nastavení vývojového prostředí a vytvoření prázdného projektu](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)

## <a name="open-your-project-in-visual-studio"></a>Otevřete projekt v aplikaci Visual Studio

Prvním krokem je ujistit se, že máte projekt otevřený v aplikaci Visual Studio.

1. Spusťte Visual Studio 2019.
2. Načtěte projekt a otevřete `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Začínáme s některým často používaným kódem

Pojďme přidat kód, který funguje jako kostra pro náš projekt. Nezapomeňte, že jste vytvořili asynchronní metodu s názvem `SynthesisToAudioFileAsync()`.

````C#

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}

````

## <a name="create-a-speech-configuration"></a>Vytvoření konfigurace řeči

Než budete moct inicializovat `SpeechSynthesizer` objekt, musíte vytvořit konfiguraci, která používá váš klíč předplatného a oblast předplatného. Vložte tento kód do `SynthesisToAudioFileAsync()` metody.

````C#
// Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
// The default language is "en-us".
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
````

## <a name="create-an-audio-configuration"></a>Vytvoření konfigurace zvuku

Nyní je třeba vytvořit ````AudioConfig```` objekt, který odkazuje na váš zvukový soubor. Tento objekt je vytvořen v rámci příkazu Using, aby bylo zajištěno správné vydání nespravovaných prostředků. Vložte tento kód do `SynthesisToAudioFileAsync()` metody hned pod konfigurací řeči.

````C#
var fileName = "helloworld.wav";
using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
{
}
````

## <a name="initialize-a-speechsynthesizer"></a>Inicializovat SpeechSynthesizer

Nyní vytvoříme `SpeechSynthesizer` objekt pomocí dříve vytvořených objektů `SpeechConfig` a. `AudioConfig` Tento objekt je vytvořen také v rámci příkazu Using, aby bylo zajištěno správné vydání nespravovaných prostředků. Vložte tento kód do `SynthesisToAudioFileAsync()` metody uvnitř příkazu Using, který zabalí váš ````AudioConfig```` objekt.

````C#
using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
{
}
````

## <a name="synthesize-text-using-speaktextasync"></a>Vysyntetizení textu pomocí SpeakTextAsync

Z `SpeechSynthesizer` objektu budete volat `SpeakTextAsync()` metodu. Tato metoda pošle váš text službě rozpoznávání řeči, která ji převede na zvuk. `SpeechSynthesizer` Pokud `config.VoiceName` není explicitně zadaný, použije se výchozí hlas.

V příkazu Using přidejte tento kód:
````C#
var text = "Hello world!";
var result = await synthesizer.SpeakTextAsync(text);
````

## <a name="check-for-errors"></a>Vyhledat chyby

Když služba pro rozpoznávání řeči vrátí výsledek shrnutí, měli byste zkontrolovat, zda byl text úspěšně syntetizované.

V příkazu Using níže `SpeakTextAsync()`přidejte tento kód:
````C#
if (result.Reason == ResultReason.SynthesizingAudioCompleted)
{
    Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
}
else if (result.Reason == ResultReason.Canceled)
{
    var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

    if (cancellation.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }
}
````

## <a name="check-your-code"></a>Kontrolovat kód

V tomto okamžiku váš kód by měl vypadat takto:

````C#
//
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
//

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            var fileName = "helloworld.wav";
            using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
            {
                using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
                {
                    var text = "Hello world!";
                    var result = await synthesizer.SpeakTextAsync(text);

                    if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                    {
                        Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
                    }
                    else if (result.Reason == ResultReason.Canceled)
                    {
                        var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                    }
                }
            }
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}
````

## <a name="build-and-run-your-app"></a>Sestavení a spuštění aplikace

Nyní jste připraveni sestavit aplikaci a otestovat syntézu řeči pomocí služby Speech.

1. **Zkompilujte kód** -z panelu nabídek v aplikaci Visual Studio, vyberte **sestavení** > **řešení**sestavení.
2. **Spusťte aplikaci** – z řádku nabídek zvolte **ladění** > **Spustit ladění** nebo stiskněte klávesu **F5**.
3. **Zahájit syntézu** – váš text se převede na řeč a uloží se do zadaného zvukového data.

   ```text
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Viz také

- [Vytvoření vlastního hlasu](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Zaznamenat vlastní ukázky hlasu](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
