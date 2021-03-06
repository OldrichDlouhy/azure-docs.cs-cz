---
title: Přehled rozhraní API pro Azure Relay | Microsoft Docs
description: Tento článek poskytuje přehled dostupných Azure Relay rozhraní API (.NET Standard, .NET Framework, Node.js atd.).
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 15e13ccac9b05e6e1b0730b38b372dec20c69dda
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85316962"
---
# <a name="available-relay-apis"></a>Dostupná předávací rozhraní API

## <a name="runtime-apis"></a>Běhová rozhraní API

V následující tabulce jsou uvedeny všechny aktuálně dostupné klienty modulu runtime přenosu.

V části [Další informace](#additional-information) najdete další informace o stavu každé běhové knihovny.

| Jazyk/platforma | Dostupná funkce | Klientský balíček | Repository |
| --- | --- | --- | --- |
| .NET Standard | Hybridní připojení | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHubu](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Relay | [WindowsAzure. ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Není k dispozici |
| Node | Hybridní připojení | [WebSockets`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[WebSockets`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[Požadavky HTTP:`hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHubu](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Další informace

#### <a name="net"></a>.NET

Ekosystém .NET má několik modulů runtime, proto existuje více knihoven .NET pro přenos. Knihovnu .NET Standard lze spustit buď pomocí .NET Core, nebo .NET Framework, zatímco knihovnu .NET Framework lze spustit pouze v .NET Framework prostředí. Další informace o rozhraní .NET Framework naleznete v tématu [verze rozhraní](/dotnet/articles/standard/frameworks).

Knihovna .NET Framework podporuje pouze programovací model WCF a spoléhá na proprietární binární protokol založený na `net.tcp` přenosu WCF. Tento protokol a knihovna se uchovávají kvůli zpětné kompatibilitě se stávajícími aplikacemi.

Knihovna .NET Standard je založena na definici otevřeného protokolu pro Hybrid Connections Relay, která je založena na protokolech HTTP a WebSockets. Knihovna podporuje abstrakci streamování přes objekty WebSockets a jednoduché gesto rozhraní API požadavku a odpovědi pro zodpovězení požadavků HTTP. Ukázka [webového rozhraní API](https://github.com/Azure/azure-relay-dotnet) ukazuje, jak integrovat Hybrid Connections s ASP.NET Core pro webové služby.

#### <a name="nodejs"></a>Node.js

Moduly Hybrid Connections uvedené v tabulce výše nahrazují nebo mění existující Node.js moduly s alternativními implementacemi, které naslouchají Azure Relay službě, nikoli v zásobníku místní sítě.

`hyco-https`Modul mění a částečně Přepisuje základní Node.js moduly `http` a `https` poskytuje implementaci naslouchacího procesu https, která je kompatibilní s mnoha existujícími Node.js moduly a aplikacemi, které jsou závislé na těchto základních modulech.

`hyco-ws`Moduly a `hyco-websocket` mění oblíbené `ws` a `websocket` moduly pro Node.js a poskytují alternativní implementace naslouchacího procesu, které umožňují modulům a aplikacím, aby fungovaly za Hybrid Connections Relay.

Podrobnosti o těchto modulech najdete v úložišti GitHub [Azure-Relay-Node](https://github.com/Azure/azure-relay-node) .

## <a name="next-steps"></a>Další kroky

Další informace o Azure Relay najdete na těchto odkazech:
* [Co je Azure Relay?](relay-what-is-it.md)
* [Přenos – nejčastější dotazy](relay-faq.md)
