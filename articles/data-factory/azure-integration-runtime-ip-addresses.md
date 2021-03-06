---
title: ID adresy modulu Azure Integration Runtime
description: Zjistěte, které IP adresy musíte povolit příchozí provoz z, aby bylo možné správně nakonfigurovat brány firewall pro zabezpečení síťového přístupu k úložištím dat.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: 0c64a38295754e4754326dec126bfbc36e1bef61
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523328"
---
# <a name="azure-integration-runtime-ip-addresses"></a>ID adresy modulu Azure Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

IP adresy, které Azure Integration Runtime používá, závisí na oblasti, ve které se nachází Azure Integration runtime. *Vše* Prostředí Azure Integration runtime, která jsou ve stejné oblasti, používají stejné rozsahy IP adres.

> [!IMPORTANT]  
> Toky dat a Azure Integration Runtime, které umožňují spravované Virtual Network nepodporují použití pevných rozsahů IP adres.
>
> Tyto rozsahy IP adres můžete použít pro provádění přesunu dat, kanálu a externích aktivit. Tyto rozsahy IP adres se dají použít k přidávání do seznamu povolených úložišť dat/skupiny zabezpečení sítě (NSG)/bran firewall pro příchozí přístup z prostředí Azure Integration runtime. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Azure Integration Runtime IP adresy: konkrétní oblasti

Povolte provoz z IP adres uvedených pro Azure Integration runtime v konkrétní oblasti Azure, ve které se vaše prostředky nacházejí. Z [odkazu na stažení rozsahu IP adres značek služby](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files)můžete získat seznam ROZSAHŮ IP adres. Pokud je například oblast Azure **AustraliaEast**, můžete získat seznam ROZSAHŮ IP adres z objektu **DataFactory. AustraliaEast**.


## <a name="known-issue-with-azure-storage"></a>Známý problém s Azure Storage

* Při připojování k účtu Azure Storage nemají pravidla sítě IP žádný vliv na požadavky pocházející z prostředí Azure Integration runtime ve stejné oblasti jako účet úložiště. Další podrobnosti najdete v [tomto článku](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range). 

  Místo toho doporučujeme [při připojování k Azure Storage používat důvěryhodné služby](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993). 

## <a name="next-steps"></a>Další kroky

* [Otázky zabezpečení při přesunu dat v Azure Data Factory](data-movement-security-considerations.md)
