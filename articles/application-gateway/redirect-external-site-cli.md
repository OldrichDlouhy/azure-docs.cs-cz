---
title: Přesměrování externího provozu pomocí CLI – Azure Application Gateway
description: Naučte se, jak vytvořit Aplikační bránu, která přesměruje vnitřní webový provoz do příslušného fondu pomocí Azure CLI.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/13/2019
ms.author: victorh
ms.openlocfilehash: b671af46d34e3b45796186cd10282d060fa8c537
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84804674"
---
# <a name="create-an-application-gateway-with-external-redirection-using-the-azure-cli"></a>Vytvoření aplikační brány s externím přesměrování pomocí Azure CLI

Rozhraní příkazového řádku Azure můžete použít ke konfiguraci [přesměrování webového provozu](multiple-site-overview.md) při vytváření [aplikační brány](overview.md). V tomto kurzu nakonfigurujete naslouchací proces a pravidlo, které přesměrují webový provoz, který je součástí aplikační brány, na externí Web.

V tomto článku získáte informace o těchto tématech:

> [!div class="checklist"]
> * Nastavit síť
> * Vytvoření naslouchacího procesu a pravidla přesměrování
> * Vytvoření služby Application Gateway

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Pomocí příkazu [az group create](/cli/azure/group) vytvořte skupinu prostředků.

V následujícím příkladu vytvoříte skupinu prostředků s názvem *myResourceGroupAG* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Vytvoření síťových prostředků 

Pomocí příkazu [az network vnet create](/cli/azure/network/vnet) vytvořte virtuální síť s názvem *myVNet* a podsíť s názvem *myAGSubnet*. Pomocí příkazu [az network public-ip create](/cli/azure/network/public-ip) vytvořte veřejnou IP adresu s názvem *myAGPublicIPAddress*. Pomocí těchto prostředků se bude poskytovat síťové připojení k bráně Application Gateway a jejím přidruženým prostředkům.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

K vytvoření aplikační brány s názvem *myAppGateway* použijte příkaz [az network application-gateway create](/cli/azure/network/application-gateway). Při vytváření aplikační brány pomocí Azure CLI zadáte konfigurační údaje, jako je kapacita, skladová položka nebo nastavení HTTP. Aplikační brána je přiřazena k *myAGSubnet* a *myPublicIPAddress* , které jste vytvořili dříve. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 8080 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

Vytvoření aplikační brány může trvat několik minut. Po vytvoření aplikační brány se zobrazí tyto její nové funkce:

- *appGatewayBackendPool* – aplikační brána musí mít aspoň jeden back-endový fond adres.
- *appGatewayBackendHttpSettings* – určuje, že se ke komunikaci používá port 80 a protokol HTTP.
- *appGatewayHttpListener* – výchozí naslouchací proces přidružený k *appGatewayBackendPool*.
- *appGatewayFrontendIP* – přiřadí adresu *myAGPublicIPAddress* naslouchacímu procesu *appGatewayHttpListener*.
- *rule1* – výchozí pravidlo směrování přidružené k naslouchacímu procesu *appGatewayHttpListener*.

### <a name="add-the-redirection-configuration"></a>Přidat konfiguraci přesměrování

Přidejte konfiguraci přesměrování, která odesílá provoz z *webové služby \. consoto.org* do naslouchacího procesu pro službu *www \. contoso.com* na službu Application Gateway pomocí funkce [AZ Network Application-Gateway redirect-config Create](/cli/azure/network/application-gateway/redirect-config).

```azurecli-interactive
az network application-gateway redirect-config create \
  --name myredirect \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --type Temporary \
  --target-url "https://bing.com"
```

### <a name="add-a-listener-and-routing-rule"></a>Přidat naslouchací proces a pravidlo směrování

Naslouchací proces je nutný k tomu, aby brána Application Gateway mohla vhodným způsobem směrovat provoz. Vytvořte naslouchací proces pomocí [AZ Network Application-Gateway http-naslouchacího procesu Create](/cli/azure/network/application-gateway) s portem front-endu vytvořeno pomocí [AZ Network Application-Gateway Endu-port Create](/cli/azure/network/application-gateway). Pro naslouchací proces, který je vyžadován pro odeslání příchozího provozu, je vyžadováno pravidlo. Vytvořte základní pravidlo s názvem *redirectRule* pomocí [AZ Network Application-Gateway Rule Create](/cli/azure/network/application-gateway).

```azurecli-interactive
az network application-gateway frontend-port create \
  --port 80 \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name redirectPort
az network application-gateway http-listener create \
  --name redirectListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port redirectPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name redirectRule \
  --resource-group myResourceGroupAG \
  --http-listener redirectListener \
  --rule-type Basic \
  --redirect-config myredirect
```

## <a name="test-the-application-gateway"></a>Testování brány Application Gateway

K získání veřejné IP adresy aplikační brány můžete použít příkaz [az network public-ip show](/cli/azure/network/public-ip). Zkopírujte veřejnou IP adresu a pak ji vložte do adresního řádku svého prohlížeče.

V prohlížeči by se měla zobrazit *Bing.com* .

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> * Nastavit síť
> * Vytvoření naslouchacího procesu a pravidla přesměrování
> * Vytvoření služby Application Gateway
