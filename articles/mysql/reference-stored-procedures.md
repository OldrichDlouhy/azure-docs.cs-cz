---
title: Uložené procedury správy – Azure Database for MySQL
description: Seznamte se s uloženými procedurami v Azure Database for MySQL, které vám pomůžou při konfiguraci replikace dat, nastavení časového pásma a dezaktivačních dotazů.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 6a3fa40eaae174d3616fd0318f81576b7c59eac7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80067701"
---
# <a name="azure-database-for-mysql-management-stored-procedures"></a>Uložené procedury správy Azure Database for MySQL

Uložené procedury jsou k dispozici na Azure Database for MySQL serverech, které vám pomůžou se správou serveru MySQL. To zahrnuje správu připojení, dotazů a nastavení Replikace vstupních dat serveru.  

## <a name="data-in-replication-stored-procedures"></a>Replikace vstupních dat uložené procedury

Replikace vstupních dat umožňuje synchronizovat data ze serveru MySQL spuštěného v místním prostředí, na virtuálních počítačích nebo v databázových službách hostovaných jinými poskytovateli cloudových služeb do služby Azure Database for MySQL.

Následující uložené procedury se používají k nastavení nebo odebrání Replikace vstupních dat mezi hlavním serverem a replikou.

|**Název uložené procedury**|**Vstupní parametry**|**Výstupní parametry**|**Poznámka k použití**|
|-----|-----|-----|-----|
|*MySQL. az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Není k dispozici|Chcete-li přenést data s režimem SSL, předejte kontext certifikátu certifikační autority do parametru master_ssl_ca. </br><br>Chcete-li přenést data bez protokolu SSL, předejte do parametru master_ssl_ca prázdný řetězec.|
|*MySQL. az_replication _start*|Není k dispozici|Není k dispozici|Spustí replikaci.|
|*MySQL. az_replication _stop*|Není k dispozici|Není k dispozici|Zastaví replikaci.|
|*MySQL. az_replication _remove_master*|Není k dispozici|Není k dispozici|Odebere vztah replikace mezi hlavním serverem a replikou.|
|*MySQL. az_replication_skip_counter*|Není k dispozici|Není k dispozici|Přeskočí jednu chybu replikace.|

Chcete-li nastavit Replikace vstupních dat mezi hlavním serverem a replikou v Azure Database for MySQL, přečtěte si téma [Postup konfigurace replikace vstupních dat](howto-data-in-replication.md).

## <a name="other-stored-procedures"></a>Jiné uložené procedury

Následující uložené procedury jsou k dispozici v Azure Database for MySQL ke správě serveru.

|**Název uložené procedury**|**Vstupní parametry**|**Výstupní parametry**|**Poznámka k použití**|
|-----|-----|-----|-----|
|*MySQL. az_kill*|processlist_id|Není k dispozici|Ekvivalent [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) příkazu Ukončí připojení přidružené k poskytnutému processlist_id po ukončení všech příkazů, které připojení provádí.|
|*MySQL. az_kill_query*|processlist_id|Není k dispozici|Ekvivalent [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) příkazu Ukončí příkaz, který připojení právě provádí. Ponechá připojení aktivní.|
|*MySQL. az_load_timezone*|Není k dispozici|Není k dispozici|Načte tabulky časového pásma, které umožňují `time_zone` nastavit parametr na pojmenované hodnoty (např. "US/Tichomoří").|

## <a name="next-steps"></a>Další kroky
- Naučte se nastavit [replikace vstupních dat](howto-data-in-replication.md)
- Naučte se používat [tabulky časových pásem](howto-server-parameters.md#working-with-the-time-zone-parameter) .
