---
title: Povolit Update Management pomocí šablony Azure Resource Manager | Microsoft Docs
description: V tomto článku se dozvíte, jak používat šablonu Azure Resource Manager k povolení Update Management.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 06/10/2020
ms.openlocfilehash: ad9029b44ffb0c98bad58bbf012eb19d084d5446
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185751"
---
# <a name="enable-update-management-using-azure-resource-manager-template"></a>Povolení Update Managementu s využitím šablony Azure Resource Manageru

K povolení funkce Update Management Azure Automation ve vaší skupině prostředků můžete použít [šablonu Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md) . Tento článek poskytuje ukázkovou šablonu, která automatizuje následující:

* Vytváření pracovního prostoru Azure Monitor Log Analytics
* Vytvoření účtu Azure Automation.
* Propojení účtu Automation s pracovním prostorem Log Analytics, pokud ještě není propojené.
* Povolování Update Management.

Šablona neautomatizuje povolení Update Management na jednom nebo několika virtuálních počítačích Azure nebo mimo Azure.

Pokud už máte pracovní prostor Log Analytics a účet Automation je nasazený v podporované oblasti v rámci vašeho předplatného, nejsou propojené. Pomocí této šablony se úspěšně vytvoří odkaz a nasadí se Update Management.

## <a name="api-versions"></a>Verze rozhraní API

V následující tabulce jsou uvedeny verze rozhraní API pro prostředky použité v této šabloně.

| Resource | Typ prostředku | Verze rozhraní API |
|:---|:---|:---|
| Pracovní prostor | pracovní prostory | 2020-03-01 – Preview |
| Účet Automation | automatizace | 2018-06-30 | 
| Řešení | Řešení | 2015-11-01 – Preview |

## <a name="before-using-the-template"></a>Před použitím šablony

Pokud se rozhodnete nainstalovat a používat PowerShell místně, vyžaduje tento článek Azure PowerShell AZ Module. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable Az`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-az-ps). Pokud používáte PowerShell místně, je také potřeba spustit [příkaz Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) a vytvořit připojení k Azure. Při použití Azure PowerShell nasazení používá [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment).

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít spuštěnou verzi Azure CLI 2.1.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Pomocí Azure CLI toto nasazení používá příkaz [AZ Group Deployment Create](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). 

Šablona JSON je nakonfigurovaná tak, aby vás vyzvala k těmto akcím:

* Název pracovního prostoru
* Oblast, ve které se má pracovní prostor vytvořit.
* Povolení oprávnění prostředků nebo pracovních prostorů.
* Název účtu Automation.
* Oblast, ve které má být účet vytvořen.

Šablona JSON určuje výchozí hodnotu pro ostatní parametry, které se pravděpodobně použijí pro standardní konfiguraci ve vašem prostředí. Šablonu můžete uložit v účtu služby Azure Storage pro sdílený přístup ve vaší organizaci. Další informace o práci se šablonami najdete v tématu [nasazení prostředků pomocí šablon Správce prostředků a Azure CLI](../azure-resource-manager/templates/deploy-cli.md).

Následující parametry v šabloně jsou nastaveny s výchozí hodnotou pro Log Analytics pracovní prostor:

* SKU – výchozí hodnota pro novou cenovou úroveň za GB vydanou v cenovém modelu z dubna 2018
* uchovávání dat – výchozí hodnota je 30 dní.

>[!WARNING]
>Pokud vytváříte nebo konfigurujete pracovní prostor Log Analytics v předplatném, které se přihlásilo k novému cenovému modelu od dubna 2018, bude jediná platná Log Analytics cenová úroveň **PerGB2018**.
>

Šablona JSON určuje výchozí hodnotu pro ostatní parametry, které by pravděpodobně byly použity jako standardní konfigurace ve vašem prostředí. Šablonu můžete uložit v účtu služby Azure Storage pro sdílený přístup ve vaší organizaci. Další informace o práci se šablonami najdete v tématu [nasazení prostředků pomocí šablon Správce prostředků a Azure CLI](../azure-resource-manager/templates/deploy-cli.md).

Pokud se Azure Automation a Azure Monitor, je důležité pochopit následující podrobnosti o konfiguraci, aby se předešlo chybám při pokusu o vytvoření, konfiguraci a používání Log Analyticsho pracovního prostoru propojeného s vaším novým účtem služby Automation.

* Přečtěte si [Další podrobnosti](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) , abyste plně pochopili možnosti konfigurace pracovního prostoru, jako je režim řízení přístupu, cenová úroveň, uchování a úroveň rezervace kapacity.

* Vzhledem k tomu, že jsou podporovány pouze některé oblasti pro propojení Log Analyticsho pracovního prostoru a účtu Automation v rámci předplatného, zkontrolujte [mapování pracovních prostorů](how-to/region-mappings.md) a určete podporované oblasti jako vložené nebo v souboru parametrů.

* Pokud Azure Monitor protokoly a ještě nemáte nasazený pracovní prostor, měli byste si projít průvodce [návrhem pracovního prostoru](../azure-monitor/platform/design-logs-deployment.md) , kde najdete informace o řízení přístupu a pochopení strategií implementace návrhu, které doporučujeme pro vaši organizaci.

## <a name="deploy-template"></a>Nasazení šablony

1. Zkopírujte a vložte do souboru následující syntaxi JSON:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "Workspace name"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "resourcePermissions": {
              "type": "bool",
              "metadata": {
                "description": "true to use resource or workspace permissions. false to require workspace permissions."
              }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
        }
    },
    "resources": [
        {
        "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2020-03-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "name": "[parameters('sku')]",
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('location')]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2018-06-30",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
        },
        {
            "apiVersion": "2020-03-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
      ]
    }
    ```

2. Upravte šablonu tak, aby splňovala vaše požadavky. Místo předání parametrů jako vložené hodnoty zvažte vytvoření [souboru parametrů správce prostředků](../azure-resource-manager/templates/parameter-files.md) .

3. Uložte tento soubor do místní složky jako **deployUMSolutiontemplate.jsna**.

4. Jste připraveni k nasazení této šablony. Můžete použít buď PowerShell, nebo rozhraní příkazového řádku Azure CLI. Po zobrazení výzvy k zadání pracovního prostoru a názvu účtu Automation zadejte název, který bude globálně jedinečný v rámci všech předplatných Azure.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    Dokončení nasazení může trvat několik minut. Po dokončení se zobrazí zpráva podobná následující, která obsahuje výsledek:

    ![Příklad výsledku po dokončení nasazení](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Další kroky

* Pokud chcete použít Update Management pro virtuální počítače, přečtěte si téma [Správa aktualizací a oprav pro virtuální počítače Azure](automation-tutorial-update-management.md).

* Pokud už nepotřebujete pracovní prostor Log Analytics, přečtěte si pokyny v tématu [zrušení propojení pracovního prostoru s účtem Automation pro Update Management](automation-unlink-workspace-update-management.md).

* Postup odstranění virtuálních počítačů z Update Management najdete v tématu [Odebrání virtuálních počítačů z Update Management](automation-remove-vms-from-update-management.md).
