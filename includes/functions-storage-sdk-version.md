---
title: zahrnout soubor
description: zahrnout soubor
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 6a37850eb6536c5399d63144e60ea210fbc194d8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "77198410"
---
#### <a name="azure-storage-sdk-version-in-functions-1x"></a>Verze sady SDK Azure Storage ve funkcích 1. x

Ve funkcích 1. x používají triggery a vazby úložiště verzi 7.2.1 sady Azure Storage SDK (balíček NuGet pro[windowsazure. Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) ). Pokud odkazujete na jinou verzi sady SDK úložiště a navážete se na typ sady SDK úložiště v signatuře funkce, modul runtime Functions může hlásit, že nemůže vytvořit vazby k tomuto typu. Řešením je zajistit, aby se váš projekt odkazoval na [windowsazure. Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
