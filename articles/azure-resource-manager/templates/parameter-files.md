---
title: Vytvoření souboru parametrů
description: Vytvoří soubor parametrů pro předávání hodnot během nasazování šablony Azure Resource Manager.
ms.topic: conceptual
ms.date: 06/19/2020
ms.openlocfilehash: 8039b63978e52b69b0f8ffb4dd4e052769f3c5e6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082932"
---
# <a name="create-resource-manager-parameter-file"></a>Vytvořit soubor Správce prostředků parametrů

Místo předání parametrů jako vložených hodnot do skriptu může být snazší použít soubor JSON, který obsahuje hodnoty parametrů. Tento článek ukazuje, jak vytvořit soubor parametrů.

## <a name="parameter-file"></a>Soubor parametrů

Soubor parametrů má následující formát:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "value": "<second-value>"
    }
  }
}
```

Všimněte si, že hodnoty parametrů jsou uloženy jako prostý text v souboru parametrů. Tento přístup funguje pro hodnoty, které nejsou citlivé, jako je například určení SKU pro prostředek. Nefunguje u citlivých hodnot, jako jsou hesla. Pokud potřebujete předat citlivou hodnotu jako parametr, uložte hodnotu do trezoru klíčů a odkazujte na Trezor klíčů v souboru parametrů. Citlivá hodnota je bezpečně načtena během nasazení.

Následující soubor parametrů obsahuje hodnotu prostého textu a hodnotu, která je uložena v trezoru klíčů.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "reference": {
        "keyVault": {
          "id": "<resource-id-key-vault>"
        },
        "secretName": "<secret-name>"
      }
    }
  }
}
```

Další informace o použití hodnot z trezoru klíčů najdete v tématu [použití Azure Key Vault k předání hodnoty zabezpečeného parametru během nasazování](key-vault-parameter.md).

## <a name="define-parameter-values"></a>Definovat hodnoty parametrů

Chcete-li zjistit, jak definovat hodnoty parametrů, otevřete šablonu, kterou nasazujete. Podívejte se do části Parameters (parametry) v šabloně. Následující příklad ukazuje parametry ze šablony.

```json
"parameters": {
  "storagePrefix": {
    "type": "string",
    "maxLength": 11
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS",
    "Standard_ZRS",
    "Premium_LRS"
    ]
  }
}
```

První podrobná oznámení je název každého parametru. Hodnoty v souboru parametrů se musí shodovat s názvy.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Všimněte si typu parametru. Hodnoty v souboru parametrů musí mít stejné typy. Pro tuto šablonu můžete zadat oba parametry jako řetězce.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": ""
    },
    "storageAccountType": {
      "value": ""
    }
  }
}
```

Potom vyhledejte výchozí hodnotu. Pokud má parametr výchozí hodnotu, můžete zadat hodnotu, ale nemusíte ji mít.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "" // This value must be provided.
    },
    "storageAccountType": {
      "value": "" // This value is optional. Template will use default value if not provided.
    }
  }
}
```

Nakonec se podívejte na povolené hodnoty a libovolná omezení, jako je maximální délka. Sděluje vám rozsah hodnot, které můžete zadat pro parametr.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "storage"
    },
    "storageAccountType": {
      "value": "Standard_ZRS"
    }
  }
}
```

## <a name="parameter-type-formats"></a>Formáty typů parametrů

Následující příklad ukazuje formáty různých typů parametrů.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "exampleString": {
      "value": "test string"
    },
    "exampleInt": {
      "value": 4
    },
    "exampleBool": {
      "value": true
    },
    "exampleArray": {
      "value": [
        "value 1",
        "value 2"
      ]
    },
    "exampleObject": {
      "value": {
        "property1": "value1",
        "property2": "value2"
      }
    }
   }
}
```

## <a name="deploy-template-with-parameter-file"></a>Nasadit šablonu se souborem parametrů

Přečtěte si:

- [Nasazení prostředků pomocí šablon ARM a Azure CLI](./deploy-cli.md#parameters)
- [Nasazení prostředků pomocí šablon ARM a Azure PowerShell](./deploy-powershell.md#pass-parameter-values)

## <a name="file-name"></a>Název souboru

Obecná konvence pro pojmenovávání souboru parametrů je přidat **. parametry** do názvu šablony. Například pokud je vaše šablona pojmenována **azuredeploy.jsna**, váš soubor s parametrem má název **azuredeploy.parameters.js**. Tato konvence pojmenování vám pomůže zobrazit propojení mezi šablonou a parametry.

Chcete-li nasadit do různých prostředí, vytvořte více než jeden soubor parametrů. Při pojmenovávání souboru parametru přidejte způsob, jak identifikovat jeho použití. Například použijte **azuredeploy.parameters-dev.json** a **azuredeploy.parameters-prod.json**

## <a name="parameter-precedence"></a>Priorita parametru

Můžete použít vložené parametry a místní soubor parametrů ve stejné operaci nasazení. Můžete například zadat některé hodnoty v souboru s místním parametrem a přidat další hodnoty vložené během nasazování. Zadáte-li hodnoty pro parametr v místním souboru parametrů i v poli inline, má hodnota inline přednost.

Je možné použít externí soubor parametrů poskytnutím identifikátoru URI souboru. Když to uděláte, nebudete moci předat jiné hodnoty buď vložené, nebo z místního souboru. Všechny vložené parametry jsou ignorovány. Zadejte všechny hodnoty parametrů v externím souboru.

## <a name="parameter-name-conflicts"></a>Konflikty názvů parametrů

Pokud vaše šablona obsahuje parametr se stejným názvem jako jeden z parametrů v příkazu PowerShellu, PowerShell prezentuje parametr z vaší šablony s příponou **FromTemplate**. Například parametr s názvem **ResourceGroupName** v šabloně koliduje s parametrem **ResourceGroupName** v rutině [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) . Budete vyzváni k zadání hodnoty pro **ResourceGroupNameFromTemplate**. Této nejasnostem se můžete vyhnout použitím názvů parametrů, které se nepoužívají pro příkazy nasazení.


## <a name="next-steps"></a>Další kroky

- Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [parametry v šablonách Azure Resource Manager](template-parameters.md).
- Další informace o použití hodnot z trezoru klíčů najdete v tématu [použití Azure Key Vault k předání hodnoty zabezpečeného parametru během nasazování](key-vault-parameter.md).
- Další informace o parametrech naleznete [v tématu Parameters in Azure Resource Manager Templates](template-parameters.md).
