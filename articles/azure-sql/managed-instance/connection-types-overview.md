---
title: Typy připojení
titleSuffix: Azure SQL Managed Instance
description: Další informace o typech připojení spravované instance SQL Azure
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: 6c6774fb462a21e721b19ae53d1d018d780b28ae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85517316"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Typy připojení spravované instance SQL Azure
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Tento článek vysvětluje, jak se klienti připojují ke službě Azure SQL Managed instance v závislosti na typu připojení. Ukázky skriptů pro změnu typů připojení jsou uvedené níže a s důležitými informacemi týkajícími se změny výchozího nastavení připojení.

## <a name="connection-types"></a>Typy připojení

Spravovaná instance Azure SQL podporuje tyto dva typy připojení:

- **Přesměrování (doporučeno):** Klienti navážou připojení přímo k uzlu, který je hostitelem databáze. Pokud chcete povolit připojení pomocí přesměrování, musíte otevřít brány firewall a skupiny zabezpečení sítě (NSG), abyste mohli povolit přístup na portech 1433 a 11000-11999. Pakety se přecházejí přímo do databáze a díky přesměrování přes proxy se pak zvyšují latence a výkon.
- **Proxy server (výchozí):** V tomto režimu všechna připojení používají komponentu proxy serveru. Chcete-li povolit připojení, je nutné otevřít pouze port 1433 pro privátní sítě a port 3342 pro veřejné připojení. Výběr tohoto režimu může mít za následek vyšší latenci a nižší propustnost, a to v závislosti na povaze úlohy. Pro nejnižší latenci a nejvyšší propustnost důrazně doporučujeme zásady připojení přesměrování použít u zásad připojení k proxy serveru.

## <a name="redirect-connection-type"></a>Přesměrování – typ připojení

Když se v typu připojení přesměrování naváže relace TCP na modul SQL, klientská relace získá cílovou virtuální IP adresu uzlu virtuálního clusteru z nástroje pro vyrovnávání zatížení. Následné pakety se přecházejí přímo na uzel virtuálního clusteru a brána se vynechá. Tento tok přenosů znázorňuje následující diagram.

![redirect.png](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Typ připojení přesměrování aktuálně funguje jenom pro soukromý koncový bod. Bez ohledu na nastavení typu připojení by připojení přicházející prostřednictvím veřejného koncového bodu byla prostřednictvím proxy serveru.

## <a name="proxy-connection-type"></a>Typ připojení proxy

V typu připojení proxy je relace TCP vytvořena pomocí brány a všech následných paketů toku. Tento tok přenosů znázorňuje následující diagram.

![proxy.png](./media/connection-types-overview/proxy.png)

## <a name="changing-connection-type"></a>Změna typu připojení

- **Používání portálu:** Chcete-li změnit typ připojení pomocí Azure Portal, otevřete stránku Virtual Network a pomocí nastavení **Typ připojení** změňte typ připojení a uložte změny.

- **Skript pro změnu nastavení typu připojení pomocí prostředí PowerShell:**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Následující skript prostředí PowerShell ukazuje, jak změnit typ připojení pro spravovanou instanci na `Redirect` .

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your managed instance, and replace {mi-name} with the name of your managed instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Další kroky

- [Obnovení databáze do spravované instance SQL](restore-sample-database-quickstart.md)
- Zjistěte, jak [nakonfigurovat veřejný koncový bod na spravované instanci SQL](public-endpoint-configure.md) .
- Další informace o [architektuře připojení spravované instance SQL](connectivity-architecture-overview.md)
