---
title: Skupiny IP adres v zásadách Azure Firewall
description: Skupiny IP adres umožňují seskupovat a spravovat IP adresy pro pravidla Azure Firewall.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 20372d9584becf18c65e13c81b99403ce88d27e4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568408"
---
# <a name="ip-groups-in-azure-firewall-policy"></a>Skupiny IP adres v zásadách Azure Firewall

Skupiny IP adres umožňují seskupit a spravovat IP adresy pro zásady Azure Firewall následujícími způsoby:

- Jako typ zdroje v pravidlech DNAT
- Jako zdrojový nebo cílový typ v síťových pravidlech
- Jako typ zdroje v pravidlech aplikací


Skupina IP adres může mít jednu IP adresu, několik IP adres nebo jeden nebo víc rozsahů IP adres.

Skupiny IP adres se dají znovu použít v Azure Firewall DNAT, síti a pravidel aplikací pro několik bran firewall v rámci oblastí a předplatných v Azure. Názvy skupin musí být jedinečné. Skupinu IP adres můžete nakonfigurovat v Azure Portal, Azure CLI nebo REST API. K dispozici je Ukázková šablona, která vám umožní začít.

## <a name="sample-format"></a>Vzorový formát

Následující příklady formátu IPv4 adres jsou platné pro použití ve skupinách IP:

- Jedna adresa: 10.0.0.0
- Zápis CIDR: 10.1.0.0/32
- Rozsah adres: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>Vytvoření skupiny IP adres

Skupina IP adres se dá vytvořit pomocí Azure Portal, Azure CLI nebo REST API. Další informace najdete v tématu [Vytvoření skupiny IP adres](../firewall/create-ip-group.md).

## <a name="browse-ip-groups"></a>Procházet skupiny IP adres
1. Na panelu hledání Azure Portal zadejte **skupiny IP adres** a vyberte je. Můžete zobrazit seznam skupin IP adres, nebo můžete vybrat **Přidat** a vytvořit novou skupinu IP adres.
2. Vyberte skupinu IP adres a otevřete stránku Přehled. Můžete upravit, přidat nebo odstranit IP adresy nebo skupiny IP adres.

   ![Přehled skupin IP adres](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>Správa skupiny IP adres

Všechny IP adresy můžete zobrazit ve skupině IP adres a podle pravidel nebo prostředků, které jsou k němu přidružené. Pokud chcete odstranit skupinu IP adres, musíte nejdřív oddělit skupinu IP od prostředku, který ji používá.

1. Pokud chcete zobrazit nebo upravit IP adresy, v části **Nastavení** v levém podokně vyberte **IP adresy** .
2. Pokud chcete přidat jednu nebo více IP adres, vyberte **Přidat IP adresy**. Tím se otevře stránka pro odeslání **nebo procházení** , nebo můžete adresu zadat ručně.
3.    Výběr tří teček (**...**) na pravé straně pro úpravu nebo odstranění IP adres. Chcete-li upravit nebo odstranit více IP adres, zaškrtněte políčka a v horní části vyberte možnost **Upravit** nebo **Odstranit** .
4. Nakonec může soubor exportovat ve formátu souboru CSV.

> [!NOTE]
> Pokud odstraníte všechny IP adresy ve skupině IP adres, když se stále používá v pravidle, toto pravidlo se přeskočí.


## <a name="use-an-ip-group"></a>Použití skupiny IP adres

Při vytváření zásad s DNAT, aplikací nebo pravidly sítě teď můžete jako typ **zdroje** nebo **cílový typ** vybrat možnost **Skupina IP** adres pro IP adresy.

:::image type="content" source="media/ip-groups/firewall-ip-group.png" alt-text="Skupiny IP adres v bráně firewall":::

## <a name="ip-address-limits"></a>Omezení IP adres

Pro 50 nebo méně skupin IP adres můžete mít každou instanci brány firewall maximálně 5000 individuálních IP adres. Pro skupiny IP adres 51 až 100 můžete pro každou instanci brány firewall určit jednotlivé IP adresy 500.

### <a name="examples"></a>Příklady

#### <a name="example-1-supported"></a>Příklad 1: podporováno

|Skupiny IP adres  |Počet IP adres  |Notace  |Pravidlo  |
|---------|---------|---------|---------|
|IPGroup1 |4 096     |10.0.0.0/20  |Rule1|
|IPGroup2     |3|196.0.0.0 - 196.0.0.2|Rule1|
|IPGroup3     |1|1.2.3.4|Rule1|
|     |**Celkem 4100**|         |         |
|     |         |         |         |

#### <a name="example-2-supported"></a>Příklad 2: podporované

|Skupiny IP adres  |Počet IP adres  |Notace  |Pravidlo  |
|---------|---------|---------|---------|
|IPGroup1 |4 096     |10.0.0.0/20  |Rule1|
|IPGroup2     |4 096|11.0.0.0/20|Rule1|
|     |**Celkem 8192**|         |         |

#### <a name="example-3-not-supported"></a>Příklad 3: nepodporováno

|Skupiny IP adres  |Počet IP adres  |Notace  |Pravidlo  |
|---------|---------|---------|---------|
|IPGroup1 |8192     |10.0.0.0/20, 11.0.0.0/20  |Rule1|
|     |**Celkem 8192**|||

#### <a name="example-4-supported"></a>Příklad 4: podporováno

|Skupiny IP adres  |Počet IP adres  |Notace  |Pravidlo  |
|---------|---------|---------|---------|
|IPGroup1 |4 096     |10.0.0.0/20  |Rule1|
|IPGroup2     |4 096|11.0.0.0/20|Rule2|
|     |**Celkem 8192**|         |         |


## <a name="related-azure-powershell-cmdlets"></a>Související rutiny Azure PowerShell

K vytváření a správě skupin IP adres se dají použít následující rutiny Azure PowerShell:

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Remove-AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [New-AzFirewallPolicyNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicynetworkrule?view=azps-3.4.0)
- [New-AzFirewallPolicyApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicyapplicationrule?view=azps-3.4.0)
- [New-AzFirewallPolicyNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallpolicynatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Další kroky

- [Kurz: zabezpečení virtuální sítě WAN pomocí správce Azure Firewall](secure-cloud-network.md)