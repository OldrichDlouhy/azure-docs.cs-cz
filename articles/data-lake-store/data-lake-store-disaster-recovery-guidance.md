---
title: Pokyny pro zotavení po havárii pro Azure Data Lake Storage Gen1 | Microsoft Docs
description: Doprovodné materiály k zajištění vysoké dostupnosti a zotavení po havárii pro Azure Data Lake Storage Gen1
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: 4931556aa6948b6b05b2bbbfa62e281e21aa6058
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2020
ms.locfileid: "87367465"
---
# <a name="high-availability-and-disaster-recovery-guidance-for-data-lake-storage-gen1"></a>Pokyny pro vysokou dostupnost a zotavení po havárii pro Data Lake Storage Gen1

Data Lake Storage Gen1 poskytuje místně redundantní úložiště (LRS). Proto jsou data ve vašem účtu Data Lake Storage Gen1 odolná proti přechodným chybám hardwaru v rámci datového centra prostřednictvím automatizovaných replik. Tím se zajistí odolnost a vysoká dostupnost, která splňuje Data Lake Storage Gen1 SLA. Tento článek poskytuje pokyny, jak dále chránit vaše data před vzácnými výpadky nebo neúmyslnými odstraněními v rámci oblastí.

## <a name="disaster-recovery-guidance"></a>Pokyny pro zotavení po havárii

Je důležité, abyste si napravili plán zotavení po havárii. Přečtěte si informace v tomto článku a další materiály, které vám pomůžou vytvořit vlastní plán.

* [Zotavení po havárii a vysoká dostupnost pro aplikace Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technické pokyny k odolnosti Azure](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practice-recommendations"></a>Doporučení pro osvědčené postupy

Doporučujeme zkopírovat kritická data do jiného Data Lake Storage Gen1 účtu v jiné oblasti s frekvencí zarovnaným na požadavky vašeho plánu zotavení po havárii. Existují různé metody kopírování dat, včetně [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md)nebo [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Pokud dojde k oblastnímu výpadku, můžete k datům přistupovat v oblasti, kde byla data zkopírována. Pokud chcete zjistit stav služby Azure po celém světě, můžete monitorovat [Azure Service Health řídicí panel](https://azure.microsoft.com/status/) .

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Pokyny pro obnovení v případě poškození nebo nechtěného odstranění dat

I když Data Lake Storage Gen1 zajišťuje odolnost dat prostřednictvím automatizovaných replik, nebrání vaší aplikaci (nebo vývojářům a uživatelům) v poškození dat nebo jejich nechtěnému odstranění.

### <a name="best-practices"></a>Osvědčené postupy

Abyste zabránili nechtěnému odstranění, doporučujeme, abyste nejdřív nastavili správné zásady přístupu pro svůj účet Data Lake Storage Gen1. To zahrnuje použití [zámků prostředků Azure](../azure-resource-manager/management/lock-resources.md) k uzamčení důležitých prostředků a použití řízení přístupu na úrovni účtu a souboru pomocí dostupných [funkcí zabezpečení Data Lake Storage Gen1](data-lake-store-security-overview.md). Doporučujeme také pravidelně vytvářet kopie důležitých dat pomocí [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) nebo [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) v jiném Data Lake Storage Gen1m účtu, složce nebo předplatném Azure. Kopie pak můžete využít k obnovení poškozených nebo odstraněných dat. Azure Data Factory je užitečná služba pro opakované vytváření a nasazování kanálů k přesunu dat.

Můžete také povolit [protokolování diagnostiky](data-lake-store-diagnostic-logs.md) pro účet Data Lake Storage Gen1, abyste mohli shromažďovat záznamy pro audit přístupu k datům. Záznamy auditu poskytují informace o tom, kdo mohl soubor odstranit nebo aktualizovat.

## <a name="next-steps"></a>Další kroky

* [Začínáme s Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* [Zabezpečení dat ve službě Data Lake Storage Gen1](data-lake-store-secure-data.md)
