---
title: 'Azure CLI: monitorování a škálování izolované databáze v Azure SQL Database'
description: Pomocí ukázkového skriptu Azure CLI můžete monitorovat a škálovat izolovanou databázi v Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: bd2f8005300aeb77a88be2609246f2d760154e36
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2020
ms.locfileid: "86231994"
---
# <a name="use-the-azure-cli-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>Pomocí Azure CLI můžete monitorovat a škálovat izolovanou databázi v Azure SQL Database

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Tento ukázkový skript Azure CLI po dotazování na informace o velikosti databáze škáluje izolovanou databázi v Azure SQL Database na jinou výpočetní velikost.

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku Azure CLI místně, musíte mít Azure CLI verze 2,0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace rozhraní příkazového řádku Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Ukázkový skript

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Spuštění skriptu

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale a database in Azure SQL Database")]

> [!TIP]
> Pomocí funkce [AZ SQL DB op list](/cli/azure/sql/db/op?#az-sql-db-op-list) získáte seznam operací provedených v databázi a pomocí funkce [AZ SQL DB op Cancel](/cli/azure/sql/db/op#az-sql-db-op-cancel) zrušíte operaci aktualizace databáze.

### <a name="clean-up-deployment"></a>Vyčištění nasazení

Pomocí následujícího příkazu odeberte skupinu prostředků a všechny k ní přidružené prostředky.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Vzorový odkaz

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Skript | Description |
|---|---|
| [az sql server](/cli/azure/sql/server) | Příkazy serveru. |
| [az sql db show-usage](/cli/azure/sql#az-sql-show-usage) | Zobrazí informace o využití velikosti databáze. |

## <a name="next-steps"></a>Další kroky

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](/cli/azure).

Další ukázkové skripty rozhraní příkazového řádku najdete v [ukázkových skriptech Azure CLI](../az-cli-script-samples-content-guide.md).
