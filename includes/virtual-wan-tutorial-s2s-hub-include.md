---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 105ab0c71d9e7e935842550ecdc4c8d2ff2a2d8c
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977905"
---
1. Vyhledejte virtuální síť WAN, kterou jste vytvořili. Na stránce virtuální síť WAN v části **připojení** vyberte **rozbočovače**.
2. Na stránce centra vyberte **+ nové centrum** a otevřete stránku **vytvořit virtuální rozbočovač** .

    ![Základy](./media/virtual-wan-tutorial-hub-include/basics.png "Základy")
3. Na kartě **základy** stránky **vytvořit virtuální rozbočovač** vyplňte následující pole:

    **Podrobnosti o projektu**

   * Oblast (dříve označovaná jako umístění)
   * Name
   * Privátní adresní prostor centra Minimální adresní prostor je/24 pro vytvoření centra, což znamená, že při vytváření dojde k chybě z rozsahu od/25 do/32. Azure Virtual WAN je spravovaná služba Microsoftu vytvoří ve virtuálním centru příslušné podsítě pro různé brány nebo služby (např. brány VPN, brány ExpressRoute, uživatelské VPN/brány, brány firewall, směrování atd.). Není nutné, aby uživatel explicitně naplánoval adresní prostor podsítě pro služby ve virtuálním rozbočovači, protože společnost Microsoft to dělá jako součást služby.
4. Vyberte **Další: Site-to-site**.

    ![Site-to-Site](./media/virtual-wan-tutorial-hub-include/site-to-site.png "Site-to-Site")

5. Na kartě **site-to-site** vyplňte následující pole:

   * Vyberte **Ano** , pokud chcete vytvořit síť VPN typu Site-to-site.
   * V současné době není možné ve virtuálním rozbočovači upravovat pole AS Number.
   * V rozevíracím seznamu vyberte hodnotu **jednotka škálování brány** . Jednotka škálování umožňuje vybrat agregovanou propustnost brány sítě VPN, která se vytváří ve virtuálním rozbočovači pro připojení lokalit k. Pokud vyberete 1 jednotku škálování = 500 MB/s, znamená to, že se vytvoří dvě instance pro redundanci, z nichž každá má maximální propustnost 500 MB/s. Pokud byste například měli pět větví, z nichž každý bude mít 10 MB/s na větvi, budete potřebovat agregaci 50 MB/s na konci hlavního umístění. Plánování agregované kapacity Azure VPN Gateway by se mělo provést po vyhodnocení kapacity potřebné k podpoře počtu větví do centra.
6. Vyberte **zkontrolovat + vytvořit** k ověření.
7. Vyberte **vytvořit** a vytvořte tak centrum. Po 30 minutách **aktualizujte** zobrazení centra na stránce **centra** . Vyberte **Přejít k prostředku** a přejděte k prostředku.
