---
title: zahrnout soubor
description: zahrnout soubor
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 671d6a18ca4c6b9ae15453d23d8188fbb4f7d957
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369412"
---
Pomocí klientské knihovny pro vytváření Language Understanding (LUIS) pro .NET:

* Vytvoření aplikace
* Přidání záměrů, entit a příkladu projevy
* Přidání funkcí, jako je seznam frází
* Výuka a publikování aplikace

[Referenční dokumentace](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)  |  [Zdrojový kód knihovny](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Authoring)  |  [Vytváření balíčku (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/)  |  [Ukázky jazyka C#](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs)

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure – [Vytvořte si ho zdarma](https://azure.microsoft.com/free/) .
* Aktuální verze [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Jakmile budete mít předplatné Azure, vytvořte v Azure Portal [prostředek pro vytváření Language Understanding](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) , abyste získali svůj klíč a koncový bod. Počkejte na nasazení a klikněte na tlačítko **Přejít k prostředku** .
    * Pro připojení aplikace k Language Understanding vytváření obsahu budete potřebovat klíč a koncový bod z prostředku, který [vytvoříte](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) . Svůj klíč a koncový bod vložíte do níže uvedeného kódu později v rychlém startu. Službu můžete vyzkoušet pomocí bezplatné cenové úrovně ( `F0` ).

## <a name="setting-up"></a>Nastavení

### <a name="create-a-new-c-application"></a>Vytvoření nové aplikace v C#

Vytvořte novou aplikaci .NET Core v upřednostňovaném editoru nebo integrovaném vývojovém prostředí (IDE).

1. V okně konzoly (například cmd, PowerShell nebo bash) `new` vytvořte pomocí příkazu dotnet novou konzolovou aplikaci s názvem `language-understanding-quickstart` . Tento příkaz vytvoří jednoduchý projekt C# "Hello World" s jedním zdrojovým souborem: `Program.cs` .

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Změňte adresář na nově vytvořenou složku aplikace.

1. Aplikaci můžete vytvořit pomocí:

    ```dotnetcli
    dotnet build
    ```

    Výstup sestavení by neměl obsahovat žádná upozornění ani chyby.

    ```console
    ...
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ...
    ```


### <a name="install-the-sdk"></a>Instalace sady SDK

V adresáři aplikace nainstalujte Language Understanding (LUIS) pro vytváření klientských knihoven pro .NET pomocí následujícího příkazu:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring --version 3.0.0
```

Pokud používáte integrované vývojové prostředí (IDE) sady Visual Studio, je knihovna klienta k dispozici jako balíček NuGet ke stažení.


## <a name="object-model"></a>Objektový model

Klient pro vytváření Language Understanding (LUIS) je objekt [LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) , který se ověřuje v Azure, který obsahuje váš klíč pro vytváření obsahu.

Po vytvoření klienta použijte tohoto klienta k přístupu k funkcím, včetně:

* Aplikace – [vytvořit](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet), [Odstranit](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet), [publikovat](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet)
* Příklad projevy – [Přidání v dávce](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet), [odstranění podle ID](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.deleteasync?view=azure-dotnet)
* Funkce – Správa [seznamů frází](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.featuresextensions.addphraselistasync?view=azure-dotnet)
* Model – Správa [záměrů](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) a entit
* Vzor – Správa [vzorů](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.patternextensions?view=azure-dotnet)
* Výuka aplikace [train](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.trainversionasync?view=azure-dotnet) a dotazování na [stav školení](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet)
* [Verze](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.versionsextensions?view=azure-dotnet) – Správa pomocí klonování, exportu a odstranění


## <a name="code-examples"></a>Příklady kódu

Tyto fragmenty kódu ukazují, jak provést následující akce pomocí klientské knihovny pro vytváření Language Understanding (LUIS) pro .NET:

* [Vytvoření aplikace](#create-a-luis-app)
* [Přidání entit](#create-entities-for-the-app)
* [Přidání záměrů](#create-intent-for-the-app)
* [Přidat ukázkové promluvy](#add-example-utterance-to-intent)
* [Výuka aplikace](#train-the-app)
* [Publikování aplikace](#publish-a-language-understanding-app)

## <a name="add-the-dependencies"></a>Přidat závislosti

V adresáři projektu otevřete soubor *program.cs* v preferovaném editoru nebo integrovaném vývojovém prostředí (IDE). Nahraďte existující `using` kód následujícími `using` direktivami:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=Dependencies)]

## <a name="authenticate-the-client"></a>Ověření klienta

1. Vytvořte proměnnou pro uložení klíče pro vytváření a vytváření koncových bodů.

    [!code-csharp[Authorization to resource key](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=Variables)]

1. Vytvořte objekt [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.apikeyserviceclientcredentials?view=azure-dotnet) s klíčem a použijte ho u svého koncového bodu k vytvoření objektu [LUISAuthoringClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-dotnet) .

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>Vytvoření aplikace LUIS

1. Vytvořte aplikaci LUIS, která bude obsahovat pracovní záměry modelu NLP (přirozený jazyk Processing), entity a příklad projevy.

1. Vytvořte [ApplicationCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.applicationcreateobject?view=azure-dotnet). Název a jazyková verze jazyka jsou požadované vlastnosti.

1. Zavolejte metodu [Apps. AddAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.addasync?view=azure-dotnet) . Odpověď je ID aplikace.

    [!code-csharp[Create a LUIS app](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringCreateApplication)]

## <a name="create-intent-for-the-app"></a>Vytvořit záměr pro aplikaci
Primárním objektem v modelu aplikace LUIS je záměr. Záměr se zarovnává se seskupením _záměrů_uživatelů utterance. Uživatel může požádat o dotaz nebo vytvořit příkaz, který hledá konkrétní _zamýšlenou_ odpověď z robota (nebo jiné klientské aplikace). Příklady záměrů je vyúčtování letu, dotazování na počasí v cílovém městě a dotazování na kontaktní informace pro zákaznické služby.

Vytvořte [ModelCreateObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modelcreateobject?view=azure-dotnet) s názvem jedinečného záměru a potom předejte ID aplikace, ID verze a ModelCreateObject do metody [model. AddIntentAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addintentasync?view=azure-dotnet) . Odpověď je ID záměru.

[!code-csharp[Create intent](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringAddIntents)]

## <a name="create-entities-for-the-app"></a>Vytvoření entit pro aplikaci

I když se entity nevyžadují, najdete je ve většině aplikací. Entita extrahuje informace z utterance uživatele, která je nutná k zaúmyslnému záměru uživatele. Existuje několik typů [předem sestavených](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions.addprebuiltasync?view=azure-dotnet) a vlastních entit, z nichž každý má vlastní modely DTO (Data Transformation Object).  Mezi běžné předem připravené entity, které se mají přidat do vaší aplikace, patří [číslo](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md)a [Ordinal](../luis-reference-prebuilt-ordinal.md).

Tato metoda **AddEntities** vytvořila `Location` jednoduchou entitu se dvěma rolemi, `Class` jednoduchou entitou, `Flight` složenou entitou a přidává několik předem sestavených entit.

Je důležité, abyste věděli, že entity nejsou označené záměrem. Mohou a obvykle se vztahují na mnoho záměrů. Pouze ukázkový uživatel projevy je označen pro konkrétní, jednotlivý záměr.

Metody vytváření entit jsou součástí třídy [modelu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.modelextensions?view=azure-dotnet) . Každý typ entity má svůj vlastní model objektu pro transformaci dat (DTO), obvykle který obsahuje slovo `model` v oboru názvů [Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models?view=azure-dotnet) .

[!code-csharp[Create entities](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringAddEntities)]

## <a name="add-example-utterance-to-intent"></a>Přidat příklad utterance k záměru

Aby bylo možné určit záměr utterance a extrahovat entity, aplikace potřebuje příklady projevy. Příklady musí cílit na konkrétní, jediný záměr a označit všechny vlastní entity. Předem připravené entity není nutné označit.

Přidejte příklad projevy vytvořením seznamu objektů [ExampleLabelObject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-dotnet) , jeden objekt pro každý příklad utterance. Každý příklad by měl označit všechny entity pomocí slovníku dvojic název-hodnota a hodnota entity. Hodnota entity by měla být přesně tak, jak se zobrazuje v textu příkladu utterance.

Zavolejte [Examples.BatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.examplesextensions.batchasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ExamplesExtensions_BatchAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_IExamples_System_Guid_System_String_System_Collections_Generic_IList_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_Models_ExampleLabelObject__System_Threading_CancellationToken_) s ID aplikace, ID verze a seznamem příkladů. Volání odpoví seznamem výsledků. Je potřeba zkontrolovat výsledek každého příkladu, abyste se ujistili, že se úspěšně přidal do modelu.

[!code-csharp[Add example utterances to a specific intent](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringBatchAddUtterancesForIntent)]

Metody **CreateUtterance** a **CreateLabel** jsou pomocné metody, které vám pomohou vytvořit objekty.

## <a name="train-the-app"></a>Trénování aplikace

Po vytvoření modelu musí být aplikace LUIS vyškolená pro tuto verzi modelu. Vyškolený model se dá použít v [kontejneru](../luis-container-howto.md)nebo ho můžete [publikovat](../luis-how-to-publish-app.md) na pracovní nebo výrobní sloty.

Metoda [vlak. TrainVersionAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions?view=azure-dotnet) potřebuje ID aplikace a ID verze.

Velmi malý model, jako je například tento rychlý Start, bude vlak velmi rychlý. V případě aplikací na úrovni produkčního prostředí by mělo školení aplikace zahrnovat volání metody [GetStatusAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.trainextensions.getstatusasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_TrainExtensions_GetStatusAsync_Microsoft_Azure_CognitiveServices_Language_LUIS_Authoring_ITrain_System_Guid_System_String_System_Threading_CancellationToken_) , které určuje, kdy nebo jestli bylo školení úspěšné. Odpověď je seznam objektů [ModelTrainingInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-dotnet) s odděleným stavem pro každý objekt. Aby bylo školení považovat za dokončené, musí být všechny objekty úspěšné.

[!code-csharp[Train the app's version](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringTrainVersion)]

## <a name="publish-a-language-understanding-app"></a>Publikování aplikace Language Understanding

Publikujte aplikaci LUIS pomocí metody [PublishAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.publishasync?view=azure-dotnet) . Tím se publikuje aktuální vyškolená verze v zadaném slotu na koncovém bodu. Klientská aplikace používá tento koncový bod k odeslání projevy uživatele pro předpověď záměru a extrakci entit.

[!code-csharp[Create entities](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/authoring/authoring-with-sdk.cs?name=AuthoringPublishVersionAndSlot)]

## <a name="run-the-application"></a>Spuštění aplikace

Spusťte aplikaci pomocí `dotnet run` příkazu z adresáře aplikace.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete vyčistit, můžete aplikaci LUIS odstranit. Odstranění aplikace se provádí pomocí metody [Apps. DeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring.appsextensions.deleteasync?view=azure-dotnet) . Aplikaci můžete také odstranit z [portálu Luis](https://www.luis.ai).