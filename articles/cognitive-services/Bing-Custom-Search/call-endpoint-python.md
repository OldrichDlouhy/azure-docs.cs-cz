---
title: 'Rychlý Start: volání koncového bodu Vlastní vyhledávání Bingu pomocí Pythonu | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Pomocí tohoto rychlého startu můžete začít požadovat výsledky hledání z vaší instance Vlastní vyhledávání Bingu pomocí Pythonu.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: b5a39951301256894553a036786ddd2fff251109
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/09/2020
ms.locfileid: "84605999"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Rychlý Start: volání koncového bodu Vlastní vyhledávání Bingu pomocí Pythonu

V tomto rychlém startu se dozvíte, jak vyžádat výsledky hledání z vaší instance Vlastní vyhledávání Bingu. I když je tato aplikace napsaná v Pythonu, rozhraní API pro vlastní vyhledávání Bingu je webová služba RESTful kompatibilní s většinou programovacích jazyků. Zdrojový kód pro tuto ukázku je k dispozici na [GitHubu](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Požadavky

- Instance Vlastní vyhledávání Bingu. Další informace najdete v tématu [rychlý Start: Vytvoření první instance vlastní vyhledávání Bingu](quick-start.md).
- [Python](https://www.python.org/) 2. x nebo 3. x.

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Vytvoření a inicializace aplikace

- Vytvořte nový soubor Pythonu v oblíbeném prostředí IDE nebo editoru a přidejte následující příkazy importu. Vytvořte proměnné pro svůj klíč předplatného, ID vlastní konfigurace a hledaný termín.

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Odeslání a přijetí žádosti o vyhledávání 

1. Vytvořte adresu URL žádosti připojením hledaného výrazu k `q=` parametru dotazu a ID vlastní konfigurace vaší instance hledání k `customconfig=` parametru. Parametry oddělte pomocí ampersandu ( `&` ). Můžete použít globální koncový bod v následujícím kódu nebo použít vlastní koncový bod [subdomény](../../cognitive-services/cognitive-services-custom-subdomains.md) zobrazený v Azure Portal pro váš prostředek.

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Odešlete požadavek do vaší instance Vlastní vyhledávání Bingu a vytiskněte vrácené výsledky hledání.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření vlastní vyhledávací webové aplikace](./tutorials/custom-search-web-page.md)
