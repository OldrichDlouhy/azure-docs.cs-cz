---
title: Odeslání událostí služby Blob Storage do webového koncového bodu – šablona
description: Pomocí Azure Event Grid a šablony Azure Resource Manager vytvořte účet úložiště objektů BLOB a přihlaste se k odběru událostí. Odeslat události do Webhooku
ms.date: 07/07/2020
ms.topic: quickstart
ms.openlocfilehash: 603d6bf11f2ec6988d52e69817bddf2fd3ccf3b3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86103309"
---
# <a name="route-blob-storage-events-to-web-endpoint-by-using-an-arm-template"></a>Směrování událostí služby Blob Storage do webového koncového bodu pomocí šablony ARM

Azure Event Grid je služba zpracování událostí pro cloud. V tomto článku použijete šablonu Azure Resource Manager (šablonu ARM) k vytvoření účtu úložiště BLOB, přihlášení k odběru událostí pro dané úložiště objektů BLOB a aktivaci události pro zobrazení výsledku. Obvykle odesíláte události do koncového bodu, který data události zpracuje a provede akce. Pro zjednodušení tohoto článku však budete události odesílat do webové aplikace, která shromažďuje a zobrazuje zprávy.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Pokud vaše prostředí splňuje požadavky a Vy jste obeznámeni s používáním šablon ARM, vyberte tlačítko **nasadit do Azure** . Šablona se otevře v Azure Portal.

[![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/), ještě než začnete.

### <a name="create-a-message-endpoint"></a>Vytvoření koncového bodu zpráv

Před přihlášením k odběru událostí úložiště objektů blob vytvoříme koncový bod pro zprávy události. Koncový bod obvykle provede akce na základě dat události. Pro zjednodušení tohoto rychlého startu nasadíte [předem připravenou webovou aplikaci](https://github.com/Azure-Samples/azure-event-grid-viewer), která zobrazuje zprávy události. Nasazené řešení zahrnuje plán služby App Service, webovou aplikaci App Service a zdrojový kód z GitHubu.

1. Vyberte **Nasadit do Azure** a nasaďte řešení do svého předplatného. Na webu Azure Portal zadejte hodnoty pro parametry.

    [Nasazení do Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json)
1. Dokončení nasazení může trvat několik minut. Po úspěšném nasazení si webovou aplikaci prohlédněte, abyste se ujistili, že funguje. Ve webovém prohlížeči přejděte na: `https://<your-site-name>.azurewebsites.net`

1. Zobrazí se web, na který se však zatím neodeslaly žádné události.

   ![Zobrazení nového webu](./media/blob-event-quickstart-portal/view-site.png)

## <a name="review-the-template"></a>Kontrola šablony

Šablona použitá v tomto rychlém startu je ze [šablon Azure pro rychlý Start](https://azure.microsoft.com/resources/templates/101-event-grid-subscription-and-storage/).

:::code language="json" source="~/quickstart-templates/101-event-grid-subscription-and-storage/azuredeploy.json" range="1-91" highlight="40-85":::

V šabloně jsou definované dva prostředky Azure:

* [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageaccounts): vytvořte účet Azure Storage.
* [**Microsoft. EventGrid/systemTopics**](/azure/templates/microsoft.eventgrid/systemtopics): Vytvořte systémové téma se zadaným názvem pro účet úložiště.
* [**Microsoft. EventGrid/systemTopics/eventSubscriptions**](/azure/templates/microsoft.eventgrid/systemtopics/eventsubscriptions): vytvořte předplatné Azure Event Grid pro systémové téma.

## <a name="deploy-the-template"></a>Nasazení šablony

1. Vyberte následující odkaz pro přihlášení do Azure a otevřete šablonu. Šablona vytvoří Trezor klíčů a tajný klíč.

    [![Nasazení do Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-event-grid-subscription-and-storage%2Fazuredeploy.json)

2. Zadejte **koncový bod**: zadejte adresu URL webové aplikace a přidejte `api/updates` ji na domovskou stránku URL.
3. Vyberte **koupit** a šablonu nasaďte.

  Azure Portal se tady používá k nasazení šablony. Můžete také použít Azure PowerShell, Azure CLI a REST API. Další informace o dalších metodách nasazení najdete v tématu [Nasazení šablon](../azure-resource-manager/templates/deploy-powershell.md).

> [!NOTE]
> Další Azure Event Grid ukázek šablon najdete [tady](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid&pageNumber=1&sort=Popular).

## <a name="validate-the-deployment"></a>Ověření nasazení

Podívejte se na webovou aplikaci znovu a všimněte si, že do ní byla odeslána událost ověření odběru. Vyberte ikonu oka a rozbalte data události. Služba Event Grid odešle událost ověření, aby koncový bod mohl ověřit, že data události chce přijímat. Webová aplikace obsahuje kód pro ověření odběru.

![Zobrazení události odběru](./media/blob-event-quickstart-portal/view-subscription-event.png)

Nyní aktivujeme událost, abychom viděli, jak služba Event Grid distribuuje zprávu do vašeho koncového bodu.

Událost pro úložiště objektů blob aktivujete nahráním souboru. Soubor nemusí obsahovat žádný konkrétní obsah. V tomto článku se předpokládá, že máte soubor testfile.txt, ale můžete použít jakýkoli soubor.

Když nahrajete soubor do úložiště objektů BLOB v Azure, Event Grid pošle zprávu na koncový bod, který jste nakonfigurovali při přihlášení k odběru. Zpráva je ve formátu JSON a obsahuje pole s jednou nebo více událostmi. V následujícím příkladu zpráva JSON obsahuje pole s jednou událostí. Zobrazte svou webovou aplikaci a všimněte si, že se přijala událost vytvoření objektu blob.

![Zobrazení výsledků](./media/blob-event-quickstart-portal/view-results.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, [odstraňte skupinu prostředků](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group
).

## <a name="next-steps"></a>Další kroky

Další informace o šablonách Azure Resource Manager najdete v následujících článcích:

* [Dokumentace k Azure Resource Manager](/azure/azure-resource-manager)
* [Definování prostředků v šablonách Azure Resource Manager](/azure/templates/)
* [Šablony pro rychlý Start Azure](https://azure.microsoft.com/resources/templates/)
* [Šablony Azure Event Grid](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Eventgrid).
