---
title: Kurz – připojení k clusteru Azure Red Hat OpenShift 4
description: Naučte se připojit cluster Microsoft Azure Red Hat OpenShift
author: sakthi-vetrivel
ms.author: suvetriv
ms.topic: tutorial
ms.service: container-service
ms.date: 04/24/2020
ms.openlocfilehash: d7efe781f1ba2beb1fa7dd4fdaaad280fc789de2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82204383"
---
# <a name="tutorial-connect-to-an-azure-red-hat-openshift-4-cluster"></a>Kurz: připojení k clusteru Azure Red Hat OpenShift 4

V tomto kurzu, který je třetí částí ze tří, se připojíte ke clusteru Azure Red Hat OpenShift se systémem OpenShift 4 jako uživatel kubeadmin prostřednictvím webové konzoly OpenShift. Získáte informace o těchto tématech:
> [!div class="checklist"]
> * Získání `kubeadmin` přihlašovacích údajů pro cluster
> * Instalace rozhraní příkazového řádku OpenShift
> * Připojení ke clusteru Azure Red Hat OpenShift pomocí rozhraní příkazového řádku OpenShift

## <a name="before-you-begin"></a>Před zahájením

V předchozích kurzech se vytvořil cluster Azure Red Hat OpenShift. Pokud jste tyto kroky neudělali a chcete je sledovat, začněte s [kurzem 1 – Vytvoření clusteru Azure Red Hat OpenShift 4.](tutorial-create-cluster.md)

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít spuštěnou verzi Azure CLI 2.0.75 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="connect-to-the-cluster"></a>Připojení ke clusteru

K tomuto clusteru se můžete přihlásit pomocí `kubeadmin` uživatele.  Spusťte následující příkaz a vyhledejte heslo pro `kubeadmin` uživatele.

```azurecli-interactive
az aro list-credentials \
  --name $CLUSTER \
  --resource-group $RESOURCEGROUP
```

Následující příklad výstupu ukazuje, že heslo bude v `kubeadminPassword`.

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Adresu URL konzoly clusteru můžete najít spuštěním následujícího příkazu, který bude vypadat nějak takto:`https://console-openshift-console.apps.<random>.<region>.aroapp.io/`

```azurecli-interactive
 az aro show \
    --name $CLUSTER \
    --resource-group $RESOURCEGROUP \
    --query "consoleProfile.url" -o tsv
```

V prohlížeči spusťte adresu URL konzoly a přihlaste se `kubeadmin` pomocí přihlašovacích údajů.

![Přihlašovací obrazovka Azure Red Hat OpenShift](media/aro4-login.png)

## <a name="install-the-openshift-cli"></a>Instalace rozhraní příkazového řádku OpenShift

Po přihlášení ke webové konzole OpenShift klikněte na **?** v pravém horním rohu a pak na **nástrojích příkazového řádku**. Stáhněte si verzi vhodnou pro váš počítač.

![Přihlašovací obrazovka Azure Red Hat OpenShift](media/aro4-download-cli.png)

Můžete si také stáhnout nejnovější verzi rozhraní příkazového řádku, která je vhodná pro <https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/>váš počítač.

Pokud spouštíte příkazy na Azure Cloud Shell, Stáhněte si nejnovější rozhraní příkazového řádku OpenShift 4 pro Linux.

```azurecli-interactive
cd ~
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz

mkdir openshift
tar -zxvf openshift-client-linux.tar.gz -C openshift
echo 'export PATH=$PATH:~/openshift' >> ~/.bashrc && source ~/.bashrc
```

## <a name="connect-using-the-openshift-cli"></a>Připojení pomocí rozhraní příkazového řádku OpenShift

Načtěte adresu serveru rozhraní API.

```azurecli-interactive
apiServer=$(az aro show -g $RESOURCEGROUP -n $CLUSTER --query apiserverProfile.url -o tsv)
```

Přihlaste se k serveru rozhraní API OpenShift clusteru pomocí následujícího příkazu. Nahraďte ** \<kubeadmin hesla>** heslem, které jste právě načetli.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

## <a name="next-steps"></a>Další kroky

V této části kurzu jste se naučili:
> [!div class="checklist"]
> * Získání `kubeadmin` přihlašovacích údajů pro cluster
> * Instalace rozhraní příkazového řádku OpenShift
> * Připojení ke clusteru Azure Red Hat OpenShift pomocí rozhraní příkazového řádku OpenShift

Přejděte k dalšímu kurzu:
> [!div class="nextstepaction"]
> [Odstranění clusteru Azure Red Hat OpenShift](tutorial-delete-cluster.md)