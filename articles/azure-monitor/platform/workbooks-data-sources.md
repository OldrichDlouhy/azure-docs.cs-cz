---
title: Zdroje dat Azure Monitor sešity | Dokumentace Microsoftu
description: Zjednodušení složitých sestav s předem sestavenými a vlastními parametrizovanými Azure Monitor sešity sestavenými z více zdrojů dat
services: azure-monitor
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: mbullwin
ms.openlocfilehash: f27771291d95770a693fa56041f7dce3de459d13
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87081419"
---
# <a name="azure-monitor-workbooks-data-sources"></a>Zdroje dat Azure Monitor sešity

Sešity jsou kompatibilní s velkým počtem zdrojů dat. Tento článek vás provede jednotlivými zdroji dat, které jsou aktuálně k dispozici pro Azure Monitor sešity.

## <a name="logs"></a>Protokoly

Pracovní sešity umožňují dotazování protokolů z následujících zdrojů:

* Protokoly Azure Monitor (Application Insights prostředky a Log Analytics pracovní prostory.)
* Data orientovaných na prostředky (protokoly aktivit)

Autoři sešitu mohou používat dotazy KQL, které transformují podkladová data prostředků k výběru sady výsledků, která se může vizuálně vyznačit jako text, grafy nebo mřížky.

![Snímek obrazovky s rozhraním sestav protokolů v sešitech](./media/workbooks-overview/logs.png)

Autoři sešitu můžou snadno dotazovat se na více zdrojů a vytvořit skutečně sjednocené prostředí pro vytváření sestav.

## <a name="metrics"></a>Metriky

Prostředky Azure emitují [metriky](data-platform-metrics.md) , ke kterým se dá dostat prostřednictvím sešitů. K metrikám lze v sešitech přicházet prostřednictvím specializovaného ovládacího prvku, který umožňuje určit cílové prostředky, požadované metriky a jejich agregaci. Tato data se pak dají vykreslovat v grafech nebo Gridech.

![Snímek obrazovky s metrikami v grafech metriky využití procesoru](./media/workbooks-overview/metrics-graph.png)

![Snímek obrazovky rozhraní metriky sešitu](./media/workbooks-overview/metrics.png)

## <a name="azure-resource-graph"></a>Azure Resource Graph

Pracovní sešity podporují dotazování na prostředky a jejich metadata pomocí Azure Resource graphu (ARG). Tato funkce se primárně používá k vytváření vlastních oborů dotazů pro sestavy. Rozsah prostředků je vyjádřen prostřednictvím KQL podmnožiny, kterou ARG podporuje – což je často dostačující pro běžné případy použití.

Chcete-li, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam typ dotazu pro výběr grafu prostředků Azure a vyberte předplatná, která chcete cílit. Pomocí ovládacího prvku pro dotaz přidejte ARG KQL-submnožiny, které vyberou zajímavou podmnožinu prostředků.

![Snímek obrazovky s dotazem na KQL v grafu prostředků Azure](./media/workbooks-overview/azure-resource-graph.png)

## <a name="azure-resource-manager"></a>Azure Resource Manager

Sešit podporuje operace REST Azure Resource Manager. Tato možnost umožňuje dotazování koncového bodu management.azure.com bez nutnosti zadat vlastní token autorizační hlavičky.

Chcete-li, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam zdroj dat a vyberte možnost Azure Resource Manager. Zadejte příslušné parametry, jako je například metoda HTTP, cesta URL, záhlaví, parametry adresy URL nebo tělo.

> [!NOTE]
> `GET`V `POST` `HEAD` současné době jsou podporovány pouze operace, a.

## <a name="azure-data-explorer"></a>Průzkumník dat Azure

Sešity teď podporují dotazování z clusterů [Azure Průzkumník dat](/azure/data-explorer/) pomocí výkonného dotazovacího jazyka [Kusto](/azure/kusto/query/index) .   

![Snímek obrazovky s oknem dotazu Kusto](./media/workbooks-overview/data-explorer.png)

## <a name="workload-health"></a>Stav úlohy

Azure Monitor mají funkce, které aktivně monitorují dostupnost a výkon hostovaných operačních systémů Windows nebo Linux. Azure Monitor klíčové komponenty modelů a jejich vztahy, kritéria, jak změřit stav těchto komponent a které komponenty vás upozorní, když se zjistí stav není v pořádku. Pracovní sešity umožňují uživatelům používat tyto informace k vytváření propracovaných interaktivních sestav.

Pokud chcete, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam **typ dotazu** k výběru možnosti stav úloh a vyberte předplatné, skupinu prostředků nebo prostředky virtuálního počítače k cíli. Rozevírací seznam filtru stavu použijte k výběru zajímavé podmnožiny stavových incidentů pro potřeby analýzy.

![Snímek obrazovky s dotazem výstrahy](./media/workbooks-overview/workload-health.png)

## <a name="azure-resource-health"></a>Stav prostředku Azure

Pracovní sešity podporují získání stavu prostředků Azure a jejich kombinaci s jinými zdroji dat a vytvářejí bohatou a interaktivní sestavy o stavu.

Chcete-li, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam **typ dotazu** k výběru možnosti stav služby Azure a vyberte prostředky, které chcete cílit. Pomocí rozevíracího seznamu filtru stavu můžete vybrat zajímavou podmnožinu problémů s prostředky pro potřeby analýzy.

![Snímek obrazovky s dotazem výstrahy](./media/workbooks-overview/resource-health.png)

## <a name="json"></a>JSON

Zprostředkovatel JSON umožňuje vytvořit výsledek dotazu ze statického obsahu JSON. Nejčastěji se používá v parametrech k vytvoření parametrů rozevíracího seznamu statických hodnot. Jednoduchá pole nebo objekty JSON se automaticky převedou na řádky a sloupce mřížky.  Pro přesnější chování můžete nakonfigurovat sloupce pomocí karty výsledky a nastavení JSONPath.

## <a name="alerts-preview"></a>Výstrahy (Preview)

> [!NOTE]
> Doporučeným způsobem, jak dotazovat se na informace o výstrahách Azure, je použít zdroj dat [grafu prostředků Azure](#azure-resource-graph) dotazem na `AlertsManagementResources` tabulku.
>
> Příklady najdete v tématu informace o [tabulce Azure Resource graphu](../../governance/resource-graph/reference/supported-tables-resources.md)nebo v [šabloně výstrahy](https://github.com/microsoft/Application-Insights-Workbooks/blob/master/Workbooks/Azure%20Resources/Alerts/Alerts.workbook) .
>
> Zdroj dat výstrah zůstane k dispozici po určitou dobu, zatímco autoři budou moci přejít na použití ARG. Použití tohoto zdroje dat v šablonách se nedoporučuje. 

Pracovní sešity umožňují uživatelům vizualizovat aktivní výstrahy související s jejich prostředky. Omezení: zdroj dat výstrah vyžaduje přístup pro čtení k předplatnému, aby bylo možné zadat dotaz na prostředky a nemusí zobrazovat novější typy výstrah. 

Chcete-li, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam _zdroj dat_ a zvolte možnost _výstrahy (Preview)_ a vyberte předplatná, skupiny prostředků nebo prostředky, které chcete cílit. Pomocí rozevíracích seznamu filtru výstrah můžete vybrat zajímavou podmnožinu výstrah pro potřeby analýzy.

## <a name="custom-endpoint"></a>Vlastní koncový bod

Pracovní sešity podporují získávání dat z libovolného externího zdroje. Pokud vaše data žijí mimo Azure, můžete je přenést do sešitů pomocí tohoto typu zdroje dat.

Chcete-li, aby ovládací prvek dotazu použil tento zdroj dat, použijte rozevírací seznam _zdroj dat_ a vyberte možnost _vlastní koncový bod_. Zadejte příslušné parametry, například, `Http method` `url` , a `headers` `url parameters` /nebo `body` . Ujistěte se, že zdroj dat podporuje [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) v opačném případě požadavek selže.

Aby nedocházelo k automatickému volání nedůvěryhodných hostitelů při používání šablon, uživatel musí označit používané hostitele jako důvěryhodné. Můžete to udělat tak, že kliknete na tlačítko _Přidat jako důvěryhodné_ nebo když ho přidáte jako důvěryhodného hostitele v nastavení sešitu. Tato nastavení budou uložená v prohlížečích, které podporují IndexDb s webovými pracovníky. Další informace [najdete tady](https://caniuse.com/#feat=indexeddb).

> [!NOTE]
> Nepište žádné tajné kódy do žádného z polí ( `headers` , `parameters` , `body` , `url` ), protože budou viditelné pro všechny uživatele sešitu.

## <a name="next-steps"></a>Další kroky

* [Začínáme](workbooks-visualizations.md) se dozvědět více o seznámcích s mnoha různými možnostmi vizualizací.
* [Řízení](workbooks-access-control.md) a sdílení přístupu k prostředkům sešitu.
* [Tipy pro optimalizaci dotazů Log Analytics](../log-query/query-optimization.md)
* 
