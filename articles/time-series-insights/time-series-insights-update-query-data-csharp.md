---
title: Dotazování dat z prostředí Gen2 pomocí C#-Azure Time Series Insights | Microsoft Docs
description: Naučte se, jak zadávat dotazy na data z prostředí Azure Time Series Insights Gen2 pomocí aplikace napsané v jazyce C#.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/14/2020
ms.custom: seodec18
ms.openlocfilehash: 1057bb908e973c74b6dfb70931469e27f77512de
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292667"
---
# <a name="query-data-from-the-azure-time-series-insights-gen2-environment-using-c"></a>Dotazování dat z prostředí Azure Time Series Insights Gen2 pomocí jazyka C #

Tento příklad v jazyce C# ukazuje, jak zadávat dotazy na data z [rozhraní API pro přístup k datům Gen2](https://docs.microsoft.com/rest/api/time-series-insights/preview) v prostředích Azure Time Series Insights Gen2.

> [!TIP]
> Prohlédněte si ukázky kódu C# Gen2 na adrese [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample) .

## <a name="summary"></a>Souhrn

Vzorový kód níže znázorňuje následující funkce:

* Podpora automatické generace sady SDK z [Azure AutoRest](https://github.com/Azure/AutoRest)
* Získání přístupového tokenu prostřednictvím Azure Active Directory pomocí [Microsoft. IdentityModel. clients. Active](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).
* Jak předat token přístupu, který získal přístup v `Authorization` hlavičce dalších požadavků na přístup k rozhraní API pro přístup k datům. 
* Ukázka poskytuje rozhraní konzoly, které demonstruje, jak jsou požadavky HTTP provedeny:

    * [Rozhraní API pro prostředí Gen2](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * Získat rozhraní API pro [dostupnost prostředí](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/getavailability) a [získat rozhraní API pro schéma událostí](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/geteventschema)
    * [Rozhraní API pro dotazy Gen2](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * Získat [API pro události](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents), získat rozhraní [API řady](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries)a [získat agregované rozhraní API řady](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries)
    * [Rozhraní API modelu časové řady](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries)
        * [Rozhraní](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies) API pro [dávkování](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies/executebatch) hierarchií a hierarchií
        * Rozhraní API pro [získání typů](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes) a [typů rozhraní Batch API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch)
        * Rozhraní [API pro dávku](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/executebatch) instancí a instance rozhraní API pro [načtení instancí](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances)
* Rozšířené možnosti [vyhledávání](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features) a [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)

## <a name="prerequisites-and-setup"></a>Požadavky a instalace

Před kompilací a spuštěním ukázkového kódu proveďte následující kroky:

1. [Zřídí prostředí Gen2 Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment) .
1. Nakonfigurujte Azure Time Series Insights prostředí pro Azure Active Directory, jak je popsáno v tématu [ověřování a autorizace](time-series-insights-authentication-and-authorization.md). 
1. Spusťte [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) , jak je uvedeno v [Readme.MD](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) a vygenerujte závislosti klienta Azure Time Series Insights Gen2.
1. Otevřete `TSIPreviewDataPlaneclient.sln` řešení a nastavte `DataPlaneClientSampleApp` jako výchozí projekt v sadě Visual Studio.
1. Požadované závislosti projektu nainstalujte pomocí kroků popsaných [níže](#project-dependencies) a zkompilujte příklad do spustitelného `.exe` souboru.
1. Spusťte `.exe` soubor dvojitým kliknutím na něj.

## <a name="project-dependencies"></a>Závislosti projektu

Doporučuje se použít nejnovější verzi sady Visual Studio:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) – verze 16.4.2 +

Vzorový kód má několik požadovaných závislostí, které lze zobrazit v souboru [packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) .

Stáhněte si balíčky v aplikaci Visual Studio 2019 tak, **Build**že vyberete  >  možnost**řešení** sestavení sestavení. 

Případně přidejte jednotlivé balíčky pomocí [NuGet 2.12 +](https://www.nuget.org/). Například:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>Ukázkový kód C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Ukázku kódu lze provést bez změny výchozích proměnných prostředí.
> * Ukázka kódu se zkompiluje do spustitelné aplikace spustitelného rozhraní .NET.

## <a name="next-steps"></a>Další kroky

- Další informace o dotazování najdete v referenčních informacích k [rozhraní API pro dotazy](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Přečtěte si, jak [připojit aplikaci JavaScriptu pomocí klientské sady SDK](https://github.com/microsoft/tsiclient) pro Azure Time Series Insights.
