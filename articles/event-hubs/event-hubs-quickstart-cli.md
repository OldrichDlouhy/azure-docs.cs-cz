---
title: Vytvoření centra událostí pomocí Azure CLI – Azure Event Hubs | Microsoft Docs
description: Tento rychlý start popisuje, jak pomocí Azure CLI vytvořit centrum událostí a pak odesílat a přijímat události pomocí Javy.
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 90828a09b925fd3af774b22db3102094c1dd0c6d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537152"
---
# <a name="quickstart-create-an-event-hub-using-azure-cli"></a>Rychlý start: Vytvoření centra událostí pomocí Azure CLI

Azure Event Hubs je platforma pro streamování velkých objemů dat a služba pro ingestování událostí, která je schopná přijmout a zpracovat miliony událostí za sekundu. Služba Event Hubs dokáže zpracovávat a ukládat události, data nebo telemetrické údaje produkované distribuovaným softwarem a zařízeními. Data odeslaná do centra událostí je možné transformovat a uložit pomocí libovolného poskytovatele analýz v reálném čase nebo adaptérů pro dávkové zpracování a ukládání. Podrobnější přehled služby Event Hubs najdete v tématech [Přehled služby Event Hubs](event-hubs-about.md) a [Funkce služby Event Hubs](event-hubs-features.md).

V tomto rychlém startu vytvoříte centrum událostí pomocí Azure CLI.

## <a name="prerequisites"></a>Předpoklady
K dokončení tohoto rychlého startu potřebujete předplatné Azure. Pokud ho ještě nemáte, [Vytvořte si bezplatný účet][] před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat Azure CLI místně, musíte mít Azure CLI verze 2.0.4 nebo novější. Spuštěním příkazu `az --version` zkontrolujte svou verzi. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace rozhraní příkazového řádku Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Pokud spouštíte příkazy ve službě Cloud Shell, následující kroky nemusíte provádět. Pokud používáte rozhraní příkazového řádku místně, provedením následujících kroků se přihlaste k Azure a nastavte své aktuální předplatné:

Spuštěním následujícího příkazu se přihlaste k Azure:

```azurecli-interactive
az login
```

Nastavte kontext na aktuální předplatné. Nahraďte `MyAzureSub` názvem předplatného Azure, které chcete použít:

```azurecli-interactive
az account set --subscription MyAzureSub
``` 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Skupina prostředků je logická kolekce prostředků Azure. Všechny prostředky se nasazují a spravují ve skupině prostředků. Spuštěním následujícího příkazu vytvoříte skupinu prostředků:

```azurecli-interactive
# Create a resource group. Specify a name for the resource group.
az group create --name <resource group name> --location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Vytvoření oboru názvů služby Event Hubs
Obor názvů služby Event Hubs poskytuje jedinečný kontejner oboru, na který se odkazuje jeho plně kvalifikovaným názvem domény a ve kterém můžete vytvořit jedno nebo více center událostí. Pokud chcete vytvořit obor názvů ve své skupině prostředků, spusťte následující příkaz:

```azurecli-interactive
# Create an Event Hubs namespace. Specify a name for the Event Hubs namespace.
az eventhubs namespace create --name <Event Hubs namespace> --resource-group <resource group name> -l <region, for example: East US>
```

## <a name="create-an-event-hub"></a>Vytvoření centra událostí
Spuštěním následujícího příkazu vytvořte centrum událostí:

```azurecli-interactive
# Create an event hub. Specify a name for the event hub. 
az eventhubs eventhub create --name <event hub name> --resource-group <resource group name> --namespace-name <Event Hubs namespace>
```

Blahopřejeme! Pomocí Azure CLI jste vytvořili obor názvů služby Event Hubs a v něm centrum událostí. 

## <a name="next-steps"></a>Další kroky

V tomto článku jste vytvořili skupinu prostředků, obor názvů služby Event Hubs a centrum událostí. Podrobné pokyny k odesílání událostí do (nebo) přijímání událostí z centra událostí najdete v tématu kurzy pro **odesílání a příjem** : 

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
- [Přejít](event-hubs-go-get-started-send.md)
- [C (jenom odesílání)](event-hubs-c-getstarted-send.md)
- [Apache Storm (jenom příjem)](event-hubs-storm-getstarted-receive.md)

[Vytvoření bezplatného účtu]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install the Azure CLI]: /cli/azure/install-azure-cli
[az group create]: /cli/azure/group#az_group_create
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
