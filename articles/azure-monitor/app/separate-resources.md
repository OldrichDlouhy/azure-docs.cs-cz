---
title: Jak navrhnout nasazení Application Insights – jeden vs mnoho prostředků?
description: Přímá telemetrie na různé prostředky pro vývoj, testování a produkční razítka.
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 159a1c5554c0ac017bc9eeb2e9df65fddba334ba
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87326541"
---
# <a name="how-many-application-insights-resources-should-i-deploy"></a>Kolik prostředků Application Insights mám nasadit

Při vývoji další verze webové aplikace nechcete kombinovat [Application Insights](./app-insights-overview.md) telemetrii od nové verze a již vydané verze. Chcete-li předejít nejasnostem, zasílejte telemetrii z různých fází vývoje a oddělte Application Insights prostředky se samostatnými klíči instrumentace (instrumentační klíče). Aby bylo snazší změnit klíč instrumentace, protože verze se přesouvá z jedné fáze na jinou, může být užitečné nastavit ikey v kódu místo v konfiguračním souboru.

(Pokud je váš systém cloudová služba Azure, existuje [Další metoda nastavení samostatné instrumentační klíče](./cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>O prostředcích a klíčích instrumentace

Když nastavíte Application Insights Monitoring pro webovou aplikaci, vytvoříte v Microsoft Azure *prostředek* Application Insights. Tento prostředek otevřete v Azure Portal, aby se zobrazila a analyzovala telemetrie shromážděná z vaší aplikace. Prostředek je identifikovaný *klíčem instrumentace* (ikey). Když nainstalujete balíček Application Insights pro monitorování vaší aplikace, nakonfigurujete ji pomocí klíče instrumentace, aby znala, kam se má telemetrie odeslat.

Každý Application Insights prostředek obsahuje metriky, které jsou k dispozici předem. Pokud se zcela samostatné komponenty hlásí ke stejnému Application Insights prostředku, tyto metriky nemusí mít smysl na řídicí panel nebo výstrahy.

### <a name="when-to-use-a-single-application-insights-resource"></a>Kdy použít jeden prostředek Application Insights

-   Pro součásti aplikace, které jsou nasazeny dohromady. Obvykle vyvinutá jediným týmem, který je spravovaný stejnou sadou uživatelů DevOps/ITOps.
-   Pokud má smysl agregovat klíčové ukazatele výkonu (KPI), jako jsou například doby trvání odezvy, míry selhání v řídicím panelu atd., ve výchozím nastavení je můžete rozdělit na všechny z nich (v prostředí Průzkumník metrik můžete segmentovat podle názvu role).
-   Pokud není potřeba spravovat Access Control na základě rolí (RBAC) odlišně mezi součástmi aplikace.
-   Pokud nepotřebujete kritéria výstrahy metrik, která se liší mezi komponentami.
-   Pokud nepotřebujete spravovat průběžné exporty různě mezi komponentami.
-   Pokud nepotřebujete, aby se fakturace a kvóty spravovaly různě mezi komponentami.
-   Pokud je v pořádku, aby měl klíč rozhraní API stejný přístup k datům ze všech komponent. A 10 klíčů rozhraní API stačí pro potřeby napříč všemi nimi.
-   Pokud má být v pořádku stejné nastavení integrace inteligentní detekce a pracovní položky napříč všemi rolemi.

### <a name="other-things-to-keep-in-mind"></a>Další věci, které je potřeba vzít v úvahu

-   Je možné, že budete muset přidat vlastní kód, abyste zajistili, že jsou smysluplné hodnoty nastaveny do atributu [Cloud_RoleName](./app-map.md?tabs=net#set-cloud-role-name) . Bez smysluplných hodnot nastavených pro tento atribut nebudou fungovat *žádné* ze zkušeností portálu.
- V případě aplikací Service Fabric a klasických cloudových služeb sada SDK automaticky načte z prostředí role Azure a nastaví je. U všech ostatních typů aplikací je pravděpodobně budete muset nastavit explicitně.
-   Prostředí živé metriky nepodporuje rozdělování podle názvu role.

## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dynamický klíč instrumentace

Aby bylo snazší změnit ikey při pohybu kódu mezi fázemi výroby, nastavte ho v kódu místo v konfiguračním souboru.

Nastavte klíč v inicializační metodě, jako je například global.aspx.cs ve službě ASP.NET:

```csharp
protected void Application_Start()
{
  Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = 
      // - for example -
      WebConfigurationManager.AppSettings["ikey"];
  ...
```

V tomto příkladu jsou instrumentační klíče pro různé prostředky umístěny v různých verzích konfiguračního souboru webu. Výměna konfiguračního souboru webu, který můžete provést jako součást skriptu pro vydání, zahodí cílový prostředek.

### <a name="web-pages"></a>Webové stránky
IKey se také používá na webových stránkách vaší aplikace ve [skriptu, který jste získali v podokně rychlý Start](./javascript.md). Místo toho, aby se do skriptu nahlásilo, vygeneruje ho ze stavu serveru. Například v aplikaci ASP.NET:

```javascript
<script type="text/javascript">
// Standard Application Insights web page script:
var appInsights = window.appInsights || function(config){ ...
// Modify this part:
}({instrumentationKey:  
  // Generate from server property:
  "@Microsoft.ApplicationInsights.Extensibility.
     TelemetryConfiguration.Active.InstrumentationKey"
  }
 )
//...
```

## <a name="create-additional-application-insights-resources"></a>Vytvoření dalších prostředků Application Insights

Pokud chcete vytvořit prostředek Application Insights, postupujte podle pokynů v [příručce pro vytváření prostředků](./create-new-resource.md).

### <a name="getting-the-instrumentation-key"></a>Získávání klíče instrumentace
Klíč instrumentace identifikuje prostředek, který jste vytvořili.

Budete potřebovat klíče instrumentace všech prostředků, na které bude vaše aplikace odesílat data.

## <a name="filter-on-build-number"></a>Filtrovat podle čísla sestavení
Když publikujete novou verzi aplikace, budete chtít být schopni oddělit telemetrii od různých sestavení.

Vlastnost verze aplikace můžete nastavit tak, aby bylo možné filtrovat výsledky [hledání](./diagnostic-search.md) a [Průzkumníka metrik](../platform/metrics-charts.md) .

Vlastnost verze aplikace se nastavuje několika různými způsoby.

* Nastavit přímo:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Zabalte tento řádek do [inicializátoru telemetrie](./api-custom-events-metrics.md#defaults) , aby se zajistilo, že všechny instance TelemetryClient jsou nastavené konzistentně.
* [ASP.NET] Nastavte verzi v `BuildInfo.config` . Webový modul vybere z uzlu BuildLabel verzi. Zahrňte tento soubor do projektu a nezapomeňte nastavit vlastnost kopírovat vždy v Průzkumník řešení.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Automatické generování BuildInfo.config v nástroji MSBuild. Uděláte to tak, že do souboru přidáte několik řádků `.csproj` :

    ```XML
    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Tím se vygeneruje soubor s názvem *názevvašehoprojektu*.BuildInfo.config. Proces publikování je přejmenoval na BuildInfo.config.

    Popisek sestavení obsahuje zástupný symbol (AutoGen_...) při sestavování pomocí sady Visual Studio. Ale při sestavení pomocí nástroje MSBuild se naplní správným číslem verze.

    Pokud chcete, aby nástroj MSBuild vygeneroval čísla verzí, nastavte verzi jako `1.0.*` v AssemblyReference.cs.

## <a name="version-and-release-tracking"></a>Sledování verzí a vydání
Pokud chcete sledovat verzi aplikace, ujistěte se, že proces Microsoft Build Engine vygeneroval soubor `buildinfo.config`. Do `.csproj` souboru přidejte:  

```XML
<PropertyGroup>
  <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>
  <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
</PropertyGroup>
```

Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie. Díky tomu můžete při provádění [diagnostických hledání](./diagnostic-search.md) nebo při [zkoumání metrik](../platform/metrics-charts.md) filtrovat podle verze.

Všimněte si však, že číslo verze sestavení je generováno pouze pomocí Microsoft Build Engine, nikoli vývojářem Build ze sady Visual Studio.

### <a name="release-annotations"></a>Poznámky k verzi
Pokud používáte Azure DevOps, můžete při každém vydání nové verze [získat značku poznámky](./annotations.md) přidanou do vašich grafů. 

## <a name="next-steps"></a>Další kroky

* [Sdílené prostředky pro více rolí](./app-map.md)
* [Vytvoření inicializátoru telemetrie pro odlišení typu | B – varianty](./api-filtering-sampling.md#add-properties)

