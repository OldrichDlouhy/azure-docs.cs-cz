---
title: Azure bastionu | Microsoft Docs
description: Další informace o Azure bastionu
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: overview
ms.date: 01/31/2020
ms.author: cherylmc
ms.openlocfilehash: 299a69675eed1ba958c6d13cf447407450df2abb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80411105"
---
# <a name="what-is-azure-bastion"></a>Co je Azure Bastion?

Služba Azure bastionu je nová plně spravovaná služba PaaS spravovaná platformou, kterou zřizujete v rámci vaší virtuální sítě. Poskytuje zabezpečené a bezproblémové připojení RDP/SSH k virtuálním počítačům přímo v Azure Portal přes TLS. Když se připojíte přes Azure Bastion, virtuální počítače nepotřebují veřejnou IP adresu.

Bastionu zajišťuje zabezpečené připojení RDP a SSH ke všem virtuálním počítačům ve virtuální síti, ve které se zřídí. Použití Azure bastionu chrání vaše virtuální počítače před vystavení portů RDP/SSH na vnějším světě a zároveň zajišťuje zabezpečený přístup pomocí protokolu RDP/SSH. S Azure bastionu se připojujete k virtuálnímu počítači přímo z Azure Portal. Nepotřebujete dalšího klienta, agenta ani software.

## <a name="architecture"></a>Architektura

Nasazení Azure bastionu je vázané na virtuální síť, ne pro předplatné/účet nebo virtuální počítač. Po zřízení služby Azure bastionu ve vaší virtuální síti bude prostředí RDP/SSH dostupné pro všechny vaše virtuální počítače ve stejné virtuální síti.

RDP a SSH jsou některé ze základních prostředků, pomocí kterých se můžete připojit ke svým úlohám, které běží v Azure. Vystavení portů RDP/SSH přes Internet se nepožaduje a zobrazuje se jako významná hladina hrozeb. To je často způsobeno chybami zabezpečení protokolu. Aby tato hrozba obsahovala tyto hrozby, můžete na veřejné straně hraniční sítě nasadit hostitele bastionu (označované také jako servery skoků). Hostitelské servery bastionu jsou navržené a nakonfigurované tak, aby odolaly útokům. Servery bastionu také poskytují připojení RDP a SSH k úlohám, které se probírají za bastionu, a dále v síti.

![Architektura](./media/bastion-overview/architecture.png)

Na tomto obrázku vidíte architekturu nasazení Azure bastionu. V tomto diagramu:

* Hostitel bastionu je nasazený ve virtuální síti.
* Uživatel se připojí k Azure Portal pomocí libovolného prohlížeče HTML5.
* Uživatel vybere virtuální počítač, ke kterému se má připojit.
* Jediným kliknutím se v prohlížeči otevře relace RDP/SSH.
* Na virtuálním počítači Azure se nevyžaduje žádná veřejná IP adresa.

## <a name="key-features"></a>Klíčové funkce

K dispozici jsou následující funkce:

* **RDP a SSH přímo v Azure Portal:** K relaci RDP a SSH se můžete přímo dostat přímo v Azure Portal pomocí jediného prostředí s možností bezproblémového kliknutí.
* **Vzdálená relace přes protokol TLS a průchod bránou firewall pro RDP/SSH:** Služba Azure bastionu využívá webového klienta založeného na HTML5, který se automaticky streamuje na vaše místní zařízení, takže se vaše relace RDP/SSH přes TLS na portu 443 umožní bezpečně procházet podnikové brány firewall.
* **Na virtuálním počítači Azure není potřeba žádná veřejná IP adresa:** Azure bastionu otevře připojení RDP/SSH k virtuálnímu počítači Azure pomocí privátní IP adresy na vašem virtuálním počítači. Na virtuálním počítači nepotřebujete veřejnou IP adresu.
* **Žádné nepříjemnosti při správě skupin zabezpečení sítě:** Azure bastionu je plně spravovaná služba platformy PaaS z Azure, která je posílená interně, aby poskytovala zabezpečené připojení RDP/SSH. Nemusíte používat žádné skupin zabezpečení sítě v podsíti Azure bastionu. Vzhledem k tomu, že se Azure bastionu připojuje k virtuálním počítačům přes soukromou IP adresu, můžete nakonfigurovat skupin zabezpečení sítě tak, aby povoloval jenom RDP/SSH jenom z Azure bastionu. Tím se eliminují starosti se správou skupin zabezpečení sítě pokaždé, když se budete muset bezpečně připojit k virtuálním počítačům.
* **Ochrana proti kontrole portů:** Vzhledem k tomu, že virtuální počítače nemusíte zveřejňovat pro veřejný Internet, jsou vaše virtuální počítače chráněné před kontrolou portů neautorizovanými a zlomyslnými uživateli, kteří se nacházejí mimo vaši virtuální síť.
* **Chraňte proti neoprávněným zneužitím. Posílení zabezpečení pouze na jednom místě:** Azure bastionu je plně spravovaná služba PaaS spravovaná platformou. Vzhledem k tomu, že se nachází na hraničních sítích vaší virtuální sítě, nemusíte se starat o posílení zabezpečení každého virtuálního počítače ve vaší virtuální síti. Platforma Azure chrání před neoprávněnými útoky tím, že zajišťuje posílení zabezpečení Azure bastionu a vždycky aktuální za vás.

## <a name="faq"></a>Nejčastější dotazy

[!INCLUDE [Bastion FAQ](../../includes/bastion-faq-include.md)]

## <a name="next-steps"></a>Další kroky

* [Vytvořte prostředek hostitele Azure bastionu](bastion-create-host-portal.md).
* Informace o některých dalších klíčových [možnostech sítě](../networking/networking-overview.md) v Azure.
