---
title: Postup nahrazení páskové infrastruktury
description: Přečtěte si, jak Azure Backup poskytuje sémantiku podobnou pásce, která vám umožní zálohovat a obnovovat data v Azure.
ms.topic: conceptual
ms.date: 04/30/2017
ms.openlocfilehash: 71bfa86611b3a63fa6a28f60f80608b394cab5a3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86514182"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Přesunutí dlouhodobého úložiště z pásky do cloudu Azure

Zákazníci Azure Backup a System Center Data Protection Manager můžou:

* Zálohujte data v plánech, které nejlépe vyhovují potřebám organizace.
* Uchovávání zálohovaných dat po delší dobu.
* Zajistěte, aby Azure byla součástí svých dlouhodobých potřeb uchovávání dat (místo pásky).

Tento článek vysvětluje, jak můžou zákazníci povolit zásady zálohování a uchovávání informací. Zákazníci, kteří používají pásky k řešení svých potřeb dlouhodobé uchovávání, teď mají výkonnou a životaschopnou alternativu s dostupností této funkce. Tato funkce je povolená v nejnovější verzi Azure Backup (která je dostupná [tady](https://aka.ms/azurebackup_agent)). Před použitím aplikace DPM se službou Azure Backup musí zákazníci nástroje System Center DPM aktualizovat aspoň na DPM 2012 R2.

## <a name="what-is-the-backup-schedule"></a>Jaký je plán zálohování?

Plán zálohování indikuje četnost operace zálohování. Například nastavení na následující obrazovce označují, že zálohování probíhá denně na 18:00 a na půlnoci.

![Denní plán](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Zákazníci mohou také naplánovat týdenní zálohování. Například nastavení na následující obrazovce signalizují, že se zálohy přijímají každou alternativou neděle & středa v 9:10:30 a 1:10:00.

![Týdenní plán](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Co jsou zásady uchovávání informací?

Zásada uchovávání informací Určuje dobu, po kterou musí být záloha uložena. Místo toho, aby pro všechny body zálohy nezadali "ploché zásady", můžou zákazníci zadat různé zásady uchovávání informací na základě toho, kdy se zálohování provádí. Například bod zálohování vydaný každý den, který slouží jako bod obnovení, se zachová po dobu 90 dnů. Bod zálohování pořízený na konci každého čtvrtletí pro účely auditu se zachová delší dobu.

![Zásady uchovávání](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Celkový počet "bodů uchování" zadaných v této zásadě je 90 (denní body) + 40 (jedno čtvrtletí po dobu 10 let) = 130.

## <a name="example--putting-both-together"></a>Příklad – vložení společně

![Ukázková obrazovka](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Denní zásady uchovávání informací**: zálohování se ukládá na dny po dobu sedmi dnů.
2. **Týdenní zásady uchovávání informací**: zálohy provedené v půlnoci a 6 pm v sobotu se uchovávají po dobu čtyř týdnů.
3. **Měsíční zásady uchovávání informací**: zálohy provedené s půlnocí a 6 hodinami v poslední sobotu v měsíci se uchovávají po dobu 12 měsíců.
4. **Roční zásady uchovávání informací**: zálohy provedené při půlnoci na poslední sobotu v každé březnu se uchovávají po dobu 10 let.

Celkový počet "bodů uchovávání" (body, ze kterých může zákazník obnovit data) v předchozím diagramu se vypočítává takto:

* dva body za den po dobu sedmi dnů = 14 bodů obnovení
* dva body za týden po dobu čtyř týdnů = 8 bodů obnovení
* dva body za měsíc po dobu 12 měsíců = 24 bodů obnovení
* jeden bod za rok za 10 let = 10 bodů obnovení

Celkový počet bodů obnovení je 56.

> [!NOTE]
> Pomocí Azure Backup můžete vytvořit až 9999 bodů obnovení na chráněnou instanci. Chráněná instance je počítač, server (fyzický nebo virtuální) nebo úloha zálohující do Azure.
>

## <a name="advanced-configuration"></a>Pokročilá konfigurace

Když na předchozí obrazovce kliknete na **Upravit** , zákazníci mají větší flexibilitu při určování plánů uchovávání.

![Modify](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Další kroky

Další informace o Azure Backup najdete v tématech:

* [Seznámení s Azure Backup](./backup-overview.md)
* [Vyzkoušejte zálohování Azure](./backup-windows-with-mars-agent.md)
