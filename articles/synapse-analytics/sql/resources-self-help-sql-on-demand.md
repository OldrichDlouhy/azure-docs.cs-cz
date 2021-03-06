---
title: Samoobslužná ochrana SQL na vyžádání (Preview)
description: Tato část obsahuje informace, které vám pomůžou při řešení problémů s SQL na vyžádání (Preview).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 7a6b145e9a1efb29bbb6c233f2a09498b4a4ea7f
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213121"
---
# <a name="self-help-for-sql-on-demand-preview"></a>Samoobslužná Pomocník pro SQL na vyžádání (Preview)

Tento článek obsahuje informace o řešení nejčastějších problémů s SQL na vyžádání (Preview) ve službě Azure synapse Analytics.

## <a name="sql-on-demand-is-grayed-out-in-synapse-studio"></a>SQL na vyžádání je v synapse studiu šedé

Pokud synapse Studio nemůže navázat připojení k SQL na vyžádání, všimnete si, že je SQL na vyžádání šedý nebo zobrazuje stav offline. K tomuto problému obvykle dochází, když dojde k jednomu z následujících případů:

1) Vaše síť brání komunikaci s back-endu Azure synapse. Nejčastějším případem je, že je port 1443 zablokovaný. Pokud chcete, aby SQL na vyžádání fungovalo, odblokujte tento port. Další problémy by mohly zabránit tomu, aby SQL na vyžádání fungovalo, a [Další informace najdete v kompletní příručce pro odstraňování potíží](../troubleshoot/troubleshoot-synapse-studio.md).
2) Nemáte oprávnění k přihlášení k SQL na vyžádání. Pokud chcete získat přístup, jeden z správců pracovního prostoru Azure synapse by vás měl přidat do role správce pracovního prostoru nebo správce SQL. [Další informace najdete v kompletní příručce k řízení přístupu](access-control.md).

## <a name="query-fails-because-file-cannot-be-opened"></a>Dotaz se nezdařil, protože soubor nelze otevřít.

Pokud se dotaz nezdařil s chybou říká ' soubor nelze otevřít, protože neexistuje nebo je používán jiným procesem ' a Vy jste si jisti, že oba soubory existují a že se nepoužívá v jiném procesu, znamená to, že SQL na vyžádání nemá přístup k souboru. K tomuto problému obvykle dochází, protože vaše Azure Active Directory identita nemá práva pro přístup k souboru. Ve výchozím nastavení se SQL na vyžádání snaží získat přístup k souboru pomocí vaší Azure Active Directory identity. Chcete-li tento problém vyřešit, musíte mít správná oprávnění pro přístup k souboru. Nejjednodušší je udělit sami sobě roli Přispěvatel dat v objektech blob služby Storage pro účet úložiště, který se pokoušíte dotazovat. [Další informace najdete v úplném průvodci řízením přístupu k úložišti pomocí Azure Active Directory](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Dotaz se nezdařil, protože jej nelze provést z důvodu aktuálních omezení prostředků. 

Pokud se dotaz nezdařil s chybovou zprávou "Tento dotaz nelze provést z důvodu aktuálních omezení prostředků", znamená to, že SQL na vyžádání není schopen ho v tuto chvíli spustit v důsledku omezení prostředků: 

- Ujistěte se, že používáte datové typy přiměřených velikostí. Kromě toho pro soubory Parquet určete schéma sloupců řetězců, které jsou ve výchozím nastavení datového typu VARCHAR(8000). 

- Pokud vaše dotazy cílí na soubory CSV, zvažte [vytvoření statistiky](develop-tables-statistics.md#statistics-in-sql-on-demand-preview). 

- Vyjděte si [osvědčené postupy výkonu pro optimalizaci dotazu SQL na vyžádání](best-practices-sql-on-demand.md) .  

## <a name="create-statement-is-not-supported-in-master-database"></a>PŘÍKAZ CREATE STATEMENT není v hlavní databázi podporován.

Pokud se dotaz nezdařil s chybovou zprávou:

> Nepovedlo se spustit dotaz. Chyba: vytvoření externí tabulky/zdroje dat/datový rozsah databáze/formát souboru není v hlavní databázi podporován. 

To znamená, že hlavní databáze na vyžádání SQL na vyžádání nepodporuje vytváření:
  - Externí tabulky
  - Externí zdroje dat
  - Přihlašovací údaje v oboru databáze
  - Formáty externích souborů

Řešení:

  1. Vytvořit uživatelskou databázi:

```sql
CREATE DATABASE <DATABASE_NAME>
```

  2. Příkaz EXECUTE Create v kontextu <DATABASE_NAME> který se dřív nezdařil pro hlavní databázi. 
  
  Příklad pro vytvoření formátu externího souboru:
    
```sql
USE <DATABASE_NAME>
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] 
WITH ( FORMAT_TYPE = PARQUET)
```

## <a name="next-steps"></a>Další kroky

Další informace o používání SQL na vyžádání najdete v následujících článcích:

- [Dotaz na jeden soubor CSV](query-single-csv-file.md)

- [Složky dotazů a více souborů CSV](query-folders-multiple-csv-files.md)

- [Dotazování konkrétních souborů](query-specific-files.md)

- [Dotazování souborů Parquet](query-parquet-files.md)

- [Dotazování vnořených typů Parquet](query-parquet-nested-types.md)

- [Dotazování souborů JSON](query-json-files.md)

- [Vytváření a používání zobrazení](create-use-views.md)

- [Vytvoření a použití externích tabulek](create-use-external-tables.md)

- [Ukládání výsledků dotazů do úložiště](create-external-table-as-select.md)
