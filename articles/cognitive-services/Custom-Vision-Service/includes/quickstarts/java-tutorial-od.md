---
author: areddish
ms.custom: devx-track-java
ms.author: areddish
ms.service: cognitive-services
ms.date: 04/14/2020
ms.openlocfilehash: 383df0d9f3c8fef01d5185be1cf69fe203ba11a2
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87375334"
---
V tomto článku se dozvíte, jak začít používat sadu Custom Vision SDK s jazykem Java k sestavení modelu detekce objektu. Po vytvoření můžete přidat tagované oblasti, nahrát obrázky, naučit projekt, získat výchozí adresu URL koncového bodu předpovědi projektu a použít koncový bod pro programové testování obrázku. Tento příklad použijte jako šablonu pro vytvoření vlastní aplikace v Javě.

## <a name="prerequisites"></a>Požadavky

- Libovolné prostředí Java IDE
- Nainstalovaná sada [JDK 7 nebo 8](https://aka.ms/azure-jdks)
- [Maven](https://maven.apache.org/) nainstalované
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Získat Custom Vision SDK a ukázkový kód

K napsání aplikace v Javě, která využívá službu Custom Vision, budete potřebovat balíčky maven pro službu Custom Vision. Tyto balíčky jsou součástí ukázkového projektu, který budete stahovat, ale můžete k nim přistupovat jednotlivě.

Custom Vision SDK můžete najít v centrálním úložišti Maven:
- [Sada SDK pro trénování](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Sada SDK pro předpověď](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Naklonujte nebo si stáhněte projekt [Ukázky pro Cognitive Services v sadě Java SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Přejděte do složky **Vision/CustomVision/**.

Tento projekt Javy vytvoří nový projekt detekce objektů pomocí služby Custom Vision s názvem __Sample Java OD Project__, který bude přístupný na [webu služby Custom Vision](https://customvision.ai/). Potom nahraje obrázky k trénování a testování klasifikátoru. V tomto projektu Klasifikátor je určen k určení, zda je objekt **rozvětvení** nebo **nůžky**.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

Program je nakonfigurován tak, aby odkazoval na data klíče jako proměnné prostředí. Přejděte do složky **Vision/CustomVision** a zadejte následující příkazy PowerShellu pro nastavení proměnných prostředí. 

> [!NOTE]
> Pokud používáte jiný operační systém než Windows, přečtěte si téma [konfigurace proměnných prostředí](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) pro pokyny.

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Vysvětlení kódu

Ve svém prostředí Java IDE načtěte projekt `Vision/CustomVision` a otevřete soubor _CustomVisionSamples.java_. Vyhledejte metodu **runSample** a odkomentujte **ImageClassification_Sample** metoda volání &mdash; této metody provádí scénář klasifikace obrázku, který není popsaný v této příručce. Metoda **ObjectDetection_Sample** implementuje primární funkce tohoto rychlého startu. Přejděte k její definici a prozkoumejte kód. 

### <a name="create-a-new-custom-vision-service-project"></a>Vytvoření nového projektu Custom Vision Service

Přejděte k bloku kódu, který vytvoří klienta trénování a projekt detekce objektů. Vytvořený projekt se zobrazí na [webu služby Custom Vision](https://customvision.ai/), který jste navštívili dříve. Pokud vytvoříte projekt (vysvětlení najdete v průvodci [vytvořením webového portálu detektoru](../../get-started-build-detector.md) ), podívejte se na přetížení metod [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) a určete další možnosti.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

### <a name="add-tags-to-your-project"></a>Přidání značek do projektu

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

### <a name="upload-and-tag-images"></a>Nahrávání a označování obrázků

Když označíte obrázky v projektech detekce objektů, je nutné zadat oblast každého tagovaného objektu pomocí normalizovaných souřadnic. Přejděte k definici mapy `regionMap`. Tento kód přidruží jednotlivé ukázkové obrázky k odpovídající označené oblasti.

> [!NOTE]
> Pokud nemáte k označení souřadnic oblastí k dispozici nástroj pro kliknutí a přetažení, můžete použít webové uživatelské rozhraní na adrese [Customvision.AI](https://www.customvision.ai/). V tomto příkladu jsou souřadnice již poskytovány.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

Potom přeskočte k bloku kódu, který přidá obrázky do projektu. Obrázky se načtou ze složky **src/main/resources** projektu a s odpovídajícími značkami a souřadnicemi oblastí se nahrají do služby.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

Předchozí fragment kódu dělá použití dvou pomocných funkcí, které načítají obrázky jako proudy prostředků a odesílají je do služby (do jedné dávky můžete nahrát až 64 imagí).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-project-and-publish"></a>Výuka projektu a publikování

Tento kód vytvoří první iteraci modelu předpovědi a pak tuto iteraci publikuje do koncového bodu předpovědi. Název zadaný pro publikovanou iteraci lze použít k odeslání požadavků předpovědi. Iterace není v koncovém bodu předpovědi k dispozici, dokud není publikována.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

### <a name="use-the-prediction-endpoint"></a>Použití koncového bodu předpovědi

Koncový bod předpovědi, který zde představuje objekt `predictor`, je odkaz, pomocí kterého můžete odeslat obrázek do aktuálního modelu a získat předpověď klasifikace. V této ukázce se `predictor` definuje někde jinde s využitím proměnné prostředí s klíčem předpovědi.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>Spuštění aplikace

Pro zkompilování a spuštění řešení pomocí Maven přejděte do adresáře projektu (**Vision/CustomVision**) na příkazovém řádku a spusťte příkaz run:

```bash
mvn compile exec:java
```

Ve výstupu konzoly zobrazte výsledky protokolování a předpovědi. Pak můžete ověřit správné označení testovacího obrázku a správnost oblasti detekce.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Další kroky

Nyní jste viděli, jak se každý krok procesu detekce objektů dá provést v kódu. Tato ukázka provádí jednu výukovou iteraci, ale často je potřeba, abyste model provedli a otestovali několikrát, aby bylo přesnější. Následující průvodce školením se zabývá klasifikací imagí, ale jeho princip se podobá detekci objektů.

> [!div class="nextstepaction"]
> [Testování a přetrénování modelu](../../test-your-model.md)
