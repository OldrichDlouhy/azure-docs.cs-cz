---
title: 'Rychlý Start: knihovna Azure Queue Storage V12 – .NET'
description: Naučte se používat knihovnu Azure V12 Library k vytvoření fronty a přidání zpráv do fronty. V dalším kroku se dozvíte, jak číst a odstraňovat zprávy z fronty. Naučíte se také, jak odstranit frontu.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 07/24/2020
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.openlocfilehash: 5931311368a6389386adb689a505feeeed03b29c
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289188"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-net"></a>Rychlý Start: Klientská knihovna Azure Queue Storage V12 pro .NET

Začněte s klientskou knihovnou Azure Queue Storage verze 12 pro .NET. Azure Queue Storage je služba pro ukládání velkého počtu zpráv pro pozdější načtení a zpracování. Postupujte podle těchto kroků a nainstalujte balíček a vyzkoušejte ukázkový kód pro základní úlohy.

Použijte klientskou knihovnu Azure Queue Storage V12 pro .NET pro:

* Vytvoření fronty
* Přidání zpráv do fronty
* Prohlížet zprávy ve frontě
* Aktualizace zprávy ve frontě
* Přijímání zpráv z fronty
* Odstranění zpráv z fronty
* Odstranění fronty

Další prostředky:

* [Referenční dokumentace k rozhraní API](/dotnet/api/azure.storage.queues)
* [Zdrojový kód knihovny](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues)
* [Balíček (NuGet)](https://www.nuget.org/packages/Azure.Storage.Queues/12.0.0)
* [ukázky](https://docs.microsoft.com/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure – [Vytvořte si ho zdarma](https://azure.microsoft.com/free/) .
* Účet úložiště Azure – [Vytvoření účtu úložiště](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Aktuální [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) pro váš operační systém. Ujistěte se, že jste získali sadu SDK a ne modul runtime.

## <a name="setting-up"></a>Nastavení

Tato část vás provede přípravou projektu pro práci s klientskou knihovnou Azure Queue Storage V12 for .NET.

### <a name="create-the-project"></a>Vytvoření projektu

Vytvořte aplikaci .NET Core s názvem *QueuesQuickstartV12*.

1. V okně konzoly (například cmd, PowerShell nebo bash) pomocí `dotnet new` příkazu vytvořte novou konzolovou aplikaci s názvem *QueuesQuickstartV12*. Tento příkaz vytvoří jednoduchý projekt C# "Hello World" s jedním zdrojovým souborem: *program.cs*.

   ```console
   dotnet new console -n QueuesQuickstartV12
   ```

1. Přepněte do nově vytvořeného adresáře *QueuesQuickstartV12* .

   ```console
   cd QueuesQuickstartV12
   ```

### <a name="install-the-package"></a>Instalace balíčku

Ještě pořád v adresáři aplikace nainstalujte klientskou knihovnu Azure Queue Storage pro balíček .NET pomocí `dotnet add package` příkazu.

```console
dotnet add package Azure.Storage.Queues
```

### <a name="set-up-the-app-framework"></a>Nastavení aplikační architektury

Z adresáře projektu:

1. V editoru otevřete soubor *program.cs*
1. Odebrat `Console.WriteLine("Hello World!");` příkaz
1. Přidat `using` direktivy
1. Aktualizujte `Main` deklaraci metody pro [podporu asynchronního kódu](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7-1#async-main) .



Zde je kód:

```csharp
using Azure;
using Azure.Storage.Queues;
using Azure.Storage.Queues.Models;
using System;
using System.Threading.Tasks;

namespace QueuesQuickstartV12
{
    class Program
    {
        static async Task Main(string[] args)
        {
        }
    }
}
```

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Objektový model

Azure Queue Storage je služba pro ukládání velkých objemů zpráv. Zpráva fronty může mít velikost až 64 KB. Fronta může obsahovat miliony zpráv až do celkového limitu kapacity účtu úložiště. Fronty se běžně používají k vytváření nevyřízených položek pro asynchronní zpracování. Queue Storage nabízí tři typy prostředků:

* Účet úložiště
* Fronta v účtu úložiště
* Zprávy ve frontě

Na následujícím diagramu jsou vztahy těchto prostředků.

![Diagram architektury služby Queue Storage](./media/storage-queues-introduction/queue1.png)

Pro interakci s těmito prostředky použijte následující třídy .NET:

* [QueueServiceClient](/dotnet/api/azure.storage.queues.queueserviceclient): `QueueServiceClient` umožňuje spravovat všechny fronty v účtu úložiště.
* [QueueClient](/dotnet/api/azure.storage.queues.queueclient): `QueueClient` Třída umožňuje spravovat a manipulovat s jednotlivou frontou a jejími zprávami.
* [QueueMessage](/dotnet/api/azure.storage.queues.models.queuemessage): `QueueMessage` Třída představuje jednotlivé objekty vracené při volání [ReceiveMessages](/dotnet/api/azure.storage.queues.queueclient.receivemessages) ve frontě.

## <a name="code-examples"></a>Příklady kódu

Tyto ukázkové fragmenty kódu ukazují, jak provést následující akce s klientskou knihovnou Azure Queue Storage pro .NET:

* [Získání připojovacího řetězce](#get-the-connection-string)
* [Vytvoření fronty](#create-a-queue)
* [Přidání zpráv do fronty](#add-messages-to-a-queue)
* [Prohlížet zprávy ve frontě](#peek-at-messages-in-a-queue)
* [Aktualizace zprávy ve frontě](#update-a-message-in-a-queue)
* [Přijímání zpráv z fronty](#receive-messages-from-a-queue)
* [Odstranění zpráv z fronty](#delete-messages-from-a-queue)
* [Odstranění fronty](#delete-a-queue)

### <a name="get-the-connection-string"></a>Získání připojovacího řetězce

Následující kód načte připojovací řetězec pro účet úložiště. Připojovací řetězec je uložený v proměnné prostředí vytvořené v oddílu [konfigurace vašeho připojovacího řetězce úložiště](#configure-your-storage-connection-string) .

Přidejte tento kód do `Main` metody:

```csharp
Console.WriteLine("Azure Queue storage v12 - .NET quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable called
// AZURE_STORAGE_CONNECTION_STRING on the machine running the application.
// If the environment variable is created after the application is launched
// in a console or with Visual Studio, the shell or application needs to be
// closed and reloaded to take the environment variable into account.
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
```

### <a name="create-a-queue"></a>Vytvoření fronty

Určete název nové fronty. Následující kód připojí hodnotu identifikátoru GUID k názvu fronty, aby bylo zajištěno, že je jedinečný.

> [!IMPORTANT]
> Názvy front můžou obsahovat jenom malá písmena, číslice a spojovníky a musí začínat písmenem nebo číslicí. Před i za každým spojovníkem musí být jiný znak než spojovník. Název musí mít také délku 3 až 63 znaků. Další informace o pojmenovávání front najdete v tématu [pojmenování front a metadat](/rest/api/storageservices/naming-queues-and-metadata).


Vytvořte instanci třídy [QueueClient](/dotnet/api/azure.storage.queues.queueclient) . Pak zavolejte metodu [CreateAsync](/dotnet/api/azure.storage.queues.queueclient.createasync) a vytvořte ve svém účtu úložiště frontu.

Přidejte tento kód na konec `Main` metody:

```csharp
// Create a unique name for the queue
string queueName = "quickstartqueues-" + Guid.NewGuid().ToString();

Console.WriteLine($"Creating queue: {queueName}");

// Instantiate a QueueClient which will be
// used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, queueName);

// Create the queue
await queueClient.CreateAsync();
```

### <a name="add-messages-to-a-queue"></a>Přidání zpráv do fronty

Následující fragment kódu asynchronně přidá zprávy do fronty voláním metody [SendMessageAsync](/dotnet/api/azure.storage.queues.queueclient.sendmessageasync) . Také ukládá [SendReceipt](/dotnet/api/azure.storage.queues.models.sendreceipt) vrácené `SendMessageAsync` voláním. Příjem se používá k aktualizaci zprávy později v programu.

Přidejte tento kód na konec `Main` metody:

```csharp
Console.WriteLine("\nAdding messages to the queue...");

// Send several messages to the queue
await queueClient.SendMessageAsync("First message");
await queueClient.SendMessageAsync("Second message");

// Save the receipt so we can update this message later
SendReceipt receipt = await queueClient.SendMessageAsync("Third message");
```

### <a name="peek-at-messages-in-a-queue"></a>Prohlížet zprávy ve frontě

Podívejte se na zprávy ve frontě voláním metody [PeekMessagesAsync](/dotnet/api/azure.storage.queues.queueclient.peekmessagesasync) . `PeekMessagesAsync`Metoda načítá jednu nebo více zpráv před frontou, ale nemění viditelnost zprávy.

Přidejte tento kód na konec `Main` metody:

```csharp
Console.WriteLine("\nPeek at the messages in the queue...");

// Peek at messages in the queue
PeekedMessage[] peekedMessages = await queueClient.PeekMessagesAsync(maxMessages: 10);

foreach (PeekedMessage peekedMessage in peekedMessages)
{
    // Display the message
    Console.WriteLine($"Message: {peekedMessage.MessageText}");
}
```

### <a name="update-a-message-in-a-queue"></a>Aktualizace zprávy ve frontě

Aktualizujte obsah zprávy voláním metody [UpdateMessageAsync](/dotnet/api/azure.storage.queues.queueclient.updatemessageasync) . `UpdateMessageAsync`Metoda může změnit časový limit viditelnosti zprávy a její obsah. Obsah zprávy musí být řetězec kódovaný v kódování UTF-8, který má velikost až 64 KB. Spolu s novým obsahem zprávy předejte hodnoty z rozhraní `SendReceipt` , které bylo uloženo dříve v kódu. `SendReceipt`Hodnoty identifikují, která zpráva se má aktualizovat.

```csharp
Console.WriteLine("\nUpdating the third message in the queue...");

// Update a message using the saved receipt from sending the message
await queueClient.UpdateMessageAsync(receipt.MessageId, receipt.PopReceipt, "Third message has been updated");
```

### <a name="receive-messages-from-a-queue"></a>Přijímání zpráv z fronty

Stáhněte dříve přidané zprávy voláním metody [ReceiveMessagesAsync](/dotnet/api/azure.storage.queues.queueclient.receivemessagesasync) .

Přidejte tento kód na konec `Main` metody:

```csharp
Console.WriteLine("\nReceiving messages from the queue...");

// Get messages from the queue
QueueMessage[] messages = await queueClient.ReceiveMessagesAsync(maxMessages: 10);
```

### <a name="delete-messages-from-a-queue"></a>Odstranění zpráv z fronty

Odstraní zprávy z fronty po jejich zpracování. V tomto případě se zpracovává jenom zpráva v konzole nástroje.

Aplikace pozastaví vstup uživatele voláním `Console.ReadLine` před jeho zpracováním a odstraněním zpráv. Před odstraněním ověřte ve svém [Azure Portal](https://portal.azure.com) , že se prostředky vytvořily správně. Všechny zprávy, které se explicitně neodstraní, se později stanou viditelnými ve frontě, aby se mohly zpracovat další šance na jejich zpracování.

Přidejte tento kód na konec `Main` metody:

```csharp
Console.WriteLine("\nPress Enter key to 'process' messages and delete them from the queue...");
Console.ReadLine();

// Process and delete messages from the queue
foreach (QueueMessage message in messages)
{
    // "Process" the message
    Console.WriteLine($"Message: {message.MessageText}");

    // Let the service know we're finished with
    // the message and it can be safely deleted.
    await queueClient.DeleteMessageAsync(message.MessageId, message.PopReceipt);
}
```

### <a name="delete-a-queue"></a>Odstranění fronty

Následující kód vyčistí prostředky, které aplikace vytvořila, odstraněním fronty pomocí metody [DeleteAsync](/dotnet/api/azure.storage.queues.queueclient.deleteasync) .

Přidejte tento kód na konec `Main` metody:

```csharp
Console.WriteLine("\nPress Enter key to delete the queue...");
Console.ReadLine();

// Clean up
Console.WriteLine($"Deleting queue: {queueClient.Name}");
await queueClient.DeleteAsync();

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>Spuštění kódu

Tato aplikace vytvoří a přidá tři zprávy do fronty Azure. Kód vypíše zprávy ve frontě a pak je načte a odstraní před tím, než se nakonec odstraní fronta.

V okně konzoly přejděte do adresáře aplikace a pak Sestavte a spusťte aplikaci.

```console
dotnet build
```

```console
dotnet run
```

Výstup aplikace je podobný následujícímu příkladu:

```output
Azure Queue storage v12 - .NET quickstart sample

Creating queue: quickstartqueues-5c72da2c-30cc-4f09-b05c-a95d9da52af2

Adding messages to the queue...

Peek at the messages in the queue...
Message: First message
Message: Second message
Message: Third message

Updating the third message in the queue...

Receiving messages from the queue...

Press Enter key to 'process' messages and delete them from the queue...

Message: First message
Message: Second message
Message: Third message has been updated

Press Enter key to delete the queue...

Deleting queue: quickstartqueues-5c72da2c-30cc-4f09-b05c-a95d9da52af2
Done
```

Když se aplikace před přijetím zpráv pozastaví, ověřte si účet úložiště v [Azure Portal](https://portal.azure.com). Ověřte, zda jsou zprávy ve frontě.

Stiskněte klávesu **ENTER** pro příjem a odstranění zpráv. Po zobrazení výzvy znovu stiskněte klávesu **ENTER** , aby se fronta odstranila, a dokončete ukázku.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak vytvořit frontu a přidat do ní zprávy pomocí asynchronního kódu .NET. Pak jste se naučili prohlížet, načítat a odstraňovat zprávy. Nakonec jste zjistili, jak odstranit frontu zpráv.

Kurzy, ukázky, rychlé starty a další dokumentace najdete na webu:

> [!div class="nextstepaction"]
> [Azure pro vývojáře na platformě .NET a .NET Core](https://docs.microsoft.com/dotnet/azure/)

* Další informace najdete v tématu [knihovny Azure Storage pro .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage).
* Pokud chcete zobrazit více ukázkových aplikací Azure Queue Storage, pokračujte do [v12ch ukázek klientské knihovny .NET pro Azure Queue Storage](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples).
* Další informace o .NET Core najdete v tématu [Začínáme s .NET během 10 minut](https://www.microsoft.com/net/learn/get-started/).
