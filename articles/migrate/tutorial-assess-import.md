---
title: Vyhodnocování serverů pomocí importovaných dat serveru s využitím hodnocení serveru Azure Migrate
description: Popisuje, jak vyhodnotit místní servery pro migraci do Azure pomocí Azure Migrateho posouzení serveru pomocí importovaných dat.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 98675b0f986ecb78ff122ed052a01d521aac1f6f
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86114206"
---
# <a name="assess-servers-by-using-imported-data"></a>Posouzení serverů pomocí importovaných dat

Tento článek vysvětluje, jak vyhodnotit místní servery pomocí nástroje [Azure Migrate: Nástroj pro vyhodnocení serveru](migrate-services-overview.md#azure-migrate-server-assessment-tool) pomocí importu metadat serveru ve formátu hodnot oddělených čárkami (CSV). Tato metoda posouzení nevyžaduje, abyste nastavili zařízení Azure Migrate, abyste mohli vytvořit posouzení. To je užitečné v těchto případech:

- Před nasazením zařízení chcete vytvořit rychlé a počáteční posouzení.
- Zařízení Azure Migrate ve vaší organizaci nemůžete nasadit.
- Nemůžete sdílet přihlašovací údaje, které povolují přístup k místním serverům.
- Omezení zabezpečení brání v shromažďování a odesílání dat shromážděných zařízením do Azure. Data, která sdílíte, můžete řídit v importovaném souboru. Také velká část dat (například poskytování IP adres) je volitelná.

## <a name="before-you-start"></a>Než začnete

Pamatujte na tyto body:

- V jednom souboru CSV můžete přidat maximálně 20 000 serverů.
- Do Azure Migrate projektu můžete přidat až 20 000 serverů pomocí CSV.
- Informace o serveru můžete odeslat do posouzení serveru několikrát pomocí CSV.
- Shromažďování informací o aplikaci je užitečné při vyhodnocování místního prostředí pro migraci. Posouzení serveru ale aktuálně neprovádí vyhodnocení na úrovni aplikace nebo při vytváření posouzení nebere v úvahu aplikace.

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Nastavte Azure Migrate projekt.
> * Do souboru CSV zadejte informace o serveru.
> * Importujte soubor a přidejte informace o serveru do posouzení serveru.
> * Vytvoření a kontrola posouzení.

> [!NOTE]
> Kurzy ukazují nejjednodušší cestu k nasazení scénáře, abyste mohli rychle nastavit zkoušku konceptu. Kurzy používají výchozí možnosti, pokud je to možné, a nezobrazují všechna možná nastavení a cesty. Podrobné pokyny najdete v tématu návody.

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/pricing/free-trial/), ještě než začnete.

## <a name="set-azure-permissions-for-azure-migrate"></a>Nastavení oprávnění Azure pro Azure Migrate

Váš účet Azure potřebuje oprávnění k vytvoření projektu Azure Migrate.

1. V Azure Portal otevřete předplatné a vyberte **řízení přístupu (IAM)**.
2. V části **kontrolovat přístup**Najděte příslušný účet a pak ho vyberte pro zobrazení oprávnění.
3. Ujistěte se, že máte oprávnění **Přispěvatel** nebo **Owner** .
    - Pokud jste právě vytvořili bezplatný účet Azure, jste vlastníkem svého předplatného.
    - Pokud nejste vlastníkem předplatného, pracujte s vlastníkem a přiřaďte roli.

## <a name="set-up-an-azure-migrate-project"></a>Nastavení Azure Migrateho projektu

Nastavení nového projektu Azure Migrate:

1. V Azure Portal vyhledejte v části **všechny služby** **Azure Migrate**.
2. V části **Služby** vyberte **Azure Migrate**.
3. V části **Přehled**v části **zjišťování, vyhodnocení a migrace serverů**vyberte možnost **zhodnotit a migrovat servery**.

    ![Zjišťování a vyhodnocení serverů](./media/tutorial-assess-import/assess-migrate.png)

4. V části **Začínáme**vyberte **Přidat nástroje**.
5. V části **Projekt migrace** vyberte své předplatné Azure a vytvořte skupinu prostředků, pokud ji ještě nemáte.
6. V části **Project Details (podrobnosti projektu**) zadejte název projektu a zeměpisnou oblast, ve které chcete vytvořit projekt. Další informace najdete tady:

    - Projděte si podporované oblasti pro [veřejný cloud](migrate-support-matrix.md#supported-geographies-public-cloud) a [cloud pro státní správu](migrate-support-matrix.md#supported-geographies-azure-government).
    - Při spouštění migrace můžete vybrat jakoukoli cílovou oblast.

    ![Vytvoření projektu Azure Migrate](./media/tutorial-assess-import/migrate-project.png)

7. Vyberte **Další**.
8. V **nástroji vybrat nástroj pro posouzení**vyberte **Azure Migrate: vyhodnocení serveru**  >  **Další**.

    ![Vytvoření posouzení Azure Migrate](./media/tutorial-assess-import/assessment-tool.png)

9. V části **Vybrat nástroj pro migraci** vyberte **V tuto chvíli přeskočit přidání nástroje pro migraci** > **Další**.
10. V okně **Revize + přidat nástroje**zkontrolujte nastavení a pak vyberte **Přidat nástroje**.
11. Počkejte několik minut, než se projekt Azure Migrate nasadí. Pak přejdete na stránku projektu. Pokud se projekt nezobrazí, můžete k němu přejít z části **Servery** na řídicím panelu služby Azure Migrate.

## <a name="prepare-the-csv"></a>Příprava sdíleného svazku clusteru

Stáhněte si šablonu sdíleného svazku clusteru a přidejte do ní informace o serveru.

### <a name="download-the-template"></a>Stažení šablony

1. V **Azure Migrate cíle migrace**  >  **Servers**  >  **Azure Migrate: Server Assessment**vyberte **Vyhledat**.
2. V možnosti **zjistit počítače**vyberte **importovat pomocí CSV**.
3. Vyberte **Stáhnout** a stáhněte šablonu sdíleného svazku clusteru. Případně si ho můžete [stáhnout přímo](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![Stáhnout šablonu CSV](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Přidat informace o serveru

Shromážděte data serveru a přidejte je do souboru CSV.

- Pokud chcete shromažďovat data, můžete je exportovat z nástrojů, které používáte pro správu místního serveru, jako je například VMware vSphere nebo vaše databáze správy konfigurace (CMDB).
- Pokud chcete zkontrolovat ukázková data, Stáhněte si náš [ukázkový soubor](https://go.microsoft.com/fwlink/?linkid=2108405).

Následující tabulka shrnuje pole souborů k vyplnění:

**Název pole** | **Závaznou** | **Podrobnosti**
--- | --- | ---
**Název serveru** | Yes | Doporučujeme zadat plně kvalifikovaný název domény (FQDN).
**IP adresa** | No | Adresa serveru.
**Cores** | Yes | Počet jader procesoru přidělených serveru.
**Memory (Paměť)** | Yes | Celková velikost paměti RAM (v MB) přidělená serveru.
**Název operačního systému** | Yes | Serverový operační systém. <br/> Vyhodnocování rozpoznávají názvy operačních systémů, které odpovídají nebo obsahují názvy v [tomto](#supported-operating-system-names) seznamu.
**Verze operačního systému** | No | Verze operačního systému serveru.
**Architektura operačního systému** | No | Architektura operačního systému serveru <br/> Platné hodnoty jsou: x64, x86, AMD64, 32-bit nebo 64-bit
**Počet disků** | No | Není nutné, pokud jsou k dispozici podrobnosti o jednotlivých discích.
**Velikost disku 1**  | No | Maximální velikost disku (v GB)<br/>[Přidáním sloupců](#add-multiple-disks) do šablony můžete přidat podrobnosti o dalších discích. Můžete přidat až osm disků.
**Disk 1 operace čtení** | No | Operace čtení z disku za sekundu
**Operace zápisu na disk 1** | No | Operace zápisu na disk za sekundu
**Propustnost čtení disku 1** | No | Data načtená z disku za sekundu, v MB za sekundu.
**Propustnost zápisu disku 1** | No | Data zapsaná na disk za sekundu, v MB za sekundu.
**Procento využití procesoru** | No | Procento využitého procesoru
**Procento využití paměti** | No | Procento využité paměti RAM
**Operace čtení z celkového počtu disků** | No | Operace čtení disku za sekundu
**Operace zápisu z celkového počtu disků** | No | Operace zápisu na disk za sekundu
**Propustnost čtení celkem disků** | No | Data načtená z disku v MB za sekundu.
**Propustnost zápisu celkem disků** | No | Data zapsaná na disk v MB za sekundu.
**Síť v propustnosti** | No | Data přijatá serverem v MB za sekundu.
**Propustnost sítě** | No | Data přenášená serverem v MB za sekundu.
**Typ firmwaru** | No | Firmware serveru. Hodnoty mohou být "BIOS" nebo "UEFI".
**Adresa MAC**| No | Adresa MAC serveru.


### <a name="add-operating-systems"></a>Přidat operační systémy

Posouzení rozpoznává konkrétní názvy operačních systémů. Libovolný název, který zadáte, musí přesně odpovídat jednomu z řetězců v [seznamu podporovaných názvů](#supported-operating-system-names).

### <a name="add-multiple-disks"></a>Přidat více disků

Šablona poskytuje výchozí pole pro první disk. Podobné sloupce můžete přidat až na osm disků.

Pokud například chcete zadat všechna pole pro druhý disk, přidejte tyto sloupce:

- Velikost disku 2
- Operace čtení disku 2
- Operace zápisu na disk 2
- Propustnost čtení disku 2
- Propustnost zápisu disku 2


## <a name="import-the-server-information"></a>Importovat informace o serveru

Po přidání informací do šablony sdíleného svazku clusteru importujte servery do vyhodnocování serveru.

1. V Azure Migrate v části **zjišťování počítačů**přejít na dokončenou šablonu.
2. Vyberte **Importovat**.
3. Zobrazí se stav importu.
    - Pokud se ve stavu zobrazí upozornění, můžete je buď opravit, nebo pokračovat bez jejich adresování.
    - Pro zlepšení přesnosti hodnocení Vylepšete informace o serveru, jak je navrženo v části upozornění.
    - Chcete-li zobrazit a opravit upozornění, vyberte možnost **Stáhnout podrobnosti upozornění. Sdílený svazek clusteru**. Tato operace stáhne sdílený svazek clusteru s upozorněními, která jsou součástí. Přečtěte si upozornění a opravte problémy podle potřeby.
    - Pokud se ve stavu objeví chyby, takže se stav importu **nezdařil**, je nutné tyto chyby opravit, aby bylo možné pokračovat v importu:
        1. Stáhněte si sdílený svazek clusteru, který teď obsahuje podrobnosti o chybě.
        1. Zkontrolujte a podle potřeby vyřešte chyby. 
        1. Znovu nahrajte změněný soubor.
4. Po **dokončení**importu se informace o serveru naimportovaly.

## <a name="update-server-information"></a>Aktualizovat informace o serveru

Informace o serveru můžete aktualizovat tak, že znovu naimportujete data pro server se stejným **názvem serveru**. Pole **název serveru** nemůžete změnit. Odstraňování serverů se v tuto chvíli nepodporuje.

## <a name="verify-servers-in-the-portal"></a>Ověřit servery na portálu

Ověření, že se servery zobrazí v Azure Portal po zjištění:

1. Otevřete řídicí panel Azure Migrate.
2. Na stránce **Azure Migrate-servery**  >  **Azure Migrate: posouzení serveru** vyberte ikonu, která zobrazuje počet **zjištěných serverů**.
3. Vyberte kartu **Import na základě** .

## <a name="set-up-and-run-an-assessment"></a>Nastavení a spuštění posouzení

Pomocí posouzení serveru můžete vytvořit dva typy posouzení.


**Typ posouzení** | **Podrobnosti**
--- | --- 
**Virtuální počítač Azure** | Posouzení migrace vašich místních serverů do virtuálních počítačů Azure. <br/><br/> Pomocí tohoto typu posouzení můžete vyhodnotit místní [virtuální počítače VMware](how-to-set-up-appliance-vmware.md), [virtuální počítače Hyper-V](how-to-set-up-appliance-hyper-v.md)a [fyzické servery](how-to-set-up-appliance-physical.md) pro migraci do Azure. (concepts-assessment-calculation.md)
**Azure VMware Solution (AVS)** | Posouzení migrace místních serverů do [Řešení Azure VMware (AVS)](../azure-vmware/introduction.md). <br/><br/> Pomocí tohoto typu posouzení můžete vyhodnotit místní [virtuální počítače VMware](how-to-set-up-appliance-vmware.md) pro migraci do řešení Azure VMware (AVS). [Další informace](concepts-azure-vmware-solution-assessment-calculation.md)

### <a name="sizing-criteria"></a>Kritéria změny velikosti

Posouzení serveru poskytuje dvě možnosti pro kritéria změny velikosti:

**Kritéria změny velikosti** | **Podrobnosti** | **Data**
--- | --- | ---
**Na základě výkonu** | Posouzení, která vytvářejí doporučení na základě shromážděných údajů o výkonu | **Posouzení virtuálního počítače Azure**: doporučení velikosti virtuálního počítače vychází z dat využití procesoru a paměti.<br/><br/> Doporučení pro typ disku (standardní disková jednotka/SSD nebo Premium – spravované disky) vychází z IOPS a propustnosti místních disků.<br/><br/> **Posouzení řešení Azure VMware (AVS)**: doporučení pro služby AVS jsou založená na datech využití procesoru a paměti.
**V místním prostředí** | Posouzení, které nepoužívají údaje o výkonu k vytváření doporučení. | **Posouzení virtuálního počítače Azure**: doporučení velikosti virtuálního počítače je založené na velikosti místního virtuálního počítače.<br/><br> Doporučený typ disku je založený na tom, co jste vybrali v nastavení typ úložiště pro posouzení.<br/><br/> **Posouzení řešení Azure VMware (AVS)**: doporučení pro služby AVS jsou založená na velikosti místního virtuálního počítače.


Spuštění posouzení:

1. Projděte si [osvědčené postupy](best-practices-assessment.md) pro vytváření hodnocení.
2. Na kartě **servery** na dlaždici **Azure Migrate: posouzení serveru** vyberte možnost **vyhodnotit**.

    ![Posouzení](./media/tutorial-assess-physical/assess.png)

3. V poli **vyhodnotit servery**zadejte název posouzení a vyberte typ **posouzení** jako *virtuální počítač Azure* , pokud chcete provést posouzení virtuálních počítačů Azure nebo *Řešení Azure VMware (AVS)* , pokud chcete provádět posouzení služby AVS.

    ![Základy hodnocení](./media/how-to-create-assessment/assess-servers-azurevm.png)

4. Ve **zdroji zjišťování**vyberte **počítače přidané prostřednictvím importu do Azure Migrate**.

5. Pokud chcete zobrazit vlastnosti posouzení, vyberte **Zobrazit vše**.

    ![Vlastnosti posouzení](./media/tutorial-assess-physical/view-all.png)

6. Klikněte na tlačítko **Další** a **Vyberte počítače, které chcete vyhodnotit**. V **Vyberte nebo vytvořte skupinu**vyberte **vytvořit novou**a zadejte název skupiny. Skupina shromažďuje jeden nebo více virtuálních počítačů dohromady pro posouzení.
7. V části **přidat počítače do skupiny**vyberte servery, které chcete přidat do skupiny.
8. Kliknutím **next** na další **Projděte si téma** a podrobnější informace o posouzení a Projděte si podrobnosti o posouzení.
9. Kliknutím na **vytvořit vyhodnocení** vytvořte skupinu a pak spusťte posouzení.

    ![Vytvoření posouzení](./media/tutorial-assess-physical/assessment-create.png)

9. Po vytvoření posouzení ho zobrazte na stránce **servery**  >  **Azure Migrate: vyhodnocení vyhodnocení serveru**  >  **Assessments**.
10. Vyberte **vyhodnocování exportu** a stáhněte ho jako soubor Microsoft Excelu.

Pokud chcete získat další podrobnosti o posouzení **Řešení Azure VMware (AVS)** , přečtěte si prosím [Toto](how-to-create-azure-vmware-solution-assessment.md). 

## <a name="review-an-azure-vm-assessment"></a>Kontrola posouzení virtuálních počítačů Azure

Posouzení virtuálního počítače Azure popisuje:

- **Připravenost na Azure**: jestli jsou servery vhodné pro migraci do Azure.
- **Odhad měsíčních nákladů**: Odhadované měsíční náklady na výpočetní prostředky a úložiště pro spouštění serverů v Azure.
- **Odhad měsíčních nákladů na úložiště**: Odhadované náklady na diskové úložiště po migraci.

### <a name="view-an-assessment"></a>Zobrazit posouzení

1. V případě **migrace na**  >  **serverech**vyberte **hodnocení** v **Azure Migrate: posouzení serveru**.
2. V **posouzení**vyberte posouzení, které chcete otevřít.

    ![Souhrn posouzení](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Kontrola připravenosti na Azure

1. V části **připravenost k Azure**určete, jestli jsou servery připravené na migraci do Azure.
2. Zkontrolujte stav:
    - **Připraveno pro Azure**: Azure Migrate doporučuje velikost virtuálního počítače a odhad nákladů pro virtuální počítače ve vyhodnocování.
    - **Připraveno s podmínkami**: zobrazuje problémy a navrhovanou nápravu.
    - **Nepřipraveno pro Azure**: zobrazuje problémy a navrhovanou nápravu.
    - **Připravenost není známa**: Azure Migrate nemůže vyhodnotit připravenost z důvodu problémů s dostupností dat.

3. Vyberte stav **připravenosti na Azure** . Můžete zobrazit podrobnosti o připravenosti serveru a přejít k podrobnostem a zobrazit podrobnosti o serveru, včetně výpočetních prostředků, úložiště a nastavení sítě.

### <a name="review-cost-details"></a>Podrobnosti o kontrole nákladů

Toto zobrazení ukazuje odhadované náklady na výpočetní prostředky a úložiště pro provozování virtuálních počítačů v Azure. Další možnosti:

- Projděte si měsíční náklady na výpočetní prostředky a úložiště. Náklady se sčítají pro všechny servery v hodnocené skupině.

    - Odhad nákladů vychází z doporučení na velikost počítače a jeho disků a vlastností.
    - Zobrazí se Odhadované měsíční náklady na výpočetní prostředky a úložiště.
    - Odhad nákladů slouží ke spuštění místních serverů jako virtuálních počítačů s IaaS (infrastruktura jako služba). Posouzení serveru nebere v úvahu náklady typu platforma jako služba (PaaS) nebo software jako služba (SaaS).

- Projděte si měsíční odhady nákladů na úložiště. Toto zobrazení ukazuje agregované náklady na úložiště pro vyhodnocenou skupinu rozdělené mezi různé typy disků úložiště.
- Přejděte k podrobnostem a zobrazte podrobnosti pro konkrétní virtuální počítače.

> [!NOTE]
> Hodnocení spolehlivosti není přiřazeno k posouzení serverů importovaných do posouzení serveru pomocí sdíleného svazku clusteru.

## <a name="supported-operating-system-names"></a>Podporované názvy operačních systémů

Názvy operačních systémů, které jsou zadány ve sdíleném svazku clusteru, musí odpovídat nebo obsahovat názvy v tomto seznamu. To je nezbytné pro názvy zadané jako platné pro posouzení.

<!-- BEGIN A - H -->

:::row:::
   :::column span="2":::
      **A-H**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Apple Mac OS X 10
   :::column-end:::
   :::column span="":::
      Asianux 3<br/>
      Asianux 4<br/>
      Asianux 5
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      CentOS<br/>
      CentOS 4/5
   :::column-end:::
   :::column span="":::
      CoreOS Linux
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Debian GNU/Linux 4<br/>
      Debian GNU/Linux 5<br/>
      Debian GNU/Linux 6<br/>
      Debian GNU/Linux 7<br/>
      Debian GNU/Linux 8
   :::column-end:::
   :::column span="":::
      FreeBSD
   :::column-end:::
:::row-end:::

<!-- BEGIN I - R -->

:::row:::
   :::column span="2":::
      **I-R**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      IBM OS/2
   :::column-end:::
   :::column span="":::
      systémem
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Novell NetWare 5<br/>
      Novell NetWare 6
   :::column-end:::
   :::column span="":::
      Oracle Linux<br/>
       Oracle Linux 4/5<br/>
      Oracle Solaris 10<br/>
       Oracle Solaris 11
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Red Hat Enterprise Linux 2<br/>
      Red Hat Enterprise Linux 3<br/>
      Red Hat Enterprise Linux 4<br/>
      Red Hat Enterprise Linux 5<br/>
      Red Hat Enterprise Linux 6<br/>
      Red Hat Enterprise Linux 7<br/>
      Red Hat Fedora
   :::column-end:::
:::row-end:::

<!-- BEGIN S - T -->

:::row:::
   :::column span="2":::
      **S-T**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      SCO OpenServer 5<br/>
      SCO OpenServer 6<br/>
      SCO UnixWare 7
   :::column-end:::
   :::column span="":::
      Serenity systémy eComStation 1<br/>
      Serenity systémy eComStation 2
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Systém Sun Microsystems Solaris 8<br/>
      Sun Microsystems Solaris 9
   :::column-end:::
   :::column span="":::
      SUSE Linux Enterprise 10<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE Linux Enterprise 12<br/>
      SUSE Linux Enterprise 8/9<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE openSUSE
   :::column-end:::
:::row-end:::

<!-- BEGIN U - Z -->
:::row:::
   :::column span="2":::
      **U-Z**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Ubuntu Linux
   :::column-end:::
   :::column span="":::
      VMware ESXi 4<br/>
      VMware ESXi 5<br/>
      VMware ESXi 6
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Windows 10<br/>
      Windows 2000<br/>
      Systém Windows 3<br/>
      Windows 7<br/>
      Windows 8<br/>
      Systém Windows 95<br/>
      Windows 98<br/>
      Systém Windows NT<br/>
      Windows Server (R) 2008<br/>
      Windows Server 2003
   :::column-end:::
   :::column span="":::
      Windows Server 2008<br/>
      Windows Server 2008 R2<br/>
      Windows Server 2012<br/>
      Windows Server 2012 R2<br/>
      Windows Server 2016<br/>
      Windows Server 2019<br/>
      Prahová hodnota pro Windows Server<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste:

> [!div class="checklist"]
> * Importované servery do Azure Migrate: posouzení serveru pomocí sdíleného svazku clusteru.
> * Bylo vytvořeno a zkontrolováno posouzení.

Nyní [Nasaďte zařízení](./migrate-appliance.md) pro přesnější posouzení a Shromážděte servery do skupin pro hlubší hodnocení pomocí [analýzy závislostí](./concepts-dependency-visualization.md).
