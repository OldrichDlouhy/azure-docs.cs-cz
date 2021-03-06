---
title: Vytvoření instance služby Azure Network Watcher | Microsoft Docs
description: Naučte se, jak povolit Network Watcher v oblasti Azure.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 6b7453a0100233896e3d01769897ed224a0b2618
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84737542"
---
# <a name="create-an-azure-network-watcher-instance"></a>Vytvoření instance služby Azure Network Watcher

Network Watcher je místní služba, která umožňuje monitorovat a diagnostikovat podmínky na úrovni síťového scénáře v, do a z Azure. Monitorování úrovně scénáře umožňuje diagnostikovat problémy v zobrazení na úrovni koncových sítí. Nástroje pro diagnostiku a diagnostiku sítě, které jsou dostupné v Network Watcher, vám pomůžou pochopit, diagnostikovat a získávat přehledy vaší sítě v Azure. Network Watcher je povolená při vytváření prostředku Network Watcher. Tento prostředek vám umožní využívat Network Watcher možnosti.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="network-watcher-is-automatically-enabled"></a>Network Watcher je automaticky povoleno.
Když ve svém předplatném vytvoříte nebo aktualizujete virtuální síť, automaticky se pro oblast této virtuální sítě povolí Network Watcher. Automatické povolení služby Network Watcher nemá žádný vliv na vaše prostředky ani se s ním nepojí žádné poplatky.

#### <a name="opt-out-of-network-watcher-automatic-enablement"></a>Výslovný nesouhlas s Network Watcher automatické povolení
Pokud se chcete odhlásit z Network Watcher automatické povolení, můžete to udělat tak, že spustíte následující příkazy:

> [!WARNING]
> Vypnutí Network Watcher automatické povolení je trvalá změna. Jakmile se odhlásíte, nemůžete se vyjádřit, aniž byste [kontaktovali podporu](https://azure.microsoft.com/support/options/) .

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName DisableNetworkWatcherAutocreation -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

```azurecli-interactive
az feature register --name DisableNetworkWatcherAutocreation --namespace Microsoft.Network
az provider register -n Microsoft.Network
```



## <a name="create-a-network-watcher-in-the-portal"></a>Vytvoření Network Watcher na portálu

Přejděte na **všechny služby**  >  **sítě**  >  **Network Watcher**. Můžete vybrat všechna předplatná, která chcete povolit Network Watcher pro. Tato akce vytvoří Network Watcher v každé oblasti, která je k dispozici.

![Vytvoření sledovacího procesu sítě](./media/network-watcher-create/figure1.png)

Pokud povolíte Network Watcher pomocí portálu, název instance Network Watcher je automaticky nastaven na *NetworkWatcher_region_name* , kde *Region_name* odpovídá oblasti Azure, ve které je instance povolená. Například Network Watcher povolený v Středozápadní USA oblasti má název *NetworkWatcher_westcentralus*.

Instance Network Watcher se automaticky vytvoří ve skupině prostředků s názvem *NetworkWatcherRG*. Skupina prostředků se vytvoří, pokud ještě neexistuje.

Pokud chcete přizpůsobit název instance Network Watcher a skupinu prostředků, do které se umístí, můžete použít PowerShell, rozhraní příkazového řádku Azure CLI, REST API nebo ARMClient metody popsané v následujících částech. V každé z možností musí skupina prostředků existovat, aby bylo možné v ní vytvořit Network Watcher.  

## <a name="create-a-network-watcher-with-powershell"></a>Vytvoření Network Watcher s využitím PowerShellu

Chcete-li vytvořit instanci Network Watcher, spusťte následující příklad:

```powershell
New-AzNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Vytvoření Network Watcher pomocí Azure CLI

Chcete-li vytvořit instanci Network Watcher, spusťte následující příklad:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Vytvoření Network Watcher s REST API

ARMclient se používá k volání REST API s využitím PowerShellu. ARMClient se nachází v čokoládě na [ARMClient při čokoládě](https://chocolatey.org/packages/ARMClient) .

### <a name="log-in-with-armclient"></a>Přihlášení pomocí ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Vytvoření sledovacího procesu sítě

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="delete-a-network-watcher-in-the-portal"></a>Odstranění Network Watcher na portálu

Přejděte na **všechny služby**  >  **sítě**  >  **Network Watcher**.

Vyberte kartu Přehled, pokud ještě nejste. Pomocí rozevírací nabídky vyberte předplatné, ve kterém chcete sledovací proces sítě vypnout.
Kliknutím na šipku rozbalte seznam oblastí zvoleného předplatného. V případě potřeby použijte pro přístup k místní nabídce 3 tečky na pravé straně.
Kliknutím na zakázat sledování sítě zahajte zakazování. Zobrazí se výzva k potvrzení tohoto kroku. Pro pokračování klikněte na Ano.
Na portálu ho budete muset udělat jednotlivě pro každou oblast v každém předplatném.


## <a name="delete-a-network-watcher-with-powershell"></a>Odstranění Network Watcher pomocí PowerShellu

Chcete-li odstranit instanci Network Watcher, spusťte následující příklad:

```powershell
New-AzResourceGroup -Name NetworkWatcherRG -Location westcentralus
New-AzNetworkWatcher -Name NetworkWatcher_westcentralus -ResourceGroup NetworkWatcherRG -Location westcentralus
Remove-AzNetworkWatcher -Name NetworkWatcher_westcentralus -ResourceGroup NetworkWatcherRG
```

## <a name="next-steps"></a>Další kroky

Teď, když máte instanci Network Watcher, přečtěte si informace o dostupných funkcích:

* [Topologie](network-watcher-topology-overview.md)
* [Zachytávání paketů](network-watcher-packet-capture-overview.md)
* [Ověření toku protokolu IP](network-watcher-ip-flow-verify-overview.md)
* [Další směrování](network-watcher-next-hop-overview.md)
* [Zobrazení skupin zabezpečení](network-watcher-security-group-view-overview.md)
* [Protokolování toku NSG](network-watcher-nsg-flow-logging-overview.md)
* [Řešení potíží s Virtual Networkovou bránou](network-watcher-troubleshoot-overview.md)
