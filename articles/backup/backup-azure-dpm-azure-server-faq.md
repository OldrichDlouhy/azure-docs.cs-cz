---
title: Nejčastější dotazy k Azure Backup Server a DPM
description: V tomto článku najdete odpovědi na běžné otázky týkající se Microsoft Azure Backupho serveru (MABS) a aplikace DPM (Data Protection Manager).
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/05/2019
ms.openlocfilehash: 35957a1e8a3d6c3d9be06d9d44dbcd47efa0e6ee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "74173163"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Backup Server a DPM – Nejčastější dotazy

## <a name="general-questions"></a>Obecné otázky

Tento článek obsahuje odpovědi na nejčastější dotazy týkající se Azure Backup Server a aplikace DPM.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server"></a>Mohu použít server Azure Backup k vytvoření zálohy úplného obnovení (BMR) pro fyzický server?

Ano.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Můžu server zaregistrovat do více trezorů?

Ne. Server DPM nebo Azure Backup lze zaregistrovat pouze do jednoho trezoru.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Můžu použít DPM k zálohování aplikací v Azure Stack?

Ne. Azure Backup můžete použít k ochraně Azure Stack, Azure Backup nepodporuje použití DPM k zálohování aplikací v Azure Stack.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Je-li nainstalován agent Azure Backup pro ochranu souborů a složek, je možné nainstalovat aplikaci System Center DPM pro zálohování místních úloh do Azure?

Ano. Měli byste ale nejdřív nastavit DPM a pak nainstalovat agenta Azure Backup.  Instalace součástí v tomto pořadí zajistí, že agent Azure Backup funguje s aplikací DPM. Instalace agenta před instalací aplikace DPM se nedoporučuje ani nepodporuje.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Proč po instalaci UR7 a nejnovějšího agenta Azure Backup nelze přidat externí server DPM?

Pro servery DPM se zdroji dat, které jsou chráněny do cloudu (pomocí kumulativní aktualizace starší než je kumulativní aktualizace 7), musíte počkat alespoň jeden den po instalaci UR7 a nejnovějšího agenta Azure Backup, abyste mohli začít **Přidat externí server DPM**. Pro nahrání metadat skupin ochrany DPM do Azure je nutné jednorázové časové období. Metadata skupiny ochrany se nahrají poprvé prostřednictvím noční úlohy.

## <a name="vmware-and-hyper-v-backup"></a>Zálohování VMware a Hyper-V

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Je možné zálohovat servery VMware vCenter do Azure?

Ano. Pomocí Azure Backup Server můžete zálohovat hostitele VMware vCenter Server a ESXi do Azure.

- [Přečtěte si další informace](backup-mabs-protection-matrix.md) o podporovaných verzích.
- Při zálohování serveru VMware [postupujte podle těchto kroků](backup-azure-backup-server-vmware.md) .

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Potřebuji samostatnou licenci k obnovení celého místního clusteru VMware/Hyper-V?

Pro ochranu VMware/Hyper-V nepotřebujete samostatnou licenci.

- Pokud jste zákazníkem nástroje System Center, použijte k ochraně virtuálních počítačů VMware aplikaci System Center Data Protection Manager (DPM).
- Pokud nejste zákazníkem nástroje System Center, můžete k ochraně virtuálních počítačů VMware použít Azure Backup Server (průběžné platby).

## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>Je možné obnovit položku SharePointu do původního umístění, pokud je server SharePoint nakonfigurován pomocí technologie AlwaysOn SQL (s ochranou na disku)?

Ano, položku lze obnovit na původní web služby SharePoint.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>Je možné obnovit databázi služby SharePoint do původního umístění, pokud je server SharePoint nakonfigurován pomocí technologie AlwaysOn systému SQL?

Vzhledem k tomu, že databáze služby SharePoint jsou nakonfigurovány v technologii SQL AlwaysOn, nelze je upravovat, pokud není odebrána skupina dostupnosti. V důsledku toho DPM nemůže obnovit databázi do původního umístění. Můžete obnovit databázi SQL Server do jiné instance SQL Server.

## <a name="next-steps"></a>Další kroky

Přečtěte si další nejčastější dotazy:

- [Přečtěte si další informace](backup-support-matrix-mabs-dpm.md) o Azure Backup Server a matrici podpory aplikace DPM.
- [Přečtěte si další informace](backup-azure-mabs-troubleshoot.md) o pokynech pro řešení potíží s Azure Backup Server a DPM.
