---
title: Nastavení analýzy závislostí bez agentů v serveru Azure Migrate Assessment
description: Nastavte analýzu závislostí bez agentů v Azure Migrate Server Assessment.
ms.topic: how-to
ms.date: 6/08/2020
ms.openlocfilehash: dc2ea0656198927cc8ae58533d296a2bedc37c13
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84771372"
---
# <a name="analyze-machine-dependencies-agentless"></a>Analýza závislostí počítačů (bez agentů)

Tento článek popisuje, jak nastavit analýzu závislostí bez agentů v Azure Migrate: posouzení serveru. [Analýza závislostí](concepts-dependency-visualization.md) vám pomůže identifikovat a pochopit závislosti mezi počítači pro účely posouzení a migrace do Azure.


> [!IMPORTANT]
> Vizualizace závislostí bez agentů je aktuálně ve verzi Preview pro virtuální počítače VMware zjištěné pomocí nástroje Azure Migrate: Server Assessment Tool.
> Funkce můžou být omezené nebo neúplné.
> Tuto verzi Preview pokrývá zákaznická podpora a je možné ji použít pro produkční úlohy.
> Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="current-limitations"></a>Aktuální omezení

- V zobrazení analýzy závislostí nemůžete aktuálně přidat nebo odebrat server ze skupiny.
- Mapování závislostí pro skupinu serverů není aktuálně k dispozici.
- Data závislosti nelze stáhnout v tabulkovém formátu.

## <a name="before-you-start"></a>Než začnete

- [Zkontrolujte](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) podporované operační systémy a požadovaná oprávnění.
- Ujistěte se, že:
    - Mít Azure Migrate projekt. Pokud to neuděláte, [vytvořte](how-to-add-tool-first-time.md) ho hned teď.
    - Ověřte, že jste [přidali](how-to-assess.md) Azure Migrate: Nástroj pro vyhodnocení serveru do projektu.
    - Nastavte [zařízení Azure Migrate](migrate-appliance.md) pro zjišťování místních počítačů. [Nastavte zařízení](how-to-set-up-appliance-vmware.md) pro virtuální počítače VMware. Zařízení zjišťuje místní počítače a odesílá data o metadatech a výkonu Azure Migrate: posouzení serveru.
- Ověřte, že na každém virtuálním počítači, který chcete analyzovat, je nainstalovaný nástroj VMware (novější než 10,2).


## <a name="create-a-user-account-for-discovery"></a>Vytvoření uživatelského účtu pro zjišťování

Nastavte uživatelský účet, aby vyhodnocování serveru mělo přístup k virtuálnímu počítači, aby bylo možné zjistit závislosti. [Přečtěte si informace](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) o požadavcích na účet pro virtuální počítače s Windows a Linux.


## <a name="add-the-user-account-to-the-appliance"></a>Přidat uživatelský účet do zařízení

Přidejte uživatelský účet do zařízení.

1. Otevřete aplikaci pro správu zařízení. 
2. Přejděte na panel **poskytnout podrobnosti vCenter** .
3. V nabídce **zjistit aplikaci a závislosti na virtuálních počítačích**klikněte na **Přidat přihlašovací údaje** .
3. Vyberte **operační systém**, zadejte popisný název účtu a heslo pro **uživatelské jméno** / **Password** .
6. Klikněte na **Uložit**.
7. Klikněte na **Uložit a spusťte zjišťování**.

    ![Přidat uživatelský účet VM](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Spustit zjišťování závislosti

Vyberte počítače, u kterých chcete povolit zjišťování závislostí.

1. V **Azure Migrate: vyhodnocování serveru**klikněte na **zjištěné servery**.
2. Klikněte na ikonu **Analýza závislostí** .
3. Klikněte na **Přidat servery**.
4. Na stránce **Přidat servery** vyberte zařízení, které zjišťuje příslušné počítače.
5. V seznamu počítač vyberte počítače.
6. Klikněte na **Přidat servery**.

    ![Spustit zjišťování závislosti](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Po spuštění zjišťování závislostí můžete vizualizovat závislosti přibližně po šesti hodinách.

## <a name="visualize-dependencies"></a>Vizualizace závislostí

1. V **Azure Migrate: vyhodnocování serveru**klikněte na **zjištěné servery**.
2. Vyhledejte počítač, který chcete zobrazit.
3. Ve sloupci **závislosti** klikněte na **Zobrazit závislosti.**
4. Změňte časové období, pro které chcete zobrazit mapu pomocí rozevíracího seznamu doba **Trvání** .
5. Rozbalte skupinu **klientů** pro výpis počítačů se závislostí na vybraném počítači.
6. Rozbalte skupinu **portů** a seznam počítačů, které mají závislost z vybraného počítače.
7. Chcete-li přejít k zobrazení mapy libovolného závislého počítače, klikněte na možnost název počítače > **načíst Server mapa** .

    ![Rozbalte položku Skupina portů serveru a mapa zátěžového serveru.](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Rozbalit skupinu klientů ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Rozbalte vybraný počítač pro zobrazení podrobností na úrovni procesu pro každou závislost.

    ![Rozbalte možnost Server a zobrazte procesy.](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Informace o procesu pro závislost není vždy k dispozici. Pokud není k dispozici, závislost je znázorněna s procesem označeným jako "Neznámý proces".

## <a name="export-dependency-data"></a>Exportovat data závislostí

1. V **Azure Migrate: vyhodnocování serveru**klikněte na **zjištěné servery**.
2. Klikněte na ikonu **Analýza závislostí** .
3. Klikněte na **exportovat závislosti aplikací**.
4. Na stránce **exportovat závislosti aplikací** vyberte zařízení, které zjišťuje příslušné počítače.
5. Vyberte čas spuštění a čas ukončení. Všimněte si, že data si můžete stáhnout jenom za posledních 30 dní.
6. Klikněte na **exportovat závislost**.

Data závislosti se exportují a stahují ve formátu CSV. Stažený soubor obsahuje data závislostí napříč všemi počítači, které jsou povoleny pro analýzu závislostí. 

![Exportovat závislosti](./media/how-to-create-group-machine-dependencies-agentless/export.png)

### <a name="dependency-information"></a>Informace o závislostech

Každý řádek exportovaného sdíleného svazku clusteru odpovídá závislosti zjištěné v určeném časovém intervalu. 

Následující tabulka shrnuje pole v exportovaném souboru CSV. Všimněte si, že pole název serveru, aplikace a proces jsou vyplněna pouze pro servery, které mají povolenou analýzu závislostí bez agenta.

**Název pole** | **Podrobnosti**
--- | --- 
Timeslot | Timeslot, během kterého byla závislost zjištěna. <br/> Data závislostí jsou v současnosti zachycena za 6 hodinových slotů.
Název zdrojového serveru | Název zdrojového počítače 
Zdrojová aplikace | Název aplikace na zdrojovém počítači 
Zdrojový proces | Název procesu na zdrojovém počítači 
Název cílového serveru | Název cílového počítače
Cílová IP adresa | IP adresa cílového počítače
Cílová aplikace | Název aplikace v cílovém počítači
Cílový proces | Název procesu na cílovém počítači 
Cílový port | Číslo portu na cílovém počítači


## <a name="stop-dependency-discovery"></a>Zastavit zjišťování závislosti

Vyberte počítače, u kterých chcete zastavit zjišťování závislostí.

1. V **Azure Migrate: vyhodnocování serveru**klikněte na **zjištěné servery**.
2. Klikněte na ikonu **Analýza závislostí** .
3. Klikněte na **odebrat servery**.
3. Na stránce **odebrat servery** vyberte **zařízení** , které zjišťuje virtuální počítače, na kterých jste se vyhledali zjišťování závislostí.
4. V seznamu počítač vyberte počítače.
5. Klikněte na **odebrat servery**.


## <a name="next-steps"></a>Další kroky

[Seskupit počítače](how-to-create-a-group.md) pro posouzení.
