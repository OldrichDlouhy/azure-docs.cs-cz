---
title: Rozsahy privátních IP adres Azure Firewall SNAT
description: Rozsahy IP adres můžete nakonfigurovat pro SNAT.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: be2bf0f9590a23f9def44a1800338c80f69a782c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610519"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Rozsahy privátních IP adres Azure Firewall SNAT

Azure Firewall poskytuje automatické SNAT pro veškerý odchozí provoz na veřejné IP adresy. Ve výchozím nastavení Azure Firewall nesnat s pravidly sítě, pokud je cílová IP adresa v rozsahu privátních IP adres na [úřad IANA RFC 1918](https://tools.ietf.org/html/rfc1918). Pravidla aplikací se vždycky aplikují pomocí [transparentního proxy serveru](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy) bez ohledu na cílovou IP adresu.

Tato logika funguje dobře při směrování provozu přímo na Internet. Pokud jste ale povolili [vynucené tunelování](forced-tunneling.md), před jejich vstupem se internetový provoz na jednu z privátních IP adres brány firewall v AzureFirewallSubnet, takže se tento zdroj skryje z místní brány firewall.

Pokud vaše organizace používá pro privátní sítě rozsah veřejných IP adres, Azure Firewall SNATs provoz na jednu z privátních IP adres brány firewall v AzureFirewallSubnet. Můžete ale nakonfigurovat Azure Firewall **, aby** nesnat na svůj rozsah veřejných IP adres.

Pokud chcete nakonfigurovat Azure Firewall na nikdy SNAT bez ohledu na cílovou IP adresu, použijte jako rozsah privátních IP adres **0.0.0.0/0** . V této konfiguraci Azure Firewall moci nikdy směrovat přenosy přímo na Internet. Pokud chcete nakonfigurovat bránu firewall tak, aby vždycky byla v nezávisle na cílové adrese, použijte jako rozsah privátních IP adres **255.255.255.255/32** .

## <a name="configure-snat-private-ip-address-ranges---azure-powershell"></a>Nakonfigurujte rozsahy privátních IP adres SNAT – Azure PowerShell

Pomocí Azure PowerShell můžete zadat rozsahy privátních IP adres pro bránu firewall.

### <a name="new-firewall"></a>Nová brána firewall

Pro novou bránu firewall Azure PowerShell příkaz:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> IANAPrivateRanges se rozšíří na aktuální výchozí hodnoty na Azure Firewall, zatímco se do nich přidají jiné rozsahy. Chcete-li zachovat výchozí IANAPrivateRanges ve specifikaci privátního rozsahu, musí zůstat ve vaší `PrivateRange` specifikaci, jak je znázorněno v následujících příkladech.

Další informace najdete v tématu [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Existující brána firewall

Ke konfiguraci existující brány firewall použijte následující příkazy Azure PowerShell:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Šablony

Do části můžete přidat následující `additionalProperties` :

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="configure-snat-private-ip-address-ranges---azure-portal"></a>Nakonfigurujte rozsahy privátních IP adres SNAT – Azure Portal

Pomocí Azure Portal můžete pro bránu firewall zadat rozsahy privátních IP adres.

1. Vyberte skupinu prostředků a potom vyberte bránu firewall.
2. Na stránce **Přehled** **rozsahy privátních IP adres**vyberte výchozí hodnotu **IANA RFC 1918**.

   Otevře se stránka **Upravit předpony privátních IP adres** :

   :::image type="content" source="media/snat-private-range/private-ip.png" alt-text="Upravit předpony privátních IP adres":::

1. Ve výchozím nastavení je **IANAPrivateRanges** nakonfigurovaný.
2. Upravte rozsahy privátních IP adres pro vaše prostředí a pak vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky

- Přečtěte si o [Azure firewall vynucené tunelování](forced-tunneling.md).