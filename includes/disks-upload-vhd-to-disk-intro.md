---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e96d205ef1a8f94baa3a0cfe6c5127b6cf570e5a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80420959"
---
Tento článek vysvětluje, jak nahrát VHD z místního počítače na spravovaný disk Azure nebo zkopírovat spravovaný disk do jiné oblasti pomocí AzCopy. Tento proces, přímé nahrávání, vám také umožní nahrát virtuální pevný disk až do 32 TiB velikosti přímo do spravovaného disku. V současné době se podporuje přímé nahrávání pro disky Standard HDD, Standard SSD a Premium SSD. U disků s Ultra ještě není podporováno.

Pokud poskytujete řešení zálohování pro virtuální počítače s IaaS v Azure, doporučujeme použít přímé nahrávání pro obnovení záloh zákazníka na spravované disky. Když nahráváte virtuální pevný disk ze zdrojového externího prostředí do Azure, budou se vám na základě vaší místní šířky pásma spoléhat i rychlost. Při nahrávání nebo kopírování z virtuálního počítače Azure by šířka pásma byla stejná jako u standardních HDD.