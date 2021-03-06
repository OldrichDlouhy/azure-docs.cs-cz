---
title: Koncepce streamování zvukového vstupu v sadě Speech SDK
titleSuffix: Azure Cognitive Services
description: Přehled možností rozhraní API pro zvukové vstupní Stream sady Speech SDK
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 23a426bf8cc3f30516fff0a672d7118a49666433
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584922"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>O rozhraní API pro audio input Stream v sadě Speech SDK

Rozhraní API pro **Audio Input Stream** v sadě Speech SDK poskytuje způsob, jak streamovat zvuk do rozpoznávání, místo abyste použili buď mikrofon, nebo vstupní soubor rozhraní API.

Při použití vstupních datových proudů je potřeba použít následující postup:

- Určete formát streamu zvuku. Formát musí být podporován sadou Speech SDK a službou Speech. V současné době je podporována pouze následující konfigurace:

  Ukázky zvuku ve formátu PCM, jeden kanál, 16 bitů na vzorek, 8000 nebo 16000 vzorků za sekundu (16000 nebo 32000 bajtů za sekundu), dvě zarovnávání bloků (16 bitů včetně odsazení vzorku).

  Odpovídající kód v sadě SDK pro vytvoření formátu zvuku vypadá takto:

  ```csharp
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000; // or 8000
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Ujistěte se, že váš kód může poskytovat nezpracovaná zvuková data podle těchto specifikací. Pokud vaše zdrojová data zvuku neodpovídají podporovaným formátům, musí být zvuk předaný do požadovaného formátu.

- Vytvořte si vlastní třídu vstupního streamu zvuku odvozenou z `PullAudioInputStreamCallback` . Implementujte členy `Read()` a `Close()`. Přesný podpis funkce je závislý na jazyku, ale kód bude vypadat podobně jako tento ukázka kódu:

  ```csharp
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public int Read(byte[] buffer, uint size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Vytvořte zvukovou konfiguraci na základě zvukového formátu a vstupního streamu. Při vytváření nástroje pro rozpoznávání můžete předávat standardní konfiguraci řeči i konfiguraci zvukového vstupu. Například:

  ```csharp
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Další kroky

- [Získání zkušebního předplatného služby Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Přečtěte si téma rozpoznávání řeči v jazyce C. #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
