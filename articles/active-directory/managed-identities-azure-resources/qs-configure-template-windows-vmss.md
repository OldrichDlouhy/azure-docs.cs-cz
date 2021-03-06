---
title: Konfigurace šablony pro použití spravovaných identit ve službě Virtual Machine Scale Sets – Azure AD
description: Podrobné pokyny pro konfiguraci spravovaných identit pro prostředky Azure v sadě škálování virtuálního počítače pomocí šablony Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5afb11a275275ac49178b30929d7896c8a082591
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85609006"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-virtual-machine-scale-using-a-template"></a>Konfigurace spravovaných identit pro prostředky Azure na škálování virtuálního počítače Azure pomocí šablony

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Spravované identity pro prostředky Azure poskytují služby Azure s automaticky spravovanou identitou v Azure Active Directory. Tuto identitu můžete použít k ověření pro libovolnou službu, která podporuje ověřování Azure AD, a to bez nutnosti přihlašovacích údajů ve vašem kódu.

V tomto článku se dozvíte, jak provádět následující spravované identity pro operace prostředků Azure v sadě škálování virtuálních počítačů Azure pomocí Azure Resource Manager šablony nasazení:
- Povolení a zakázání spravované identity přiřazené systémem v sadě škálování virtuálních počítačů Azure
- Přidání a odebrání spravované identity přiřazené uživatelem v sadě škálování virtuálních počítačů Azure

## <a name="prerequisites"></a>Požadavky

- Pokud neznáte spravované identity prostředků Azure, přečtěte si [část přehled](overview.md). **Nezapomeňte si projít [rozdíl mezi spravovanou identitou přiřazenou systémem a uživatelem](overview.md#managed-identity-types)**.
- Pokud ještě nemáte účet Azure, [zaregistrujte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než budete pokračovat.
- K provedení operací správy v tomto článku potřebuje váš účet následující přiřazení řízení přístupu na základě role Azure:

    > [!NOTE]
    > Nevyžadují se žádné další přiřazení role adresáře Azure AD.

    - [Přispěvatel virtuálních počítačů](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) , aby vytvořil sadu škálování virtuálního počítače a povolil a odebral systémovou a/nebo uživatelsky spravovanou identitu ze sady škálování virtuálního počítače.
    - Role [Přispěvatel spravovaných identit](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) k vytvoření spravované identity přiřazené uživatelem.
    - Role [operátora spravované identity](/azure/role-based-access-control/built-in-roles#managed-identity-operator) k přiřazení a odebrání spravované identity přiřazené uživatelem z a do sady škálování virtuálního počítače.

## <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Stejně jako u Azure Portal a skriptování poskytují [Azure Resource Manager](../../azure-resource-manager/management/overview.md) šablony možnost nasazení nových nebo upravených prostředků definovaných skupinou prostředků Azure. K dispozici je několik možností pro úpravu a nasazení šablony, včetně místních i na portálu, včetně:

   - Použití [vlastní šablony z Azure Marketplace](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template), což vám umožňuje vytvořit zcela novou šablonu, nebo ji založit na stávající společné nebo [rychlé šabloně](https://azure.microsoft.com/documentation/templates/).
   - Odvození z existující skupiny prostředků exportováním šablony z [původního nasazení](../../azure-resource-manager/templates/export-template-portal.md)nebo z [aktuálního stavu nasazení](../../azure-resource-manager/templates/export-template-portal.md).
   - Použití místního [editoru JSON (například vs Code)](../../azure-resource-manager/resource-manager-create-first-template.md)a následného nahrávání a nasazování pomocí PowerShellu nebo rozhraní příkazového řádku.
   - Použití [projektu skupiny prostředků Azure](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) sady Visual Studio k vytvoření a nasazení šablony.  

Bez ohledu na zvolenou možnost je syntaxe šablony stejná při počátečním nasazení a opětovném nasazení. Povolení spravovaných identit pro prostředky Azure na novém nebo existujícím virtuálním počítači se provádí stejným způsobem. Ve výchozím nastavení Azure Resource Manager také [přírůstkovou aktualizaci](../../azure-resource-manager/templates/deployment-modes.md) nasazení.

## <a name="system-assigned-managed-identity"></a>Spravovaná identita přiřazená systémem

V této části povolíte nebo zakážete spravovanou identitu přiřazenou systémem pomocí šablony Azure Resource Manager.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-an-existing-virtual-machine-scale-set"></a>Během vytváření povolit spravovanou identitu přiřazenou systémem a vytvořit sadu škálování virtuálních počítačů nebo existující sadu škálování virtuálního počítače

1. Bez ohledu na to, jestli se k Azure přihlašujete místně nebo prostřednictvím Azure Portal, použijte účet, který je přidružený k předplatnému Azure, které obsahuje sadu škálování virtuálního počítače.
2. Pokud chcete povolit spravovanou identitu přiřazenou systémem, načtěte šablonu do editoru, vyhledejte `Microsoft.Compute/virtualMachinesScaleSets` prostředek zájmu v části prostředky a přidejte `identity` vlastnost na stejnou úroveň jako `"type": "Microsoft.Compute/virtualMachinesScaleSets"` vlastnost. Použijte následující syntaxi:

   ```JSON
   "identity": {
       "type": "SystemAssigned"
   }
   ```

> [!NOTE]
> Můžete volitelně zřídit spravované identity pro rozšíření sady škálování virtuálních počítačů Azure, a to tak, že je zadáte v `extensionProfile` elementu šablony. Tento krok je nepovinný, protože můžete použít koncový bod identity Azure Instance Metadata Service (IMDS) a načíst taky tokeny.  Další informace najdete v tématu [migrace z rozšíření virtuálního počítače do Azure IMDS pro ověřování](howto-migrate-vm-extension.md).


4. Až budete hotovi, do oddílu prostředků vaší šablony by se měly přidat následující oddíly, které by měly vypadat takto:

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            }
                        ]
                    }
                }
            }
        }
    ]
   ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Zakažte spravovanou identitu přiřazenou systémem ze sady škálování virtuálních počítačů Azure.

Pokud máte sadu škálování virtuálního počítače, která už nepotřebuje spravovanou identitu přiřazenou systémem:

1. Bez ohledu na to, jestli se k Azure přihlašujete místně nebo prostřednictvím Azure Portal, použijte účet, který je přidružený k předplatnému Azure, které obsahuje sadu škálování virtuálního počítače.

2. Načtěte šablonu do [editoru](#azure-resource-manager-templates) a vyhledejte `Microsoft.Compute/virtualMachineScaleSets` prostředek zájmu v rámci `resources` oddílu. Pokud máte virtuální počítač, který má pouze spravovanou identitu přiřazenou systémem, můžete ho zakázat změnou typu identity na `None` .

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2018-06-01**

   Pokud vaše apiVersion je `2018-06-01` a váš virtuální počítač má spravované identity přiřazené systémem i uživatelem, odeberte `SystemAssigned` z typu identity a zachovejte `UserAssigned` spolu s hodnotami slovníku userAssignedIdentities.

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2018-06-01**

   Pokud vaše apiVersion je `2017-12-01` a vaše sada škálování virtuálního počítače obsahuje spravované identity systému i uživatele, odeberte `SystemAssigned` z typu identity a zachovejte `UserAssigned` spolu s `identityIds` polem spravovaných identit přiřazených uživatelem.



   Následující příklad ukazuje, jak odebrat spravovanou identitu přiřazenou systémem ze sady škálování virtuálních počítačů bez přiřazených uživatelem definovaných identit:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-managed-identity"></a>Spravovaná identita přiřazená uživatelem

V této části přiřadíte uživatelem přiřazenou identitu pro sadu škálování virtuálního počítače pomocí šablony Azure Resource Manager.

> [!Note]
> Chcete-li vytvořit uživatelem přiřazenou identitu pomocí šablony Azure Resource Manager, přečtěte si téma [Vytvoření uživatelem přiřazené spravované identity](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-a-virtual-machine-scale-set"></a>Přiřazení spravované identity přiřazené uživateli k sadě škálování virtuálního počítače

1. V rámci `resources` elementu přidejte následující položku pro přiřazení spravované identity přiřazené uživateli k sadě škálování virtuálního počítače.  Nezapomeňte nahradit `<USERASSIGNEDIDENTITY>` názvem uživatelsky přiřazené spravované identity, kterou jste vytvořili.

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2018-06-01**

   Pokud je vaše apiVersion `2018-06-01` , vaše uživatelem přiřazené spravované identity jsou uložené ve `userAssignedIdentities` formátu slovníku a `<USERASSIGNEDIDENTITYNAME>` hodnota musí být uložená v proměnné definované v `variables` části šablony.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }

   }
   ```   

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2017-12-01**

   Pokud jste `apiVersion` `2017-12-01` nebo dříve, vaše uživatelem přiřazené spravované identity jsou uloženy v `identityIds` poli a `<USERASSIGNEDIDENTITYNAME>` hodnota musí být uložena v proměnné definované v části proměnné v šabloně.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ```
> [!NOTE]
> Můžete volitelně zřídit spravované identity pro rozšíření sady škálování virtuálních počítačů Azure, a to tak, že je zadáte v `extensionProfile` elementu šablony. Tento krok je nepovinný, protože můžete použít koncový bod identity Azure Instance Metadata Service (IMDS) a načíst taky tokeny.  Další informace najdete v tématu [migrace z rozšíření virtuálního počítače do Azure IMDS pro ověřování](howto-migrate-vm-extension.md).

3. Po dokončení by šablona měla vypadat nějak takto:

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            }
                        ]
                    }
                }
            }
        }
    ]
   ```

   **Microsoft. COMPUTE/virtualMachines API verze 2017-12-01**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)    
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
                                "properties": {
                                  "publisher": "Microsoft.ManagedIdentity",
                                  "type": "ManagedIdentityExtensionForWindows",
                                  "typeHandlerVersion": "1.0",
                                  "autoUpgradeMinorVersion": true,
                                  "settings": {
                                      "port": 50342
                                  }
                                }
                            }
                        ]
                    }
                }
            }
        }
    ]
   ```
   ### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Odebrání spravované identity přiřazené uživatelem ze sady škálování virtuálních počítačů Azure

Pokud máte sadu škálování virtuálního počítače, která už nepotřebuje spravovanou identitu přiřazenou uživatelem:

1. Bez ohledu na to, jestli se k Azure přihlašujete místně nebo prostřednictvím Azure Portal, použijte účet, který je přidružený k předplatnému Azure, které obsahuje sadu škálování virtuálního počítače.

2. Načtěte šablonu do [editoru](#azure-resource-manager-templates) a vyhledejte `Microsoft.Compute/virtualMachineScaleSets` prostředek zájmu v rámci `resources` oddílu. Pokud máte sadu škálování virtuálního počítače, která má pouze spravovanou identitu přiřazenou uživatelem, můžete ji zakázat změnou typu identity na `None` .

   Následující příklad ukazuje, jak odebrat všechny spravované identity přiřazené uživatelem z virtuálního počítače bez spravovaných identit přiřazených systémem:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2018-06-01**

   Pokud chcete ze sady škálování virtuálního počítače odebrat jednu spravovanou identitu přiřazenou uživatelem, odeberte ji ze `userAssignedIdentities` slovníku.

   Pokud máte identitu přiřazenou systémem, zachovejte ji v `type` hodnotě pod `identity` hodnotou.

   **Microsoft. COMPUTE/virtualMachineScaleSets API verze 2017-12-01**

   Pokud chcete ze sady škálování virtuálního počítače odebrat jednu spravovanou identitu přiřazenou uživatelem, odeberte ji z tohoto `identityIds` pole.

   Pokud máte spravovanou identitu přiřazenou systémem, ponechte ji v hodnotě `type` pod `identity` hodnotou.

## <a name="next-steps"></a>Další kroky

- [Přehled spravovaných identit pro prostředky Azure](overview.md)
