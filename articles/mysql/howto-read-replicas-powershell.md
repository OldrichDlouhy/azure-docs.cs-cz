---
title: Spravovat repliky čtení-Azure PowerShell-Azure Database for MySQL
description: Naučte se, jak nastavit a spravovat repliky pro čtení v Azure Database for MySQL pomocí PowerShellu.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 6/10/2020
ms.openlocfilehash: f6d24ba0d31020b82669947189da180348f2a46b
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86107984"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-powershell"></a>Jak vytvářet a spravovat repliky pro čtení v Azure Database for MySQL pomocí prostředí PowerShell

V tomto článku se naučíte vytvářet a spravovat repliky pro čtení ve službě Azure Database for MySQL pomocí prostředí PowerShell. Další informace o replikách pro čtení najdete v tématu [Přehled](concepts-read-replicas.md).

## <a name="azure-powershell"></a>Azure PowerShell

Pomocí PowerShellu můžete vytvářet a spravovat repliky pro čtení.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto průvodce budete potřebovat:

- [Modul AZ PowerShell](/powershell/azure/install-az-ps) nainstalovaný místně nebo [Azure Cloud Shell](https://shell.azure.com/) v prohlížeči
- [Server Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> I když je modul PowerShell AZ. MySql ve verzi Preview, musíte ho nainstalovat samostatně z modulu AZ PowerShellu pomocí následujícího příkazu: `Install-Module -Name Az.MySql -AllowPrerelease` .
> Jakmile je modul PowerShellu AZ. MySql všeobecně dostupný, bude součástí budoucna k budoucímu AZ PowerShell Release releases a k dispozici nativně z Azure Cloud Shell.

Pokud se rozhodnete použít prostředí PowerShell místně, připojte se k účtu Azure pomocí rutiny [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Funkce replika čtení je k dispozici pouze pro Azure Database for MySQL servery v cenové úrovni optimalizované pro Pro obecné účely nebo paměť. Ujistěte se, že je hlavní server v jedné z těchto cenových úrovní.

### <a name="create-a-read-replica"></a>Vytvoření repliky pro čtení

> [!IMPORTANT]
> Když vytvoříte repliku pro hlavní server, který nemá žádné existující repliky, hlavní počítač se nejprve restartuje a připraví se pro replikaci. Vezměte v úvahu a udělejte tyto operace v době mimo špičku.

Server repliky pro čtení se dá vytvořit pomocí následujícího příkazu:

```azurepowershell-interactive
Get-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

`New-AzMySqlServerReplica`Příkaz vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Description  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Skupina prostředků, ve které se vytvoří server repliky.  |
| Name | mydemoreplicaserver | Název nového serveru repliky, který se vytvoří. |

Pokud chcete vytvořit repliku čtení ve více oblastech, použijte parametr **Location (umístění** ). V následujícím příkladu se vytvoří replika v oblasti **západní USA** .

```azurepowershell-interactive
Get-AzMySqlServer -Name mrdemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup -Location westus
```

Další informace o tom, které oblasti můžete vytvořit repliku v, najdete v [článku věnovaném konceptům pro čtení replik](concepts-read-replicas.md).

Ve výchozím nastavení se repliky čtení vytvoří se stejnou konfigurací serveru jako hlavní server, pokud není zadaný parametr **SKU** .

> [!NOTE]
> Doporučuje se udržovat konfiguraci serveru repliky ve stejné nebo větší hodnotě než hlavní, aby bylo zajištěno, že je replika schopná s hlavní hodnotou.

### <a name="list-replicas-for-a-master-server"></a>Vypíše repliky pro hlavní server.

Chcete-li zobrazit všechny repliky pro daný hlavní server, spusťte následující příkaz:

```azurepowershell-interactive
Get-AzMySqlReplica -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

`Get-AzMySqlReplica`Příkaz vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Description  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Skupina prostředků, do které se vytvoří server repliky.  |
| ServerName | mydemoserver | Název nebo ID hlavního serveru. |

### <a name="delete-a-replica-server"></a>Odstranění serveru repliky

Odstranění serveru repliky pro čtení se dá provést spuštěním `Remove-AzMySqlServer` rutiny.

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

### <a name="delete-a-master-server"></a>Odstranění hlavního serveru

> [!IMPORTANT]
> Odstraněním hlavního serveru se zastaví replikace na všechny servery replik a odstraní se samotný hlavní server. Ze serverů replik se stanou samostatné servery, které teď podporují čtení i zápis.

Pokud chcete odstranit hlavní server, můžete spustit `Remove-AzMySqlServer` rutinu.

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Restartování serveru Azure Database for MySQL pomocí prostředí PowerShell](howto-restart-server-powershell.md)
