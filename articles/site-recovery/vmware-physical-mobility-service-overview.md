---
title: O službě mobility pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů s Azure Site Recovery | Microsoft Docs
description: Přečtěte si o agentovi služby mobility pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů do Azure pomocí služby Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: how-to
ms.date: 04/10/2020
ms.author: ramamill
ms.openlocfilehash: d73e2776d0d9c86fe0331f9804bfeade3f1de676
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131800"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>O službě mobility pro virtuální počítače VMware a fyzické servery

Když nastavíte zotavení po havárii pro virtuální počítače VMware a fyzické servery pomocí [Azure Site Recovery](site-recovery-overview.md), nainstalujete službu Site Recovery mobility na každý místní virtuální počítač VMware a fyzický server. Služba mobility zachycuje zápisy dat na počítači a předá je na Site Recovery procesový Server. Služba mobility je nainstalovaná softwarem agenta služby mobility, který můžete nasadit pomocí následujících metod:

- [Nabízená instalace](#push-installation): když je ochrana povolená přes Azure Portal, Site Recovery nainstaluje na server službu mobility.
- Ruční instalace: službu mobility můžete nainstalovat ručně na každém počítači prostřednictvím [uživatelského rozhraní (UI)](#install-the-mobility-service-using-ui) nebo [příkazového řádku](#install-the-mobility-service-using-command-prompt).
- [Automatizované nasazení](vmware-azure-mobility-install-configuration-mgr.md): můžete automatizovat instalaci služby mobility pomocí nástrojů pro nasazení softwaru, jako je Configuration Manager.

> [!NOTE]
> Služba mobility využívá na zdrojových počítačích přibližně 6% – 10% paměti pro virtuální počítače VMware nebo fyzické počítače.

## <a name="antivirus-on-replicated-machines"></a>Antivirová ochrana na replikovaných počítačích

Pokud počítače, které chcete replikovat, používají antivirový software, vylučte instalační složku služby mobility _C:\ProgramData\ASR\agent_ z antivirových operací. Toto vyloučení zajišťuje, že replikace bude fungovat podle očekávání.

## <a name="push-installation"></a>Nabízená instalace

Nabízená instalace je nedílnou součástí úlohy, která se spouští z Azure Portal pro [Povolení replikace](vmware-azure-enable-replication.md#enable-replication). Po výběru sady virtuálních počítačů, které chcete chránit a povolit replikaci, konfigurační server vloží agenta služby mobility na servery, nainstaluje agenta a dokončí registraci agenta s konfiguračním serverem.

### <a name="prerequisites"></a>Požadavky

- Ujistěte se, že jsou splněné všechny [požadavky](vmware-azure-install-mobility-service.md) na nabízenou instalaci.
- Zajistěte, aby všechny konfigurace serveru splňovaly kritéria [pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů do Azure na](vmware-physical-azure-support-matrix.md)základě kritérií v matici podpory.

Pracovní postup nabízené instalace je popsaný v následujících částech:

### <a name="mobility-service-agent-version-923-and-higher"></a>Agent služby mobility verze 9,23 a vyšší

Další informace o verzi 9,23 najdete v části [kumulativní aktualizace 35 pro Azure Site Recovery](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery).

Během nabízené instalace služby mobility se provádí následující kroky:

1. Agent je vložen na zdrojový počítač. Kopírování agenta do zdrojového počítače může selhat kvůli několika chybám v životním prostředí. Pokud chcete řešit chyby nabízených instalací, navštivte [naše pokyny](vmware-azure-troubleshoot-push-install.md) .
1. Po úspěšném zkopírování agenta na server se na serveru provede kontrola požadavků.
   - Pokud jsou splněné všechny požadavky, spustí se instalace.
   - Instalace se nezdařila, pokud není splněna jedna nebo více [požadavků](vmware-physical-azure-support-matrix.md) .
1. V rámci instalace agenta se nainstalují poskytovatel služba Stínová kopie svazku (VSS) pro Azure Site Recovery. Zprostředkovatel služby Stínová kopie svazku se používá ke generování bodů obnovení konzistentních vzhledem k aplikacím. Pokud instalace poskytovatele služby VSS neproběhne úspěšně, tento krok se přeskočí a instalace agenta bude pokračovat.
1. Pokud je instalace agenta úspěšná, ale instalace poskytovatele služby VSS selže, bude stav úlohy označen jako **varování**. To nemá vliv na generování bodu obnovení konzistentního vzhledem k chybě.

    - Pokud chcete vygenerovat body obnovení konzistentní vzhledem k aplikacím, přečtěte si [naše doprovodné](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) materiály, abyste dokončili ruční instalaci poskytovatele služby VSS Site Recovery.
    - Pokud nechcete generovat body obnovení konzistentní vzhledem k aplikacím, [upravte zásady replikace](vmware-azure-set-up-replication.md#create-a-policy) tak, aby byly vypnuty body obnovení konzistentní vzhledem k aplikacím.

### <a name="mobility-service-agent-version-922-and-below"></a>Agent služby mobility verze 9,22 a nižší

1. Agent je vložen na zdrojový počítač. Kopírování agenta do zdrojového počítače může selhat kvůli několika chybám v životním prostředí. Pokyny k řešení chyb nabízené instalace najdete v [našich pokynech](vmware-azure-troubleshoot-push-install.md) .
1. Po úspěšném zkopírování agenta na server se na serveru provede kontrola požadavků.
   - Pokud jsou splněné všechny požadavky, spustí se instalace.
   - Instalace se nezdařila, pokud není splněna jedna nebo více [požadavků](vmware-physical-azure-support-matrix.md) .

1. V rámci instalace agenta se nainstalují poskytovatel služba Stínová kopie svazku (VSS) pro Azure Site Recovery. Zprostředkovatel služby Stínová kopie svazku se používá ke generování bodů obnovení konzistentních vzhledem k aplikacím.
   - Pokud se instalace poskytovatele služby VSS nezdaří, instalace agenta se nezdaří. Chcete-li se vyhnout selhání instalace agenta, použijte [verzi 9,23](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery) nebo vyšší k vygenerování bodů obnovení konzistentních vzhledem k chybě a proveďte ruční instalaci poskytovatele služby Stínová kopie svazku (VSS).

## <a name="install-the-mobility-service-using-ui"></a>Instalace služby mobility pomocí uživatelského rozhraní

### <a name="prerequisites"></a>Požadavky

- Zajistěte, aby všechny konfigurace serveru splňovaly kritéria [pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů do Azure na](vmware-physical-azure-support-matrix.md)základě kritérií v matici podpory.
- [Vyhledejte instalační program](#locate-installer-files) operačního systému serveru.

>[!IMPORTANT]
> Nepoužívejte metodu instalace uživatelského rozhraní, pokud provádíte replikaci virtuálního počítače infrastruktury Azure jako služby (IaaS) z jedné oblasti Azure do jiné. Použijte instalaci [příkazového řádku](#install-the-mobility-service-using-command-prompt) .

1. Zkopírujte instalační soubor do počítače a spusťte ho.
1. V **Možnosti instalace**vyberte **nainstalovat službu mobility**.
1. Zvolte umístění instalace a vyberte **nainstalovat**.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility1.png" alt-text="Stránka možností instalace služby mobility":::

1. Sledujte instalaci v **průběhu instalace**. Po dokončení instalace vyberte **pokračovat ke konfiguraci** pro registraci služby na konfiguračním serveru.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility3.png" alt-text="Registrační stránka služby mobility":::

1. V části **Podrobnosti konfiguračního serveru**zadejte IP adresu a heslo, které jste nakonfigurovali.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility4.png" alt-text="Registrační stránka služby mobility":::

1. Kliknutím na **Registrovat** dokončete registraci.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility5.png" alt-text="Finální stránka registrace služby mobility":::

## <a name="install-the-mobility-service-using-command-prompt"></a>Instalace služby mobility pomocí příkazového řádku

### <a name="prerequisites"></a>Požadavky

- Zajistěte, aby všechny konfigurace serveru splňovaly kritéria [pro zotavení po havárii virtuálních počítačů VMware a fyzických serverů do Azure na](vmware-physical-azure-support-matrix.md)základě kritérií v matici podpory.
- [Vyhledejte instalační program](#locate-installer-files) operačního systému serveru.

### <a name="windows-machine"></a>Počítač s Windows

- Z příkazového řádku spusťte následující příkazy ke zkopírování instalačního programu do místní složky (například _C:\Temp_) na serveru, který chcete chránit. Nahraďte název souboru instalačního programu skutečným názvem souboru.

  ```cmd
  cd C:\Temp
  ren Microsoft-ASR_UA_version_Windows_GA_date_release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted
  ```

- Spuštěním tohoto příkazu nainstalujte agenta.

  ```cmd
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

- Spusťte tyto příkazy k registraci agenta u konfiguračního serveru.

  ```cmd
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Nastavení instalace

Nastavení | Podrobnosti
--- | ---
Syntaxe | `UnifiedAgent.exe /Role \<MS/MT> /InstallLocation \<Install Location> /Platform "VmWare" /Silent`
Instalační protokoly | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log`
`/Role` | Povinný parametr instalace Určuje, jestli má být nainstalovaná služba mobility (MS) nebo hlavní cíl (MT).
`/InstallLocation`| Volitelný parametr. Určuje umístění instalace služby mobility (všechny složky).
`/Platform` | Povinné. Určuje platformu, na které je nainstalovaná služba mobility: <br/> **VMware** pro virtuální počítače VMware nebo fyzické servery. <br/> **Azure** pro virtuální počítače Azure.<br/><br/> Pokud pracujete s virtuálními počítači Azure jako s fyzickými počítači, zadejte **VMware**.
`/Silent`| Nepovinný parametr. Určuje, jestli se má spustit instalační program v tichém režimu.

#### <a name="registration-settings"></a>Nastavení registrace

Nastavení | Podrobnosti
--- | ---
Syntaxe | `UnifiedAgentConfigurator.exe  /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>`
Protokoly konfigurace agenta | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log`
`/CSEndPoint` | Povinný parametr. `<CSIP>`Určuje IP adresu konfiguračního serveru. Použijte jakoukoli platnou IP adresu.
`/PassphraseFilePath` |  Povinné. Umístění přístupového hesla Použijte jakoukoli platnou cestu UNC nebo místní cestu k souboru.

### <a name="linux-machine"></a>Počítač se systémem Linux

1. Z relace terminálu zkopírujte instalační program do místní složky, například _adresáře/TMP_ , na serveru, který chcete chránit. Nahraďte název souboru instalačního programu skutečným názvem souboru distribuce systému Linux a spusťte příkazy.

   ```shell
   cd /tmp ;
   tar -xvf Microsoft-ASR_UA_version_LinuxVersion_GA_date_release.tar.gz
   ```

2. Nainstalujte následujícím způsobem:

   ```shell
   sudo ./install -d <Install Location> -r MS -v VmWare -q
   ```

3. Po dokončení instalace je potřeba službu mobility zaregistrovat na konfiguračním serveru. Spuštěním následujícího příkazu zaregistrujte službu mobility pomocí konfiguračního serveru.

   ```shell
   /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
   ```

#### <a name="installation-settings"></a>Nastavení instalace

Nastavení | Podrobnosti
--- | ---
Syntaxe | `./install -d \<Install Location> -r \<MS/MT> -v VmWare -q`
`-r` | Povinný parametr instalace Určuje, jestli má být nainstalovaná služba mobility (MS) nebo hlavní cíl (MT).
`-d` | Volitelný parametr. Určuje umístění instalace služby mobility: `/usr/local/ASR` .
`-v` | Povinné. Určuje platformu, na které je nainstalovaná služba mobility. <br/> **VMware** pro virtuální počítače VMware nebo fyzické servery. <br/> **Azure** pro virtuální počítače Azure.
`-q` | Nepovinný parametr. Určuje, jestli se má spustit instalační program v tichém režimu.

#### <a name="registration-settings"></a>Nastavení registrace

Nastavení | Podrobnosti
--- | ---
Syntaxe | `cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP> -P \<PassphraseFilePath>`
`-i` | Povinný parametr. `<CSIP>`Určuje IP adresu konfiguračního serveru. Použijte jakoukoli platnou IP adresu.
`-P` |  Povinné. Úplná cesta k souboru, ve kterém se heslo ukládá Použijte libovolnou platnou složku.

## <a name="azure-virtual-machine-agent"></a>Agent virtuálního počítače Azure

- **Virtuální počítače s Windows**: od verze 9.7.0.0 služby mobility se [Agent virtuálního počítače Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent) nainstaluje pomocí instalačního programu služby mobility. Tím se zajistí, že při převzetí služeb při selhání počítače do Azure bude virtuální počítač Azure splňovat požadavky na instalaci agenta pro použití libovolného rozšíření virtuálního počítače.
- **Virtuální počítače se systémem Linux**: [WALinuxAgent](../virtual-machines/extensions/update-linux-agent.md) musí být po převzetí služeb při selhání na virtuálním počítači Azure nainstalovaný ručně.

## <a name="locate-installer-files"></a>Vyhledání instalačních souborů

Na konfiguračním serveru přejdete do složky _%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository_. V závislosti na operačním systému Zjistěte, který instalační program potřebujete. Následující tabulka shrnuje soubory instalačního programu pro každý virtuální počítač VMware a operační systém fyzického serveru. Než začnete, můžete si projít [podporované operační systémy](vmware-physical-azure-support-matrix.md#replicated-machines).

> [!NOTE]
> Názvy souborů používají syntaxi zobrazenou v následující tabulce s _verzí_ a _datem_ jako zástupné symboly pro reálné hodnoty. Skutečné názvy souborů budou vypadat podobně jako v těchto příkladech:
> - `Microsoft-ASR_UA_9.30.0.0_Windows_GA_22Oct2019_release.exe`
> - `Microsoft-ASR_UA_9.30.0.0_UBUNTU-16.04-64_GA_22Oct2019_release.tar.gz`

Instalační soubor | Operační systém (pouze 64 bitů)
--- | ---
`Microsoft-ASR_UA_version_Windows_GA_date_release.exe` | Windows Server 2016 </br> Windows Server 2012 R2 </br> Windows Server 2012 </br> Windows Server 2008 R2 SP1
`Microsoft-ASR_UA_version_RHEL6-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 6 </br> CentOS 6
`Microsoft-ASR_UA_version_RHEL7-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 7 </br> CentOS 7
`Microsoft-ASR_UA_version_SLES12-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 12 SP1 </br> Zahrnuje aktualizace SP2 a SP3.
`Microsoft-ASR_UA_version_SLES11-SP3-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 11 SP3
`Microsoft-ASR_UA_version_SLES11-SP4-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 11 SP4
`Microsoft-ASR_UA_version_OL6-64_GA_date_release.tar.gz` | Oracle Enterprise Linux 6,4 </br> Oracle Enterprise Linux 6,5
`Microsoft-ASR_UA_version_UBUNTU-14.04-64_GA_date_release.tar.gz` | Ubuntu Linux 14,04
`Microsoft-ASR_UA_version_UBUNTU-16.04-64_GA_date_release.tar.gz` | Server Ubuntu Linux 16,04 LTS
`Microsoft-ASR_UA_version_DEBIAN7-64_GA_date_release.tar.gz` | Debian 7
`Microsoft-ASR_UA_version_DEBIAN8-64_GA_date_release.tar.gz` | Debian 8

## <a name="next-steps"></a>Další kroky

[Nastavte nabízenou instalaci služby mobility](vmware-azure-install-mobility-service.md).
