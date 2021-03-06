---
title: Přehled automatického ladění
description: Azure SQL Database a Azure Managed instance SQL analyzují dotaz SQL a automaticky se přizpůsobí uživatelskému zatížení.
services: sql-database
ms.service: sql-db-mi
ms.subservice: performance
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/30/2020
ms.openlocfilehash: d2a00be4d08a7a2dfa8e11a22593d017d184a368
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2020
ms.locfileid: "85982707"
---
# <a name="automatic-tuning-in-azure-sql-database-and-azure-sql-managed-instance"></a>Automatické ladění v Azure SQL Database a spravované instanci Azure SQL
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL Database a automatické ladění spravované instance Azure SQL nabízí špičkový výkon a stabilní úlohy prostřednictvím neustálého ladění výkonu na základě AI a strojového učení.

Automatické ladění je plně spravovaná služba inteligentního výkonu, která pomocí integrovaných inteligentních funkcí neustále monitoruje dotazy spouštěné v databázi a automaticky zlepšuje jejich výkon. K tomu je možné využít dynamickou úpravu databáze na měnící se úlohy a použití doporučení pro ladění. Automatické ladění se učí vodorovně ze všech databází v Azure prostřednictvím AI a dynamicky vylepšuje své akce optimalizace. Čím déle se databáze spustí s automatickým laděním, tím lepší je výkon.

Azure SQL Database a automatické ladění spravované instance Azure SQL může být jednou z nejdůležitějších funkcí, které můžete povolit pro zajištění stabilních a špičkových úloh pro databáze.

## <a name="what-can-automatic-tuning-do-for-you"></a>Co umožňuje automatické ladění

- Automatizované ladění výkonu databází
- Automatizované ověřování nárůstů výkonu
- Automatické vrácení zpět a automatické opravy
- Historie ladění
- Ladění skriptů Transact-SQL (T-SQL) pro ruční nasazení
- Monitorování výkonu proaktivní úlohy
- Možnosti horizontálního navýšení kapacity na stovkách tisíc databází
- Kladný dopad na prostředky DevOps a celkové náklady na vlastnictví

## <a name="safe-reliable-and-proven"></a>Bezpečná, spolehlivá a prověřená

Operace optimalizace aplikované na databáze v Azure SQL Database jsou plně bezpečné pro výkon nejvýraznějších úloh. Systém je navržený tak, aby nenarušil úlohy uživatelů. Doporučení automatického ladění se aplikují jenom v době nízkého využití. Systém může také dočasně zakázat operace automatického ladění, aby se chránil výkon úloh. V takovém případě se v Azure Portal zobrazí zpráva "zakázáno systémem". Automatické ladění se týká úloh s nejvyšší prioritou prostředku.

Automatické optimalizační mechanismy jsou vyspělé a byly dokonalé na několika milionových databázích běžících na Azure. Použité automatizované operace ladění jsou automaticky ověřovány, aby bylo zajištěno kladné zlepšení výkonu úloh. Jsou dynamicky zjištěna doporučení k převráceným výsledkům a okamžitě se vrátí. V zaznamenané historii ladění existuje jasné trasování optimalizace pro každou databázi v Azure SQL Database a spravované instanci Azure SQL.

![Jak funguje automatické ladění](./media/automatic-tuning-overview/how-does-automatic-tuning-work.png)

Azure SQL Database automatické ladění sdílí svou základní logiku s funkcí automatického ladění SQL Server v databázovém stroji. Další technické informace o integrovaném mechanismu inteligentních funkcí najdete v tématu [SQL Server automatické ladění](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

Přehled toho, jak funguje automatické ladění a pro typické scénáře použití, najdete v tématu vložené video:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Improve-Azure-SQL-Database-Performance-with-automatic-tuning/player]

## <a name="enable-automatic-tuning"></a>Povolení automatického ladění

- [Automatické ladění pro Azure SQL Database můžete povolit v Azure Portal](automatic-tuning-enable.md) nebo pomocí příkazu [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) T-SQL.
- Automatické ladění pro spravovanou instanci SQL Azure povolíte pomocí příkazu [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-mi-current) jazyka T-SQL.

## <a name="automatic-tuning-options"></a>Možnosti automatického ladění

K dispozici jsou možnosti automatického ladění Azure SQL Database a Azure SQL Managed instance:

| Možnost automatického ladění | Podpora jedné databáze a databáze ve fondu | Podpora databáze instancí |
| :----------------------------- | ----- | ----- |
| **Vytvořit index** – identifikuje indexy, které mohou zlepšit výkon úloh, vytváří indexy a automaticky ověřuje, zda se zvýšil výkon dotazů. | Yes | No |
| **Drop index** – identifikuje redundantní a duplicitní indexy denně, s výjimkou jedinečných indexů a indexů, které se nepoužívaly dlouhou dobu (>90 dní). Upozorňujeme, že tato možnost není kompatibilní s aplikacemi, které používají přepínání oddílů a parametry indexu. Vyřazování nepoužívaných indexů se u úrovní služeb Premium a Pro důležité obchodní informace nepodporuje. | Yes | No |
| **Vynutit poslední dobrý dobrý plán** (automatický opravný plán) – identifikuje dotazy Azure SQL pomocí plánu spuštění, který je pomalejší než předchozí plán, a dotazuje se pomocí posledního známého funkčního plánu místo navýšení plánu. | Ano | Ano |

### <a name="automatic-tuning-for-sql-database"></a>Automatické ladění pro SQL Database

Automatické ladění pro Azure SQL Database používá k optimalizaci výkonu databáze doporučení **Create index**, **drop index**a **Force (poslední dobrý plán** Database Advisor). Další informace najdete v tématu [doporučení ke službě Database Advisor v Azure Portal](database-advisor-find-recommendations-portal.md), v [PowerShellu](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction)a v [REST API](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).

Doporučení pro vyladění můžete buď ručně použít Azure Portal, nebo můžete nechat automatické ladění, které vám pro vás pomůžou použít doporučení pro ladění. K tomu, aby systém mohl samostatně použít doporučení pro vyladění, je to, že se automaticky ověří, že existuje kladný zisk na výkon úlohy a pokud se nezjistí žádné významné zlepšení výkonu, automaticky obnoví doporučení optimalizace. Počítejte s tím, že v případě dotazů, které jsou ovlivněné vyladěním doporučení, která se neprovádí často, může trvat až 72 hodin, než se návrh povede.

V případě, že použijete doporučení pro ladění prostřednictvím T-SQL, automatické ověřování výkonu a mechanismy zpětného zpracování nejsou k dispozici. Doporučení použitá tímto způsobem zůstanou aktivní a budou se zobrazovat v seznamu doporučení pro ladění 24-48 hodin. předtím, než je systém automaticky stáhne. Pokud byste chtěli doporučení odebrat dřív, můžete ho zahodit z Azure Portal.

Možnosti automatického ladění můžou být nezávisle povolené nebo zakázané pro každou databázi, nebo je můžete nakonfigurovat na úrovni serveru a použít na všech databázích, které dědí nastavení ze serveru. Servery můžou dědit výchozí nastavení Azure pro automatické ladění. Výchozí nastavení Azure v tuto chvíli jsou nastavená na FORCE_LAST_GOOD_PLAN je povolená CREATE_INDEX povolená a DROP_INDEX je zakázaná.

> [!IMPORTANT]
> Od března 2020 změny ve výchozích nastaveních Azure pro automatické ladění se projeví takto:
>
> - Nové výchozí hodnoty Azure budou FORCE_LAST_GOOD_PLAN = Enabled, CREATE_INDEX = disabled a DROP_INDEX = Disabled.
> - Stávající servery bez nakonfigurovaných předvoleb automatického ladění se automaticky nakonfigurují tak, aby ZDĚDILy nové výchozí hodnoty Azure. To platí pro všechny zákazníky, kteří aktuálně mají nastavení serveru pro automatické ladění v nedefinovaném stavu.
> - Nově vytvořené servery se automaticky nakonfigurují tak, aby ZDĚDILy nové výchozí hodnoty Azure (na rozdíl od dřív, kdy byla konfigurace automatického ladění v nedefinovaném stavu při vytváření nového serveru).

Konfigurace možností automatického ladění na serveru a dědění nastavení pro databáze patřící k nadřazenému serveru je doporučená metoda pro konfiguraci automatického ladění, protože zjednodušuje správu možností automatického ladění pro velký počet databází.

Informace o vytváření e-mailových oznámení pro doporučení automatického ladění najdete v tématu [e-mailová oznámení pro automatické ladění](automatic-tuning-email-notifications-configure.md).

### <a name="automatic-tuning-for-azure-sql-managed-instance"></a>Automatické ladění pro spravovanou instanci Azure SQL

Automatické ladění pro spravovanou instanci SQL podporuje pouze **Vynutit poslední dobrý plán**. Další informace o konfiguraci možností automatického ladění prostřednictvím T-SQL najdete v tématu [Automatické ladění zavádí automatickou opravu plánu](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) a [automatickou opravu plánu](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning?view=sql-server-ver15#automatic-plan-correction).

## <a name="next-steps"></a>Další kroky

- Další informace o integrovaných inteligentních informacích, které se používají při automatickém ladění, najdete v tématu [uměle vyAzure SQL Database](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Další informace o tom, jak funguje automatické ladění v digestoři, najdete v tématu [automatické indexování milionů databází v Microsoft Azure SQL Database](https://www.microsoft.com/research/uploads/prod/2019/02/autoindexing_azuredb.pdf).
