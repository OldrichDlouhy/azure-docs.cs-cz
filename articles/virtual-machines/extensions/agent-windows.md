---
title: Přehled agenta virtuálního počítače Azure
description: Přehled agenta virtuálního počítače Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: mimckitt
manager: gwallace
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/20/2019
ms.author: akjosh
ms.openlocfilehash: 6ff5825f3272f0dadc74147d36e8c5fd8e7838d7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010950"
---
# <a name="azure-virtual-machine-agent-overview"></a>Přehled agenta virtuálního počítače Azure
Agent virtuálního počítače Microsoft Azure (agent virtuálního počítače) je zabezpečený a odlehčený proces, který spravuje interakci virtuálních počítačů s řadičem prostředků infrastruktury Azure. Agent virtuálního počítače má primární roli při povolování a provádění rozšíření virtuálních počítačů Azure. Rozšíření virtuálních počítačů umožňují konfiguraci po nasazení virtuálního počítače, jako je instalace a konfigurace softwaru. Rozšíření virtuálních počítačů také umožňují funkce pro obnovení, jako je resetování hesla pro správu virtuálního počítače. Bez agenta virtuálního počítače Azure nejde spustit rozšíření virtuálních počítačů.

Tento článek podrobně popisuje instalaci a detekci agenta virtuálního počítače Azure.

## <a name="install-the-vm-agent"></a>Instalace agenta virtuálního počítače

### <a name="azure-marketplace-image"></a>Obrázek Azure Marketplace

Agent virtuálního počítače Azure se ve výchozím nastavení instaluje na libovolný virtuální počítač s Windows nasazený z bitové kopie Azure Marketplace. Když nasadíte Azure Marketplace image z portálu, PowerShellu, rozhraní příkazového řádku nebo šablony Azure Resource Manager, nainstaluje se taky agent virtuálního počítače Azure.

Balíček agenta hosta systému Windows je rozdělen do dvou částí:

- Agent zřizování (PA)
- Agent hosta systému Windows (křídlo)

Pokud chcete spustit virtuální počítač, musíte mít na virtuálním počítači nainstalovanou službu PA, ale není potřeba ji instalovat. V době nasazení virtuálního počítače můžete vybrat možnost neinstalovat křídlo. Následující příklad ukazuje, jak vybrat možnost *provisionVmAgent* pomocí šablony Azure Resource Manager:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Pokud nemáte nainstalované agenty, nemůžete použít některé služby Azure, například Azure Backup nebo zabezpečení Azure. Tyto služby vyžadují instalaci rozšíření. Pokud jste virtuální počítač nasadili bez křídla, můžete nainstalovat nejnovější verzi agenta později.

### <a name="manual-installation"></a>Ruční instalace
Agenta virtuálního počítače s Windows je možné ručně nainstalovat pomocí balíčku Instalační služby systému Windows. Ruční instalace může být nutná při vytváření vlastní image virtuálního počítače, která je nasazena do Azure. Chcete-li ručně nainstalovat agenta virtuálního počítače s Windows, [Stáhněte si instalační program agenta virtuálního počítače](https://go.microsoft.com/fwlink/?LinkID=394789). Agent virtuálního počítače je podporovaný v systému Windows Server 2008 (64 bitů) a novějším.

> [!NOTE]
> Možnost AllowExtensionOperations je důležité aktualizovat po ruční instalaci VMAgent na virtuální počítač, který byl nasazený z image bez povolení ProvisionVMAgent.

```powershell
$vm.OSProfile.AllowExtensionOperations = $true
$vm | Update-AzVM
```

### <a name="prerequisites"></a>Předpoklady
- Agent virtuálního počítače s Windows potřebuje ke spuštění aspoň Windows Server 2008 (64), přičemž rozhraní .NET Framework 4,0. Podívejte [se na podporu minimálních verzí pro agenty virtuálních počítačů v Azure](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) .

- Ujistěte se, že váš virtuální počítač má přístup k IP adrese 168.63.129.16. Další informace najdete v tématu [co je IP adresa 168.63.129.16](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="detect-the-vm-agent"></a>Zjištění agenta virtuálního počítače

### <a name="powershell"></a>PowerShell

Modul Azure Resource Manager PowerShellu se dá použít k načtení informací o virtuálních počítačích Azure. Pokud chcete zobrazit informace o virtuálním počítači, jako je stav zřizování agenta virtuálního počítače Azure, použijte [příkaz Get-AzVM](/powershell/module/az.compute/get-azvm):

```powershell
Get-AzVM
```

Následující zhuštěný příklad výstupu ukazuje vlastnost *ProvisionVMAgent* vnořenou v *OSProfile*. Tato vlastnost slouží k určení, jestli je na virtuálním počítači nasazený agent virtuálního počítače:

```powershell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Pomocí následujícího skriptu můžete vracet stručný seznam názvů virtuálních počítačů a stav agenta virtuálního počítače:

```powershell
$vms = Get-AzVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Ruční zjišťování

Když se přihlásíte k virtuálnímu počítači s Windows, můžete ke kontrole spuštěných procesů použít Správce úloh. Pokud chcete zkontrolovat agenta virtuálního počítače Azure, otevřete Správce úloh, klikněte na kartu *Podrobnosti* a vyhledejte název procesu **WindowsAzureGuestAgent.exe**. Přítomnost tohoto procesu indikuje, že je agent virtuálního počítače nainstalovaný.


## <a name="upgrade-the-vm-agent"></a>Upgrade agenta virtuálního počítače
Agent virtuálního počítače Azure pro Windows se upgraduje automaticky. Když se do Azure nasadí nové virtuální počítače, dostanou nejnovějšího agenta virtuálního počítače při zřizování virtuálních počítačů. Vlastní image virtuálních počítačů by se měly aktualizovat ručně, aby se při vytváření image zahrnul nový agent virtuálního počítače.

## <a name="windows-guest-agent-automatic-logs-collection"></a>Kolekce automatických protokolů agenta hosta systému Windows
Agent hosta systému Windows obsahuje funkci pro automatické shromáždění některých protokolů. Tato funkce je řadičem procesu CollectGuestLogs.exe. Existuje jak pro PaaS Cloud Services, tak pro IaaS Virtual Machines a jejím cílem je rychle & automaticky shromažďovat některé diagnostické protokoly z virtuálního počítače, aby se mohly použít pro offline analýzu. Shromážděné protokoly jsou protokoly událostí, protokoly operačního systému, protokoly Azure a některé klíče registru. Vytvoří soubor ZIP, který se přenese na hostitele virtuálního počítače. Tento soubor ZIP si pak můžete prověřit technickými týmy a odborníky na podporu a prozkoumat problémy na žádost zákazníka, který vlastní virtuální počítač.

## <a name="next-steps"></a>Další kroky
Další informace o rozšíření virtuálních počítačů najdete v tématu [Přehled rozšíření a funkcí virtuálních počítačů Azure](overview.md).
