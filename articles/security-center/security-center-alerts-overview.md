---
title: Výstrahy zabezpečení v Azure Security Center | Microsoft Docs
description: Toto téma vysvětluje, jaké jsou výstrahy zabezpečení, a různé typy, které jsou k dispozici v Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: memildin
ms.openlocfilehash: 25b5569c123a4c7376aecd01b8e36f68b0a8e9b5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519603"
---
# <a name="security-alerts-in-azure-security-center"></a>Výstrahy zabezpečení ve službě Azure Security Center

V Azure Security Center existuje celá řada výstrah pro mnoho různých typů prostředků. Security Center generuje výstrahy pro prostředky nasazené v Azure a také pro prostředky nasazené v místních i hybridních cloudových prostředích.

Výstrahy zabezpečení se spouštějí pokročilými detekcemi a jsou dostupné jenom na úrovni Standard Azure Security Center. K dispozici je bezplatná zkušební verze. Můžete upgradovat ze stránky **Nastavení cenové &** . Přečtěte si další informace o [cenách Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="responding-to-todays-threats"></a>Reakce na dnešní hrozby <a name="respond-threats"> </a>

Za posledních 20 let došlo ve světě hrozeb k významným změnám. V minulosti se společnosti obvykle musely zabývat pouze tím, že se týká jenom individuálních útočníků, kteří byli většinou zájem, aby viděli "co můžou dělat". Dnešní útočníci jsou mnohem sofistikovanější a organizované. Často mají konkrétní finanční a strategické cíle. Mají také k dispozici větší zdroje, protože mohou být financováni státy nebo organizovanou trestnou činností.

Tyto změny v realitách vedly k navýšení nebývalého profesionálního postavení v útočníkovi. Již je nezajímá pouhé poškození vzhledu webu. Nyní mají zájem ukrást informace, finanční účty a soukromá data – to vše, co můžou využít ke generování hotovosti na otevřeném trhu nebo k využití konkrétní obchodní, politické nebo vojenské pozice. Ještě znepokojivější než útočníci s finančními cíli jsou útočníci, kteří pronikají do sítí s cílem poškodit infrastrukturu a lidi.

V reakci na tyto hrozby společnosti často nasazují řadu jednotlivých řešení, která se zaměřují na ochranu firemní zóny nebo na koncové body tím, že hledají příznaky známých útoků. Tato řešení často generují velký objem málo specifických výstrah, které pak musí bezpečnostní analytik třídit a prošetřovat. Většina organizací nemá dostatek času a potřebné odborné znalosti, aby si s těmito výstrahami poradily, a mnoho z nich tak zůstane neprošetřených.  

Kromě toho útočníci vyvinuli své metody pro přepočet obrany na základě podpisu a [přizpůsobení cloudovým prostředím](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Je nutné zavést nové přístupy za účelem rychlejší identifikace nově se vynořujících hrozeb a urychlení detekce a reakce na ně.

## <a name="what-are-security-alerts-and-security-incidents"></a>Co jsou výstrahy zabezpečení a bezpečnostní incidenty? 

**Výstrahy** jsou oznámení, která Security Center generují při detekci hrozeb na vašich prostředcích. Security Center určí prioritu a seznam výstrah spolu s informacemi potřebnými k rychlému prozkoumání problému. Security Center také poskytuje doporučení pro nápravu útoku.

**Incident zabezpečení** je kolekce souvisejících výstrah, místo aby se jednotlivé výstrahy zobrazovaly jednotlivě. Security Center používá [korelaci cloudových inteligentních výstrah](security-center-alerts-cloud-smart.md) ke korelaci různých výstrah a signálů s nízkou věrností do incidentů zabezpečení.

Pomocí incidentů vám Security Center poskytuje jediné zobrazení kampaně útoku a všech souvisejících výstrah. Toto zobrazení vám umožní rychle pochopit, jaké akce útočníka trvala, a jaké prostředky byly ovlivněny. Další informace najdete v tématu [korelace cloudové inteligentní výstrahy](security-center-alerts-cloud-smart.md).



## <a name="how-does-security-center-detect-threats"></a>Jak Security Center detekuje hrozby? <a name="detect-threats"> </a>

Výzkumníci v oblasti zabezpečení ze společnosti Microsoft neustále vyhledávají nové hrozby. Z důvodu globální přítomnosti Microsoftu v cloudu i v místním prostředí mají přístup k obsáhlém sadě telemetrie. Souběžná a různorodá kolekce datových sad umožňuje zjistit nové vzory útoků a trendy v rámci svých místních zákaznických a podnikových produktů a také jejich online služby. Díky tomu dokáže Security Center rychle aktualizovat své algoritmy detekce spolu s tím, jak útočníci provádějí nové a stále sofistikovanější kousky. Tento přístup pomáhá udržet krok s rychle se rozvíjejícím prostředím hrozeb.

Security Center shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure a sítě, aby zjistila skutečné hrozby a snížila falešně pozitivní výsledky. Funguje taky s připojenými partnerskými řešeními, jako jsou brány firewall a řešení ochrany koncových bodů. Security Center analyzuje tyto informace a často koreluje informace z různých zdrojů k identifikaci hrozeb.

![Shromažďování a prezentace dat ve službě Security Center](./media/security-center-alerts-overview/security-center-detection-capabilities.png)

Služba Security Center využívá pokročilou analýzu zabezpečení, která daleko překračuje možnosti detekce založené na signaturách či příznacích. Využívá objevy v oblasti zpracování velkých objemů dat a [strojového učení](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) k vyhodnocování událostí v rámci všech prostředků cloudové infrastruktury – a pomocí manuálních metod a předvídání vývoje útoků detekuje hrozby, které by jinak nebylo možné identifikovat. Do této analýzy zabezpečení patří:

* **Integrovaná analýza hrozeb**: Microsoft má obrovské množství globálních analýz hrozeb. Telemetrie se nachází z různých zdrojů, jako jsou Azure, Microsoft 365, Microsoft CRM Online, Microsoft Dynamics AX, outlook.com, MSN.com, DCU (Microsoft Digital Zločins Unit) a Microsoft Security Response Center (MSRC). Výzkumníki také dostávají informace analýzy hrozeb, které se sdílejí mezi hlavními poskytovateli cloudových služeb a informačními kanály od jiných třetích stran. Služba Azure Security Center vás pomocí těchto informací může upozornit na hrozby známých útočníků.

* **Analýza chování**: analýza chování je technika, která analyzuje a porovnává data s kolekcí známých vzorů. Tato schémata však nepředstavují jednoduché příznaky. Určují se prostřednictvím komplexních algoritmů strojového učení, které se aplikují na rozsáhlé datové sady. Určují se také prostřednictvím pečlivé analýzy škodlivého chování, kterou provádí zkušení analytici. Azure Security Center může pomocí analýzy chování identifikovat ohrožené prostředky na základě analýzy protokolů virtuálních počítačů, protokolů virtuálních síťových zařízení, protokolů prostředků infrastruktury a dalších zdrojů.

* **Detekce anomálií**: Azure Security Center k identifikaci hrozeb používá taky detekci anomálií. Na rozdíl od behaviorální analýzy (která závisí na známých vzorech odvozených z velkých datových sad) je detekce anomálií více "individuálních" a zaměřuje se na směrné plány, které jsou specifické pro vaše nasazení. Pomocí strojového učení se určí běžné úrovně aktivity pro vaše nasazení a poté se vygenerují pravidla definující neobvyklé hodnoty, které by mohly představovat událost zabezpečení.

## <a name="how-are-alerts-classified"></a>Jak jsou klasifikovány výstrahy?

Security Center přiřadí závažnosti výstrahám, které vám pomůžou určit prioritu pořadí, ve kterém se zúčastníte jednotlivých výstrah, takže pokud dojde k ohrožení určitého prostředku, můžete se k němu hned dostat. Závažnost je založena na tom, jak se nachází Security Center ve vyhledávání nebo v analytickém formátu, který vydává výstrahu, a také na úrovni spolehlivosti, u kterých došlo k škodlivému záměru za aktivitu, která vedla k upozornění.

> [!NOTE]
> Závažnost výstrahy se na portálu a ve verzích REST API, které předchází 01-01-2019, zobrazuje jinak. Pokud používáte starší verzi rozhraní API, upgradujte na konzistentní prostředí popsané níže.

- **Vysoká:** Existuje vysoká pravděpodobnost, že dojde k ohrožení zabezpečení prostředku. Měli byste se podívat hned. Security Center má vysokou důvěru v škodlivém záměru i ve zjištěních použitých k vystavení výstrahy. Například výstraha, která detekuje spuštění známého škodlivého nástroje, jako je například Mimikatz, společný nástroj používaný ke krádeži přihlašovacích údajů.
- **Střední:** Je pravděpodobné, že podezřelá aktivita může znamenat, že došlo k ohrožení zabezpečení prostředku.
Při analýze nebo hledání je jistota Security Center střední a důvěra škodlivého záměru je střední až vysoká. Obvykle se jedná o strojové učení nebo detekci na základě anomálií. Například pokus o přihlášení z umístění neobvyklé.
- **Nízká úroveň:** Může se jednat o neškodný pozitivní nebo blokovaný útok.
   * Security Center není dostatečně jistý, že záměr je škodlivý a aktivita může být Innocent. Například vymazání protokolu je akce, ke které může dojít, když se útočník pokusí skrýt jejich stopy, ale v mnoha případech je rutinní operací prováděná správci.
   * Security Center vám většinou neřekne při zablokování útoků, pokud se nejedná o zajímavý případ, který doporučujeme najít. 
- **Informační:** Informační výstrahy se zobrazí jenom v případě, že přejdete k incidentu zabezpečení, nebo pokud použijete REST API s konkrétním ID výstrahy. Incident se většinou skládá z několika výstrah. některé z nich se můžou zobrazovat jenom s informacemi, ale v kontextu dalších výstrah se můžou důvěryhodného o bližším vzhledu. 

## <a name="continuous-monitoring-and-assessments"></a>Průběžné monitorování a posouzení

Azure Security Center výhody z oblasti výzkumu zabezpečení a týmů pro datové vědy v rámci Microsoftu, kteří neustále monitorují změny v rámci hrozby. To zahrnuje následující iniciativy:

* **Monitorování analýzy hrozeb**: Analýza hrozeb zahrnuje mechanismy, ukazatele, dopady a Rady s případnými názory na stávající nebo nově vznikající hrozby. Tyto informace se sdílí v bezpečnostní komunitě a společnost Microsoft kanály analýzy hrozeb z interních i externích zdrojů nepřetržitě monitoruje.
* **Sdílení signálu**: přehledy z bezpečnostních týmů napříč širokou škálou cloudových a místních služeb, serverů a klientských koncových zařízení od Microsoftu se sdílejí a analyzují.
* **Odborníci na zabezpečení společnosti Microsoft**: Průběžné zapojování týmů v rámci společnosti Microsoft, které pracují ve specializovaných oblastech zabezpečení, například forenzní analýza nebo detekce webových útoků.
* **Optimalizace detekce**: V datových sadách reálných zákazníků se spouští algoritmy a výzkumníci z oblasti bezpečnosti pracují se zákazníky na ověřování výsledků. Pravdivě a falešně pozitivní výsledky pak slouží ke zlepšování algoritmů strojového učení.

Toto kombinované úsilí je ukončené novými a vylepšenými detekcemi, které vám umožní využít okamžitě – neexistuje žádná akce, kterou je potřeba provést.


## <a name="next-steps"></a>Další kroky

V tomto článku jste se dozvěděli o různých typech výstrah, které jsou k dispozici v Security Center. Další informace najdete tady:

* [Ochrana před hrozbami v Azure Security Center](threat-protection.md) – stručný popis zdrojů výstrah zabezpečení zobrazených pomocí Azure Security Center 
* **Výstrahy zabezpečení v protokolu aktivit Azure** – navíc k dispozici v Azure Portal nebo programově se výstrahy a incidenty zabezpečení auditují jako události v [protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view). Další informace o schématu událostí najdete v tématu [výstrahy zabezpečení v protokolu aktivit Azure](https://go.microsoft.com/fwlink/?linkid=2114113) .

