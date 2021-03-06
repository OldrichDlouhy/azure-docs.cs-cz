---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 121f6ffa5c1a7c903e59be8a5bc3e1e1db0834fc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673338"
---
## <a name="add-an-output-binding-definition-to-the-function"></a>Přidání definice výstupní vazby do funkce

I když funkce může mít jenom jednu Trigger, může mít víc vstupních a výstupních vazeb, které vám umožní připojit se k dalším službám a prostředkům Azure bez nutnosti psát vlastní kód pro integraci. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Tyto vazby deklarujete v souboru *Function. JSON* ve složce Functions. Z předchozího rychlého startu soubor *Function. JSON* ve složce *HttpExample* obsahuje dvě vazby v `bindings` kolekci:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Každá vazba má alespoň typ, směr a název. V předchozím příkladu je první vazba typu `httpTrigger` s směrem. `in` Pro `in` směr `name` Určuje název vstupního parametru, který je odeslán funkci při vyvolání triggerem.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
Druhá vazba v kolekci je pojmenována `res`. Tato `http` vazba je výstupní vazba (`out`), která se používá k zápisu odpovědi HTTP. 

Chcete-li z této funkce zapisovat do fronty Azure Storage, přidejte `out` vazbu typu `queue` s názvem `msg`, jak je znázorněno v následujícím kódu:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
Druhá vazba `http` v kolekci je typu s směrem `out`. v takovém případě `name` to `$return` znamená, že tato vazba používá návratovou hodnotu funkce namísto zadání vstupního parametru.

Chcete-li z této funkce zapisovat do fronty Azure Storage, přidejte `out` vazbu typu `queue` s názvem `msg`, jak je znázorněno v následujícím kódu:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
Druhá vazba v kolekci je pojmenována `res`. Tato `http` vazba je výstupní vazba (`out`), která se používá k zápisu odpovědi HTTP. 

Chcete-li z této funkce zapisovat do fronty Azure Storage, přidejte `out` vazbu typu `queue` s názvem `msg`, jak je znázorněno v následujícím kódu:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
V tomto případě `msg` je funkce předána funkci jako výstupní argument. V případě `queue` typu je nutné zadat také název fronty v `queueName` a zadat *název* připojení Azure Storage (z *Local. Settings. JSON*) v `connection`. 
::: zone-end  
