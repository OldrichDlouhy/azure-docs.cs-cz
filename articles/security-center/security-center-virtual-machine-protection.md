---
title: Ochrana počítačů a aplikací
description: Tento dokument popisuje doporučení v Security Center, která vám pomůžou chránit vaše virtuální počítače a počítače a vaše webové aplikace a App Service prostředí.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: d79d1bf846cc023b86c3e33b17c91cce77ffe9ee
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089868"
---
# <a name="protect-your-machines-and-applications"></a>Ochrana počítačů a aplikací
Když Azure Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří doporučení, která vás provedou procesem konfigurace potřebných ovládacích prvků k posílení a ochraně vašich prostředků.

Tento článek vysvětluje stránku **COMPUTE a aplikace** v části zabezpečení prostředků Security Center.

Úplný seznam doporučení, která se vám můžou zobrazit na této stránce, najdete v tématu [doporučení k výpočtům a aplikacím](recommendations-reference.md#recs-computeapp).


## <a name="view-the-security-of-your-compute-and-apps-resources"></a>Zobrazení zabezpečení prostředků COMPUTE a Apps

[![Řídicí panel Security Center](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png)](./media/security-center-virtual-machine-recommendations/compute-and-apps-recs-overview.png#lightbox)

Pokud chcete zobrazit stav prostředků COMPUTE a Apps, vyberte v levém podokně Security Center možnost **compute & aplikace**. K dispozici jsou následující karty:

* **Přehled**: uvádí doporučení pro všechny prostředky výpočtů a aplikací a jejich aktuální stav zabezpečení. 

* [**Virtuální počítače a servery**](#vms-and-computers): uvádí doporučení pro vaše virtuální počítače, počítače a aktuální stav zabezpečení pro každý z nich.

* [**VM Scale Sets**](#vmscale-sets): uvádí doporučení pro vaše sady škálování, 

* [**Cloud Services**](#cloud-services): uvádí doporučení pro webové a pracovní role monitorované nástrojem Security Center

* [**App Services**](#app-services): uvádí doporučení pro vaše aplikační služby App Service Environment a aktuální stav zabezpečení každého

* [**Kontejnery**](#containers): uvádí doporučení pro vaše kontejnery a posouzení zabezpečení jejich konfigurací.

* **Výpočetní prostředky**: uvádí doporučení pro výpočetní prostředky, jako jsou Service Fabric clustery a centra událostí.

### <a name="whats-in-each-tab"></a>Co je na jednotlivých kartách?

Každá karta má více oddílů a v každé části můžete přejít k podrobnostem a zobrazit další podrobnosti o zobrazené položce.

Na každé kartě se také zobrazí doporučení pro relevantní prostředky v monitorovaném prostředí. První sloupec uvádí doporučení, druhý zobrazuje celkový počet ovlivněných prostředků a třetí zobrazuje závažnost problému.

Každé doporučení obsahuje sadu akcí, které můžete provést po jeho výběru. Pokud například vyberete **chybějící aktualizace systému**, zobrazí se počet virtuálních počítačů a počítačů, ve kterých chybí opravy, a závažnost chybějící aktualizace.

> [!NOTE]
> Doporučení zabezpečení jsou stejná jako ta na stránce **doporučení** , ale tady se filtrují na konkrétní vybraný typ prostředku. Další informace o řešení doporučení najdete [v tématu Implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).
>




### <a name="vms-and-servers"></a><a name="vms-and-computers"></a>Virtuální počítače a servery
Část virtuální počítače a počítače poskytuje přehled všech doporučení týkajících se zabezpečení pro vaše virtuální počítače a počítače. Jsou zahrnuty čtyři typy počítačů:

![Počítač umístěný mimo Azure](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png) Počítač mimo Azure.

![Azure Resource Manager virtuálního počítače](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png) Azure Resource Manager virtuální počítač.

![Virtuální počítač Azure Classic](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png) Virtuální počítač Azure Classic.

![Virtuální počítače identifikované z pracovního prostoru](./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png) Virtuální počítače identifikované pouze z pracovního prostoru, který je součástí zobrazeného předplatného. To zahrnuje virtuální počítače z jiných předplatných, které jsou součástí pracovního prostoru v tomto předplatném, a virtuální počítače, které byly nainstalovány s Operations Manager přímým agentem, a nemají žádné ID prostředku.

Ikona, která se zobrazí pod každým doporučením, vám pomůže rychle identifikovat virtuální počítač a počítač, který vyžaduje pozornost, a typ doporučení. Pomocí filtrů můžete také Hledat v seznamu podle **typu prostředku** a podle **závažnosti**.

Pokud chcete přejít k podrobnostem o zabezpečení pro každý virtuální počítač, klikněte na virtuální počítač.
Tady vidíte podrobnosti zabezpečení pro virtuální počítač nebo počítač. V dolní části vidíte doporučenou akci a závažnost každého problému.

[![Cloudové služby](./media/security-center-virtual-machine-recommendations/recommendation-list.png)](./media/security-center-virtual-machine-recommendations/recommendation-list.png#lightbox)




### <a name="virtual-machine-scale-sets"></a><a name="vmscale-sets"></a>Virtual Machine Scale Sets
Security Center automaticky zjišťuje, zda máte sady škálování a doporučuje nainstalovat agenta Log Analytics na ně.

Instalace agenta Log Analytics: 

1. Vyberte doporučení **nainstalovat agenta monitorování do sady škálování virtuálních počítačů**. Zobrazí se seznam nemonitorované sady škálování.

1. Vyberte skupinu škálování, která není v pořádku. Postupujte podle pokynů pro instalaci agenta monitorování pomocí stávajícího naplněné pracovní plochy nebo vytvořte nový. Pokud není nastavená [cenová úroveň](security-center-pricing.md) pracovního prostoru, ujistěte se, že jste ji nastavili.

   ![Instalace MMS](./media/security-center-virtual-machine-recommendations/install-mms.png)

Chcete-li nastavit nové sady škálování na automatickou instalaci agenta Log Analytics:
1. Přejděte na Azure Policy a klikněte na **definice**.

1. Vyhledejte **nasazení zásady Log Analytics agenta pro sadu škálování virtuálních počítačů s Windows** a klikněte na něj.

1. Klikněte na **Přiřadit**.

1. Nastavte **Rozsah** a **Log Analytics pracovní prostor** a klikněte na **přiřadit**.

Pokud chcete nastavit všechny existující sady škálování pro instalaci agenta Log Analytics, můžete v Azure Policy přejít k **nápravě** a použít existující zásady na existující sady škálování.





### <a name="cloud-services"></a><a name="cloud-services"></a>Cloudové služby
Pro cloudové služby se vytvoří doporučení, když je verze operačního systému zastaralá.

![Cloud Services](./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png)

V případě, kdy máte doporučení, postupujte podle kroků v doporučení pro aktualizaci operačního systému. Pokud je k dispozici aktualizace, budete mít upozornění (červená nebo oranžová) v závislosti na závažnosti problému. Úplné vysvětlení tohoto doporučení získáte kliknutím na možnost **Aktualizovat verzi operačního systému** ve sloupci **Popis** .






### <a name="app-services"></a><a name="app-services"></a>Aplikační služby
Pokud chcete zobrazit informace o App Service, musíte být v Security Center cenové úrovně Standard a povolit App Service v předplatném. Pokyny k povolení této funkce najdete v tématu [ochrana App Service pomocí Azure Security Center](security-center-app-services.md).

V části **App Services**najdete seznam prostředí App Service Environment a shrnutí stavu na základě Security Center prováděného hodnocení.

![Aplikační služby](./media/security-center-virtual-machine-recommendations/app-services.png)

Zobrazují se tři typy aplikačních služeb:

![Prostředí App Services](./media/security-center-virtual-machine-recommendations/ase.png) Prostředí App Services

![Webová aplikace](./media/security-center-virtual-machine-recommendations/web-app.png) Webová aplikace

![Aplikace Function](./media/security-center-virtual-machine-recommendations/function-app.png) Aplikace Function

Pokud vyberete webovou aplikaci, otevře se souhrnné zobrazení se třemi kartami:

   - **Doporučení**: na základě posouzení provedených Security Center, která selhala.
   - **Úspěšná vyhodnocení**: seznam hodnocení provedených Security Center, která byla úspěšná.
   - **Nedostupná posouzení**: seznam posouzení, která se nepodařilo spustit z důvodu chyby, nebo doporučení není relevantní pro konkrétní službu App Service.

   V části **doporučení** je seznam doporučení pro vybranou webovou aplikaci a závažnost každého doporučení.

   ![App Services doporučení](./media/security-center-virtual-machine-recommendations/app-services-rec.png)

Vyberte doporučení, abyste zobrazili popis doporučení a seznam špatných prostředků, zdravých prostředků a nekontrolovaných prostředků.

   - Sloupec **úspěšné vyhodnocení** zobrazuje seznam předaných vyhodnocení. Závažnost těchto hodnocení je vždycky zelená.

   - V seznamu vyberte úspěšné posouzení, seznam stavů, které jsou v pořádku, a v seznamu nekontrolovaných prostředků. Pro prostředky, které nejsou v pořádku, je k dispozici karta, ale tento seznam je vždy prázdný, protože hodnocení bylo úspěšné.





### <a name="containers"></a><a name="containers"></a>Kontejnery

Když otevřete kartu **kontejnery** v závislosti na vašem prostředí, může se zobrazit některý ze tří typů prostředků:

![Hostitel kontejneru](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png) Hostitelé kontejneru – virtuální počítače s Docker 

![Kubernetes ](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png) clustery služby Azure Kubernetes Service (AKS). [Další informace Security Center o AKS sadě prostředků](azure-kubernetes-service-integration.md)

![Registry kontejnerů ](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png) Azure Container Registry (ACR). [Další informace Security Center o ACR sadě prostředků](azure-container-registry-integration.md)

Pokyny, jak používat funkce zabezpečení kontejnerů, najdete v tématu [monitorování zabezpečení kontejnerů](monitor-container-security.md).



[![Karta kontejnery](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png)](./media/security-center-virtual-machine-recommendations/container-recommendations-all-types.png#lightbox)

Chcete-li zobrazit doporučení pro konkrétní prostředek v seznamu, klikněte na tento prostředek.

#### <a name="visibility-into-container-registries"></a>Viditelnost registrů kontejnerů

Například kliknutím na registr ASC-demo ACR ze seznamu zobrazeného na obrázku výše navedete na tuto stránku s podrobnostmi:

[![Doporučení pro určitý registr ACR](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png)](./media/security-center-virtual-machine-recommendations/acr-registry-recs-list.png#lightbox)


#### <a name="visibility-into-containers-hosted-on-iaas-linux-machines"></a>Viditelnost kontejnerů hostovaných na počítačích s IaaS Linux

Po kliknutí na jeden z virtuálních počítačů s Docker se zobrazí stránka s podrobnostmi s informacemi týkajícími se kontejnerů v počítači, jako je například verze Docker a počet imagí spuštěných na hostiteli.

![Doporučení pro virtuální počítač s Docker](./media/security-center-virtual-machine-recommendations/docker-recommendation.png)


#### <a name="security-recommendations-based-on-cis-benchmark-for-docker"></a>Doporučení týkající se zabezpečení na základě srovnávacího testu CIS pro Docker

Security Center kontroluje konfigurace Dockeru a poskytuje vám vhled do nesprávných konfigurací tím, že poskytuje seznam všech neúspěšných pravidel, která byla posouzena. Security Center poskytuje pokyny, které vám pomůžou tyto problémy rychle vyřešit a ušetřit čas. Security Center nepřetržitě posuzuje konfigurace Dockeru a poskytuje vám jejich nejnovější stav.

![karta kontejner](./media/security-center-virtual-machine-recommendations/container-cis-benchmark.png)


## <a name="next-steps"></a>Další kroky
Další informace o doporučeních, která se vztahují na jiné typy prostředků Azure, najdete v následujících článcích:

* [Úplný seznam odkazů na doporučení zabezpečení Azure Security Center](recommendations-reference.md)
* [Monitorování identity a přístupu v Azure Security Center](security-center-identity-access.md)
* [Ochrana sítě pomocí Azure Security Center](security-center-network-recommendations.md)
* [Ochrana služby Azure SQL Service v Azure Security Center](security-center-sql-service-recommendations.md)