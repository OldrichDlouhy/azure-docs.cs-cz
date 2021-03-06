---
title: 'VPN Gateway: Upravte nastavení IP adresy brány: Azure Portal'
description: Tento článek vás provede změnou předpon IP adres pro bránu místní sítě pomocí Azure Portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fa43df8c4f17bff4e97d999c6653bdcb045bfec3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84985213"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Úprava nastavení místní síťové brány pomocí webu Azure Portal

Někdy se nastavení pro bránu místní sítě AddressPrefix nebo GatewayIPAddress změní. V tomto článku se dozvíte, jak upravit nastavení místní síťové brány. Tato nastavení můžete také upravit pomocí jiné metody výběrem jiné možnosti z následujícího seznamu:

Než připojení odstraníte, možná budete chtít stáhnout konfiguraci pro připojení zařízení, aby se získal definovaný PSK. Tímto způsobem ho nemusíte předefinovat na druhé straně.

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Upravit předpony IP adres

Když upravíte předpony IP adres, postup podle následujících kroků závisí na tom, jestli má brána místní sítě připojení.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Úprava IP adresy brány

Pokud zařízení VPN, ke kterému se chcete připojit, změnilo svou veřejnou IP adresu, musíte upravit bránu místní sítě, aby odrážela tuto změnu. Když změníte veřejnou IP adresu, postupujte podle kroků uvedených v závislosti na tom, jestli má brána místní sítě připojení.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Další kroky

Můžete ověřit připojení brány. Viz [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).
