---
title: Referenční architektura pro řešení monitorování kvality vody sestavená pomocí Azure IoT Central | Microsoft Docs
description: Seznamte se s koncepty řešení pro monitorování kvality vody sestavené s využitím Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 0c4b13c56a68205195bd5ad4b696d9e01786a8dd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77016540"
---
# <a name="water-quality-monitoring-reference-architecture"></a>Referenční architektura monitorování kvality vody 



Řešení pro monitorování kvality vody se dají vytvořit pomocí **šablony aplikace Azure IoT Central** jako aplikace IoT Starter. Tento článek poskytuje podrobné pokyny k architektuře referenční architektury pro vytváření kompletního řešení. 


![Architektura monitorování kvality vody](./media/concepts-waterqualitymonitoring-architecture/concepts-waterqualitymonitoring-architecture1.png)

Koncepty:

1. Zařízení a připojení  
1. IoT Central 
2. Rozšiřitelnost a integrace
3. Obchodní aplikace

Pojďme se podívat na klíčové komponenty, které obecně hrají součást v řešení monitorování kvality vody.

## <a name="devices-and-connectivity"></a>Zařízení a připojení 
V této části budeme označovat zařízení, která se používají při monitorování kvality vody nebo monitorování spotřeby vody, a to obvykle jako zařízení s inteligentními vodami. Inteligentní zařízení může být Flow měřičů, monitorů kvality vody, inteligentní ventily, detektory nevracení a tak dále.

Zařízení použitá v inteligentních vodách budou obecně připojena prostřednictvím operátoru sítě (LPWAN) s nízkou spotřebou (WAN). U těchto typů zařízení můžete využít [most služby Azure IoT Central](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) k posílání dat zařízení do vaší aplikace IoT ve službě Azure IoT Central. Alternativně můžete mít brány zařízení, které jsou schopné IP a můžou se přímo připojit k IoT Central.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central je aplikační platforma IoT, která vám umožní rychle začít pracovat v řešení IoT. Vaše řešení můžete označit, přizpůsobit a integrovat s službami třetích stran.
Po připojení inteligentních zařízení k IoT Central získáte příkazy a ovládání zařízení, monitorování a upozorňování, uživatelské rozhraní s integrovanými RBACmi, konfigurovatelnými řídicími panely a možnostmi rozšiřitelnosti. 

## <a name="extensibility-and-integrations"></a>Rozšiřitelnost a integrace 
Aplikaci IoT můžete v IoT Central zvětšit a volitelně:
* Transformujte a Integrujte data IoT pro pokročilé analýzy, například školení modelů strojového učení, prostřednictvím průběžného exportu dat z aplikace IoT Central.
* automatizace pracovních postupů v jiných systémech aktivací akcí pomocí Microsoft Flow nebo webhooků z aplikace IoT Central
* programově přístup k aplikaci IoT v IoT Central prostřednictvím rozhraní API IoT Central

## <a name="business-applications"></a>Obchodní aplikace 
Data IoT je možné využít k napájení různých podnikových aplikací v rámci vodního nástroje. Informace o tom, jak připojit IoT Central aplikaci pro monitorování kvality vody k polím služby, najdete v článku o [integraci s poli služeb Dynamics 365](./how-to-configure-connected-field-services.md). 


## <a name="next-steps"></a>Další kroky
* Informace o tom, jak [vytvořit monitorování kvality vody](./tutorial-water-quality-monitoring.md) IoT Central aplikaci
* Další informace o [IoT Central státních šablonách](./overview-iot-central-government.md)
* Další informace o IoT Central najdete v tématu [IoT Central Overview](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central) .

