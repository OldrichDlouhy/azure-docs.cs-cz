---
title: Kurz – použití Azure Key Vault s virtuálním počítačem v .NET | Microsoft Docs
description: V tomto kurzu nakonfigurujete aplikaci ASP.NET Core pro čtení tajného klíče z vašeho trezoru klíčů.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: e77701e17ef1b47aa6b8e3b8f2d10e93bf5e054e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101450"
---
# <a name="tutorial-use-azure-key-vault-with-a-virtual-machine-in-net"></a>Kurz: použití Azure Key Vault s virtuálním počítačem v .NET

Azure Key Vault pomáhá chránit tajné kódy, jako jsou klíče rozhraní API, databázové připojovací řetězce, které potřebujete pro přístup k aplikacím, službám a prostředkům IT.

V tomto kurzu se naučíte, jak získat konzolovou aplikaci pro čtení informací z Azure Key Vault. Aplikace by používala k ověření Key Vault spravovanou identitu virtuálního počítače. 

V tomto kurzu získáte informace o následujících postupech:

> [!div class="checklist"]
> * Vytvořte skupinu prostředků.
> * Vytvořte trezor klíčů.
> * Přidejte tajný klíč do trezoru klíčů.
> * Načtení tajného klíče z trezoru klíčů
> * Vytvořte virtuální počítač Azure.
> * Povolte [spravovanou identitu](../../active-directory/managed-identities-azure-resources/overview.md) pro virtuální počítač.
> * Přiřaďte oprávnění k identitě virtuálního počítače.

Než začnete, přečtěte si téma [Key Vault Basic koncepty](basic-concepts.md). 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Předpoklady

Pro Windows, Mac a Linux:
  * [Git](https://git-scm.com/downloads)
  * [.NET Core 3,1 SDK nebo novější](https://dotnet.microsoft.com/download/dotnet-core/3.1).
  * Rozhraní příkazového [řádku Azure](/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="create-resources-and-assign-permissions"></a>Vytváření prostředků a přiřazování oprávnění

Než začnete s psaním kódu, potřebujete vytvořit nějaké prostředky, vložit do trezoru klíčů tajný klíč a přiřadit oprávnění.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

Pokud se chcete přihlásit k Azure pomocí rozhraní příkazového řádku Azure, zadejte:

```azurecli
az login
```

### <a name="create-a-resource-group-and-key-vault"></a>Vytvoření skupiny prostředků a trezoru klíčů

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

### <a name="populate-your-key-vault-with-a-secret"></a>Naplnění trezoru klíčů pomocí tajného klíče

Nyní do trezoru klíčů přidejte tajný klíč pomocí příkazu [AZ Key trezor tajné sady](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) . Pokud chcete v trezoru klíčů s názvem **mySecret**vytvořit tajný klíč, zadejte následující příkaz:

```azurecli
az keyvault secret set --vault-name "<your-unique-key-vault-name>" --name "mySecret" --value "MySecret"
```

Tento tajný kód ukládá hodnotu **MySecret**.

### <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
Vytvořte virtuální počítač se systémem Windows nebo Linux pomocí jedné z následujících metod:

| Windows | Linux |
|--|--|
| [Azure CLI](../../virtual-machines/windows/quick-create-cli.md) | [Azure CLI](../../virtual-machines/linux/quick-create-cli.md) |  
| [PowerShell](../../virtual-machines/windows/quick-create-powershell.md) | [PowerShell](../../virtual-machines/linux/quick-create-powershell.md) |
| [Azure Portal](../../virtual-machines/windows/quick-create-portal.md) | [Azure Portal](../../virtual-machines/linux/quick-create-portal.md) |

### <a name="assign-an-identity-to-the-vm"></a>Přiřazení identity k virtuálnímu počítači
Vytvořte pro virtuální počítač identitu přiřazenou systémem pomocí příkazu [AZ VM identity Assign](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) :

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Poznamenejte si identitu přiřazenou systémem, která se zobrazí v následujícím kódu. Výstup předchozího příkazu by byl: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

### <a name="assign-permissions-to-the-vm-identity"></a>Přiřazení oprávnění k identitě virtuálního počítače
Přiřaďte dříve vytvořená oprávnění identity k trezoru klíčů pomocí příkazu [AZ Key trezor set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) :

```azurecli
az keyvault set-policy --name '<your-unique-key-vault-name>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Přihlaste se k virtuálnímu počítači.

Pokud se chcete přihlásit k virtuálnímu počítači, postupujte podle pokynů v tématu [připojení a přihlášení k virtuálnímu počítači Azure s Windows](../../virtual-machines/windows/connect-logon.md) nebo se [připojte k virtuálnímu počítači Azure s operačním systémem Linux a přihlaste se](../../virtual-machines/linux/login-using-aad.md).

## <a name="set-up-the-console-app"></a>Nastavení konzolové aplikace

Vytvořte konzolovou aplikaci a nainstalujte požadované balíčky pomocí `dotnet` příkazu.

### <a name="install-net-core"></a>Instalace .NET Core

Pokud chcete nainstalovat .NET Core, navštivte stránku [soubory ke stažení pro .NET](https://www.microsoft.com/net/download) .

### <a name="create-and-run-a-sample-net-app"></a>Vytvoření a spuštění ukázkové aplikace .NET

Otevřete příkazový řádek.

Hello World můžete do konzoly vytisknout spuštěním následujících příkazů:

```console
dotnet new console -n keyvault-console-app
cd keyvault-console-app
dotnet run
```

### <a name="install-the-package"></a>Instalace balíčku

V okně konzoly nainstalujte klientskou knihovnu Azure Key Vault tajných klíčů pro .NET:

```console
dotnet add package Azure.Security.KeyVault.Secrets
```

Pro tento rychlý Start budete muset nainstalovat následující balíček identity, aby se ověřilo Azure Key Vault:

```console
dotnet add package Azure.Identity
```

## <a name="edit-the-console-app"></a>Upravit konzolovou aplikaci

Otevřete soubor *program.cs* a přidejte tyto balíčky:

```csharp
using System;
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
```

Přidejte tyto řádky a aktualizujte identifikátor URI tak, aby odrážely `vaultUri` váš Trezor klíčů. Pod kódem používá [DefaultAzureCredential ()](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet) pro ověřování do trezoru klíčů, který používá token ze spravované identity aplikace k ověření. Používá se také exponenciální omezení rychlosti pro opakované pokusy v případě omezení trezoru klíčů.

```csharp
  class Program
    {
        static void Main(string[] args)
        {
            string secretName = "mySecret";

            var kvUri = "https://<your-key-vault-name>.vault.azure.net";
            SecretClientOptions options = new SecretClientOptions()
            {
                Retry =
                {
                    Delay= TimeSpan.FromSeconds(2),
                    MaxDelay = TimeSpan.FromSeconds(16),
                    MaxRetries = 5,
                    Mode = RetryMode.Exponential
                 }
            };

            var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential(),options);

            Console.Write("Input the value of your secret > ");
            string secretValue = Console.ReadLine();

            Console.Write("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");

            client.SetSecret(secretName, secretValue);

            Console.WriteLine(" done.");

            Console.WriteLine("Forgetting your secret.");
            secretValue = "";
            Console.WriteLine("Your secret is '" + secretValue + "'.");

            Console.WriteLine("Retrieving your secret from " + keyVaultName + ".");

            KeyVaultSecret secret = client.GetSecret(secretName);

            Console.WriteLine("Your secret is '" + secret.Value + "'.");

            Console.Write("Deleting your secret from " + keyVaultName + " ...");

            client.StartDeleteSecret(secretName);

            System.Threading.Thread.Sleep(5000);
            Console.WriteLine(" done.");

        }
    }
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, odstraňte virtuální počítač a trezor klíčů.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
