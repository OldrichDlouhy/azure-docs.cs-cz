---
title: Konfigurace aplikací pro Linux ASP.NET Core
description: Naučte se konfigurovat předem sestavený ASP.NET Core kontejner pro vaši aplikaci. Tento článek ukazuje nejběžnější konfigurační úlohy.
ms.devlang: dotnet
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: e009f5b1fc656f700b3f0e76dda6e545aed535d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905761"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Konfigurace aplikace ASP.NET Core pro Linux pro Azure App Service

ASP.NET Core aplikace musí být nasazeny jako zkompilované binární soubory. Nástroj pro publikování sady Visual Studio sestaví řešení a potom nasadí zkompilované binární soubory přímo, zatímco modul nasazení App Service nasadí úložiště kódu jako první a potom zkompiluje binární soubory.

Tato příručka poskytuje klíčové koncepty a pokyny pro ASP.NET Core vývojářů, kteří používají integrovaný kontejner Linux v App Service. Pokud jste Azure App Service nikdy nepoužili, postupujte jako první v kurzu [ASP.NET Core rychlý Start](quickstart-dotnetcore.md) a [ASP.NET Core SQL Database](tutorial-dotnetcore-sqldb-app.md) .

## <a name="show-net-core-version"></a>Zobrazit verzi .NET Core

Chcete-li zobrazit aktuální verzi rozhraní .NET Core, spusťte následující příkaz v [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Pokud chcete zobrazit všechny podporované verze .NET Core, spusťte v [Cloud Shell](https://shell.azure.com)následující příkaz:

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>Nastavit verzi .NET Core

Spuštěním následujícího příkazu v [Cloud Shell](https://shell.azure.com) nastavte verzi .NET Core na 2,1:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="customize-build-automation"></a>Přizpůsobení automatizace sestavení

Pokud nasadíte aplikaci s použitím balíčků Git nebo zip se zapnutou možností automatizace sestavení, App Service sestavování kroků automatizace pomocí následujícího postupu:

1. Spusťte vlastní skript, pokud je určen `PRE_BUILD_SCRIPT_PATH` .
1. Spusťte `dotnet restore` pro obnovení závislostí NuGet.
1. Spusťte příkaz `dotnet publish` pro sestavení binárního souboru pro produkční prostředí.
1. Spusťte vlastní skript, pokud je určen `POST_BUILD_SCRIPT_PATH` .

`PRE_BUILD_COMMAND`a `POST_BUILD_COMMAND` jsou proměnné prostředí, které jsou ve výchozím nastavení prázdné. Chcete-li spustit příkazy před sestavením, definujte `PRE_BUILD_COMMAND` . Chcete-li spustit příkazy po sestavení, definujte `POST_BUILD_COMMAND` .

Následující příklad určuje dvě proměnné pro řadu příkazů, které jsou odděleny čárkami.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Další proměnné prostředí pro přizpůsobení automatizace sestavení naleznete v tématu [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

Další informace o tom, jak App Service spouštět a sestavují ASP.NET Core aplikace v systému Linux, najdete v [dokumentaci k Oryx: jak se zjišťují a vytváří aplikace .NET Core](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/dotnetcore.md).

## <a name="access-environment-variables"></a>Přístup k proměnným prostředí

V App Service můžete [nastavit nastavení aplikace](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) mimo kód vaší aplikace. Pak k nim můžete přistupovat v libovolné třídě pomocí vzoru pro vkládání závislostí Standard ASP.NET Core:

```csharp
using Microsoft.Extensions.Configuration;

namespace SomeNamespace 
{
    public class SomeClass
    {
        private IConfiguration _configuration;
    
        public SomeClass(IConfiguration configuration)
        {
            _configuration = configuration;
        }
    
        public SomeMethod()
        {
            // retrieve nested App Service app setting
            var myHierarchicalConfig = _configuration["My:Hierarchical:Config:Data"];
            // retrieve App Service connection string
            var myConnString = _configuration.GetConnectionString("MyDbConnection");
        }
    }
}
```

Pokud nakonfigurujete nastavení aplikace se stejným názvem v App Service a v *appsettings.js*, například hodnota App Service má přednost před *appsettings.jsna* hodnotě. Místní *appsettings.js* hodnota umožňuje ladit aplikaci místně, ale hodnota App Service umožňuje spuštění aplikace v produktu s nastavením produkčního prostředí. Připojovací řetězce fungují stejným způsobem. Tímto způsobem můžete zachovat tajné klíče aplikace mimo vaše úložiště kódu a přistupovat k odpovídajícím hodnotám beze změny kódu.

> [!NOTE]
> Všimněte si, že [Hierarchická konfigurační data](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/#hierarchical-configuration-data) v *appsettings.js* jsou k dispozici pomocí `:` oddělovače standard pro .NET Core. Pokud chcete v App Service přepsat konkrétní hierarchické nastavení konfigurace, v klíči nastavte název nastavení aplikace na stejný formát s oddělovači. v [Cloud Shell](https://shell.azure.com)můžete spustit následující příklad:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings My:Hierarchical:Config:Data="some value"
```

## <a name="get-detailed-exceptions-page"></a>Získat podrobné stránky výjimek

Když aplikace ASP.NET vygeneruje výjimku v ladicím programu sady Visual Studio, prohlížeč zobrazí stránku podrobností o výjimce, ale v App Service tuto stránku nahradila Obecná chyba **HTTP 500** nebo při **zpracování vaší žádosti došlo k chybě.** zpráva. Pokud chcete zobrazit stránku podrobností o výjimce v App Service, přidejte do `ASPNETCORE_ENVIRONMENT` aplikace nastavení aplikace spuštěním následujícího příkazu v <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>Zjistit relaci HTTPS

V App Service dojde k [ukončení protokolu SSL](https://wikipedia.org/wiki/TLS_termination_proxy) v nástrojích pro vyrovnávání zatížení sítě, takže všechny požadavky HTTPS dosáhnou vaší aplikace jako nešifrované požadavky HTTP. Pokud vaše logika aplikace potřebuje zjistit, jestli jsou požadavky uživatele zašifrované, nebo ne, nakonfigurujte v *Startup.cs*middleware pro předávané hlavičky:

- Nakonfigurujte middleware pomocí [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) k přeposílání `X-Forwarded-For` `X-Forwarded-Proto` hlaviček a v `Startup.ConfigureServices` .
- Přidejte do známých sítí rozsahy privátních IP adres, aby middleware mohl důvěřovat nástroji pro vyrovnávání zatížení App Service.
- Vyvolat metodu [UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) v v `Startup.Configure` před voláním jiných middlewarů.

Vložení všech tří prvků dohromady, váš kód vypadá jako v následujícím příkladu:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

Další informace najdete v tématu [konfigurace ASP.NET Core pro práci se servery proxy a nástroji pro vyrovnávání zatížení](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer).

## <a name="deploy-multi-project-solutions"></a>Nasazení řešení s více projekty

Při nasazení úložiště ASP.NET do modulu nasazení se souborem *. csproj* v kořenovém adresáři modul nasadí projekt. Když nasadíte úložiště ASP.NET se souborem *. sln* v kořenovém adresáři, modul vybere první web nebo projekt webové aplikace, který se najde jako aplikace App Service. Je možné, že stroj nechce vybrat požadovaný projekt.

Chcete-li nasadit řešení pro více projektů, můžete určit projekt, který se má použít v App Service dvěma různými způsoby:

### <a name="using-deployment-file"></a>Použití souboru. Deployment

Přidejte soubor *. Deployment* do kořenového adresáře úložiště a přidejte následující kód:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Použití nastavení aplikace

V <a target="_blank" href="https://shell.azure.com">Azure Cloud Shell</a>přidejte do aplikace App Service nastavení aplikace spuštěním následujícího příkazu CLI. Nahraďte *\<app-name>* , *\<resource-group-name>* a *\<project-name>* odpovídajícími hodnotami.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Přístup k diagnostickým protokolům

ASP.NET Core poskytuje [integrovaného zprostředkovatele protokolování pro App Service](https://docs.microsoft.com/aspnet/core/fundamentals/logging/#azure-app-service). V *program.cs* projektu přidejte poskytovatele do aplikace prostřednictvím `ConfigureLogging` metody rozšíření, jak je znázorněno v následujícím příkladu:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(logging =>
        {
            logging.AddAzureWebAppDiagnostics();
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Pak můžete nakonfigurovat a vygenerovat protokoly se [standardním vzorem .NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

Další informace o řešení potíží s ASP.NET Core aplikacemi v App Service najdete v tématu věnovaném [řešení potíží ASP.NET Core na Azure App Service a IIS](https://docs.microsoft.com/aspnet/core/test/troubleshoot-azure-iis) .

## <a name="open-ssh-session-in-browser"></a>Otevřít relaci SSH v prohlížeči

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Kurz: ASP.NET Core aplikace s SQL Database](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [Nejčastější dotazy k App Service Linux](app-service-linux-faq.md)
