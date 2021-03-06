---
title: Povolení Azure Monitor pro hybridní prostředí
description: Tento článek popisuje, jak povolíte Azure Monitor pro virtuální počítače pro hybridní cloudové prostředí, které obsahuje jeden nebo víc virtuálních počítačů.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 7a6105e8742a4cb3d2f113c6ef723f6171baf4d9
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87328197"
---
# <a name="enable-azure-monitor-for-vms-for-a-hybrid-virtual-machine"></a>Povolit Azure Monitor pro virtuální počítače pro hybridní virtuální počítač
Tento článek popisuje, jak povolit Azure Monitor pro virtuální počítače pro virtuální počítač mimo Azure, včetně místních a dalších cloudových prostředí.

> [!IMPORTANT]
> Doporučenou metodou povolení hybridních virtuálních počítačů je nejprve povolit [pro servery Azure ARC](/azure-arc/servers/overview.md) , aby bylo možné virtuální počítače povolit Azure monitor pro virtuální počítače pomocí procesů podobných virtuálním počítačům Azure. Tento článek popisuje, jak připojit hybridní virtuální počítače, pokud se rozhodnete nepoužívat ARC Azure.

## <a name="prerequisites"></a>Požadavky

- [Vytvořte a nakonfigurujte Log Analytics pracovní prostor](vminsights-configure-workspace.md).
- V části [podporované operační systémy](vminsights-enable-overview.md#supported-operating-systems) se ujistěte, že je podporovaný operační systém virtuálního počítače nebo sady škálování virtuálních počítačů, které chcete povolit. 


## <a name="overview"></a>Přehled
Virtuální počítače mimo Azure vyžadují stejného agenta Log Analytics agenta a závislostí, které se používají pro virtuální počítače Azure. Vzhledem k tomu, že rozšíření virtuálních počítačů nemůžete použít k instalaci agentů, je nutné je ručně nainstalovat do hostovaného operačního systému nebo je nainstalovat pomocí nějaké jiné metody. 

Podrobnosti o nasazení Log Analytics agenta najdete v tématu [připojení počítačů s Windows k Azure monitor](../platform/agent-windows.md) nebo [připojení počítačů se systémem Linux k Azure monitor](../platform/agent-linux.md) . Podrobnosti o agentovi závislostí jsou uvedeny v tomto článku. 

## <a name="firewall-requirements"></a>Požadavky na bránu firewall
Požadavky na bránu firewall pro agenta Log Analytics jsou k dispozici v článku [Přehled agenta Log Analytics](..//platform/log-analytics-agent.md#network-requirements). Agent závislostí Azure Monitor pro virtuální počítače neodesílá žádná data a nevyžaduje žádné změny bran firewall nebo portů. Data mapy jsou vždy přenášena agentem Log Analytics do služby Azure Monitor, a to buď přímo, nebo prostřednictvím [brány Operations Management Suite](../../azure-monitor/platform/gateway.md) , pokud zásady zabezpečení IT nedovolují počítačům v síti připojení k Internetu.


## <a name="dependency-agent"></a>Agent závislostí

>[!NOTE]
>Následující informace popsané v této části se vztahují také na [řešení Service map](service-map.md).  

Agenta závislostí si můžete stáhnout z těchto umístění:

| Soubor | Operační systém | Verze | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.4.10090 | B4E1FF9C1E5CD254AA709AEF9723A81F04EC0763C327567C582CE99C0C5A0BAE  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.4.10090 | A56E310D297CE3B343AE8F4A6F72980F1C3173862D6169F1C713C2CA09660A9F |


## <a name="install-the-dependency-agent-on-windows"></a>Instalace agenta závislostí ve Windows

Agenta závislostí můžete nainstalovat ručně do počítačů se systémem Windows `InstallDependencyAgent-Windows.exe` . Pokud tento spustitelný soubor spustíte bez jakýchkoli možností, spustí se Průvodce instalací nástroje, který můžete použít k interaktivní instalaci agenta. Pro instalaci nebo odinstalaci agenta potřebujete oprávnění *správce* v HOSTOVANÉM operačním systému.

Následující tabulka popisuje parametry, které jsou podporovány instalačním programem pro agenta z příkazového řádku.

| Parametr | Popis |
|:--|:--|
| /? | Vrátí seznam možností příkazového řádku. |
| Parametr | Provede tichou instalaci bez zásahu uživatele. |

Pokud třeba chcete spustit instalační program s `/?` parametrem, zadejte **InstallDependencyAgent-Windows.exe/?**.

Soubory pro agenta závislostí Windows se ve výchozím nastavení instalují do složky *C:\Program Files\Microsoft Dependency agent* . Pokud se agent závislostí nepovede spustit po dokončení instalace, podívejte se na protokoly, kde najdete podrobné informace o chybě. Adresář protokolu je *%ProgramFiles%\Microsoft Dependency Agent\logs*.

### <a name="powershell-script"></a>Skript prostředí PowerShell
Pomocí následujícího ukázkového skriptu PowerShellu Stáhněte a nainstalujte agenta:

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```


## <a name="install-the-dependency-agent-on-linux"></a>Instalace agenta závislostí na Linux

Agent závislostí je nainstalovaný na serverech se systémem Linux z *InstallDependencyAgent-Linux64. bin*, skriptu prostředí s samorozbalovacím binárním souborem. Soubor můžete spustit pomocí `sh` nebo přidat oprávnění ke spuštění do samotného souboru.

>[!NOTE]
> K instalaci nebo konfiguraci tohoto agenta se vyžaduje přístup uživatele root.
>

| Parametr | Popis |
|:--|:--|
| -Help | Získá seznam parametrů příkazového řádku. |
| -s | Provede tichou instalaci bez zobrazení výzev uživateli. |
| --check | Ověřte oprávnění a operační systém, ale agenta neinstalujte. |

Pokud například chcete spustit instalační program s `-help` parametrem, zadejte **InstallDependencyAgent-Linux64. bin-Help**. Nainstalujte agenta závislostí pro Linux jako kořen spuštěním příkazu `sh InstallDependencyAgent-Linux64.bin` .

Pokud se nepovede spustit agenta závislostí, podrobnější informace o chybě najdete v protokolech. V agentech Linux se adresář protokolu */var/opt/Microsoft/Dependency-agent/log*.

Soubory pro agenta závislostí jsou umístěny v následujících adresářích:

| Soubory | Umístění |
|:--|:--|
| Základní soubory | /opt/microsoft/dependency-agent |
| Soubory protokolu | /var/opt/microsoft/dependency-agent/log |
| Konfigurační soubory | /etc/opt/microsoft/dependency-agent/config |
| Spustitelné soubory služby | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binární soubory úložiště | /var/opt/microsoft/dependency-agent/storage |

### <a name="shell-script"></a>Skript prostředí 
Pomocí následujícího ukázkového skriptu prostředí Stáhněte a nainstalujte agenta:

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Desired State Configuration

Chcete-li nasadit agenta závislostí pomocí konfigurace požadovaného stavu (DSC), můžete použít modul xPSDesiredStateConfiguration s následujícím příkladem kódu:

```powershell
configuration VMInsights {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```



## <a name="troubleshooting"></a>Řešení potíží

### <a name="vm-doesnt-appear-on-the-map"></a>Virtuální počítač se na mapě nezobrazí.

Pokud se instalace agenta závislostí zdařila, ale váš počítač se na mapě nezobrazuje, Diagnostikujte problém pomocí následujících kroků.

1. Je Dependency Agent správně nainstalovaný? Můžete to ověřit tak, že zkontrolujete, jestli je příslušná služba nainstalovaná a spuštěná.

    **Windows**: vyhledejte službu s názvem Microsoft Dependency agent.

    **Linux**: vyhledejte běžící proces Microsoft-Dependency-agent.

2. Jste na [cenové úrovni bezplatné Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? Bezplatný plán umožňuje až pět jedinečných počítačů. Žádné následné počítače se na mapě nezobrazí, i když už předchozí pět neposílá data.

3. Odesílá počítač data protokolu a výkonu do Azure Monitor protokolů? Pro váš počítač proveďte následující dotaz:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Vrátil (a) jeden nebo více výsledků? Jsou data aktuální? Pokud ano, Váš agent Log Analytics správně funguje a komunikuje se službou. Pokud ne, ověřte agenta na serveru: [Log Analytics agenta pro řešení potíží s Windows](../platform/agent-windows-troubleshoot.md) nebo [agenta Log Analytics pro řešení potíží](../platform/agent-linux-troubleshoot.md)se systémem Linux.

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Počítač se zobrazí na mapě, ale nemá žádné procesy.

Pokud na mapě vidíte Server, ale nemá žádná data o procesu nebo připojení, která indikuje, že je agent závislostí nainstalovaný a spuštěný, ale ovladač jádra se nenačetl.

Zkontrolujte soubor C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log (Windows) nebo soubor /var/opt/microsoft/dependency-agent/log/service.log (Linux). Poslední řádky souboru by měly obsahovat informace o tom, proč se jádro nenačetlo. Například pokud jste jádro aktualizovali, nemusí být podporované v Linuxu.


## <a name="next-steps"></a>Další kroky

Teď, když je monitorování povolené pro vaše virtuální počítače, jsou tyto informace k dispozici pro analýzu pomocí Azure Monitor pro virtuální počítače.

- Pokud chcete zobrazit zjištěné závislosti aplikací, přečtěte si téma [zobrazení Azure monitor pro virtuální počítače mapa](vminsights-maps.md).

- Pokud chcete zjistit kritické body a celkové využití výkonu vašeho virtuálního počítače, přečtěte si téma [zobrazení výkonu virtuálních počítačů Azure](vminsights-performance.md).
