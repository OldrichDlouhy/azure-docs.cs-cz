---
title: Podpora pro posouzení fyzického serveru v Azure Migrate
description: Další informace o podpoře pro posouzení fyzického serveru pomocí Azure Migrateho posouzení serveru
ms.topic: conceptual
ms.date: 06/03/2020
ms.openlocfilehash: 3391a29b2297d192aad0d499f688105db9af528b
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87304322"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Matice podpory pro posouzení fyzického serveru 

Tento článek shrnuje požadavky a požadavky na podporu při vyhodnocování fyzických serverů pro migraci do Azure pomocí nástroje [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) Tool. Pokud chcete migrovat fyzické servery do Azure, přečtěte si [matici podpora migrace](migrate-support-matrix-physical-migration.md).


Chcete-li vyhodnotit fyzické servery, vytvořte projekt Azure Migrate a přidejte do projektu nástroj pro vyhodnocení serveru. Po přidání nástroje nasadíte [zařízení Azure Migrate](migrate-appliance.md). Zařízení nepřetržitě zjišťuje místní počítače a odesílá do Azure metadata počítače a data o výkonu. Po dokončení zjišťování shromáždíte zjištěné počítače do skupin a spustíte posouzení pro skupinu.


## <a name="limitations"></a>Omezení

**Podpora** | **Podrobnosti**
--- | ---
**Omezení hodnocení** | V jednom [Azure Migrate projektu](migrate-support-matrix.md#azure-migrate-projects)můžete zjistit a posoudit až 35 000 fyzických serverů.
**Omezení projektu** | V předplatném Azure můžete vytvořit více projektů. Kromě fyzických serverů může projekt zahrnovat virtuální počítače VMware a virtuální počítače Hyper-V, a to až do limitů hodnocení pro každý z nich.
**Zjišťování** | Zařízení Azure Migrate může zjistit až 1000 fyzických serverů.
**Posouzení** | Do jedné skupiny můžete přidat až 35 000 počítačů.<br/><br/> V jednom posouzení můžete vyhodnotit až 35 000 počítačů.

[Přečtěte si další informace](concepts-assessment-calculation.md) o posouzení.

## <a name="physical-server-requirements"></a>Požadavky na fyzický server

| **Podpora**                | **Podrobnosti**               
| :-------------------       | :------------------- |
| **Nasazení fyzického serveru**       | Fyzický server může být samostatný nebo nasazený v clusteru. |
| **Oprávnění**           | **Windows:** Použijte doménový účet pro počítače připojené k doméně a místní účet pro počítače, které jsou připojené k doméně. Uživatelský účet by měl být přidán do těchto skupin: Uživatelé vzdálené správy, sledování výkonu a uživatelé protokolu výkonu. <br/><br/> **Linux:** Na serverech se systémem Linux, které chcete zjistit, potřebujete kořenový účet. |
| **Operační systém** | Pro migraci se dají posoudit všechny operační systémy s výjimkou systémů Windows Server 2003 a SUSE Linux. |


## <a name="azure-migrate-appliance-requirements"></a>Požadavky zařízení Azure Migrate

Azure Migrate používá [zařízení Azure Migrate](migrate-appliance.md) ke zjišťování a hodnocení. Zařízení pro fyzické servery může běžet na VIRTUÁLNÍm počítači nebo na fyzickém počítači. 

- Přečtěte si informace o [požadavcích na zařízení](migrate-appliance.md#appliance---physical) pro fyzické servery.
- Přečtěte si o adresách URL, které zařízení potřebuje k přístupu ve [veřejných](migrate-appliance.md#public-cloud-urls) a [státních](migrate-appliance.md#government-cloud-urls) cloudech.
- Zařízení se nastavuje pomocí [skriptu PowerShellu](how-to-set-up-appliance-physical.md) , který stáhnete z Azure Portal.
V Azure Government nasaďte zařízení [pomocí tohoto skriptu](deploy-appliance-script-government.md).

## <a name="port-access"></a>Přístup k portu

Následující tabulka shrnuje požadavky na porty pro posouzení.

**Zařízení** | **Připojení**
--- | ---
**Náplně** | Příchozí připojení na portu TCP 3389 umožňující připojení ke vzdálené ploše zařízení.<br/><br/> Příchozí připojení na portu 44368 pro vzdálený přístup k aplikaci pro správu zařízení pomocí adresy URL:``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Odchozí připojení na portech 443 (HTTPS), která odesílají metadata zjišťování a výkonu Azure Migrate.
**Fyzické servery** | **Windows:** Příchozí připojení na portu WinRM 5985 (HTTP) k vyžádání metadat konfigurace a výkonu ze serverů Windows. <br/><br/> **Linux:**  Příchozí připojení na portu 22 (TCP) k vyžádání metadat konfigurace a výkonu ze serverů se systémem Linux. |

## <a name="agent-based-dependency-analysis-requirements"></a>Požadavky na analýzu závislostí na základě agentů

[Analýza závislostí](concepts-dependency-visualization.md) vám pomůže identifikovat závislosti mezi místními počítači, které chcete vyhodnotit a migrovat do Azure. Tabulka shrnuje požadavky na nastavení analýzy závislostí založené na agentech. Pro fyzické servery je aktuálně podporována pouze analýza závislostí založená na agentech.

**Požadavek** | **Podrobnosti** 
--- | --- 
**Před nasazením** | Měli byste mít Azure Migrate projekt, pomocí nástroje pro vyhodnocení serveru přidaný do projektu.<br/><br/>  Vizualizace závislostí nasadíte po nastavení zařízení Azure Migrate pro zjištění vašich místních počítačů.<br/><br/> [Naučte](create-manage-projects.md) se, jak poprvé vytvořit projekt.<br/> [Přečtěte si, jak](how-to-assess.md) přidat nástroj pro vyhodnocení do existujícího projektu.<br/> Naučte se, jak nastavit zařízení Azure Migrate pro posouzení [technologie Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)nebo fyzických serverů.
**Azure Government** | Vizualizace závislostí není v Azure Government k dispozici.
**Log Analytics** | Azure Migrate používá řešení [Service map](../azure-monitor/insights/service-map.md) v [protokolech Azure monitor](../azure-monitor/log-query/log-query-overview.md) pro vizualizaci závislostí.<br/><br/> K projektu Azure Migrate přidružíte nový nebo existující Log Analytics pracovní prostor. Pracovní prostor pro Azure Migrate projekt nelze po přidání změnit. <br/><br/> Pracovní prostor musí být ve stejném předplatném jako projekt Azure Migrate.<br/><br/> Pracovní prostor se musí nacházet v oblastech Východní USA, jihovýchodní Asie nebo Západní Evropa. Pracovní prostory v jiných oblastech nelze přidružit k projektu.<br/><br/> Pracovní prostor musí být v oblasti, ve které [je podporovaná Service map](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> V Log Analytics je pracovní prostor přidružený k Azure Migrate označený klíčem projektu migrace a názvem projektu.
**Vyžadovaná agenti** | Na každém počítači, který chcete analyzovat, nainstalujte následující agenty:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Agent závislostí](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Pokud nejsou místní počítače připojené k Internetu, musíte na ně stáhnout a nainstalovat bránu Log Analytics.<br/><br/> Přečtěte si další informace o instalaci [agenta závislostí](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) a [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Pracovní prostor Log Analytics** | Pracovní prostor musí být ve stejném předplatném jako projekt Azure Migrate.<br/><br/> Azure Migrate podporuje pracovní prostory, které jsou umístěné v oblastech Východní USA, jihovýchodní Asie a Západní Evropa.<br/><br/>  Pracovní prostor musí být v oblasti, ve které [je podporovaná Service map](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> Pracovní prostor pro Azure Migrate projekt nelze po přidání změnit.
**Náklady** | V řešení Service Map se neúčtují žádné poplatky za prvních 180 dní (od dne, kdy přidružíte pracovní prostor Log Analytics k projektu Azure Migrate)/<br/><br/> Po uplynutí 180 dnů se začnou účtovat standardní poplatky za Log Analytics.<br/><br/> Použití jiného řešení než Service Map v přidruženém pracovním prostoru Log Analytics bude účtovat [standardní poplatky](https://azure.microsoft.com/pricing/details/log-analytics/) za Log Analytics.<br/><br/> Když se projekt Azure Migrate odstraní, pracovní prostor se spolu s ním neodstraní. Po odstranění projektu Service Map využití není volné a každý uzel se bude účtovat podle placené úrovně Log Analytics pracovního prostoru/<br/><br/>Pokud máte projekty, které jste vytvořili před Azure Migrate všeobecné dostupnosti (GA-28 února 2018), mohly by vám být účtovány další Service Map poplatky. Aby se zajistila platba jenom po 180 dnech, doporučujeme vytvořit nový projekt, protože stávající pracovní prostory před GAm jsou stále Fakturovatelné.
**správy** | Při registraci agentů do pracovního prostoru použijete ID a klíč poskytnutý Azure Migrate projektem.<br/><br/> Pracovní prostor Log Analytics můžete použít mimo Azure Migrate.<br/><br/> Pokud odstraníte přidružený Azure Migrate projekt, pracovní prostor se automaticky neodstraní. [Odstraňte ji ručně](../azure-monitor/platform/manage-access.md).<br/><br/> Pokud neodstraníte projekt Azure Migrate, neodstraňujte pracovní prostor vytvořený pomocí Azure Migrate. Pokud to uděláte, funkce vizualizace závislosti nebude fungovat podle očekávání.
**Připojení k internetu** | Pokud nejsou počítače připojené k Internetu, musíte na ně nainstalovat bránu Log Analytics.
**Azure Government** | Analýza závislostí založená na agentech není podporována.

## <a name="next-steps"></a>Další kroky

[Příprava na posouzení fyzického serveru](tutorial-prepare-physical.md).
