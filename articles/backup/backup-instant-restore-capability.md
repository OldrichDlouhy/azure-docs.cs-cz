---
title: Možnost okamžitého obnovení Azure
description: Možnosti a nejčastější dotazy ke službě Azure Instant Restore pro zásobník zálohování virtuálních počítačů, model nasazení Správce prostředků
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 04/23/2019
ms.openlocfilehash: 6225a7eb8198ffcca0d1e3e15f64e2b3c977dc16
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84248273"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Zlepšení výkonu zálohování a obnovení pomocí funkce Azure Backup pro okamžité obnovení

> [!NOTE]
> Na základě zpětné vazby od uživatelů přejmenováváme **zásobník záloh virtuálních počítačů v2** na **okamžité obnovení** , aby se snížila nejasnosti s funkcemi Azure Stack.
> Všichni uživatelé Azure Backup se teď upgradují na **okamžité obnovení**.

Nový model pro okamžité obnovení nabízí následující vylepšení funkcí:

* Možnost používat snímky provedené jako součást úlohy zálohování, která je k dispozici pro obnovení bez čekání na dokončení přenosu dat do trezoru. Před aktivací obnovení zkracuje dobu čekání na kopírování snímků do trezoru.
* Omezí časy zálohování a obnovení tím, že se snímky ponechají místně, a to ve výchozím nastavení po dobu dvou dnů. Tato výchozí hodnota uchování snímku se dá nakonfigurovat na libovolnou hodnotu od 1 do 5 dní.
* Podporuje velikosti disků až do 32 TB. Změna velikosti disků se Azure Backup nedoporučuje.
* Podporuje SSD úrovně Standard disky spolu s HDD úrovně Standard disky a SSD úrovně Premium disky.
* Možnost při obnovení použít pro původní účty úložiště (na disk) nespravované virtuální počítače Tato možnost je k dispozici i v případě, že virtuální počítač obsahuje disky distribuované mezi účty úložiště. Zrychluje operace obnovení pro širokou škálu konfigurací virtuálních počítačů.
* Pro zálohování virtuálních počítačů, které používají nespravované disky Premium v účtech úložiště s okamžitým obnovením, doporučujeme přidělit *50%* volného místa celkového přiděleného prostoru úložiště, který se vyžaduje **jenom** pro první zálohování. 50% volného místa není požadavkem na zálohování po dokončení prvního zálohování.

## <a name="whats-new-in-this-feature"></a>Co je nového v této funkci

V současné době se úloha zálohování skládá ze dvou fází:

1. Pořízení snímku virtuálního počítače
2. Přenos snímku virtuálního počítače do trezoru služby Azure Recovery Services

Bod obnovení se považuje za vytvořený teprve po dokončení obou fází. V rámci tohoto upgradu se okamžitě po dokončení snímku vytvoří bod obnovení typu Snímek, pomocí kterého je možné provést obnovení s využitím stejného postupu obnovení. Tento bod obnovení v Azure Portal můžete určit pomocí "snapshot" jako typ bodu obnovení a po přenosu snímku do trezoru se typ bodu obnovení změní na "Snapshot and trezor".

![Úloha zálohování v zásobníku zálohování virtuálních počítačů Správce prostředků modelu nasazení – úložiště a trezor](./media/backup-azure-vms/instant-rp-flow.png)

Ve výchozím nastavení se snímky uchovávají po dobu dvou dnů. Tato funkce umožňuje operaci obnovení z těchto snímků vyjmutím doby obnovení. Zkracuje dobu potřebnou k transformaci a zkopírování dat zpět z trezoru.

## <a name="feature-considerations"></a>Požadavky na funkce

* Snímky jsou uloženy spolu s disky pro zvýšení vytvoření bodu obnovení a urychlení operací obnovení. V důsledku toho se zobrazí náklady na úložiště, které odpovídají snímkům provedeným během tohoto období.
* Přírůstkové snímky se ukládají jako objekty blob stránky. Pro všechny uživatele, kteří používají nespravované disky, se účtují snímky uložené v místním účtu úložiště. Vzhledem k tomu, že kolekce bodů obnovení používané zálohami spravovaného virtuálního počítače používají snímky objektů blob na základní úrovni úložiště, budou se u spravovaných disků zobrazovat náklady odpovídající ceně snímku objektu BLOB a přírůstkové.
* Pro účty Premium Storage se snímky pořízené pro rychlé body obnovení počítají do limitu 10 TB přiděleného místa.
* Získáte možnost konfigurovat uchovávání snímků na základě potřeb obnovení. V závislosti na požadavku můžete nastavit uchování snímků na minimálně jeden den v okně zásady zálohování, jak je popsáno níže. To vám pomůže ušetřit náklady pro uchování snímků, pokud neprovádíte obnovení často.
* Po upgradu na okamžité obnovení se jedná o jeden směrový upgrade, který se nedá vrátit zpět.

>[!NOTE]
>U tohoto upgradu okamžitého obnovení bude doba uchování snímku všech zákazníků (**zahrnutá v seznamu nová i existující**) nastavená na výchozí hodnotu dva dny. Můžete ale nastavit dobu trvání podle vašeho požadavku na libovolnou hodnotu od 1 do 5 dní.

## <a name="cost-impact"></a>Dopad na náklady

Přírůstkové snímky se ukládají v účtu úložiště virtuálního počítače, který se používá pro okamžité obnovení. Přírůstkový snímek znamená, že místo obsazené snímkem je rovné místo obsazené stránkami, které jsou zapsány po vytvoření snímku. Fakturuje se pořád za využité místo na GB využité snímkem a cena za GB je stejná, jak je uvedeno na [stránce s cenami](https://azure.microsoft.com/pricing/details/managed-disks/). Pro virtuální počítače, které používají nespravované disky, se snímky můžou zobrazit v nabídce pro soubor VHD každého disku. U spravovaných disků se snímky ukládají v prostředku kolekce bodů obnovení v určené skupině prostředků a samotné snímky nejsou přímo viditelné.

>[!NOTE]
> Pro týdenní zásady je doba uchování snímku pevně nastavená na 5 dní.

## <a name="configure-snapshot-retention"></a>Konfigurace uchovávání snímků

### <a name="using-azure-portal"></a>Pomocí webu Azure Portal

V Azure Portal uvidíte pole přidané v okně **zásady zálohování virtuálního počítače** v části **okamžité obnovení** . Dobu uchování snímku můžete změnit v okně **zásady zálohování virtuálního počítače** pro všechny virtuální počítače přidružené ke konkrétním zásadám zálohování.

![Možnost okamžitého obnovení](./media/backup-azure-vms/instant-restore-capability.png)

### <a name="using-powershell"></a>Pomocí prostředí PowerShell

>[!NOTE]
> Z AZ PowerShell verze 1.6.0 a vyšší můžete aktualizovat dobu uchování snímku okamžitého obnovení v zásadách pomocí PowerShellu.

```powershell
$bkpPol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
$bkpPol.SnapshotRetentionInDays=5
Set-AzureRmRecoveryServicesBackupProtectionPolicy -policy $bkpPol
```

Výchozí uchování snímku pro každou zásadu je nastavené na dva dny. Uživatel může hodnotu změnit na minimálně 1 a maximálně na pět dní. Pro týdenní zásady je uchování snímku pevně nastavené na pět dní.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Jaké jsou náklady na okamžité obnovení?

Snímky se ukládají společně s disky pro urychlení operací vytváření a obnovení bodů obnovení. V důsledku toho se zobrazí náklady na úložiště, které odpovídají uchování snímku vybranému jako součást zásad zálohování virtuálních počítačů.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>U Premium Storage účtů mají snímky provedené pro rychlý bod obnovení zabírat limit snímku o velikosti 10 TB?

Ano, pro účty Premium Storage se snímky pořízené pro rychlý bod obnovení zabírají 10 TB přiděleného místa snímku.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Jak funguje uchovávání snímků během pětiletého období?

Každý den se vytvoří nový snímek a pak existuje pět jednotlivých přírůstkových snímků. Velikost snímku závisí na četnosti změn dat, které jsou ve většině případů přibližně 2% až 7%.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Je snímek okamžitého obnovení přírůstkovým nebo úplným snímkem?

Snímky, které se považují za součást možností okamžitého obnovení, jsou přírůstkové snímky.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Jak mohu vypočítat přibližné náklady zvýšené z důvodu funkce okamžitého obnovení?

Záleží na tom, jak virtuální počítač funguje. V stabilním stavu můžete předpokládat zvýšení nákladů, protože doba uchování snímku je denní Poměrová cena za úložiště na jeden virtuální počítač za GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Pokud je typ obnovení pro bod obnovení "Snímke and trezor" a provádím operaci obnovení, který typ obnovení bude použit?

Pokud je typ obnovení "snímke and trezor", obnoví se automaticky z místního snímku, což bude v porovnání s obnovením z trezoru mnohem rychlejší.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Co se stane, když vyberu dobu uchování bodu obnovení (vrstva 2) menší než doba uchování snímku (Tier1)?

Nový model nepovoluje odstranění bodu obnovení (2), pokud se neodstraní snímek (Tier1). Doporučujeme, abyste naplánujete dobu uchování bodu obnovení (2), která je delší než doba uchování snímku.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Proč je můj snímek existující i po nastavení Doba uchování v zásadách zálohování?

Pokud má bod obnovení snímek, který je nejnovějším dostupným RP, bude uchován až do doby, kdy bude k dispozici další úspěšná záloha. To je v souladu se zásadami navrženými pro uvolňování paměti (GC) ještě dnes, že pro případ, že se na virtuálním počítači ještě nezdařily všechny zálohy, má vždy k dispozici alespoň jeden nejnovější RP. V normálních scénářích se RPs vyčistí po dobu jejich vypršení platnosti po dobu 24 hodin.

### <a name="i-dont-need-instant-restore-functionality-can-it-be-disabled"></a>Nepotřebuji funkci okamžitého obnovení. Je možné ho zakázat?

Funkce okamžitého obnovení je povolená pro všechny uživatele a nedá se zakázat. Uchování snímku můžete snížit minimálně na jeden den.

>[!NOTE]
> **Azure Backup teď podporuje zálohování a obnovení selektivního disku pomocí řešení zálohování virtuálních počítačů Azure.**
>
>V současné době Azure Backup podporuje zálohování všech disků (operačního systému a dat) na virtuálním počítači společně s využitím řešení zálohování virtuálních počítačů. Díky funkci vyloučení disku získáte možnost zálohovat jeden nebo několik datových disků ve virtuálním počítači. To poskytuje efektivní a nákladově efektivní řešení pro potřeby zálohování a obnovení. Každý bod obnovení obsahuje data disků zahrnutých v operaci zálohování, která dále umožňuje mít v průběhu operace obnovení podmnožinu disků obnovených z daného bodu obnovení. To platí pro obnovení ze snímku i z trezoru.
>
>**Pokud si chcete zaregistrovat verzi Preview, napište nám naAskAzureBackupTeam@microsoft.com**
