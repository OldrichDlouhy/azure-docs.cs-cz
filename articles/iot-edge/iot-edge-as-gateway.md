---
title: Brány pro zařízení pro příjem dat – Azure IoT Edge | Microsoft Docs
description: Pomocí Azure IoT Edge můžete vytvořit transparentní, neprůhledné nebo proxy zařízení brány proxy, které odesílá data z více podřízených zařízení do cloudu nebo je místně zpracuje.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: d7c924af297d9a315b61351b69d2fe6346bc1178
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232623"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Jak se dá zařízení IoT Edge využít jako brána

Brány v IoT Edge řešení poskytují připojení zařízení a hraniční analýzy pro zařízení IoT, která by jinak neposkytovala tyto možnosti. Azure IoT Edge můžete použít k uspokojení jakékoli potřeby brány IoT, ať už se jedná o připojení, identitu nebo Edge Analytics. Vzorce brány v tomto článku odkazují jenom na vlastnosti připojení zařízení a identity zařízení, a ne na to, jak se data zařízení zpracovávají v bráně.

## <a name="patterns"></a>Vzory

Existují tři způsoby použití zařízení IoT Edge jako brány: transparentní, překlad protokolu a překlad identity:

* **Transparentní** – zařízení, která se teoreticky můžou připojit k IoT Hub se můžou místo toho připojit k zařízení brány. Podřízená zařízení mají vlastní identity IoT Hubu a používají protokoly MQTT, AMQP nebo HTTP. Brána jednoduše předává komunikaci mezi zařízeními a IoT Hubem. Zařízení i uživatelé, kteří s nimi pracují prostřednictvím IoT Hub, nevědí, že brána Mediating jejich komunikaci. Nedostatečné povědomí znamená, že brána je považována za *transparentní*. Konkrétní informace týkající se používání zařízení IoT Edge jako transparentní brány najdete v tématu [Vytvoření transparentní brány](how-to-create-transparent-gateway.md).
* **Překlad protokolu** – známý taky jako neprůhledný model brány, zařízení, která nepodporují MQTT, AMQP nebo http, můžou používat zařízení brány k posílání dat, která se IoT Hub jménem. Brána rozumí protokolu, který podřízená zařízení používají, a je jediným zařízením, které má v IoT Hubu identitu. Všechny informace vypadají jako z jednoho zařízení a brány. Pokud cloudové aplikace chtějí analyzovat data podle jednotlivých zařízení, musí podřízená zařízení vložit do svých zpráv dodatečné identifikační údaje. Primitiva IoT Hubu, jako jsou dvojčata a metody, jsou navíc k dispozici pouze pro zařízení brány, nikoli pro podřízená zařízení.
* **Překlad identity** – zařízení, která se nemůžou připojit k IoT Hub se můžou místo toho připojit k zařízení brány. Brána poskytuje identitu IoT Hubu a překlad protokolu jménem podřízených zařízení. Brána je dostatečně inteligentní, aby porozuměla protokolu používanému v podřízených zařízeních, poskytla jim identitu a přeložila primitiva IoT Hubu. Podřízená zařízení se v IoT Hubu zobrazují jako prvotřídní zařízení s dvojčaty a metodami. Uživatel může se zařízeními v IoT Hubu pracovat a není si vědom zprostředkujícího zařízení brány.

![Modely transparentního diagramu, protokolu a brány identity](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Případy použití

Všechny vzory bran poskytují následující výhody:

* **Analýzy na hraničních** zařízeních – používají se místně služby AI ke zpracování dat přicházejících ze zařízení, která se neodesílají do cloudu, bez odesílání telemetrie s plnou věrností. Najděte a reagují na přehledy místně a odešlete jenom podmnožinu dat, která IoT Hub.
* **Izolace zařízení** po jednotlivých zařízeních – zařízení brány může chránit všechna zařízení pro příjem dat před expozicí Internetu. Může se navázat mezi sítí, která nemá připojení, a sítí IT, která poskytuje přístup k webu.
* **Multiplexing připojení** – všechna zařízení, která se připojují k IoT Hub přes IoT Edge bránu, používají stejné základní připojení.
* **Vyhlazení provozu** – IoT Edge zařízení automaticky implementuje exponenciální omezení rychlosti, pokud IoT Hub omezuje provoz, zatímco se zprávy ukládají místně. Díky této výhodě je vaše řešení odolné vůči špičkám v provozu.
* **Podpora offline** – zařízení brány ukládá zprávy a zdvojené aktualizace, které nelze doručit IoT Hub.

Brána, která překládá protokol, může také provádět Edge Analytics, izolaci zařízení, vyhlazování provozu a podporu offline pro stávající zařízení a nová zařízení, která jsou omezená na prostředky. Mnoho stávajících zařízení vyrábí data, která můžou být v Power Business Insights. nejsou ale navržené s ohledem na cloudové připojení. Neprůhledné brány umožňují, aby tato data byla odemčená a používána v řešení IoT.

Brána, která provádí překlad identity, nabízí výhody překladu protokolů a navíc umožňuje úplnou správu zařízení v cloudu. Všechna zařízení v řešení IoT se zobrazí v IoT Hub bez ohledu na protokol, který používají.

## <a name="cheat-sheet"></a>Tahák

Tady je rychlý tahák list, který porovnává IoT Hub primitivních elementů při použití transparentních, neprůhledných (protokolů) a proxy bran.

| Primitivní | Transparentní brána | Překlad protokolu | Překlad identity |
|--------|-------------|--------|--------|
| Identity uložené v registru IoT Hub identity | Identity všech připojených zařízení | Jenom identita zařízení brány | Identity všech připojených zařízení |
| Dvojče zařízení | Každé připojené zařízení má vlastní zařízení. | Pouze brána má nevlákenná zařízení a moduly. | Každé připojené zařízení má vlastní zařízení. |
| Přímé metody a zprávy z cloudu na zařízení | Cloud může adresovat každé připojené zařízení jednotlivě. | Cloud může adresovat jenom zařízení brány. | Cloud může adresovat každé připojené zařízení jednotlivě. |
| [Omezení IoT Hub a kvóty](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Použít pro každé zařízení | Použít na zařízení brány | Použít pro každé zařízení |

Při použití vzoru neprůhledné brány (překladu protokolu) všechna zařízení připojující přes tuto bránu sdílejí stejnou frontu typu cloud-zařízení, která může obsahovat maximálně 50 zpráv. Postupuje podle toho, že by se měl model neprůhledné brány použít jenom v případě, že se přes každou bránu pole připojuje několik zařízení a jejich provoz z cloudu na zařízení je nízký.

## <a name="next-steps"></a>Další kroky

Přečtěte si, jak nastavit transparentní bránu:

* [Konfigurace zařízení IoT Edge tak, aby fungovalo jako transparentní brána](how-to-create-transparent-gateway.md)
* [Ověření podřízeného zařízení pro Azure IoT Hub](how-to-authenticate-downstream-device.md)
* [Připojení podřízeného zařízení k bráně služby Azure IoT Edge](how-to-connect-downstream-device.md)
