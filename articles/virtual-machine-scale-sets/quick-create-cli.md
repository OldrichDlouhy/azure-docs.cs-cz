---
title: Rychlý Start – vytvoření sady škálování virtuálních počítačů pomocí Azure CLI
description: Začněte s nasazeními a Naučte se, jak rychle vytvořit sadu škálování virtuálního počítače pomocí Azure CLI.
author: ju-shim
ms.author: jushiman
ms.topic: quickstart
ms.service: virtual-machine-scale-sets
ms.subservice: cli
ms.date: 03/27/2018
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 416b5c03686f1b008473dd843c71bbe1139c444c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083561"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-with-the-azure-cli"></a>Rychlý start: Vytvoření škálovací sady virtuálních počítačů pomocí Azure CLI
Sada škálování virtuálního počítače umožňuje nasadit a spravovat sadu automatických škálování virtuálních počítačů. Počet virtuálních počítačů ve škálovací sadě můžete škálovat ručně nebo můžete definovat pravidla pro automatické škálování podle využití prostředků, například podle požadavků na CPU a paměť nebo podle provozu. Nástroj pro vyrovnávání zatížení Azure pak bude distribuovat provoz do instancí virtuálních počítačů ve škálovací sadě. V tomto rychlém startu vytvoříte škálovací sadu virtuálních počítačů a nasadíte ukázkovou aplikaci pomocí Azure CLI.

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít Azure CLI verze 2.0.29 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI]( /cli/azure/install-azure-cli). 


## <a name="create-a-scale-set"></a>Vytvoření škálovací sady
Než vytvoříte škálovací sadu, vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v umístění *eastus* :

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Teď vytvořte škálovací sadu virtuálních počítačů pomocí příkazu [az vmss create](/cli/azure/vmss). Následující příklad vytvoří škálovací sadu *myScaleSet* nastavenou tak, aby se při provedení změn automaticky aktualizovala, a vygeneruje klíče SSH v adresáři *~/.ssh/id_rsa*, pokud ještě neexistují. Pomocí těchto klíčů SSH se v případě potřeby můžete přihlásit k instancím virtuálních počítačů. Pokud chcete použít existující sadu klíčů SSH, použijte parametr `--ssh-key-value` a zadejte umístění svých klíčů.

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

Vytvoření a konfigurace všech prostředků škálovací sady a virtuálních počítačů trvá několik minut.


## <a name="deploy-sample-application"></a>Nasazení ukázkové aplikace
Pokud chcete svou škálovací sadu otestovat, nainstalujte základní webovou aplikaci. Ke stažení a spuštění skriptu, který na instance virtuálních počítačů nainstaluje aplikaci, se používá rozšíření vlastních skriptů Azure. Toto rozšíření je užitečné pro konfiguraci po nasazení, instalaci softwaru nebo jakékoli jiné úlohy konfigurace nebo správy. Další informace najdete v tématu [Přehled rozšíření vlastních skriptů](../virtual-machines/extensions/custom-script-linux.md).

Pomocí rozšíření vlastních skriptů nainstalujte základní webový server NGINX. Následujícím způsobem použijte rozšíření vlastních skriptů, které pomocí příkazu [az vmss extension set](/cli/azure/vmss/extension) nainstaluje server NGINX:

```azurecli-interactive
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"],"commandToExecute":"./automate_nginx.sh"}'
```


## <a name="allow-traffic-to-application"></a>Povolení provozu do aplikace
Při vytvoření škálovací sady se automaticky nasadil nástroj pro vyrovnávání zatížení Azure. Nástroj pro vyrovnávání zatížení distribuuje provoz do instancí virtuálních počítačů ve škálovací sadě. Pokud chcete ukázkové webové aplikaci povolit příjem provozu, vytvořte pravidlo nástroje pro vyrovnávání zatížení pomocí příkazu [az network lb rule create](/cli/azure/network/lb/rule). Následující příklad vytvoří pravidlo *myLoadBalancerRuleWeb*:

```azurecli-interactive
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```


## <a name="test-your-scale-set"></a>Test škálovací sady
Pokud chcete vidět svou škálovací sadu v akci, přejděte ve webovém prohlížeči na ukázkovou webovou aplikaci. Získejte veřejnou IP adresu svého nástroje pro vyrovnávání zatížení pomocí příkazu [az network public-ip show](/cli/azure/network/public-ip). Následující příklad získá IP adresu *myScaleSetLBPublicIP* vytvořenou jako součást škálovací sady:

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetLBPublicIP \
  --query '[ipAddress]' \
  --output tsv
```

Zadejte veřejnou IP adresu nástroje pro vyrovnávání zatížení do webového prohlížeče. Nástroj pro vyrovnávání zatížení distribuuje provoz do jedné z vašich instancí virtuálních počítačů, jak je znázorněno v následujícím příkladu:

![Výchozí webová stránka na serveru NGINX](media/virtual-machine-scale-sets-create-cli/running-nginx-site.png)


## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, škálovací sady a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group), jak je znázorněno níže. Parametr `--no-wait` vrátí řízení na příkazový řádek bez čekání na dokončení operace. Parametr `--yes` potvrdí, že chcete prostředky odstranit, aniž by se na to zobrazoval další dotaz.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```


## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste vytvořili základní škálovací sadu a pomocí rozšíření vlastních skriptů jste na instance virtuálních počítačů nainstalovali základní webový server NGINX. Další informace najdete v kurzu věnovaném vytváření a správě škálovacích sad virtuálních počítačů Azure.

> [!div class="nextstepaction"]
> [Vytváření a správa škálovacích sad virtuálních počítačů Azure](tutorial-create-and-manage-cli.md)
