---
title: Zálohování Azure File Share
description: Naučte se zálohovat sdílené složky Azure v trezoru Recovery Services.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 7441157d6346eefc256e9e7c29f9bb1fa5c13b79
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289521"
---
# <a name="about-azure-file-share-backup"></a>Zálohování Azure File Share

Zálohování sdílené složky Azure je nativní cloudové řešení zálohování, které chrání vaše data v cloudu a odstraňuje další režijní náklady spojené s místními řešeními zálohování. Služba Azure Backup hladce integruje s Azure File Sync a umožňuje centralizovat vaše data sdílené složky i zálohy. Toto jednoduché, spolehlivé a zabezpečené řešení umožňuje nakonfigurovat ochranu firemních sdílených složek v několika jednoduchých krocích s jistotou, že můžete obnovit data v případě jakéhokoli scénáře havárie.

## <a name="key-benefits-of-azure-file-share-backup"></a>Klíčové výhody zálohování Azure File Share

* **Nulová infrastruktura**: ke konfiguraci ochrany sdílených složek není nutné žádné nasazení.
* **Přizpůsobené uchovávání**: v závislosti na vašich požadavcích můžete nakonfigurovat zálohy s denním/týdenním/měsíčním/ročním uchováváním.
* **Integrované možnosti správy**: můžete naplánovat zálohování a zadat požadovanou dobu uchování bez další režie vyřazování dat.
* **Okamžité obnovení**: zálohování sdílené složky Azure používá snímky sdílené složky, takže můžete vybrat jenom soubory, které se mají okamžitě obnovit.
* **Výstrahy a vytváření sestav**: můžete nakonfigurovat výstrahy pro selhání zálohování a obnovení a použít řešení pro vytváření sestav, které poskytuje Azure Backup, abyste získali přehled o zálohách ve sdílených složkách souborů.
* **Ochrana před náhodným odstraněním sdílených složek**: Azure Backup povoluje [funkci obnovitelného odstranění](../storage/files/storage-files-prevent-file-share-deletion.md) na úrovni účtu úložiště s dobou uchování 14 dní. I když škodlivý objekt actor odstraní sdílenou složku, obsah sdílené složky a body obnovení (snímky) se uchovávají pro konfigurovatelnou dobu uchování, což umožňuje úspěšné a úplné obnovení zdrojového obsahu a snímků bez ztráty dat.

## <a name="architecture"></a>Architektura

![Architektura zálohování sdílené složky Azure](./media/azure-file-share-backup-overview/azure-file-shares-backup-architecture.png)

## <a name="how-the-backup-process-works"></a>Jak funguje proces zálohování

1. Prvním krokem při konfiguraci zálohování sdílených složek Azure je vytvoření trezoru služby Recovery Services. Trezor poskytuje konsolidované zobrazení záloh konfigurovaných napříč různými úlohami.

2. Po vytvoření trezoru služba Azure Backup zjistí účty úložiště, které se dají zaregistrovat v trezoru. Můžete vybrat účet úložiště hostující sdílené složky, které chcete chránit.

3. Po výběru účtu úložiště vypíše služba Azure Backup sadu sdílených složek v účtu úložiště a uloží jejich názvy v katalogu vrstev pro správu.

4. Pak nakonfigurujte zásady zálohování (plán a uchovávání) podle vašich požadavků a vyberte sdílené složky, které chcete zálohovat. Služba Azure Backup registruje plány v řídicí rovině pro plánované zálohování.

5. Na základě zadaných zásad Azure Backup Scheduler spustí zálohování v naplánovaném čase. V rámci této úlohy se snímek sdílené složky vytvoří pomocí rozhraní API pro sdílení souborů. V úložišti metadat je uložena pouze adresa URL snímku.

    >[!NOTE]
    >Data sdílené složky se nepřenášejí do služby zálohování, protože služba zálohování vytváří a spravuje snímky, které jsou součástí vašeho účtu úložiště, a zálohy se nepřenášejí do trezoru.

6. Obsah sdílené složky Azure (jednotlivé soubory nebo celou sdílenou složku) můžete obnovit ze snímků dostupných ve zdrojové sdílené složce. Po aktivaci operace se z úložiště metadat načte adresa URL snímku a data se zobrazí a přenesou ze zdrojového snímku do cílové sdílené složky, kterou si zvolíte. 

7. Pokud používáte Azure File Sync, služba zálohování indikuje službě Azure File Sync cestu k obnoveným souborům, které potom spustí proces detekce změn na pozadí u těchto souborů. Všechny soubory, které se změnily, se synchronizují s koncovým bodem serveru. K tomuto procesu dochází paralelně s původním obnovením sdílené složky Azure. 

8. Data sledování úloh zálohování a obnovení se odešlou do služby monitorování Azure Backup. To vám umožní monitorovat zálohy souborů v cloudu na jednom řídicím panelu. Kromě toho můžete nakonfigurovat také výstrahy nebo e-mailová oznámení v případě, že bude ovlivněn stav zálohování. E-maily se odesílají přes e-mailovou službu Azure.

## <a name="backup-costs"></a>Náklady na zálohování

V současné době se vám budou účtovat jenom snímky, protože zálohování sdílené složky Azure je řešení založené na snímcích. Poplatky za úložiště vynaložené pro snímky se účtují spolu se spotřebou souborů Azure [podle uvedených podrobností](https://azure.microsoft.com/pricing/details/storage/files/)o cenách.

## <a name="next-steps"></a>Další kroky

* Informace o [Zálohování sdílených složek Azure](backup-afs.md)
* Najít odpovědi na [otázky týkající se zálohování souborů Azure](backup-azure-files-faq.md)
