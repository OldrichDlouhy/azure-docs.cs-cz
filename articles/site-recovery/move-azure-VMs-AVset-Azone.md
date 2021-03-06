---
title: Přesun virtuálních počítačů do oblasti Azure se zónami dostupnosti pomocí Azure Site Recovery
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 7d92311dfa699247995c7ded3e3930e19a9a537a
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135461"
---
# <a name="move-azure-vms-into-availability-zones"></a>Přesun virtuálních počítačů Azure do zón dostupnosti
Zóny dostupnosti v Azure vám pomůžou chránit vaše aplikace a data při selhání datacentra. Každou zónu dostupnosti tvoří jedno nebo několik datových center vybavených nezávislým napájením, chlazením a sítí. Aby se zajistila odolnost, existuje minimálně tři samostatné zóny ve všech povolených oblastech. Fyzické oddělení Zóny dostupnosti v rámci oblasti pomáhá chránit aplikace a data při selhání datacentra. V Zóny dostupnosti nabízí Azure smlouvu o úrovni služeb (SLA) 99,99% po dobu provozu virtuálních počítačů. Zóny dostupnosti jsou podporovány ve vybraných oblastech, jak je uvedeno v [oblastech, které podporují zóny dostupnosti](../availability-zones/az-region.md).

V situaci, kdy jsou vaše virtuální počítače nasazené jako *jediná instance* do konkrétní oblasti a chcete zlepšit dostupnost tím, že tyto virtuální počítače přesunete do zóny dostupnosti, můžete to udělat pomocí Azure Site Recovery. Tuto akci je dále možné rozdělit do kategorií:

- Přesun virtuálních počítačů s jednou instancí do Zóny dostupnosti v cílové oblasti
- Přesun virtuálních počítačů ve skupině dostupnosti do Zóny dostupnosti v cílové oblasti

> [!IMPORTANT]
> V současné době Azure Site Recovery podporuje přesun virtuálních počítačů z jedné oblasti do druhé. Podporuje pouze přesun mezi zónami v rámci oblasti v několika oblastech. [Další informace](./azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md).

## <a name="check-prerequisites"></a>Kontrola požadavků

- Ověřte, zda je v cílové oblasti [podporovaná zóny dostupnosti](../availability-zones/az-region.md). Ověřte, že [je podporovaná kombinace zdrojové oblasti/cílové oblasti](./azure-to-azure-support-matrix.md#region-support). Zajistěte si rozhodnutí o tom, co je v cílové oblasti.
- Ujistěte se, že rozumíte [komponentám a architektuře řešení](azure-to-azure-architecture.md).
- Zkontrolujte [omezení podpory a požadavky](azure-to-azure-support-matrix.md).
- Ověřte oprávnění účtu. Pokud jste si právě vytvořili bezplatný účet Azure, jste správcem předplatného. Pokud nejste správcem předplatného, ve spolupráci se správcem přiřaďte potřebná oprávnění. Pokud chcete pro virtuální počítač povolit replikaci a nakonec zkopírovat data do cíle pomocí Azure Site Recovery, musíte mít:

    1. Oprávnění k vytvoření virtuálního počítače v prostředcích Azure. Předdefinovaná role *Přispěvatel virtuálních počítačů* má tato oprávnění, která zahrnují:
        - Oprávnění k vytvoření virtuálního počítače ve vybrané skupině prostředků
        - Oprávnění k vytvoření virtuálního počítače ve vybrané virtuální síti
        - Oprávnění zapisovat na vybraný účet úložiště

    2. Oprávnění ke správě úloh Azure Site Recovery. Role *přispěvatel Site Recovery* má všechna oprávnění vyžadovaná ke správě akcí Site Recovery v trezoru Recovery Services.

## <a name="prepare-the-source-vms"></a>Příprava zdrojových virtuálních počítačů

1. Pokud je chcete přesunout do zóny dostupnosti pomocí Site Recovery, vaše virtuální počítače by měly používat spravované disky. Stávající virtuální počítače s Windows, které používají nespravované disky, můžete převést na používání spravovaných disků. Postupujte podle kroků v části [převedení virtuálního počítače s Windows z nespravovaných disků na Managed disks](../virtual-machines/windows/convert-unmanaged-to-managed-disks.md). Ujistěte se, že je skupina dostupnosti nakonfigurovaná jako *spravovaná*.
2. Ověřte, že se na virtuálních počítačích Azure, které chcete přesunout, nachází všechny nejnovější kořenové certifikáty. Pokud nejsou k dispozici nejnovější kořenové certifikáty, nelze v důsledku omezení zabezpečení povolit kopírování dat do cílové oblasti.

3. U virtuálních počítačů s Windows zajistíte přítomnost všech důvěryhodných kořenových certifikátů tím, že na ně nainstalujete všechny nejnovější aktualizace Windows. V odpojeném prostředí použijte standardní procesy služby Windows Update a aktualizace certifikátů ve vaší organizaci.

4. Pro virtuální počítače se systémem Linux postupujte podle pokynů, které poskytuje distributor pro Linux, a získejte nejnovější důvěryhodné kořenové certifikáty a seznam odvolaných certifikátů na virtuálním počítači.
5. Ujistěte se, že nepoužíváte ověřovací proxy server k řízení připojení k síti pro virtuální počítače, které chcete přesunout.

6. Pokud virtuální počítač, který se pokoušíte přesunout, nemá přístup k Internetu a k řízení odchozího přístupu používá proxy server brány firewall, Projděte si požadavky v části [Konfigurace odchozího připojení k síti](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms).

7. Identifikujte rozložení zdrojové sítě a prostředky, které aktuálně používáte k ověřování, včetně nástrojů pro vyrovnávání zatížení, skupin zabezpečení sítě a veřejné IP adresy.

## <a name="prepare-the-target-region"></a>Příprava cílové oblasti

1. Ověřte, že vaše předplatné Azure umožňuje vytvářet virtuální počítače v cílové oblasti, která se používá pro zotavení po havárii. V případě potřeby se obraťte na podporu, aby se povolila požadovaná kvóta.

2. Zkontrolujte, jestli máte v rámci předplatného k dispozici dostatek prostředků pro podporu virtuálních počítačů s takovými velikostmi, které odpovídají velikostem zdrojových virtuálních počítačů. Použijete-li Site Recovery ke kopírování dat do cíle, bude pro cílový virtuální počítač použita stejná velikost nebo nejbližší možná velikost.

3. Vytvořte cílový prostředek pro každou součást identifikovanou v rozložení zdrojové sítě. Tato akce zajistí, že po přejmutí do cílové oblasti budou mít vaše virtuální počítače všechny funkce a funkce, které jste měli ve zdroji.

    > [!NOTE]
    > Azure Site Recovery automaticky zjistí a vytvoří virtuální síť a účet úložiště, když povolíte replikaci pro zdrojový virtuální počítač. Tyto prostředky můžete také předem vytvořit a přiřadit k virtuálnímu počítači jako součást kroku povolení replikace. U jakýchkoli jiných prostředků, jak je uvedeno později, je třeba je ručně vytvořit v cílové oblasti.

     V následujících dokumentech se dozvíte, jak vytvořit nejčastěji používané síťové prostředky, které jsou pro vás relevantní, na základě konfigurace zdrojového virtuálního počítače.

    - [Skupiny zabezpečení sítě](../virtual-network/manage-network-security-group.md)
    - [Služby vyrovnávání zatížení](../load-balancer/index.yml)
    - [Veřejná IP adresa](../virtual-network/virtual-network-public-ip-address.md)
    
   Další síťové součásti najdete v [dokumentaci](../index.yml?pivot=products&panel=network)k síti.

    > [!IMPORTANT]
    > Ujistěte se, že v cíli používáte redundantní Nástroj pro vyrovnávání zatížení v zóně. Další informace najdete na [Standard Load Balancer a zóny dostupnosti](../load-balancer/load-balancer-standard-availability-zones.md).

4. Ruční [Vytvoření neprodukční sítě](../virtual-network/quick-create-portal.md) v cílové oblasti, pokud chcete otestovat konfiguraci před tím, než budete přecházet do cílové oblasti. Doporučujeme tento přístup, protože způsobuje minimální rušení s produkčním prostředím.

## <a name="enable-replication"></a>Povolení replikace
Následující kroky vás provedou při použití Azure Site Recovery k povolení replikace dat do cílové oblasti, než je nakonec přesunete do Zóny dostupnosti.

> [!NOTE]
> Tyto kroky jsou pro jeden virtuální počítač. Stejný postup můžete roztáhnout na více virtuálních počítačů. Přejít do trezoru Recovery Services, vybrat **+ replikovat**a vybrat příslušné virtuální počítače společně.

1. V Azure Portal vyberte **virtuální počítače**a vyberte virtuální počítač, do kterého chcete přejít zóny dostupnosti.
2. V části **Operace** vyberte **Zotavení po havárii**.
3. V části **Konfigurovat zotavení po havárii**  >  **cílová oblast**vyberte cílovou oblast, do které budete replikovat. Ujistěte se, že tato oblast [podporuje](../availability-zones/az-region.md) zóny dostupnosti.

    ![Výběr cílové oblasti](media/azure-vms-to-zones/enable-rep-1.PNG)

4. Vyberte **Další: Upřesnit nastavení**.
5. Vyberte odpovídající hodnoty pro cílové předplatné, cílovou skupinu prostředků virtuálního počítače a virtuální síť.
6. V části **dostupnost** vyberte zónu dostupnosti, do které chcete virtuální počítač přesunout. 
   > [!NOTE]
   > Pokud nevidíte možnost pro skupinu dostupnosti nebo zónu dostupností, ujistěte se, že jsou splněné [předpoklady](#prepare-the-source-vms) a že se dokončí [Příprava](#prepare-the-source-vms) zdrojových virtuálních počítačů.
  
    ![Výběry pro výběr zóny dostupnosti](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Vyberte **Povolit replikaci**. Tato akce spustí úlohu, která povolí replikaci pro virtuální počítač.

## <a name="check-settings"></a>Kontrolovat nastavení

Po dokončení úlohy replikace můžete zkontrolovat stav replikace, upravit nastavení replikace a otestovat nasazení.

1. V nabídce virtuálního počítače vyberte **Zotavení po havárii**.
2. Můžete kontrolovat stav replikace, vytvořené body obnovení a zdrojovou a cílovou oblast na mapě.

   ![Stav replikace](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Otestování konfigurace

1. V nabídce virtuální počítač vyberte možnost **zotavení po havárii**.
2. Vyberte ikonu **Test převzetí služeb při selhání** .
3. V části **testovací převzetí služeb při selhání**vyberte bod obnovení, který chcete použít pro převzetí služeb při selhání:

   - **Nejnovější zpracovaný:** Vrátí virtuální počítač k nejnovějšímu bodu obnovení zpracovanému službou Site Recovery. Zobrazí se časové razítko. U této možnosti nedochází k prodlevě při zpracování dat, takže poskytuje nízkou plánovanou dobu obnovení (RTO).
   - **Nejnovější konzistentní vzhledem k aplikacím**: Tato možnost vrátí všechny virtuální počítače k nejnovějšímu konzistentnímu bodu obnovení vzhledem k aplikacím. Zobrazí se časové razítko.
   - **Vlastní**: Vyberete si libovolný bod obnovení.

3. Vyberte testovací cílovou virtuální síť Azure, ke které chcete přesunout virtuální počítače Azure, abyste mohli otestovat konfiguraci. 

    > [!IMPORTANT]
    > Doporučujeme použít pro selhání testu samostatnou síť virtuálních počítačů Azure, nikoli produkční síť v cílové oblasti, do které chcete virtuální počítače přesunout.

4. Chcete-li zahájit testování přesunu, vyberte **OK**. Pokud chcete sledovat průběh, vyberte virtuální počítač a otevřete jeho vlastnosti. Nebo můžete vybrat úlohu **testovací převzetí služeb při selhání** v **Nastavení**název trezoru >  >  **úlohy**  >  **Site Recovery úlohy**.
5. Po dokončení převzetí služeb při selhání se na portálu Azure Portal v části **Virtuální počítače** objeví replika virtuálního počítače Azure. Ujistěte se, že je virtuální počítač spuštěný, má odpovídající velikost a je připojený k odpovídající síti.
6. Pokud chcete virtuální počítač vytvořený jako součást testování přesunu odstranit, vyberte v replikované položce možnost **vyčistit testovací převzetí služeb při selhání** . V části **poznámky**si zaznamenejte a uložte všechny poznámky spojené s testem.

## <a name="move-to-the-target-region-and-confirm"></a>Přejděte do cílové oblasti a potvrďte

1.  V nabídce virtuální počítač vyberte možnost **zotavení po havárii**.
2. Vyberte ikonu **převzetí služeb při selhání** .
3. V okně **Převzetí služeb při selhání** vyberte **Nejnovější**. 
4. Vyberte **Před spuštěním převzetí služeb při selhání vypnout počítač**. Služba Site Recovery se před aktivací převzetí služeb při selhání pokusí zdrojový virtuální počítač vypnout. Převzetí služeb při selhání bude pokračovat i v případě, že se vypnutí nepovede. Průběh převzetí služeb při selhání můžete sledovat na stránce **úlohy** . 
5. Po dokončení úlohy ověřte, že se virtuální počítač zobrazuje v cílové oblasti Azure podle očekávání.
6. V části **Replikované položky** klikněte pravým tlačítkem na virtuální počítač a pak na **Potvrdit**. Tím se dokončí proces přesunu do cílové oblasti. Počkejte, než se dokončí úloha potvrzení změn.

## <a name="discard-the-resource-in-the-source-region"></a>Zahodí prostředek ve zdrojové oblasti.

Přejít na virtuální počítač. Vyberte **Zakázat replikaci**. Tato akce zastaví proces kopírování dat pro virtuální počítač.  

> [!IMPORTANT]
> Projděte si předchozí krok, abyste se vyhnuli Site Recovery replikace po přesunutí. Automaticky se vyčistí nastavení replikace zdroje. Všimněte si, že rozšíření Site Recovery, které je nainstalováno jako součást replikace, není odebráno a je třeba je odebrat ručně.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zvýšili dostupnost virtuálního počítače Azure tak, že přejdete do skupiny dostupnosti nebo zóny dostupnosti. Nyní můžete nastavit zotavení po havárii pro přesunutý virtuální počítač.

> [!div class="nextstepaction"]
> [Nastavení zotavení po havárii po migraci](azure-to-azure-quickstart.md)
