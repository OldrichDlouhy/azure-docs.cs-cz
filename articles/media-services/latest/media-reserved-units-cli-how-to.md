---
title: Použití rozhraní příkazového řádku pro škálování rezervovaných jednotek médií – Azure | Microsoft Docs
description: V tomto tématu se dozvíte, jak pomocí rozhraní příkazového řádku škálovat zpracování médií pomocí Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6715014485b227713447ce5d552cf7ba79737845
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87053216"
---
# <a name="scaling-media-processing"></a>Škálování zpracování médií

Služba Azure Media Services umožňuje škálovat zpracování médií ve vašem účtu správou rezervovaných jednotek médií (MRU). MRUs určuje rychlost zpracování úloh zpracování médií. Můžete si vybrat mezi následujícími typy rezervovaných jednotek: **S1**, **S2** nebo **S3**. Například stejná úloha kódování bude rychlejší, když použijete typ rezervované jednotky **S2**, než kdybyste použili typ **S1**. 

Kromě určení typu rezervované jednotky můžete zadat, aby se účet zřídil rezervovanými jednotkami. Počet zřízených rezervovaných jednotek určuje počet úloh médií, které je možné v daném účtu zpracovávat současně. Pokud má váš účet například pět rezervovaných jednotek, pak pět mediálních úloh bude spuštěno souběžně, dokud budou zpracovány úkoly. Zbývající úlohy budou čekat ve frontě a budou vyzvednuty pro zpracování po dokončení běžící úlohy. Pokud účet nemá zřízeny žádné rezervované jednotky, budou úkoly postupně vyzvednuty. V tomto případě bude doba čekání mezi dokončením jednoho úkolu a dalším počátkem záviset na dostupnosti prostředků v systému.

## <a name="choosing-between-different-reserved-unit-types"></a>Volba mezi různými typy rezervovaných jednotek

Následující tabulka vám pomůže při rozhodování o tom, jak určit různé rychlosti kódování. Poskytuje také několik případů srovnávacích testů na [videu, které můžete stáhnout,](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) abyste mohli provádět vlastní testy:

|Typ RU|Scénář|Příklady výsledků pro [video o 7 min](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) .|
|---|---|---|
| **S1**|Kódování s jednou přenosovou rychlostí. <br/>Soubory na SD nebo pod rozlišením, nezávislá na čase, nízké náklady.|Kódování souboru MP4 s jednou přenosovou rychlostí SD pomocí "H264 s jednou přenosovou rychlostí" 16x9 "trvá přibližně 7 minut.|
| **S2**|Jedna přenosová rychlost a s více přenosovými rychlostmi.<br/>Normální použití pro kódování SD i HD.|Kódování s přednastavenou H264 Single přenosovou rychlostí 720p trvá přibližně 6 minut.<br/><br/>Kódování s přednastaveným H264 Multiple přenosovou rychlostí 720p trvá přibližně 12 minut.|
| **S3**|Jedna přenosová rychlost a s více přenosovými rychlostmi.<br/>Kompletní videa o rozlišení HD a 4K. Kódování citlivé na čas, rychlejší zadoba vyřízení.|Kódování pomocí přednastavené H264 s jednou přenosovou rychlostí 1080p trvá přibližně 3 minuty.<br/><br/>Kódování s přednastavenou H264 s více přenosovými rychlostmi 1080p trvá přibližně 8 minut.|

## <a name="considerations"></a>Požadavky

* Pro analýzy zvuku a úlohy analýzy videí, které se aktivují Media Services V3 nebo Video Indexer, se důrazně doporučuje typ jednotky S3.
* Pokud používáte sdílený fond, to znamená, že bez rezervovaných jednotek, budou mít vaše úlohy kódování stejný výkon jako u ru S1. Není však k dispozici horní mez doby, kterou mohou úlohy ve stavu zařazeny do fronty, a v jednom okamžiku bude spuštěna pouze jedna úloha.

Ve zbývající části článku se dozvíte, jak pomocí [Media Services V3 CLI](https://aka.ms/ams-v3-cli-ref) škálovat MRUs.

> [!NOTE]
> Pro úlohy analýzy zvuku a analýzy videa, které jsou aktivované službou Media Services v3 nebo Video Indexerem, důrazně doporučujeme zřídit váš účet s 10 rezervovanými jednotkami S3. Pokud potřebujete více než 10 S3 MRUs, otevřete lístek podpory pomocí [Azure Portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Předpoklady 

[Vytvořte účet Media Services](./create-account-howto.md).

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="scale-media-reserved-units-with-cli"></a>Škálování rezervovaných jednotek médií pomocí rozhraní příkazového řádku

Spusťte příkaz `mru`.

Následující příkaz [AZ AMS Account MRU](/cli/azure/ams/account/mru?view=azure-cli-latest) nastaví rezervované jednotky médií na účtu amsaccount pomocí parametrů **Count** a **Type** .

```azurecli
az ams account mru set -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Fakturace

Účtují se vám poplatky podle počtu minut, po které jsou rezervované jednotky médií zřízené ve vašem účtu. K tomu dochází nezávisle na tom, zda se ve vašem účtu spouštějí nějaké úlohy. Podrobné vysvětlení najdete v části Nejčastější dotazy stránky s [cenami Media Services](https://azure.microsoft.com/pricing/details/media-services/) .   

## <a name="next-step"></a>Další krok

[Analýza videí](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Viz také

* [Kvóty a omezení](limits-quotas-constraints.md)
* [Azure CLI](/cli/azure/ams?view=azure-cli-latest)
