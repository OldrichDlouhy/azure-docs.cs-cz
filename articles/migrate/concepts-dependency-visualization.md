---
title: Analýza závislostí v Azure Migrate Server Assessment
description: Popisuje, jak používat analýzu závislostí pro posouzení pomocí Azure Migrateho posouzení serveru.
ms.topic: conceptual
ms.date: 06/14/2020
ms.openlocfilehash: 386a8cefce722c4bff09e2a7fe6d25957630ff61
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118796"
---
# <a name="dependency-analysis"></a>Analýza závislostí

Tento článek popisuje analýzu závislostí v Azure Migrate: posouzení serveru.


Analýza závislostí identifikuje závislosti mezi zjištěnými místními počítači. Nabízí tyto výhody: 

- Můžete shromáždit počítače do skupin pro posouzení, přesněji a s větší jistotou.
- Můžete určit počítače, které musí být migrovány dohromady. To je zvlášť užitečné, pokud si nejste jistí, které počítače jsou součástí nasazení aplikace, které chcete migrovat do Azure.
- Můžete zjistit, jestli se počítače používají, a které počítače se místo migrace dají vyřadit z provozu.
- Analýza závislostí pomáhá zajistit, že nic nezůstane na konci, a proto se po migraci vyhnete výpadkům.
- [Přečtěte si](common-questions-discovery-assessment.md#what-is-dependency-visualization) běžné otázky k analýze závislostí.


## <a name="analysis-types"></a>Typy analýz

Pro nasazení analýzy závislostí existují dvě možnosti.

**Nastavení** | **Podrobnosti** | **Veřejný cloud** | **Azure Government**
----  |---- | ---- 
**Bez agenta** | Dotazuje data z virtuálních počítačů VMware pomocí rozhraní API vSphere.<br/><br/> Nemusíte instalovat agenty na virtuální počítače.<br/><br/> Tato možnost je v současnosti ve verzi Preview, jenom pro virtuální počítače VMware. | Podporuje se. | Podporuje se.
**Analýza založená na agentovi** | Nástroj používá [Service map řešení](../azure-monitor/insights/service-map.md) v Azure monitor, aby bylo možné povolit vizualizaci a analýzu závislostí.<br/><br/> Musíte nainstalovat agenty na každý místní počítač, který chcete analyzovat. | Podporuje se | Není podporováno.


## <a name="agentless-analysis"></a>Analýza bez agentů

Analýza závislostí bez agentů funguje tak, že zachytává data připojení TCP z počítačů, pro které je povolená. Na virtuálních počítačích nejsou nainstalované žádné agenty. Připojení ke stejnému zdrojovému serveru a procesu a cílový server, proces a port jsou logicky seskupeny do závislosti. Zachycená data závislosti můžete vizualizovat v zobrazení mapy nebo je exportovat jako sdílený svazek clusteru. Na počítačích, které chcete analyzovat, nejsou nainstalované žádné agenty.

### <a name="dependency-data"></a>Data závislostí

Po zahájení zjišťování dat závislostí začíná dotazování:

- Zařízení Azure Migrate se dotazuje data připojení TCP z počítačů každých pět minut, aby se shromáždila data.
- Data se shromažďují z virtuálních počítačů hosta prostřednictvím vCenter Server pomocí rozhraní API vSphere.
- Cyklické dotazování shromažďuje tato data:

    - Názvy procesů, které mají aktivní připojení.
    - Název aplikace, která spouští procesy, které mají aktivní připojení.
    - Cílový port aktivních připojení.

- Shromážděná data se zpracovávají na zařízení Azure Migrate, aby se odvodit informace o identitě a odesílaly se Azure Migrate každých 6 hodin.


## <a name="agent-based-analysis"></a>Analýza založená na agentovi

V případě analýzy založené na agentech používá posouzení serveru [Service map](../azure-monitor/insights/service-map.md) řešení v Azure monitor. Do každého počítače, který chcete analyzovat, nainstalujete [agenta Microsoft Monitoring Agent/Log Analytics](../azure-monitor/platform/agents-overview.md#log-analytics-agent) a [agenta závislostí](../azure-monitor/platform/agents-overview.md#dependency-agent).

### <a name="dependency-data"></a>Data závislostí

Analýza založená na agentech poskytuje tato data:

- Název zdrojového počítačového serveru, proces, název aplikace
- Název cílového počítačového serveru, proces, název aplikace a port.
- Pro Log Analytics dotazy se shromažďují a k dispozici informace o počtu připojení, latenci a přenosu dat. 



## <a name="compare-agentless-and-agent-based"></a>Porovnání bez agentů a založené na agentovi

Rozdíly mezi vizualizacemi bez agentů a vizualizací na základě agentů jsou shrnuty v tabulce.

**Požadavek** | **Bez agenta** | **Založené na agentovi**
--- | --- | ---
**Podpora** | Ve verzi Preview jenom pro virtuální počítače VMware. [Zkontrolujte](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) podporované operační systémy. | Obecně dostupná (GA).
**Agenta** | Na počítačích, které chcete analyzovat, nejsou potřeba žádní agenti. | Agenti vyžadovaná na každém místním počítači, který chcete analyzovat.
**Log Analytics** | Nepožadováno. | Azure Migrate používá řešení [Service map](../azure-monitor/insights/service-map.md) v [protokolech Azure monitor](../azure-monitor/log-query/log-query-overview.md) k analýze závislostí. 
**Proces** | Zachycuje data připojení TCP. Po zjištění se data shromáždí v intervalech po pěti minutách. | Agenti Service Map nainstalovaná na počítači shromažďují data o procesech TCP a příchozích a odchozích připojeních pro jednotlivé procesy.
**Data** | Název zdrojového počítačového serveru, proces, název aplikace<br/><br/> Název cílového počítačového serveru, proces, název aplikace a port. | Název zdrojového počítačového serveru, proces, název aplikace<br/><br/> Název cílového počítačového serveru, proces, název aplikace a port.<br/><br/> Pro Log Analytics dotazy se shromažďují a k dispozici informace o počtu připojení, latenci a přenosu dat. 
**Vizualizac** | Mapa závislostí jednoho serveru se dá zobrazit po dobu od 1 hodiny do 30 dnů. | Mapa závislostí pro jeden server.<br/><br/> Mapa závislostí skupiny serverů.<br/><br/>  Mapu lze zobrazit pouze za hodinu.<br/><br/> Přidejte nebo odeberte servery ve skupině z zobrazení mapy.
Export dat | Posledních 30 dní data je možné stáhnout ve formátu CSV. | Data se dají dotazovat pomocí Log Analytics.



## <a name="next-steps"></a>Další kroky

- [Nastavte](how-to-create-group-machine-dependencies.md) vizualizaci závislostí založenou na agentech.
- [Vyzkoušejte](how-to-create-group-machine-dependencies-agentless.md) vizualizaci závislostí bez agentů pro virtuální počítače VMware.
- Přečtěte si [běžné otázky](common-questions-discovery-assessment.md#what-is-dependency-visualization) k vizualizaci závislostí.
