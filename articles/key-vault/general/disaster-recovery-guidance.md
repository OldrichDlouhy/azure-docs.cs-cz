---
title: Co dělat v případě výpadku služby Azure, který má vliv na Azure Key Vault-Azure Key Vault | Microsoft Docs
description: Seznamte se s tím, co dělat v případě výpadku služby Azure, který má vliv na Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 05/04/2020
ms.author: sudbalas
ms.openlocfilehash: 4796e6c555ca67794409fb1476f3c4fd0d760719
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780449"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault dostupnost a redundance

Azure Key Vault funkce více vrstev redundance, aby se zajistilo, že vaše klíče a tajné kódy zůstanou k dispozici pro vaši aplikaci, i když jednotlivé komponenty služby selžou.

Obsah trezoru klíčů se replikuje v rámci oblasti a do sekundární oblasti minimálně 150 kilometrů, ale ve stejné geografické oblasti. Tím se zachovává vysoká odolnost klíčů a tajných kódů. Podrobnosti o dvojicích konkrétních oblastí najdete v dokumentu s [spárovanými](../../best-practices-availability-paired-regions.md) oblastmi Azure.

Pokud jednotlivé komponenty v rámci služby trezoru klíčů selžou, alternativní komponenty v rámci této oblasti v rámci vaší žádosti budou sloužit k tomu, aby se zajistilo, že nedojde k žádnému zhoršení funkčnosti. K aktivaci této akce není nutné provádět žádnou akci. K tomu dochází automaticky a pro vás bude transparentní.

V vzácných událostech, že celá oblast Azure není k dispozici, jsou požadavky, které provedete Azure Key Vault v této oblasti, automaticky směrovány (*převzetí*služeb při selhání) do sekundární oblasti. Pokud je primární oblast opět k dispozici, požadavky se přesměrují zpět (po*obnovení*) do primární oblasti. Znovu nemusíte provádět žádnou akci, protože k tomu dochází automaticky.

Tento návrh vysoké dostupnosti Azure Key Vault nevyžaduje pro aktivity údržby žádné výpadky.

Je potřeba mít na paměti několik upozornění:

* V případě převzetí služeb při selhání v oblasti může trvat několik minut, než služba převezme služby při selhání. Žádosti, které se provedou během této doby, můžou selhat, dokud se převzetí služeb při selhání nedokončí.
* Po dokončení převzetí služeb při selhání je váš Trezor klíčů v režimu jen pro čtení. Požadavky, které jsou podporovány v tomto režimu:
  * Vypsat trezory klíčů
  * Získat vlastnosti trezorů klíčů
   * Výpis certifikátů
  * Získat certifikáty
  * Výpis tajných kódů
  * Získání tajných kódů
  * Zobrazit seznam klíčů
  * Získat klíče (vlastnosti)
  * Šifrování
  * Dešifrování
  * Balí
  * Rozbalení
  * Ověřit
  * Znaménko
  * Backup
* Po převzetí služeb při selhání se vrátí všechny typy požadavků (včetně požadavků *na čtení a* zápis).

