---
title: Ukázkový skript Azure CLI – Výpočet velikosti kontejneru objektů blob | Microsoft Docs
description: Vypočítejte velikost kontejneru v úložišti objektů blob v Azure součtem velikostí jednotlivých objektů blob v kontejneru.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: cli
ms.topic: sample
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: 83ec8c7b3bf5ba9d23d50d8fa1bce563dc56c135
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80067115"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Výpočet velikosti kontejneru úložiště objektů blob

Tento skript vypočítá velikost kontejneru v úložišti objektů blob v Azure součtem velikostí jednotlivých objektů blob v kontejneru.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Tento skript rozhraní příkazového řádku poskytuje odhadovanou velikost kontejneru a neměl by se používat k výpočtům fakturace.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Spuštěním následujícího příkazu odeberte skupinu prostředků, kontejner a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k výpočtu velikosti kontejneru úložiště objektů blob používá následující příkazy. Každá položka v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az group create](/cli/azure/group) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [az storage blob upload](/cli/azure/storage/account) | Nahraje místní soubory do kontejneru úložiště objektů blob v Azure. |
| [az storage blob list](/cli/azure/storage/account/keys) | Vypíše objekty blob v kontejneru úložiště objektů blob v Azure. |

## <a name="next-steps"></a>Další kroky

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](/cli/azure).

Další ukázkové skripty rozhraní příkazového řádku pro úložiště najdete v tématu [Ukázky v Azure CLI pro úložiště objektů blob v Azure](../blobs/storage-samples-blobs-cli.md).
