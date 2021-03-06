---
title: Vytvořit klienta pro model nasazený jako webovou službu
titleSuffix: Azure Machine Learning
description: Naučte se volat koncový bod webové služby, který byl vygenerován při nasazení modelu z Azure Machine Learning. Koncový bod zpřístupňuje REST API, které můžete volat pro provedení odvození s modelem. Vytvořte klienty pro toto rozhraní API pomocí programovacího jazyka dle vašeho výběru.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 06/17/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: 991ad3afc51cc2f6dc1853a6b26f53bcb2fd1503
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87326405"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Využívání modelu služby Azure Machine Learning nasazeného jako webová služba
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Nasazení modelu Azure Machine Learning jako webové služby vytvoří koncový bod REST API. Do tohoto koncového bodu můžete posílat data a získat předpovědi vrácenou modelem. V tomto dokumentu se naučíte vytvářet klienty pro webovou službu pomocí jazyků C#, cestách, Java a Python.

Webovou službu vytvoříte při nasazení modelu do svého místního prostředí, Azure Container Instances, služby Azure Kubernetes nebo polí s programovatelnými poli brány (FPGA). Identifikátor URI, který se používá pro přístup k webové službě, načtete pomocí [sady Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). Pokud je povolené ověřování, můžete k získání ověřovacích klíčů nebo tokenů použít taky sadu SDK.

Obecný pracovní postup pro vytvoření klienta, který používá webovou službu Machine Learning, je:

1. K získání informací o připojení použijte sadu SDK.
1. Určete typ dat požadavku, který model používá.
1. Vytvořte aplikaci, která bude volat webovou službu.

> [!TIP]
> Příklady v tomto dokumentu jsou ručně vytvořeny bez použití specifikace OpenAPI (Swagger). Pokud jste pro nasazení povolili specifikaci OpenAPI, můžete k vytváření klientských knihoven pro vaši službu použít nástroje, jako je třeba [Swagger-CodeGen](https://github.com/swagger-api/swagger-codegen) .

## <a name="connection-information"></a>Informace o připojení

> [!NOTE]
> K získání informací o webové službě použijte sadu SDK Azure Machine Learning. Toto je sada Python SDK. Pro vytvoření klienta pro službu můžete použít libovolný jazyk.

Třída [AzureML. Core. WebService](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) poskytuje informace, které potřebujete k vytvoření klienta. Následující `Webservice` vlastnosti jsou užitečné při vytváření klientské aplikace:

* `auth_enabled`– Pokud je povolené ověřování klíčů, `True` ; jinak `False` .
* `token_auth_enabled`– Pokud je povolené ověřování tokenu, `True` ; jinak `False` .
* `scoring_uri`– Adresa REST API.
* `swagger_uri`– Adresa specifikace OpenAPI Tento identifikátor URI je k dispozici, pokud jste povolili automatické generování schématu. Další informace najdete v tématu [nasazení modelů pomocí Azure Machine Learning](how-to-deploy-and-where.md).

Existují tři způsoby, jak načíst tyto informace pro nasazené webové služby:

* Při nasazení modelu se `Webservice` vrátí objekt s informacemi o službě:

    ```python
    service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
    service.wait_for_deployment(show_output = True)
    print(service.scoring_uri)
    print(service.swagger_uri)
    ```

* Můžete použít `Webservice.list` k načtení seznamu nasazených webových služeb pro modely v pracovním prostoru. Chcete-li zúžit seznam vrácených informací, můžete přidat filtry. Další informace o tom, co je možné filtrovat, najdete v dokumentaci ke službě [WebService. list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) .

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    print(services[0].swagger_uri)
    ```

* Pokud znáte název nasazené služby, můžete vytvořit novou instanci a `Webservice` zadat název pracovního prostoru a služby jako parametry. Nový objekt obsahuje informace o nasazené službě.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    print(service.swagger_uri)
    ```

### <a name="secured-web-service"></a>Zabezpečená webová služba

Pokud jste nasadili nasazenou webovou službu pomocí certifikátu TLS/SSL, můžete se pomocí [protokolu HTTPS](https://en.wikipedia.org/wiki/HTTPS) připojit ke službě pomocí bodování nebo identifikátoru URI Swagger. Protokol HTTPS pomáhá zabezpečit komunikaci mezi klientem a webovou službou tím, že šifruje komunikaci mezi nimi. Šifrování používá [protokol TLS (Transport Layer Security)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Protokol TLS se někdy označuje jako *SSL (Secure Sockets Layer)* (SSL), což bylo předchůdce TLS.

> [!IMPORTANT]
> Webové služby nasazené pomocí Azure Machine Learning podporují pouze TLS verze 1,2. Při vytváření klientské aplikace se ujistěte, že tato verze podporuje.

Další informace najdete v tématu [použití protokolu TLS k zabezpečení webové služby prostřednictvím Azure Machine Learning](how-to-secure-web-service.md).

### <a name="authentication-for-services"></a>Ověřování pro služby

Azure Machine Learning poskytuje dva způsoby, jak řídit přístup k webovým službám.

|Metoda ověřování|ACI|AKS|
|---|---|---|
|Klíč|Zakázáno ve výchozím nastavení| Ve výchozím nastavení povolená|
|Token| Není k dispozici| Zakázáno ve výchozím nastavení |

Když posíláte požadavek službě, která je zabezpečená pomocí klíče nebo tokenu, použijte k předání klíče nebo tokenu __autorizační__ hlavičku. Klíč nebo token musí být formátován jako `Bearer <key-or-token>` , kde `<key-or-token>` je vaše hodnota klíče nebo tokenu.

#### <a name="authentication-with-keys"></a>Ověřování pomocí klíčů

Pokud povolíte ověřování pro nasazení, automaticky se vytvoří ověřovací klíče.

* Ověřování je ve výchozím nastavení povolené při nasazení do služby Azure Kubernetes.
* Ověřování je ve výchozím nastavení zakázáno při nasazení do Azure Container Instances.

Pro řízení ověřování použijte `auth_enabled` parametr při vytváření nebo aktualizaci nasazení.

Pokud je povoleno ověřování, můžete použít `get_keys` metodu k načtení primárního a sekundárního ověřovacího klíče:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Pokud potřebujete znovu vygenerovat klíč, použijte [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) .

#### <a name="authentication-with-tokens"></a>Ověřování pomocí tokenů

Pokud povolíte ověřování tokenu pro webovou službu, musí uživatel poskytnout webové službě Azure Machine Learning token JWT, aby k němu měl přístup. 

* Ověřování tokenu je ve výchozím nastavení zakázáno při nasazení do služby Azure Kubernetes.
* Ověřování tokenu není při nasazení do Azure Container Instances podporováno.

K řízení ověřování tokenu použijte `token_auth_enabled` parametr při vytváření nebo aktualizaci nasazení.

Pokud je povoleno ověřování tokenu, můžete použít `get_token` metodu k načtení nosného tokenu a jeho doby vypršení platnosti tokenu:

```python
token, refresh_by = service.get_token()
print(token)
```

Pokud máte rozhraní příkazového [řádku Azure CLI a rozšíření Machine Learning](reference-azure-machine-learning-cli.md), můžete k získání tokenu použít následující příkaz:

```azurecli
az ml service get-access-token -n <service-name>
```

> [!IMPORTANT]
> Momentálně jediným způsobem, jak token načíst, je použít Azure Machine Learning SDK nebo rozšíření Azure CLI Machine Learning.

Po čase tokenu budete muset požádat o nový token `refresh_by` . 

## <a name="request-data"></a>Data žádosti

REST API očekává, že tělo požadavku bude dokument JSON s následující strukturou:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> Struktura dat musí odpovídat skriptu bodování a modelu očekávanému v rámci služby. Skript bodování může data před předáním do modelu změnit.

Například model v [vlaku v rámci poznámkového bloku](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) očekává pole 10 čísel. Skript bodování pro tento příklad vytvoří pole numpy z požadavku a předá ho do modelu. Následující příklad ukazuje data, která tato služba očekává:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
```

Webová služba může v jednom požadavku přijmout více sad dat. Vrátí dokument JSON obsahující pole odpovědí.

### <a name="binary-data"></a>Binární data

Informace o tom, jak povolit podporu binárních dat ve službě, najdete v tématu [binární data](how-to-deploy-and-where.md#binary).

> [!TIP]
> Povolení podpory pro binární data se provádí v souboru score.py, který používá nasazený model. Z klienta nástroje použijte funkce HTTP vašeho programovacího jazyka. Například následující fragment kódu odesílá obsah souboru JPG do webové služby:
>
> ```python
> import requests
> # Load image data
> data = open('example.jpg', 'rb').read()
> # Post raw data to scoring URI
> res = request.post(url='<scoring-uri>', data=data, headers={'Content-Type': 'application/> octet-stream'})
> ```

### <a name="cross-origin-resource-sharing-cors"></a>Sdílení prostředků mezi zdroji (CORS)

Informace o povolení podpory CORS ve službě najdete v tématu [sdílení prostředků mezi zdroji](how-to-deploy-and-where.md#cors).

## <a name="call-the-service-c"></a>Volání služby (C#)

Tento příklad ukazuje, jak použít jazyk C# k volání webové služby vytvořené z [vlaku v rámci poznámkového bloku](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) :

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key or token
            string scoringUri = "<your web service URI>";
            string authKey = "<your key or token>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

Vrácené výsledky jsou podobné následujícímu dokumentu JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Volání služby (Přejít)

Tento příklad ukazuje, jak použít funkci přejít k volání webové služby vytvořené z [vlaku v rámci poznámkového bloku](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) :

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key or token (if any) for your service
var authKey string = "<your key or token>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

Vrácené výsledky jsou podobné následujícímu dokumentu JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Volání služby (Java)

Tento příklad ukazuje, jak použít jazyk Java k volání webové služby vytvořené z [vlaku v rámci poznámkového bloku](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) :

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key or token
        String key = "<your key or token>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

Vrácené výsledky jsou podobné následujícímu dokumentu JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Volání služby (Python)

Tento příklad ukazuje, jak použít Python pro volání webové služby vytvořené z [vlaku v rámci poznámkového bloku](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) :

```python
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key or token
key = '<your key or token>'

# Two sets of data to score, so we get two results back
data = {"data":
        [
            [
                0.0199132141783263,
                0.0506801187398187,
                0.104808689473925,
                0.0700725447072635,
                -0.0359677812752396,
                -0.0266789028311707,
                -0.0249926566315915,
                -0.00259226199818282,
                0.00371173823343597,
                0.0403433716478807
            ],
            [
                -0.0127796318808497,
                -0.044641636506989,
                0.0606183944448076,
                0.0528581912385822,
                0.0479653430750293,
                0.0293746718291555,
                -0.0176293810234174,
                0.0343088588777263,
                0.0702112981933102,
                0.00720651632920303]
        ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = {'Content-Type': 'application/json'}
# If authentication is enabled, set the authorization header
headers['Authorization'] = f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.text)
```

Vrácené výsledky jsou podobné následujícímu dokumentu JSON:

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="consume-the-service-from-power-bi"></a>Využívání služby v Power BI

Power BI podporuje využití webových služeb Azure Machine Learning k rozšíření dat v Power BI pomocí předpovědi. 

Pro vygenerování webové služby, která je podporována pro použití v Power BI, musí schéma podporovat formát vyžadovaný Power BI. [Naučte se vytvořit schéma podporované Power BI](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where#example-entry-script).

Po nasazení webové služby je tato služba příchodná z Power BIch toků dat. [Naučte se využívat Azure Machine Learning webové služby od Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="next-steps"></a>Další kroky

Pokud chcete zobrazit referenční architekturu pro bodování modelů Pythonu a hloubkového učení v reálném čase, jděte do [centra architektury Azure](/azure/architecture/reference-architectures/ai/realtime-scoring-python).
