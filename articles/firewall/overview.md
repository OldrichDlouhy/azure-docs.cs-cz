---
title: Co je brána Azure Firewall?
description: Azure Firewall je spravovaná cloudová služba síťového zabezpečení, která chrání vaše prostředky ve virtuálních sítích Azure.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 06/18/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 7a5b21551cd549f6a495f6cca7a8c5f96c72ddaa
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081008"
---
# <a name="what-is-azure-firewall"></a>Co je brána Azure Firewall?

![Certifikace ICSA](media/overview/icsa-cert-firewall-small.png)

Azure Firewall je spravovaná cloudová služba síťového zabezpečení, která chrání vaše prostředky ve virtuálních sítích Azure. Jedná se o plně stavovou bránu firewall jako službu s integrovanou vysokou dostupností a neomezenou škálovatelností v cloudu.

![Přehled brány firewall](media/overview/firewall-threat.png)

Můžete centrálně vytvářet, vynucovat a protokolovat zásady připojení k aplikacím a sítím napříč různými předplatnými a virtuálními sítěmi. Brána Azure Firewall používá statickou veřejnou IP adresu pro prostředky virtuální sítě a díky tomu umožňuje venkovním bránám firewall identifikovat provoz pocházející z vaší virtuální sítě.  Služba je plně integrovaná se službou Azure Monitor zajišťující protokolování a analýzy.

## <a name="features"></a>Funkce

Další informace o funkcích Azure Firewall najdete v tématu [Azure firewall funkce](features.md).

## <a name="known-issues"></a>Známé problémy

Brána Azure Firewall má následující známé problémy:

|Problém  |Description  |Omezení rizik  |
|---------|---------|---------|
Pravidla síťového filtrování pro jiné protokoly než TCP/UDP (třeba ICMP) nebudou fungovat pro provoz do internetu.|Pravidla filtrování sítě pro protokoly jiné než TCP/UDP nefungují s SNAT na veřejnou IP adresu. Jiné protokoly než TCP/UDP jsou ale podporované mezi koncovými podsítěmi a virtuálními sítěmi.|Azure Firewall používá vyvažování zatížení úrovně Standard, [které v současnosti nepodporuje SNAT pro protokol IP](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Zkoumáme možnosti podpory tohoto scénáře v budoucí verzi.|
|Chybějící podpora PowerShellu a rozhraní příkazového řádku pro protokol ICMP|Azure PowerShell a CLI v síťových pravidlech nepodporují protokol ICMP jako platný protokol.|Protokol ICMP je stále možné používat prostřednictvím portálu a REST API. Pracujeme na přidání protokolu ICMP v PowerShellu a rozhraní příkazového řádku brzy.|
|Značky plně kvalifikovaného názvu domény vyžadují, aby byl nastavený protokol: port|Pravidla aplikací s značkami plně kvalifikovaného názvu domény vyžadují port: definice protokolu.|Jako hodnotu port: protokol můžete použít **https**. Pracujeme na tom, aby toto pole bylo volitelné při použití značek FQDN.|
|Přesunutí brány firewall do jiné skupiny prostředků nebo předplatného se nepodporuje.|Přesunutí brány firewall do jiné skupiny prostředků nebo předplatného se nepodporuje.|Podpora této funkce je naše mapa cest. Pokud chcete bránu firewall přesunout do jiné skupiny prostředků nebo předplatného, musíte odstranit aktuální instanci a znovu ji vytvořit v nové skupině prostředků nebo předplatném.|
|Výstrahy analýzy hrozeb se můžou maskovat.|Pravidla sítě s cílem 80/443 pro filtry odchozího filtrování upozorňující výstrahy, když jsou nakonfigurovaná jenom na režim výstrahy.|Vytvořte filtrování odchozího připojení pro 80/443 pomocí pravidel aplikací. Nebo změňte režim analýzy hrozeb na **Alert a Deny**.|
|Azure Firewall používá Azure DNS jenom pro překlad názvů.|Azure Firewall řeší plně kvalifikované názvy domény pouze pomocí Azure DNS. Vlastní server DNS není podporovaný. V ostatních podsítích není nijak ovlivněn překlad DNS.|Pracujeme na zmírnit toto omezení.|
|Azure Firewall DNAT nefunguje pro cíle privátních IP adres|Podpora Azure Firewall DNAT je omezená na odchozí přenosy z Internetu a příchozí přenos dat. DNAT v současné době nefunguje pro cíle privátních IP adres. Například paprskový na paprskový.|Toto je aktuální omezení.|
|První konfiguraci veřejné IP adresy nejde odebrat.|Každá Azure Firewall veřejná IP adresa je přiřazena ke *konfiguraci protokolu IP*.  Při nasazení brány firewall se přiřadí první konfigurace IP adresy a obvykle obsahuje odkaz na podsíť brány firewall (Pokud není explicitně nakonfigurovaný přes nasazení šablony). Tuto konfiguraci protokolu IP nemůžete odstranit, protože by to vedlo ke zrušení přidělení brány firewall. Pokud má brána firewall k dispozici alespoň jednu jinou veřejnou IP adresu, můžete i nadále změnit nebo odebrat veřejnou IP adresu přidruženou k této konfiguraci protokolu IP.|Toto chování je úmyslné.|
|Zóny dostupnosti se dají konfigurovat jenom během nasazování.|Zóny dostupnosti se dají konfigurovat jenom během nasazování. Po nasazení brány firewall nemůžete Zóny dostupnosti nakonfigurovat.|Toto chování je úmyslné.|
|SNAT při příchozích připojeních|Kromě DNAT jsou připojení přes veřejnou IP adresu (příchozí) brány firewall před jejich vstupem na jednu z privátních IP adres brány firewall. Tento požadavek dnes (také pro aktivní/aktivní síťová virtuální zařízení) zajistíte tak, aby se zajistilo symetrické směrování.|Pokud chcete zachovat původní zdroj pro HTTP/S, zvažte použití hlaviček [xff](https://en.wikipedia.org/wiki/X-Forwarded-For) . Můžete například použít službu, jako je například [přední vrátka Azure](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) nebo [Azure Application Gateway](../application-gateway/rewrite-http-headers.md) před bránou firewall. WAF můžete také přidat jako součást služby Azure front-dveří a řetězit k bráně firewall.
|Podpora filtrování plně kvalifikovaného názvu domény SQL pouze v režimu proxy (port 1433)|Pro Azure SQL Database, Azure SQL Data Warehouse a Azure SQL Managed instance:<br><br>V průběhu verze Preview se filtrování plně kvalifikovaného názvu domény SQL podporuje jenom v režimu proxy serveru (port 1433).<br><br>Pro Azure SQL IaaS:<br><br>Pokud používáte nestandardní porty, můžete tyto porty zadat v pravidlech aplikací.|V případě SQL v režimu přesměrování (výchozí při připojování z Azure) můžete místo toho filtrovat přístup pomocí značky služby SQL jako součást Azure Firewallch síťových pravidel.
|Odchozí provoz na portu TCP 25 není povolený.| Odchozí připojení SMTP, která používají port TCP 25, jsou blokovaná. Port 25 se primárně používá pro neověřené doručování e-mailů. Toto je výchozí chování platformy pro virtuální počítače. Další informace najdete v tématu řešení potíží s [odchozím připojením SMTP v Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Na rozdíl od virtuálních počítačů ale tuto funkci v tuto chvíli nemůžete povolit na Azure Firewall. Poznámka: Pokud chcete umožnit ověřenou službu SMTP (port 587) nebo SMTP přes jiný port než 25, ujistěte se prosím, že jste nakonfigurovali síťové pravidlo a ne pravidlo aplikace, protože kontrola SMTP se v tuto chvíli nepodporuje.|Použijte doporučenou metodu k odeslání e-mailu, jak je popsáno v článku věnovaném odstraňování potíží SMTP. Nebo vylučte virtuální počítač, který potřebuje odchozí přístup SMTP z výchozí trasy k bráně firewall. Místo toho nakonfigurujte odchozí přístup přímo na Internet.
|Aktivní FTP se nepodporuje.|Aktivní FTP je v Azure Firewall zakázané, aby se chránily proti útokům na vrácení FTP pomocí příkazu FTP PORT.|Místo toho můžete použít pasivní FTP. V bráně firewall je stále nutné explicitně otevřít porty TCP 20 a 21.
|Metrika využití portu SNAT zobrazuje 0%|Metrika využití portů Azure Firewall SNAT může zobrazovat 0% využití i v případě, že se používají porty SNAT. V takovém případě poskytuje použití metriky jako součást metriky stavu brány firewall nesprávný výsledek.|Tento problém byl opraven a zavedení do produkčního prostředí je cílené na květen 2020. V některých případech se problém vyřeší opětovným nasazením brány firewall, ale není konzistentní. V rámci dočasného řešení je třeba stav brány firewall použít jenom k vyhledání *stavu = snížený*, ne pro *stav = v pořádku*. Vyčerpání portů se zobrazí jako *degradované*. *Není v pořádku* , pokud jde o budoucí použití v případě, že jsou další metriky ovlivněny stavem brány firewall.
|DNAT se nepodporuje s povoleným vynuceným tunelovým propojením.|Brány firewall nasazené s povoleným vynuceným tunelovým propojením nemůžou podporovat příchozí přístup z Internetu kvůli asymetrickému směrování.|Jedná se o návrh z důvodu asymetrického směrování. Návratová cesta pro příchozí připojení prochází přes místní bránu firewall, která neviděla navázání připojení.
|Odchozí pasivní FTP nefunguje pro brány firewall s více veřejnými IP adresami.|Pasivní FTP vytvoří různá připojení pro řídicí a datové kanály. Když brána firewall s více veřejnými IP adresami odesílá odchozí data, náhodně vybere jednu z jejích veřejných IP adres pro zdrojovou IP adresu. FTP se nezdařila, pokud datové a řídicí kanály používají jiné zdrojové IP adresy.|Naplánovala se explicitní konfigurace SNAT. V tuto situaci zvažte použití jediné IP adresy.|
|V NetworkRuleHitu metriky chybí dimenze protokolu.|Metrika ApplicationRuleHit umožňuje protokol založený na filtrování, ale tato funkce chybí v odpovídající NetworkRuleHitové metrikě.|Probíhá šetření opravy.|
|Pravidla překladu adres (NAT) s porty mezi 64000 a 65535 nejsou podporovaná.|Azure Firewall povoluje jakýkoli port v rozsahu 1-65535 v pravidlech sítě a aplikace, ale pravidla NAT podporují jenom porty v rozsahu 1-63999.|Toto je aktuální omezení.
|Aktualizace konfigurace můžou v průměru trvat pět minut.|Aktualizace konfigurace Azure Firewall může v průměru trvat tři až pět minut a paralelní aktualizace nejsou podporované.|Probíhá šetření opravy.|
|Azure Firewall používá k filtrování provozu HTTPS a MSSQL hlavičky SNI TLS.|Pokud prohlížeč nebo serverový software nepodporuje rozšíření SNI (název serveru), nebudete se moct připojit prostřednictvím Azure Firewall.|Pokud prohlížeč nebo serverový software nepodporuje SNI, může být možné řídit připojení pomocí síťového pravidla namísto pravidla aplikace. Software, který podporuje SNI, najdete v tématu [indikace názvu serveru](https://wikipedia.org/wiki/Server_Name_Indication) .

## <a name="next-steps"></a>Další kroky

- [Kurz: Nasazení a konfigurace brány Azure Firewall pomocí webu Azure Portal](tutorial-firewall-deploy-portal.md)
- [Nasazení brány Azure Firewall pomocí šablony](deploy-template.md)
- [Vytvoření testovacího prostředí brány Azure Firewall](scripts/sample-create-firewall-test.md)
