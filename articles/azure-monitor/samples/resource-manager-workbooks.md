---
title: Ukázky šablon Správce prostředků pro sešity
description: Ukázka Azure Resource Manager šablon pro nasazení Azure Monitor sešitů.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/18/2020
ms.openlocfilehash: 72e6e085135c07e962dea6253af7d40333295a86
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854496"
---
# <a name="resource-manager-template-samples-for-workbooks-in-azure-monitor"></a>Ukázky šablon Správce prostředků pro sešity v Azure Monitor
Tento článek obsahuje vzorové [Azure Resource Manager šablony](../../azure-resource-manager/templates/template-syntax.md) pro vytváření sešitů v Azure monitor. Každá ukázka obsahuje soubor šablony a soubor parametrů s ukázkovými hodnotami, které se mají poskytnout šabloně.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

Sešity můžou být složité, takže typickou strategií je vytvořit sešit v Azure Portal a potom vygenerovat šablonu Správce prostředků. Podrobnosti o této metodě najdete v tématu [šablony Azure Resource Manager pro nasazení sešitů](../platform/workbooks-automate.md).

## <a name="create-a-workbook"></a>Vytvořit sešit
Následující příklad vytvoří jednoduchý sešit.


### <a name="template-file"></a>Soubor šablony

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName":  {             
            "type":"string",
            "defaultValue": "My Workbook",
            "metadata": {
                "description": "The friendly name for the workbook that is used in the Gallery or Saved List. Needs to be unique in the scope of the resource group and source" 
            }
        },
        "workbookType":  {             
            "type":"string",
            "defaultValue": "tsg",
            "metadata": {
                "description": "The gallery that the workbook will been shown under. Supported values include workbook, `tsg`, Azure Monitor, etc." 
            }
        },
        "workbookSourceId":  {             
            "type":"string",
            "defaultValue": "<insert-your-resource-id-here>",
            "metadata": {
                "description": "The id of resource instance to which the workbook will be associated" 
            }
        },
        "workbookId": {
            "type":"string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "The unique guid for this workbook instance" 
            }
        }
    },    
    "resources": [
        {
            "name": "[parameters('workbookId')]",
            "type": "Microsoft.Insights/workbooks",
            "location": "[resourceGroup().location]",
            "kind": "shared",
            "apiVersion": "2018-06-17-preview",
            "dependsOn": [],
            "properties": {
                "displayName": "[parameters('workbookDisplayName')]",
                "serializedData": "{\"version\":\"Notebook/1.0\",\"items\":[{\"type\":1,\"content\":\"{\\\"json\\\":\\\"Hello World!\\\"}\",\"conditionalVisibility\":null}],\"isLocked\":false}",
                "version": "1.0",
                "sourceId": "[parameters('workbookSourceId')]",
                "category": "[parameters('workbookType')]"
            }
        }
    ],
    "outputs": {
        "workbookId": {
            "type": "string",
            "value": "[resourceId( 'Microsoft.Insights/workbooks', parameters('workbookId'))]"
        }
    }
}
```

### <a name="parameter-file"></a>Soubor parametrů

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName": {
            "value": "Sample Hello World workbook"
        },
      "workbookType": {
        "value": "workbook"
      },
      "workbookSourceId": {
        "value": "Azure Monitor"
      }
    }
}
```

## <a name="next-steps"></a>Další kroky

* [Získejte další ukázkové šablony pro Azure monitor](resource-manager-samples.md).
* [Přečtěte si další informace o skupinách akcí](../platform/workbooks-overview.md).
