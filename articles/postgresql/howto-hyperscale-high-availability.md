---
title: Konfigurace vysoké dostupnosti – Citus () – Azure Database for PostgreSQL
description: Jak povolit nebo zakázat vysokou dostupnost
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/04/2019
ms.openlocfilehash: 0c7702c8832e22d889a5d785dad845430bfb7d17
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116875"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Konfigurace vysoké dostupnosti Citus (High-Scale)

Azure Database for PostgreSQL – Citus (vysoká úroveň) poskytuje vysokou dostupnost (HA), aby nedocházelo k výpadkům databáze. Když je povolený HA, každý uzel ve skupině serverů získá pohotovostní režim. Pokud se původní uzel změní na není v pořádku, bude jeho pohotovostní režim povýšen na jeho náhradu.

> [!IMPORTANT]
> Vzhledem k tomu, že HA nazdvojnásobuje počet serverů ve skupině, bude také dvojnásobek nákladů.

Povolení vysoké dostupnosti je možné během vytváření skupiny serverů nebo později na kartě **Konfigurace** pro skupinu serverů v Azure Portal. Uživatelské rozhraní vypadá podobně v obou případech. Přetáhněte posuvník pro **vysokou dostupnost** na Ano:

![posuvník ha](./media/howto-hyperscale-high-availability/01-ha-slider.png)

Kliknutím na tlačítko **Uložit** použijte výběr. Povolení HA může nějakou dobu trvat, protože skupiny serverů zřídí pohotovostní data a proudy dat.

Karta **Přehled** pro skupinu serverů zobrazí seznam všech uzlů a jejich pohotovostních a společně se sloupcem s **vysokou dostupností** , který označuje, jestli je u každého uzlu úspěšně povolený ha.

![sloupec ha ve skupině serverů – přehled](./media/howto-hyperscale-high-availability/02-ha-column.png)

### <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [vysoké dostupnosti](concepts-hyperscale-high-availability.md).
