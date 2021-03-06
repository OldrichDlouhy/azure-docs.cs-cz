---
title: Skript Azure CLI – škálování a monitorování Azure Database for PostgreSQL
description: Ukázkový skript Azure CLI – Škálování serveru Azure Database for PostgreSQL na jinou úroveň výkonu po dotazování metrik.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc
ms.topic: sample
ms.date: 08/07/2019
ms.openlocfilehash: 24aaf461576e6e043979660f9de968358763e003
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "68882985"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Monitorování a škálování jednoho serveru PostgreSQL pomocí Azure CLI
Tento ukázkový skript rozhraní příkazového řádku škáluje výpočetní výkon a úložiště pro jeden Azure Database for PostgreSQL Server po dotazování metrik. Výpočetní výkon se může škálovat nahoru nebo dolů. Velikost úložiště se dá škálovat jenom nahoru. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít verzi Azure CLI 2.0 nebo novější. Zkontrolujte verzi spuštěním příkazu `az --version`. Informace o instalaci nebo upgradu verze Azure CLI najdete v tématu [Instalace Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Ukázkový skript
Aktualizujte skript pomocí ID vašeho předplatného.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení
Po spuštění skriptu pomocí následujícího příkazu odeberte skupinu prostředků a všechny k ní přidružené prostředky. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Vysvětlení skriptu
Tento skript používá příkazy uvedené v následující tabulce:

| **Příkaz** | **Poznámky** |
|---|---|
| [az group create](/cli/azure/group) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [az postgres server create](/cli/azure/postgres/server#az-postgres-server-create) | Vytvoří server PostgreSQL, který je hostitelem databází. |
| [AZ Postgres Server Update](/cli/azure/postgres/server#az-postgres-server-update) | Aktualizuje vlastnosti serveru PostgreSQL. |
| [az monitor metrics list](/cli/azure/monitor/metrics) | Vypíše hodnotu metriky pro prostředky. |
| [az group delete](/cli/azure/group) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky
- Další informace o [Azure Database for PostgreSQL COMPUTE a úložišti](../concepts-pricing-tiers.md)
- Vyzkoušejte i další skripty: [Ukázky v Azure CLI pro službu Azure Database for PostgreSQL](../sample-scripts-azure-cli.md).
- Další informace o [Azure CLI](/cli/azure)
