---
title: zahrnout soubor
description: zahrnout soubor
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f7495c52e36bdc207145f17ec72eb57b7ebf1784
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "76279300"
---
Vazby Azure Cosmos DB se podporují jenom pro použití s rozhraním SQL API. U všech ostatních Azure Cosmos DB rozhraní API byste měli k databázi přistupovat z vaší funkce pomocí statického klienta pro vaše rozhraní API, včetně [rozhraní api Azure Cosmos DB pro MongoDB](../articles/cosmos-db/mongodb-introduction.md), [rozhraní API CASSANDRA](../articles/cosmos-db/cassandra-introduction.md), [Gremlin API](../articles/cosmos-db/graph-introduction.md)a [rozhraní API pro tabulky](../articles/cosmos-db/table-introduction.md).
