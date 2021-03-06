---
title: Automatické škálování a zónově redundantní služby Application Gateway v2
description: V tomto článku se seznámíte se službou Azure Application Standard_v2 a WAF_v2 skladovou jednotkou, která zahrnuje automatické škálování a funkce redundantní v zóně.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 06/06/2020
ms.author: victorh
ms.custom: fasttrack-edit, references_regions
ms.openlocfilehash: f10bb1f4065f3bdb517fcad4f3eb6caa331c5233
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87273197"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Automatické škálování a zónově redundantní služby Application Gateway v2 

Application Gateway je k dispozici v rámci Standard_v2 SKU. Firewall webových aplikací (WAF) je k dispozici v rámci WAF_v2 SKU. SKU v2 nabízí vylepšení výkonu a přidává podporu pro důležité nové funkce, jako je automatické škálování, redundance zóny a podpora statických virtuálních IP adres. Stávající funkce v rámci Standard a WAF SKU se budou dál podporovat v nové SKU verze V2 a s několika výjimkami uvedenými v části [porovnání](#differences-from-v1-sku) .

Nová SKU v2 obsahuje následující vylepšení:

- Automatické **škálování**: Application Gateway nebo WAF nasazení v rámci SKU automatického škálování se dá škálovat nahoru nebo dolů na základě změny schémat zatížení provozu. Automatické škálování také eliminuje nutnost zvolit během zřizování velikost nasazení nebo počet instancí. Tato SKU nabízí skutečnou pružnost. V Standard_v2 a WAF_v2 SKU může Application Gateway fungovat v pevné kapacitě (automatické škálování je zakázané) a v režimu s povoleným automatickým škálováním. Režim pevné kapacity je vhodný pro scénáře s konzistentními a předvídatelnými úlohami. Režim automatického škálování je užitečný v aplikacích, které se zobrazují jako odchylka v provozu aplikací.
- **Redundance zóny**: nasazení Application Gateway nebo WAF může zahrnovat více zóny dostupnosti a odstraňuje nutnost zřídit v každé zóně samostatné instance Application Gateway Traffic Manager. Můžete zvolit jednu zónu nebo několik zón, ve kterých se Application Gateway instance nasazují, což zjednodušuje selhání zóny. Back-end fond pro aplikace se dá v rámci zón dostupnosti podobně distribuovat.

  Redundance zóny je dostupná jenom v případě, že jsou dostupné zóny Azure. V ostatních oblastech jsou všechny ostatní funkce podporované. Další informace najdete v tématech [oblasti a zóny dostupnosti v Azure](../availability-zones/az-overview.md) .
- **Statická virtuální IP adresa**: Application Gateway v2 SKU podporuje výhradně typ VIP typu static. Tím se zajistí, že se virtuální IP adresa přidružená k aplikační bráně nemění pro životní cyklus nasazení, ani po restartování.  V v1 není statická virtuální IP adresa, proto musíte místo IP adresy pro směrování názvu domény použít adresu URL služby Application Gateway, aby bylo možné App Services prostřednictvím aplikační brány.
- **Přepisování hlaviček**: Application Gateway umožňuje přidávat, odebírat nebo aktualizovat žádosti HTTP a hlavičky odpovědí s SKU v2. Další informace najdete v tématu [přepis hlaviček protokolu HTTP pomocí Application Gateway](rewrite-http-headers.md)
- **Key Vault Integration**: Application Gateway v2 podporuje integraci s Key Vault pro serverové certifikáty, které jsou připojené k naslouchacím procesům s POVOLENým protokolem HTTPS. Další informace najdete v tématu [ukončení protokolu TLS s certifikáty Key Vault](key-vault-certs.md).
- Kontroler příchozího přenosu dat **služby Azure Kubernetes**: kontroler příchozího přenosu Application Gateway v2 umožňuje použití Application Gateway Azure jako příchozího přenosu pro službu Azure KUBERNETES (AKS), která se označuje jako cluster AKS. Další informace najdete v tématu [co je Application Gateway kontroler](ingress-controller-overview.md)příchozího přenosu dat?.
- **Vylepšení výkonu**: SKU v2 nabízí až pětinásobné vyšší výkon při snižování zátěže TLS ve srovnání s SKU Standard/WAF.
- **Rychlejší nasazení a čas aktualizace** SKU verze 2 poskytuje rychlejší nasazení a dobu aktualizace ve srovnání s SKU Standard/WAF. To zahrnuje také změny konfigurace WAF.

![Diagram zóny automatického škálování](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Podporované oblasti

SKU Standard_v2 a WAF_v2 jsou k dispozici v následujících oblastech: Střed USA – sever, Střed USA – jih, Západní USA, Západní USA 2, Východní USA, Východní USA 2, Střed USA, Severní Evropa, Západní Evropa, jihovýchodní Asie, Francie – střed, Velká Británie – západ, Japonsko – východ, Japonsko – západ, Austrálie – jihovýchod, Brazílie – jih, Kanada – střed, Kanada – jih, východní Austrálie, Jižní Korea Velká Británie – jih, Střed Indie, Západní Indie Jižní Indie.

## <a name="pricing"></a>Ceny

V případě SKU verze v2 je cenový model založený na spotřebě a již není připojen k počtům nebo velikostem instancí. Ceny SKU v2 mají dvě komponenty:

- **Pevná cena** – je to hodinová (nebo částečná hodinová) cena za účelem zřízení Standard_v2 nebo WAF_v2 brány. Počítejte s tím, že 0 dalších minimálních instancí stále zajišťuje vysokou dostupnost služby, která je vždycky zahrnutá do pevné ceny.
- **Jednotková cena kapacity** – jedná se o náklady založené na spotřebě, které se účtují i s pevnými náklady. Poplatky za jednotku se vypočítávají také každou hodinu nebo částečně. Existují tři dimenze kapacity jednotek a výpočetní jednotky, trvalá připojení a propustnost. Výpočetní jednotka je míra spotřebované kapacity procesoru. Faktory ovlivňujícími výpočetní jednotku jsou počet připojení TLS za sekundu, výpočty přepisu adres URL a zpracování pravidel WAF. Trvalé připojení je míra navázaných připojení TCP k aplikační bráně v daném fakturačním intervalu. Propustnost je průměr MB/s zpracováno systémem v daném fakturačním intervalu.  Fakturace se provádí na úrovni kapacitní jednotky pro cokoli nad rezervovaným počtem instancí.

Každá jednotka kapacity se skládá z maximálně: 1 výpočetní jednotka, 2500 trvalá připojení a propustnost 2,22-MB/s.

Doprovodné materiály k výpočetním jednotkám:

- **Standard_v2** – pro každý výpočetní jednotku je možné, že certifikát TLS s klíčem RSA 2048 je schopný přibližně 50 připojení za sekundu.
- **WAF_v2** – každá výpočetní jednotka může podporovat přibližně 10 souběžných požadavků za sekundu po 70-30% kombinace provozu s 70% požadavky menší než 2 KB pro získání/odeslání a zbývající vyšší. WAF výkon aktuálně neovlivní velikost odpovědí.

> [!NOTE]
> Každá instance teď může podporovat přibližně 10 kapacitních jednotek.
> Počet požadavků, které může výpočetní jednotka zpracovat, závisí na různých kritériích velikosti klíče certifikátu TLS, algoritmu výměny klíčů, přepisování hlaviček a v případě příchozí velikosti požadavku WAF. Doporučujeme, abyste provedli testy aplikací k určení míry požadavků na výpočetní jednotku. Jednotka kapacity i výpočetní jednotka budou k dispozici jako metrika před zahájením fakturace.

V následující tabulce jsou uvedeny ukázkové ceny a jsou pouze pro ilustrativní účely.

**Ceny v USA – východ**:

|              Název SKU                             | Pevná cena ($/h)  | Jednotková cena kapacity ($/CU-hr)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0,20             | 0,0080                          |
| WAF_v2                                            |    0.36             | 0,0144                          |

Další informace o cenách najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/application-gateway/). 

**Příklad 1**

Standard_v2 Application Gateway se zřídí bez automatického škálování v režimu ručního škálování s pevnou kapacitou pět instancí.

Pevná cena = 744 (hodiny) × $0,20 = $148,8 <br>
Jednotky kapacity = 744 (hodiny) * 10 jednotek kapacity na instanci * 5 instancí * $0,008 na kapacitu jednotky = $297,6

Total Price = $148,8 + $297,6 = $446,4

**Příklad 2**

Application Gateway standard_v2 se zřídí za měsíc, s nulovými minimálními instancemi a během této doby obdrží 25 nových připojení TLS/s, průměr 8,88 – MB/s a přenos dat. Za předpokladu, že připojení jsou krátkodobá, vaše cena by byla:

Pevná cena = 744 (hodiny) × $0,20 = $148,8

Jednotková cena kapacity = 744 (hodiny) * Max (25/50 výpočetních jednotek pro připojení/s, jednotka kapacity 8.88/2.22 pro propustnost) * $0,008 = 744 × 4 * 0,008 = $23,81

Total Price = $148.8 + 23.81 = $172,61

Jak vidíte, účtují se jenom čtyři jednotky kapacity, ne celá instance. 

> [!NOTE]
> Funkce Max vrátí největší hodnotu ve dvojici hodnot.


**Příklad 3**

Standard_v2 Application Gateway je zřízena po měsících s minimálně pěti instancemi. Za předpokladu, že nedochází k žádným přenosům dat a připojení, bude vaše cena:

Pevná cena = 744 (hodiny) × $0,20 = $148,8

Jednotková cena kapacity = 744 (hodiny) * Max (0/50 výpočetních jednotek pro připojení/s, 0/2.22 jednotka kapacity pro propustnost) * $0,008 = 744 * 50 * 0,008 = $297,60

Total Price = $148.80 + 297.60 = $446,4

V takovém případě se vám účtuje celé pět instancí, i když nedochází k provozu.

**Příklad 4**

Standard_v2 Application Gateway se zřídí po měsících, minimálně v pěti instancích, ale v tuto chvíli je k dispozici průměr přenosu dat 125 MB/s a 25 připojení TLS za sekundu. Za předpokladu, že nedochází k žádným přenosům dat a připojení, bude vaše cena:

Pevná cena = 744 (hodiny) × $0,20 = $148,8

Jednotková cena kapacity = 744 (hodiny) * Max (25/50 výpočetních jednotek pro připojení/s, 125/2.22 jednotka kapacity pro propustnost) * $0,008 = 744 * 57 * 0,008 = $339,26

Total Price = $148.80 + 339.26 = $488,06

V takovém případě se vám bude účtovat celá pět instancí a navíc sedm jednotek kapacity (což je 7/10 instance).  

**Příklad 5**

WAF_v2 Application Gateway zřídit po dobu měsíce. Během této doby obdrží 25 nových připojení TLS za sekundu, průměrně 8,88 MB/s přenos dat a požadavek 80 za sekundu. Za předpokladu, že připojení jsou krátkodobá a že výpočet výpočetní jednotky pro aplikaci podporuje 10 RPS na výpočetní jednotku, vaše cena by byla:

Pevná cena = 744 (hodiny) × $0,36 = $267,84

Jednotková cena kapacity = 744 (hodiny) * Max (maximální výpočetní jednotka (25/50 za připojení/s, 80/10 WAF RPS), 8.88/2.22 kapacita jednotky pro propustnost) * $0,0144 = 744 × 8 × 0,0144 = $85,71

Total Price = $267,84 + $85,71 = $353,55

> [!NOTE]
> Funkce Max vrátí největší hodnotu ve dvojici hodnot.

## <a name="scaling-application-gateway-and-waf-v2"></a>Škálování Application Gateway a WAF v2

Application Gateway a WAF se dají nakonfigurovat tak, aby se škálují ve dvou režimech:

- Automatické **škálování** – s povoleným automatickým škálováním jsou SKU Application Gateway a WAF v2 vertikálně navyšovat nebo snížit zatížení na základě požadavků na provoz aplikací. Tento režim nabízí lepší pružnost vaší aplikace a eliminuje nutnost odhadnout velikost nebo počet instancí aplikační brány. Tento režim také umožňuje ušetřit náklady tím, že nevyžaduje, aby brána běžela ve špičce zřízené kapacity pro očekávané maximální zatížení provozu. Je nutné zadat minimální a volitelně maximální počet instancí. Minimální kapacita zajišťuje, že Application Gateway a WAF v2 nedosahují pod stanoveným minimálním počtem instancí, a to i v případě absence provozu. Každá instance je zhruba ekvivalentní 10 dalších rezervovaných jednotek kapacity. Nula znamená žádnou rezervovanou kapacitu a je čistě automatické škálování v podstatě. Volitelně můžete zadat také maximální počet instancí, který zajistí, že Application Gateway nepřekračuje zadaný počet instancí. Bude se vám účtovat jenom objem provozu obsluhované bránou. Počty instancí můžou být v rozsahu od 0 do 125. Výchozí hodnota pro maximální počet instancí je 20, pokud není zadána.
- **Ručně** – můžete také zvolit ruční režim, ve kterém brána neprovede automatické škálování. Pokud je v tomto režimu více provozu, než jaké Application Gateway nebo WAF může zvládnout, mohlo by dojít ke ztrátě provozu. V případě ručního režimu je zadání počtu instancí povinné. Počet instancí se může v rozmezí od 1 do 125 instancí lišit.

## <a name="autoscaling-and-high-availability"></a>Automatické škálování a vysoká dostupnost

Aplikační brány Azure se vždycky nasazují vysoce dostupným způsobem. Služba je tvořena několika instancemi, které jsou vytvořeny jako nakonfigurované (Pokud automatické škálování je zakázané) nebo vyžadované zatížením aplikace (Pokud je povolené automatické škálování). Všimněte si, že z perspektivy uživatele nebudete nutně mít přehled o jednotlivých instancích, ale jenom do služby Application Gateway jako celek. Pokud dojde k potížím s určitou instancí a přestane fungovat, Azure Application Gateway transparentně vytvoří novou instanci.

Pamatujte na to, že i když konfigurujete automatické škálování s nulovými minimálními instancemi, bude služba stále dostupná, což je vždycky zahrnuté do pevné ceny.

Vytvoření nové instance může ale nějakou dobu trvat (přibližně po šesti nebo sedmi minutách). Proto pokud se nechcete vypořádat s tímto výpadkem, můžete nakonfigurovat minimální počet instancí 2, v ideálním případě s podporou zóny dostupnosti. Tímto způsobem budete mít v Azure Application Gateway za normálních okolností aspoň dvě instance, takže pokud jedna z nich nastala nějaký problém, při vytváření nové instance se v případě jednoho z nich pokusí s přenosy vypořádat. Všimněte si, že instance služby Azure Application Gateway může podporovat zhruba 10 kapacitních jednotek, takže v závislosti na tom, kolik přenosů obvykle máte, možná budete chtít nakonfigurovat nastavení automatického škálování minimální instance na hodnotu vyšší než 2.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>Porovnání funkcí mezi SKU V1 a SKU v2

Následující tabulka porovnává funkce, které jsou k dispozici u jednotlivých SKU.

| Funkce                                           | SKU v1   | V2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Automatické škálování                                       |          | &#x2713; |
| Zónová redundance                                   |          | &#x2713; |
| Statická virtuální IP adresa                                        |          | &#x2713; |
| Řadič příchozího přenosu dat (AKS) Azure Kubernetes Service () |          | &#x2713; |
| Integrace se službou Azure Key Vault                       |          | &#x2713; |
| Přepsat hlavičky HTTP (S)                           |          | &#x2713; |
| Směrování na základě adresy URL                                 | &#x2713; | &#x2713; |
| Hostování několika webů                             | &#x2713; | &#x2713; |
| Přesměrování provozu                               | &#x2713; | &#x2713; |
| Firewall webových aplikací (WAF)                    | &#x2713; | &#x2713; |
| Vlastní pravidla WAF                                  |          | &#x2713; |
| Ukončení protokolu TLS (Transport Layer Security)/Secure Sockets Layer (SSL)            | &#x2713; | &#x2713; |
| Komplexní šifrování TLS                         | &#x2713; | &#x2713; |
| Spřažení relací                                  | &#x2713; | &#x2713; |
| Vlastní chybové stránky                                | &#x2713; | &#x2713; |
| Podpora protokolu WebSocket                                 | &#x2713; | &#x2713; |
| Podpora HTTP/2                                    | &#x2713; | &#x2713; |
| Vyprázdnění připojení                               | &#x2713; | &#x2713; |

> [!NOTE]
> SKU autoškále v2 teď podporuje [výchozí sondy stavu](application-gateway-probe-overview.md#default-health-probe) , aby automaticky sledovaly stav všech prostředků v jeho fondu back-endu, a zvýrazňuje tyto členy, které se považují za chybné. Výchozí sonda stavu je automaticky nakonfigurovaná pro back-endy, které nemají žádnou vlastní konfiguraci sondy. Další informace najdete v tématu [sondy stavu ve služby Application Gateway](application-gateway-probe-overview.md).

## <a name="differences-from-v1-sku"></a>Rozdíly z SKU v1

Tato část popisuje funkce a omezení skladové položky v2, která se liší od SKU v1.

|Rozdíl|Podrobnosti|
|--|--|
|Ověřovací certifikát|Nepodporováno<br>Další informace najdete v tématu [Přehled koncového protokolu TLS s Application Gateway](ssl-overview.md#end-to-end-tls-with-the-v2-sku).|
|Kombinování Standard_v2 a standardních Application Gateway ve stejné podsíti|Nepodporováno|
|Trasa definovaná uživatelem (UDR) na Application Gateway podsíti|Podporované (konkrétní scénáře) Ve verzi Preview.<br> Další informace o podporovaných scénářích najdete v tématu [Přehled konfigurace Application Gateway](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet).|
|NSG pro rozsah portů pro příchozí spojení| -65200 až 65535 pro Standard_v2 SKU<br>-65503 až 65534 pro SKU Standard.<br>Další informace najdete v [nejčastějších dotazech](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Protokoly výkonu v diagnostice Azure|Nepodporováno<br>Měly by se používat metriky Azure.|
|Fakturace|Fakturace naplánovala zahájení od 1. července 2019.|
|Režim FIPS|V tuto chvíli se nepodporují.|
|Jenom interního nástroje režim|To se v tuto chvíli nepodporuje. Podporují se veřejné a interního nástroje režimy.|
|Integrace se službou NET Watch|Nepodporováno|
|Integrace služby Azure Security Center|Ještě není k dispozici.

## <a name="migrate-from-v1-to-v2"></a>Migrace z v1 na v2

Skript Azure PowerShell je k dispozici v galerii prostředí PowerShell, který vám může při migraci z verze V1 Application Gateway/WAF do SKU automatického škálování v2. Tento skript vám pomůže zkopírovat konfiguraci z vaší brány v1. Migrace provozu je stále vaše zodpovědnost. Další informace najdete v tématu [migrace Azure Application Gateway z verze V1 na verzi 2](migrate-v1-v2.md).

## <a name="next-steps"></a>Další kroky

- [Rychlý start: Směrování webového provozu pomocí služby Azure Application Gateway – Azure Portal](quick-create-portal.md)
- [Vytvoření automatického škálování, zóny redundantní aplikační brány s vyhrazenou virtuální IP adresou pomocí Azure PowerShell](tutorial-autoscale-ps.md)
- Přečtěte si další informace o [Application Gateway](overview.md).
- Přečtěte si další informace o [Azure firewall](../firewall/overview.md).
