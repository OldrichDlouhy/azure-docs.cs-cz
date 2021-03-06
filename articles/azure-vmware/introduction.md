---
title: Úvod
description: Seznamte se s funkcemi a výhodami řešení Azure VMware (AVS) pro nasazení a správu úloh na bázi VMware v Azure.
ms.topic: overview
ms.date: 05/04/2020
ms.openlocfilehash: 9e3ab0abfdaa613a08675356bc5b01949e0381ae
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749678"
---
# <a name="what-is-azure-vmware-solution-avs-preview"></a>Co je Azure VMware Solution (AVS) Preview?

Řešení Azure VMware (AVS) nabízí privátní cloudy v Azure. Tyto privátní cloudy obsahují clustery vSphere a využívají vyhrazenou infrastrukturu holých počítačů Azure. Clustery v privátních cloudech můžete škálovat na 3 až 16 hostitelů a v jednom privátním cloudu můžete mít více clusterů. Všechny privátní cloudy se zřídí s vCenter Server, síti vSAN, vSphere a NSX-T. Do privátních cloudů můžete migrovat úlohy z místních prostředků, můžete v nich vytvářet nebo nasazovat nové virtuální počítače a využívat v nich služby Azure.

Funkce AVS je ověřované řešení VMware s průběžným ověřováním a testováním vylepšení a upgradů. Infrastruktura a software privátního cloudu spravuje a udržuje společnost Microsoft a umožňuje zaměřit se na vývoj a spouštění úloh v privátních cloudech.

Následující diagram znázorňuje vztah mezi soukromými cloudy a virtuální sítě v Azure, službami Azure a místním prostředí. Přístup k síti z privátních cloudů do služeb Azure nebo virtuální sítě zajišťuje integraci koncových bodů služeb Azure řízených smlouvou SLA. Přístup k privátnímu cloudu z místních prostředí používá Global Reach ExpressRoute pro privátní a zabezpečené připojení.

![Obrázek synchronizačního podílu privátního cloudu služby AVS do Azure a místního prostředí](./media/adjacency-overview-drawing-final.png)

## <a name="hosts-clusters-and-private-clouds"></a>Hostitelé, clustery a privátní cloudy

Privátní cloudy a clustery služby AVS jsou postavené z holého hostitele infrastruktury Azure, který se konverguje pomocí technologie Hyper-v. Hostitelé s vysokým konečným objemem jsou 576 – GB paměti RAM a duální procesory Intel 18 Core a 2,3 GHz. Hostitelé mají dvě síti vSAN diskgroups s 15,36 celkovou úrovní kapacity síti vSAN TB (SSD) a 3,2 TB (NVMe) síti vSAN úrovně mezipaměti.

Nové privátní cloudy se nasazují prostřednictvím Azure Portal nebo Azure CLI.

## <a name="networking"></a>Sítě

Při nasazení privátního cloudu se vytvoří privátní sítě pro správu, zřizování a vMotion. Tyto privátní sítě se používají pro přístup ke Správci vCenter a NSX-T a pro vMotion nebo nasazení virtuálních počítačů. Všechny privátní sítě jsou přístupné z virtuální sítě v Azure nebo z místních prostředí. ExpressRoute Global Reach slouží k propojení privátních cloudů s místními prostředími a toto připojení vyžaduje virtuální síť s okruhem ExpressRoute v rámci vašeho předplatného.

Přístup k Internetu a službám Azure se zřídí při nasazení privátního cloudu. Přístup je k dispozici, aby virtuální počítače v sítích produkčních úloh mohly využívat služby Azure nebo internetové služby. Přístup k Internetu je ve výchozím nastavení zakázán pro nové privátní cloudy a může být kdykoli povolen nebo zakázán.

Další informace o sítích a vzájemné propojení najdete v článku [Koncepty sítě](concepts-networking.md) .

## <a name="access-and-security"></a>Přístup a zabezpečení

Pro zvýšení zabezpečení používají privátní cloudy služby AVS řízení přístupu na základě role vSphere. vSphere jednotné přihlašování LDAP je možné integrovat s Azure Active Directory. Další informace o identitě a oprávněních najdete v článku [koncepce přístupu a identit](concepts-identity.md) .

Síti vSAN šifrování na REST je ve výchozím nastavení povolené a používá se k zajištění zabezpečení úložiště síti vSAN. Podrobněji se popisuje v článku [Koncepty úložiště](concepts-storage.md) .

## <a name="host-and-software-lifecycle-maintenance"></a>Údržba životního cyklu hostitele a softwaru

Pravidelná upgrade privátního cloudu služby AVS a softwaru VMware zajišťuje, že se v privátních cloudech spustí nejnovější sady funkcí zabezpečení, stability a funkcí. Další podrobnosti o údržbě a údržbě platforem jsou k dispozici v článku [Koncepty upgradu](concepts-upgrades.md) .

## <a name="monitoring-your-private-cloud"></a>Monitorování privátního cloudu

Pomocí [protokolů v Azure monitor](../azure-monitor/overview.md) můžete shromažďovat protokoly na virtuálních počítačích, které běží ve vašem privátním cloudu služby AVS. [Agenta MMA můžete stáhnout a nainstalovat](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration) na virtuální počítače se systémem Linux a Windows v privátních cloudech služby AVS pomocí stejných dotazů, které spouštíte na místních virtuálních počítačích. Můžete spouštět stejné dotazy, které byste normálně spustili na virtuálních počítačích, a to jenom stejně. Další informace o vytváření dotazů najdete v tématu [Postup zápisu dotazů](../azure-monitor/log-query/log-query-overview.md#how-can-i-learn-how-to-write-queries).

## <a name="next-steps"></a>Další kroky

Dalším krokem je seznámit se s klíčovým [koncepcím privátního cloudu a clusteru](concepts-private-clouds-clusters.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md
