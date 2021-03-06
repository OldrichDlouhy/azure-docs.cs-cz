---
title: Plán Azure Functions Premium
description: Podrobnosti a možnosti konfigurace (virtuální síť, bez počátečního startu, neomezené trvání spuštění) pro plán Azure Functions Premium.
author: jeffhollan
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: jehollan
ms.custom: references_regions
ms.openlocfilehash: 5ab506c57a78c67b33b888f1f50d83fe9813d0af
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86506192"
---
# <a name="azure-functions-premium-plan"></a>Plán Azure Functions Premium

Plán Azure Functions Premium (někdy označovaný jako elastický plán Premium) je možnost hostování pro aplikace Function App. Plán Premium poskytuje funkce, jako je připojení k virtuální síti, žádný studený start a špičkový hardware.  Do stejného plánu Premium se dá nasadit víc aplikací Function App a plán vám umožní nakonfigurovat velikost instance COMPUTE, velikost základního plánu a maximální velikost plánu.  Porovnání plánu Premium a dalších typů plánování a hostování najdete v tématu [možnosti škálování a hostování funkcí](functions-scale.md).

## <a name="create-a-premium-plan"></a>Vytvořit plán Premium

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Plán Premium můžete vytvořit také pomocí příkaz [AZ functionapp Plan Create](/cli/azure/functionapp/plan#az-functionapp-plan-create) v rozhraní příkazového řádku Azure CLI. Následující příklad vytvoří plán vrstvy _elastické úrovně Premium 1_ :

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

V tomto příkladu nahraďte `<RESOURCE_GROUP>` svou skupinou prostředků a `<PLAN_NAME>` názvem vašeho plánu, který je ve skupině prostředků jedinečný. Zadejte [podporovanou `<REGION>` ](https://azure.microsoft.com/global-infrastructure/services/?products=functions)hodnotu. Pokud chcete vytvořit plán Premium, který podporuje Linux, zahrňte `--is-linux` možnost.

Pomocí vytvořeného plánu můžete vytvořit aplikaci Function App pomocí [AZ functionapp Create](/cli/azure/functionapp#az-functionapp-create) . Na portálu se současně vytvoří plán i aplikace. Příklad kompletního skriptu Azure CLI najdete v tématu [Vytvoření aplikace Function App v plánu Premium](scripts/functions-cli-create-premium-plan.md).

## <a name="features"></a>Funkce

K dispozici jsou následující funkce pro aplikace Functions nasazené do plánu Premium.

### <a name="pre-warmed-instances"></a>Předem zahřívání instance

Pokud v plánu spotřeby nejsou žádné události a spuštění, vaše aplikace se může škálovat na nulové instance. Když se přidají nové události v, musí být speciální instance specializovaná na svou aplikaci, která je na ní spuštěná.  Specializace nových instancí může v závislosti na aplikaci nějakou dobu trvat.  Tato další latence při prvním volání se často označuje jako studený start aplikace.

V plánu Premium můžete mít aplikaci předem zahřívání na určitém počtu instancí až do minimální velikosti plánu.  Předem zavedené instance také umožňují předem škálovat aplikaci před velkým objemem zátěže. Vzhledem k tomu, že se aplikace škáluje, nejprve se škáluje do předem zahřívání instancí. Další instance pokračují ve vyrovnávací paměti a zahřívá se hned po přípravě na další operaci škálování. Když máte vyrovnávací paměť předběžně zavedených instancí, můžete efektivně zabránit latenci při počátečním startu.  Předem zavedené instance jsou součástí plánu Premium a je potřeba, abyste zachovali aspoň jednu instanci, která je spuštěná a dostupná vždy, když je plán aktivní.

Počet předem zavedených instancí můžete v Azure Portal nakonfigurovat tak, že vyberete **Function App**a kliknete na kartu **funkce platformy** a vyberete možnosti **horizontálního** navýšení kapacity. V okně pro úpravu aplikace Function App jsou předem zavedené instance specifické pro danou aplikaci, ale minimální a maximální počet instancí platí pro celý plán.

![Nastavení elastického škálování](./media/functions-premium-plan/scale-out.png)

V Azure CLI můžete také nakonfigurovat předem zavedené instance pro aplikaci.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Připojení k privátní síti

Azure Functions nasazené do plánu Premium využívá [novou integraci virtuální sítě pro webové aplikace](../app-service/web-sites-integrate-with-vnet.md).  Když nakonfigurujete aplikaci, může komunikovat s prostředky ve vaší virtuální síti nebo zabezpečená prostřednictvím koncových bodů služby.  V aplikaci jsou k dispozici také omezení IP pro omezení příchozího provozu.

Při přiřazování podsítě do aplikace Function App v plánu Premium budete potřebovat podsíť s dostatečnými IP adresami pro každou potenciální instanci. Vyžadujeme blok IP s minimálně 100 dostupnými adresami.

Další informace najdete v tématu [integrace aplikace Function App s virtuální](functions-create-vnet.md)sítí.

### <a name="rapid-elastic-scale"></a>Rychlé Elastické škálování

Další výpočetní instance se automaticky přidají do vaší aplikace pomocí stejné logiky rychlé škálování jako u plánu spotřeby. Aplikace ve stejném App Service plánu se škálují nezávisle na sobě, a to na základě potřeb jednotlivých aplikací. Aplikace Functions ve stejném App Service naplánují sdílení prostředků virtuálních počítačů, které vám pomůžou snížit náklady, pokud je to možné. Počet aplikací přidružených k virtuálnímu počítači závisí na tom, jaké aplikace a velikost virtuálního počítače.

Další informace o tom, jak škálování funguje, najdete v tématu [škálování a hostování funkcí](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Delší doba běhu

Azure Functions v plánu spotřeby se pro jedno spuštění omezí na 10 minut.  V plánu Premium je doba běhu standardně 30 minut, aby se zabránilo provádění. Můžete ale [upravit host.jsv konfiguraci](./functions-host-json.md#functiontimeout) , aby to nebylo pro aplikace Premium Plan (garantované 60 minut) nevázané.

## <a name="plan-and-sku-settings"></a>Nastavení plánu a SKU

Při vytváření plánu nakonfigurujete dvě nastavení: minimální počet instancí (nebo velikost plánu) a maximální limit shlukování.  Minimální instance jsou rezervované a vždycky spuštěné.

> [!IMPORTANT]
> Za každou instanci přidělenou v minimálním počtu instancí se účtuje bez ohledu na to, jestli jsou funkce spuštěné nebo ne.

Pokud vaše aplikace vyžaduje instance nad rámec velikosti vašeho plánu, může pokračovat horizontální navýšení kapacity, dokud počet instancí nedosáhne maximálního limitu shlukování.  Účtují se za instance přesahující váš plán jenom v době, kdy jsou spuštěné a pronajaté.  Připravujeme úsilí, aby se vaše aplikace přihlásila na vymezený maximální limit, zatímco pro vaši aplikaci jsou zaručené minimální instance plánu.

Velikost plánu a maximum v Azure Portal můžete nakonfigurovat výběrem možností **horizontálního** navýšení kapacity v plánu nebo aplikace Function App nasazené do tohoto plánu (v části **funkce platformy**).

Můžete taky zvýšit maximální limit shluku z Azure CLI:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Dostupné skladové položky instance

Při vytváření nebo škálování plánu si můžete vybrat mezi třemi velikostmi instancí.  Bude se vám účtovat celkový počet jader a využité paměti za sekundu.  Vaše aplikace se může podle potřeby automaticky škálovat na více instancí.  

|Skladová položka|Cores|Paměť|Storage|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7GB|250 GB|
|EP3|4|S frekvencí|250 GB|

### <a name="memory-utilization-considerations"></a>Požadavky na využití paměti
Spuštění v počítači, který má více paměti, neznamená vždycky, že vaše aplikace Function App bude používat veškerou dostupnou paměť.

Například aplikace funkcí JavaScriptu je omezená na výchozí omezení paměti v Node.js. Pokud chcete zvýšit toto omezení pevné paměti, přidejte nastavení aplikace `languageWorkers:node:arguments` s hodnotou `--max-old-space-size=<max memory in MB>` .

## <a name="region-max-scale-out"></a>Maximální horizontální navýšení kapacity oblasti

Níže jsou uvedené maximální podporované hodnoty horizontálního navýšení kapacity pro jeden plán v každé oblasti a konfiguraci operačního systému. Pokud chcete požádat o zvýšení, otevřete prosím lístek podpory.

Kompletní regionální dostupnost funkcí najdete tady: [Azure.com](https://azure.microsoft.com/global-infrastructure/services/?products=functions)

|Oblast| Windows | Linux |
|--| -- | -- |
|Austrálie – střed| 20 | Není k dispozici |
|Austrálie – střed 2| 20 | Není k dispozici |
|Austrálie – východ| 100 | 20 |
|Australia Southeast | 100 | 20 |
|Brazil South| 60 | 20 |
|Střední Kanada| 100 | 20 |
|Střední USA| 100 | 20 |
|Východní Asie| 100 | 20 |
|East US | 100 | 20 |
|USA – východ 2| 100 | 20 |
|Francie – střed| 100 | 20 |
|Německo – středozápad| 100 | Není k dispozici |
|Japan East| 100 | 20 |
|Japonsko – západ| 100 | 20 |
|Jižní Korea – střed| 100 | 20 |
|USA – středosever| 100 | 20 |
|Severní Evropa| 100 | 20 |
|Norsko – východ| 20 | 20 |
|Středojižní USA| 100 | 20 |
|Indie – jih | 100 | Není k dispozici |
|Jihovýchodní Asie| 100 | 20 |
|Spojené království – jih| 100 | 20 |
|Spojené království – západ| 100 | 20 |
|Západní Evropa| 100 | 20 |
|Západní Indie| 100 | 20 |
|USA – středozápad| 20 | 20 |
|USA – západ| 100 | 20 |
|Západní USA 2| 100 | 20 |

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Pochopení možností škálování a hostování Azure Functions](functions-scale.md)
