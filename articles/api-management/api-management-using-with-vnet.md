---
title: Použití služby Azure API Management s virtuálními sítěmi
description: Naučte se, jak nastavit připojení k virtuální síti ve službě Azure API Management a přistupovat k webovým službám prostřednictvím ní.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/22/2020
ms.author: apimpm
ms.custom: references_regions
ms.openlocfilehash: e3acfb9552db9fa972b0a407e52cece014b45389
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87025009"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Použití služby Azure API Management s virtuálními sítěmi
Virtuální sítě Azure umožňují umístit jakékoli prostředky Azure do jiné než internetové sítě podporující směrování, ke které můžete řídit přístup. Tyto sítě je pak možné připojit k místním sítím pomocí různých technologií VPN. Další informace o virtuálních sítích Azure najdete tady: [Přehled Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

Službu Azure API Management lze nasadit v rámci virtuální sítě (VNET), aby mohla přistupovat k back-end službám v síti. Portál pro vývojáře a brána API je možné nakonfigurovat tak, aby byly přístupné buď z Internetu, nebo jenom v rámci virtuální sítě.

> [!NOTE]
> Adresa URL pro import dokumentu v rozhraní API musí být hostovaná na veřejně přístupné internetové adrese.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Předpoklady

K provedení kroků popsaných v tomto článku musíte mít:

+ Musíte mít aktivní předplatné Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Instance APIM Další informace najdete v tématu [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).

## <a name="enable-vnet-connection"></a><a name="enable-vpn"> </a>Povolit připojení k virtuální síti

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Povolení připojení VNET pomocí Azure Portal

1. Pokud chcete najít instanci API Management, přejít na [Azure Portal](https://portal.azure.com) . Vyhledejte a vyberte **API Management Services**.

2. Vyberte instanci API Management.

3. Vyberte **virtuální síť**.
4. Nakonfigurujte instanci API Management, která se má nasadit v rámci virtuální sítě.

    ![Nabídka virtuální síť API Management][api-management-using-vnet-menu]
5. Vyberte požadovaný typ přístupu:

    * **Off**: Toto je výchozí nastavení. API Management není nasazený ve virtuální síti.

    * **Externí**: API Management brána a portál pro vývojáře jsou přístupné z veřejného Internetu prostřednictvím externího nástroje pro vyrovnávání zatížení. Brána má přístup k prostředkům v rámci virtuální sítě.

        ![Veřejný partnerský vztah][api-management-vnet-public]

    * **Interní**: API Management brána a portál pro vývojáře jsou přístupné jenom z virtuální sítě prostřednictvím interního nástroje pro vyrovnávání zatížení. Brána má přístup k prostředkům v rámci virtuální sítě.

        ![Soukromý partnerský vztah][api-management-vnet-private]

6. Pokud jste vybrali možnost **externí** nebo **interní**, zobrazí se seznam všech oblastí, ve kterých se služba API Management zřídí. Zvolte **umístění**a pak vyberte svou **virtuální síť** a **podsíť**. Seznam Virtual Network se naSprávce prostředků plní v rámci předplatného Azure, které jsou dostupné v předplatných Azure, které jsou nastavené v oblasti, kterou konfigurujete.

    > [!IMPORTANT]
    > Při nasazování instance Azure API Management do virtuální sítě Správce prostředků musí být služba ve vyhrazené podsíti, která neobsahuje žádné další prostředky s výjimkou instancí Azure API Management. Pokud se provede pokus o nasazení instance služby Azure API Management do podsítě Správce prostředků virtuální sítě, která obsahuje další prostředky, nasazení se nezdaří.

    Pak vyberte **Použít**. Stránka **virtuální síť** vaší instance API Management se aktualizuje novými možnostmi virtuální sítě a podsítě.

    ![Vybrat síť VPN][api-management-setup-vpn-select]

7. V horním navigačním panelu vyberte **Uložit**a pak vyberte **použít konfiguraci sítě**.

> [!NOTE]
> Adresa VIP instance API Management se změní pokaždé, když je virtuální síť povolená nebo zakázaná.
> Adresa VIP se také změní, když API Management přesunete z **externích** do **interního**nebo naopak.
>

> [!IMPORTANT]
> Pokud API Management odeberete z virtuální sítě nebo změníte nasazení v nástroji, dříve použitá síť VNET může zůstat uzamčena po dobu až šesti hodin. Během této doby nebude možné virtuální síť odstranit ani do ní nasadit nový prostředek. Toto chování platí pro klienty, kteří používají rozhraní API verze 2018-01-01 a starší. Klienti používající rozhraní API verze 2019-01-01 a novější, virtuální síť se uvolní hned po odstranění přidružené API Management služby.

## <a name="enable-vnet-connection-using-powershell-cmdlets"></a><a name="enable-vnet-powershell"> </a>Povolení připojení VNet pomocí rutin PowerShellu
Připojení k virtuální síti můžete taky povolit pomocí rutin PowerShellu.

* **Vytvoření služby API Management v rámci virtuální**sítě: pomocí rutiny [New-AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) vytvořte službu Azure API Management v rámci virtuální sítě.

* **Nasazení existující služby API Management v rámci virtuální**sítě: pomocí rutiny [Update-AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) můžete přesunout existující službu Azure API Management do Virtual Network.

## <a name="connect-to-a-web-service-hosted-within-a-virtual-network"></a><a name="connect-vnet"> </a>Připojení k webové službě hostované ve virtuální síti
Po připojení služby API Management k virtuální síti se přístup k back-endu službám v rámci této sítě neliší od přístupu k veřejným službám. Stačí zadat místní IP adresu nebo název hostitele (Pokud je server DNS nakonfigurovaný pro virtuální síť) webové služby na pole **Adresa URL webové služby** při vytváření nového rozhraní API nebo úpravou existujícího rozhraní API.

![Přidat rozhraní API z sítě VPN][api-management-setup-vpn-add-api]

## <a name="common-network-configuration-issues"></a><a name="network-configuration-issues"> </a>Běžné problémy s konfigurací sítě
Následuje seznam běžných potíží s chybou konfigurace, ke kterým může dojít při nasazování služby API Management do Virtual Network.

* **Nastavení vlastního serveru DNS**: služba API Management závisí na několika službách Azure. Pokud je API Management hostovaný ve virtuální síti s vlastním serverem DNS, musí přeložit názvy hostitelů těchto služeb Azure. [Postupujte prosím podle pokynů pro](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) vlastní nastavení DNS. Podívejte se na tabulku porty níže a další požadavky na síť pro referenci.

> [!IMPORTANT]
> Pokud máte v úmyslu používat pro virtuální síť vlastní servery DNS, měli byste ji nastavit **před** nasazením služby API Management do ní. V opačném případě je potřeba aktualizovat API Management službu pokaždé, když změníte servery DNS spuštěním [operace použít konfiguraci sítě](/rest/api/apimanagement/2019-12-01/apimanagementservice/applynetworkconfigurationupdates) .

* **Porty vyžadované pro API Management**: příchozí a odchozí provoz do podsítě, ve které API Management nasazená, se dají řídit pomocí [skupiny zabezpečení sítě][Network Security Group]. Pokud některý z těchto portů není dostupný, API Management nemusí správně fungovat a může být nepřístupný. Pokud se některý z těchto portů zablokuje, je při použití API Management s virtuální sítí další běžný problém s konfigurací.

<a name="required-ports"> </a> Když je instance služby API Management hostovaná ve virtuální síti, použijí se porty v následující tabulce.

| Zdrojový nebo cílový port (y) | Směr          | Transportní protokol |   [Značky služeb](../virtual-network/security-overview.md#service-tags) <br> Zdroj/cíl   | Účel ( \* )                                                 | Typ Virtual Network |
|------------------------------|--------------------|--------------------|---------------------------------------|-------------------------------------------------------------|----------------------|
| */[80], 443                  | Příchozí            | TCP                | INTERNET/VIRTUAL_NETWORK            | Komunikace klienta s API Management                      | Externí             |
| */3443                     | Příchozí            | TCP                | ApiManagement/VIRTUAL_NETWORK       | Koncový bod správy pro Azure Portal a PowerShell         | Externí & interní  |
| */443                  | Odchozí           | TCP                | VIRTUAL_NETWORK/úložiště             | **Závislost na Azure Storage**                             | Externí & interní  |
| */443                  | Odchozí           | TCP                | VIRTUAL_NETWORK/Azureactivedirectory selhala | [Azure Active Directory](api-management-howto-aad.md) (Pokud je k dispozici)                   | Externí & interní  |
| */1433                     | Odchozí           | TCP                | VIRTUAL_NETWORK/SQL                 | **Přístup k koncovým bodům SQL Azure**                           | Externí & interní  |
| */5671, 5672, 443          | Odchozí           | TCP                | VIRTUAL_NETWORK/EventHub            | Závislost pro [protokolování do zásad centra událostí](api-management-howto-log-event-hubs.md) a agenta monitorování | Externí & interní  |
| */445                      | Odchozí           | TCP                | VIRTUAL_NETWORK/úložiště             | Závislost na sdílené složce Azure pro [Git](api-management-configuration-repository-git.md)                      | Externí & interní  |
| */443                     | Odchozí           | TCP                | VIRTUAL_NETWORK/AzureCloud            | Rozšíření stavu a monitorování         | Externí & interní  |
| */1886, 443                     | Odchozí           | TCP                | VIRTUAL_NETWORK/AzureMonitor         | Publikování [diagnostických protokolů a metrik](api-management-howto-use-azure-monitor.md), [Resource Health](../service-health/resource-health-overview.md) a [Application Insights](api-management-howto-app-insights.md)                   | Externí & interní  |
| */25, 587, 25028                       | Odchozí           | TCP                | VIRTUAL_NETWORK/INTERNET            | Připojení k předávání SMTP pro odesílání e-mailů                    | Externí & interní  |
| */6381 – 6383              | Příchozí & odchozí | TCP                | VIRTUAL_NETWORK/VIRTUAL_NETWORK     | Přístup ke službě Redis pro zásady [mezipaměti](api-management-caching-policies.md) mezi počítači         | Externí & interní  |
| */4290              | Příchozí & odchozí | UDP                | VIRTUAL_NETWORK/VIRTUAL_NETWORK     | Čítače synchronizace pro zásady [Omezení četnosti](api-management-access-restriction-policies.md#LimitCallRateByKey) mezi počítači         | Externí & interní  |
| * / *                        | Příchozí            | TCP                | AZURE_LOAD_BALANCER/VIRTUAL_NETWORK | Load Balancer infrastruktury Azure                          | Externí & interní  |

>[!IMPORTANT]
> Pro úspěšné nasazení API Management služby se vyžadují porty, pro které je *účel* **tučný** . Blokování dalších portů ale způsobí, že se **bude zpomalovat** schopnost používat a **monitorovat běžící službu a poskytovat potvrzenou smlouvu SLA**.

+ **Funkce TLS**: Pokud chcete povolit sestavení a ověření řetězu certifikátů TLS/SSL, služba API Management potřebuje odchozí síťové připojení k ocsp.msocsp.com, mscrl.microsoft.com a CRL.Microsoft.com. Tato závislost není povinná, pokud kterýkoli certifikát, který nahráváte do API Management, obsahuje úplný řetěz k kořenovému adresáři certifikační autority.

+ **Přístup DNS**: pro komunikaci se servery DNS se vyžaduje odchozí přístup na portu 53. Pokud na druhém konci brány VPN existuje vlastní server DNS, musí být server DNS dosažitelný z podsítě hostující API Management.

+ **Metriky a sledování stavu**: odchozí síťové připojení ke koncovým bodům monitorování Azure, které se řeší v následujících doménách. Jak je znázorněno v tabulce, tyto adresy URL jsou reprezentované v rámci značky služby AzureMonitor pro použití se skupinami zabezpečení sítě.

    | Prostředí Azure | Koncové body                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Veřejný partnerský vztah Azure      | <ul><li>gcs.prod.monitoring.core.windows.net (**nové**)</li><li>prod.warmpath.msftcloudes.com (**bude zastaralé**)</li><li>global.prod.microsoftmetrics.com (**nové**)</li><li>global.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2.prod.microsoftmetrics.com (**nové**)</li><li>shoebox2.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**nové**)</li><li>prod3.metrics.nsatc.net (**bude zastaralé**)</li><li>prod3-black.prod.microsoftmetrics.com (**nové**)</li><li>prod3-black.prod3.metrics.nsatc.net (**bude zastaralé**)</li><li>prod3-red.prod.microsoftmetrics.com (**nové**)</li><li>prod3-red.prod3.metrics.nsatc.net (**bude zastaralé**)</li><li>gcs.prod.warm.ingestion.monitoring.azure.com</li></ul> |
    | Azure Government  | <ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>global.prod.microsoftmetrics.com (**nové**)</li><li>global.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2.prod.microsoftmetrics.com (**nové**)</li><li>shoebox2.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**nové**)</li><li>prod3.metrics.nsatc.net (**bude zastaralé**)</li><li>prod3-black.prod.microsoftmetrics.com</li><li>prod3-red.prod.microsoftmetrics.com</li><li>prod5.prod.microsoftmetrics.com</li><li>prod5-black.prod.microsoftmetrics.com</li><li>prod5-red.prod.microsoftmetrics.com</li><li>gcs.prod.warm.ingestion.monitoring.azure.us</li></ul>                                                                                                                                                                                                                                                |
    | Azure (Čína) 21Vianet     | <ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>global.prod.microsoftmetrics.com (**nové**)</li><li>global.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2.prod.microsoftmetrics.com (**nové**)</li><li>shoebox2.metrics.nsatc.net (**bude zastaralé**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**nové**)</li><li>prod3.metrics.nsatc.net (**bude zastaralé**)</li><li>prod3-black.prod.microsoftmetrics.com</li><li>prod3-red.prod.microsoftmetrics.com</li><li>prod5.prod.microsoftmetrics.com</li><li>prod5-black.prod.microsoftmetrics.com</li><li>prod5-red.prod.microsoftmetrics.com</li><li>gcs.prod.warm.ingestion.monitoring.azure.cn</li></ul>                                                                                                                                                                                                                                                |

  >[!IMPORTANT]
  > Změna clusterů výše se zónou DNS **. nsatc.NET** na **. Microsoftmetrics.com** je většinou změnou DNS. IP adresa clusteru se nezmění.

+ **Značky regionální služby**: NSG pravidla umožňující odchozí připojení k úložištím, SQL a klíčovým službám Event Hubs můžou používat regionální verze těchto značek odpovídající oblasti, která obsahuje instanci API Management (například Storage. WestUS pro instanci API Management v západní USA oblasti). V nasazeních ve více oblastech by NSG v každé oblasti měl umožňovat provoz do značek služeb pro tuto oblast a primární oblast.

+ **Předávání SMTP**: odchozí připojení k síti pro přenos SMTP, které se řeší v rámci hostitele `smtpi-co1.msn.com` ,, `smtpi-ch1.msn.com` `smtpi-db3.msn.com` `smtpi-sin.msn.com` a`ies.global.microsoft.com`

+ **Portál pro vývojáře CAPTCHA**: odchozí připojení k síti pro CAPTCHA portálu pro vývojáře, které se řeší v rámci hostitelů `client.hip.live.com` a `partner.hip.live.com` .

+ **Diagnostika Azure Portal**: Chcete-li povolit tok diagnostických protokolů z Azure Portal při použití rozšíření API Management v rámci Virtual Network, je vyžadován odchozí přístup k `dc.services.visualstudio.com` portu 443. To pomáhá při řešení problémů, se kterými se můžete setkat při používání rozšíření.

+ **Azure Load Balancer**: povolení příchozího požadavku ze značky služby není `AZURE_LOAD_BALANCER` požadavkem na `Developer` skladovou položku, protože pro ni nasazujeme jenom jednu jednotku Compute. Příchozí z [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md) se ale bude velmi důležitý při škálování na větší skladovou položku `Premium` , jako v případě selhání sondy stavu od Load Balancer, selže nasazení.

+ **Application Insights**: Pokud je v API Management povolené monitorování [Azure Application Insights](api-management-howto-app-insights.md) , musíme z Virtual Network povolit odchozí připojení ke [koncovému bodu telemetrie](/azure/azure-monitor/app/ip-addresses#outgoing-ports) . 

+ **Vynucení tunelového přenosu do místní brány firewall pomocí expresního nebo síťového virtuálního zařízení**: běžným zákaznickým nastavením je definování vlastní výchozí trasy (0.0.0.0/0), která vynucuje veškerý provoz z API Management delegované podsítě pro přenos prostřednictvím místní brány firewall nebo síťového virtuálního zařízení. Tento tok přenosů invariably přerušení připojení k Azure API Management, protože odchozí přenosy jsou buď blokované místně, nebo překlad adres (NAT) na nerozpoznatelnou sadu adres, které už nefungují s různými koncovými body Azure. Řešení vyžaduje, abyste provedete několik věcí:

  * Povolte koncové body služby v podsíti, ve které je nainstalovaná služba API Management. Pro Azure SQL, Azure Storage, Azure EventHub a Azure ServiceBus musí být povolené [koncové body služby][ServiceEndpoints] . Povolení koncových bodů přímo z API Management delegované podsítě těmto službám umožňuje používat páteřní síť Microsoft Azure, která poskytuje optimální směrování pro provoz služeb. Pokud používáte koncové body služby s vynuceným tunelovou správou rozhraní API, výše uvedený provoz služeb Azure není vynuceně tunelování. Druhý provoz závislosti služby API Management je vynucené tunelování a nelze ho ztratit nebo služba API Management nebude správně fungovat.
    
  * Všechny přenosy řídicích rovin z Internetu do koncového bodu správy služby API Management jsou směrovány přes konkrétní sadu příchozích IP adres hostovaných API Management. V případě vynuceného tunelování nebudou odpovědi symetricky mapovány zpět na tyto příchozí IP adresy příchozích dat. Abychom překonali omezení, musíme přidat následující uživatelsky definované trasy ([udr][UDRs]) k řízení provozu zpátky do Azure tím, že nastavíte cíl těchto hostitelských tras na Internet. Sada příchozích IP adres pro provoz řídicí roviny je doložená [IP adresami řídicí roviny](#control-plane-ips) .

  * Pro jiné závislosti API Management služby, které jsou vynuceným tunelovým propojením, by měl existovat způsob, jak tento název hostitele vyřešit a jak se obrátit na koncový bod. Mezi ně patří
      - Metriky a sledování stavu
      - Diagnostika Azure Portal
      - Předávání SMTP
      - Portál pro vývojáře CAPTCHA

## <a name="troubleshooting"></a><a name="troubleshooting"> </a>Poradce při potížích
* **Počáteční nastavení**: když počáteční nasazení služby API Management do podsítě neproběhne úspěšně, doporučuje se nejdřív nasadit virtuální počítač do stejné podsítě. Další Vzdálená plocha na virtuální počítač a ověřte, že existuje připojení k jednomu z prostředků níže v předplatném Azure.
    * Azure Storage objekt BLOB
    * Azure SQL Database
    * Azure Storage tabulka

  > [!IMPORTANT]
  > Po ověření připojení nezapomeňte odebrat všechny prostředky nasazené v podsíti, než nasadíte API Management do podsítě.

* **Přírůstkové aktualizace**: při provádění změn v síti se podívejte na [NetworkStatus API](/rest/api/apimanagement/2019-12-01/networkstatus), abyste ověřili, že služba API Management neztratila přístup k žádnému z kritických prostředků, na kterých závisí. Stav připojení by se měl aktualizovat každých 15 minut.

* **Navigační odkazy prostředků**: při nasazení do podsítě virtuálních sítí ve stylu Správce prostředků API Management rezervuje podsíť tím, že vytvoří navigační odkaz na prostředek. Pokud podsíť již obsahuje prostředek od jiného poskytovatele, nasazení se **nezdaří**. Obdobně platí, že když přesunete API Management službu do jiné podsítě nebo ji odstraníte, odebereme tento odkaz na tento prostředek.

## <a name="subnet-size-requirement"></a><a name="subnet-size"> </a> Požadavek na velikost podsítě
Azure rezervuje některé IP adresy v rámci každé podsítě a tyto adresy se nedají použít. První a poslední IP adresa podsítí jsou vyhrazené pro shodu protokolu a tři další adresy, které se používají pro služby Azure. Další informace najdete v tématu [jakákoli omezení používání IP adres v těchto podsítích](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) .

Kromě IP adres, které používá infrastruktura virtuální sítě Azure, každá instance služby API Management v podsíti používá dvě IP adresy na jednotku SKU úrovně Premium nebo jednu IP adresu pro SKU pro vývojáře. Každá instance si vyhrazuje další IP adresu pro externí nástroj pro vyrovnávání zatížení. Při nasazování do interní virtuální sítě musí být pro interní nástroj pro vyrovnávání zatížení k další IP adresu.

Vzhledem k výpočtu výše minimální velikosti podsítě, ve které API Management lze nasadit, je/29, které poskytuje tři použitelné IP adresy.

Každá další jednotka API Management škálování vyžaduje dvě další IP adresy.

## <a name="routing"></a><a name="routing"> </a> Směrování
+ Veřejná IP adresa (VIP) s vyrovnáváním zatížení bude vyhrazena pro poskytování přístupu ke všem koncovým bodům služby.
+ IP adresa z rozsahu IP adres podsítě (DIP) se použije pro přístup k prostředkům v rámci virtuální sítě a veřejná IP adresa (VIP) se použije pro přístup k prostředkům mimo virtuální síť.
+ Veřejnou IP adresu vyrovnávání zatížení najdete v okně Přehled/základní informace v Azure Portal.

## <a name="limitations"></a><a name="limitations"> </a>Omezení
* Podsíť obsahující API Management Instances nemůže obsahovat žádné další typy prostředků Azure.
* Podsíť a služba API Management musí být ve stejném předplatném.
* Podsíť obsahující API Management instance se nedá přesunout mezi předplatnými.
* Pro API Management nasazení ve více oblastech, která jsou nakonfigurovaná v režimu interní virtuální sítě, se uživatelům zodpovídá za správu vyrovnávání zatížení napříč různými oblastmi, a to při vlastním směrování.
* Připojení z prostředku v globálně partnerské virtuální síti v jiné oblasti do API Management služby v interním režimu nebude fungovat v důsledku omezení platformy. Další informace najdete v tématu [prostředky v jedné virtuální síti nemůžou komunikovat s interním nástrojem pro vyrovnávání zatížení Azure v partnerské virtuální síti](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints) .

## <a name="control-plane-ip-addresses"></a><a name="control-plane-ips"> </a> IP adresy řídicí roviny

IP adresy se dělí **prostředím Azure**. Pokud je povolená IP adresa příchozích požadavků označená s **globálním** umístěním, musí být na seznamu povolená i IP adresa specifická pro **oblast** .

| **Prostředí Azure**|   **Oblast**|  **IP adresa**|
|-----------------|-------------------------|---------------|
| Veřejný partnerský vztah Azure| Střed USA – jih (globální)| 104.214.19.224|
| Veřejný partnerský vztah Azure| Střed USA – sever (globální)| 52.162.110.80|
| Veřejný partnerský vztah Azure| USA – středozápad| 52.253.135.58|
| Veřejný partnerský vztah Azure| Jižní Korea – střed| 40.82.157.167|
| Veřejný partnerský vztah Azure| Spojené království – západ| 51.137.136.0|
| Veřejný partnerský vztah Azure| Japonsko – západ| 40.81.185.8|
| Veřejný partnerský vztah Azure| USA – středosever| 40.81.47.216|
| Veřejný partnerský vztah Azure| Spojené království – jih| 51.145.56.125|
| Veřejný partnerský vztah Azure| Západní Indie| 40.81.89.24|
| Veřejný partnerský vztah Azure| East US| 52.224.186.99|
| Veřejný partnerský vztah Azure| West Europe| 51.145.179.78|
| Veřejný partnerský vztah Azure| Japan East| 52.140.238.179|
| Veřejný partnerský vztah Azure| Francie – střed| 40.66.60.111|
| Veřejný partnerský vztah Azure| Kanada – východ| 52.139.80.117|
| Veřejný partnerský vztah Azure| Spojené arabské emiráty sever| 20.46.144.85|
| Veřejný partnerský vztah Azure| Brazil South| 191.233.24.179|
| Veřejný partnerský vztah Azure| Southeast Asia| 40.90.185.46|
| Veřejný partnerský vztah Azure| Jižní Afrika – sever| 102.133.130.197|
| Veřejný partnerský vztah Azure| Střední Kanada| 52.139.20.34|
| Veřejný partnerský vztah Azure| Jižní Korea – jih| 40.80.232.185|
| Veřejný partnerský vztah Azure| Indie – střed| 13.71.49.1|
| Veřejný partnerský vztah Azure| USA – západ| 13.64.39.16|
| Veřejný partnerský vztah Azure| Australia Southeast| 20.40.160.107|
| Veřejný partnerský vztah Azure| Austrálie – střed| 20.37.52.67|
| Veřejný partnerský vztah Azure| Indie – jih| 20.44.33.246|
| Veřejný partnerský vztah Azure| Střední USA| 13.86.102.66|
| Veřejný partnerský vztah Azure| Austrálie – východ| 20.40.125.155|
| Veřejný partnerský vztah Azure| Západní USA 2| 51.143.127.203|
| Veřejný partnerský vztah Azure| Východní USA 2 EUAP| 52.253.229.253|
| Veřejný partnerský vztah Azure| Střed USA EUAP| 52.253.159.160|
| Veřejný partnerský vztah Azure| Středojižní USA| 20.188.77.119|
| Veřejný partnerský vztah Azure| USA – východ 2| 20.44.72.3|
| Veřejný partnerský vztah Azure| Severní Evropa| 52.142.95.35|
| Veřejný partnerský vztah Azure| Východní Asie| 52.139.152.27|
| Veřejný partnerský vztah Azure| Francie – jih| 20.39.80.2|
| Veřejný partnerský vztah Azure| Švýcarsko – západ| 51.107.96.8|
| Veřejný partnerský vztah Azure| Austrálie – střed 2| 20.39.99.81|
| Veřejný partnerský vztah Azure| Spojené arabské emiráty – střed| 20.37.81.41|
| Veřejný partnerský vztah Azure| Švýcarsko – sever| 51.107.0.91|
| Veřejný partnerský vztah Azure| Jižní Afrika – západ| 102.133.0.79|
| Veřejný partnerský vztah Azure| Německo – středozápad| 51.116.96.0|
| Veřejný partnerský vztah Azure| Německo – sever| 51.116.0.0|
| Veřejný partnerský vztah Azure| Norsko – východ| 51.120.2.185|
| Veřejný partnerský vztah Azure| Norsko – západ| 51.120.130.134|
| Azure (Čína) 21Vianet| Čína – sever (globální)| 139.217.51.16|
| Azure (Čína) 21Vianet| Čína – východ (globální)| 139.217.171.176|
| Azure (Čína) 21Vianet| Čína – sever| 40.125.137.220|
| Azure (Čína) 21Vianet| Čína – východ| 40.126.120.30|
| Azure (Čína) 21Vianet| Čína – sever 2| 40.73.41.178|
| Azure (Čína) 21Vianet| Čína – východ 2| 40.73.104.4|
| Azure Government| USGov) – Virginia (globální)| 52.127.42.160|
| Azure Government| USGov Texas (Global)| 52.127.34.192|
| Azure Government| USGov Virginie| 52.227.222.92|
| Azure Government| USGov Iowa| 13.73.72.21|
| Azure Government| USGov Arizona| 52.244.32.39|
| Azure Government| USGov Texas| 52.243.154.118|
| Azure Government| USDoD – střed| 52.182.32.132|
| Azure Government| USDoD východ| 52.181.32.192|

## <a name="related-content"></a><a name="related-content"> </a>Související obsah
* [Připojení Virtual Network k back-endu pomocí služby VPN Gateway](../vpn-gateway/design.md#s2smulti)
* [Připojení Virtual Network z různých modelů nasazení](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Jak používat inspektora rozhraní API k trasování volání v Azure API Management](api-management-howto-api-inspector.md)
* [Virtual Network nejčastějších dotazech](../virtual-network/virtual-networks-faq.md)
* [Značky služeb](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
[ServiceEndpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[ServiceTags]: ../virtual-network/security-overview.md#service-tags
