---
title: Přepsání podsítě Azure Traffic Manager pomocí rozhraní příkazového řádku Azure | Microsoft Docs
description: Tento článek vám pomůže porozumět tomu, jak Traffic Manager přepsání podsítě použít k přepsání metody směrování Traffic Manager profilu pro směrování provozu do koncového bodu na základě IP adresy koncového uživatele prostřednictvím předdefinovaného rozsahu IP adres pro mapování koncových bodů.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
ms.topic: how-to
ms.service: traffic-manager
ms.date: 09/18/2019
ms.author: rohink
ms.openlocfilehash: 33fdc2875d8d766f212301ddfecf9dbb734b876a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84689686"
---
# <a name="traffic-manager-subnet-override-using-azure-cli"></a>Přepsání podsítě Traffic Manager pomocí Azure CLI

Přepsání podsítě Traffic Manager umožňuje změnit metodu směrování profilu.  Přidání přepsání bude směrovat provoz na základě IP adresy koncového uživatele s předdefinovaným rozsahem IP adres na mapování koncových bodů. 

## <a name="how-subnet-override-works"></a>Jak funguje přepsání podsítě

Při přidání přepsání podsítě do profilu Traffic Manageru se Traffic Manager nejdřív zkontroluje, jestli pro IP adresu koncového uživatele existuje přepsání podsítě. Je-li nalezen, bude dotaz DNS uživatele směrován do odpovídajícího koncového bodu.  Pokud mapování nenajdete, Traffic Manager se vrátí do původní metody směrování tohoto profilu. 

Rozsahy IP adres se dají zadat buď jako rozsahy CIDR (například 1.2.3.0/24), nebo jako rozsah adres (například 1.2.3.4-5.6.7.8). Rozsahy IP adres přidružené ke každému koncovému bodu musí být pro tento koncový bod jedinečné. V případě překrytí rozsahů IP adres mezi různými koncovými body dojde k odmítnutí profilu Traffic Manager.

Existují dva typy profilů směrování, které podporují přepsání podsítí:

* **Geografické** – Pokud Traffic Manager najde přepsání podsítě pro IP adresu dotazu DNS, bude směrovat dotaz na koncový bod bez ohledu na stav koncového bodu.
* **Výkon** – Pokud Traffic Manager nalezne přepsání podsítě pro IP adresu dotazu DNS, bude směrovat provoz pouze do koncového bodu, pokud je v pořádku.  Traffic Manager se vrátí do heuristiky směrování výkonu, pokud koncový bod přepsání podsítě není v pořádku.

## <a name="create-a-traffic-manager-subnet-override"></a>Vytvoření přepsání podsítě Traffic Manager

Pokud chcete vytvořit přepsání podsítě Traffic Manager, můžete pomocí Azure CLI přidat podsítě pro přepsání do koncového bodu Traffic Manager.

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít Azure CLI verze 2.0.28 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI]( /cli/azure/install-azure-cli).

## <a name="update-the-traffic-manager-endpoint-with-subnet-override"></a>Aktualizujte Traffic Manager koncový bod pomocí přepsání podsítě.
Pomocí Azure CLI aktualizujte koncový bod pomocí [AZ Network Traffic-Manager Endpoint Update](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update).

```azurecli

### Add a range of IPs ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 1.2.3.4-5.6.7.8 \
    --type AzureEndpoints

### Add a subnet ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 9.10.11.0:24 \
    --type AzureEndpoints

```

Rozsahy IP adres můžete odebrat spuštěním příkazu [AZ Network Traffic-Manager Endpoint Update](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update) s možností **--Remove** .

```azurecli

az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --remove subnets \
    --type AzureEndpoints

```
## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [metodách směrování provozu](traffic-manager-routing-methods.md)Traffic Manager.

Přečtěte si o [způsobu směrování provozu podsítě](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#subnet-traffic-routing-method)