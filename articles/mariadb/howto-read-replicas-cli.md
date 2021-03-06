---
title: Správa replik čtení – Azure CLI, REST API-Azure Database for MariaDB
description: Tento článek popisuje, jak nastavit a spravovat repliky pro čtení v Azure Database for MariaDB pomocí rozhraní příkazového řádku Azure a REST API.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/10/2020
ms.openlocfilehash: aff8eb27b1488f06edbc3ebd8c91b0a777837f91
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121108"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-cli-and-rest-api"></a>Vytvoření a Správa replik pro čtení v Azure Database for MariaDB pomocí rozhraní příkazového řádku Azure a REST API

V tomto článku se naučíte, jak vytvářet a spravovat repliky pro čtení ve službě Azure Database for MariaDB pomocí rozhraní příkazového řádku Azure a REST API.

## <a name="azure-cli"></a>Azure CLI
Repliky pro čtení můžete vytvořit a spravovat pomocí rozhraní příkazového řádku Azure CLI.

### <a name="prerequisites"></a>Požadavky

- [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Server Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md) , který se bude používat jako hlavní server. 

> [!IMPORTANT]
> Funkce replika čtení je k dispozici pouze pro Azure Database for MariaDB servery v cenové úrovni optimalizované pro Pro obecné účely nebo paměť. Ujistěte se, že je hlavní server v jedné z těchto cenových úrovní.

### <a name="create-a-read-replica"></a>Vytvoření repliky pro čtení

> [!IMPORTANT]
> Když vytvoříte repliku pro hlavní server, který nemá žádné existující repliky, hlavní počítač se nejprve restartuje a připraví se pro replikaci. Vezměte v úvahu a udělejte tyto operace v době mimo špičku.

Server repliky pro čtení se dá vytvořit pomocí následujícího příkazu:

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica create`Příkaz vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Skupina prostředků, do které se vytvoří server repliky.  |
| name | mydemoreplicaserver | Název nového serveru repliky, který se vytvoří. |
| source-server | mydemoserver | Název nebo ID existujícího hlavního serveru, ze kterého se má replikovat. |

Chcete-li vytvořit repliku čtení ve více oblastech, použijte `--location` parametr. 

Níže uvedený příklad rozhraní příkazového řádku vytvoří repliku v Západní USA.

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Další informace o tom, které oblasti můžete vytvořit repliku v, najdete v [článku věnovaném konceptům pro čtení replik](concepts-read-replicas.md). 

> [!NOTE]
> Repliky čtení se vytvářejí se stejnou konfigurací serveru jako hlavní. Konfiguraci serveru repliky je možné po vytvoření změnit. Doporučuje se udržovat konfiguraci serveru repliky ve stejné nebo větší hodnotě než hlavní, aby bylo zajištěno, že je replika schopná s hlavní hodnotou.

### <a name="list-replicas-for-a-master-server"></a>Vypíše repliky pro hlavní server.

Chcete-li zobrazit všechny repliky pro daný hlavní server, spusťte následující příkaz: 

```azurecli-interactive
az mariadb server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica list`Příkaz vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Skupina prostředků, do které se vytvoří server repliky.  |
| název-serveru | mydemoserver | Název nebo ID hlavního serveru. |

### <a name="stop-replication-to-a-replica-server"></a>Zastavení replikace na server repliky

> [!IMPORTANT]
> Zastavení replikace na serveru je nevratné. Po zastavení replikace mezi hlavním serverem a replikou nelze vrátit zpět. Server repliky se pak stal samostatným serverem a teď podporuje čtení i zápis. Tento server nelze znovu vytvořit do repliky.

Replikaci na server repliky pro čtení lze zastavit pomocí následujícího příkazu:

```azurecli-interactive
az mariadb server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mariadb server replica stop`Příkaz vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Skupina prostředků, ve které existuje server repliky.  |
| name | mydemoreplicaserver | Název serveru repliky, na kterém má být replikace zastavena. |

### <a name="delete-a-replica-server"></a>Odstranění serveru repliky

Odstranění serveru repliky pro čtení se dá provést spuštěním příkazu **[AZ MariaDB Server Delete](/cli/azure/mariadb/server)** .

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-master-server"></a>Odstranění hlavního serveru

> [!IMPORTANT]
> Odstraněním hlavního serveru se zastaví replikace na všechny servery replik a odstraní se samotný hlavní server. Ze serverů replik se stanou samostatné servery, které teď podporují čtení i zápis.

Pokud chcete odstranit hlavní server, můžete spustit příkaz **[AZ MariaDB Server Delete](/cli/azure/mariadb/server)** .

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="rest-api"></a>Rozhraní REST API
Repliky pro čtení můžete vytvářet a spravovat pomocí [REST API Azure](/rest/api/azure/).

### <a name="create-a-read-replica"></a>Vytvoření repliky pro čtení
Repliku pro čtení můžete vytvořit pomocí [rozhraní API pro vytvoření](/rest/api/mariadb/servers/create):

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> Další informace o tom, které oblasti můžete vytvořit repliku v, najdete v [článku věnovaném konceptům pro čtení replik](concepts-read-replicas.md). 

Pokud jste nenastavili `azure.replication_support` parametr na **repliku** na pro obecné účely nebo paměťově optimalizovaném hlavním serveru a server restartovali, zobrazí se chyba. Před vytvořením repliky tyto dva kroky proveďte.

Replika se vytvoří pomocí stejného nastavení výpočtů a úložiště jako hlavní. Po vytvoření repliky se dá několik nastavení měnit nezávisle na hlavním serveru: generování výpočetních prostředků, virtuální jádra, úložiště a doba uchovávání záloh. Cenová úroveň se dá změnit také nezávisle, s výjimkou nebo z úrovně Basic.


> [!IMPORTANT]
> Než bude nastavení hlavního serveru aktualizováno na novou hodnotu, aktualizujte nastavení repliky na hodnotu rovná se nebo větší. Tato akce pomůže replice uchovávat všechny změny provedené v hlavní větvi.

### <a name="list-replicas"></a>Vypsat repliky
Seznam replik hlavního serveru můžete zobrazit pomocí [rozhraní API seznamu replik](/rest/api/mariadb/replicas/listbyserver):

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Zastavení replikace na server repliky
Replikaci mezi hlavním serverem a replikou pro čtení můžete zastavit pomocí [rozhraní API pro aktualizaci](/rest/api/mariadb/servers/update).

Po zastavení replikace na hlavní server a repliku pro čtení ji nejde vrátit zpět. Replika čtení se stal samostatným serverem, který podporuje čtení i zápis. Samostatný server se nedá znovu vytvořit do repliky.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Odstranění hlavního serveru nebo serveru repliky
K odstranění hlavního serveru nebo serveru repliky použijte [rozhraní API pro odstranění](/rest/api/mariadb/servers/delete):

Při odstranění hlavního serveru se zastaví replikace do všech replik čtení. Repliky čtení se stanou samostatnými servery, které nyní podporují čtení i zápis.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>Další kroky

- Další informace o [replikách pro čtení](concepts-read-replicas.md)
