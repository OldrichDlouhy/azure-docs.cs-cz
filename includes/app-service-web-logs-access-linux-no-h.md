---
title: zahrnout soubor
description: zahrnout soubor
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 0dd6618bdee8e6810d414d4b04b16a1e0a9c90ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905613"
---
Můžete přistupovat k protokolům konzoly generovaným zevnitř kontejneru. Nejprve zapněte protokolování kontejneru spuštěním následujícího příkazu v Cloud Shell:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Jakmile je protokolování kontejneru zapnuté, spusťte následující příkaz, který zobrazí datový proud protokolu:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Pokud nevidíte protokoly konzoly okamžitě, podívejte se znovu za 30 sekund.

> [!NOTE]
> Soubory protokolu můžete také zkontrolovat v prohlížeči na adrese `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .

Pokud chcete streamování protokolů kdykoli zastavit, zadejte `Ctrl` + `C` .
