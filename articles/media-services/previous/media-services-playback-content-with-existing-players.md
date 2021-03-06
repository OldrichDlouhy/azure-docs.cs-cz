---
title: Přehrávání obsahu pomocí stávajících hráčů – Azure | Microsoft Docs
description: Tento článek obsahuje seznam existujících přehrávačů, které můžete použít k přehrávání obsahu.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 2d3c22e17c37bc46c16a9cc80eb3cf4b9ec93ecf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "81686921"
---
# <a name="playing-your-content-with-existing-players"></a>Přehrávání obsahu ve stávajících přehrávačích
Azure Media Services podporuje spoustu oblíbených formátů streamování, například Smooth Streaming, HTTP Live Streaming a MPEG-pomlčky. Toto téma ukazuje na existující přehrávače, které můžete použít k otestování vašich datových proudů.

### <a name="the-azure-portal-media-services-content-player"></a>Přehrávač obsahu Azure Portal Media Services
**Azure** Portal poskytuje přehrávač obsahu, který můžete použít k otestování videa.

Klikněte na požadované video (Ujistěte se, že je [publikované](media-services-portal-publish.md)) a klikněte na tlačítko **Přehrát** v dolní části portálu.

Musí být splněny určité předpoklady:

* Přehrávač **MEDIA SERVICES CONTENT PLAYER** přehrává z výchozího koncového bodu streamování. Pokud chcete přehrávat z jiného než výchozích koncového bodu streamování, použijte jiný přehrávač. Například [Azure Media Player](https://aka.ms/azuremediaplayer).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Přehrávač médií Azure

Použijte [Azure Media Player](https://aka.ms/azuremediaplayer) k přehrávání obsahu (Clear nebo Protected) v některém z následujících formátů:

* Technologie Smooth Streaming
* MPEG DASH
* HLS
* Progresivní MP4

### <a name="flash-player"></a>Přehrávač Flash

#### <a name="playready-with-token"></a>PlayReady s tokenem

[https://sltoken.azurewebsites.net](https://sltoken.azurewebsites.net)

### <a name="dash-players"></a>PŘERUŠOVANé přehrávače

[https://dashplayer.azurewebsites.net](https://dashplayer.azurewebsites.net)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Jiné
K testování adres URL HLS můžete také použít:

* **Safari** na zařízení s iOS nebo
* **3ivx HLS Player** ve Windows

## <a name="media-services-learning-paths"></a>Mapy kurzů k Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
