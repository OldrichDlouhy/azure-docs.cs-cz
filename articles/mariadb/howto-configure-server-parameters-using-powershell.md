---
title: Konfigurace parametrů serveru-Azure PowerShell-Azure Database for MariaDB
description: Tento článek popisuje, jak nakonfigurovat parametry služby v Azure Database for MariaDB pomocí prostředí PowerShell.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurepowershell
ms.topic: how-to
ms.date: 5/26/2020
ms.openlocfilehash: 4ee0e5bd0d9d1fa941af245ecc6182ba1608eef0
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104958"
---
# <a name="configure-server-parameters-in-azure-database-for-mariadb-using-powershell"></a>Konfigurace parametrů serveru v Azure Database for MariaDB pomocí prostředí PowerShell

Pomocí prostředí PowerShell můžete vypsat, zobrazit a aktualizovat parametry konfigurace serveru Azure Database for MariaDB. Podmnožina konfigurací modulu se zveřejňuje na úrovni serveru a dá se upravit.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto průvodce budete potřebovat:

- [Modul AZ PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) nainstalovaný místně nebo [Azure Cloud Shell](https://shell.azure.com/) v prohlížeči
- [Server Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> I když je modul PowerShell AZ. MariaDb ve verzi Preview, musíte ho nainstalovat samostatně z modulu AZ PowerShellu pomocí následujícího příkazu: `Install-Module -Name Az.MariaDb -AllowPrerelease` .
> Jakmile je modul PowerShellu AZ. MariaDb všeobecně dostupný, bude součástí budoucna k tomu, aby vydaná vydání modulu PowerShellu a nativně dostupná v rámci Azure Cloud Shell.

Pokud se rozhodnete použít prostředí PowerShell místně, připojte se k účtu Azure pomocí rutiny [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Vypsat parametry konfigurace serveru pro Azure Database for MariaDB Server

Chcete-li zobrazit seznam všech parametrů s modifikátory na serveru a jejich hodnotách, spusťte `Get-AzMariaDbConfiguration` rutinu.

Následující příklad zobrazí seznam parametrů konfigurace serveru pro server **mydemoserver** ve skupině prostředků **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Definice všech uvedených parametrů naleznete v části Reference MariaDB na [proměnných systému serveru](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Zobrazit podrobnosti parametru konfigurace serveru

Chcete-li zobrazit podrobnosti o konkrétním parametru konfigurace pro server, spusťte `Get-AzMariaDbConfiguration` rutinu a zadejte parametr **Name** .

Tento příklad ukazuje podrobnosti parametru konfigurace serveru **pomalého \_ dotazu \_ ** pro server **mydemoserver** v části Skupina prostředků **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Změna hodnoty parametru konfigurace serveru

Můžete také změnit hodnotu určitého parametru konfigurace serveru, který aktualizuje základní konfigurační hodnotu pro modul MariaDB serveru. Pokud chcete aktualizovat konfiguraci, použijte `Update-AzMariaDbConfiguration` rutinu.

Pokud chcete aktualizovat parametr konfigurace serveru s **pomalým \_ dotazem \_ ** na serveru **mydemoserver** v části Skupina prostředků **myresourcegroup**.

```azurepowershell-interactive
Update-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Automatické zvětšování úložiště na serveru Azure Database for MariaDB pomocí prostředí PowerShell](howto-auto-grow-storage-powershell.md).
