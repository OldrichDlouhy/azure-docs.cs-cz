---
title: Správa serverů a trezorů služby Azure Recovery Services
description: V tomto článku se dozvíte, jak pomocí řídicího panelu přehled služby Recovery Services trezor monitorovat a spravovat vaše trezory Recovery Services.
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 1b3cc317b8a26b16bbeda9793f87d089f2fbc59f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87054906"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Monitorování a správa trezorů služby Recovery Services

Tento článek vysvětluje, jak pomocí řídicího panelu **Přehled** služby Recovery Services trezor monitorovat a spravovat vaše trezory Recovery Services. Když v seznamu otevřete trezor Recovery Services, otevře se řídicí panel **Přehled** vybraného trezoru. Řídicí panel poskytuje různé podrobnosti o trezoru. K dispozici jsou *dlaždice* , které znázorňují stav kritických a varovných výstrah, probíhajících a neúspěšných úloh zálohování a množství místně redundantního úložiště (LRS) a geograficky redundantního úložiště (GRS). Pokud zálohujete virtuální počítače Azure do trezoru, na [dlaždici **stav předběžné kontroly zálohování** se zobrazí všechny kritické nebo varovné položky](#backup-pre-check-status). Následující obrázek je řídicí panel s **přehledem** pro **trezor společnosti Contoso**. Na dlaždici **zálohované položky** se zobrazí devět položek zaregistrovaných do trezoru.

![řídicí panel trezoru služby Recovery Services](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Požadavky pro tento článek jsou: předplatné Azure, Recovery Services trezor a že je pro trezor nakonfigurovaná aspoň jedna položka zálohování.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="open-a-recovery-services-vault"></a>Otevření trezoru Recovery Services

Pokud chcete monitorovat výstrahy nebo zobrazit data správy o trezoru Recovery Services, otevřete trezor.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/) pomocí svého předplatného Azure.

2. Na portálu klikněte na **všechny služby**.

   ![Otevřít seznam trezorů Recovery Services Krok 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. V dialogovém okně **všechny služby** zadejte **Recovery Services**. Seznam se průběžně filtruje podle zadávaného textu. Když se zobrazí možnost **trezory Recovery Services** , kliknutím na ni otevřete seznam trezorů Recovery Services ve vašem předplatném.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. V seznamu trezorů kliknutím na trezor otevřete jeho řídicí panel s **přehledem** .

    ![řídicí panel trezoru služby Recovery Services](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Řídicí panel přehled používá dlaždice pro poskytování výstrah a dat úloh zálohování.

## <a name="monitor-backup-jobs-and-alerts"></a>Monitorování úloh a upozornění zálohování

Řídicí panel **Přehled** služby Recovery Services trezor poskytuje dlaždice pro informace o monitorování a využití. Dlaždice v části monitorování zobrazují kritická upozornění a upozorňující výstrahy a probíhající a neúspěšné úlohy. Kliknutím na konkrétní výstrahu nebo úlohu otevřete nabídku zálohovací výstrahy nebo úlohy zálohování, která je filtrovaná pro danou úlohu nebo výstrahu.

![Úlohy řídicího panelu pro zálohování](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

V části monitorování se zobrazují výsledky předdefinovaných **výstrah zálohování** a dotazy na **úlohy zálohování** . Dlaždice monitorování poskytují aktuální informace o:

* Kritické výstrahy a upozornění na úlohy zálohování (za posledních 24 hodin)
* Stav předběžné kontroly pro virtuální počítače Azure Úplné informace o stavu předběžné kontroly najdete v tématu [stav předběžné kontroly zálohování](#backup-pre-check-status).
* Probíhající úlohy zálohování a úlohy, které selhaly (za posledních 24 hodin).

Dlaždice využití poskytují:

* Počet zálohovaných položek nakonfigurovaných pro trezor
* Služba Azure Storage (oddělená LRS a GRS) spotřebovaná trezorem.

Kliknutím na dlaždice (s výjimkou úložiště zálohování) otevřete přidruženou nabídku. Na obrázku výše na dlaždici zálohovací výstrahy se zobrazují tři kritické výstrahy. Kliknutím na řádek kritické výstrahy na dlaždici zálohovací výstrahy otevřete výstrahy zálohování, které se vyfiltrují pro kritické výstrahy.

![Nabídka výstrah zálohování je filtrovaná pro kritické výstrahy.](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

V nabídce výstrahy zálohování se na obrázku výše filtruje podle: stav je aktivní, závažnost je kritická a čas je uvedený za posledních 24 hodin.

### <a name="backup-pre-check-status"></a>Stav předběžné kontroly zálohování

Předběžná kontrola zálohování kontroluje v konfiguraci virtuálních počítačů problémy, které můžou negativně ovlivnit zálohy. Tyto informace jsou shrnuté, takže je můžete zobrazit přímo z řídicího panelu Recovery Services trezoru a poskytnout doporučení pro nápravná opatření, aby bylo zajištěno úspěšné zálohování konzistentní se soubory nebo aplikacemi. Nevyžadují žádnou infrastrukturu a nemají žádné další náklady.  

Předběžné kontroly zálohování se spouštějí jako součást plánovaných operací zálohování pro vaše virtuální počítače Azure. Dokončí jeden z následujících stavů:

* **Úspěch**: Tento stav označuje, že by konfigurace vašeho virtuálního počítače měla vést k úspěšným zálohám a není nutné provádět žádné nápravné akce.
* **Upozornění**: Tento stav označuje jeden nebo více problémů v konfiguraci virtuálního počítače, které *mohou* vést k selhání zálohování. Poskytuje *Doporučené* kroky k zajištění úspěšných záloh. Například pokud není nainstalován nejnovější agent virtuálního počítače, může dojít k výpadku zálohování. Tato situace by poskytovala stav upozornění.
* **Kritické**: Tento stav indikuje minimálně jeden kritický problém v konfiguraci virtuálního *počítače, který vede k* selhání zálohování, a poskytne *potřebné* kroky k zajištění úspěšných záloh. Například problém se sítí způsobený aktualizací pravidel NSG virtuálního počítače způsobí selhání zálohování, protože zabrání tomu, aby virtuální počítač komunikoval se službou Azure Backup. Tato situace by poskytovala kritický stav.

Postupujte podle následujících kroků a začněte řešit všechny problémy hlášené předběžnými kontrolami zálohování virtuálních počítačů ve vašem úložišti Recovery Services.

* Na řídicím panelu trezoru Recovery Services vyberte dlaždici **stav předběžné kontroly zálohování (virtuální počítače Azure)** .
* Vyberte libovolný virtuální počítač se stavem předběžné kontroly zálohování buď **kritického** , nebo **Upozornění**. Tato akce otevře podokno **Podrobnosti o virtuálním počítači** .
* V horní části podokna vyberte oznámení v podokně, které odhalí popis problému s konfigurací a nápravné kroky.

## <a name="manage-backup-alerts"></a>Spravovat výstrahy zálohování

Chcete-li získat přístup k nabídce výstrahy zálohování, v nabídce trezoru Recovery Services klikněte na možnost **zálohování výstrah**.

![Výstrahy zálohování](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Sestava výstrahy zálohování obsahuje výstrahy pro trezor.

![Výstrahy zálohování](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Výstrahy

V seznamu výstrahy zálohování se zobrazí vybrané informace pro filtrovaná upozornění. V nabídce výstrahy zálohování můžete vyfiltrovat kritické výstrahy nebo upozornění.

| Alert Level | Události, které generují výstrahy |
| ----------- | ----------- |
| Kritické | Zobrazí se důležité výstrahy, když se nezdaří úlohy zálohování, úlohy obnovení selžou a když zastavíte ochranu na serveru, ale zachováte data.|
| Upozornění | Upozornění se zobrazí, když se úlohy zálohování dokončí s upozorněními, například pokud je méně než 100 souborů nezálohovaných kvůli problémům s poškozením nebo při úspěšném zálohování více než 1 000 000 souborů. |
| Informační | v současné době se nepoužívají žádné informativní výstrahy. |

### <a name="viewing-alert-details"></a>Zobrazení podrobností výstrah

Sestava výstrahy zálohování sleduje osm podrobností o jednotlivých výstrahách. Použijte tlačítko **Vybrat sloupce** a upravte podrobnosti v sestavě.

![Výstrahy zálohování](./media/backup-azure-manage-windows-server/backup-alerts.png)

Ve výchozím nastavení se v sestavě zobrazí všechny podrobnosti, s výjimkou **času posledního výskytu**.

* Výstrahy
* Zálohovaná položka
* Chráněný Server
* Severity
* Doba trvání
* Čas vytvoření
* Status
* Čas posledního výskytu

### <a name="change-the-details-in-alerts-report"></a>Změna podrobností v sestavě výstrahy

1. Chcete-li změnit informace o sestavě, klikněte v nabídce **výstrahy zálohování** na možnost **Zvolit sloupce**.

   ![Výstrahy zálohování](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   Otevře se nabídka **Zvolit sloupce** .

2. V nabídce **Vybrat sloupce** vyberte podrobnosti, které chcete zobrazit v sestavě.

    ![Nabídka zvolit sloupce](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Kliknutím na **Hotovo** uložte změny a zavřete nabídku vybrat sloupce.

   Pokud provedete změny, ale nechcete změny zachovat, kliknutím na **obnovit** vraťte vybrané nastavení k poslední uložené konfiguraci.

### <a name="change-the-filter-in-alerts-report"></a>Změna filtru v sestavě výstrahy

Pomocí nabídky **Filtr** můžete změnit závažnost, stav, čas spuštění a čas ukončení pro výstrahy.

> [!NOTE]
> Když upravíte filtr výstrahy zálohování, nemění se kritická upozornění nebo upozornění na řídicím panelu přehled trezoru.
>  

1. Chcete-li změnit filtr výstrahy zálohování, klikněte v nabídce výstrahy zálohování na možnost **Filtr**.

   ![Výběr nabídky filtru](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Zobrazí se nabídka filtr.

   ![Výběr nabídky filtru](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Upravte závažnost, stav, čas spuštění nebo čas ukončení a klikněte na **Hotovo** a uložte provedené změny.

## <a name="configuring-notifications-for-alerts"></a>Konfigurace oznámení pro výstrahy

Konfigurace oznámení pro generování e-mailů při výskytu upozornění nebo kritické výstrahy. Můžete posílat e-mailová upozornění každou hodinu nebo v případě, že dojde k určité výstraze.

   ![Filtrovat výstrahy](./media/backup-azure-manage-windows-server/configure-notification.png)

Ve výchozím nastavení jsou e-mailová oznámení **zapnutá**. Kliknutím na **vypnout** zastavte e-mailová oznámení.

V ovládacím prvku **Upozornění** vyberte **výstrahu** v případě, že nechcete seskupení nebo nemá mnoho položek, které by mohly generovat výstrahy. Každá výstraha má za následek jedno oznámení (výchozí nastavení) a okamžitě se pošle e-mail s řešením.

Když vyberete **hodinový výtah**, pošle se příjemci e-mail s vysvětlením nevyřešených výstrah vygenerovaných za poslední hodinu. E-mail s rozlišením se pošle na konci hodiny.

Vyberte Závažnost výstrahy (kritickou nebo varovnou) použitou k vygenerování e-mailu. V současné době nejsou k dispozici žádné informační výstrahy.

## <a name="manage-backup-items"></a>Spravovat zálohované položky

Trezor Recovery Services obsahuje mnoho typů zálohovaných dat. [Přečtěte si další informace](backup-overview.md#what-can-i-back-up) o tom, co můžete zálohovat. Pokud chcete spravovat různé servery, počítače, databáze a úlohy, klikněte na dlaždici **zálohované položky** a zobrazte obsah trezoru.

![Dlaždice zálohované položky](./media/backup-azure-manage-windows-server/backup-items.png)

Otevře se seznam zálohovaných položek uspořádaných podle typu správy zálohování.

![seznam zálohovaných položek](./media/backup-azure-manage-windows-server/list-backup-items.png)

Pokud chcete prozkoumat konkrétní typ chráněné instance, klikněte na položku ve sloupci typ správy zálohování. Například na výše uvedeném obrázku existují dva virtuální počítače Azure chráněné v tomto trezoru. Kliknutím na **virtuální počítač Azure**otevřete seznam chráněných virtuálních počítačů v tomto trezoru.

![seznam typu zálohování](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

Seznam virtuálních počítačů má užitečná data: přidružená skupina prostředků, předchozí [záloha před kontrolou](#backup-pre-check-status), poslední stav zálohování a datum posledního bodu obnovení. Tři tečky v posledním sloupci otevřou nabídku pro aktivaci běžných úloh. Užitečná data, která jsou uvedena ve sloupcích, se liší pro každý typ zálohování.

![seznam typu zálohování](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Správa úloh zálohování

Dlaždice **úlohy zálohování** na řídicím panelu trezoru zobrazuje počet probíhajících úloh nebo selhání za posledních 24 hodin. Dlaždice poskytuje nakoukněte do nabídky úlohy zálohování.

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

Pokud chcete zobrazit další podrobnosti o úlohách, klikněte na **průběh** nebo **se nepodařilo** otevřít nabídku zálohovací úlohy, která je pro daný stav filtrovaná.

### <a name="backup-jobs-menu"></a>Nabídka úlohy zálohování

V nabídce **úlohy zálohování** se zobrazí informace o typu položky, operaci, stavu, času spuštění a době trvání.  

Chcete-li otevřít nabídku úlohy zálohování, v hlavní nabídce trezoru klikněte na **úlohy zálohování**.

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

Otevře se seznam úloh zálohování.

![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

V nabídce úlohy zálohování se zobrazuje stav všech operací pro všechny typy zálohování za posledních 24 hodin. Filtry změníte pomocí **filtru** . Filtry jsou vysvětleny v následujících částech.

Změna filtrů:

1. V nabídce zálohovací úlohy trezoru klikněte na **Filtr**.

   ![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Otevře se nabídka filtr.

   ![Zálohování položek z nastavení](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Vyberte nastavení filtru a klikněte na **Hotovo**. Filtrovaný seznam se aktualizuje v závislosti na novém nastavení.

#### <a name="item-type"></a>Typ položky

Typ položky je typ správy zálohování chráněné instance. Existují čtyři typy; Podívejte se na následující seznam. Můžete zobrazit všechny typy položek nebo jeden typ položky. Nemůžete vybrat dva nebo tři typy položek. Dostupné typy položek jsou:

* Všechny typy položek
* Virtuální počítač Azure
* Soubory a složky
* Azure Storage
* Zatížení Azure

#### <a name="operation"></a>Operace

Můžete zobrazit jednu operaci nebo všechny operace. Nemůžete vybrat dvě nebo tři operace. K dispozici jsou tyto operace:

* Všechny operace
* Registrovat
* Konfigurace zálohování
* Backup
* Obnovení
* Zakázat zálohování
* Odstranění zálohovaných dat

#### <a name="status"></a>Status

Můžete zobrazit všechny stavy nebo jeden. Nemůžete vybrat dva nebo tři stavy. Dostupné stavy jsou:

* Všechny stavy
* Dokončeno
* Rozpracované
* Neúspěšný
* Zrušeno
* Dokončeno s upozorněními

#### <a name="start-time"></a>Čas spuštění

Den a čas, kdy dotaz začíná. Výchozí hodnota je 24 hodinová perioda.

#### <a name="end-time"></a>Čas ukončení

Den a čas, kdy má dotaz končit.

### <a name="export-jobs"></a>Exportovat úlohy

Pomocí **úlohy exportu** můžete vytvořit tabulku obsahující informace nabídky všechny úlohy. Tabulka obsahuje jeden list, který obsahuje souhrn všech úloh a jednotlivých listů pro každou úlohu.

Pokud chcete exportovat informace o úlohách do tabulky, klikněte na **exportovat úlohy**. Služba vytvoří tabulku pomocí názvu trezoru a data, ale můžete změnit název.

## <a name="monitor-backup-usage"></a>Monitorování využití zálohy

Dlaždice úložiště zálohování na řídicím panelu zobrazuje úložiště spotřebované v Azure. Využití úložiště je k dispozici pro:

* Využití úložiště Cloud LRS přidružené k trezoru
* Využití úložiště Cloud GRS přidružené k trezoru

## <a name="troubleshooting-monitoring-issues"></a>Řešení potíží s monitorováním

**Problém:** Úlohy a výstrahy od agenta Azure Backup se nezobrazí na portálu.

**Postup řešení potíží:** Proces, ```OBRecoveryServicesManagementAgent``` odešle data úlohy a výstrahy do služby Azure Backup. Občas se může stát, že se tento proces zablokuje nebo vypne.

1. Pokud chcete ověřit, že proces není spuštěný, otevřete **Správce úloh**a zkontrolujte, jestli ```OBRecoveryServicesManagementAgent``` je spuštěný.

2. Pokud proces není spuštěn, otevřete **Ovládací panely**a procházejte seznam služeb. Spusťte nebo restartujte **agenta pro správu Microsoft Azure Recovery Services**.

    Další informace najdete v protokolech v těchto umístěních:<br/>
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Například:<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Další kroky

* [Obnovení Windows serveru nebo klienta Windows z Azure](backup-azure-restore-windows-server.md)
* Další informace o Azure Backup najdete v tématu [Azure Backup Overview](./backup-overview.md) .
