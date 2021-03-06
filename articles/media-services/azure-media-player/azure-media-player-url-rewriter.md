---
title: Azure Media Player přepisu adresy URL
description: Azure Media Player přepíše zadanou adresu URL z Azure Media Services a poskytne streamování pro hladké, PŘERUŠOVANé, HLS v3 a HLS v4.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: f238a2a3c499cf1e36f5e7c40e087375b7db0a70
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "81726456"
---
# <a name="url-rewriter"></a>Přepisovač adres URL #

Ve výchozím nastavení Azure Media Player přepíše zadanou adresu URL z Azure Media Services, aby poskytovala proudy pro hladké, PŘERUŠOVANé, HLS v3 a HLS v4. Například pokud je zdroj uveden následujícím způsobem, Azure Media Player zajistěte, aby se pokusy o přehrání všech výše uvedených protokolů:

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" />
    </video>
```

Pokud ale chcete přepsat adresu URL, můžete to udělat tak, že do parametru přidáte `disableUrlRewriter` vlastnost. To znamená, že všechny informace, které jsou předány do zdrojů, jsou přímo předány do přehrávače beze změny.  Tady je příklad přidání dvou zdrojů do přehrávače, na PŘERUŠOVANém a jednom HLADKÉm streamování.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)" type="application/dash+xml" data-setup='{"disableUrlRewriter": true}'/>
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"disableUrlRewriter": true}'/>
    </video>
```

– nebo –

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest(format=mpd-time-csf)", type: "application/dash+xml", disableUrlRewriter: true },
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", disableUrlRewriter: true }
    ]);
```

Pokud chcete, můžete také zadat konkrétní formáty streamování, které chcete přepsat, aby bylo možné tento `streamingFormats` parametr Azure Media Player použít. Mezi možnosti `DASH`patří `SMOOTH`, `HLSv3`, `HLSv4`, `HLS`,. Rozdíl mezi HLS a HLSv3 & v4 je v tom, že formát HLS podporuje přehrávání FairPlay obsahu. Verze v3 a v4 nepodporují FairPlay. To je užitečné, pokud nemáte zásady doručování pro konkrétní protokol k dispozici.  Tady je příklad, kdy není pro váš Asset povolený protokol typu SPOJOVNÍK.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin">
        <source src="//example/path/to/myVideo.ism/manifest" type="application/vnd.ms-sstr+xml" data-setup='{"streamingFormats": ["SMOOTH", "HLS","HLS-V3", "HLS-V4"] }'/>
    </video>
```

– nebo –

```javascript
    myPlayer.src([
        { src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml", streamingFormats: ["SMOOTH", "HLS","HLS-V3", "HLS-V4"]},
    ]);
```

Výše uvedené dvě lze v kombinaci vzájemně použít pro více okolností na základě vašeho konkrétního assetu.

> [!NOTE]
> Informace o ochraně Widevine trvají jenom u protokolu POMLČKy.

## <a name="next-steps"></a>Další kroky ##

- [Rychlý Start Azure Media Player](azure-media-player-quickstart.md)