---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 873fd8cbc211f098c93b8fb3fbe701e4a34d8487
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "68320517"
---
`Logging` Nastavení spravovat ASP.NET Core podporu protokolování pro váš kontejner. Můžete použít stejné nastavení konfigurace a hodnoty pro váš kontejner, který používáte pro aplikaci ASP.NET Core. 

Kontejner podporuje následující zprostředkovatele protokolování:

|Poskytovatel|Účel|
|--|--|
|[Konzola](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|Zprostředkovatel protokolování `Console` ASP.NET Core. Všechna nastavení konfigurace ASP.NET Core a výchozí hodnoty pro tohoto zprostředkovatele protokolování jsou podporovány.|
|[Ladí](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|Zprostředkovatel protokolování `Debug` ASP.NET Core. Všechna nastavení konfigurace ASP.NET Core a výchozí hodnoty pro tohoto zprostředkovatele protokolování jsou podporovány.|
|[Disk](#disk-logging)|Zprostředkovatel protokolování JSON. Tento zprostředkovatel protokolování zapisuje data protokolu do výstupního připojení.|

Tento příkaz kontejneru ukládá do výstupního připojení informace o protokolování ve formátu JSON:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Tento příkaz kontejneru zobrazuje informace o ladění s `dbug`předponou, zatímco je spuštěný kontejner:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Protokolování disku

Zprostředkovatel `Disk` protokolování podporuje následující nastavení konfigurace:

| Název | Datový typ | Popis |
|------|-----------|-------------|
| `Format` | Řetězec | Výstupní formát pro soubory protokolu.<br/> **Poznámka:** Tato hodnota musí být nastavena na `json` hodnotu pro povolení poskytovatele protokolování. Pokud je tato hodnota zadána bez zadání výstupního připojení při vytváření instance kontejneru, dojde k chybě. |
| `MaxFileSize` | Integer | Maximální velikost souboru protokolu v megabajtech (MB). Když velikost aktuálního souboru protokolu splňuje nebo překračuje tuto hodnotu, spustí zprostředkovatel protokolování nový soubor protokolu. Je-li zadána hodnota-1, velikost souboru protokolu je omezena pouze maximální velikostí souboru (pokud existuje) pro připojení pro výstup. Výchozí hodnota je 1. |

Další informace o konfiguraci podpory protokolování ASP.NET Core najdete v tématu [nastavení souboru konfigurace](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

