---
title: Vytvoření první funkce v Azure pomocí Kotlin a Maven
description: Vytvoření a publikování funkce aktivované protokolem HTTP do Azure pomocí Kotlin a Maven.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.openlocfilehash: d8abf6cdf8506dc491f4e026c9a61ac1391f6ea4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86506294"
---
# <a name="quickstart-create-your-first-function-with-kotlin-and-maven"></a>Rychlý Start: Vytvoření první funkce pomocí Kotlin a Maven

Tento článek vás provede použitím nástroje příkazového řádku Maven k vytvoření a publikování projektu funkce Kotlin pro Azure Functions. Až budete hotovi, kód funkce poběží v [plánu Consumption](functions-scale.md#consumption-plan) v Azure a je možné ho aktivovat prostřednictvím požadavku HTTP.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Předpoklady

K vývoji funkcí pomocí Kotlin musíte mít nainstalované následující:

- [Java Developer Kit](https://aka.ms/azure-jdks) verze 8
- [Apache Maven](https://maven.apache.org) verze 3.0 nebo novější
- [Azure CLI](/cli/azure)
- [Azure Functions Core Tools](./functions-run-local.md#v2) verze 2.6.666 nebo vyšší

> [!IMPORTANT]
> Pro dokončení tohoto rychlého startu musí být proměnná prostředí JAVA_HOME nastavená na umístění instalace sady JDK.

## <a name="generate-a-new-functions-project"></a>Vygenerování nového projektu Functions

Spuštěním následujícího příkazu v prázdné složce vygenerujte projekt Functions z [archetypu Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

# <a name="bash"></a>[bash](#tab/bash)
```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

> [!NOTE]
> Pokud máte problémy se spuštěním příkazu, Prohlédněte si, jaká `maven-archetype-plugin` verze se používá. Vzhledem k tomu, že příkaz spouštíte v prázdném adresáři bez `.pom` souboru, může se pokusit použít modul plug-in starší verze z, `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` Pokud jste Maven upgradovali ze starší verze. Pokud ano, zkuste odstranit `maven-archetype-plugin` adresář a znovu spustit příkaz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
mvn archetype:generate `
    "-DarchetypeGroupId=com.microsoft.azure" `
    "-DarchetypeArtifactId=azure-functions-kotlin-archetype"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```
---

Maven vás vyzve k zadání hodnot potřebných k dokončení generování projektu. Informace o hodnotách _groupId_, _artifactId_ a _version_ najdete v referenčních informacích k [zásadám vytváření názvů pro Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). Hodnota _appName_ musí být v rámci Azure jedinečná, takže Maven ve výchozím nastavení vygeneruje název aplikace na základě dříve zadané hodnoty _artifactId_. Hodnota vlastnosti _balíèku_ určuje balíček Kotlin pro vygenerovaný kód funkce.

Níže uvedené identifikátory `com.fabrikam.functions` a `fabrikam-functions` slouží jako příklad a k zpřehlednění pozdějších kroků v tomto rychlém startu. Doporučujeme, abyste do Maven v tomto kroku zadali vlastní hodnoty.

<pre>
[INFO] Parameter: groupId, Value: com.fabrikam.function
[INFO] Parameter: artifactId, Value: fabrikam-function
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.fabrikam.function
[INFO] Parameter: packageInPathFormat, Value: com/fabrikam/function
[INFO] Parameter: appName, Value: fabrikam-function-20190524171507291
[INFO] Parameter: resourceGroup, Value: java-functions-group
[INFO] Parameter: package, Value: com.fabrikam.function
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: groupId, Value: com.fabrikam.function
[INFO] Parameter: appRegion, Value: westus
[INFO] Parameter: artifactId, Value: fabrikam-function
</pre>

Maven vytvoří soubory projektu, v tomto příkladu `fabrikam-functions`, v nové složce _artifactId_. Vygenerovaný kód připravený k použití v projektu je jednoduchá funkce [aktivovaná protokolem HTTP](./functions-bindings-http-webhook.md), která vypisuje text žádosti:

```kotlin
class Function {

    /**
     * This function listens at endpoint "/api/HttpTrigger-Java". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/HttpTrigger-Java&code={your function key}
     * 2. curl "{your host}/api/HttpTrigger-Java?name=HTTP%20Query&code={your function key}"
     * Function Key is not needed when running locally, it is used to invoke function deployed to Azure.
     * More details: https://aka.ms/functions_authorization_keys
     */
    @FunctionName("HttpTrigger-Java")
    fun run(
            @HttpTrigger(
                    name = "req",
                    methods = [HttpMethod.GET, HttpMethod.POST],
                    authLevel = AuthorizationLevel.FUNCTION) request: HttpRequestMessage<Optional<String>>,
            context: ExecutionContext): HttpResponseMessage {

        context.logger.info("HTTP trigger processed a ${request.httpMethod.name} request.")

        val query = request.queryParameters["name"]
        val name = request.body.orElse(query)

        name?.let {
            return request
                    .createResponseBuilder(HttpStatus.OK)
                    .body("Hello, $name!")
                    .build()
        }

        return request
                .createResponseBuilder(HttpStatus.BAD_REQUEST)
                .body("Please pass a name on the query string or in the request body")
                .build()
    }
}
```

## <a name="run-the-function-locally"></a>Místní spuštění funkce

Změňte adresář na složku nově vytvořeného projektu a sestavte a spusťte funkci pomocí Mavenu:

```
cd fabrikam-function
mvn clean package
mvn azure-functions:run
```

> [!NOTE]
> Pokud se zobrazuje tato výjimka `javax.xml.bind.JAXBException` pro Javu 9, najdete alternativní řešení na [GitHubu](https://github.com/jOOQ/jOOQ/issues/6477).

Když je funkce spuštěná místně ve vašem systému a připravená reagovat na požadavky HTTP, zobrazí se tento výstup:

<pre>
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpTrigger-Java: [GET,POST] http://localhost:7071/api/HttpTrigger-Java
</pre>

V novém okně terminálu aktivujte funkci z příkazového řádku pomocí příkazu curl:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/HttpTrigger-Java
```

<pre>
Hello LocalFunction!
</pre>

Pomocí klávesové zkratky `Ctrl-C` v terminálu zastavte kód aplikace.

## <a name="deploy-the-function-to-azure"></a>Nasazení funkce do Azure

V procesu nasazení do služby Azure Functions se používají přihlašovací údaje účtu z Azure CLI. Než budete pokračovat, [Přihlaste se pomocí Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) .

```azurecli
az login
```

Nasaďte svůj kód do nové aplikace funkcí s použitím cíle Maven `azure-functions:deploy`.

> [!NOTE]
> Když použijete Visual Studio Code k nasazení aplikace Function App, nezapomeňte zvolit předplatné bez bezplatného předplatného nebo se zobrazí chyba. Své předplatné můžete sledovat na levé straně rozhraní IDE.

```
mvn azure-functions:deploy
```

Po dokončení nasazení se zobrazí adresa URL, pomocí které můžete přistupovat k vaší aplikaci funkcí Azure:

<pre>
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
</pre>

Otestujte aplikaci funkcí spuštěnou v Azure pomocí `cURL`. V níže uvedené ukázce budete muset změnit adresu URL, aby odpovídala adrese URL nasazené vlastní aplikace funkcí z předchozího kroku.

> [!NOTE]
> Ujistěte se, že jste nastavili **přístupová práva** na `Anonymous` . Když zvolíte výchozí úroveň `Function` , budete muset [klíč funkce](functions-bindings-http-webhook-trigger.md#authorization-keys) prezentovat v žádosti o přístup ke koncovému bodu funkce.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/HttpTrigger-Java -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Provedení změn a opětovné nasazení

Abyste upravili text vrácený aplikací funkcí, upravte ve vygenerovaném projektu zdrojový soubor `src/main.../Function.java`. Změňte tento řádek:

```kotlin
return request
        .createResponseBuilder(HttpStatus.OK)
        .body("Hello, $name!")
        .build()
```

Na následující kód:

```kotlin
return request
        .createResponseBuilder(HttpStatus.OK)
        .body("Hi, $name!")
        .build()
```

Změny uložte a stejně jako předtím proveďte opětovné nasazení spuštěním příkazu `azure-functions:deploy`. Aplikace funkcí se aktualizuje a tento požadavek:

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

Zobrazí se aktualizovaný výstup:

<pre>
Hi, AzureFunctionsTest
</pre>


## <a name="reference-bindings"></a>Referenční vazby

Pokud chcete pracovat s [triggery funkcí a dalšími vazbami](functions-triggers-bindings.md) než Trigger http a Trigger časovače, musíte nainstalovat rozšíření vazby. I když tento článek nepožadujete, budete muset znát, jak povolit rozšíření při práci s jinými typy vazeb.

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste aplikaci funkcí Kotlin s jednoduchou triggerem HTTP a nasadili ji na Azure Functions.

- Další informace o vývoji funkcí Java a Kotlin najdete v [příručce pro vývojáře Java Functions](functions-reference-java.md) .
- Do svého projektu můžete přidat další funkce s jinými triggery s použitím cíle Maven `azure-functions:add`.
- Funkce můžete psát a ladit místně pomocí nástrojů [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) a [Eclipse](functions-create-maven-eclipse.md). 
- Funkce ladění můžete do Azure nasadit pomocí editoru Visual Studio Code. Pokyny najdete v dokumentaci editoru Visual Studio Code o [aplikacích bez serveru v Javě](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud).
