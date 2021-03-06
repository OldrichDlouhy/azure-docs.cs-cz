---
title: 'Rychlý Start: syntetizace řeči, objektivní-C-Speech Service'
titleSuffix: Azure Cognitive Services
description: Naučte se, jak syntetizovat řeč v cíli – C v macOS pomocí sady Speech SDK
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: yulili
ms.openlocfilehash: 2c859965c951ad271b1b9e272ce60de64aa3d3d5
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391362"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Rychlý Start: syntetizace řeči v cíli – C v macOS pomocí sady Speech SDK

V tomto článku se dozvíte, jak vytvořit aplikaci macOS v cíli-C pomocí sady Cognitive Services Speech SDK pro syntetizaci řeči z textu a přehrání s výchozím zvukovým výstupem.

## <a name="prerequisites"></a>Požadavky

Než začnete, tady je seznam požadavků:

* [Klíč předplatného](~/articles/cognitive-services/Speech-Service/get-started.md) pro službu pro rozpoznávání řeči
* MacOS počítač s [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) nebo novějším a MacOS 10,13 nebo novějším

## <a name="get-the-speech-sdk-for-macos"></a>Získat sadu Speech SDK pro macOS

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Všimněte si, že tento kurz nebude fungovat s verzí sady SDK starší než 1.7.0.

Sada Cognitive Services Speech SDK for Mac je distribuována jako sada rozhraní.
Dá se použít v projektech Xcode jako [CocoaPod](https://cocoapods.org/), nebo stahovat z https://aka.ms/csspeech/macosbinary a propojit ručně. Tato příručka používá CocoaPod.

## <a name="create-an-xcode-project"></a>Vytvoření projektu Xcode

Spusťte Xcode a spusťte nový projekt kliknutím na **soubor**  >  **Nový**  >  **projekt**.
V dialogovém okně Výběr šablony vyberte šablonu aplikace pro kakao.

V následujících dialogových oknech proveďte následující výběry:

1. Dialogové okno Project Options (Možnosti projektu)
    1. Zadejte název aplikace Rychlý start, například `helloworld`.
    1. Zadejte název příslušné organizace a identifikátor organizace, pokud již máte účet Apple Developer. Pro účely testování stačí vybrat jakýkoli název, třeba `testorg`. K podepsání aplikace potřebujete správný zřizovací profil. Podrobnosti najdete na [webu pro vývojáře Apple](https://developer.apple.com/) .
    1. Ujistěte se, že jako jazyk projektu je zvolený jazyk Objective-C.
    1. Pokud chcete použít scénáře a vytvořit aplikaci založenou na dokumentu, zakažte zaškrtávací políčka. Jednoduché uživatelské rozhraní pro ukázkovou aplikaci se vytvoří programově.
    1. Zrušte zaškrtnutí všech políček týkajících se testů a základních dat.
    ![Nastavení projektu](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)
1. Vyberte adresář projektu.
    1. Vyberte adresář, do kterého se má projekt umístit. Tím se vytvoří `helloworld` adresář v domovském adresáři, který obsahuje všechny soubory projektu Xcode.
    1. Zakažte vytvoření úložiště Git pro tento ukázkový projekt.
1. Nastavte oprávnění pro přístup k síti. Kliknutím na název aplikace v prvním řádku v přehledu vlevo získáte konfiguraci aplikace a pak zvolíte kartu Možnosti.
    1. Povolte pro aplikaci nastavení "Sandbox aplikace".
    1. Zaškrtněte políčka pro přístup odchozí připojení.
    ![Nastavení izolovaného prostoru](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox-tts.png)
1. Zavřete projekt Xcode. Později po nastavení CocoaPods budete používat jinou instanci tohoto programu.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalace sady SDK jako CocoaPod

1. Nainstalujte správce závislostí CocoaPod, jak je popsáno v [pokynech k instalaci](https://guides.cocoapods.org/using/getting-started.html).
1. Přejděte do adresáře ukázkové aplikace ( `helloworld` ). Umístěte textový soubor s názvem `Podfile` a následujícím obsahem v tomto adresáři:  
   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/Podfile)]
1. Přejděte do `helloworld` adresáře v terminálu a spusťte příkaz `pod install` . Tím se vygeneruje `helloworld.xcworkspace` pracovní prostor Xcode obsahující ukázkovou aplikaci a sadu Speech SDK jako závislost. Tento pracovní prostor bude použit v následujících.

## <a name="add-the-sample-code"></a>Přidání vzorového kódu

1. Otevřete `helloworld.xcworkspace` pracovní prostor v Xcode.
1. Nahraďte obsah automaticky vygenerovaného souboru `AppDelegate.m` následujícím kódem:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/helloworld/AppDelegate.m#code)]
1. Řetězec `YourSubscriptionKey` nahraďte klíčem předplatného.
1. Řetězec `YourServiceRegion` nahraďte [oblastí](~/articles/cognitive-services/Speech-Service/regions.md) přidruženou k vašemu předplatnému (například `westus` pro bezplatnou zkušební verzi předplatného).

## <a name="build-and-run-the-sample"></a>Sestavení a spuštění ukázky

1. Nastavte výstup ladění jako viditelný (**Zobrazit**  >  **Debug Area**  >  **konzolu pro aktivaci**oblasti ladění).
1. Sestavte a spusťte ukázkový kód tak **Product**, že v nabídce vyberete  ->  **Spustit** produkt nebo kliknete na tlačítko **Přehrát** .
1. Po zadání nějakého textu a kliknutí na tlačítko v aplikaci byste měli slyšet syntetizované zvuky.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Zkoumání objektivů a ukázek v jazyce C na GitHubu](https://aka.ms/csspeech/samples)

