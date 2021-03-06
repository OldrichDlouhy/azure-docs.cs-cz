---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: b6829f52cb8aaac6f243d3a4a1fa7f6bc1b150d7
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544990"
---
## <a name="prerequisites"></a>Předpoklady

Než začnete:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Vytvoření prostředku Azure Speech<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Nastavení vývojového prostředí a vytvoření prázdného projektu](../../../../quickstarts/setup-platform.md)

## <a name="create-a-new-website-folder"></a>Vytvoření složky nového webu

Vytvořte novou prázdnou složku. Pokud chcete ukázku hostovat na webovém serveru, ujistěte se, že má webový server k této složce přístup.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Rozbalení sady Speech SDK pro JavaScript do příslušné složky

Stáhněte si sadu Speech SDK jako [balíček .zip](https://aka.ms/csspeech/jsbrowserpackage) a rozbalte ho do nově vytvořené složky. Výsledkem je pět souborů, které jsou rozbaleny, včetně `microsoft.cognitiveservices.speech.sdk.bundle.js` a `microsoft.cognitiveservices.speech.sdk.bundle.js.map` .
Druhý soubor je nepovinný a je vhodný pro ladění kódu SDK.

## <a name="create-an-indexhtml-page"></a>Vytvoření stránky index.html

Vytvořte ve složce nový soubor `index.html` a otevřete ho v textovém editoru.

1. Vytvořte následující kostru HTML:

 [!code-html [SampleCode](~/samples-cognitive-services-speech-sdk/quickstart/javascript/browser/index-translate-stt.html)]

## <a name="create-the-token-source-optional"></a>Vytvoření zdroje tokenu (volitelné)

Pokud chcete webovou stránku hostovat na webovém serveru, můžete pro ukázkovou aplikaci volitelně zadat zdroj tokenu.
Díky tomu váš klíč předplatného nikdy neopustí váš server a uživatelé budou moct využívat řečové funkce, aniž by sami museli zadávat jakýkoli autorizační kód.

Vytvořte nový soubor s názvem `token.php`. V tomto příkladu předpokládáme, že váš webový server podporuje skriptovací jazyk PHP s povoleným kudrlinkou. Zadejte následující kód:

```php
<?php
header('Access-Control-Allow-Origin: ' . $_SERVER['SERVER_NAME']);

// Replace with your own subscription key and service region (e.g., "westus").
$subscriptionKey = 'YourSubscriptionKey';
$region = 'YourServiceRegion';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://' . $region . '.api.cognitive.microsoft.com/sts/v1.0/issueToken');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{}');
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json', 'Ocp-Apim-Subscription-Key: ' . $subscriptionKey));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
echo curl_exec($ch);
?>
```

> [!NOTE]
> Autorizační tokeny mají pouze omezenou dobu života.
> Tento zjednodušený příklad neukazuje, jak autorizační tokeny automaticky aktualizovat. Jako uživatel můžete stránku ručně znovu načíst nebo aktualizovat stisknutím klávesy F5.

## <a name="build-and-run-the-sample-locally"></a>Místní sestavení a spuštění ukázky

Pokud chcete aplikaci spustit, dvakrát klikněte na soubor index.html nebo tento soubor otevřete v oblíbeném webovém prohlížeči. Zobrazí se jednoduché grafické rozhraní (GUI), které vám umožní zadat klíč předplatného [a text a aktivovat](../../../../regions.md) vstupní řeč.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Sestavení a spuštění ukázky pomocí webového serveru

Pokud chcete aplikaci spustit, otevřete oblíbený webový prohlížeč a najeďte ho na veřejnou adresu URL, na které hostuje složku, zadejte svou [oblast](../../../../regions.md)a spusťte text přepisu vstupního řeči. Pokud je tak nakonfigurovaná, aplikace získá token z vašeho zdroje tokenu.

## <a name="next-steps"></a>Další kroky

[!INCLUDE [footer](./footer.md)]