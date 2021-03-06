---
title: 'Rychlý Start: spuštění aplikace pružiny v jazyce Java pomocí rozhraní příkazového řádku Azure'
description: V tomto rychlém startu nasadíte ukázkovou aplikaci do služby Azure jaře Cloud v Azure CLI.
author: bmitchell287
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 02/15/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 64da0e62850433233c00f430b104121adc1979c1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87021507"
---
# <a name="quickstart-launch-a-java-spring-application-using-the-azure-cli"></a>Rychlý Start: spuštění aplikace pružiny v jazyce Java pomocí rozhraní příkazového řádku Azure

Jarní cloud Azure umožňuje snadno spustit aplikaci mikroslužeb založenou na jarním startu v Azure.

V tomto rychlém startu se dozvíte, jak nasadit stávající cloudovou aplikaci Java do Azure. Až budete hotovi, můžete pokračovat v správě aplikace prostřednictvím rozhraní příkazového řádku Azure nebo pomocí Azure Portal.

Po tomto rychlém startu se dozvíte, jak:

> [!div class="checklist"]
> * Zřízení instance služby
> * Nastavení konfiguračního serveru pro instanci
> * Místní sestavení aplikace mikroslužeb
> * Nasazení jednotlivých mikroslužeb
> * Přiřazení veřejného koncového bodu vaší aplikaci

## <a name="prerequisites"></a>Předpoklady

>[!Note]
> Jarní cloud Azure se teď nabízí jako verze Public Preview. Nabídky veřejné verze Preview umožňují zákazníkům experimentovat s novými funkcemi před jejich oficiální verzí.  Funkce a služby verze Public Preview nejsou určeny pro produkční použití.  Další informace o podpoře v rámci verzí Preview najdete v našich [nejčastějších dotazech](https://azure.microsoft.com/support/faq/) nebo v souboru o [support Request](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) , kde se dozvíte víc.

>[!TIP]
> Azure Cloud Shell je bezplatné interaktivní prostředí, které můžete použít k provedení kroků v tomto článku.  Má předinstalované běžné nástroje Azure, včetně nejnovějších verzí Git, JDK, Maven a Azure CLI. Pokud jste přihlášeni ke svému předplatnému Azure, spusťte [Azure Cloud Shell](https://shell.azure.com) z Shell.Azure.com.  Další informace o Azure Cloud Shell najdete v [naší dokumentaci](../cloud-shell/overview.md) .

K provedení kroků v tomto kurzu Rychlý start je potřeba:

1. [Nainstalovat Git](https://git-scm.com/).
2. [Nainstalovat JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Nainstalujte Maven 3,0 nebo novější.](https://maven.apache.org/download.cgi)
4. [Instalace rozhraní příkazového řádku Azure CLI 2.0.67 nebo vyšší verze](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Registrace předplatného Azure](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Instalace rozšíření Azure CLI

Pomocí následujícího příkazu nainstalujte rozšíření Azure jaře Cloud pro rozhraní příkazového řádku Azure.

```azurecli
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-on-the-azure-cli"></a>Zřízení instance služby v Azure CLI

1. Přihlaste se k Azure CLI a vyberte své aktivní předplatné. Nezapomeňte zvolit aktivní předplatné, které je na seznamu povolených pro Azure jaře Cloud.

    ```azurecli
        az login
        az account list -o table
        az account set --subscription <Name or ID of subscription from the last step>
    ```

2. Připravte si název služby pro jarní cloudovou službu Azure.  Název musí být dlouhý 4 až 32 znaků a může obsahovat jenom malá písmena, číslice a spojovníky.  První znak názvu služby musí být písmeno a poslední znak musí být písmeno nebo číslo.

3. Vytvořte skupinu prostředků, která bude obsahovat službu pro jarní cloudovou službu Azure.

    ```azurecli
        az group create --location eastus --name <resource group name>
    ```

    Další informace o [skupinách prostředků Azure](../azure-resource-manager/management/overview.md).

4. Otevřete okno Azure CLI a spusťte následující příkazy, abyste zřídili instanci Azure Pramenitého cloudu.

    ```azurecli
        az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    Nasazení instance služby bude trvat přibližně pět minut.

5. Pomocí následujících příkazů nastavte výchozí název skupiny prostředků a název clusteru:

    ```azurecli
        az configure --defaults group=<resource group name>
        az configure --defaults spring-cloud=<service instance name>
    ```

> [!div class="nextstepaction"]
> [Narazil(a) jsem na problém](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=provision)

## <a name="setup-your-configuration-server"></a>Nastavení konfiguračního serveru

Aktualizujte konfiguraci-server s umístěním úložiště Git pro náš projekt:

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/piggymetrics-config
```

> [!div class="nextstepaction"]
> [Narazil(a) jsem na problém](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=config-server)

## <a name="build-the-microservices-applications-locally"></a>Místní sestavení aplikací mikroslužeb

1. Vytvořte novou složku a naklonujte úložiště ukázkové aplikace do svého cloudového účtu Azure.  

    ```console
        mkdir source-code
        git clone https://github.com/Azure-Samples/piggymetrics
    ```

2. Změňte adresář a sestavte projekt.

    ```console
        cd piggymetrics
        mvn clean package -D skipTests
    ```

Kompilace projektu trvá přibližně 5 minut.  Po dokončení byste měli mít jednotlivé soubory JAR pro každou službu v příslušných složkách.

## <a name="create-the-microservices"></a>Vytvoření mikroslužeb

Vytvářejte jarní cloudové mikroslužby pomocí souborů JAR vytvořených v předchozím kroku. Vytvoříte tři mikroslužby: **Brána**, **auth-Service**a **account-Service**.

```azurecli
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

## <a name="deploy-applications-and-set-environment-variables"></a>Nasazení aplikací a nastavení proměnných prostředí

Musíme skutečně nasadit naše aplikace do Azure. K nasazení všech tří aplikací použijte následující příkazy:

```azurecli
az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar
az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar
az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar
```

> [!div class="nextstepaction"]
> [Narazil(a) jsem na problém](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=deploy)

## <a name="assign-public-endpoint-to-gateway"></a>Přiřazení veřejného koncového bodu k bráně

Potřebujeme způsob, jak získat přístup k aplikaci přes webový prohlížeč. Naše aplikace brány potřebuje veřejný koncový bod.

1. Přiřaďte koncový bod pomocí následujícího příkazu:

```azurecli
az spring-cloud app update -n gateway --is-public true
```

2. Dotazování aplikace **brány** na veřejnou IP adresu vám umožní ověřit, jestli je aplikace spuštěná:

```azurecli
az spring-cloud app show --name gateway --query properties.url
```

3. Pokud chcete spustit aplikaci PiggyMetrics, přejděte na adresu URL poskytnutou předchozím příkazem.
    ![Snímek obrazovky s PiggyMetrics spuštěným](media/spring-cloud-quickstart-launch-app-cli/launch-app.png)

Můžete také přejít na Azure Portal a najít tak adresu URL. 
1. Přejít ke službě
2. Vybrat **aplikace**
3. Vybrat **bránu**

    ![Snímek obrazovky s PiggyMetrics spuštěným](media/spring-cloud-quickstart-launch-app-cli/navigate-app1.png)
    
4. Najít adresu URL na stránce s **přehledem brány** ![ snímku obrazovky PiggyMetrics Running](media/spring-cloud-quickstart-launch-app-cli/navigate-app2-url.png)

> [!div class="nextstepaction"]
> [Narazil(a) jsem na problém](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste nasadili jarní cloudovou aplikaci z Azure CLI.  Další informace o jarním cloudu Azure najdete v kurzu Příprava aplikace na nasazení.

> [!div class="nextstepaction"]
> [Příprava aplikace pro jarní cloudy Azure pro nasazení](spring-cloud-tutorial-prepare-app-deployment.md)

Další ukázky jsou k dispozici na GitHubu: [ukázky Azure pro jarní Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/service-binding-cosmosdb-sql).
