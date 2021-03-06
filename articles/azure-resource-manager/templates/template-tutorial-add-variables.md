---
title: Kurz – přidání proměnné do šablony
description: K zjednodušení syntaxe přidejte proměnné do šablony Azure Resource Manager.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b1df86e5b593edec784de21e21a4399274d820bb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80411687"
---
# <a name="tutorial-add-variables-to-your-arm-template"></a>Kurz: Přidání proměnných do šablony ARM

V tomto kurzu se dozvíte, jak přidat proměnnou do šablony Azure Resource Manager (ARM). Proměnné zjednodušují vaše šablony tím, že umožňují napsat výraz jednou a znovu ho použít v celé šabloně. Dokončení tohoto kurzu trvá **7 minut** .

## <a name="prerequisites"></a>Požadavky

Doporučujeme, abyste dokončili [kurz týkající se funkcí](template-tutorial-add-functions.md), ale není to nutné.

Musíte mít Visual Studio Code s rozšířením Správce prostředků Tools a buď Azure PowerShell, nebo v rozhraní příkazového řádku Azure. Další informace najdete v tématu [nástroje šablon](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Zkontrolovat šablonu

Na konci předchozího kurzu má vaše šablona následující JSON:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json":::

Parametr názvu účtu úložiště je nevratný, protože je nutné zadat jedinečný název. Pokud jste dokončili předchozí kurzy v této sérii, budete pravděpodobně už vás unavuje jedinečný název. Tento problém vyřešíte tak, že přidáte proměnnou, která vytvoří jedinečný název pro účet úložiště.

## <a name="use-variable"></a>Použít proměnnou

Následující příklad zvýrazní změny a přidá proměnnou do šablony, která vytvoří jedinečný název účtu úložiště. Zkopírujte celý soubor a nahraďte šablonu jeho obsahem.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.json" range="1-47" highlight="5-9,29-31,36":::

Všimněte si, že obsahuje proměnnou s názvem **uniqueStorageName**. Tato proměnná používá čtyři funkce k vytvoření řetězcové hodnoty.

Již jste obeznámeni s funkcí [Parameters](template-functions-deployment.md#parameters) , takže ji nebudeme posuzovat.

Také jste obeznámeni se funkcí [Resource](template-functions-resource.md#resourcegroup) . V takovém případě získáte vlastnost **ID** místo vlastnosti **Location** , jak je znázorněno v předchozím kurzu. Vlastnost **ID** vrátí úplný identifikátor skupiny prostředků, včetně ID předplatného a názvu skupiny prostředků.

Funkce [uniqueString](template-functions-string.md#uniquestring) vytvoří hodnotu hash znaku 13 znaků. Vrácená hodnota je určena parametry, které předáte. V tomto kurzu použijete ID skupiny prostředků jako vstup pro hodnotu hash. To znamená, že můžete tuto šablonu nasadit do různých skupin prostředků a získat jinou jedinečnou řetězcovou hodnotu. Pokud však nasadíte do stejné skupiny prostředků, získáte stejnou hodnotu.

Funkce [Concat](template-functions-string.md#concat) přebírá hodnoty a kombinuje je. Pro tuto proměnnou přebírá řetězec z parametru a řetězce z funkce uniqueString a kombinuje je do jednoho řetězce.

Parametr **storagePrefix** vám umožní předat předponu, která vám pomůže identifikovat účty úložiště. Můžete vytvořit vlastní zásadu vytváření názvů, která usnadňuje identifikaci účtů úložiště po nasazení z dlouhého seznamu prostředků.

Nakonec si všimněte, že název úložiště je nyní nastaven na proměnnou namísto parametru.

## <a name="deploy-template"></a>Nasazení šablony

Pojďme šablonu nasadit. Nasazení této šablony je snazší než u předchozích šablon, protože zadáváte pouze předponu názvu úložiště.

Pokud jste ještě nevytvořili skupinu prostředků, přečtěte si téma [Vytvoření skupiny prostředků](template-tutorial-create-first-template.md#create-resource-group). V příkladu se předpokládá, že jste nastavili proměnnou **templateFile** na cestu k souboru šablony, jak je znázorněno v [prvním kurzu](template-tutorial-create-first-template.md#deploy-template).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnamevariable `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Pokud chcete spustit tento příkaz nasazení, musíte mít [nejnovější verzi](/cli/azure/install-azure-cli) rozhraní příkazového řádku Azure CLI.

```azurecli
az deployment group create \
  --name addnamevariable \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

> [!NOTE]
> Pokud se nasazení nepovedlo, použijte k zobrazení protokolů ladění přepínač **ladění** s příkazem nasazení.  Můžete také použít **podrobný** přepínač k zobrazení úplných protokolů ladění.

## <a name="verify-deployment"></a>Ověření nasazení

Nasazení můžete ověřit prozkoumáním skupiny prostředků z Azure Portal.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
1. V nabídce vlevo vyberte **skupiny prostředků**.
1. Vyberte skupinu prostředků, do které jste nasadili.
1. Vidíte, že byl nasazen prostředek účtu úložiště. Název účtu úložiště je **uložený** spolu s řetězcem náhodných znaků.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud se chystáte pokračovat k dalšímu kurzu, nemusíte odstranit skupinu prostředků.

Pokud nyní zastavíte, budete možná chtít vyčistit prostředky, které jste nasadili, odstraněním skupiny prostředků.

1. Z Azure Portal v nabídce vlevo vyberte **Skupina prostředků** .
2. Do pole **Filtrovat podle názvu** zadejte název skupiny prostředků.
3. Vyberte název skupiny prostředků.
4. V horní nabídce vyberte **Odstranit skupinu prostředků** .

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste přidali proměnnou, která pro účet úložiště vytvoří jedinečný název. V dalším kurzu vrátíte hodnotu z nasazeného účtu úložiště.

> [!div class="nextstepaction"]
> [Přidat výstupy](template-tutorial-add-outputs.md)
