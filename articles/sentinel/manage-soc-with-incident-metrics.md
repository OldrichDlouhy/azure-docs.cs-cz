---
title: Lepší správa SOC díky metrikám incidentů ve službě Azure Sentinel | Microsoft Docs
description: Použijte informace z obrazovky a sešitu metriky incidentů služby Azure Sentinel, které vám pomůžou spravovat provozní centrum zabezpečení (SOC).
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2020
ms.author: yelevin
ms.openlocfilehash: f14b0050aefc598d26dec7a7781a3378ccaa7570
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294094"
---
# <a name="manage-your-soc-better-with-incident-metrics"></a>Lepší správa SOC s využitím metrik pro incidenty

> [!IMPORTANT]
> Funkce metriky incidentů jsou momentálně ve verzi Public Preview.
> Tyto funkce se poskytují bez smlouvy o úrovni služeb a nedoporučují se pro produkční úlohy.
> Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Jako správce SOC (Security Operations Center) musíte mít na dosah ruky celkové metriky a míry efektivity, abyste mohli vyhodnocovat výkon svého týmu. V průběhu času budete chtít zobrazit operace incidentu podle mnoha různých kritérií, jako je závažnost, MITRE taktiku, střední doba třídění, Průměrná doba, kterou je třeba vyřešit, a další. Azure Sentinel teď zpřístupňuje tato data s novou tabulkou **SecurityIncident** a schématem v Log Analytics a doprovodném sešitem **efektivity zabezpečení provozu** . Budete moct vizualizovat výkon svého týmu v průběhu času a využít tento přehled ke zvýšení efektivity. Můžete také zapisovat a používat vlastní dotazy KQL proti tabulce incidentů a vytvářet přizpůsobené sešity, které odpovídají vašim konkrétním potřebám auditování a klíčovým ukazatelům výkonu.

## <a name="use-the-security-incidents-table"></a>Použití tabulky incidentů zabezpečení

Tabulka **SecurityIncident** je integrovaná do Azure Sentinel. Najdete ho s ostatními tabulkami v kolekci **SecurityInsights** v části **protokoly**. Můžete se dotazovat na jinou tabulku v Log Analytics.

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incident-table.png" alt-text="Tabulka incidentů zabezpečení":::

Pokaždé, když vytvoříte nebo aktualizujete incident, bude do tabulky přidána nová položka protokolu. To vám umožňuje sledovat změny provedené u incidentů a umožňuje ještě výkonnější SOC metriky, ale při sestavování dotazů pro tuto tabulku musíte být vědomi, že budete možná potřebovat odebrat duplicitní položky pro incident (závisí na přesném dotazu, který používáte). 

Pokud byste například chtěli vrátit seznam všech incidentů seřazených podle čísla incidentu, ale chtěli byste vrátit nejnovější protokol na incident, můžete to udělat pomocí [operátoru KQL sumarizace](https://docs.microsoft.com/azure/data-explorer/kusto/query/summarizeoperator) s `arg_max()` [agregační funkcí](https://docs.microsoft.com/azure/data-explorer/kusto/query/arg-max-aggfunction):

`SecurityIncident` <br>
`| summarize arg_max(LastModifiedTime, *) by IncidentNumber`

## <a name="security-operations-efficiency-workbook"></a>Sešit efektivity zabezpečení provozu

Pokud chcete doplnit tabulku **SecurityIncidents** , poskytli jsme vám předem připravenou šablonu sešitu **efektivity operací zabezpečení** , kterou můžete použít k monitorování operací SOC. Sešit obsahuje následující metriky: 
- Incident vytvořený v průběhu času 
- Incidenty vytvořené uzávěrkou klasifikace, závažnost, vlastník a stav 
- Průměrná doba třídění 
- Střední čas uzavření 
- Incidenty vytvořené závažností, vlastníkem, stavem, produktem a taktiku v průběhu času 
- Čas do třídění percentily 
- Čas do uzavření percentily 
- Průměrná doba třídění podle vlastníka 
- Nedávné aktivity 
- Nedávné uzavírací klasifikace  

Tuto novou šablonu sešitu můžete najít tak, že v nabídce navigace Azure Sentinel kliknete na možnost **sešity** a vyberete kartu **šablony** . z Galerie zvolte možnost **efektivita operací zabezpečení** a klikněte na jedno z tlačítek **Zobrazit uložený sešit** a **Zobrazit šablonu** .

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incidents-workbooks-gallery.png" alt-text="Galerie sešitů s incidenty zabezpečení":::

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-operations-workbook-1.png" alt-text="Sešit incidentů zabezpečení byl dokončen.":::

Šablonu můžete použít k vytvoření vlastních sešitů přizpůsobených vašim konkrétním potřebám.

## <a name="securityincidents-schema"></a>SecurityIncidents schéma

[!INCLUDE [SecurityIncidents schema](../../includes/sentinel-schema-security-incident.md)]

## <a name="next-steps"></a>Další kroky

- Abyste mohli začít používat službu Azure Sentinel, potřebujete Microsoft Azure předplatné. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
- Naučte se, jak začlenit [data do Azure Sentinel](quickstart-onboard.md)a [získat přehled o vašich datech a potenciálních hrozbách](quickstart-get-visibility.md).
