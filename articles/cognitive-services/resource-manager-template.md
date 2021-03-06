---
title: 'Rychlý Start: vytvoření prostředku Azure Cognitive Services pomocí šablon ARM | Microsoft Docs'
description: Začínáme s používáním šablony Azure Resource Manager k nasazení prostředku Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 07/27/2020
ms.author: aahi
ms.custom: subject-armqs
ms.openlocfilehash: 9ecbd7778480d37fb0a0cf135d3cc5db48bf2add
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323651"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-an-arm-template"></a>Rychlý Start: vytvoření prostředku Cognitive Services pomocí šablony ARM

Pomocí tohoto článku můžete vytvořit a nasadit prostředek Cognitive Services pomocí šablony Azure Resource Manager (šablona ARM). Tento prostředek s více službami vám umožní:
* Přístup k více Cognitive Servicesům Azure s jedním klíčem a koncovým bodem.
* Konsolidujte účtování ze služeb, které používáte.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Pokud vaše prostředí splňuje požadavky a jste obeznámeni s používáním šablon ARM, vyberte tlačítko **Nasazení do Azure**. Šablona se otevře v prostředí Azure Portal.

[![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cognitive-services-universalkey%2Fazuredeploy.json)

## <a name="prerequisites"></a>Požadavky

* Pokud nemáte předplatné Azure, [Vytvořte si ho zdarma](https://azure.microsoft.com/free/cognitive-services).

## <a name="review-the-template"></a>Kontrola šablony

Šablona použitá v tomto rychlém startu je jednou z [šablon pro rychlý start Azure](https://azure.microsoft.com/resources/templates/101-cognitive-services-universalkey/).

:::code language="json" source="~/quickstart-templates/101-cognitive-services-universalkey/azuredeploy.json" highlight="27-41":::

V této šabloně je definovaný jeden prostředek Azure:
* [Microsoft. cognitiveservices Account/Accounts](https://docs.microsoft.com/azure/templates/microsoft.cognitiveservices/accounts): vytvoří prostředek Cognitive Services.

## <a name="deploy-the-template"></a>Nasazení šablony

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Klikněte na tlačítko **nasadit do Azure** .

    [![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cognitive-services-universalkey%2Fazuredeploy.json)

2. Zadejte následující hodnoty.
    
    |Hodnota  |Popis  |
    |---------|---------|
    | **Předplatné** | Vyberte předplatné služby Azure. |
    | **Skupina prostředků** | Vyberte **vytvořit nový**, zadejte jedinečný název skupiny prostředků a pak klikněte na **OK**. |
    | **Oblast** | Vyberte oblast.  Například **východní USA** |
    | **Název služby pro rozpoznávání** | Nahraďte jedinečným názvem prostředku. Při ověřování nasazení budete potřebovat název v další části. |
    | **Umístění** | Nahraďte výše použitou oblastí. |
    | **Skladové** | [Cenová úroveň](https://azure.microsoft.com/pricing/details/cognitive-services/) pro váš prostředek. |
    
    :::image type="content" source="media/arm-template/universal-key-portal-template.png" alt-text="Obrazovka pro vytvoření prostředku":::

3. Vyberte **Zkontrolovat a vytvořit** a potom **Vytvořit**. Po úspěšném nasazení prostředku se zvýrazní tlačítko **Přejít na prostředek** .

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

> [!NOTE]
> `az deployment group`Create vyžaduje Azure CLI verze 2,6 nebo novější. Pro zobrazení typu verze `az --version` . Další informace najdete v [dokumentaci](https://docs.microsoft.com/cli/azure/deployment/group).

Spusťte následující skript pomocí rozhraní příkazového řádku Azure (CLI) [na vašem místním počítači](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)nebo v prohlížeči pomocí tlačítka **vyzkoušet** . Zadejte název a umístění (například `centralus` ) pro novou skupinu prostředků a šablona ARM se použije k nasazení prostředku Cognitive Services v rámci něj. Zapamatujte si název, který používáte. Později ji budete používat k ověření nasazení.


```azurecli-interactive
read -p "Enter a name for your new resource group:" resourceGroupName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cognitive-services-universalkey/azuredeploy.json" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri  $templateUri &&
echo "Press [ENTER] to continue ..." &&
read
```

---

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]


## <a name="validate-the-deployment"></a>Ověření nasazení

# <a name="portal"></a>[Azure Portal](#tab/portal)

Po dokončení nasazení budete moci kliknout na tlačítko **Přejít k prostředku** a zobrazit nový prostředek. Můžete také vyhledat skupinu prostředků podle:

1. V navigační nabídce vlevo vyberte **skupiny prostředků** .
2. Výběr názvu skupiny prostředků.

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

Pomocí Azure CLI spusťte následující skript a zadejte název skupiny prostředků, kterou jste předtím vytvořili.

```azurecli-interactive
echo "Enter the resource group where the Cognitive Services resource exists:" &&
read resourceGroupName &&
az cognitiveservices account list -g $resourceGroupName
```

---


## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete vyčistit a odebrat předplatné Cognitive Services, můžete prostředek nebo skupinu prostředků odstranit. Odstraněním skupiny prostředků dojde také k odstranění všech dalších prostředků obsažených ve skupině.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Na webu Azure Portal rozbalením nabídky na levé straně otevřete nabídku služeb a zvolte **Skupiny prostředků**. Zobrazí se seznam skupin prostředků.
2. Vyhledejte skupinu prostředků obsahující prostředek, který chcete odstranit.
3. Klikněte pravým tlačítkem na výpis skupiny prostředků. Vyberte **Odstranit skupinu prostředků** a potvrďte tuto akci.

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

Pomocí Azure CLI spusťte následující skript a zadejte název skupiny prostředků, kterou jste předtím vytvořili. 

```azurecli-interactive
echo "Enter the resource group name, for deletion:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>Další kroky

* [Ověřování požadavků do Azure Cognitive Services](authentication.md)
* [Co je Azure Cognitive Services?](Welcome.md)
* [Podpora přirozeného jazyka](language-support.md)
* [Podpora kontejneru Docker](cognitive-services-container-support.md)
