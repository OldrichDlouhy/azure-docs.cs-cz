---
title: Základní hodnoty zabezpečení Azure pro Azure Data Factory
description: Základní hodnoty zabezpečení Azure pro Azure Data Factory
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 771f692ade9c7616eb7d217b4728a8c80d2aeac5
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044171"
---
# <a name="azure-security-baseline-for-azure-data-factory"></a>Základní hodnoty zabezpečení Azure pro Azure Data Factory

Základní plán zabezpečení Azure pro Azure Data Factory obsahuje doporučení, která vám pomůžou vylepšit stav zabezpečení vašeho nasazení.

Základní hodnota této služby se vykreslí z [bezpečnostního testu Azure Security 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview), který poskytuje doporučení k zabezpečení cloudových řešení v Azure s využitím našich osvědčených postupů.

Další informace najdete v tématu [Přehled standardních hodnot zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Zabezpečení sítě

*Další informace najdete v tématu [řízení zabezpečení: zabezpečení sítě](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: Ochrana prostředků Azure v rámci virtuálních sítí

**Doprovodné**materiály: při vytváření Azure-SSIS Integration runtime (IR) máte možnost je připojit k virtuální síti. To umožní Azure Data Factory vytvořit určité síťové prostředky, jako je například skupina zabezpečení sítě (NSG) a nástroj pro vyrovnávání zatížení. Máte také možnost poskytnout vlastní statickou IP adresu nebo ji Azure Data Factory vytvořit. V NSG, který je automaticky vytvořený pomocí Azure Data Factory, je port 3389 ve výchozím nastavení otevřený pro veškerý provoz. Tento zámek zajistěte, aby měli přístup jenom správci.

V místním počítači nebo virtuálním počítači Azure v rámci virtuální sítě se dá nasadit samoobslužné úřad pro místní hostování. Ujistěte se, že nasazení podsítě virtuální sítě má NSG nakonfigurovaný tak, aby povolovalo jenom přístup pro správu. Azure-SSIS IR ve výchozím nastavení v pravidle brány Windows Firewall u každého uzlu IR pro ochranu nepovolil odchozí port 3389. Prostředky nakonfigurované ve virtuální síti můžete zabezpečit přidružením NSG k podsíti a nastavením striktních pravidel.

Pokud je k dispozici privátní odkaz, použijte privátní koncové body k zabezpečení všech prostředků, které jsou propojeny s kanálem Azure Data Factory, jako je například Azure SQL Server. Pomocí privátního propojení se provoz mezi vaší virtuální sítí a službou přechází přes páteřní síť Microsoftu, což eliminuje expozici veřejného Internetu.

* [Vytvoření Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

* [Postup vytvoření a konfigurace prostředí IR pro místní hostování](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)

* [Vytvoření Virtual Network](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Připojení Azure-SSIS IR k virtuální síti](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network#virtual-network-configuration)

* [Principy privátního odkazu Azure](https://docs.microsoft.com/azure/private-link/private-link-overview)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: Sledujte a protokolujte konfiguraci a provoz virtuálních sítí, podsítí a síťových karet

**Doprovodné**materiály: použijte Azure Security Center a opravte doporučení k ochraně sítě pro virtuální síť a skupinu zabezpečení sítě přidruženou k vašemu nasazení Integration runtime.

Kromě toho povolte protokoly toku NSG (Network Security Group) pro NSG ochranu nasazení Integration Runtime a odešlete protokoly do Azure Storage účtu pro auditování provozu.

Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

* [Pochopení zabezpečení sítě, které poskytuje Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Pochopení zabezpečení sítě, které poskytuje Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="13-protect-critical-web-applications"></a>1,3: Chraňte kritické webové aplikace

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro služby Azure apps nebo výpočetní prostředky hostující webové aplikace.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: zakažte komunikaci se známými škodlivými IP adresami.

**Doprovodné**materiály: ve virtuálních sítích, které jsou přidružené k vašemu nasazení Integration runtime, povolte DDoS Protection Standard, aby se mohla chránit před distribuovanými útoky DoS (Denial of Service). K odepření komunikace se známými škodlivými nebo nepoužívanými internetovými IP adresami použijte Azure Security Center integrovanou analýzu hrozeb.

* [Jak nakonfigurovat DDoS Protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Pochopení Azure Security Center integrované analýzy hrozeb](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="15-record-network-packets"></a>1,5: zaznamenání síťových paketů

**Pokyny**: povolení protokolů Flow skupiny zabezpečení sítě (NSG) pro NSG ochranu nasazení Integration runtime a odesílání protokolů do účtu Azure Storage pro auditování provozu.

Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Pochopení zabezpečení sítě, které poskytuje Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: nasazení systémů ochrany před internetovými útoky/systémy prevence vniknutí (ID/IP adresy)

**Doprovodné**materiály: Pokud chcete zkontrolovat odchozí provoz z Azure-SSIS IR, můžete směrovat provoz iniciované z Azure-SSIS IR na místní zařízení brány firewall prostřednictvím vynuceného tunelového připojení Azure ExpressRoute nebo k síťovému virtuálnímu zařízení (síťové virtuální zařízení) z Azure Marketplace podporujícího možnosti identifikátorů/IP adres. Pokud zjišťování neoprávněných vniknutí nebo prevence na základě kontroly zatížení není požadavkem, je možné použít Azure Firewall s analýzou hrozeb.

* [Připojení Azure-SSIS Integration Runtime k virtuální síti](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

* [Postup nasazení Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Postup konfigurace výstrah pomocí Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Správa provozu do webových aplikací

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro služby Azure apps nebo výpočetní prostředky hostující webové aplikace.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: Minimalizujte složitost a administrativní režii pravidel zabezpečení sítě

**Pokyny**: pomocí značek služby virtuální sítě definujte řízení přístupu k síti na skupině zabezpečení sítě (NSG) nebo Azure firewall. Značky služeb můžete používat místo konkrétních IP adres při vytváření pravidel zabezpečení. Zadáním názvu značky služby (např. DataFactoryManagement) v příslušném zdrojovém nebo cílovém poli pravidla můžete povolit nebo odepřít příchozí přenosy pro odpovídající službu. Společnost Microsoft spravuje předpony adres, které jsou součástí značky služby, a automaticky aktualizuje označení služby jako adresy změny.

* [Pochopení a použití značek služeb](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Pochopení Azure Data Factory specifických značek služeb](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: Udržujte standardní konfigurace zabezpečení pro síťová zařízení.

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro nastavení sítě a síťové prostředky přidružené k instancím služby Azure Data Factory pomocí Azure Policy. Pomocí aliasů Azure Policy v oborech názvů Microsoft. DataFactory a Microsoft. Network můžete vytvářet vlastní zásady pro auditování nebo prosazování konfigurace sítě vašich Azure Data Factorych instancí. Můžete také využít integrované definice zásad týkající se sítě nebo instancí služby Azure Data Factory, například:
- Měla by být povolená DDoS Protection Standard.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Ukázky Azure Policy pro sítě](https://docs.microsoft.com/azure/governance/policy/samples/#network)

* [Vytvoření Azure Blueprint](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="110-document-traffic-configuration-rules"></a>1,10: pravidla pro konfiguraci provozu dokumentu

**Doprovodné**materiály: používejte značky pro prostředky, které se týkají zabezpečení sítě a toku provozu pro vaše Azure Data Factory instance pro poskytování metadat a logické organizace.

Použijte některou z předdefinovaných Azure Policy definic souvisejících s označováním, jako je "vyžadovat značku a její hodnotu", aby se zajistilo, že všechny prostředky budou vytvořeny pomocí značek, a upozorní vás na stávající neoznačené prostředky.

Pomocí Azure PowerShell nebo Azure CLI můžete vyhledávat nebo provádět akce s prostředky na základě jejich značek.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: pomocí automatizovaných nástrojů monitorujte konfigurace síťových prostředků a zjišťují změny.

**Pokyny**: pomocí protokolu aktivit Azure můžete monitorovat konfigurace síťových prostředků a zjišťovat změny síťových prostředků, které souvisejí s vašimi Azure Data Factory instancemi. Vytvoří výstrahy v rámci Azure Monitor, které se aktivují, když budou provedeny změny v kritických síťových prostředcích.

* [Jak zobrazit a načíst události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Vytváření výstrah v Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

## <a name="logging-and-monitoring"></a>Protokolování a monitorování

*Další informace najdete v tématu [řízení zabezpečení: protokolování a monitorování](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: Použijte schválené zdroje synchronizace času

**Pokyny**: Společnost Microsoft udržuje zdroj času používaný pro prostředky Azure, například Azure Data Factory pro časová razítka v protokolech.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Konfigurace centrální správy protokolů zabezpečení

**Pokyny**: ingestování protokolů prostřednictvím Azure monitor k agregaci dat zabezpečení vygenerovaných Azure Data Factory. V rámci Azure Monitor se můžete dotazovat na pracovní prostor Log Analytics, který je nakonfigurovaný tak, aby přijímal protokoly aktivit Azure Data Factory. Použijte účty Azure Storage pro dlouhodobé nebo archivní úložiště protokolů nebo centra událostí pro export dat do jiných systémů.

Alternativně můžete povolit a začlenit data do Azure Sentinel nebo incidentu zabezpečení a řízení událostí zabezpečení (SIEM) třetí strany. Můžete také integrovat Azure Data Factory s Git a využít několik výhod správy zdrojového kódu, jako je například schopnost sledovat nebo auditovat změny a schopnost vracet změny, které zavádějí chyby.

* [Postup konfigurace nastavení diagnostiky](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Jak začít s Azure Monitor a integrací SIEM třetích stran](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

* [Správa zdrojového kódu v Azure Data Factory](https://docs.microsoft.com/azure/data-factory/source-control)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: povolení protokolování auditu pro prostředky Azure

**Doprovodné**materiály: pro protokolování auditu řídicích rovin povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do Log Analytics pracovního prostoru, Azure Event Hubs nebo Azure Storage účtu pro archivaci. Pomocí dat protokolu aktivit Azure můžete určit "co, kdo a kdy" pro všechny operace zápisu (PUT, POST, DELETE) prováděné na úrovni ovládacího prvku pro vaše prostředky Azure.

Pomocí nastavení diagnostiky můžete nakonfigurovat diagnostické protokoly pro nevýpočetní prostředky v Azure Data Factory, jako jsou metriky a data spuštění kanálů. Azure Data Factory ukládá data o běhu kanálu po dobu 45 dnů. Pokud chcete tato data po delší dobu uchovávat, uložte diagnostické protokoly do účtu úložiště pro auditování nebo ruční kontrolu a zadejte dobu uchování ve dnech. Protokoly můžete také streamovat do Azure Event Hubs nebo je odeslat do Log Analytics pracovního prostoru pro účely analýzy.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Vysvětlení diagnostických protokolů Azure Data Factory](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: shromáždění protokolů zabezpečení z operačních systémů

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, můžete pomocí Azure monitor shromažďovat data z virtuálního počítače. Když nainstalujete rozšíření Log Analytics VM, umožníte Azure Monitor shromažďovat data z vašich virtuálních počítačů Azure. Azure Security Center pak může pro Virtual Machines poskytnout monitorování protokolu událostí zabezpečení. Daný objem dat, který protokol událostí zabezpečení generuje, není ve výchozím nastavení uložen.

Pokud by vaše organizace měla uchovávat data protokolu událostí zabezpečení, může být uložená v rámci úrovně shromažďování dat, v takovém případě se může dotazovat na Log Analytics.

* [Jak shromažďovat data z Azure Virtual Machines v Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

* [Povolení shromažďování dat v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="25-configure-security-log-storage-retention"></a>2,5: Konfigurace uchovávání úložiště protokolu zabezpečení

**Doprovodné**materiály: povolení nastavení diagnostiky pro Azure Data Factory. Pokud se rozhodnete ukládat protokoly v pracovním prostoru Log Analytics, nastavte dobu uchování pracovního prostoru Log Analytics podle předpisů pro dodržování předpisů vaší organizace. Používejte účty Azure Storage pro dlouhodobé a archivační úložiště.

* [Postup povolení diagnostických protokolů v Azure Data Factory](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#set-up-diagnostic-logs)

* [Postup nastavení parametrů uchovávání protokolů pro Log Analytics pracovní prostory](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="26-monitor-and-review-logs"></a>2,6: Sledujte a kontrolujte protokoly

**Doprovodné**materiály: Povolte nastavení diagnostiky pro Azure Data Factory a odešlete protokoly do pracovního prostoru Log Analytics. Pomocí Log Analytics můžete analyzovat a monitorovat protokoly pro neobvyklé chování a pravidelně kontrolovat výsledky. Ujistěte se také, že jste povolili nastavení diagnostiky pro všechna úložiště dat související s nasazením Azure Data Factory. Návod, jak povolit nastavení diagnostiky, najdete v základních hodnotách zabezpečení jednotlivých služeb.

Pokud používáte Integration Runtime na virtuálním počítači Azure (VM), povolte taky nastavení diagnostiky pro virtuální počítač.

Alternativně můžete povolit a začlenit data do Azure Sentinel nebo SIEM třetí strany.

* [Log Analytics schéma](https://docs.microsoft.com/azure/data-factory/monitor-using-azure-monitor#schema-of-logs-and-events)

* [Jak shromažďovat data z virtuálního počítače Azure pomocí Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: povolení výstrah pro aktivity neobvyklé

**Pokyny**: upozornění na podporované metriky můžete vygenerovat v Data Factory v &amp; části metriky výstrahy v Azure monitor.

Nakonfigurujte nastavení diagnostiky pro Azure Data Factory a odešlete protokoly do pracovního prostoru Log Analytics. V rámci pracovního prostoru Log Analytics nakonfigurujte výstrahy, které se mají provést, když dojde k předdefinované sadě podmínek. Alternativně můžete povolit a začlenit data do Azure Sentinel nebo SIEM třetí strany.

Navíc se ujistěte, že jste povolili nastavení diagnostiky pro služby související s vašimi úložišti dat. Pokyny najdete v jednotlivých standardních hodnotách zabezpečení služby.

* [Výstrahy v Azure Data Factory](https://docs.microsoft.com/azure/data-factory/monitor-visually#alerts)

* [Stránka všechny podporované metriky](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)

* [Postup konfigurace výstrah v pracovním prostoru Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="28-centralize-anti-malware-logging"></a>2,8: centralizace protokolování proti malwaru

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, můžete použít Microsoft Antimalware pro Azure Cloud Services a Virtual Machines a nakonfigurovat své virtuální počítače tak, aby protokoloval události na účet Azure Storage. Nakonfigurujte Log Analytics pracovní prostor pro ingestování událostí z účtů úložiště a v případě potřeby vytvořte výstrahy. Použijte doporučení v Azure Security Center: "COMPUTE &amp; Apps".

* [Jak nakonfigurovat Microsoft Antimalware pro Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Jak povolit monitorování na úrovni hosta pro Virtual Machines](https://docs.microsoft.com/azure/cost-management/azure-vm-extended-metrics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="29-enable-dns-query-logging"></a>2,9: povolení protokolování dotazů DNS

**Doprovodné**materiály: nepoužitelné; Azure Data Factory nezpracovává ani nevytváří protokoly související se službou DNS.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="210-enable-command-line-audit-logging"></a>2,10: povolení protokolování auditu příkazového řádku

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, můžete povolit protokolování auditu z příkazového řádku. Azure Security Center poskytuje monitorování protokolu událostí zabezpečení pro virtuální počítače Azure. Security Center zřídí Microsoft Monitoring Agent na všech podporovaných virtuálních počítačích Azure a všech nově vytvořených, pokud je povolené Automatické zřizování, nebo můžete agenta nainstalovat ručně. Agent umožňuje událost vytvoření procesu 4688 a pole CommandLine v události 4688. Nové procesy vytvořené na virtuálním počítači se zaznamenávají protokolem událostí a monitorují služby zjišťování Security Center.

* [Shromažďování dat v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="identity-and-access-control"></a>Identita a řízení přístupu

*Další informace najdete v tématu [řízení zabezpečení: identita a řízení přístupu](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: udržování inventáře účtů pro správu

**Doprovodné**materiály: v rámci Azure Data Factory se ujistěte, že pravidelně sledujete a slučujete přístup uživatelů. Pro vytvoření instancí služby Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem role přispěvatel nebo vlastník nebo správcem předplatného Azure.

Na úrovni tenanta Azure Active Directory (AD) navíc předdefinované role, které se musí explicitně přiřadit a které se Queryable. Pomocí modulu Azure AD PowerShell můžete provádět ad hoc dotazy a zjišťovat účty, které jsou členy skupin pro správu, které mají přístup pro správu k rovině řízení Azure Data Factory instancí.

I když je služba Azure AD doporučovanou metodou správy uživatelského přístupu, pamatujte, že pokud používáte Integration Runtime na virtuálním počítači Azure, může mít virtuální počítač také místní účty. Místní i doménové účty by měly být přezkoumány a spravovány, normálně s minimálními nároky. Kromě toho doporučujeme, aby byl správce privilegované identity zkontrolován pro funkci just in time, aby se snížila dostupnost oprávnění správce.

* [Role a oprávnění pro službu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

* [Informace o Privileged identity Manageru](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Jak získat roli adresáře ve službě Azure AD pomocí PowerShellu](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Jak načíst členy role adresáře v Azure AD pomocí PowerShellu](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

* [Informace pro místní účty](https://docs.microsoft.com/azure/active-directory/devices/assign-local-admin#manage-the-device-administrator-role)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="32-change-default-passwords-where-applicable"></a>3,2: Změna výchozích hesel tam, kde je to možné

**Pokyny**: Azure Data Factory používá Azure Active Directory (AD) k poskytnutí přístupu k Azure Portal a také ke konzole Azure Data Factory. Azure AD nemá koncept výchozích hesel, ale zodpovídáte za změnu nebo povolení výchozích hesel pro jakékoli vlastní aplikace nebo aplikace třetích stran.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: použijte vyhrazené účty pro správu.

**Doprovodné**materiály: vytvořte standardní operační postupy kolem použití vyhrazených účtů pro správu pro přístup k rovině ovládacích prvků Azure (Azure Portal) a také ke konzole Azure Data Factory. Pomocí Azure Security Center správy identit a přístupu můžete monitorovat počet účtů pro správu v rámci služby Azure AD.

Kromě toho můžete použít doporučení z Azure Security Center nebo integrovaných zásad Azure, jako je například:
- K vašemu předplatnému by měl být přiřazený víc než jeden vlastník.
- Zastaralé účty s oprávněním vlastníka by se měly odebrat z vašeho předplatného.
- Z vašeho předplatného byste měli odebrat externí účty s oprávněním vlastníka.

Pokud používáte Integration Runtime na virtuálním počítači Azure, účty správců v Azure Virtual Machines se dají nakonfigurovat taky pomocí Azure Privileged identity Manageru (PIM). Azure Privileged Identity Manager nabízí několik možností, jako jsou možnosti zvýšení úrovně oprávnění, Multi-Factor Authentication a delegování, takže oprávnění jsou dostupná jenom pro konkrétní časové rámce a vyžadují druhou osobu ke schválení.

* [Pochopení Azure Security Center identity a přístupu](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Jak používat Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Informace o Privileged identity Manageru](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Role a oprávnění pro službu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: použijte jednotné přihlašování (SSO) s Azure Active Directory

**Pokyny**: použijte registraci aplikace Azure (instanční objekt) k získání tokenu, který může vaše aplikace nebo funkce použít pro přístup k trezorům Recovery Services a k interakci s nimi.

* [Volání rozhraní Azure REST API](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Postup registrace klientské aplikace (instančního objektu) ve službě Azure AD](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)

* [Informace o rozhraní API pro Azure Recovery Services](https://docs.microsoft.com/rest/api/recoveryservices/)

* [Informace o REST API pro Azure Data Factory](https://docs.microsoft.com/rest/api/datafactory/)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: Používejte vícefaktorové ověřování pro veškerý přístup založený na Azure Active Directory

**Doprovodné**materiály: povolte Azure Active Directory Multi-Factor Authentication (MFA) a sledujte Azure Security Center doporučení pro správu identit a přístupu.

* [Jak povolit vícefaktorové ověřování v Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Jak monitorovat identitu a přístup v rámci Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: Používejte vyhrazené počítače (privilegovaný přístup k pracovní stanici) pro všechny úlohy správy

**Pokyny**: použití pracovních stanic s privilegovaným přístupem (privilegovaným přístupem) s nakonfigurovaným Multi-Factor Authentication (MFA), které jsou nakonfigurovány pro přihlášení a konfiguraci prostředků Azure.

* [Další informace o pracovních stanicích s privilegovaným přístupem](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Jak povolit vícefaktorové ověřování v Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: protokolovat a upozornit na podezřelé aktivity z účtů pro správu

**Doprovodné**materiály: použití sestav zabezpečení Azure Active Directory pro generování protokolů a výstrah v případě, že v prostředí dojde k podezřelé nebo nebezpečné aktivitě. Pomocí Azure Security Center můžete monitorovat aktivitu identity a přístupu.

Pokud vaše Integration Runtime spouštíte na virtuálním počítači Azure, můžete také na kartě připojit virtuální počítač k Azure Sentinel. Microsoft Azure Sentinel je škálovatelná, cloudová, nativní, SIEM (Security Information Management) a automatizované řešení pro orchestraci zabezpečení (společnosti). Služba Azure Sentinel nabízí inteligentní analýzu zabezpečení a analýzu hrozeb napříč podnikem a poskytuje jediné řešení pro detekci výstrah, viditelnost hrozeb, proaktivní lov a reakci na hrozby.

* [Jak identifikovat uživatele Azure AD označené příznakem rizika pro rizikové aktivity](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Jak monitorovat identitu uživatelů a aktivity přístupu v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Postup zprovoznění služby Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Správa prostředků Azure pouze ze schválených umístění

**Pokyny**: pomocí pojmenovaných umístění podmíněného přístupu povolíte přístup jenom z konkrétních logických skupin rozsahů IP adres nebo zemí nebo oblastí.

* [Postup konfigurace pojmenovaných umístění v Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="39-use-azure-active-directory"></a>3,9: použijte Azure Active Directory

**Pokyny**: objekt pro vytváření dat je možné přidružit ke spravované identitě pro prostředky Azure, které představují konkrétní objekt pro vytváření dat. Tuto spravovanou identitu můžete použít pro ověřování Azure SQL Database. Určená továrna má přístup k datům a jejich zkopírování z databáze nebo do databáze pomocí této identity.

Pokud na virtuálním počítači Azure spouštíte Integration Runtime (IR), můžete pomocí spravovaných identit ověřit jakoukoli službu, která podporuje ověřování Azure AD, včetně Key Vault, a to bez jakýchkoli přihlašovacích údajů v kódu. Váš kód, který běží na virtuálním počítači, může používat spravovanou identitu k vyžádání přístupových tokenů pro služby, které podporují ověřování Azure AD.

* [Jak vytvořit a nakonfigurovat instanci Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

* [Jaké jsou spravované identity prostředků Azure?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

* [Kopírování a transformace dat v Azure SQL Database pomocí Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#using-service-principal-authentication)

* [Jak nakonfigurovat a spravovat Azure Active Directory ověřování pomocí Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: pravidelně kontrolovat a sjednotit přístup uživatelů

**Doprovodné**materiály: Azure Active Directory (AD) poskytuje protokoly, které vám pomůžou zjistit zastaralé účty. Navíc můžete pomocí kontrol přístupu Azure identity efektivně spravovat členství ve skupinách, přístup k podnikovým aplikacím a přiřazování rolí. Přístup uživatelů se dá pravidelně kontrolovat, aby se zajistilo, že budou mít přístup jenom přípravní uživatelé.

Pokud spouštíte integraci modulu runtime na virtuálním počítači Azure, budete muset zkontrolovat místní skupiny zabezpečení a uživatele, abyste se ujistili, že neexistují žádné neočekávané účty, které by mohly ohrozit systém.

* [Jak používat recenze Azure identity Access](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Pochopení sestav Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Jak používat recenze Azure identity Access](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: sledování pokusů o přístup k deaktivovaným přihlašovacím údajům

**Doprovodné**materiály: máte přístup k aktivitě přihlašování Azure Active Directory (AD), událostem auditu a rizikovým protokolům událostí, které umožňují integraci s jakýmkoli nástrojem Siem/monitoring. Tento proces můžete zjednodušit vytvořením nastavení diagnostiky pro uživatelské účty Azure AD a odesláním protokolů auditu a protokolů přihlášení do Log Analytics pracovního prostoru. Požadované výstrahy protokolu můžete nakonfigurovat v rámci Log Analytics.

Pokud spouštíte Integration Runtime na virtuálním počítači Azure, připojte se k virtuálnímu počítači na kartě Azure Sentinel. Microsoft Azure Sentinel je škálovatelná, cloudová, nativní, SIEM (Security Information Management) a automatizované řešení pro orchestraci zabezpečení (společnosti). Služba Azure Sentinel nabízí inteligentní analýzu zabezpečení a analýzu hrozeb napříč podnikem a poskytuje jediné řešení pro detekci výstrah, viditelnost hrozeb, proaktivní lov a reakci na hrozby.

* [Jak integrovat protokoly aktivit Azure do Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Autorizace přístupu k prostředkům Event Hubs pomocí Azure Active Directory](https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory)

* [Postup zprovoznění služby Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: upozornění na odchylku chování přihlášení k účtu

**Doprovodné**materiály: použijte Azure Active Directory (AD) jako centrální ověřování a systém autorizace pro vaše prostředky Azure Data Factory, jako je Azure SQL Database nebo Virtual Machines Azure. V případě odchylky chování přihlášení k účtu na rovině ovládacího prvku (Azure Portal) použijte funkce Azure AD Identity Protection a detekce rizik ke konfiguraci automatizovaných odpovědí na zjištěné podezřelé akce týkající se identit uživatelů. Můžete také ingestovat data do služby Azure Sentinel pro další šetření.

* [Jak zobrazit rizikové přihlašování Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Jak nakonfigurovat a povolit zásady rizik ochrany identity](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Konfigurace a Správa ověřování Azure Active Directory pomocí SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell)

* [Povolení ověřování Azure Active Directory pro Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: Poskytněte Microsoftu přístup k relevantním zákaznickým datům během scénářů podpory.

**Doprovodné**materiály: ve scénářích podpory, kde Microsoft potřebuje získat přístup k zákaznickým datům, Azure Customer Lockbox poskytuje rozhraní pro zákazníky, kteří budou kontrolovat a schvalovat nebo odmítat žádosti o přístup k datům zákazníků. Všimněte si, že přestože není k dispozici žádný bezpečnostní modul Azure pro Azure Data Factory sám, podporuje bezpečnostní modul Azure Azure SQL Database a Azure Virtual Machines.

* [Pochopení Customer Lockbox](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="data-protection"></a>Ochrana dat

*Další informace najdete v tématu [řízení zabezpečení: Ochrana dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: Udržujte inventář citlivých informací

**Doprovodné**materiály: používejte značky, které vám pomůžou při sledování prostředků Azure, které ukládají nebo zpracovávají citlivé informace.

Použijte funkci zjišťování a klasifikace dat Azure SQL Database. Zjišťování a klasifikace dat poskytuje pokročilé funkce, které jsou integrované v Azure SQL Database pro zjišťování, klasifikaci a označování &amp; ochrany citlivých dat ve vašich databázích.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Použití zjišťování a klasifikace dat pro Azure SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: izolujte systémy, které ukládají nebo zpracovávají citlivé informace.

**Pokyny**: implementace samostatných předplatných nebo skupin pro správu pro vývoj, testování a produkci. Prostředí Integration runtime by mělo být oddělené virtuální sítí (VNet)/Subnet a odpovídajícím způsobem označeno.

 K izolaci sítě můžete použít také soukromé koncové body. Privátní koncový bod Azure je síťové rozhraní, které se připojuje soukromě a bezpečně ke službě využívající privátní propojení Azure. Privátní koncový bod používá privátní IP adresu z vaší virtuální sítě a efektivně ho přinášejí do vaší virtuální sítě.

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Pochopení privátního odkazu](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: Sledujte a zablokujte neoprávněný přenos citlivých informací

**Doprovodné**materiály: u zdrojů dat (například Azure SQL Database) ukládání nebo zpracování citlivých informací pro nasazení Azure Data Factory můžete označit související prostředky jako citlivé pomocí značek.

Pokud je k dispozici privátní odkaz, použijte privátní koncové body k zabezpečení všech prostředků, které jsou propojeny s vaším kanálem Azure Data Factory. Provoz mezi vaší virtuální sítí a službou prochází přes páteřní síť Microsoftu a eliminuje rizika vystavení na veřejném internetu. Riziko exfiltrace dat můžete také snížit konfigurací striktní sady odchozích pravidel pro skupinu zabezpečení sítě (NSG) a přidružením tohoto NSG k vaší podsíti.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Principy privátního odkazu Azure](https://docs.microsoft.com/azure/private-link/private-link-overview)

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: šifrování všech citlivých informací během přenosu

**Doprovodné**materiály: Pokud cloudové úložiště dat podporuje protokol HTTPS nebo TLS, všechna přenosová data mezi službami přesunu dat v Data Factory a cloudovým úložištěm dat jsou prostřednictvím zabezpečeného kanálu HTTPS nebo TLS. Použitá verze protokolu TLS je 1,2.

Všechna připojení k Azure SQL Database a Azure SQL Data Warehouse vyžadovat šifrování (SSL/TLS) při přenosu dat do a z databáze. Když vytváříte kanál pomocí formátu JSON, přidejte vlastnost šifrování a nastavte ji na hodnotu true v připojovacím řetězci. V případě Azure Storage můžete v připojovacím řetězci použít HTTPS.

* [Principy šifrování při přenosu v Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)

**Monitorování Azure Security Center**: Ano

**Odpovědnost**: sdílená

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: k identifikaci citlivých dat použijte aktivní nástroj zjišťování.

**Pokyny**: Pokud použijete Azure Data Factory ke kopírování a transformaci instancí Azure SQL Database, použijte funkci Azure SQL Database zjišťování a klasifikace dat. Zjišťování a klasifikace dat poskytuje pokročilé funkce, které jsou integrované v Azure SQL Database pro zjišťování, klasifikaci a označování &amp; ochrany citlivých dat ve vašich databázích.

Funkce zjišťování dat a klasifikace ještě nejsou k dispozici pro jiné služby Azure.

* [Použití zjišťování a klasifikace dat pro Azure SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: k řízení přístupu k prostředkům použijte řízení přístupu na základě role

**Doprovodné**materiály: použijte řízení přístupu na základě role (RBAC) Azure Active Directory (AD) k řízení přístupu k rovině ovládacího prvku Azure Data Factory (Azure Portal).

Pro vytvoření instancí služby Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem role přispěvatel nebo vlastník nebo správcem předplatného Azure.

U Data Factorych zdrojů dat, jako je například Azure SQL Database, najdete další informace o RBAC, které se týkají standardních hodnot zabezpečení této služby.

* [Jak nakonfigurovat RBAC v Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

* [Role a oprávnění pro službu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: použití prevence ztráty dat na základě hostitele k vymáhání řízení přístupu

**Pokyny**: funkce pro identifikaci, klasifikaci a ochranu před únikem informací ještě nejsou k dispozici pro Azure Storage nebo výpočetní prostředky. Implementujte řešení třetích stran, pokud je to potřeba pro účely dodržování předpisů.

Pro základní platformu, která je spravovaná Microsoftem, Microsoft považuje veškerý obsah zákazníka za citlivý a vede na skvělé délky, aby se zabránilo ochraně před ztrátou a únikem informací a riziky zákazníků. Aby se zajistilo zabezpečení zákaznických dat v Azure, společnost Microsoft implementovala a udržuje sadu robustních ovládacích prvků a možností ochrany dat.

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: sdílená

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: šifrování citlivých informací v klidovém umístění

**Pokyny**: doporučujeme povolit mechanismus šifrování dat pro všechna úložiště dat související s nasazením Azure Data Factory. Další informace o Šifrování neaktivních dat najdete v základních informacích o zabezpečení této služby.

Pokud na virtuálním počítači Azure spouštíte Integration Runtime, virtuální disky na Windows Virtual Machines (VM) se zašifrují v klidovém stavu pomocí šifrování na straně serveru nebo služby Azure Disk Encryption (ADE). Azure Disk Encryption využívá funkci nástroje BitLocker systému Windows k šifrování spravovaných disků pomocí klíčů spravovaných zákazníkem v rámci virtuálního počítače hosta. Šifrování na straně serveru pomocí klíčů spravovaných zákazníkem se zlepšuje v ADE tím, že vám umožní používat pro vaše virtuální počítače jakékoli typy operačních systémů a image šifrováním dat ve službě úložiště.

Přihlašovací údaje nebo tajné hodnoty můžete ukládat do Azure Key Vault a použít je během provádění kanálu, aby je bylo možné předat k vašim aktivitám. Přihlašovací údaje pro úložiště dat a výpočetní služby můžete také ukládat v Azure Key Vault. Azure Data Factory načte pověření při provádění aktivity, která používá úložiště nebo výpočetní prostředky.

* [Principy šifrování v Azure Data Factory v klidovém umístění](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)

* [Šifrování na straně serveru Azure Managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption)

* [Azure Disk Encryption pro virtuální počítače s Windows](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption-overview)

* [Použití Azure Key Vault tajných klíčů v aktivitách kanálu](https://docs.microsoft.com/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities)

* [Jak přihlašovací údaje v Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: protokolovat a upozornit na změny kritických prostředků Azure

**Doprovodné**materiály: pomocí Azure monitor s protokolem aktivit Azure můžete vytvářet upozornění na to, kdy se změny provedly pro Azure Data Factory a související prostředky.

* [Vytvoření upozornění pro události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Vytvoření upozornění pro události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Protokolování analýz služby Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

## <a name="vulnerability-management"></a>Správa ohrožení zabezpečení

*Další informace najdete v tématu [řízení zabezpečení: Správa ohrožení](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)zabezpečení.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: spuštění automatizovaných nástrojů pro kontrolu ohrožení zabezpečení

**Doprovodné**materiály: pokud používáte Azure SQL Database jako úložiště dat, povolte rozšířené zabezpečení dat pro Azure SQL Database a použijte doporučení od Azure Security Center při provádění posouzení ohrožení zabezpečení na vašich serverech SQL Azure.

Pokud používáte Integration Runtime na virtuálním počítači Azure, postupujte podle doporučení Azure Security Center při provádění posouzení ohrožení zabezpečení vašich virtuálních počítačů. Použijte doporučené řešení zabezpečení Azure nebo řešení třetí strany pro provádění posouzení ohrožení zabezpečení vašich virtuálních počítačů.

* [Spuštění posouzení ohrožení zabezpečení na Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment)

* [Jak povolit pokročilé zabezpečení dat](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [Implementace doporučení pro posouzení ohrožení zabezpečení Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: nasazení automatizovaného řešení pro správu oprav operačního systému

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, použijte řešení Azure Update Management ke správě aktualizací a oprav pro vaše virtuální počítače. Update Management spoléhá na místně nakonfigurované úložiště aktualizací, které opraví podporované systémy Windows. Nástroje, jako je System Center Updates Publisher (Updates Publisher), umožňují publikovat vlastní aktualizace do Windows Server Update Services (WSUS). Tento scénář umožňuje Update Management opravit počítače, které používají Configuration Manager jako úložiště aktualizací se softwarem třetích stran.

Pro základní platformu, která je spravovaná Microsoftem, Microsoft považuje veškerý obsah zákazníka za citlivý a vede na skvělé délky, aby se zabránilo ochraně před ztrátou a únikem informací a riziky zákazníků. Aby se zajistilo zabezpečení zákaznických dat v Azure, společnost Microsoft implementovala a udržuje sadu robustních ovládacích prvků a možností ochrany dat.

* [Řešení Update Management v Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

* [Správa aktualizací a oprav pro virtuální počítače Azure](https://docs.microsoft.com/azure/automation/automation-tutorial-update-management)

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: Ano

**Odpovědnost**: sdílená

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: nasazení automatizovaného řešení pro správu oprav pro softwarové tituly třetích stran

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, můžete použít řešení pro správu oprav třetí strany. Pomocí řešení Azure Update Management můžete spravovat aktualizace a opravy pro virtuální počítače. Update Management spoléhá na místně nakonfigurované úložiště aktualizací, které opraví podporované systémy Windows. Nástroje, jako je System Center Updates Publisher (Updates Publisher), umožňují publikovat vlastní aktualizace do Windows Server Update Services (WSUS). Tento scénář umožňuje Update Management opravit počítače, které používají Configuration Manager jako úložiště aktualizací se softwarem třetích stran.

* [Řešení Update Management v Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

* [Správa aktualizací a oprav pro virtuální počítače Azure](https://docs.microsoft.com/azure/automation/automation-tutorial-update-management)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: porovnání kontrol zabezpečení back-to-back

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, exportujte výsledky kontroly v konzistentních intervalech a porovnejte výsledky, abyste ověřili, že chyby zabezpečení byly opraveny. Při použití doporučení pro správu ohrožení zabezpečení navržených v Azure Security Center můžete překlopit na portál vybraného řešení a zobrazit historická data kontroly.

* [Vysvětlení integrovaného skeneru ohrožení zabezpečení pro virtuální počítače](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: použijte proces hodnocení rizik k určení priorit nápravy zjištěných ohrožení zabezpečení

**Pokyny**: pokud spustíte Integration runtime na virtuálním počítači Azure, můžete použít nativní skener ohrožení zabezpečení. Skener ohrožení zabezpečení, který je součástí Azure Security Center, je napájený pomocí Qualys. Qualys je špičkovým nástrojem pro identifikaci ohrožení zabezpečení v reálném čase v Azure Virtual Machines.

Když Security Center identifikuje chyby zabezpečení, prezentuje závěry a související informace jako doporučení. Související informace zahrnují kroky pro nápravu, související CVEs, hodnocení CVSS a další. Zjištěná ohrožení zabezpečení můžete zobrazit pro jedno nebo více předplatných nebo pro konkrétní virtuální počítač.

* [Integrovaná bezpečnostní skenery pro virtuální počítače](https://docs.microsoft.com/azure/security-center/built-in-vulnerability-assessment)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="inventory-and-asset-management"></a>Správa inventáře a aktiv

*Další informace najdete v tématu [řízení zabezpečení: inventář a Správa prostředků](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: použití řešení automatizovaného zjišťování prostředků

**Pokyny**: pomocí grafu prostředků Azure můžete v rámci vašich předplatných dotazovat a zjišťovat všechny prostředky (například výpočetní prostředky, úložiště, síť, porty a protokoly atd.). Zajistěte, aby ve vašem tenantovi byla vhodná (číst) oprávnění a aby se v rámci předplatných mohli vytvořit výčet všech předplatných Azure i prostředků

I když je možné zjistit klasické prostředky Azure pomocí grafu prostředků, důrazně doporučujeme vytvořit a používat prostředky Azure Resource Manager, které budou předány.

* [Jak vytvářet dotazy pomocí Azure Resource graphu](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Jak zobrazit vaše předplatná Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Pochopení Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="62-maintain-asset-metadata"></a>6,2: Údržba metadat assetu

**Doprovodné**materiály: použití značek pro prostředky Azure poskytující metadata k logickému uspořádání do taxonomie.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: odstranění neautorizovaných prostředků Azure

**Doprovodné**materiály: Používejte označení, skupiny pro správu a samostatné odběry, pokud je to vhodné, k organizování a sledování prostředků Azure. Proveďte pravidelné sjednocení inventáře a zajistěte si včas odstranění neautorizovaných prostředků z předplatného.

Kromě toho použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v předplatných zákazníka pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: definování a údržba inventáře schválených prostředků Azure

**Pokyny**: definování schválených prostředků Azure a schváleného softwaru pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitorování neschválených prostředků Azure

**Doprovodné**materiály: použijte Azure Policy k omezení typu prostředků, které se dají vytvořit ve vašich předplatných.

Pomocí grafu prostředků Azure můžete v rámci svých předplatných dotazovat nebo zjišťovat prostředky. Ujistěte se, že všechny prostředky Azure přítomné v daném prostředí jsou schválené.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak vytvářet dotazy pomocí Azure graphu](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: monitorujte neschválené softwarové aplikace v rámci výpočetních prostředků.

**Doprovodné**materiály: pokud používáte Integration runtime na virtuálním počítači Azure, využijte inventarizaci virtuálních počítačů Azure pro automatizaci shromažďování informací o veškerém softwaru v Virtual Machines. Azure Automation poskytuje úplnou kontrolu v průběhu nasazení, provozu i vyřazení úloh a prostředků z provozu.

Poznámka: název softwaru, verze, Vydavatel a čas aktualizace jsou dostupné z Azure Portal. Aby bylo možné získat přístup k datu instalace a dalším informacím, musí zákazník vyžadovat diagnostiku na úrovni hosta a přenést protokoly událostí systému Windows do Log Analytics pracovního prostoru.

* [Seznámení s Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro)

* [Jak povolit inventář virtuálních počítačů Azure](https://docs.microsoft.com/azure/automation/automation-tutorial-installed-software)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: Odeberte neschválené prostředky Azure a softwarové aplikace

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, poskytuje Azure Automation úplnou kontrolu během nasazování, operací a vyřazení úloh a prostředků z provozu. K identifikaci veškerého softwaru nainstalovaného v Virtual Machines můžete použít Change Tracking. Můžete implementovat vlastní proces nebo použít konfiguraci Azure Automation stav pro odebrání neautorizovaného softwaru.

* [Seznámení s Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro)

* [Sledování změn ve vašem prostředí pomocí Change Tracking řešení](https://docs.microsoft.com/azure/automation/change-tracking)

* [Přehled konfigurace stavu Azure Automation](https://docs.microsoft.com/azure/automation/automation-dsc-overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="68-use-only-approved-applications"></a>6,8: Používejte pouze schválené aplikace.

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, použijte Azure Security Center Adaptivní řízení aplikací, abyste zajistili, že se spustí jenom autorizovaný software, a veškerý neoprávněný software se zablokuje spouštění na virtuálních počítačích.

* [Jak používat Azure Security Center Adaptivní řízení aplikací](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="69-use-only-approved-azure-services"></a>6,9: Používejte jenom schválené služby Azure.

**Doprovodné**materiály: použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v zákaznických předplatných, pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak odepřít konkrétní typ prostředku pomocí Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: udržování inventáře schválených softwarových titulů

**Doprovodné**materiály: Adaptivní řízení aplikací je inteligentní, automatizované a ucelené řešení od Azure Security Center, které vám pomůže určit, které aplikace se můžou spouštět na počítačích Azure a mimo Azure (Windows a Linux). Implementujte řešení třetích stran, pokud to nevyhovuje požadavkům vaší organizace.

Všimněte si, že to platí jenom v případě, že vaše Integration Runtime běží na virtuálním počítači Azure.

* [Jak používat Azure Security Center Adaptivní řízení aplikací](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: Omezte schopnost uživatelů pracovat s Azure Resource Manager

**Pokyny**: Nakonfigurujte podmíněný přístup Azure tak, aby uživatelé mohli komunikovat s Azure Resource Manager konfigurací možnosti blokovat přístup pro aplikaci Microsoft Azure Management.

* [Postup konfigurace podmíněného přístupu pro blokování přístupu k Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: Omezte schopnost uživatelů spouštět skripty ve výpočetních prostředcích.

**Pokyny**: Pokud spouštíte integraci modulu runtime na virtuálním počítači Azure, v závislosti na typu skriptů můžete použít konfigurace specifické pro operační systém nebo prostředky třetích stran k omezení schopnosti uživatelů spouštět skripty v rámci výpočetních prostředků Azure. Můžete také využít Azure Security Center Adaptivní řízení aplikací, abyste zajistili, že se spustí jenom autorizovaný software, a veškerý neautorizovaný software bude zablokovaný na Azure Virtual Machines.

* [Řízení spouštění skriptu PowerShellu v prostředích Windows](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

* [Jak používat Azure Security Center Adaptivní řízení aplikací](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: fyzicky nebo logicky oddělené aplikace s vysokým rizikem

**Doprovodné**materiály: vysoce rizikové aplikace nasazené ve vašem prostředí Azure mohou být izolované pomocí virtuální sítě, podsítě, předplatných, skupin pro správu atd. a dostatečně zabezpečených pomocí Azure firewall, firewallu webových aplikací (WAF) nebo skupiny zabezpečení sítě (NSG).

* [Virtuální sítě a virtuální počítače v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/network-overview)

* [Co je brána Azure Firewall?](https://docs.microsoft.com/azure/firewall/overview)

* [Co je Firewall webových aplikací Azure?](https://docs.microsoft.com/azure/web-application-firewall/overview)

* [Skupiny zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/security-overview)

* [Co je Azure Virtual Network?](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)

* [Uspořádání prostředků s využitím skupin pro správu Azure](https://docs.microsoft.com/azure/governance/management-groups/overview)

* [Průvodce rozhodováním ohledně předplatného](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="secure-configuration"></a>Zabezpečená konfigurace

*Další informace najdete v tématu [řízení zabezpečení: zabezpečená konfigurace](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: Vytvoření zabezpečených konfigurací pro všechny prostředky Azure

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro Azure Data Factory s Azure Policy. Použijte aliasy Azure Policy v oboru názvů Microsoft. DataFactory, abyste vytvořili vlastní zásady pro auditování nebo prosazování konfigurace vašich Azure Data Factorych instancí.

* [Jak zobrazit dostupné aliasy Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: Vytvoření zabezpečených konfigurací operačního systému

**Doprovodné**materiály: Pokud spouštíte integraci modulu runtime na virtuálním počítači Azure, použijte Azure Security Center doporučení [napravení ohrožení zabezpečení v konfiguracích zabezpečení v Virtual Machines] a udržujte tak konfigurace zabezpečení ve všech výpočetních prostředcích.

* [Jak monitorovat Azure Security Center doporučení](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

* [Jak opravit Azure Security Center doporučení](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: udržování zabezpečených konfigurací prostředků Azure

**Doprovodné**materiály: použijte Azure Policy [Deny] a [Deploy, pokud neexistuje] pro vymáhání zabezpečených nastavení napříč prostředky Azure.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Pochopení Azure Policych efektů](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

* [Informace o vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: udržování zabezpečených konfigurací operačního systému

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure (VM), pamatujte na to, že existuje několik možností, jak spravovat zabezpečenou konfiguraci pro virtuální počítače pro nasazení:
- Šablony Azure Resource Manager: Jedná se o soubory založené na formátu JSON, které slouží k nasazení virtuálního počítače z Azure Portal a vlastní šablonu bude nutné zachovat. Společnost Microsoft provádí údržbu základních šablon.
- Vlastní virtuální pevný disk (VHD): v některých případech se může vyžadovat, aby se použily vlastní soubory VHD, jako třeba při řešení složitých prostředí, která není možné spravovat prostřednictvím jiných prostředků. -Azure Automation konfigurace stavu: po nasazení základního operačního systému je možné ho použít pro přesnější řízení nastavení a pro zajištění prostřednictvím rozhraní Automation.

Pro většinu scénářů se může při schůzi a údržbě požadavků na zabezpečení pomáhat šablony základních virtuálních počítačů společnosti Microsoft kombinované s Azure Automation konfigurací požadovaného stavu.

* [Informace o tom, jak stáhnout šablonu virtuálního počítače](https://docs.microsoft.com/azure/virtual-machines/windows/download-template)

* [Informace o vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)

* [Jak nahrát vlastní virtuální pevný disk virtuálního počítače do Azure](https://docs.microsoft.com/azure-stack/operator/azure-stack-add-vm-image?view=azs-1910)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Konfigurace prostředků Azure v zabezpečeném úložišti

**Doprovodné**materiály: Pokud používáte vlastní definice Azure Policy, použijte k bezpečnému ukládání a správě kódu službu Azure DevOps nebo Azure Repos.

* [Jak v Azure DevOps ukládat kód](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentace k Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: bezpečné uložení vlastních imagí operačního systému

**Pokyny**: Pokud používáte vlastní image, použijte řízení přístupu na základě role (RBAC), abyste zajistili, že k imagí budou mít přístup jenom autorizovaní uživatelé. V případě imagí kontejnerů je uložte do Azure Container Registry a využijte RBAC, aby bylo zajištěno, že k imagí budou mít přístup jenom autorizovaní uživatelé.

Roli Přispěvatel Data Factory lze použít k vytváření a správě datových továren a také k podřízeným prostředkům v nich.

* [Princip RBAC v Azure](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)

* [Vysvětlení RBAC pro Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-roles)

* [Jak nakonfigurovat RBAC v Azure](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal)

* [Role a oprávnění pro službu Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-roles-permissions)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: nasazení nástrojů pro správu konfigurace pro prostředky Azure

**Doprovodné**materiály: použijte předdefinované definice Azure Policy a také Azure Policy aliasy v oboru názvů Microsoft. DataFactory, abyste mohli vytvářet vlastní zásady pro upozornění, audit a prosazování systémových konfigurací. Dále můžete vyvinout proces a kanál pro správu výjimek zásad.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: nasazení nástrojů pro správu konfigurace pro operační systémy

**Doprovodné**materiály: Toto doporučení může platit, pokud Integration Runtime běží na virtuálním počítači Azure. Konfigurace stavu Azure Automation je služba správy konfigurace pro požadované uzly Konfigurace stavu (DSC) v jakémkoli cloudu nebo v místním datacentru. Umožňuje snadnou a snadnou škálovatelnost v tisících počítačů z centrálního a bezpečného umístění. Můžete snadno připojit počítače, přiřazovat je k deklarativním konfiguracím a zobrazovat sestavy, které zobrazují kompatibilitu jednotlivých počítačů s požadovaným stavem, který jste zadali.

* [Připojování počítačů pro správu podle konfigurace stavu Azure Automation](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Implementujte automatizované monitorování konfigurace pro prostředky Azure.

**Doprovodné**materiály: použijte předdefinované definice Azure Policy a také Azure Policy aliasy v oboru názvů Microsoft. DataFactory, abyste mohli vytvářet vlastní zásady pro upozornění, audit a prosazování systémových konfigurací. K automatickému vymáhání konfigurací pro prostředky Azure použijte Azure Policy [audit], [Deny] a [nasazení, pokud neexistuje].

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Implementujte automatizované monitorování konfigurace pro operační systémy

**Doprovodné**materiály: Toto doporučení může platit, pokud Integration Runtime běží na virtuálním počítači Azure. Konfigurace stavu Azure Automation je služba správy konfigurace pro požadované uzly Konfigurace stavu (DSC) v jakémkoli cloudu nebo v místním datacentru. Umožňuje snadnou a snadnou škálovatelnost v tisících počítačů z centrálního a bezpečného umístění. Můžete snadno připojit počítače, přiřazovat je k deklarativním konfiguracím a zobrazovat sestavy, které zobrazují kompatibilitu jednotlivých počítačů s požadovaným stavem, který jste zadali.

* [Připojování počítačů pro správu podle konfigurace stavu Azure Automation](https://docs.microsoft.com/azure/automation/automation-dsc-onboarding)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="711-manage-azure-secrets-securely"></a>7,11: zabezpečená Správa tajných kódů Azure

**Doprovodné**materiály: použití identita spravované služby ve spojení s Azure Key Vault ke zjednodušení a zabezpečení správy tajných kódů pro vaše cloudové aplikace.

Přihlašovací údaje nebo tajné hodnoty můžete také ukládat do Azure Key Vault a použít je během provádění kanálu, aby je bylo možné předat k vašim aktivitám. Ujistěte se, že je povolené obnovitelné odstranění.

* [Integrace se spravovanými identitami Azure](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

* [Vytvoření Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [Jak zajistit Key Vault ověřování pomocí spravované identity](https://docs.microsoft.com/azure/key-vault/managed-identity)

* [Použití tajných kódů služby Azure Key Vault v aktivitách kanálu](https://docs.microsoft.com/azure/data-factory/how-to-use-azure-key-vault-secrets-pipeline-activities)

* [Obnovitelné odstranění v Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: bezpečně a automaticky spravujte identity

**Doprovodné**materiály: při vytváření datové továrny je možné vytvořit spravovanou identitu společně s vytvářením továrny. Spravovaná identita je spravovaná aplikace zaregistrovaná pro Azure Active Directory, která představuje konkrétní objekt pro vytváření dat.

* [Spravovaná identita pro Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: Eliminujte nezamýšlenou expozici přihlašovacích údajů

**Pokyny**: implementace skeneru přihlašovacích údajů pro identifikaci přihlašovacích údajů v rámci kódu. Skener přihlašovacích údajů taky bude povzbudit přesunutí zjištěných přihlašovacích údajů do bezpečnějších umístění, jako je Azure Key Vault.

* [Jak nastavit skener přihlašovacích údajů](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="malware-defense"></a>Obrana před malwarem

*Další informace najdete v tématu [řízení zabezpečení: obrana proti malwaru](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: použití centrálně spravovaného malwarového softwaru

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, můžete k nepřetržitému monitorování a obraně prostředků použít Microsoft Antimalware pro Azure Windows Virtual Machines.

* [Jak nakonfigurovat Microsoft Antimalware pro Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: předběžná kontrola souborů, které se mají nahrát do prostředků Azure, které nejsou COMPUTE

**Pokyny**: Microsoft Antimalware je povolený na podkladovém hostiteli, který podporuje služby Azure (například Azure App Service), ale neběží na vašem obsahu.

Předem Prohledejte všechny soubory nahrané do nevýpočetních prostředků Azure, například App Service, Data Lake Storage, Blob Storage atd.

K detekci malwaru nahraného do účtů úložiště použijte detekci hrozeb Azure Security Center pro datové služby.

* [Pochopení Microsoft antimalwaru pro Azure Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Vysvětlení detekce hrozeb Azure Security Center pro datové služby](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: Ujistěte se, že antimalwarový software a signatury jsou aktualizované.

**Pokyny**: po nasazení bude Microsoft Antimalware pro Azure ve výchozím nastavení automaticky instalovat nejnovější signatury, platformy a aktualizace stroje. Použijte doporučení v Azure Security Center: "COMPUTE &amp; Apps", abyste zajistili aktuálnost všech koncových bodů s nejnovějšími podpisy. OPERAČNÍ systém Windows je možné dále chránit pomocí dalšího zabezpečení a omezit tak riziko virů nebo malwarových útoků pomocí služby Microsoft Defender Advanced Threat Protection, která se integruje s Azure Security Center.

* [Jak nasadit Microsoft Antimalware pro Azure Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

* [Rozšířená ochrana před internetovými útoky v programu Microsoft Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/onboard-configure)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="data-recovery"></a>Obnovení dat

*Další informace najdete v tématu [řízení zabezpečení – obnovení dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: zajištění pravidelného automatického zálohování

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure, povolte Azure Backup a nakonfigurujte virtuální počítač a také požadovanou četnost a dobu uchování pro automatické zálohování.

Pro některá z vašich úložišť dat si Projděte základní informace o zabezpečení této služby, kde najdete doporučení, jak provádět pravidelné automatizované zálohování.

* [Přehled zálohování virtuálních počítačů Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)

* [Zálohování virtuálního počítače Azure z nastavení virtuálního počítače](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: proveďte kompletní systémové zálohy a zálohujte všechny klíče spravované zákazníkem.

**Doprovodné**materiály: Pokud spouštíte Integration runtime na virtuálním počítači Azure, povolte Azure Backup a cílové virtuální počítače Azure a požadovanou frekvenci a dobu uchování. Zálohování klíčů spravovaných zákazníkem v rámci Azure Key Vault.

Pro některá z vašich úložišť dat si Projděte základní informace o zabezpečení této služby, kde najdete doporučení, jak provádět pravidelné automatizované zálohování.

* [Přehled zálohování virtuálních počítačů Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)

* [Postup zálohování klíčů trezoru klíčů v Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: ověření všech záloh včetně klíčů spravovaných zákazníkem

**Doprovodné**materiály: pokud používáte Integration runtime na virtuálním počítači Azure, ujistěte se, že schopnost pravidelně provádět obnovování dat v rámci Azure Backup. V případě potřeby proveďte test obnovení obsahu na izolovanou síť VLAN. Pravidelně testujte obnovení zálohovaných klíčů spravovaných zákazníkem.

V případě jakýchkoli úložišť dat najdete pokyny k ověřování záloh v základních hodnotách zabezpečení této služby.

* [Postup obnovení souborů ze zálohy virtuálního počítače Azure](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm)

* [Postup obnovení klíčů trezoru klíčů v Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: Zajistěte ochranu záloh a klíčů spravovaných zákazníkem

**Pokyny**: pokud používáte Integration runtime na virtuálním počítači Azure a Vy jste tento virtuální počítač provedete s Azure Backup, je váš virtuální počítač zašifrovaný v klidovém stavu šifrování služby Storage (SSE). Azure Backup taky můžou zálohovat virtuální počítače Azure, které jsou zašifrované pomocí Azure Disk Encryption. Azure Disk Encryption se integruje s šifrovacími klíči BitLockeru (BEKs), které jsou v trezoru klíčů zabezpečené jako tajné klíče. Azure Disk Encryption se taky integruje s klíči šifrovacího klíče Azure Key Vault (KEK). Povolit obnovitelné odstranění v Key Vault k ochraně klíčů proti náhodnému nebo škodlivému odstranění.

* [Obnovitelné odstranění pro virtuální počítače](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#soft-delete)

* [Přehled obnovitelného odstranění ve službě Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="incident-response"></a>Reakce na incidenty

*Další informace najdete v tématu [řízení zabezpečení: reakce na incidenty](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: Vytvoření Průvodce odpověďmi na incidenty

**Pokyny**: Vytvoření Průvodce odpověďmi na incidenty pro vaši organizaci. Zajistěte, aby existovaly písemné plány odpovědí na incidenty, které definují všechny role pracovníků, a také fáze zpracování nebo správy incidentů z detekce až po přezkoumání po jednotlivých událostech.

* [Postup konfigurace automatizace pracovních postupů v rámci služby Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Pokyny k vytvoření vlastního procesu reakce na incidenty zabezpečení](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Anatomie centra Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Zákazník může také využít příručku pro zpracování incidentů zabezpečení počítače v NIST, aby mohl pomoci při vytváření vlastního plánu reakce na incidenty.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: vytvoření bodování incidentu a postupu stanovení priorit

**Doprovodné**materiály: Security Center přiřadí každému upozornění závažnost závažnosti, které vám pomůžou určit, které výstrahy by se měly prozkoumat jako první. Závažnost je založena na tom, jak se nachází Security Center ve vyhledávání nebo v analytickém formátu, který vydává výstrahu, a také na úrovni spolehlivosti, u kterých došlo k škodlivému záměru za aktivitu, která vedla k upozornění.

Kromě toho jasně označte odběry (pro např. Výroba, nevýrobní zakázka a vytvoření názvového systému pro zřetelné identifikaci a kategorizaci prostředků Azure.

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="103-test-security-response-procedures"></a>10,3: testovací postupy pro odpověď zabezpečení

**Doprovodné**materiály: proveďte cvičení a otestujte možnosti reakce na incidenty v pravidelných tempo. Identifikujte slabá místa a mezery a podle potřeby upravte plán.

* [Přečtěte si téma publikace NIST: Průvodce testováním, školením a cvičením programů pro plány a možnosti IT](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: zadání podrobností o kontaktu incidentu zabezpečení a konfigurace oznámení o výstrahách pro incidenty zabezpečení

**Doprovodné**materiály: kontaktní informace incidentu zabezpečení bude společnost Microsoft používat ke kontaktování v případě, že služba Microsoft Security Response Center (MSRC) zjistí, že k datům zákazníka přistupovala protiprávní nebo neoprávněná strana. Projděte si incidenty, abyste měli jistotu, že jsou vyřešené problémy.

* [Jak nastavit kontakt zabezpečení Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: zahrňte výstrahy zabezpečení do systému reakce na incidenty.

**Doprovodné**materiály: vyexportujte výstrahy a doporučení Azure Security Center pomocí funkce průběžného exportu. Průběžný export umožňuje exportovat výstrahy a doporučení buď ručně, nebo nepřetržitě, průběžným způsobem. Pomocí konektoru Azure Security Center Data můžete streamovat ověřovací data výstrah.

* [Postup konfigurace průběžného exportu](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Jak streamovat výstrahy do Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: automatizujte reakci na výstrahy zabezpečení

**Doprovodné**materiály: použití funkce automatizace pracovního postupu v Azure Security Center k automatickému spouštění odpovědí prostřednictvím "Logic Apps" na výstrahy a doporučení zabezpečení.

* [Jak nakonfigurovat automatizaci pracovních postupů a Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="penetration-tests-and-red-team-exercises"></a>Penetrační testy a tzv. red team exercises

*Další informace najdete v tématu [řízení zabezpečení: testy průniku a cvičení červeného týmu](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: proveďte pravidelné testování průniku vašich prostředků Azure a zajistěte nápravu všech kritických poznatků zabezpečení.

**Doprovodné**materiály: 

* [Využijte pravidla zapojení Microsoftu, abyste zajistili, že testy průniku nejsou v rozporu s zásadami Microsoftu.](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [V této části najdete další informace o strategii Microsoftu a provádění testování v rámci červeného seskupování a testování průniku na cloudové infrastruktuře, služby a aplikace spravované Microsoftem.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: sdílená

## <a name="next-steps"></a>Další kroky

- Zobrazit [Srovnávací test zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Další informace o [plánech zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
