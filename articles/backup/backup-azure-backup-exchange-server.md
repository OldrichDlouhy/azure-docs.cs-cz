---
title: Zálohování Exchange serveru přes System Center DPM
description: Naučte se, jak zálohovat Exchange Server pro Azure Backup pomocí softwaru System Center 2012 R2 DPM.
ms.reviewer: kasinh
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 2d547b1d86b95a4f90d3faaa2f676c7cc37255d3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091126"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>Zálohování serveru Exchange do služby Azure Backup pomocí nástroje System Center 2012 R2 DPM

Tento článek popisuje, jak nakonfigurovat server System Center 2012 R2 Data Protection Manager (DPM) pro zálohování serveru Microsoft Exchange pro Azure Backup.  

## <a name="updates"></a>Aktualizace

Chcete-li úspěšně zaregistrovat server aplikace DPM pomocí Azure Backup, je nutné nainstalovat nejnovější kumulativní aktualizaci pro System Center 2012 R2 DPM a nejnovější verzi agenta Azure Backup. Získejte nejnovější kumulativní aktualizaci z [katalogu Microsoftu](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> V příkladech v tomto článku je nainstalována verze 2.0.8719.0 agenta Azure Backup a kumulativní aktualizace 6 je nainstalována na portálu System Center 2012 R2 DPM.
>
>

## <a name="prerequisites"></a>Předpoklady

Než budete pokračovat, ujistěte se, že byly splněny všechny [požadavky](backup-azure-dpm-introduction.md#prerequisites-and-limitations) pro použití Microsoft Azure Backup k ochraně úloh. Mezi tyto požadavky patří následující:

* Vytvořili jste úložiště záloh na webu Azure.
* Přihlašovací údaje agenta a trezoru se stáhly na server DPM.
* Agent je nainstalován na serveru DPM.
* Přihlašovací údaje trezoru se použily k registraci serveru DPM.
* Pokud chráníte Exchange 2016, upgradujte prosím na DPM 2012 R2 UR9 nebo novější.

## <a name="dpm-protection-agent"></a>Agent ochrany aplikace DPM

Chcete-li nainstalovat agenta ochrany aplikace DPM na server Exchange, postupujte podle následujících kroků:

1. Ujistěte se, že brány firewall jsou správně nakonfigurované. Viz [Konfigurace výjimek brány firewall pro agenta](/system-center/dpm/configure-firewall-settings-for-dpm?view=sc-dpm-2019).
2. Nainstalujte agenta na server Exchange kliknutím na **správa > agenti > instalaci** v konzola správce aplikace DPM. Podrobné pokyny najdete v tématu [instalace agenta ochrany aplikace DPM](/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019) .

## <a name="create-a-protection-group-for-the-exchange-server"></a>Vytvoření skupiny ochrany pro server Exchange

1. V Konzola správce aplikace DPM klikněte na **ochrana**a pak na pásu karet nástroje klikněte na **Nový** . otevře se průvodce **vytvořením nové skupiny ochrany** .
2. Na **úvodní** obrazovce průvodce klikněte na tlačítko **Další**.
3. Na obrazovce **Vybrat typ skupiny ochrany** vyberte **servery** a klikněte na **Další**.
4. Vyberte databázi systému Exchange Server, kterou chcete chránit, a klikněte na tlačítko **Další**.

   > [!NOTE]
   > Pokud chráníte Exchange 2013, podívejte se na [požadavky exchange 2013](/system-center/dpm/back-up-exchange).
   >
   >

    V následujícím příkladu je vybraná databáze Exchange 2010.

    ![Vybrat členy skupiny](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Vyberte metodu ochrany dat.

    Pojmenujte skupinu ochrany a pak vyberte obě z následujících možností:

   * Chci krátkodobou ochranu pomocí disku.
   * Chci online ochranu.
6. Klikněte na **Next** (Další).
7. Zaškrtněte možnost **Spustit Eseutil pro kontrolu integrity dat** , pokud chcete ověřit integritu databází serveru Exchange.

    Po výběru této možnosti se kontrola konzistence zálohy spustí na serveru DPM, aby se zabránilo vstupně-výstupnímu přenosu generovanému spuštěním příkazu **eseutil** na serveru Exchange.

   > [!NOTE]
   > Pokud chcete použít tuto možnost, musíte zkopírovat Ese.dll a Eseutil.exe soubory do adresáře C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin na serveru DPM. V opačném případě se aktivuje následující chyba:  
   > ![Chyba programu Eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klikněte na **Next** (Další).
9. Vyberte databázi pro **zálohování kopírováním**a pak klikněte na **Další**.

   > [!NOTE]
   > Pokud nevyberete možnost Úplná záloha pro alespoň jednu DAG kopii databáze, protokoly se nezkrátí.
   >
   >
10. Nakonfigurujte cíle pro **krátkodobé zálohování**a pak klikněte na **Další**.
11. Zkontrolujte dostupné místo na disku a potom klikněte na tlačítko **Další**.
12. Vyberte čas, kdy bude server DPM vytvořit počáteční replikaci, a poté klikněte na tlačítko **Další**.
13. Vyberte možnosti kontroly konzistence a potom klikněte na tlačítko **Další**.
14. Zvolte databázi, kterou chcete zálohovat do Azure, a potom klikněte na **Další**. Příklad:

    ![Zadat data online ochrany](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Zadejte plán pro **Azure Backup**a potom klikněte na tlačítko **Další**. Příklad:

    ![Zadat plán online zálohování](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Poznámka: body obnovení online jsou založené na expresním úplném bodu obnovení. Proto je nutné naplánovat online bod obnovení po uplynutí času zadaného pro expresní úplný bod obnovení.
    >
    >
16. Nakonfigurujte zásady uchovávání informací pro **Azure Backup**a pak klikněte na **Další**.
17. Vyberte možnost online replikace a klikněte na **Další**.

    Pokud máte rozsáhlou databázi, může trvat dlouhou dobu, než se počáteční záloha vytvoří po síti. Chcete-li se tomuto problému vyhnout, můžete vytvořit offline zálohování.  

    ![Zadat zásady uchovávání informací online](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Potvrďte nastavení a potom klikněte na **vytvořit skupinu**.
19. Klikněte na **Zavřít**.

## <a name="recover-the-exchange-database"></a>Obnovení databáze Exchange

1. Chcete-li obnovit databázi systému Exchange, klikněte na tlačítko **obnovit** v konzola správce aplikace DPM.
2. Vyhledejte databázi serveru Exchange, kterou chcete obnovit.
3. V rozevíracím seznamu *čas obnovení* vyberte bod obnovení online.
4. Kliknutím na tlačítko **obnovit** spusťte **Průvodce obnovením**.

Pro body obnovení online existuje pět typů obnovení:

* **Obnovit na původní umístění serveru Exchange Server:** Data budou obnovena do původního serveru Exchange.
* **Obnovit do jiné databáze na serveru Exchange Server:** Data budou obnovena do jiné databáze na jiném serveru Exchange.
* **Obnovit do databáze obnovení:** Data budou obnovena do databáze obnovení systému Exchange (RDB).
* **Kopírovat do síťové složky:** Data budou obnovena do síťové složky.
* **Kopírovat na pásku:** Pokud máte páskovou knihovnu nebo samostatnou páskovou jednotku připojenou a nakonfigurovanou na serveru DPM, bod obnovení se zkopíruje na volnou pásku.

    ![Zvolit online replikaci](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Další kroky

* [Nejčastější dotazy k Azure Backup](backup-azure-backup-faq.md)
