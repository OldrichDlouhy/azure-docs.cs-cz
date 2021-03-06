---
title: Rychlý Start klientské knihovny Vyhledávání entit Bingu C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 39a6c21ad056980e8c7b146e36a6e185cb3ed95e
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "79136778"
---
V tomto rychlém startu můžete začít vyhledávat entity pomocí Vyhledávání entit Bingu klientské knihovny pro jazyk C#. I když Vyhledávání entit Bingu má REST API kompatibilní s většinou programovacích jazyků, Klientská knihovna poskytuje snadný způsob, jak integrovat službu do vašich aplikací. Zdrojový kód pro tuto ukázku najdete na [GitHubu](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch).


## <a name="prerequisites"></a>Požadavky

* Libovolná edice sady [Visual Studio 2017 nebo novější](https://www.visualstudio.com/downloads/).
* Rozhraní [Json.NET](https://www.newtonsoft.com/json), k dispozici jako balíček NuGet.
* Pokud používáte Linux nebo MacOS, je možné tuto aplikaci spustit pomocí [Mono](https://www.mono-project.com/).
* [Balíček NuGet sady vyhledávání zpráv Bingu SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Instalace tohoto balíčku také nainstaluje následující:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Chcete-li přidat knihovnu klienta Vyhledávání entit Bingu do projektu aplikace Visual Studio, použijte možnost **Spravovat balíčky NuGet** z **Průzkumník řešení**a přidejte `Microsoft.Azure.CognitiveServices.Search.EntitySearch` balíček.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Vytvoření a inicializace aplikace

1. Vytvořte nové řešení konzoly C# v aplikaci Visual Studio. Pak přidejte následující do hlavního souboru kódu.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>Vytvoření klienta a odeslání žádosti o vyhledávání

1. Vytvořit nového klienta vyhledávání. Přidejte svůj klíč předplatného tak, `ApiKeyServiceClientCredentials`že vytvoříte nový.

    ```csharp
    var client = new EntitySearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. K vyhledání dotazu použijte `Entities.Search()` funkci klienta:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Získání a tisk popisu entity

1. Pokud rozhraní API vrátilo výsledky hledání, Získejte hlavní entitu `entityData`z.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Tisk popisu hlavní entity 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření jednostránkové webové aplikace](../../tutorial-bing-entities-search-single-page-app.md)

* [Co je rozhraní API Bingu pro vyhledávání entit?](../../overview.md )