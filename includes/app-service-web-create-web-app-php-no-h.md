---
title: zahrnout soubor
description: zahrnout soubor
services: app-service
author: msangapu-msft
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: 14b84c27140c7aebf83684d7c80dfbae5713850b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82085193"
---
Vytvořte [webovou aplikaci](../articles/app-service/containers/app-service-linux-intro.md) v plánu `myAppServicePlan` App Service. 

V Cloud Shell můžete použít [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) příkaz. V následujícím příkladu nahraďte `<app-name>` globálně jedinečným názvem aplikace (platné znaky jsou `a-z`, `0-9` a `-`). Modul runtime je nastavený na `PHP|7.3`. Pokud chcete zobrazit všechny podporované moduly runtime, [`az webapp list-runtimes --linux`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes)spusťte příkaz. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
```

Po vytvoření webové aplikace Azure CLI zobrazí výstup podobný následujícímu příkladu:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

Vytvořili jste novou prázdnou webovou aplikaci s povoleným nasazením Gitu.

> [!NOTE]
> Adresa URL vzdáleného úložiště Git se zobrazuje ve vlastnosti `deploymentLocalGitUrl` ve formátu `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Tuto adresu URL si uložte, protože ji budete potřebovat později.
>
