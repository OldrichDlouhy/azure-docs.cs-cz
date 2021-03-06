---
title: Základní hodnoty zabezpečení Azure pro Azure Application Gateway
description: Základní hodnoty zabezpečení Azure pro Azure Application Gateway
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 562a0fbd64fca530598a58599160dbdd7e479557
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84485627"
---
# <a name="azure-security-baseline-for-azure-application-gateway"></a>Základní hodnoty zabezpečení Azure pro Azure Application Gateway

Základní plán zabezpečení Azure pro Azure Application Gateway obsahuje doporučení, která vám pomůžou vylepšit stav zabezpečení vašeho nasazení.

Základní hodnota této služby se vykreslí z [bezpečnostního testu Azure Security 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview), který poskytuje doporučení k zabezpečení cloudových řešení v Azure s využitím našich osvědčených postupů.

Další informace najdete v tématu [Přehled standardních hodnot zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Zabezpečení sítě

*Další informace najdete v tématu [řízení zabezpečení: zabezpečení sítě](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: Ochrana prostředků Azure v rámci virtuálních sítí

**Pokyny**: Ujistěte se, že všechna Virtual Network nasazení podsítí Application Gateway Azure obsahuje skupinu zabezpečení sítě (NSG) s ovládacími prvky pro přístup k síti, které jsou specifické pro důvěryhodné porty a zdroje vaší aplikace. I když jsou v Azure Application Gateway podporovány skupiny zabezpečení sítě, existují určitá omezení a požadavky, které musí být dodrženy, aby vaše NSG a Azure Application Gateway fungovaly podle očekávání.

* [Vysvětlení omezení a požadavků v používání skupin zabezpečení sítě s Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: Sledujte a protokolujte konfiguraci a provoz virtuálních sítí, podsítí a síťových karet

**Doprovodné**materiály: u skupin zabezpečení sítě (skupin zabezpečení sítě) přidružených k vašim podsítím Azure Application Gateway povolit protokoly toku NSG a odesílat protokoly do účtu úložiště pro audit provozu. Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

Poznámka: v některých případech se v protokolech NSG Flow přidružených k vašim podsítím Azure Application Gateway nezobrazuje povolený provoz. Pokud vaše konfigurace odpovídá následujícímu scénáři, neuvidíte v protokolech toku NSG povolený provoz:
- Nasadili jste Application Gateway v2
- Máte NSGu v podsíti služby Application Gateway.
- Povolili jste protokoly toku NSG na těchto NSG.

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Pochopení zabezpečení sítě, které poskytuje Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Nejčastější dotazy týkající se diagnostiky a protokolování pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-faq#diagnostics-and-logging)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="13-protect-critical-web-applications"></a>1,3: Chraňte kritické webové aplikace

**Pokyny**: nasazení firewallu webových aplikací Azure (WAF) před kritickými webovými aplikacemi pro další kontrolu příchozího provozu. Firewall webových aplikací (WAF) je služba (funkce Azure Application Gateway), která poskytuje centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. Služba Azure WAF vám může pomáhat zabezpečit vaše Azure App Service webové aplikace tím, že zkontroluje příchozí webový provoz a zablokuje útoky, jako jsou například injektáže SQL, skriptování mezi weby, nakládku malwaru a útoky DDoS. WAF je založená na pravidlech z OWASP (Open Web Application Security Project) základních sad pravidel 3,1 (jenom WAF_v2), 3,0 a 2.2.9.

* [Vysvětlení funkcí služby Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/features)

* [Pochopení Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview)

* [Jak nasadit Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: zakažte komunikaci se známými škodlivými IP adresami.

**Doprovodné**materiály: povolení DDoS Standard Protection na virtuálních sítích Azure přidružených k provozním instancím Azure Application Gateway pro ochranu před útoky DDoS. K odepření komunikace se známými škodlivými IP adresami použijte Azure Security Center integrovanou analýzu hrozeb.

* [Jak nakonfigurovat DDoS Protection](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Pochopení Azure Security Center integrované analýzy hrozeb](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="15-record-network-packets"></a>1,5: zaznamenání síťových paketů

**Doprovodné**materiály: u skupin zabezpečení sítě (skupin zabezpečení sítě) přidružených k vašim podsítím Azure Application Gateway povolit protokoly toku NSG a odesílat protokoly do účtu úložiště pro audit provozu. Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

Poznámka: v některých případech se v protokolech NSG Flow přidružených k vašim podsítím Azure Application Gateway nezobrazuje povolený provoz. Pokud vaše konfigurace odpovídá následujícímu scénáři, neuvidíte v protokolech toku NSG povolený provoz:
- Nasadili jste Application Gateway v2
- Máte NSGu v podsíti služby Application Gateway.
- Povolili jste protokoly toku NSG na těchto NSG.

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Pochopení zabezpečení sítě, které poskytuje Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Nejčastější dotazy týkající se diagnostiky a protokolování pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-faq#diagnostics-and-logging)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: nasazení systémů ochrany před internetovými útoky/systémy prevence vniknutí (ID/IP adresy)

**Pokyny**: nasazení firewallu webových aplikací Azure (WAF) před kritickými webovými aplikacemi pro další kontrolu příchozího provozu. Firewall webových aplikací (WAF) je služba (funkce Azure Application Gateway), která poskytuje centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. Služba Azure WAF vám může pomáhat zabezpečit vaše Azure App Service webové aplikace tím, že zkontroluje příchozí webový provoz a zablokuje útoky, jako jsou například injektáže SQL, skriptování mezi weby, nakládku malwaru a útoky DDoS. WAF je založená na pravidlech z OWASP (Open Web Application Security Project) základních sad pravidel 3,1 (jenom WAF_v2), 3,0 a 2.2.9.

Alternativně existuje více možností Marketplace, jako je Barracuda WAF pro Azure, které jsou k dispozici na Azure Marketplace, které zahrnují funkce ID a IP adresy.

* [Vysvětlení funkcí služby Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/features)

* [Pochopení Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview)

* [Jak nasadit Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

* [Principy cloudové služby Barracuda WAF](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-web-application-firewall#configuring-your-barracuda-waf-cloud-service)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Správa provozu do webových aplikací

**Pokyny**: nasazení služby Azure Application Gateway pro webové aplikace s POVOLENým protokolem HTTPS/SSL u důvěryhodných certifikátů.

* [Postup nasazení Application Gateway](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)

* [Postup konfigurace Application Gateway pro použití protokolu HTTPS](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)

* [Princip vyrovnávání zatížení vrstvy 7 pomocí bran webových aplikací Azure](https://docs.microsoft.com/azure/application-gateway/overview)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: Minimalizujte složitost a administrativní režii pravidel zabezpečení sítě

**Doprovodné**materiály: použití značek Virtual Network služby k definování řízení přístupu k síti u skupin zabezpečení sítě nebo Azure firewall. Značky služeb můžete používat místo konkrétních IP adres při vytváření pravidel zabezpečení. Zadáním názvu značky služby (např. GatewayManager) v příslušném zdrojovém nebo cílovém poli pravidla můžete povolit nebo odepřít provoz pro příslušnou službu. Společnost Microsoft spravuje předpony adres, které jsou součástí značky služby, a automaticky aktualizuje označení služby jako adresy změny.

Pro skupiny zabezpečení sítě (skupin zabezpečení sítě) přidružené k vašim podsítím Azure Application Gateway musíte povolen příchozí internetový provoz na portech TCP 65503-65534 pro SKU Application Gateway V1 a porty TCP 65200-65535 pro SKU v2 s cílovou podsítí jako libovolný a zdroj jako značka služby GatewayManager. Tento rozsah portů je nutný pro komunikaci s infrastrukturou Azure. Tyto porty jsou chráněné (uzamčené) pomocí certifikátů Azure. Externí entity, včetně zákazníků těchto bran, nemůžou komunikovat na těchto koncových bodech.

* [Pochopení a použití značek služeb](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Přehled konfigurace Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: Udržujte standardní konfigurace zabezpečení pro síťová zařízení.

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro nastavení sítě související s nasazeními Azure Application Gateway. Pomocí aliasů Azure Policy v oboru názvů Microsoft. Network můžete vytvářet vlastní zásady pro auditování nebo vymáhání konfigurace sítě vašich bran Azure Application Gateway, virtuálních sítí Azure a skupin zabezpečení sítě. Můžete také využít vestavěnou definici zásad.

Pomocí plánů Azure můžete také zjednodušit rozsáhlá nasazení Azure tím, že zabalíte klíčové artefakty prostředí, jako jsou například šablony Azure Resource Manager, řízení přístupu na základě role (RBAC) a zásady v jediné definici podrobného plánu. V rámci správy verzí můžete snadno použít podrobný plán na nová předplatná, prostředí a vyladit řízení a správu.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Vytvoření Azure Blueprint](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="110-document-traffic-configuration-rules"></a>1,10: pravidla pro konfiguraci provozu dokumentu

**Pokyny**: používejte značky pro skupiny zabezpečení sítě (skupin zabezpečení sítě) přidružené k vaší podsíti Azure Application Gateway a také všechny další prostředky, které se týkají zabezpečení sítě a toku provozu. U individuálních pravidel NSG použijte pole Popis k zadání obchodních potřeb a/nebo doby trvání (atd.) pro všechna pravidla, která umožňují provoz do/ze sítě.

Použijte některou z vestavěných definic zásad Azure souvisejících s označováním, jako je "vyžadovat značku a její hodnotu", aby se zajistilo, že se vytvoří všechny prostředky s značkami a upozorní vás na stávající neoznačené prostředky.

Pomocí Azure PowerShell nebo Azure CLI můžete vyhledávat nebo provádět akce s prostředky na základě jejich značek.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Vytvoření Virtual Network](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: pomocí automatizovaných nástrojů monitorujte konfigurace síťových prostředků a zjišťují změny.

**Pokyny**: pomocí protokolu aktivit Azure můžete monitorovat konfigurace síťových prostředků a zjišťovat změny nastavení sítě a prostředků souvisejících s nasazeními Azure Application Gateway. Vytvoří výstrahy v rámci Azure Monitor, které se aktivují, když dojde ke změně kritických nastavení sítě nebo prostředků.

* [Jak zobrazit a načíst události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Vytváření výstrah v Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

## <a name="logging-and-monitoring"></a>Protokolování a monitorování

*Další informace najdete v tématu [řízení zabezpečení: protokolování a monitorování](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: Použijte schválené zdroje synchronizace času

**Pokyny**: Společnost Microsoft udržuje zdroj času používaný pro prostředky Azure, například Azure App Service.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Konfigurace centrální správy protokolů zabezpečení

**Doprovodné**materiály: pro protokolování auditu řídicích rovin povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do Log Analytics pracovního prostoru, centra událostí Azure nebo účtu Azure Storage. Pomocí dat protokolu aktivit Azure můžete určit "co, kdo a kdy" pro všechny operace zápisu (PUT, POST, DELETE) prováděné na úrovni ovládacího prvku pro Azure Application Gateway a souvisejících prostředků, jako jsou třeba skupiny zabezpečení sítě (skupin zabezpečení sítě), se používají k ochraně podsítě Azure Application Gateway.

Kromě protokolů aktivit můžete nakonfigurovat nastavení diagnostiky pro nasazení služby Azure Application Gateway. nastavení diagnostiky se používají ke konfiguraci exportu protokolů a metrik platforem pro prostředek do cílového umístění podle vašeho výběru (účty úložiště, Event Hubs a Log Analytics).

Azure Application Gateway také nabízí integrovanou integraci s Azure Application Insights. Application Insights shromažďuje údaje o protokolech, výkonu a chybách. Application Insights automaticky detekuje anomálie výkonu a zahrnuje výkonné analytické nástroje, které vám pomůžou diagnostikovat problémy a pochopit, jak se používají vaše webové aplikace. Průběžný export můžete povolit pro export telemetrie z Application Insights do centralizovaného umístění, aby data byla delší než standardní doba uchovávání dat.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Postup povolení nastavení diagnostiky pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

* [Postup povolení Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)

* [Postup konfigurace průběžného exportu](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: povolení protokolování auditu pro prostředky Azure

**Doprovodné**materiály: pro protokolování auditu řídicích rovin povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do Log Analytics pracovního prostoru, centra událostí Azure nebo účtu Azure Storage. Pomocí dat protokolu aktivit Azure můžete určit "co, kdo a kdy" pro všechny operace zápisu (PUT, POST, DELETE) prováděné na úrovni ovládacího prvku pro Azure Application Gateway a souvisejících prostředků, jako jsou třeba skupiny zabezpečení sítě (skupin zabezpečení sítě), se používají k ochraně podsítě Azure Application Gateway.

Kromě protokolů aktivit můžete nakonfigurovat nastavení diagnostiky pro nasazení služby Azure Application Gateway. nastavení diagnostiky se používají ke konfiguraci exportu protokolů a metrik platforem pro prostředek do cílového umístění podle vašeho výběru (účty úložiště, Event Hubs a Log Analytics).

Azure Application Gateway také nabízí integrovanou integraci s Azure Application Insights. Application Insights shromažďuje údaje o protokolech, výkonu a chybách. Application Insights automaticky detekuje anomálie výkonu a zahrnuje výkonné analytické nástroje, které vám pomůžou diagnostikovat problémy a pochopit, jak se používají vaše webové aplikace. Průběžný export můžete povolit pro export telemetrie z Application Insights do centralizovaného umístění, aby data byla delší než standardní doba uchovávání dat.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Postup povolení nastavení diagnostiky pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

* [Postup povolení Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)

* [Postup konfigurace průběžného exportu](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: shromáždění protokolů zabezpečení z operačních systémů

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="25-configure-security-log-storage-retention"></a>2,5: Konfigurace uchovávání úložiště protokolu zabezpečení

**Doprovodné**materiály: v rámci Azure monitor nastavte dobu uchování pracovního prostoru Log Analytics podle předpisů pro dodržování předpisů vaší organizace. Používejte účty Azure Storage pro dlouhodobé a archivační úložiště.

* [Postup nastavení parametrů uchovávání protokolů pro Log Analytics pracovní prostory](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="26-monitor-and-review-logs"></a>2,6: Sledujte a kontrolujte protokoly

**Doprovodné**materiály: Povolte nastavení diagnostiky protokolu aktivit Azure a také nastavení diagnostiky pro váš Application Gateway Azure a protokoly odešlete do pracovního prostoru Log Analytics. Můžete provádět dotazy v Log Analytics k hledání podmínek, identifikaci trendů, analýze vzorů a poskytování mnoha dalších přehledů na základě shromážděných dat.

Pomocí Azure Monitor pro sítě získáte komplexní přehled o stavu a metrikách všech nasazených síťových prostředků, včetně vašich aplikačních bran Azure.

Volitelně můžete povolit a začlenit data do Azure Sentinel nebo SIEM třetí strany.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Postup povolení nastavení diagnostiky pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

* [Jak používat Azure Monitor pro sítě](https://docs.microsoft.com/azure/azure-monitor/insights/network-insights-overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: povolení výstrah pro aktivity neobvyklé

**Pokyny**: nasaďte SKU Azure Web Application firewall (WAF) v2 SKU před kritickými webovými aplikacemi za účelem další kontroly příchozího provozu. Firewall webových aplikací (WAF) je služba (funkce Azure Application Gateway), která poskytuje centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. Služba Azure WAF vám může pomáhat zabezpečit vaše Azure App Service webové aplikace tím, že zkontroluje příchozí webový provoz a zablokuje útoky, jako jsou například injektáže SQL, skriptování mezi weby, nakládku malwaru a útoky DDoS. WAF je založená na pravidlech z OWASP (Open Web Application Security Project) základních sad pravidel 3,1 (jenom WAF_v2), 3,0 a 2.2.9.

Povolte nastavení diagnostiky protokolu aktivit Azure a také nastavení diagnostiky pro Azure WAF a odešlete protokoly do pracovního prostoru Log Analytics. Můžete provádět dotazy v Log Analytics k hledání podmínek, identifikaci trendů, analýze vzorů a poskytování mnoha dalších přehledů na základě shromážděných dat. Můžete vytvářet výstrahy na základě vašich dotazů na pracovní prostor Log Analytics.

Pomocí Azure Monitor pro sítě získáte komplexní přehled o stavu a metrikách všech nasazených síťových prostředků, včetně vašich aplikačních bran Azure. V konzole Azure Monitor pro sítě můžete zobrazit a vytvořit výstrahy pro Azure Application Gateway.

* [Jak nasadit Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Postup povolení nastavení diagnostiky pro Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

* [Jak používat Azure Monitor pro sítě](https://docs.microsoft.com/azure/azure-monitor/insights/network-insights-overview)

* [Vytvoření upozornění v rámci Azure](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="28-centralize-anti-malware-logging"></a>2,8: centralizace protokolování proti malwaru

**Pokyny**: nasazení firewallu webových aplikací Azure (WAF) v2 před kritickými webovými aplikacemi pro další kontrolu příchozího provozu. Firewall webových aplikací (WAF) je služba (funkce Azure Application Gateway), která poskytuje centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. Služba Azure WAF vám může pomáhat zabezpečit vaše Azure App Service webové aplikace tím, že zkontroluje příchozí webový provoz a zablokuje útoky, jako jsou například injektáže SQL, skriptování mezi weby, nakládku malwaru a útoky DDoS.

Nakonfigurujte nastavení diagnostiky pro nasazení služby Azure Application Gateway. nastavení diagnostiky se používají ke konfiguraci exportu protokolů a metrik platforem pro prostředek do cílového umístění podle vašeho výběru (účty úložiště, Event Hubs a Log Analytics).

* [Jak nasadit Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

* [Jak nakonfigurovat nastavení diagnostiky pro Azure WAF](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="29-enable-dns-query-logging"></a>2,9: povolení protokolování dotazů DNS

**Doprovodné**materiály: nepoužitelné; Azure Application Gateway nezpracovává ani nevytváří protokoly související s DNS, které jsou přístupné pro uživatele.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="210-enable-command-line-audit-logging"></a>2,10: povolení protokolování auditu příkazového řádku

**Doprovodné**materiály: nepoužitelné; Tyto zásady jsou určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

## <a name="identity-and-access-control"></a>Identita a řízení přístupu

*Další informace najdete v tématu [řízení zabezpečení: identita a řízení přístupu](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: udržování inventáře účtů pro správu

**Doprovodné**materiály: Azure Active Directory (AD) mají předdefinované role, které se musí explicitně přiřadit a které jsou Queryable. Pomocí modulu Azure AD PowerShell můžete provádět ad hoc dotazy a zjišťovat účty, které jsou členy skupin pro správu.

* [Jak získat roli adresáře ve službě Azure AD pomocí PowerShellu](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Jak načíst členy role adresáře v Azure AD pomocí PowerShellu](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="32-change-default-passwords-where-applicable"></a>3,2: Změna výchozích hesel tam, kde je to možné

**Pokyny**: přístup roviny řízení přístupu k Azure Application Gateway je řízen prostřednictvím Azure Active Directory (AD). Azure AD nemá koncept výchozích hesel.

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: použijte vyhrazené účty pro správu.

**Doprovodné**materiály: vytvořte standardní operační postupy kolem používání vyhrazených účtů pro správu. Pomocí Azure Security Center správy identit a přístupu můžete monitorovat počet účtů pro správu.

Kromě toho můžete použít doporučení z Azure Security Center nebo integrovaných zásad Azure, jako je například:
- K vašemu předplatnému by měl být přiřazený víc než jeden vlastník.
- Zastaralé účty s oprávněním vlastníka by se měly odebrat z vašeho předplatného.
- Z vašeho předplatného byste měli odebrat externí účty s oprávněním vlastníka.

* [Použití Azure Security Center k monitorování identity a přístupu (Preview)](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Jak používat Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: použijte jednotné přihlašování (SSO) s Azure Active Directory

**Pokyny**: pomocí registrace aplikace Azure (instanční objekt) načtěte token, který se dá použít k interakci se službou Azure Application Gateway prostřednictvím volání rozhraní API.

* [Volání rozhraní Azure REST API](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Postup registrace klientské aplikace (instančního objektu) ve službě Azure AD](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)

* [Informace o rozhraní API pro Azure Recovery Services](https://docs.microsoft.com/rest/api/recoveryservices/)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: Používejte vícefaktorové ověřování pro veškerý přístup založený na Azure Active Directory

**Doprovodné**materiály: Povolte Azure AD MFA a sledujte Azure Security Center doporučení pro správu identit a přístupu.

* [Jak povolit vícefaktorové ověřování v Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Jak monitorovat identitu a přístup v rámci Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: Používejte vyhrazené počítače (privilegovaný přístup k pracovní stanici) pro všechny úlohy správy

**Doprovodné**materiály: použití privilegovaným přístupem (privilegovaných pracovních stanic) s MFA nakonfigurovaným pro přihlášení a konfiguraci prostředků Azure.

* [Další informace o pracovních stanicích s privilegovaným přístupem](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Jak povolit vícefaktorové ověřování v Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: protokolovat a upozornit na podezřelé aktivity z účtů pro správu

**Doprovodné**materiály: použití sestav zabezpečení Azure Active Directory pro generování protokolů a výstrah v případě, že v prostředí dojde k podezřelé nebo nebezpečné aktivitě. Pomocí Azure Security Center můžete monitorovat aktivitu identity a přístupu.

* [Jak identifikovat uživatele Azure AD označené příznakem rizika pro rizikové aktivity](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Jak monitorovat identitu uživatelů a aktivity přístupu v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Správa prostředků Azure pouze ze schválených umístění

**Pokyny**: pomocí pojmenovaných umístění podmíněného přístupu povolíte přístup jenom z konkrétních logických skupin rozsahů IP adres nebo zemí nebo oblastí.

* [Postup konfigurace pojmenovaných umístění v Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="39-use-azure-active-directory"></a>3,9: použijte Azure Active Directory

**Doprovodné**materiály: jako centrální ověřování a systém autorizací použijte Azure Active Directory (AAD). AAD chrání data pomocí silného šifrování pro data v klidovém umístění a při přenosu. AAD taky soli, hodnoty hash a bezpečně ukládají přihlašovací údaje uživatele.

* [Postup vytvoření a konfigurace instance AAD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: pravidelně kontrolovat a sjednotit přístup uživatelů

**Doprovodné**materiály: Azure AD poskytuje protokoly, které vám pomůžou zjistit zastaralé účty. Navíc můžete pomocí kontrol přístupu Azure identity efektivně spravovat členství ve skupinách, přístup k podnikovým aplikacím a přiřazování rolí. Přístup uživatelů se dá pravidelně kontrolovat, aby se zajistilo, že budou mít přístup jenom přípravní uživatelé.

* [Pochopení sestav Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Jak používat recenze Azure identity Access](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: sledování pokusů o přístup k deaktivovaným přihlašovacím údajům

**Doprovodné**materiály: máte přístup k aktivitám přihlášení ke službě Azure AD, k auditu a rizikovým zdrojům protokolu událostí, které vám umožní integraci s jakýmkoli nástrojem Siem/monitoring.

Tento proces můžete zjednodušit vytvořením nastavení diagnostiky pro Azure Active Directory uživatelských účtů a odesláním protokolů auditu a protokolů přihlášení do pracovního prostoru Log Analytics. Požadované výstrahy můžete nakonfigurovat v pracovním prostoru Log Analytics.

* [Jak integrovat protokoly aktivit Azure do Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: upozornění na odchylku chování přihlášení k účtu

**Pokyny**: pomocí funkcí Azure AD Identity Protection a detekce rizik můžete nakonfigurovat automatizované odezvy na zjištěné podezřelé akce týkající se identit uživatelů. Můžete také ingestovat data do služby Azure Sentinel pro další šetření.

* [Jak zobrazit rizikové přihlašování Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Jak nakonfigurovat a povolit zásady rizik ochrany identity](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: Poskytněte Microsoftu přístup k relevantním zákaznickým datům během scénářů podpory.

**Doprovodné**materiály: nepoužitelné; Customer Lockbox se nedá použít pro Azure Application Gateway.

* [Seznam služeb podporovaných Customer Lockbox](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: netýká se

## <a name="data-protection"></a>Ochrana dat

*Další informace najdete v tématu [řízení zabezpečení: Ochrana dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: Udržujte inventář citlivých informací

**Doprovodné**materiály: používejte značky, které vám pomůžou při sledování prostředků Azure, které ukládají nebo zpracovávají citlivé informace.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: izolujte systémy, které ukládají nebo zpracovávají citlivé informace.

**Pokyny**: implementace samostatných předplatných nebo skupin pro správu pro vývoj, testování a produkci. Zajistěte, aby všechna Virtual Network nasazení podsítí Application Gateway Azure používala skupinu zabezpečení sítě (NSG) s ovládacími prvky pro přístup k síti, které jsou specifické pro důvěryhodné porty a zdroje vaší aplikace. I když jsou v Azure Application Gateway podporovány skupiny zabezpečení sítě, existují určitá omezení a požadavky, které musí být dodrženy, aby vaše NSG a Azure Application Gateway fungovaly podle očekávání.

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Vysvětlení omezení a požadavků v používání skupin zabezpečení sítě s Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: Sledujte a zablokujte neoprávněný přenos citlivých informací

**Pokyny**: Ujistěte se, že všechna Virtual Network nasazení podsítí Application Gateway Azure obsahuje skupinu zabezpečení sítě (NSG) s ovládacími prvky pro přístup k síti, které jsou specifické pro důvěryhodné porty a zdroje vaší aplikace. Omezte odchozí provoz jenom na důvěryhodná umístění, aby se zabránilo zmírnění hrozby exfiltrace dat. I když jsou v Azure Application Gateway podporovány skupiny zabezpečení sítě, existují určitá omezení a požadavky, které musí být dodrženy, aby vaše NSG a Azure Application Gateway fungovaly podle očekávání.

* [Vysvětlení omezení a požadavků v používání skupin zabezpečení sítě s Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: sdílená

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: šifrování všech citlivých informací během přenosu

**Pokyny**: Konfigurace komplexního šifrování s protokolem TLS pro vaše aplikační brány Azure.

* [Jak nakonfigurovat kompletní protokol TLS pomocí Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/end-to-end-ssl-portal)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: sdílená

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: k identifikaci citlivých dat použijte aktivní nástroj zjišťování.

**Doprovodné**materiály: neplatí, Azure Application Gateway neukládá neaktivní data o zákaznících.

Microsoft spravuje základní infrastrukturu pro Azure Application Gateway a implementuje přísné ovládací prvky, které zabrání ztrátě nebo expozici zákaznických dat.

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: k řízení přístupu k prostředkům použijte řízení přístupu na základě role

**Pokyny**: pomocí Azure Active Directory (AD) řízení přístupu na základě role (RBAC) můžete řídit přístup k rovině ovládacího prvku Azure Application Gateway (Azure Portal).

* [Jak nakonfigurovat RBAC v Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: použití prevence ztráty dat na základě hostitele k vymáhání řízení přístupu

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: šifrování citlivých informací v klidovém umístění

**Doprovodné**materiály: nepoužitelné; V Azure Application Gateway neukládají zákaznická data.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: protokolovat a upozornit na změny kritických prostředků Azure

**Pokyny**: pomocí Azure monitor s protokolem aktivit Azure můžete vytvářet upozornění na případy, kdy změny probíhají v produkčních instancích Azure Application Gateway, i v jiných důležitých nebo souvisejících prostředcích.

* [Vytvoření upozornění pro události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="vulnerability-management"></a>Správa ohrožení zabezpečení

*Další informace najdete v tématu [řízení zabezpečení: Správa ohrožení](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)zabezpečení.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: spuštění automatizovaných nástrojů pro kontrolu ohrožení zabezpečení

**Doprovodné**materiály: aktuálně není k dispozici; posouzení ohrožení zabezpečení v Azure Security Center ještě není pro Azure Application Gateway dostupné.

Nadřazená platforma byla zkontrolována a opravena společností Microsoft. Přečtěte si bezpečnostní mechanismy, které jsou dostupné pro Azure Application Gateway, abyste snížili ohrožení zabezpečení související s konfigurací služby

* [Pokrytí funkcí (včetně posouzení ohrožení zabezpečení) pro služby Azure PaaS](https://docs.microsoft.com/azure/security-center/features-paas)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: nasazení automatizovaného řešení pro správu oprav operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: nasazení automatizovaného řešení pro správu oprav pro softwarové tituly třetích stran

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: porovnání kontrol zabezpečení back-to-back

**Doprovodné**materiály: ještě není k dispozici; posouzení ohrožení zabezpečení v Azure Security Center ještě není pro Azure Application Gateway dostupné.

Nadřazená platforma byla zkontrolována a opravena společností Microsoft. Přečtěte si bezpečnostní mechanismy, které jsou dostupné pro Azure Application Gateway, abyste snížili ohrožení zabezpečení související s konfigurací služby

* [Pokrytí funkcí (včetně posouzení ohrožení zabezpečení) pro služby Azure PaaS](https://docs.microsoft.com/azure/security-center/features-paas)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: použijte proces hodnocení rizik k určení priorit nápravy zjištěných ohrožení zabezpečení

**Doprovodné**materiály: ještě není k dispozici; posouzení ohrožení zabezpečení v Azure Security Center ještě není pro Azure Application Gateway dostupné.

Nadřazená platforma byla zkontrolována a opravena společností Microsoft. Přečtěte si bezpečnostní mechanismy, které jsou dostupné pro Azure Application Gateway, abyste snížili ohrožení zabezpečení související s konfigurací služby

* [Pokrytí funkcí (včetně posouzení ohrožení zabezpečení) pro služby Azure PaaS](https://docs.microsoft.com/azure/security-center/features-paas)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

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

Kromě toho použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v předplatných zákazníka, pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: definování a údržba inventáře schválených prostředků Azure

**Doprovodné**materiály: Definujte schválené prostředky Azure.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitorování neschválených prostředků Azure

**Doprovodné**materiály: použijte Azure Policy k omezení typu prostředků, které se dají vytvořit ve vašich předplatných.

Pomocí grafu prostředků Azure můžete v rámci svých předplatných dotazovat nebo zjišťovat prostředky. Ujistěte se, že všechny prostředky Azure přítomné v daném prostředí jsou schválené.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak vytvářet dotazy pomocí Azure graphu](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: monitorujte neschválené softwarové aplikace v rámci výpočetních prostředků.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: Odeberte neschválené prostředky Azure a softwarové aplikace

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="68-use-only-approved-applications"></a>6,8: Používejte pouze schválené aplikace.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="69-use-only-approved-azure-services"></a>6,9: Používejte jenom schválené služby Azure.

**Doprovodné**materiály: použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v zákaznických předplatných, pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak odepřít konkrétní typ prostředku pomocí Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: udržování inventáře schválených softwarových titulů

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: Omezte schopnost uživatelů pracovat s Azure Resource Manager

**Pokyny**: Nakonfigurujte podmíněný přístup Azure tak, aby uživatelé mohli komunikovat s Azure Resource Manager konfigurací možnosti blokovat přístup pro aplikaci Microsoft Azure Management.

* [Postup konfigurace podmíněného přístupu pro blokování přístupu k Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: Omezte schopnost uživatelů spouštět skripty ve výpočetních prostředcích.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: fyzicky nebo logicky oddělené aplikace s vysokým rizikem

**Pokyny**: implementace samostatných předplatných nebo skupin pro správu pro vývoj, testování a produkci. Zajistěte, aby všechna Virtual Network nasazení podsítí Application Gateway Azure používala skupinu zabezpečení sítě (NSG) s ovládacími prvky pro přístup k síti, které jsou specifické pro důvěryhodné porty a zdroje vaší aplikace. I když jsou v Azure Application Gateway podporovány skupiny zabezpečení sítě, existují určitá omezení a požadavky, které musí být dodrženy, aby vaše NSG a Azure Application Gateway fungovaly podle očekávání.

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Vysvětlení omezení a požadavků v používání skupin zabezpečení sítě s Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="secure-configuration"></a>Zabezpečená konfigurace

*Další informace najdete v tématu [řízení zabezpečení: zabezpečená konfigurace](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: Vytvoření zabezpečených konfigurací pro všechny prostředky Azure

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro nastavení sítě související s nasazeními Azure Application Gateway. Pomocí aliasů Azure Policy v oboru názvů Microsoft. Network můžete vytvářet vlastní zásady pro auditování nebo vymáhání konfigurace sítě vašich bran Azure Application Gateway, virtuálních sítí Azure a skupin zabezpečení sítě. Můžete také využít vestavěnou definici zásad.

* [Jak zobrazit dostupné aliasy Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: Vytvoření zabezpečených konfigurací operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: udržování zabezpečených konfigurací prostředků Azure

**Pokyny**: použijte zásady Azure [Deny] a [nasazení, pokud neexistuje], abyste vynutili zabezpečená nastavení napříč prostředky Azure.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Pochopení Azure Policych efektů](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: udržování zabezpečených konfigurací operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Konfigurace prostředků Azure v zabezpečeném úložišti

**Doprovodné**materiály: Pokud používáte vlastní definice zásad Azure, použijte k bezpečnému ukládání a správě kódu službu Azure DevOps nebo Azure Repos.

* [Jak v Azure DevOps ukládat kód](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentace k Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: bezpečné uložení vlastních imagí operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: nasazení nástrojů pro správu konfigurace pro prostředky Azure

**Doprovodné**materiály: použijte předdefinované definice Azure Policy a také Azure Policy aliasy v oboru názvů Microsoft. Network k vytváření vlastních zásad pro upozornění, audit a prosazování konfigurace systému. Dále můžete vyvinout proces a kanál pro správu výjimek zásad.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: nasazení nástrojů pro správu konfigurace pro operační systémy

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Implementujte automatizované monitorování konfigurace pro prostředky Azure.

**Doprovodné**materiály: použijte předdefinované definice Azure Policy a také Azure Policy aliasy v oboru názvů Microsoft. Network k vytváření vlastních zásad pro upozornění, audit a prosazování konfigurace systému. K automatickému vymáhání konfigurací pro prostředky Azure použijte zásady Azure [audit], [Deny] a [nasazení, pokud neexistuje].

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Implementujte automatizované monitorování konfigurace pro operační systémy

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro IaaS výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="711-manage-azure-secrets-securely"></a>7,11: zabezpečená Správa tajných kódů Azure

**Pokyny**: použití spravovaných identit k poskytnutí Application Gateway Azure s automaticky spravovanou identitou v Azure Active Directory (AD). Spravované identity vám umožňují ověřit jakoukoli službu, která podporuje ověřování Azure AD, včetně Key Vault bez jakýchkoli přihlašovacích údajů ve vašem kódu.

Používejte Azure Key Vault k bezpečnému ukládání certifikátů. Azure Key Vault je úložiště tajné databáze spravované platformou, které můžete použít k ochraně tajných klíčů, klíčů a certifikátů SSL. Azure Application Gateway podporuje integraci s Key Vault pro certifikáty serveru, které jsou připojené k naslouchacím procesům s povoleným protokolem HTTPS. Tato podpora je omezená na SKU Application Gateway v2.

* [Jak nakonfigurovat ukončení protokolu SSL pomocí Key Vault certifikátů pomocí Azure PowerShell](https://docs.microsoft.com/azure/application-gateway/configure-keyvault-ps)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: bezpečně a automaticky spravujte identity

**Pokyny**: použití spravovaných identit k poskytnutí Application Gateway Azure s automaticky spravovanou identitou v Azure Active Directory (AD). Spravované identity vám umožňují ověřit jakoukoli službu, která podporuje ověřování Azure AD, včetně Key Vault bez jakýchkoli přihlašovacích údajů ve vašem kódu.

Používejte Azure Key Vault k bezpečnému ukládání certifikátů. Azure Key Vault je úložiště tajné databáze spravované platformou, které můžete použít k ochraně tajných klíčů, klíčů a certifikátů SSL. Azure Application Gateway podporuje integraci s Key Vault pro certifikáty serveru, které jsou připojené k naslouchacím procesům s povoleným protokolem HTTPS. Tato podpora je omezená na SKU Application Gateway v2.

* [Jak nakonfigurovat ukončení protokolu SSL pomocí Key Vault certifikátů pomocí Azure PowerShell](https://docs.microsoft.com/azure/application-gateway/configure-keyvault-ps)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: Eliminujte nezamýšlenou expozici přihlašovacích údajů

**Pokyny**: implementace skeneru přihlašovacích údajů pro identifikaci přihlašovacích údajů v rámci kódu. Skener přihlašovacích údajů taky bude povzbudit přesunutí zjištěných přihlašovacích údajů do bezpečnějších umístění, jako je Azure Key Vault.

* [Jak nastavit skener přihlašovacích údajů](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="malware-defense"></a>Obrana před malwarem

*Další informace najdete v tématu [řízení zabezpečení: obrana proti malwaru](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: použití centrálně spravovaného malwarového softwaru

**Pokyny**: nasazení firewallu webových aplikací Azure (WAF) v2 před kritickými webovými aplikacemi pro další kontrolu příchozího provozu. Firewall webových aplikací (WAF) je služba (funkce Azure Application Gateway), která poskytuje centralizovanou ochranu webových aplikací před běžným zneužitím a ohrožením zabezpečení. Služba Azure WAF vám může pomáhat zabezpečit vaše Azure App Service webové aplikace tím, že zkontroluje příchozí webový provoz a zablokuje útoky, jako jsou například injektáže SQL, skriptování mezi weby, nakládku malwaru a útoky DDoS.

Nakonfigurujte nastavení diagnostiky pro nasazení služby Azure Application Gateway. nastavení diagnostiky se používají ke konfiguraci exportu protokolů a metrik platforem pro prostředek do cílového umístění podle vašeho výběru (účty úložiště, Event Hubs a Log Analytics).

* [Jak nasadit Azure WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag)

* [Jak nakonfigurovat nastavení diagnostiky pro Azure WAF](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: předběžná kontrola souborů, které se mají nahrát do prostředků Azure, které nejsou COMPUTE

**Doprovodné**materiály: nepoužitelné; V Azure Application Gateway neukládají zákaznická data.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: Ujistěte se, že antimalwarový software a signatury jsou aktualizované.

**Doprovodné**materiály: při použití firewallu webových aplikací Azure (WAF) můžete nakonfigurovat zásady WAF. Zásady WAF se skládají ze dvou typů pravidel zabezpečení: vlastní pravidla, která jsou vytvořená zákazníkem, a sady spravovaných pravidel, které jsou kolekcí předem nakonfigurovaných sad pravidel spravovaných Azure. Sady pravidel spravované pomocí Azure poskytují snadný způsob, jak nasadit ochranu proti běžné sadě bezpečnostních hrozeb. Vzhledem k tomu, že tato RuleSets spravuje Azure, pravidla se aktualizují podle potřeby k ochraně před novými signaturami útoku.

* [Porozumění sadám pravidel WAF spravovaných v Azure](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview#waf-policy)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: sdílená

## <a name="data-recovery"></a>Obnovení dat

*Další informace najdete v tématu [řízení zabezpečení – obnovení dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: zajištění pravidelného automatického zálohování

**Pokyny**: v Azure Application Gateway neukládají zákaznická data. Pokud ale používáte vlastní definice zásad Azure, použijte k bezpečnému ukládání a správě kódu službu Azure DevOps nebo Azure Repos.

Azure DevOps Services využívá mnoho funkcí úložiště Azure k zajištění dostupnosti dat v případě selhání hardwaru, přerušení služby nebo havárie v oblasti. Kromě toho tým Azure DevOps sleduje postupy, které chrání data před náhodným nebo škodlivým odstraněním.

* [Pochopení dostupnosti dat v Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/data-protection?view=azure-devops#data-availability)

* [Jak v Azure DevOps ukládat kód](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentace k Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: proveďte kompletní systémové zálohy a zálohujte všechny spravované klíče zákazníka.

**Pokyny**: zálohování spravovaných certifikátů zákazníků v rámci Azure Key Vault.

* [Postup zálohování certifikátů trezoru klíčů v Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: Ověřte všechny zálohy včetně spravovaných klíčů zákazníků.

**Pokyny**: testování obnovení zálohovaných certifikátů spravovaných zákazníkem.

* [Postup obnovení certifikátů trezoru klíčů](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: Zajistěte ochranu záloh a spravovaných klíčů zákazníků.

**Pokyny**: Ujistěte se, že je pro Azure Key Vault povolené obnovitelné odstranění. Obnovitelné odstranění umožňuje obnovit odstraněné trezory klíčů a objekty trezoru, jako jsou klíče, tajné klíče a certifikáty.

* [Použití obnovitelného odstranění Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell)

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
