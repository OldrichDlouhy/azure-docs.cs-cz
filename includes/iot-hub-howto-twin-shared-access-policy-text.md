---
title: zahrnout soubor
description: zahrnout soubor
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e81ce2743d2509ce52f3190218decfa4e518bdad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "70050451"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

V tomto článku vytvoříte back-endové služby, které do vlákna zařízení přidají požadované vlastnosti, a pak se dotazuje registru identity, aby se vyhledala všechna zařízení s oznámenými vlastnostmi, která se odpovídajícím způsobem aktualizovala. Vaše služba potřebuje oprávnění **připojit službu** pro úpravu požadovaných vlastností vlákna zařízení a potřebuje oprávnění **ke čtení registru** pro dotazování registru identity. Nejsou k dispozici žádné výchozí zásady sdíleného přístupu, které obsahují pouze tato dvě oprávnění, takže je třeba ji vytvořit.
