---
title: Nastavení zařízení Azure Migrate pro Hyper-V
description: Naučte se, jak nastavit zařízení Azure Migrate pro vyhodnocení a migraci virtuálních počítačů Hyper-V.
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: 56b034709309a3afe9d18df7af9ababc74a24cee
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109701"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Nastavení zařízení pro virtuální počítače Hyper-V

Podle tohoto článku nastavte zařízení Azure Migrate pro posouzení virtuálních počítačů Hyper-V pomocí nástroje [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) Tool.

[Zařízení Azure Migrate](migrate-appliance.md) je jednoduché zařízení, které používá Azure Migrate: posouzení serveru/migrace za účelem zjišťování místních virtuálních počítačů Hyper-V a odeslání metadat virtuálního počítače nebo dat o výkonu do Azure.

Zařízení můžete nasadit pomocí několika metod:

- Nastavte na virtuálním počítači s technologií Hyper-V pomocí staženého virtuálního pevného disku. Tato metoda je popsaná v tomto článku.
- Nastavte na VIRTUÁLNÍm počítači Hyper-V nebo na fyzickém počítači pomocí skriptu instalačního programu PowerShell. [Tato metoda](deploy-appliance-script.md) by se měla použít, pokud nemůžete nastavit virtuální počítač pomocí virtuálního pevného disku nebo pokud jste v Azure Government.

Po vytvoření zařízení zkontrolujete, že se může připojit k Azure Migrate: posouzení serveru, nakonfigurovat ho poprvé a zaregistrujte ho v projektu Azure Migrate.

## <a name="appliance-deployment-vhd"></a>Nasazení zařízení (VHD)

Nastavení zařízení pomocí šablony VHD:

- Z Azure Portal Stáhněte komprimovaný VHD Hyper-V.
- Vytvořte zařízení a ověřte, že se může připojit k Azure Migrate posouzení serveru.
- Nakonfigurujete zařízení poprvé a zaregistrujete ho do projektu Azure Migrate.

## <a name="download-the-vhd"></a>Stažení virtuálního pevného disku

Stáhněte pro zařízení šablonu VHD s příponou.

1. V Azure Migrate **cíle migrace**  >  na**servery**  >  **: vyhodnocování serveru**klikněte na **zjistit**.
2. V rozevíracích **seznamech počítačů**, ve  >  **kterých jsou počítače virtualizované?** klikněte na **Ano, s technologií Hyper-V**.
3. Kliknutím na **Stáhnout** Stáhněte soubor VHD.

    ![Stáhnout virtuální počítač](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Ověřit zabezpečení

Před nasazením souboru ZIP ověřte, zda je soubor zip zabezpečený.

1. Na počítači, do kterého jste soubor stáhli, otevřete jako správce příkazový řádek.
2. Spusťte následující příkaz, který vygeneruje hodnotu hash pro virtuální pevný disk.
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Příklady použití: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.vhd SHA256```
3.  U zařízení verze 2.19.11.12 by se měla vygenerovaná hodnota hash shodovat s tímto [nastavením](./tutorial-assess-hyper-v.md#verify-security).




## <a name="create-the-appliance-vm"></a>Vytvoření virtuálního počítače zařízení

Naimportujte stažený soubor a vytvořte virtuální počítač.

1. Extrahujte soubor VHD s příponou ZIP do složky na hostiteli Hyper-V, která bude hostovat virtuální počítač zařízení. Jsou extrahovány tři složky.
2. Spusťte Správce technologie Hyper-V. V nabídce **Akce**klikněte na **importovat virtuální počítač**.

    ![Nasazení VHD](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

2. V Průvodci importem virtuálního počítače > **než začnete**, klikněte na **Další**.
3. V části **Vyhledat složku**zadejte složku obsahující extrahovaný virtuální pevný disk. Potom klikněte na **Další**.
1. V nabídce **Vybrat virtuální počítač**klikněte na **Další**.
2. V části **zvolit typ importu**klikněte na **zkopírovat virtuální počítač (vytvořit nové jedinečné ID)**. Potom klikněte na **Další**.
3. V části **zvolit cíl**ponechte výchozí nastavení. Klikněte na **Další**.
4. V části **složky úložiště**ponechte výchozí nastavení. Klikněte na **Další**.
5. V části **zvolit síť**zadejte virtuální přepínač, který bude virtuální počítač používat. Přepínač potřebuje připojení k Internetu, aby bylo možné odesílat data do Azure.
6. V části **Souhrn**zkontrolujte nastavení. Klikněte na **Dokončit**.
7. Ve Správci technologie Hyper-V > **Virtual Machines**spusťte virtuální počítač.


### <a name="verify-appliance-access-to-azure"></a>Ověření přístupu zařízení k Azure

Ujistěte se, že se virtuální počítač zařízení může připojit k adresám URL Azure pro cloudy [veřejné](migrate-appliance.md#public-cloud-urls) a [státní správy](migrate-appliance.md#government-cloud-urls) .

## <a name="configure-the-appliance"></a>Konfigurace zařízení

Nastavte zařízení poprvé. Pokud zařízení nasadíte pomocí skriptu místo virtuálního pevného disku, nepoužijí se první dva kroky v proceduře.

1. Ve Správci technologie Hyper-V > **Virtual Machines**klikněte pravým tlačítkem na virtuální počítač > **připojit**.
2. Zadejte jazyk, časové pásmo a heslo pro zařízení.
3. Otevřete prohlížeč na jakémkoli počítači, který se může připojit k VIRTUÁLNÍmu počítači, a otevřete adresu URL webové aplikace zařízení: ***název zařízení https://nebo IP adresa*: 44368**.

   Alternativně můžete aplikaci otevřít z plochy zařízení kliknutím na zástupce aplikace.
1. Ve webové aplikaci > **nastavení požadavků**postupujte takto:
    - **Licence**: přijměte licenční podmínky a přečtěte si informace třetích stran.
    - **Připojení**: aplikace kontroluje, jestli má virtuální počítač přístup k Internetu. Pokud virtuální počítač používá proxy server:
        - Klikněte na **nastavení proxy serveru**a zadejte adresu proxy serveru a port naslouchání ve formuláři http://ProxyIPAddress nebo http://ProxyFQDN .
        - Pokud proxy server potřebuje přihlašovací údaje, zadejte je.
        - Podporuje se jen proxy protokolu HTTP.
    - **Časová synchronizace**: čas je ověřený. Čas v zařízení by měl být synchronizovaný s internetovým časem, aby zjišťování virtuálních počítačů fungovalo správně.
    - **Instalovat aktualizace**: posouzení Azure Migrate serveru kontroluje, jestli má zařízení nainstalované nejnovější aktualizace.

### <a name="register-the-appliance-with-azure-migrate"></a>Zaregistrovat zařízení ve Azure Migrate

1. Klikněte na **Přihlásit se**. Pokud se nezobrazí, ujistěte se, že jste v prohlížeči zakázali blokování automaticky otevíraných oken.
2. Na nové kartě se přihlaste pomocí svých přihlašovacích údajů Azure.
    - Přihlaste se pomocí svého uživatelského jména a hesla.
    - Přihlášení pomocí PIN kódu se nepodporuje.
3. Po úspěšném přihlášení se vraťte k webové aplikaci.
4. Vyberte předplatné, ve kterém byl vytvořen Azure Migrate projekt. Pak vyberte projekt.
5. Zadejte název zařízení. Název by měl být alfanumerický a nesmí obsahovat více než 14 znaků.
6. Klikněte na **Zaregistrovat**.


### <a name="delegate-credentials-for-smb-vhds"></a>Pověření delegáta pro virtuální pevné disky SMB

Pokud používáte na SMB virtuální pevné disky, musíte povolit delegování přihlašovacích údajů ze zařízení na hostitele Hyper-V. Provedete to pomocí zařízení:

1. Na virtuálním počítači zařízení spusťte tento příkaz. HyperVHost1/HyperVHost2 jsou příklady názvů hostitelů.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Případně to udělejte v Editor místních zásad skupiny na zařízení:
    - V konfiguraci počítače **Zásady místního počítače**  >  **Computer Configuration**klikněte na **šablony pro správu**  >  **System**  >  **delegování přihlašovacích údajů**systému.
    - Dvakrát klikněte na **Povolit delegování nových přihlašovacích údajů**a vyberte **povoleno**.
    - V nabídce **Možnosti**klikněte na **Zobrazit**a do seznamu přidejte každého hostitele Hyper-V, který chcete zjistit, a použijte příkaz **WSMan/** jako předponu.
    - V **delegování přihlašovacích údajů**poklikejte na možnost **umožňuje delegovat nové přihlašovací údaje pomocí ověřování serveru jenom s protokolem NTLM**. Znovu přidejte všechny hostitele Hyper-V, které chcete vyhledat, do seznamu s použitím nástroje **WSMan/** jako předpony.

## <a name="start-continuous-discovery"></a>Spustit průběžné zjišťování

Připojte se ze zařízení k hostitelům nebo clusterům Hyper-V a spusťte zjišťování virtuálních počítačů.

1. Do pole **uživatelské jméno** a **heslo**zadejte přihlašovací údaje účtu, které zařízení použije ke zjištění virtuálních počítačů. Zadejte popisný název přihlašovacích údajů a klikněte na **Uložit podrobnosti**.
2. Klikněte na **Přidat hostitele**a zadejte podrobnosti o hostiteli nebo clusteru Hyper-V.
3. Klikněte na **Validate** (Ověřit). Po ověření se zobrazí počet virtuálních počítačů, které se dají zjistit u každého hostitele nebo clusteru.
    - Pokud se ověření pro hostitele nepovede, přečtěte si chybu, když najedete myší na ikonu ve sloupci **stav** . Opravte problémy a znovu ověřte.
    - Chcete-li odebrat hostitele nebo clustery, vyberte možnost > **Odstranit**.
    - Z clusteru nelze odebrat konkrétního hostitele. Můžete odebrat jenom celý cluster.
    - Cluster můžete přidat i v případě, že v clusteru dojde k problémům s konkrétními hostiteli.
4. Po ověření klikněte na **Uložit a spusťte zjišťování a** spusťte proces zjišťování.

Spustí se zjišťování. Zobrazení metadat zjištěných virtuálních počítačů v Azure Portal trvá přibližně 15 minut.

## <a name="verify-vms-in-the-portal"></a>Kontrola virtuálních počítačů na portálu

Po dokončení zjišťování můžete ověřit, že se virtuální počítače zobrazují na portálu.

1. Otevřete řídicí panel Azure Migrate.
2. V **Azure Migrate-servery**  >  **Azure Migrate: na stránce posouzení serveru** klikněte na ikonu, která zobrazuje počet **zjištěných serverů**.


## <a name="next-steps"></a>Další kroky

Vyzkoušejte si [vyhodnocení technologie Hyper-V](tutorial-assess-hyper-v.md) s Azure Migrate posouzení serveru.
