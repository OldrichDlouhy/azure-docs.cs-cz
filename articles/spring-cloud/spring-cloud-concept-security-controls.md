---
title: Koncepce – řízení zabezpečení pro jarní cloudovou službu Azure
description: Používejte bezpečnostní mechanismy, které jsou integrované v Azure jaře Cloud Service.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 2e001e5e927d9d4c5dc4c3eb74f7b5ad33617b99
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037572"
---
# <a name="security-controls-for-azure-spring-cloud-service"></a>Ovládací prvky zabezpečení pro službu Azure Spring Cloud
Ovládací prvky zabezpečení jsou integrované do služby Azure jaře Cloud Service.

Řízení zabezpečení je kvalita nebo funkce služby Azure, která přispívá ke schopnostem služby zabránit, zjišťovat a reagovat na ohrožení zabezpečení.  Pro každý ovládací prvek používáme *Ano* nebo *ne* k označení, zda je v tuto chvíli pro službu nastavená.  Pro ovládací prvek, který není použitelný pro službu, používáme *N/a* . 

**Ovládací prvky zabezpečení ochrany dat**

| Řízení zabezpečení | Ano/Ne | Poznámky | Dokumentace |
|:-------------|:-------|:-------------------------------|:----------------------|
| Šifrování na straně serveru v klidovém umístění: klíče spravované společností Microsoft | Yes | Uživatel nahrál zdroje a artefakty, nastavení konfiguračního serveru, nastavení aplikace a data v trvalém úložišti jsou uložená v Azure Storage, která automaticky zašifruje obsah v klidovém umístění.<br><br>Mezipaměť serveru konfigurace, běhové binární soubory sestavené z nahraného zdroje a protokoly aplikací během životnosti aplikace se ukládají do disku spravovaného Azure, který automaticky šifruje obsah v klidovém režimu.<br><br>Image kontejnerů sestavené ze zdroje odeslaného uživatelem jsou uloženy v Azure Container Registry, který automaticky šifruje obsah image v klidovém umístění. | [Šifrování služby Azure Storage pro neaktivní uložená data](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)<br><br>[Šifrování na straně serveru Azure Managed disks](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption)<br><br>[Úložiště imagí kontejneru v Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-storage) |
| Šifrování v přechodném případě | Yes | Veřejné koncové body uživatelské aplikace používají k příchozímu přenosu standardně HTTPS. |  |
| Zašifrovaná volání rozhraní API | Yes | Volání správy ke konfiguraci jarní cloudové služby Azure nastávají prostřednictvím Azure Resource Manager volání přes protokol HTTPS. | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/) |

**Ovládací prvky zabezpečení přístupu k síti**

| Řízení zabezpečení | Ano/Ne | Poznámky | Dokumentace |
|:-------------|:-------|:-------------------------------|:----------------------|
| Značka služby | Yes | Pomocí značky služby **AzureSpringCloud** můžete definovat odchozí řízení přístupu k síti pro [skupiny zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) nebo [Azure firewall](https://docs.microsoft.com/azure/firewall/service-tags), aby se povolil provoz do cloudových aplikací Azure.<br><br>*Poznámka:* V současné době je v současnosti jenom nová **instance služby jarní** cloudová služba Azure vytvořená po 2020/07/14. | [Značky služeb](https://docs.microsoft.com/azure/virtual-network/service-tags-overview) |
