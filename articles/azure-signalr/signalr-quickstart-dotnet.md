---
title: Vývoj pomocí služby ASP.NET-Azure Signal Service
description: Rychlý Start pro použití služby signalizace Azure k vytvoření chatovací místnosti s ASP.NET Framework.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/20/2019
ms.author: zhshang
ms.openlocfilehash: ec5b7a75bced4b7cd81a120925558b8c1be57818
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "74158172"
---
# <a name="quickstart-create-a-chat-room-with-aspnet-and-signalr-service"></a>Rychlý Start: vytvoření chatovací místnosti pomocí služby ASP.NET and Signal Service

Služba signalizace Azure je založená na nástroji [Signal pro ASP.NET Core 2,0](https://docs.microsoft.com/aspnet/core/signalr/introduction), což není **100%** kompatibilní s nástrojem ASP.NET Signal. Služba signálů Azure znovu implementovala protokol dat signálu ASP.NET na základě nejnovějších technologií ASP.NET Core. Při použití služby signalizace Azure pro signál ASP.NET už některé funkce nástroje ASP.NET Signal nejsou podporované, například služba Azure Signal nehraje zprávy, když se klient znovu připojí. Také přenos snímků navždy a JSONP nejsou podporovány. Některé změny kódu a správnou verzi závislých knihoven jsou potřeba k tomu, aby aplikace ASP.NET signalizace fungovala se službou Signal. 

Úplný seznam porovnání funkcí mezi signálem ASP.NET a signálem ASP.NET Core naleznete v [dokumentu rozdíly v verzích](https://docs.microsoft.com/aspnet/core/signalr/version-differences?view=aspnetcore-2.2) .

V tomto rychlém startu se dozvíte, jak začít s ASP.NET a službou Azure Signaler pro podobnou [aplikaci chatovací místnosti](./signalr-quickstart-dotnet-core.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
* [.NET 4.6.1](https://www.microsoft.com/net/download/windows)
* [ASP.NET – signál 2.4.1](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu [Azure Portal](https://portal.azure.com/) pomocí svého účtu Azure.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

Pro aplikace signalizace ASP.NET se nepodporuje režim bez *serveru* . Pro instanci služby signalizace Azure vždy použijte *výchozí* nebo *klasický* .

Prostředky Azure používané v tomto rychlém startu můžete vytvořit také pomocí [skriptu vytvořit skript služby Signal](scripts/signalr-cli-create-service.md).

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Zatímco probíhá nasazování služby, pojďme se podívat na práci s kódem. Naklonujte [ukázkovou aplikaci z GitHubu](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom), nastavte připojovací řetězec služby SignalR Service a spusťte aplikaci místně.

1. Otevřete okno terminálu Git. Přejděte do složky, kam chcete klonovat ukázkový projekt.

1. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří na vašem počítači kopii ukázkové aplikace.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="configure-and-run-chat-room-web-app"></a>Konfigurace a spuštění webové aplikace chatovací místnosti

1. Spusťte sadu Visual Studio a otevřete řešení ve složce *ASPNET-Samples/ChatRoom/* Folder klonovaného úložiště.

1. V prohlížeči, kde je otevřený Azure Portal, najděte a vyberte instanci, kterou jste vytvořili.

1. Výběrem možnosti **Klíče** zobrazte připojovací řetězce instance služby SignalR.

1. Vyberte a zkopírujte primární připojovací řetězec.

1. Nyní nastavte připojovací řetězec v souboru Web. config.

    ```xml
    <configuration>
    <connectionStrings>
        <add name="Azure:SignalR:ConnectionString" connectionString="<Replace By Your Connection String>"/>
    </connectionStrings>
    ...
    </configuration>
    ```

1. V *Startup.cs*, namísto volání `MapSignalR()`, je třeba volat `MapAzureSignalR({your_applicationName})` a předávat připojovací řetězec, aby se aplikace připojovala ke službě namísto samotného hostitelského signálu. Nahraďte `{YourApplicationName}` názvem vaší aplikace. Tento název je jedinečný název, který rozlišuje tuto aplikaci od ostatních aplikací. Můžete použít `this.GetType().FullName` jako hodnotu.

    ```cs
    public void Configuration(IAppBuilder app)
    {
        // Any connection or hub wire up and configuration should go here
        app.MapAzureSignalR(this.GetType().FullName);
    }
    ```

    Před použitím těchto rozhraní API se také musíte odkazovat na sadu SDK služby. Otevřete **nástroje | Správce balíčků NuGet | Konzola správce balíčků** a příkaz spustit:

    ```powershell
    Install-Package Microsoft.Azure.SignalR.AspNet
    ```

    Kromě těchto změn zůstane vše ostatní, ale stále je možné používat rozhraní rozbočovače, které už znáte, a vytvořit obchodní logiku.

    > [!NOTE]
    > V implementaci je vystavený `/signalr/negotiate` koncový bod pro vyjednávání pomocí sady SDK služby Azure Signal. Při pokusu klienta o připojení a přesměrování klientů na koncový bod služby definovaný v připojovacím řetězci vrátí speciální odpověď na vyjednávání.

1. Stisknutím klávesy **F5** spusťte projekt v režimu ladění. Můžete vidět, že se aplikace spouští místně. Místo hostování modulu runtime signálu pomocí samotné aplikace se nyní připojí ke službě Azure Signal.

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]



> [!IMPORTANT]
> Odstranění skupiny prostředků je nevratné a skupina prostředků včetně všech v ní obsažených prostředků bude trvale odstraněna. Ujistěte se, že nechtěně neodstraníte nesprávnou skupinu prostředků nebo prostředky. Pokud jste vytvořili prostředky pro hostování této ukázky ve stávající skupině prostředků obsahující prostředky, které chcete zachovat, můžete místo odstranění skupiny prostředků odstranit jednotlivé prostředky z jejich odpovídajících oken.
> 
> 

Přihlaste se na web [Azure Portal ](https://portal.azure.com) a klikněte na **Skupiny prostředků**.

Do textového pole **Filtrovat podle názvu** zadejte název vaší skupiny prostředků. V pokynech v tomto rychlém startu se používala skupina prostředků *SignalRTestResources*. Ve výsledcích hledání klikněte na **...** u vaší skupiny prostředků a pak na **Odstranit skupinu prostředků**.

   
![Odstranit](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)

Po chvíli bude skupina prostředků včetně všech obsažených prostředků odstraněná.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili nový prostředek služby signalizace Azure a použili ho s webovou aplikací ASP.NET. V dalším kroku se naučíte vyvíjet aplikace v reálném čase pomocí služby Azure Signal Service pomocí ASP.NET Core.

> [!div class="nextstepaction"]
> [Služba signalizace Azure pomocí ASP.NET Core](./signalr-quickstart-dotnet-core.md)
