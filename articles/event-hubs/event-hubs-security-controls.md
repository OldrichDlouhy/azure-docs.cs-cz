---
title: Řízení zabezpečení pro Azure Event Hubs
description: Tento článek poskytuje kontrolní seznam kontrol zabezpečení pro vyhodnocení Event Hubs Azure (síť, identita, ochrana dat atd.).
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: da20778f1e24372e445d635e675df6484905f195
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85315397"
---
# <a name="security-controls-for-azure-event-hubs"></a>Řízení zabezpečení pro Azure Event Hubs

Tento článek popisuje ovládací prvky zabezpečení integrované do Azure Event Hubs.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Síť

| Řízení zabezpečení | Ano/Ne | Poznámky | Dokumentace |
|---|---|--|--|
| Podpora koncového bodu služby| Yes |  |  |
| Podpora vkládání virtuální sítě| No | |  |
| Izolace sítě a podpora brány firewall| Yes |  |  |
| Podpora vynuceného tunelování| No |  |  |

## <a name="monitoring--logging"></a>Monitorování protokolování &

| Řízení zabezpečení | Ano/Ne | Poznámky| Dokumentace |
|---|---|--|--|
| Podpora monitorování Azure (Log Analytics, App Insights atd.)| Yes | |  |
| Protokolování a audit roviny řízení a správy| Yes |  |  |
| Protokolování a audit roviny dat| Yes |   |  |

## <a name="identity"></a>Identita

| Řízení zabezpečení | Ano/Ne | Poznámky| Dokumentace |
|---|---|--|--|
| Authentication| Yes | | [Autorizace přístupu k Azure Event Hubs](authorize-access-event-hubs.md), [autorizace přístupu k prostředkům Event Hubs pomocí Azure Active Directory](authorize-access-azure-active-directory.md), [autorizace přístupu k prostředkům Event Hubs pomocí sdílených přístupových podpisů](authorize-access-shared-access-signature.md) |
| Autorizace|  Yes | | [Ověření spravované identity pomocí Azure Active Directory pro přístup k prostředkům Event Hubs](authenticate-managed-identity.md), [ověření aplikace pomocí Azure Active Directory pro přístup](authenticate-application.md)k prostředkům Event Hubs, [ověření přístupu k Event Hubs prostředkům pomocí sdílených přístupových podpisů (SAS)](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Ochrana dat

| Řízení zabezpečení | Ano/Ne | Poznámky | Dokumentace |
|---|---|--|--|
| Šifrování na straně serveru v klidovém umístění: klíče spravované společností Microsoft |  Yes | |  |
| Šifrování na straně serveru v klidovém umístění: klíče spravované zákazníkem (BYOK) | Ano. K dispozici pro vyhrazené clustery. | Klíč spravovaný zákazníkem ve službě Azure Key trezor se dá použít k šifrování dat v centru událostí v klidovém formátu. | [Konfigurace klíčů spravovaných zákazníkem pro šifrování dat Azure Event Hubs v klidovém formátu pomocí Azure Portal](configure-customer-managed-key.md) |
| Šifrování na úrovni sloupce (Azure Data Services)| Není k dispozici | |  |
| Šifrování při přenosu (například šifrování ExpressRoute, šifrování virtuální sítě a šifrování virtuální sítě)| Yes | |  |
| Zašifrovaná volání rozhraní API| Yes |  |  |

## <a name="configuration-management"></a>Správa konfigurace

| Řízení zabezpečení | Ano/Ne | Poznámky| Dokumentace |
|---|---|--|--|
| Podpora správy konfigurace (Správa verzí konfigurace atd.)| Ano | |  |

## <a name="next-steps"></a>Další kroky

- Přečtěte si další informace o [integrovaných kontrolních prvcích zabezpečení napříč službami Azure](../security/fundamentals/security-controls.md).
