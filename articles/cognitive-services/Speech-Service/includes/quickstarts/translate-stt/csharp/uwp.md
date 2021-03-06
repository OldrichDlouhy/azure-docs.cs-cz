---
title: 'Rychlý Start: Převod řeči na text, C# (UWP) – služba Speech'
titleSuffix: Azure Cognitive Services
description: Bude doplněno
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.author: erhopf
ms.topic: include
ms.openlocfilehash: 62993b2e553630edd228228b4faa82de44997063
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "80671447"
---
## <a name="prerequisites"></a>Požadavky

Než začnete, nezapomeňte:

> [!div class="checklist"]
> * [Vytvoření prostředku Azure Speech](../../../../get-started.md)
> * [Nastavení vývojového prostředí a vytvoření prázdného projektu](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Přidání ukázkového kódu

Nyní přidejte kód jazyka XAML, který definuje uživatelské rozhraní aplikace, a přidejte implementaci C# kódu na pozadí.

1. V **Průzkumník řešení**otevřete `MainPage.xaml`.

1. V zobrazení jazyka XAML návrháře vložte následující fragment kódu XAML do značky **Grid** (mezi `<Grid>` a `</Grid>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. V **Průzkumník řešení**otevřete zdrojový soubor `MainPage.xaml.cs`kódu na pozadí. (Je seskupena pod `MainPage.xaml`.)

1. Nahraďte veškerý kód v něm následujícím fragmentem kódu:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. V `SpeechTranslationFromMicrophone_ButtonClicked` obslužné rutině v tomto souboru vyhledejte řetězec `YourSubscriptionKey`a nahraďte ho klíčem předplatného.

1. V `SpeechTranslationFromMicrophone_ButtonClicked` obslužné rutině Najděte řetězec `YourServiceRegion`a nahraďte ho [oblastí](~/articles/cognitive-services/Speech-Service/regions.md) , která je přidružená k vašemu předplatnému. (Například použijte `westus` pro předplatné bezplatné zkušební verze.)

1. V řádku **nabídek výběrem možnosti** > **Uložit vše** uložte změny.

## <a name="build-and-run-the-application"></a>Sestavení a spuštění aplikace

Teď jste připraveni sestavit a otestovat svoji aplikaci.

1. V řádku nabídek vyberte **sestavit** > **sestavení řešení** a sestavte aplikaci. Kód by se teď měl zkompilovat bez chyb.

1. Zvolte **ladění** > **Spustit ladění** (nebo stiskněte klávesu **F5**) a spusťte aplikaci. Zobrazí se okno **HelloWorld** .

   ![Ukázková aplikace překladu UWP v C# – rychlý Start](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. Vyberte možnost **Povolit mikrofon**a když se zobrazí žádost o přístupové oprávnění, vyberte **Ano**.

   ![Žádost o oprávnění k přístupu k mikrofonu](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Vyberte možnost **přeložit řeč ze vstupu mikrofonu**a mluvte do mikrofonu zařízení anglickou frázi nebo větu. Aplikace přenáší váš hlas do služby pro rozpoznávání řeči, která překládá řeč na text v jiném jazyce (v tomto případě je to němčina). Služba Speech odesílá přeložený text zpátky do aplikace, která zobrazuje překlad v okně.

   ![Uživatelské rozhraní překladu řeči](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Další kroky

[!INCLUDE [footer](./footer.md)]
