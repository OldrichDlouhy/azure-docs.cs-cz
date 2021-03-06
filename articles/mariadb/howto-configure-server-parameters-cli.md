---
title: Konfigurace parametrů serveru – Azure CLI – Azure Database for MariaDB
description: Tento článek popisuje, jak nakonfigurovat parametry služby v Azure Database for MariaDB pomocí nástroje příkazového řádku Azure CLI.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: how-to
ms.date: 6/11/2020
ms.openlocfilehash: a5aed10927a808d0002f765d493709e0e49483ff
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104805"
---
# <a name="configure-server-parameters-in-azure-database-for-mariadb-using-the-azure-cli"></a>Konfigurace parametrů serveru v Azure Database for MariaDB pomocí rozhraní příkazového řádku Azure
Pomocí Azure CLI, nástroje příkazového řádku Azure můžete vypsat, zobrazit a aktualizovat parametry konfigurace Azure Database for MariaDB serveru. Podmnožina konfigurací modulu se zveřejňuje na úrovni serveru a dá se upravit.

## <a name="prerequisites"></a>Požadavky
Pokud chcete projít tento průvodce, budete potřebovat:
- [Server Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md)
- Nástroj příkazového řádku [Azure CLI](/cli/azure/install-azure-cli) nebo použijte Azure Cloud Shell v prohlížeči.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Vypsat parametry konfigurace serveru pro Azure Database for MariaDB Server
Pokud chcete zobrazit seznam všech parametrů, které lze upravovat na serveru a jejich hodnotách, spusťte příkaz [AZ MariaDB Server Configuration list](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) .

Můžete uvést parametry konfigurace serveru pro server **mydemoserver.MariaDB.Database.Azure.com** v části Skupina prostředků **myresourcegroup**.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

Definice všech uvedených parametrů naleznete v části Reference MariaDB na [proměnných systému serveru](https://mariadb.com/kb/en/library/server-system-variables/).

## <a name="show-server-configuration-parameter-details"></a>Zobrazit podrobnosti parametru konfigurace serveru
Chcete-li zobrazit podrobnosti o konkrétním parametru konfigurace pro server, spusťte příkaz [AZ MariaDB Server Configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) .

Tento příklad ukazuje podrobnosti parametru konfigurace serveru **pomalého \_ dotazu \_ ** pro server **mydemoserver.MariaDB.Database.Azure.com** v části Skupina prostředků **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Změna hodnoty parametru konfigurace serveru
Můžete také změnit hodnotu určitého parametru konfigurace serveru, který aktualizuje základní konfigurační hodnotu pro modul MariaDB serveru. Chcete-li aktualizovat konfiguraci, použijte příkaz [AZ MariaDB Server Configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) . 

Pokud chcete aktualizovat parametr konfigurace serveru s **pomalým \_ dotazem \_ ** na serveru **mydemoserver.MariaDB.Database.Azure.com** v části Skupina prostředků **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Pokud chcete resetovat hodnotu parametru konfigurace, vynechejte volitelný `--value` parametr a služba použije výchozí hodnotu. V příkladu výše by vypadalo takto:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Tento kód resetuje konfiguraci ** \_ \_ protokolu pomalých dotazů** na výchozí hodnotu **vypnuto**. 

## <a name="setting-parameters-not-listed"></a>Nastavení neuvedených parametrů
Pokud parametr serveru, který chcete aktualizovat, není uveden v Azure Portal, můžete volitelně nastavit parametr na úrovni připojení pomocí `init_connect` . Tím se nastaví parametry serveru pro každého klienta, který se připojuje k serveru. 

Aktualizujte parametr konfigurace serveru **init \_ connect** serveru **mydemoserver.MariaDB.Database.Azure.com** v části Skupina prostředků **myresourcegroup** pro nastavení hodnot, jako je například znaková sada.
```azurecli-interactive
az mariadb server configuration set --name init_connect --resource-group myresourcegroup --server mydemoserver --value "SET character_set_client=utf8;SET character_set_database=utf8mb4;SET character_set_connection=latin1;SET character_set_results=latin1;"
```

## <a name="working-with-the-time-zone-parameter"></a>Práce s parametrem časového pásma

### <a name="populating-the-time-zone-tables"></a>Naplnění tabulek časových pásem

Tabulky časových pásem na vašem serveru můžou být vyplněné voláním `mysql.az_load_timezone` uložené procedury z nástroje, jako je příkazový řádek MariaDB nebo MariaDB Workbench.

> [!NOTE]
> Pokud spouštíte `mysql.az_load_timezone` příkaz z MariaDB Workbench, možná budete muset nejdřív vypnout režim bezpečné aktualizace pomocí `SET SQL_SAFE_UPDATES=0;` .

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Je nutné restartovat server, aby bylo zajištěno, že tabulky časových pásem budou správně vyplněny. K restartování serveru použijte [Azure Portal](howto-restart-server-portal.md) nebo [CLI](howto-restart-server-cli.md).

Chcete-li zobrazit dostupné hodnoty časového pásma, spusťte následující příkaz:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Nastavení časového pásma globální úrovně

Časové pásmo globální úrovně lze nastavit pomocí příkazu [AZ MariaDB Server Configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) .

Následující příkaz aktualizuje parametr konfigurace **serveru \_ časového pásma** serveru **mydemoserver.MariaDB.Database.Azure.com** ve skupině prostředků **myresourcegroup** na **US/Tichomoří**.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Nastavení časového pásma úrovně relace

Časové pásmo úrovně relace můžete nastavit spuštěním `SET time_zone` příkazu z nástroje, jako je příkazový řádek MariaDB nebo MariaDB Workbench. Následující příklad nastaví časové pásmo na časové pásmo **USA/Tichomoří** .  

```sql
SET time_zone = 'US/Pacific';
```

Informace o [funkcích data a času](https://mariadb.com/kb/en/library/date-time-functions/)najdete v dokumentaci k MariaDB.

## <a name="next-steps"></a>Další kroky

- Postup konfigurace [parametrů serveru v Azure Portal](howto-server-parameters.md)
