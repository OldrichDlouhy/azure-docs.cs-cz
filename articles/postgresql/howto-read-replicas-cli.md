---
title: Správa replik čtení – Azure CLI, REST API-Azure Database for PostgreSQL-Single server
description: Naučte se spravovat repliky pro čtení v Azure Database for PostgreSQL jednom serveru z Azure CLI a REST API
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: de74258cadcdf81da211561a84ff06927830e690
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274403"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli-rest-api"></a>Vytváření a Správa replik pro čtení z Azure CLI, REST API

V tomto článku se naučíte vytvářet a spravovat repliky pro čtení v Azure Database for PostgreSQL pomocí rozhraní příkazového řádku Azure a REST API. Další informace o replikách pro čtení najdete v tématu [Přehled](concepts-read-replicas.md).

## <a name="azure-replication-support"></a>Podpora replikace Azure
[Repliky čtení](concepts-read-replicas.md) a [logické dekódování](concepts-logical.md) závisí na protokolu Postgres Write předem log (WAL). Tyto dvě funkce vyžadují různé úrovně protokolování z Postgres. Logické dekódování potřebuje vyšší úroveň protokolování než repliky čtení.

Ke konfiguraci správné úrovně protokolování použijte parametr podpory replikace Azure. Podpora replikace Azure má tři možnosti nastavení:

* **Off** – vloží do Wal minimální informace. Toto nastavení není k dispozici na většině Azure Database for PostgreSQL serverů.  
* **Replika** – přesnější podrobnější informace než **vypnuto**. Toto je minimální úroveň protokolování potřebného pro fungování [replik pro čtení](concepts-read-replicas.md) . Toto nastavení je na většině serverů výchozí.
* **Logický** – další podrobnější informace než **replika** Toto je minimální úroveň protokolování pro logické dekódování, které funguje. V tomto nastavení fungují i repliky pro čtení.

Po změně tohoto parametru je nutné restartovat server. Interně tento parametr nastaví parametry Postgres `wal_level` , `max_replication_slots` a `max_wal_senders` .

## <a name="azure-cli"></a>Azure CLI
Repliky pro čtení můžete vytvořit a spravovat pomocí rozhraní příkazového řádku Azure CLI.

### <a name="prerequisites"></a>Požadavky

- [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Server Azure Database for PostgreSQL](quickstart-create-server-up-azure-cli.md) , který bude hlavním serverem.


### <a name="prepare-the-master-server"></a>Příprava hlavního serveru

1. Ověřte hodnotu hlavního serveru `azure.replication_support` . Měla by být aspoň REPLIKa, aby repliky pro čtení fungovaly.

   ```azurecli-interactive
   az postgres server configuration show --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support
   ```

2. Pokud `azure.replication_support` není aspoň replika, nastavte ji. 

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

3. Restartujte server, aby se změna projevila.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

### <a name="create-a-read-replica"></a>Vytvoření repliky pro čtení

Příkaz [AZ Postgres Server Replica Create](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) vyžaduje následující parametry:

| Nastavení | Příklad hodnoty | Popis  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Skupina prostředků, ve které se vytvoří server repliky.  |
| name | mydemoserver – replika | Název nového serveru repliky, který se vytvoří. |
| source-server | mydemoserver | Název nebo ID prostředku existujícího hlavního serveru, ze kterého se má replikovat. ID prostředku použijte v případě, že chcete, aby se skupiny prostředků repliky a hlavního serveru lišily. |

V následujícím příkladu rozhraní příkazového řádku je replika vytvořená ve stejné oblasti jako hlavní.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Chcete-li vytvořit repliku čtení ve více oblastech, použijte `--location` parametr. Níže uvedený příklad rozhraní příkazového řádku vytvoří repliku v Západní USA.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Další informace o tom, které oblasti můžete vytvořit repliku v, najdete v [článku věnovaném konceptům pro čtení replik](concepts-read-replicas.md). 

Pokud jste nenastavili `azure.replication_support` parametr na **repliku** na pro obecné účely nebo paměťově optimalizovaném hlavním serveru a server restartovali, zobrazí se chyba. Před vytvořením repliky tyto dva kroky proveďte.

> [!IMPORTANT]
> Přečtěte si [část s informacemi v tématu Přehled repliky čtení](concepts-read-replicas.md#considerations).
>
> Než bude nastavení hlavního serveru aktualizováno na novou hodnotu, aktualizujte nastavení repliky na hodnotu rovná se nebo větší. Tato akce pomůže replice uchovávat všechny změny provedené v hlavní větvi.

### <a name="list-replicas"></a>Vypsat repliky
Seznam replik hlavního serveru můžete zobrazit pomocí příkazu [AZ Postgres Server Replica list](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) .

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

### <a name="stop-replication-to-a-replica-server"></a>Zastavení replikace na server repliky
Replikaci mezi hlavním serverem a replikou pro čtení můžete zastavit pomocí příkazu [AZ Postgres Server Replica stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) .

Po zastavení replikace na hlavní server a repliku pro čtení ji nejde vrátit zpět. Replika čtení se stal samostatným serverem, který podporuje čtení i zápis. Samostatný server se nedá znovu vytvořit do repliky.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

### <a name="delete-a-master-or-replica-server"></a>Odstranění hlavního serveru nebo serveru repliky
Pokud chcete odstranit hlavní server nebo server repliky, použijte příkaz [AZ Postgres Server Delete](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) .

Při odstranění hlavního serveru se zastaví replikace do všech replik čtení. Repliky čtení se stanou samostatnými servery, které nyní podporují čtení i zápis.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="rest-api"></a>REST API
Repliky pro čtení můžete vytvářet a spravovat pomocí [REST API Azure](/rest/api/azure/).

### <a name="prepare-the-master-server"></a>Příprava hlavního serveru

1. Ověřte hodnotu hlavního serveru `azure.replication_support` . Měla by být aspoň REPLIKa, aby repliky pro čtení fungovaly.

   ```http
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

2. Pokud `azure.replication_support` není aspoň replika, nastavte ji.

   ```http
   PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

   ```JSON
   {
    "properties": {
    "value": "replica"
    }
   }
   ```

2. [Restartujte server](/rest/api/postgresql/servers/restart) , aby se změna projevila.

   ```http
   POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/restart?api-version=2017-12-01
   ```

### <a name="create-a-read-replica"></a>Vytvoření repliky pro čtení
Repliku pro čtení můžete vytvořit pomocí [rozhraní API pro vytvoření](/rest/api/postgresql/servers/create):

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}"
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
Seznam replik hlavního serveru můžete zobrazit pomocí [rozhraní API seznamu replik](/rest/api/postgresql/replicas/listbyserver):

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Zastavení replikace na server repliky
Replikaci mezi hlavním serverem a replikou pro čtení můžete zastavit pomocí [rozhraní API pro aktualizaci](/rest/api/postgresql/servers/update).

Po zastavení replikace na hlavní server a repliku pro čtení ji nejde vrátit zpět. Replika čtení se stal samostatným serverem, který podporuje čtení i zápis. Samostatný server se nedá znovu vytvořit do repliky.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Odstranění hlavního serveru nebo serveru repliky
K odstranění hlavního serveru nebo serveru repliky použijte [rozhraní API pro odstranění](/rest/api/postgresql/servers/delete):

Při odstranění hlavního serveru se zastaví replikace do všech replik čtení. Repliky čtení se stanou samostatnými servery, které nyní podporují čtení i zápis.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{serverName}?api-version=2017-12-01
```

## <a name="next-steps"></a>Další kroky
* Přečtěte si další informace o [replikách pro čtení v Azure Database for PostgreSQL](concepts-read-replicas.md).
* Naučte se [vytvářet a spravovat repliky pro čtení v Azure Portal](howto-read-replicas-portal.md).
