---
title: Konfigurace sestav Azure Backup
description: Konfigurace a zobrazení sestav pro Azure Backup pomocí Log Analytics a sešitů Azure
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 5d1c7d628a61e550aa9dc4a5265ae16c5ed5336a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513621"
---
# <a name="configure-azure-backup-reports"></a>Konfigurace sestav Azure Backup

Běžným požadavkem pro správce zálohování je získat přehled o zálohách na základě dat, která jsou delší dobu. Případy použití takového řešení zahrnují:

- Přidělování a prognózování spotřebovaného cloudového úložiště.
- Auditování záloh a obnovení.
- Identifikujte klíčové trendy v různých úrovních členitosti.

Dnes Azure Backup poskytuje řešení pro vytváření sestav, které používá [protokoly Azure monitor](../azure-monitor/log-query/get-started-portal.md) a [sešity Azure](../azure-monitor/platform/workbooks-overview.md). Tyto prostředky vám pomůžou získat přehled o vašich zálohách napříč celou záloze. Tento článek vysvětluje, jak konfigurovat a zobrazovat sestavy Azure Backup.

## <a name="supported-scenarios"></a>Podporované scénáře

- Sestavy zálohování se podporují pro virtuální počítače Azure, SQL ve virtuálních počítačích Azure, SAP HANA ve virtuálních počítačích Azure, Microsoft Azure Recovery Services (MARS) agent, Microsoft Azure Backup Server (MABS) a System Center Data Protection Manager (DPM). Pro zálohování sdílené složky Azure se zobrazí data pro všechny záznamy vytvořené v nebo po 1. června 2020.
- Pro úlohy DPM jsou sestavy zálohování podporované pro DPM verze 5.1.363.0 a novější a verze agenta 2.0.9127.0 a vyšší.
- Pro úlohy MABS jsou sestavy zálohování podporované pro MABS verze 13.0.415.0 a vyšší a verze agenta 2.0.9170.0 a vyšší.
- Sestavy zálohování můžete zobrazit ve všech zálohovaných položkách, trezorech, předplatných a oblastech, pokud jsou data odesílána do Log Analyticsho pracovního prostoru, ke kterému má uživatel přístup. Chcete-li zobrazit sestavy pro sadu trezorů, stačí mít přístup čtenář k pracovnímu prostoru Log Analytics, do kterého trezory odesílají svá data. Nemusíte mít přístup k jednotlivým trezorům.
- Pokud jste uživatelem [Azure Lighthouse](../lighthouse/index.yml) , který má delegovaný přístup k předplatným vašich zákazníků, můžete pomocí těchto sestav s Azure Lighthouse zobrazit sestavy pro všechny klienty.
- V současné době je možné data zobrazit v sestavách zálohování v rámci maximálního počtu 100 Log Analytics pracovních prostorů (mezi klienty).
- Data pro úlohy zálohování protokolů aktuálně nejsou v sestavách zobrazená.

## <a name="get-started"></a>Začínáme

Chcete-li začít používat sestavy, postupujte podle těchto kroků.

### <a name="1-create-a-log-analytics-workspace-or-use-an-existing-one"></a>1. Vytvořte pracovní prostor Log Analytics nebo použijte existující.

Nastavte jeden nebo více pracovních prostorů Log Analytics pro uložení dat generování sestav zálohování. Umístění a předplatné, kde se dá tento Log Analytics pracovní prostor vytvořit, nezávisí na umístění a předplatném, kde existují vaše trezory.

Pokud chcete nastavit pracovní prostor Log Analytics, přečtěte si téma [Vytvoření pracovního prostoru Log Analytics v Azure Portal](../azure-monitor/learn/quick-create-workspace.md).

Ve výchozím nastavení jsou data v pracovním prostoru Log Analytics uchována po dobu 30 dnů. Chcete-li zobrazit data pro časový horizont delší dobu, změňte dobu uchování Log Analytics pracovního prostoru. Pokud chcete změnit dobu uchovávání, přečtěte si téma [Správa využití a nákladů pomocí protokolů Azure monitor](../azure-monitor/platform/manage-cost-storage.md).

### <a name="2-configure-diagnostics-settings-for-your-vaults"></a>2. Konfigurace nastavení diagnostiky pro vaše trezory

Azure Resource Manager prostředky, jako jsou například trezory Recovery Services, zaznamenávají informace o plánovaných operacích a uživatelem aktivované operace jako diagnostická data.

V části monitorování vašeho trezoru Recovery Services vyberte **nastavení diagnostiky** a zadejte cíl pro diagnostická data trezoru Recovery Services. Další informace o použití diagnostických událostí najdete v tématu [použití nastavení diagnostiky pro trezory Recovery Services](./backup-azure-diagnostic-events.md).

![Podokno nastavení diagnostiky](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup také poskytuje integrovanou Azure Policy definici, která automatizuje konfiguraci nastavení diagnostiky pro všechny trezory v daném oboru. Informace o tom, jak používat tuto zásadu, najdete v tématu [Konfigurace nastavení diagnostiky trezoru ve velkém měřítku](./azure-policy-configure-diagnostics.md).

> [!NOTE]
> Po dokončení konfigurace diagnostiky může trvat až 24 hodin, než se počáteční nabízení dat dokončí. Po zahájení toku dat do Log Analytics pracovního prostoru se data v sestavách nemusí okamžitě zobrazovat, protože v sestavách není zobrazená data pro aktuální částečný den. Další informace najdete v tématu [konvence používané v sestavách zálohování](#conventions-used-in-backup-reports). Doporučujeme, abyste začali zobrazovat sestavy dvou dnů po konfiguraci trezorů, aby odesílaly data do Log Analytics.

#### <a name="3-view-reports-in-the-azure-portal"></a>3. zobrazení sestav v Azure Portal

Po nakonfigurování trezorů pro posílání dat Log Analytics můžete zobrazit sestavy zálohování tak, že v podokně trezoru vyberete možnost **sestavy zálohování**.

![Řídicí panel trezoru](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Kliknutím na tento odkaz otevřete sešit zálohované sestavy.

> [!NOTE]
>
> - V současné době počáteční načtení sestavy může trvat až 1 minutu.
> - Trezor Recovery Services je pouze vstupním bodem pro sestavy zálohování. Po otevření sešitu sestavy zálohování z podokna trezoru vyberte příslušnou sadu Log Analytics pracovních prostorů, abyste viděli data agregovaná napříč všemi vašimi trezory.

Sestava obsahuje různé karty:

- **Shrnutí**: na této kartě můžete získat podrobný přehled o vaší nemovitosti k zálohování. Můžete získat rychlý přehled o celkovém počtu zálohovaných položek, celkovém využitém cloudovém úložišti, počtu chráněných instancí a četnosti úspěšnosti úlohy na jeden typ pracovního vytížení. Podrobnější informace o konkrétním typu artefaktu zálohování získáte, když přejdete na příslušné karty.

   ![Karta se shrnutím](./media/backup-azure-configure-backup-reports/summary.png)

- **Zálohované položky**: pomocí této karty můžete zobrazit informace a trendy v cloudovém úložišti spotřebovaném na úrovni záložních položek. Pokud například použijete SQL v zálohování virtuálního počítače Azure, můžete zobrazit cloudové úložiště spotřebované pro každou zálohovanou databázi SQL. Můžete se také rozhodnout, že se budou zobrazovat data pro záložní položky určitého stavu ochrany. Například když vyberete dlaždici **zastavená ochrana** v horní části karty, vyfiltruje všechny widgety pod tím, aby zobrazovaly data pouze pro zálohované položky ve stavu ochrana zastaveno.

   ![Karta zálohované položky](./media/backup-azure-configure-backup-reports/backup-items.png)

- **Použití**: na této kartě můžete zobrazit parametry fakturace klíče pro vaše zálohy. Informace zobrazené na této kartě jsou na úrovni fakturačních entit (chráněných kontejnerů). Například v případě zálohování serveru DPM do Azure můžete zobrazit trend chráněných instancí a cloudového úložiště spotřebovaného pro server DPM. Podobně platí, že pokud použijete SQL ve Azure Backup nebo SAP HANA v Azure Backup, tato karta poskytuje informace související s využitím na úrovni virtuálního počítače, ve kterém jsou tyto databáze obsažené.

   ![Karta použití](./media/backup-azure-configure-backup-reports/usage.png)

> [!NOTE]
> U úloh aplikace DPM se uživatelům může v porovnání s hodnotou agregovaného využití zobrazit lehký rozdíl (z pořadí 20 MB na server DPM) mezi hodnotami využití zobrazenými v sestavách, jak je znázorněno na kartě Přehled trezoru služby Recovery Services. Tento rozdíl je vydaný faktem, že každý server DPM, který se zaregistruje pro zálohování, má přidružený zdroj dat metadata, který není povrchový jako artefakt pro vytváření sestav.

- **Úlohy**: na této kartě můžete zobrazit dlouhotrvající trendy na úlohách, jako je počet neúspěšných úloh za den a nejvyšší příčiny selhání úlohy. Tyto informace můžete zobrazit jak na agregované úrovni, tak na úrovni záložních položek. Vyberte konkrétní položku zálohy v mřížce pro zobrazení podrobných informací o každé úloze, která byla aktivována u dané zálohované položky ve vybraném časovém rozsahu.

   ![Karta úlohy](./media/backup-azure-configure-backup-reports/jobs.png)

- **Zásady**: na této kartě můžete zobrazit informace o všech aktivních zásadách, například o počtu přidružených položek a celkovém cloudovém úložišti spotřebovanému položkami zálohovanými v rámci dané zásady. Vyberte konkrétní zásadu pro zobrazení informací o všech přidružených zálohovaných položkách.

   ![Karta Zásady](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="export-to-excel"></a>Exportovat do aplikace Excel

Vyberte tlačítko se šipkou dolů v pravém horním rohu libovolného widgetu, jako je tabulka nebo graf, a exportujte obsah tohoto widgetu jako excelový list, protože se používá existující filtry. Chcete-li exportovat více řádků tabulky do aplikace Excel, můžete zvýšit počet řádků zobrazených na stránce pomocí šipky rozevíracího seznamu **řádky na stránce** v horní části každé mřížky.

## <a name="pin-to-dashboard"></a>Připnout na řídicí panel

Vyberte tlačítko připnout v horní části každého widgetu a připněte pomůcku na řídicí panel Azure Portal. Tato funkce vám pomůže vytvořit přizpůsobené řídicí panely, které budou zobrazovat nejdůležitější informace, které potřebujete.

## <a name="cross-tenant-reports"></a>Sestavy mezi klienty

Pokud používáte [Azure Lighthouse](../lighthouse/index.yml) s delegovaným přístupem k předplatným napříč více klientskými prostředími, můžete použít výchozí filtr předplatného. Vyberte tlačítko Filtr v pravém horním rohu Azure Portal a zvolte všechna předplatná, pro která chcete zobrazit data. Díky tomu můžete v rámci svých klientů vybrat Log Analytics pracovní prostory pro zobrazení víceklientské sestav.

## <a name="conventions-used-in-backup-reports"></a>Konvence používané v sestavách zálohování

- Filtry fungují zleva doprava a shora dolů na každé kartě. To znamená, že libovolný filtr platí pouze pro všechny widgety, které jsou umístěny vpravo od daného filtru nebo pod tímto filtrem.
- Výběr barevné dlaždice filtruje widgety pod dlaždicí pro záznamy, které se vztahují k hodnotě této dlaždice. Když například vyberete dlaždici **ochrana zastaveno** na kartě **zálohované položky** , vyfiltrují se v níže uvedené mřížce a grafy data zálohované položky ve stavu ochrana zastaveno.
- Dlaždice, u kterých není barva, nelze kliknout.
- Data pro aktuální částečný den nejsou uvedena v sestavách. Takže když je vybraná hodnota **časového rozsahu** **Poslední 7 dní**, v sestavě se zobrazí záznamy za posledních sedmi dokončených dní. Aktuální den není zahrnutý.
- Tato sestava obsahuje podrobnosti o úlohách (kromě úloh protokolu), které se *aktivovaly* ve vybraném časovém rozsahu.
- Hodnoty zobrazené pro **cloudové úložiště** a **chráněné instance** jsou na *konci* vybraného časového rozsahu.
- Záložní položky zobrazené v sestavách jsou ty položky, které existují na *konci* vybraného časového rozsahu. Zálohované položky, které se odstranily uprostřed vybraného časového rozsahu, se nezobrazí. Stejné konvence platí i pro zásady zálohování.

## <a name="query-load-times"></a>Doba načítání dotazů

Widgety v sestavě zálohování využívají dotazy Kusto, které se spouštějí v pracovních prostorech Log Analytics uživatele. Tyto dotazy obvykle zahrnují zpracování velkých objemů dat s více spojeními, které umožňují rozsáhlejší přehledy. V důsledku toho se widgety nemusí okamžitě načíst, když uživatel zobrazí sestavy v rámci rozsáhlého zálohování. Tato tabulka poskytuje přibližný odhad doby, kterou mohou různé widgety načítat, a to na základě počtu zálohovaných položek a časového rozsahu, pro který sestavu prohlížíte.

| **Počet zdrojů dat**                         | **Časový horizont** | **Přibližná doba načítání**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~ 5 K                       | 1 měsíc          | Dlaždice: 5-10 sekund <br> Mřížky: 5-10 sekund <br> Grafy: 5-10 sekund <br> Filtry na úrovni sestavy: 5-10 sekund|
| ~ 5 K                       | 3 měsíce          | Dlaždice: 5-10 sekund <br> Mřížky: 5-10 sekund <br> Grafy: 5-10 sekund <br> Filtry na úrovni sestavy: 5-10 sekund|
| ~ 10 K                       | 3 měsíce          | Dlaždice: 15-20 sekund <br> Mřížky: 15-20 sekund <br> Grafy: 1-2 min. <br> Filtry na úrovni sestavy: 25-30 sekund|
| ~ 15 K                       | 1 měsíc          | Dlaždice: 15-20 sekund <br> Mřížky: 15-20 sekund <br> Grafy: 50-60 sekund <br> Filtry na úrovni sestavy: 20-25 sekund|
| ~ 15 K                       | 3 měsíce          | Dlaždice: 20-30 sekund <br> Mřížky: 20-30 sekund <br> Grafy: 2-3 min. <br> Filtry na úrovni sestavy: 50-60 sekund |

## <a name="what-happened-to-the-power-bi-reports"></a>Co se stalo se sestavami Power BI? 

- Předchozí Power BI App Template pro vytváření sestav, která zdrojová data z účtu služby Azure Storage, je na cestě k vyřazení. Pro zobrazení sestav doporučujeme spustit odesílání diagnostických dat trezoru pro Log Analytics.

- Kromě toho [schéma v1](./backup-azure-diagnostics-mode-data-model.md#v1-schema-vs-v2-schema) odesílání diagnostických dat do účtu úložiště nebo v pracovním prostoru La je také na cestě k vyřazení. To znamená, že pokud jste napsali vlastní dotazy nebo automatizace založené na schématu V1, doporučujeme tyto dotazy aktualizovat tak, aby používaly aktuálně podporované schéma v2.

## <a name="next-steps"></a>Další kroky

[Další informace o monitorování a vytváření sestav pomocí Azure Backup](./backup-azure-monitor-alert-faq.md)
