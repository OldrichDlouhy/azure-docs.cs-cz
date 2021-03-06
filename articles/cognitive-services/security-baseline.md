---
title: Základní hodnoty zabezpečení Azure pro Cognitive Services
description: Základní hodnoty zabezpečení Azure pro Cognitive Services
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 1c5ce50a3736d6e96620e25cf084c5c66c456a5f
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2020
ms.locfileid: "85375107"
---
# <a name="azure-security-baseline-for-cognitive-services"></a>Základní hodnoty zabezpečení Azure pro Cognitive Services

Základní plán zabezpečení Azure pro Cognitive Services obsahuje doporučení, která vám pomůžou vylepšit stav zabezpečení vašeho nasazení.

Základní hodnota této služby se vykreslí z [bezpečnostního testu Azure Security 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview), který poskytuje doporučení k zabezpečení cloudových řešení v Azure s využitím našich osvědčených postupů.

Další informace najdete v tématu [Přehled standardních hodnot zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Zabezpečení sítě

*Další informace najdete v tématu [řízení zabezpečení: zabezpečení sítě](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: Ochrana prostředků Azure v rámci virtuálních sítí

**Doprovodné**materiály: Azure Cognitive Services poskytuje vrstvený model zabezpečení. Tento model vám umožní zabezpečit účty Cognitive Services pro konkrétní podmnožinu sítí. Při konfiguraci síťových pravidel mají přístup k účtu jenom aplikace požadující data přes zadanou sadu sítí. Můžete omezit přístup k vašim prostředkům pomocí filtrování požadavků a povolit pouze požadavky, které pocházejí ze zadaných IP adres, rozsahů IP adres nebo ze seznamu podsítí ve službě Azure Virtual Network.

Podpora služby Virtual Network a koncového bodu služby pro Cognitive Services je omezená na konkrétní sadu oblastí.

* [Jak nakonfigurovat virtuální sítě Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal)

* [Přehled virtuálních sítí Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: Sledujte a protokolujte konfiguraci a provoz virtuálních sítí, podsítí a síťových karet

**Pokyny**: když Virtual Machines nasadíte ve stejné virtuální síti jako kontejner Azure Cognitive Services, můžete použít skupiny zabezpečení sítě (NSG) a snížit tak riziko exfiltrace dat. Povolte protokoly toku NSG a odešlete protokoly do účtu Azure Storage pro audit provozu. Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="13-protect-critical-web-applications"></a>1,3: Chraňte kritické webové aplikace

**Doprovodné**materiály: pokud používáte Cognitive Services v rámci kontejneru, můžete rozšířit nasazení kontejneru pomocí řešení firewallu webových aplikací s front-endu, které filtruje škodlivý provoz a podporuje komplexní šifrování TLS, a přitom udržuje koncový bod kontejneru privátní a bezpečný.

Pamatujte, že Cognitive Services kontejnery jsou požadovány k odeslání informací o měření pro účely fakturace. Jedinou výjimkou jsou offline kontejnery, které sledují jinou metodologii fakturace. Nepovedlo se zakázat seznam různých síťových kanálů, na kterých se spoléhá Cognitive Services kontejnery, aby se zabránilo tomu, že kontejner nebude fungovat. Hostitel by měl povolený seznam port 443 a následující domény:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Všimněte si také, že je nutné zakázat hloubkovou kontrolu paketů pro vaše řešení brány firewall na zabezpečených kanálech, které kontejnery Cognitive Services vytvoří na serverech společnosti Microsoft. V takovém případě se zabrání správnému fungování kontejneru.

* [Principy zabezpečení služby Azure Cognitive Services Container](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: zakažte komunikaci se známými škodlivými IP adresami.

**Doprovodné**materiály: když jsou virtuální počítače nasazené ve stejné virtuální síti jako kontejner Azure Cognitive Services, definujte a Implementujte standardní konfigurace zabezpečení pro související síťové prostředky s Azure Policy. Pomocí aliasů Azure Policy v oborech názvů Microsoft. Cognitiveservices Account a Microsoft. Network můžete vytvářet vlastní zásady pro auditování nebo vymáhání konfigurace sítě pro instance Azure cache. Můžete také využít integrované definice zásad, například:
- Měla by být povolená DDoS Protection Standard.

Plány Azure můžete použít také ke zjednodušení rozsáhlých nasazení Azure tím, že zabalíte artefakty klíčových prostředí, jako jsou například šablony Azure Resource Manager, řízení přístupu na základě role (RBAC) a zásady v rámci jedné definice podrobného plánu. Podrobné sestavování můžete snadno použít pro nová předplatná a prostředí a vyladit řízení a správu prostřednictvím správy verzí.

Pokud používáte Cognitive Services v rámci kontejneru, můžete rozšířit nasazení kontejnerů pomocí řešení firewallu webových aplikací s front-endu, které filtruje škodlivý provoz a podporuje komplexní šifrování TLS, a přitom udržuje koncový bod kontejneru privátní a zabezpečený.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Vytvoření Azure Blueprint](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

* [Principy zabezpečení služby Azure Cognitive Services Container](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="15-record-network-packets"></a>1,5: zaznamenání síťových paketů

**Doprovodné**materiály: Pokud jsou virtuální počítače nasazené ve stejné virtuální síti jako kontejner Azure Cognitive Services, můžete použít skupiny zabezpečení sítě (NSG) a snížit tak riziko exfiltrace dat. Povolte protokoly toku NSG a odešlete protokoly do účtu Azure Storage pro audit provozu. Protokoly toku NSG můžete také odesílat do pracovního prostoru Log Analytics a používat Analýza provozu k poskytování přehledů o toku přenosů ve vašem cloudu Azure. Mezi výhody Analýza provozu patří schopnost vizualizovat síťovou aktivitu a identifikovat aktivní body, identifikovat bezpečnostní hrozby, pochopit vzory toků provozu a označovat nesprávné konfigurace sítě.

* [Jak povolit protokoly toku NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Postup povolení a použití Analýza provozu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: nasazení systémů ochrany před internetovými útoky/systémy prevence vniknutí (ID/IP adresy)

**Doprovodné**materiály: pokud používáte Cognitive Services v rámci kontejneru, můžete rozšířit nasazení kontejneru pomocí řešení firewallu webových aplikací s front-endu, které filtruje škodlivý provoz a podporuje komplexní šifrování TLS, a přitom zachovat privátní a zabezpečené koncové body kontejneru. Můžete vybrat nabídku z Azure Marketplace, která podporuje funkčnost IDENTIFIKÁTORů/IP adres, s možností zakázat kontrolu datové části.

Pamatujte, že Cognitive Services kontejnery jsou požadovány k odeslání informací o měření pro účely fakturace. Jedinou výjimkou jsou offline kontejnery, protože sledují jinou metodologii fakturace. Nepovedlo se zakázat seznam různých síťových kanálů, na kterých se spoléhá Cognitive Services kontejnery, aby se zabránilo tomu, že kontejner nebude fungovat. Hostitel by měl povolený seznam port 443 a následující domény:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Všimněte si také, že je nutné zakázat hloubkovou kontrolu paketů pro vaše řešení brány firewall na zabezpečených kanálech, které kontejnery Cognitive Services vytvoří na serverech společnosti Microsoft. V takovém případě se zabrání správnému fungování kontejneru.

* [Principy zabezpečení služby Azure Cognitive Services Container](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Správa provozu do webových aplikací

**Doprovodné**materiály: pokud používáte Cognitive Services v rámci kontejneru, můžete rozšířit nasazení kontejneru pomocí řešení firewallu webových aplikací s front-endu, které filtruje škodlivý provoz a podporuje komplexní šifrování TLS, a přitom zachovat privátní a zabezpečené koncové body kontejneru.

Pamatujte, že Cognitive Services kontejnery jsou požadovány k odeslání informací o měření pro účely fakturace. Jedinou výjimkou jsou offline kontejnery, které sledují jinou metodologii fakturace. Nepovedlo se zakázat seznam různých síťových kanálů, na kterých se spoléhá Cognitive Services kontejnery, aby se zabránilo tomu, že kontejner nebude fungovat. Hostitel by měl povolený seznam port 443 a následující domény:
- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

Všimněte si také, že je nutné zakázat hloubkovou kontrolu paketů pro vaše řešení brány firewall na zabezpečených kanálech, které kontejnery Cognitive Services vytvoří na serverech společnosti Microsoft. V takovém případě se zabrání správnému fungování kontejneru.

* [Principy zabezpečení služby Azure Cognitive Services Container](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: Minimalizujte složitost a administrativní režii pravidel zabezpečení sítě

**Pokyny**: pomocí značek služby virtuální sítě můžete definovat řízení přístupu k síti pro skupiny zabezpečení sítě (NSG) nebo Azure firewall. Značky služeb můžete používat místo konkrétních IP adres při vytváření pravidel zabezpečení. Zadáním názvu značky služby (např. ApiManagement) v příslušném zdrojovém nebo cílovém poli pravidla můžete povolit nebo odepřít provoz pro příslušnou službu. Společnost Microsoft spravuje předpony adres, které jsou součástí značky služby, a automaticky aktualizuje označení služby jako adresy změny.

Můžete také použít skupiny zabezpečení aplikací (ASG), které vám pomohou zjednodušit složitou konfiguraci zabezpečení. Skupiny ASG vám umožní nakonfigurovat zabezpečení sítě jako přirozené rozšíření struktury aplikace, což vám umožní seskupovat virtuální počítače a definovat zásady zabezpečení sítě na základě těchto skupin.

* [Značky služby virtuální sítě](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Skupiny zabezpečení aplikací](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: Udržujte standardní konfigurace zabezpečení pro síťová zařízení.

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro síťové prostředky související s vaším kontejnerem Cognitive Services Azure pomocí Azure Policy. Pomocí aliasů Azure Policy v oborech názvů Microsoft. Cognitiveservices Account a Microsoft. Network můžete vytvářet vlastní zásady pro auditování nebo vymáhání konfigurace sítě pro instance Azure cache.

Pomocí plánů Azure můžete také zjednodušit rozsáhlá nasazení Azure tím, že zabalíte klíčové artefakty prostředí, jako jsou například šablony Azure Resource Manager, řízení přístupu na základě role (RBAC) a zásady v rámci jedné definice podrobného plánu. Podrobné sestavování můžete snadno použít pro nová předplatná a prostředí a vyladit řízení a správu prostřednictvím správy verzí.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Vytvoření Azure Blueprint](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="110-document-traffic-configuration-rules"></a>1,10: pravidla pro konfiguraci provozu dokumentu

**Doprovodné**materiály: používejte značky pro síťové prostředky přidružené k vašemu kontejneru Azure Cognitive Services, aby je bylo možné logicky uspořádat do taxonomie.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: pomocí automatizovaných nástrojů monitorujte konfigurace síťových prostředků a zjišťují změny.

**Pokyny**: pomocí protokolu aktivit Azure můžete monitorovat konfigurace síťových prostředků a zjišťovat změny síťových prostředků, které se vztahují k vašemu kontejneru Azure Cognitive Services. Vytvoří výstrahy v rámci Azure Monitor, které se aktivují, když budou provedeny změny v kritických síťových prostředcích.

* [Jak zobrazit a načíst události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Vytváření výstrah v Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

## <a name="logging-and-monitoring"></a>Protokolování a monitorování

*Další informace najdete v tématu [řízení zabezpečení: protokolování a monitorování](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: Použijte schválené zdroje synchronizace času

**Pokyny**: Společnost Microsoft udržuje zdroj času používaný pro prostředky Azure, jako je například Azure Cognitive Services pro časová razítka v protokolech.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Konfigurace centrální správy protokolů zabezpečení

**Doprovodné**materiály: Povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do Log Analytics pracovního prostoru, centra událostí Azure nebo účtu úložiště Azure pro archivaci. Protokoly aktivit poskytují přehled o operacích, které byly provedeny na kontejneru Azure Cognitive Services na úrovni roviny řízení. Pomocí dat protokolu aktivit Azure můžete určit "co, kdo a kdy" pro všechny operace zápisu (PUT, POST, DELETE) prováděné na úrovni řídicích roviny pro instance služby Azure cache pro Redis.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: povolení protokolování auditu pro prostředky Azure

**Doprovodné**materiály: pro protokolování auditu řídicích rovin povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do Log Analytics pracovního prostoru, centra událostí Azure nebo účtu Azure Storage pro archivaci. Pomocí dat protokolu aktivit Azure můžete určit "co, kdo a kdy" pro všechny operace zápisu (PUT, POST, DELETE) prováděné na úrovni ovládacího prvku pro vaše prostředky Azure.

Kromě toho Azure Cognitive Services odesílá diagnostické události, které se dají shromáždit a použít pro účely analýzy, upozorňování a vytváření sestav. Nastavení diagnostiky pro kontejner Cognitive Services můžete nakonfigurovat pomocí Azure Portal. K účtu úložiště, centru událostí nebo pracovnímu prostoru Log Analytics můžete odeslat jednu nebo více diagnostických událostí.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Použití nastavení diagnostiky pro pro Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/diagnostic-logging)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: shromáždění protokolů zabezpečení z operačních systémů

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="25-configure-security-log-storage-retention"></a>2,5: Konfigurace uchovávání úložiště protokolu zabezpečení

**Doprovodné**materiály: v rámci Azure monitor nastavte dobu uchování pracovního prostoru Log Analytics podle předpisů pro dodržování předpisů vaší organizace. Používejte účty Azure Storage pro dlouhodobé a archivační úložiště.

* [Postup nastavení parametrů uchovávání protokolů pro Log Analytics pracovní prostory](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="26-monitor-and-review-logs"></a>2,6: Sledujte a kontrolujte protokoly

**Doprovodné**materiály: Povolte nastavení diagnostiky protokolu aktivit Azure a odešlete protokoly do pracovního prostoru Log Analytics. Tyto protokoly poskytují bohatá a často používaná data o provozu prostředku, který se používá k identifikaci a ladění problémů. Provádějte dotazy v Log Analytics k hledání podmínek, identifikujte trendy, analyzujte vzorce a poskytněte spoustu dalších přehledů na základě dat protokolů aktivit, které se mohly shromažďovat pro Azure Cognitive Services.

* [Postup povolení nastavení diagnostiky pro protokol aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Jak shromažďovat a analyzovat protokoly aktivit Azure v pracovním prostoru Log Analytics v Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: povolení výstrah pro aktivity neobvyklé

**Doprovodné**materiály: upozornění na podporované metriky v Azure Cognitive Services můžete vyvolat v &amp; části metriky výstrahy v Azure monitor.

Nakonfigurujte nastavení diagnostiky pro kontejner Cognitive Services a odešlete protokoly do pracovního prostoru Log Analytics. V rámci pracovního prostoru Log Analytics nakonfigurujte výstrahy, které se mají provést, když dojde k předdefinované sadě podmínek. Alternativně můžete povolit a začlenit data do Azure Sentinel nebo SIEM třetí strany.

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Vytváření, zobrazování a správa výstrah protokolu pomocí Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="28-centralize-anti-malware-logging"></a>2,8: centralizace protokolování proti malwaru

**Doprovodné**materiály: nepoužitelné; Azure Cognitive Services nezpracovává ani nevytváří protokoly související s malwarem.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="29-enable-dns-query-logging"></a>2,9: povolení protokolování dotazů DNS

**Doprovodné**materiály: nepoužitelné; Azure Cognitive Services nezpracovává ani nevytváří protokoly související s DNS.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="210-enable-command-line-audit-logging"></a>2,10: povolení protokolování auditu příkazového řádku

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

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

**Pokyny**: přístup roviny řízení přístupu k Azure Cognitive Services je řízen prostřednictvím Azure Active Directory (AD). Azure AD nemá koncept výchozích hesel.

Přístup k rovině dat do Azure Cognitive Services je řízen prostřednictvím přístupových klíčů. Tyto klíče používají klienti, kteří se připojují ke svojí mezipaměti a dají se kdykoli znovu vygenerovat.

Nedoporučujeme vytvářet výchozí hesla do aplikace. Místo toho můžete ukládat hesla v Azure Key Vault a pak je pomocí Azure Active Directory načíst.

* [Jak znovu vygenerovat Azure cache pro přístupové klíče Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings)

**Monitorování Azure Security Center**: nelze použít

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

**Pokyny**: Azure Cognitive Services používá přístupové klíče k ověřování uživatelů a nepodporuje jednotné přihlašování (SSO) na úrovni datové roviny. Přístup k rovině ovládacího prvku pro Azure Cognitive Services je k dispozici prostřednictvím REST API a podporuje jednotné přihlašování. Pro ověření nastavte hlavičku autorizace pro vaše požadavky na JSON Web Token, které získáte z Azure Active Directory.

* [Pochopení Cognitive Services Azure REST API](https://docs.microsoft.com/rest/api/cognitiveservices/)

* [Vysvětlení jednotného přihlašování pomocí Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: Používejte vícefaktorové ověřování pro veškerý přístup založený na Azure Active Directory

**Doprovodné**materiály: Povolte Azure Active Directory (AD) Multi-Factor Authentication (MFA) a sledujte Azure Security Center doporučení pro správu identit a přístupu.

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

**Pokyny**: pomocí Azure Active Directory (AD) PRIVILEGED Identity Management (PIM) můžete generovat protokoly a výstrahy, když dojde k podezřelé nebo nebezpečné aktivitě v prostředí.

Navíc můžete pomocí zjišťování rizik Azure AD zobrazovat výstrahy a sestavy týkající se rizikového chování uživatelů.

* [Postup nasazení Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Vysvětlení zjišťování rizik Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Správa prostředků Azure pouze ze schválených umístění

**Pokyny**: Nakonfigurujte pojmenovaná umístění v Azure Active Directory (AD) podmíněný přístup, aby se povolil přístup jenom z konkrétních logických skupin rozsahů IP adres nebo zemí nebo oblastí.

* [Postup konfigurace pojmenovaných umístění v Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="39-use-azure-active-directory"></a>3,9: použijte Azure Active Directory

**Doprovodné**materiály: jako centrální ověřování a systém autorizací použijte Azure Active Directory (AD). Azure AD chrání data pomocí silného šifrování pro neaktivní a tranzitní data. Azure AD také nasolete, hodnoty hash a bezpečně ukládají přihlašovací údaje uživatele. Pokud váš případ použití podporuje ověřování AD, použijte Azure AD k ověřování požadavků na rozhraní Cognitive Services API.

V současné době platí jenom rozhraní API pro počítačové zpracování obrazu, Face API, rozhraní API pro analýzu textu, moderní čtečka, funkce pro rozpoznávání formulářů, detektor anomálií a všechny služby Bingu s výjimkou Vlastní vyhledávání Bingu ověřování pomocí Azure AD.

* [Ověření požadavků na Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-azure-active-directory)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: pravidelně kontrolovat a sjednotit přístup uživatelů

**Doprovodné**materiály: Azure AD poskytuje protokoly, které vám pomůžou zjistit zastaralé účty. Kromě toho zákazník využije kontroly přístupu Azure identity k efektivní správě členství ve skupinách, přístupu k podnikovým aplikacím a přiřazování rolí. Přístup uživatele se může pravidelně kontrolovat, aby se zajistilo, že budou mít přístup jenom přípravní uživatelé.

Zákazníkům udržovat inventář API Managementch uživatelských účtů, podle potřeby sjednotit přístup. V API Management jsou vývojáři uživateli rozhraní API, které vystavíte pomocí API Management. Ve výchozím nastavení jsou nově vytvořené účty pro vývojáře aktivní a přidruženy ke skupině Developers. Účty pro vývojáře, které jsou v aktivním stavu, se dají použít pro přístup ke všem rozhraním API, pro která mají předplatná.

* [Správa uživatelských účtů ve službě Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-create-or-invite-developers)

* [Jak získat seznam API Management uživatelů](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-3.1.0)

* [Jak používat recenze Azure identity Access](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: sledování pokusů o přístup k deaktivovaným přihlašovacím údajům

**Doprovodné**materiály: máte přístup k aktivitě přihlašování Azure Active Directory (AD), událostem auditu a rizikovým protokolům událostí, které umožňují integraci s ověřováním Azure Sentinel nebo Siem třetí strany.

Tento proces můžete zjednodušit vytvořením nastavení diagnostiky pro uživatelské účty Azure AD a odesláním protokolů auditu a protokolů přihlášení do Log Analytics pracovního prostoru. Požadované výstrahy protokolu můžete nakonfigurovat v rámci Log Analytics.

* [Jak integrovat protokoly aktivit Azure do Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Postup zprovoznění služby Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: upozornění na odchylku chování přihlášení k účtu

**Doprovodné**materiály: pro odchylku chování přihlášení k účtu u roviny ovládacího prvku použijte funkce Azure Active Directory (AD) Identity Protection a detekce rizik ke konfiguraci automatizovaných odpovědí na zjištěné podezřelé akce týkající se identit uživatelů. Můžete také ingestovat data do služby Azure Sentinel pro další šetření.

* [Jak zobrazit rizikové přihlašování Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Jak nakonfigurovat a povolit zásady rizik ochrany identity](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Jak připojit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: Poskytněte Microsoftu přístup k relevantním zákaznickým datům během scénářů podpory.

**Doprovodné**materiály: ještě není k dispozici; Customer Lockbox pro Azure Cognitive Services ještě není podporovaný.

* [Seznam služeb podporovaných Customer Lockbox](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: aktuálně není k dispozici.

## <a name="data-protection"></a>Ochrana dat

*Další informace najdete v tématu [řízení zabezpečení: Ochrana dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: Udržujte inventář citlivých informací

**Doprovodné**materiály: používejte značky, které vám pomůžou při sledování prostředků Azure, které ukládají nebo zpracovávají citlivé informace.

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: izolujte systémy, které ukládají nebo zpracovávají citlivé informace.

**Pokyny**: implementace samostatných předplatných nebo skupin pro správu pro vývoj, testování a produkci. Prostředky by měly být oddělené podle virtuální sítě a podsítě, musí se vhodně označit a zabezpečit pomocí NSG nebo Azure Firewall. Prostředky, které ukládají nebo zpracovávají citlivá data, by měly být dostatečně izolované. Pokud Virtual Machines ukládáte nebo zpracováváte citlivá data, implementujte zásady a postupy pro jejich vypnutí, pokud se nepoužívají.

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Vytvoření Virtual Network](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Vytvoření NSG s konfigurací zabezpečení](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

* [Postup nasazení Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)

* [Jak nakonfigurovat výstrahu nebo upozornění a odepřít pomocí Azure Firewall](https://docs.microsoft.com/azure/firewall/threat-intel)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: Sledujte a zablokujte neoprávněný přenos citlivých informací

**Doprovodné**materiály: ještě není k dispozici; pro Azure Cognitive Services ještě nejsou dostupné funkce pro identifikaci, klasifikaci a ochranu před únikem informací.

Microsoft spravuje základní infrastrukturu pro Azure Cognitive Services a implementuje přísné ovládací prvky, které zabrání ztrátě nebo expozici zákaznických dat.

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: aktuálně není k dispozici.

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: šifrování všech citlivých informací během přenosu

**Doprovodné**materiály: všechny koncové body Cognitive Services vystavené přes protokol HTTP vynutil TLS 1,2. S vynutilým protokolem zabezpečení se zákazníci, kteří se pokoušejí zavolat na Cognitive Services koncový bod, musí dodržovat tyto pokyny:
- Operační systém klienta (OS) musí podporovat protokol TLS 1,2.
- Jazyk (a platforma), který se používá k provedení volání HTTP, musí jako součást žádosti zadat TLS 1,2. (V závislosti na jazyku a platformě je určení TLS provedeno implicitně nebo explicitně.)

* [Principy zabezpečení transportní vrstvy pro Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Odpovědnost**: sdílená

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: k identifikaci citlivých dat použijte aktivní nástroj zjišťování.

**Doprovodné**materiály: funkce pro identifikaci, klasifikaci a ochranu před únikem informací ještě nejsou pro Azure Cognitive Services k dispozici. Označení instancí obsahujících citlivé údaje jako takové a implementace řešení třetích stran, pokud je to nutné pro účely dodržování předpisů.

Pro základní platformu, která je spravovaná Microsoftem, Microsoft považuje veškerý obsah zákazníka za citlivý a vede na skvělé délky, aby se zabránilo ochraně před ztrátou a únikem informací a riziky zákazníků. Aby se zajistilo zabezpečení zákaznických dat v Azure, společnost Microsoft implementovala a udržuje sadu robustních ovládacích prvků a možností ochrany dat.

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: sdílená

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: k řízení přístupu k prostředkům použijte řízení přístupu na základě role

**Pokyny**: pomocí Azure Active Directory (Azure AD) řízení přístupu na základě role (RBAC) můžete řídit přístup k rovině ovládacího prvku Azure Cognitive Services (tj. Azure Portal).

* [Jak nakonfigurovat službu Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: použití prevence ztráty dat na základě hostitele k vymáhání řízení přístupu

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

Microsoft spravuje základní infrastrukturu pro Azure Cognitive Services a implementuje přísné ovládací prvky, které zabrání ztrátě nebo expozici zákaznických dat.

* [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: šifrování citlivých informací v klidovém umístění

**Pokyny**: šifrování v klidovém provozu pro Cognitive Services závisí na používané konkrétní službě. Ve většině případů se data šifrují a dešifrují s využitím 256 šifrování AES kompatibilního se standardem FIPS 140-2. Šifrování a dešifrování je transparentní, což znamená, že se pro vás spravuje šifrování a přístup. Vaše data jsou ve výchozím nastavení zabezpečená a nemusíte upravovat kód ani aplikace, abyste mohli využívat šifrování.

K ukládání klíčů spravovaných zákazníkem můžete použít taky Azure Key Vault. Můžete buď vytvořit vlastní klíče a uložit je do trezoru klíčů, nebo můžete použít rozhraní API Azure Key Vault k vygenerování klíčů.

* [Seznam služeb, které zašifrují informace v klidovém znění](https://docs.microsoft.com/azure/cognitive-services/encryption/cognitive-services-encryption-keys-portal)

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: zákazník

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: protokolovat a upozornit na změny kritických prostředků Azure

**Doprovodné**materiály: pomocí Azure monitor s protokolem aktivit Azure můžete vytvářet výstrahy pro případy, kdy změny probíhají v produkčních instancích Azure Cognitive Services a dalších důležitých nebo souvisejících prostředcích.

* [Vytvoření upozornění pro události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="vulnerability-management"></a>Správa ohrožení zabezpečení

*Další informace najdete v tématu [řízení zabezpečení: Správa ohrožení](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)zabezpečení.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: spuštění automatizovaných nástrojů pro kontrolu ohrožení zabezpečení

**Pokyny**: Microsoft provádí správu ohrožení zabezpečení v základních systémech, které podporují Azure Cognitive Services.

**Monitorování Azure Security Center**: aktuálně není k dispozici.

**Zodpovědnost**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: nasazení automatizovaného řešení pro správu oprav operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: nasazení automatizovaného řešení pro správu oprav pro softwarové tituly třetích stran

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: porovnání kontrol zabezpečení back-to-back

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: použijte proces hodnocení rizik k určení priorit nápravy zjištěných ohrožení zabezpečení

**Pokyny**: Microsoft provádí správu ohrožení zabezpečení v základních systémech, které podporují Azure Cognitive Services.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: Microsoft

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

**Doprovodné**materiály: Používejte označení, skupiny pro správu a samostatné odběry, pokud je to vhodné, k organizování a sledování mezipaměti Azure pro instance Redis a související prostředky. Proveďte pravidelné sjednocení inventáře a zajistěte si včas odstranění neautorizovaných prostředků z předplatného.

Kromě toho použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v předplatných zákazníka pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

* [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Vytvoření skupin pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: definování a údržba inventáře schválených prostředků Azure

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky a Azure jako celek.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitorování neschválených prostředků Azure

**Doprovodné**materiály: použijte Azure Policy k omezení typu prostředků, které se dají vytvořit v zákaznických předplatných, pomocí následujících integrovaných definic zásad:
- Nepovolené typy prostředků
- Povolené typy prostředků

Kromě toho použijte Azure Resource Graph k dotazování nebo zjišťování prostředků v rámci předplatných.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak vytvářet dotazy pomocí Azure Resource graphu](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: monitorujte neschválené softwarové aplikace v rámci výpočetních prostředků.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: Odeberte neschválené prostředky Azure a softwarové aplikace

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky a Azure jako celek.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="68-use-only-approved-applications"></a>6,8: Používejte pouze schválené aplikace.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

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

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: Omezte schopnost uživatelů pracovat s Azure Resource Manager

**Pokyny**: Nakonfigurujte podmíněný přístup Azure tak, aby uživatelé mohli komunikovat s Azure Resource Manager konfigurací možnosti blokovat přístup pro aplikaci Microsoft Azure Management.

* [Postup konfigurace podmíněného přístupu pro blokování přístupu k Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: Omezte schopnost uživatelů spouštět skripty ve výpočetních prostředcích.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: fyzicky nebo logicky oddělené aplikace s vysokým rizikem

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro webové aplikace běžící na Azure App Service nebo výpočetních prostředcích.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

## <a name="secure-configuration"></a>Zabezpečená konfigurace

*Další informace najdete v tématu [řízení zabezpečení: zabezpečená konfigurace](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: Vytvoření zabezpečených konfigurací pro všechny prostředky Azure

**Pokyny**: definování a implementace standardních konfigurací zabezpečení pro váš kontejner Azure Cognitive Services pomocí Azure Policy. Pomocí aliasů Azure Policy v oboru názvů Microsoft. Cognitiveservices Account můžete vytvářet vlastní zásady pro auditování nebo prosazování konfigurace mezipaměti Azure pro instance Redis.

* [Jak zobrazit dostupné aliasy Azure Policy](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: Vytvoření zabezpečených konfigurací operačního systému

**Doprovodné**materiály: nepoužitelné; Tyto zásady jsou určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: udržování zabezpečených konfigurací prostředků Azure

**Doprovodné**materiály: použijte Azure Policy [Deny] a [Deploy, pokud neexistuje] pro vymáhání zabezpečených nastavení napříč prostředky Azure.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Pochopení Azure Policych efektů](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: udržování zabezpečených konfigurací operačního systému

**Doprovodné**materiály: nepoužitelné; Tyto zásady jsou určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Konfigurace prostředků Azure v zabezpečeném úložišti

**Pokyny**: Pokud používáte vlastní definice Azure Policy nebo šablony Azure Resource Manager pro kontejnery Cognitive Services Azure a související prostředky, použijte Azure Repos k bezpečnému ukládání a správě kódu.

* [Jak v Azure DevOps ukládat kód](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentace k Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: bezpečné uložení vlastních imagí operačního systému

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: nasazení nástrojů pro správu konfigurace pro prostředky Azure

**Pokyny**: použijte aliasy Azure Policy v oboru názvů Microsoft. cache k vytvoření vlastních zásad pro upozornění, audit a prosazování konfigurace systému. Dále můžete vyvinout proces a kanál pro správu výjimek zásad.

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: nasazení nástrojů pro správu konfigurace pro operační systémy

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Implementujte automatizované monitorování konfigurace pro prostředky Azure.

**Pokyny**: pomocí aliasů Azure Policy v oboru názvů Microsoft. cognitiveservices Account můžete vytvořit vlastní definice Azure Policy pro upozornění, audit a vymáhání systémových konfigurací. K automatickému vymáhání konfigurace mezipaměti Azure pro instance Redis a související prostředky použijte Azure Policy [audit], [Deny] a [nasazení, pokud neexistuje].

* [Konfigurace a Správa Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Implementujte automatizované monitorování konfigurace pro operační systémy

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="711-manage-azure-secrets-securely"></a>7,11: zabezpečená Správa tajných kódů Azure

**Doprovodné**materiály: u virtuálních počítačů Azure nebo webových aplikací, které běží na Azure App Service používají pro přístup k rozhraní Azure Cognitive Services API, použijte identita spravované služby ve spojení s Azure Key Vault ke zjednodušení a zabezpečení správy klíčů Azure Cognitive Services. Ujistěte se, že je povolené Key Vault obnovitelné odstranění.

* [Integrace se spravovanými identitami Azure](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

* [Vytvoření Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal)

* [Jak zajistit Key Vault ověřování pomocí spravované identity](https://docs.microsoft.com/azure/key-vault/managed-identity)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: bezpečně a automaticky spravujte identity

**Doprovodné**materiály: u virtuálních počítačů Azure nebo webových aplikací, které běží na Azure App Service používají pro přístup k rozhraní Azure Cognitive Services API, použijte identita spravované služby ve spojení s Azure Key Vault ke zjednodušení a zabezpečení správy klíčů Azure Cognitive Services. Ujistěte se, že je povolené Key Vault obnovitelné odstranění.

Spravované identity použijte k poskytování služeb Azure s automaticky spravovanou identitou v Azure Active Directory. Spravované identity vám umožňují ověřit jakoukoli službu, která podporuje ověřování Azure AD, včetně Azure Key Vault bez jakýchkoli přihlašovacích údajů ve vašem kódu.

* [Postup konfigurace spravovaných identit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

* [Integrace se spravovanými identitami Azure](https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: Eliminujte nezamýšlenou expozici přihlašovacích údajů

**Pokyny**: implementace skeneru přihlašovacích údajů pro identifikaci přihlašovacích údajů v rámci kódu. Skener přihlašovacích údajů taky bude povzbudit přesunutí zjištěných přihlašovacích údajů do bezpečnějších umístění, jako je Azure Key Vault.

* [Jak nastavit skener přihlašovacích údajů](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

## <a name="malware-defense"></a>Obrana před malwarem

*Další informace najdete v tématu [řízení zabezpečení: obrana proti malwaru](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: použití centrálně spravovaného malwarového softwaru

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

Microsoft Anti-malware je povolený na podkladovém hostiteli, který podporuje služby Azure (například Azure Cognitive Services), ale neběží na zákaznickém obsahu.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: předběžná kontrola souborů, které se mají nahrát do prostředků Azure, které nejsou COMPUTE

**Pokyny**: ochrana proti malwaru od Microsoftu je povolená na podkladovém hostiteli, který podporuje služby Azure (například mezipaměť Azure pro Redis), ale neběží na zákaznickém obsahu.

Předem prohledejte veškerý obsah, který se nahrává do nevýpočetních prostředků Azure, jako jsou App Service, Data Lake Storage, Blob Storage, Azure Database for PostgreSQL atd. Společnost Microsoft nemá přístup k vašim datům v těchto instancích.

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: Ujistěte se, že antimalwarový software a signatury jsou aktualizované.

**Doprovodné**materiály: nepoužitelné; Toto doporučení je určené pro výpočetní prostředky.

Microsoft Anti-malware je povolený na podkladovém hostiteli, který podporuje služby Azure (například Azure Cognitive Services), ale neběží na zákaznickém obsahu.

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: netýká se

## <a name="data-recovery"></a>Obnovení dat

*Další informace najdete v tématu [řízení zabezpečení – obnovení dat](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: zajištění pravidelného automatického zálohování

**Doprovodné**materiály: data v Microsoft azurem účtu úložiště se vždycky automaticky replikují, aby se zajistila odolnost a vysoká dostupnost. Azure Storage kopíruje vaše data, aby byla chráněná před plánovanými a neplánovanými událostmi, včetně přechodných selhání hardwaru, sítě nebo výpadků výpadků a obrovských přirozených havárií. Můžete se rozhodnout, že budete replikovat data ve stejném datovém centru, napříč datovými centry oblastí v rámci stejné oblasti nebo v geograficky oddělených oblastech.

Funkci správy životního cyklu můžete také použít k zálohování dat do archivní úrovně. Navíc můžete povolit obnovitelné odstranění záloh uložených v účtu úložiště.

* [Principy Azure Storage a smlouvy o úrovni služeb](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

* [Správa životního cyklu úložiště objektů blob v Azure](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)

* [Obnovitelné odstranění objektů blob služby Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: proveďte kompletní systémové zálohy a zálohujte všechny klíče spravované zákazníkem.

**Pokyny**: k nasazení Cognitive Services a souvisejících prostředků použijte Azure Resource Manager. Azure Resource Manager poskytuje možnost exportovat šablony, což vám umožní znovu nasadit řešení v celém životním cyklu vývoje a mít jistotu, že se prostředky nasadí v konzistentním stavu. Použijte Azure Automation k pravidelnému volání rozhraní API pro export Azure Resource Manager šablony. Zálohujte předem sdílené klíče v rámci Azure Key Vault.

* [Přehled Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)

* [Postup vytvoření prostředku Cognitive Services pomocí šablony Azure Resource Manager](https://docs.microsoft.com/azure/cognitive-services/resource-manager-template?tabs=portal)

* [Export jednoho a více prostředků do šablony v Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)

* [Skupiny prostředků – Exportovat šablonu](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate)

* [Úvod do Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro)

* [Postup zálohování klíčů trezoru klíčů v Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: ověření všech záloh včetně klíčů spravovaných zákazníkem

**Doprovodné**materiály: Zajistěte, aby v případě potřeby pravidelně prováděly nasazení Azure Resource Manager šablon pro izolované předplatné do izolovaného předplatného. Test obnovení zálohovaných předsdílených klíčů.

* [Nasazení prostředků pomocí šablon ARM a Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal)

* [Postup obnovení klíčů trezoru klíčů v Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Monitorování Azure Security Center**: nelze použít

**Zodpovědnost**: zákazník

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: Zajistěte ochranu záloh a klíčů spravovaných zákazníkem

**Pokyny**: pomocí Azure DevOps bezpečně ukládejte a spravujte šablony Azure Resource Manager. K ochraně prostředků, které spravujete v Azure DevOps, můžete udělit nebo odepřít oprávnění konkrétním uživatelům, vestavěným skupinám zabezpečení nebo skupinám definovaným v Azure Active Directory (Azure AD), pokud jsou integrované s Azure DevOps, nebo Active Directory, pokud je integrovaná se sadou TFS.  Použijte řízení přístupu na základě rolí k ochraně zákaznických klíčů. Povolení ochrany před náhodným odstraněním a vyprázdněním v Key Vault k ochraně klíčů proti náhodnému nebo škodlivému odstranění. 

* [Jak v Azure DevOps ukládat kód](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [O oprávněních a skupinách v Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions)

* [Jak povolit ochranu s možnostmi obnovitelného odstranění a vyprázdnění v Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Monitorování Azure Security Center**: Ano

**Zodpovědnost**: zákazník

## <a name="incident-response"></a>Reakce na incidenty

*Další informace najdete v tématu [řízení zabezpečení: reakce na incidenty](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: Vytvoření Průvodce odpověďmi na incidenty

**Pokyny**: Vytvoření Průvodce odpověďmi na incidenty pro vaši organizaci. Zajistěte, aby existovaly písemné plány odpovědí na incidenty, které definují všechny role pracovníků, a také fáze zpracování nebo správy incidentů z detekce až po přezkoumání po jednotlivých událostech.

* [Postup konfigurace automatizace pracovních postupů v rámci služby Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Pokyny k vytvoření vlastního procesu reakce na incidenty zabezpečení](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Anatomie centra Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [K podpoře při vytváření vlastního plánu odpovědí na incidenty můžete také využít příručku pro zpracování incidentů v počítači s NIST.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

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

**Pokyny**: * [použijte pravidla zapojení Microsoftu a ujistěte se, že testy průniku nejsou v rozporu s zásadami Microsoftu](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) .

* [V této části najdete další informace o strategii Microsoftu a provádění testování v rámci červeného seskupování a testování průniku na cloudové infrastruktuře, služby a aplikace spravované Microsoftem.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitorování Azure Security Center**: nelze použít

**Odpovědnost**: sdílená

## <a name="next-steps"></a>Další kroky

- Zobrazit [Srovnávací test zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Další informace o [plánech zabezpečení Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
