---
title: Kurz – zřízení instance cloudového cloudu Azure pomocí terraformu
description: Zřízení instance cloudového cloudu Azure pomocí Terraformu
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 06/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 817a7c132657ba0ad8910a334b571f9d05481a0d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87021218"
---
# <a name="tutorial-provision-an-azure-spring-cloud-instance-with-terraform"></a>Kurz: zřízení instance služby jarního cloudu Azure pomocí Terraformu

V tomto kurzu se vytvoří instance cloudu Azure jaře pomocí Terraformu. Postupy vás provedou vytvořením následujících prostředků:

> [!div class="checklist"]
> * Skupina prostředků
> * Instance Azure jaře Cloud
> * Azure Storage pro Log Analytics

> [!NOTE]
> Pro podporu specifickou pro Terraformu použijte jeden z komunitních kanálů podpory HashiCorp na Terraformu:
>
> * Dotazy, případy použití a užitečné vzory: [část terraformu na portálu komunity HashiCorp](https://discuss.hashicorp.com/c/terraform-core)
> * Otázky související se zprostředkovatelem: [oddíl Terraformu Providers na portálu komunity HashiCorp](https://discuss.hashicorp.com/c/terraform-providers)

## <a name="prerequisites"></a>Předpoklady

- **Předplatné Azure:** Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) před tím, než začnete.

## <a name="create-configuration-file"></a>Vytvoření konfiguračního souboru

1. Přihlaste se na portál [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Otevřete [Azure Cloud Shell](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-java#use-azure-cloud-shell).

1. Spusťte Editor Cloud Shell:

    ```bash
    code main.tf
    ```

1. Konfigurace v tomto kroku modeluje prostředky Azure, včetně skupiny prostředků Azure a instance jarního cloudu Azure.

    ```hcl
    provider "azurerm" {
        features {}
    }

    resource "azurerm_resource_group" "example" {
      name     = "username"
      location = "eastus"
    }

    resource "azurerm_spring_cloud_service" "example" {
      name                = "usernamesp"
      resource_group_name = azurerm_resource_group.example.name
      location            = azurerm_resource_group.example.location

      config_server_git_setting {
        uri          = "https://github.com/Azure-Samples/piggymetrics-config"
        label        = "master"
        search_paths = ["."]
      }
    }
    ```

1. Uložte soubor (** &lt; CTRL>S**) a ukončete editor (** &lt; CTRL>Q**).

## <a name="apply-the-configuration"></a>Použít konfiguraci

V této části použijete několik příkazů Terraformu ke spuštění konfigurace.

1. Příkaz [terraformu init](https://www.terraform.io/docs/commands/init.html) inicializuje pracovní adresář. Spusťte následující příkaz v Cloud Shell:

    ```bash
    terraform init
    ```

1. K ověření syntaxe konfigurace se používá [plán příkazu terraformu](https://www.terraform.io/docs/commands/plan.html) . `-out`Parametr přesměruje výsledky do souboru. Výstupní soubor lze použít později pro použití konfigurace. Spusťte následující příkaz v Cloud Shell:

    ```bash
    terraform plan --out plan.out
    ```

1. Použití příkazu [terraformu](https://www.terraform.io/docs/commands/apply.html) se používá k aplikování konfigurace. Příkaz určuje výstupní soubor z předchozího kroku. Tento příkaz vytvoří prostředky Azure. Spusťte následující příkaz v Cloud Shell:

    ```bash
    terraform apply plan.out
    ```

1. Pokud chcete ověřit výsledky v rámci Azure Portal, přejděte do nové skupiny prostředků. Nová **instance Azure Cosmos DB** se zobrazí v nové skupině prostředků.

## <a name="update-configuration-to-config-logs-and-metrics"></a>Aktualizace konfigurace pro konfigurační protokoly a metriky

V této části se dozvíte, jak aktualizovat konfiguraci a povolit protokol a metriky pro jarní cloud Azure pomocí účtu Azure Storage.

1. Spusťte Editor Cloud Shell:

    ```bash
    code main.tf
    ```

1. Na konec souboru přidejte následující konfiguraci:

    ```hcl
    resource "azurerm_storage_account" "example" {
      name                     = "usernamest"
      resource_group_name      = azurerm_resource_group.example.name
      location                 = azurerm_resource_group.example.location
      account_tier             = "Standard"
      account_replication_type = "GRS"
    }

    resource "azurerm_monitor_diagnostic_setting" "example" {
      name               = "example"
      target_resource_id = "${azurerm_spring_cloud_service.example.id}"
      storage_account_id = "${azurerm_storage_account.example.id}"

      log {
        category = "SystemLogs"
        enabled  = true

        retention_policy {
          enabled = false
        }
      }

      metric {
        category = "AllMetrics"

        retention_policy {
          enabled = false
        }
      }
    }
    ```

1. Uložte soubor (** &lt; CTRL>S**) a ukončete editor (** &lt; CTRL>Q**).

1. Jak je uvedeno v předchozí části, spusťte následující příkaz, který provede změny:

    ```bash
    terraform plan --out plan.out
    ```

1. Spusťte `terraform apply` příkaz pro použití konfigurace.

    ```bash
    terraform apply plan.out
    ```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, odstraňte prostředky vytvořené v tomto článku.

Pokud chcete odebrat prostředky Azure vytvořené v tomto kurzu, spusťte příkaz [terraformu Destroy](https://www.terraform.io/docs/commands/destroy.html) :

```bash
terraform destroy -auto-approve
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Nainstalujte a nakonfigurujte terraformu pro zřizování prostředků Azure](https://docs.microsoft.com/azure/developer/terraform/getting-started-cloud-shell).