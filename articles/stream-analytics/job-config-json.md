---
title: JobConfig.jsAzure Stream Analytics pro pole
description: V tomto článku jsou uvedena podporovaná pole pro Azure Stream Analytics JobConfig.jsv souboru, který slouží k vytváření úloh v Visual Studio Code.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 0676b987725a33049d9da3256bdd4e6dc8028d00
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045174"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>JobConfig.jsAzure Stream Analytics pro pole

Následující pole jsou podporovaná v *JobConfig.jspro* soubor, který se používá k [Vytvoření úlohy Azure Stream Analytics pomocí Visual Studio Code](quick-create-vs-code.md).

```json
{
    "DataLocale": "string",
    "OutputErrorPolicy": "string",
    "EventsLateArrivalMaxDelayInSeconds": "integer",
    "EventsOutOfOrderMaxDelayInSeconds": "integer",
    "EventsOutOfOrderPolicy": "string",
    "StreamingUnits": "integer",
    "CompatibilityLevel": "string",
    "UseSystemAssignedIdentity": "boolean",
    "GlobalStorage": {
        "AccountName": "string",
        "AccountKey": "string",
    },
    "DataSourceCredentialDomain": "string",
    "ScriptType": "string",
    "Tags": {}
}
```

|Name|Typ|Vyžadováno|Hodnota|
|----|----|--------|-----|
|Místní dataprostředí|řetězec|No|Národní prostředí pro data úlohy Stream Analytics. Hodnota by měla být název podporovaného typu. Pokud není zadaný, použije se výchozí hodnota EN-US.|
|OutputErrorPolicy|řetězec|No|Určuje zásadu, která se použije na události, které přicházejí do výstupu, a nejde je zapsat do externího úložiště, protože jsou poškozené (chybějící hodnoty sloupce, hodnoty sloupce nesprávného typu nebo velikosti). -Stop nebo drop|
|EventsLateArrivalMaxDelayInSeconds|celé číslo|No|Maximální přípustná prodleva v sekundách, kdy mohou být zahrnuty události přicházející pozdě. Podporovaný rozsah je-1 až 1814399 (20.23:59:59 dní) a-1 se používá k určení čekání na neomezenou dobu. Pokud vlastnost chybí, je interpretována tak, aby měla hodnotu-1.|
|EventsOutOfOrderMaxDelayInSeconds|celé číslo|No|Maximální přípustná prodleva v sekundách, kdy se události mimo pořadí dají upravit tak, aby se znovu nastavily.|
|EventsOutOfOrderPolicy|řetězec|No|Určuje zásadu, která se použije na události, které dorazí do vstupního proudu událostí mimo pořadí. -Upravit nebo vyřadit|
|StreamingUnits|celé číslo|Yes|Určuje počet jednotek streamování, které používá úloha streamování.|
|CompatibilityLevel|řetězec|No|Řídí určitá chování za běhu úlohy streamování. -Přijatelné hodnoty jsou "1,0", "1,1", "1,2"|
|UseSystemAssignedIdentity|Boolean|No|Nastavte hodnotu true, pokud chcete, aby tato úloha komunikovala s ostatními službami Azure samostatně pomocí spravované Azure Active Directory identity.|
|GlobalStorage. Account|řetězec|No|Globální účet úložiště se používá k ukládání obsahu souvisejícího s vaší úlohou Stream Analytics, jako jsou snímky dat SQL Reference.|
|GlobalStorage. AccountKey|řetězec|No|Odpovídající klíč pro globální účet úložiště|
|DataSourceCredentialDomain|řetězec|No|Rezervovaná vlastnost pro místní úložiště přihlašovacích údajů|
|ScriptType|řetězec|Yes|Rezervovaná vlastnost, která označuje typ tohoto zdrojového souboru. Přijatelná hodnota je "JobConfig" pro JobConfig.jsna.|
|Značky|Páry klíč-hodnota JSON|No|Značky jsou páry název-hodnota, které umožňují kategorizaci prostředků a zobrazení konsolidované fakturace, a to použitím stejné značky na více prostředků a skupin prostředků. V názvech značek jsou rozlišována velká a malá písmena.|

## <a name="next-steps"></a>Další kroky

* [Vytvoření úlohy Azure Stream Analytics v Visual Studio Code](quick-create-vs-code.md)
* [Test Stream Analytics dotazy místně s použitím ukázkových dat pomocí Visual Studio Code](visual-studio-code-local-run.md)
* [Test Stream Analytics dotazy místně proti vstupu živého datového proudu pomocí Visual Studio Code](visual-studio-code-local-run-live-input.md) 
* [Nasazení úlohy Azure Stream Analytics pomocí balíčku CI/CD npm](setup-cicd-vs-code.md)