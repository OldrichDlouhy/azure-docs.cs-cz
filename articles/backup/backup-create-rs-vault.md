---
title: Vytvoření a konfigurace trezorů Recovery Services
description: V tomto článku se dozvíte, jak vytvořit a nakonfigurovat trezory Recovery Services, které ukládají zálohy a body obnovení.
ms.topic: conceptual
ms.date: 05/30/2019
ms.custom: references_regions
ms.openlocfilehash: 244562efdc4c274a79ea27cdfa00dd51ae671fa4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87032948"
---
# <a name="create-and-configure-a-recovery-services-vault"></a>Vytvoření a konfigurace trezoru Recovery Services

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy"></a>Nastavit redundanci úložiště

Azure Backup automaticky zpracovává úložiště pro trezor. Musíte určit způsob replikace tohoto úložiště.

> [!NOTE]
> Změna **typu replikace úložiště** (místně redundantní/geograficky redundantní) pro trezor služby Recovery Services je potřeba provést před konfigurací záloh v trezoru. Když nakonfigurujete zálohování, možnost změnit je zakázaná.
>
>- Pokud jste ještě nenakonfigurovali zálohu, zkontrolujte a upravte nastavení [podle těchto pokynů](#set-storage-redundancy) .
>- Pokud jste už zálohu nakonfigurovali a musíte se přesunout z GRS na LRS, [Přečtěte si tato alternativní řešení](#how-to-change-from-grs-to-lrs-after-configuring-backup).

1. V okně **Trezory služby Recovery Services** klikněte na nový trezor. V části **Nastavení** klikněte na **vlastnosti**.
1. V části **vlastnosti**v části **Konfigurace zálohování**klikněte na **aktualizovat**.

1. Vyberte typ replikace úložiště a klikněte na **Uložit**.

     ![Nastavení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Pokud používáte Azure jako primární koncový bod úložiště záloh, doporučujeme, abyste používali výchozí **geograficky redundantní** nastavení.
   - Pokud Azure nepoužíváte jako primární koncový bod úložiště záloh, vyberte **Místně redundantní** – snížíte tím náklady na úložiště Azure.
   - Přečtěte si další informace o [geografické](../storage/common/storage-redundancy.md) a [místní](../storage/common/storage-redundancy.md) redundanci.

>[!NOTE]
>Nastavení replikace úložiště pro trezor nejsou relevantní pro zálohování sdílené složky Azure, protože aktuální řešení je založené na snímku a do trezoru se nepřenesla žádná data. Snímky se ukládají do stejného účtu úložiště jako zálohovaná sdílená složka.

## <a name="set-cross-region-restore"></a>Nastavení obnovení mezi oblastmi

Jedna z možností obnovení (CRR) umožňuje obnovení virtuálních počítačů Azure v sekundární oblasti, která je [spárována se službou Azure](../best-practices-availability-paired-regions.md). Tato možnost vám umožní:

- postupovat v případě, že dojde k auditu nebo požadavkům na dodržování předpisů
- Pokud dojde k havárii v primární oblasti, obnovte virtuální počítač nebo jeho disk.

Tuto funkci zvolíte tak, že v okně **Konfigurace zálohování** vyberete **Povolit obnovení mezi oblastmi** .

Pro tento proces se na úrovni úložiště účtují cenové dopady.

>[!NOTE]
>Než začnete:
>
>- Seznam podporovaných spravovaných typů a oblastí najdete v [matici podpory](backup-support-matrix.md#cross-region-restore) .
>- Funkce obnovení mezi oblastmi (CRR) je teď v současnosti zobrazená ve všech veřejných oblastech Azure.
>- CRR je funkce výslovných přihlášení na úrovni trezoru pro libovolný trezor GRS (ve výchozím nastavení vypnutý).
>- Po odsouhlasení může trvat až 48 hodin, než se zálohované položky zpřístupní v sekundárních oblastech.
>- Aktuálně se podporuje jenom CRR typu správy zálohování – ARM Azure (klasický virtuální počítač Azure se nepodporuje).  Když další typy správy podporují CRR, pak se **automaticky** zaregistrují.
>- Obnovení mezi oblastmi se v tuto chvíli nedá vrátit zpátky na GRS nebo LRS, jakmile se ochrana poprvé iniciuje.

### <a name="configure-cross-region-restore"></a>Konfigurace obnovení mezi oblastmi

Trezor vytvořený s redundancí GRS zahrnuje možnost konfigurace funkce obnovení mezi oblastmi. Každý trezor GRS bude obsahovat banner, který bude odkazovat na dokumentaci. Pokud chcete nakonfigurovat CRR pro trezor, otevřete okno Konfigurace zálohování, které obsahuje možnost povolit tuto funkci.

 ![Banner konfigurace zálohování](./media/backup-azure-arm-restore-vms/banner.png)

1. Na portálu klikněte na Recovery Services trezor > nastavení > vlastnosti.
2. Pokud chcete povolit funkci, klikněte na **Povolit obnovení mezi oblastmi v tomto trezoru** .

   ![Než kliknete na Povolit obnovení mezi oblastmi v tomto trezoru](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![Po kliknutí na Povolit obnovení mezi oblastmi v tomto trezoru](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

Přečtěte si, jak [Zobrazit zálohované položky v sekundární oblasti](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region).

Naučte se [obnovit v sekundární oblasti](backup-azure-arm-restore-vms.md#restore-in-secondary-region).

Naučte se [monitorovat úlohy obnovení sekundární oblasti](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs).

## <a name="modifying-default-settings"></a>Úprava výchozích nastavení

Důrazně doporučujeme před konfigurací záloh v trezoru zkontrolovat výchozí nastavení pro **typ replikace úložiště** a **nastavení zabezpečení** .

- **Typ replikace úložiště** je ve výchozím nastavení nastaven na **geograficky redundantní** (GRS). Po nakonfigurování zálohy bude možnost úprav zakázána.
  - Pokud jste ještě nenakonfigurovali zálohu, zkontrolujte a upravte nastavení [podle těchto pokynů](#set-storage-redundancy) .
  - Pokud jste už zálohu nakonfigurovali a musíte se přesunout z GRS na LRS, [Přečtěte si tato alternativní řešení](#how-to-change-from-grs-to-lrs-after-configuring-backup).

- **Obnovitelné odstranění** je ve výchozím nastavení **povolené** u nově vytvořených trezorů za účelem ochrany zálohovaných dat před náhodnými nebo škodlivými odstraněními. Chcete-li zkontrolovat a upravit nastavení, [postupujte podle těchto kroků](./backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete) .

### <a name="how-to-change-from-grs-to-lrs-after-configuring-backup"></a>Postup změny z GRS na LRS po konfiguraci zálohování

Než se rozhodnete přesunout z GRS do místně redundantního úložiště (LRS), Projděte si kompromisy mezi nižšími náklady a vyšší trvanlivostí dat, které vyhovují vašemu scénáři. Pokud je nutné přesunout z GRS do LRS, máte dvě možnosti. Jsou závislé na vašich obchodních požadavcích na uchovávání zálohovaných dat:

- [Nemusíte uchovávat předchozí zálohovaná data.](#dont-need-to-preserve-previous-backed-up-data)
- [Musí zachovat předchozí zálohovaná data.](#must-preserve-previous-backed-up-data)

#### <a name="dont-need-to-preserve-previous-backed-up-data"></a>Nemusíte uchovávat předchozí zálohovaná data.

Aby bylo možné chránit úlohy v novém trezoru LRS, bude nutné odstranit aktuální ochranu a data v trezoru GRS a znovu nakonfigurované zálohy.

>[!WARNING]
>Následující operace je destruktivní a nelze ji vrátit zpět. Všechna zálohovaná data a zálohované položky přidružené k chráněnému serveru budou trvale odstraněny. Postupujte však opatrně.

Zastavte a odstraňte aktuální ochranu v trezoru GRS:

1. Zakažte obnovitelné odstranění ve vlastnostech trezoru GRS. Chcete-li zakázat obnovitelné odstranění, postupujte podle [těchto kroků](backup-azure-security-feature-cloud.md#disabling-soft-delete-using-azure-portal) .

1. Zastavte ochranu a odstraňte zálohy z existujícího trezoru GRS. V nabídce řídicího panelu trezoru vyberte **zálohované položky**. Zde uvedené položky, které je třeba přesunout do trezoru LRS, je třeba odebrat spolu s jejich zálohovanými daty. Podívejte se, jak [Odstranit chráněné položky v cloudu](backup-azure-delete-vault.md#delete-protected-items-in-the-cloud) a [Odstranit chráněné položky místně](backup-azure-delete-vault.md#delete-protected-items-on-premises).

1. Pokud plánujete přesunout službu AFS (sdílené složky Azure), servery SQL nebo SAP HANA servery, budete je muset taky zrušit. V nabídce řídicího panelu trezoru vyberte **infrastruktura zálohování**. Podívejte se, jak [zrušit registraci SQL serveru](manage-monitor-sql-database-backup.md#unregister-a-sql-server-instance), [zrušit registraci účtu úložiště přidruženého ke sdíleným složkám Azure](manage-afs-backup.md#unregister-a-storage-account)a [zrušit registraci instance SAP HANA](sap-hana-db-manage.md#unregister-an-sap-hana-instance).

1. Po odebrání z trezoru GRS pokračujte v konfiguraci záloh pro vaše úlohy v novém trezoru LRS.

#### <a name="must-preserve-previous-backed-up-data"></a>Musí zachovat předchozí zálohovaná data.

Pokud potřebujete zachovat aktuální chráněná data v trezoru GRS a pokračovat v ochraně v novém trezoru LRS, existují pro některé úlohy omezené možnosti:

- V případě MARS můžete [Zastavit ochranu pomocí zachování dat](backup-azure-manage-mars.md#stop-protecting-files-and-folder-backup) a zaregistrovat agenta v novém trezoru LRS.

  - Služba Azure Backup bude nadále uchovávat všechny existující body obnovení trezoru GRS.
  - Abyste zachovali body obnovení v trezoru GRS, budete muset platit.
  - Zálohovaná data bude možné obnovit pouze v případě neplatných bodů obnovení v trezoru GRS.
  - V trezoru LRS se musí vytvořit nová počáteční replika dat.

- U virtuálního počítače Azure můžete [Zastavit ochranu pomocí zachování dat](backup-azure-manage-vms.md#stop-protecting-a-vm) pro virtuální počítač v trezoru GRS, přesunout virtuální počítač do jiné skupiny prostředků a pak chránit virtuální počítač v trezoru LRS. Přečtěte si [doprovodné materiály a omezení](../azure-resource-manager/management/move-limitations/virtual-machines-move-limitations.md) pro přesun virtuálního počítače do jiné skupiny prostředků.

  Virtuální počítač se dá v jednom okamžiku chránit jenom v jednom trezoru. Virtuální počítač v nové skupině prostředků ale můžete chránit v LRS trezoru, protože se považuje za jiný virtuální počítač.

  - Služba Azure Backup zachová body obnovení, které byly zálohovány v trezoru GRS.
  - Abyste zachovali body obnovení v trezoru GRS, bude nutné platit za to, že se budou Azure Backup zobrazovat informace o [cenách](azure-backup-pricing.md) .
  - V případě potřeby budete moci obnovit virtuální počítač z trezoru GRS.
  - První záloha v LRS trezoru virtuálního počítače v novém prostředku bude počáteční replikou.


## <a name="next-steps"></a>Další kroky

[Další informace](backup-azure-recovery-services-vault-overview.md) Recovery Services trezory.
[Další informace](backup-azure-delete-vault.md) Odstraňte trezory Recovery Services.
