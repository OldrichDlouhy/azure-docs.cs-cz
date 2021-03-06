---
title: Funkce šablon – numerický
description: Popisuje funkce, které se použijí v šabloně Azure Resource Manager pro práci s čísly.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 00b44d971a487a0bbec27f3fc2d0746cedd6f874
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84677912"
---
# <a name="numeric-functions-for-arm-templates"></a>Číselné funkce pro šablony ARM

Správce prostředků poskytuje následující funkce pro práci s celými čísly v šabloně Azure Resource Manager (ARM):

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [Plovák](#float)
* [int](#int)
* [počet](#max)
* [dlouhé](#min)
* [střední](#mod)
* [mul](#mul)
* [jednotk](#sub)

## <a name="add"></a>add

`add(operand1, operand2)`

Vrátí součet dvou poskytnutých celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
|operand1 |Yes |int |První číslo, které se má přidat |
|Operand2 |Yes |int |Druhé číslo, které se má přidat |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo, které obsahuje součet parametrů.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) přidává dva parametry.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| addResult | Int | 8 |

## <a name="copyindex"></a>copyIndex

`copyIndex(loopName, offset)`

Vrátí index iterační smyčky.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| opakovat | No | řetězec | Název smyčky pro získání iterace. |
| posun |No |int |Číslo, které se má přidat do hodnoty iterace založené na nule |

### <a name="remarks"></a>Poznámky

Tato funkce se vždycky používá s objektem **copy** . Pokud není k dispozici žádná hodnota pro **posun**, vrátí se aktuální hodnota iterace. Hodnota iterace začíná nulou.

Vlastnost **Loop** umožňuje určit, zda copyIndex odkazuje na iteraci prostředku nebo iteraci vlastnosti. Pokud není k dispozici žádná hodnota pro **Loop**, použije se aktuální typ prostředku iterace. Zadejte hodnotu pro **Loop** při iteraci u vlastnosti.

Další informace o používání kopírování najdete v tématech:

* [Iterace prostředků v šablonách ARM](copy-resources.md)
* [Iterace vlastnosti v šablonách ARM](copy-properties.md)
* [Iterace proměnných v šablonách ARM](copy-variables.md)
* [Výstupní iterace v šablonách ARM](copy-outputs.md)

### <a name="example"></a>Příklad

Následující příklad ukazuje smyčku kopírování a hodnotu indexu, která je obsažena v názvu.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {}
}
```

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující aktuální index iterace.

## <a name="div"></a>div

`div(operand1, operand2)`

Vrátí celočíselnou část dvou poskytnutých celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Yes |int |Číslo, které se má rozdělit. |
| Operand2 |Yes |int |Číslo, které se používá k rozdělení. Nemůže být 0. |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující dělení.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) vydělí jeden parametr jiným parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| divResult | Int | 2 |

## <a name="float"></a>float

`float(arg1)`

Převede hodnotu na číslo s plovoucí desetinnou čárkou. Tuto funkci použijete jenom při předávání vlastních parametrů do aplikace, jako je například aplikace logiky.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |řetězec nebo int |Hodnota, která má být převedena na číslo s plovoucí desetinnou čárkou. |

### <a name="return-value"></a>Vrácená hodnota

Číslo s plovoucí desetinnou čárkou.

### <a name="example"></a>Příklad

Následující příklad ukazuje, jak použít float k předání parametrů do aplikace logiky:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

## <a name="int"></a>int

`int(valueToConvert)`

Převede zadanou hodnotu na celé číslo.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| valueToConvert |Yes |řetězec nebo int |Hodnota, která má být převedena na celé číslo. |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo převedené hodnoty.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) převede uživatelem zadanou hodnotu parametru na Integer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": {
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| intResult | Int | 4 |

## <a name="max"></a>max

`max (arg1)`

Vrátí maximální hodnotu z pole celých čísel nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |pole celých čísel nebo seznam celých čísel oddělených čárkami |Kolekce, která získá maximální hodnotu |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující maximální hodnotu z kolekce.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) ukazuje, jak použít Max s polem a seznam celých čísel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

## <a name="min"></a>min

`min (arg1)`

Vrátí minimální hodnotu z pole celých čísel nebo seznam celých čísel oddělených čárkami.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Yes |pole celých čísel nebo seznam celých čísel oddělených čárkami |Kolekce, která získá minimální hodnotu. |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující minimální hodnotu z kolekce.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) ukazuje, jak použít minimum s polem a seznam celých čísel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

## <a name="mod"></a>střední

`mod(operand1, operand2)`

Vrátí zbytek celočíselného dělení pomocí dvou poskytnutých celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Yes |int |Číslo, které se má rozdělit. |
| Operand2 |Yes |int |Číslo, které se používá k rozdělení, nemůže být 0. |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující zbytek.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) vrátí zbytek dělení jednoho parametru jiným parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| modResult | Int | 1 |

## <a name="mul"></a>mul

`mul(operand1, operand2)`

Vrací násobení dvou poskytnutých celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Yes |int |První číslo, které se má vynásobit |
| Operand2 |Yes |int |Druhé číslo, které se má vynásobit |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující násobení.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) vynásobí jeden parametr jiným parametrem.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

## <a name="sub"></a>jednotk

`sub(operand1, operand2)`

Vrátí odčítání dvou poskytnutých celých čísel.

### <a name="parameters"></a>Parametry

| Parametr | Požaduje se | Typ | Description |
|:--- |:--- |:--- |:--- |
| operand1 |Yes |int |Číslo, které je odečteno od. |
| Operand2 |Yes |int |Číslo, které se odečte. |

### <a name="return-value"></a>Vrácená hodnota

Celé číslo představující odčítání.

### <a name="example"></a>Příklad

Následující [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) odečte jeden parametr od jiného parametru.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Výstup z předchozího příkladu s výchozími hodnotami je:

| Name | Typ | Hodnota |
| ---- | ---- | ----- |
| podvýsledek | Int | 4 |

## <a name="next-steps"></a>Další kroky

* Popis sekcí v šabloně Azure Resource Manager najdete v tématu [pochopení struktury a syntaxe šablon ARM](template-syntax.md).
* Informace o iteraci zadaného počtu výskytů při vytváření typu prostředku najdete v tématu [vytvoření více instancí prostředků v Azure Resource Manager](copy-resources.md).
