---
title: Zobrazit metriky pomocí Azure Monitor
description: Tento článek ukazuje, jak monitorovat metriky pomocí Azure Portalch grafů a Azure CLI.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 5df104efb65152f5bcb71a86911e694611d8a742
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87022867"
---
# <a name="monitor-media-services-metrics"></a>Monitorování metrik služby Media Services

[Azure monitor](../../azure-monitor/overview.md) vám umožní monitorovat metriky a diagnostické protokoly, které vám pomůžou pochopit, jak vaše aplikace provádí. Podrobný popis této funkce a informace o tom, proč byste chtěli použít Azure Media Services metriky a diagnostické protokoly, najdete v tématu [monitorování metrik Media Services a diagnostických protokolů](media-services-metrics-diagnostic-logs.md).

Azure Monitor poskytuje několik způsobů, jak pracovat s metrikami, včetně jejich grafu na portálu, přístupu k nim přes REST API nebo jejich dotazování pomocí rozhraní příkazového řádku Azure CLI. Tento článek ukazuje, jak monitorovat metriky pomocí Azure Portalch grafů a Azure CLI.

## <a name="prerequisites"></a>Předpoklady

- [Vytvoření účtu Media Services](./create-account-howto.md)
- Kontrola [monitorování Media Services metriky a diagnostické protokoly](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>Zobrazit metriky v Azure Portal

1. Přihlaste se k webu Azure Portal na adrese https://portal.azure.com.
1. Přejděte na účet Azure Media Services a vyberte **metriky**.
1. Klikněte na pole **prostředek** a vyberte prostředek, pro který chcete monitorovat metriky.

    Na pravé straně se zobrazí okno **Vybrat prostředek** se seznamem dostupných zdrojů. V takovém případě se zobrazí:

    * &lt;Název účtu Media Services&gt;
    * &lt;&gt; / &lt; Název koncového bodu streamování názvu Media Services účtu&gt;
    * &lt;název účtu úložiště&gt;

    Vyberte prostředek a stiskněte **použít**. Podrobnosti o podporovaných prostředcích a metrikách najdete v tématu [monitorování Media Services metriky](media-services-metrics-diagnostic-logs.md).

    ![Metriky](media/media-services-metrics/metrics02.png)

    > [!NOTE]
    > Pokud chcete přepínat mezi prostředky, pro které chcete monitorovat metriky, klikněte znovu na pole **prostředek** a opakujte tento krok.
1. (Volitelně) zadejte název grafu (upravte název tak, že stisknete tužku v horní části).
1. Přidejte metriky, které chcete zobrazit.

    ![Metriky](media/media-services-metrics/metrics03.png)
1. Svůj graf můžete připnout na řídicí panel.

## <a name="view-metrics-with-azure-cli"></a>Zobrazení metrik pomocí Azure CLI

Pokud chcete získat metriky "odchozích" pomocí Azure CLI, spusťte následující `az monitor metrics` příkaz:

```azurecli-interactive
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Pokud chcete získat další metriky, nahraďte "výstup" pro název metriky, které vás zajímá.

## <a name="see-also"></a>Viz také

* [Azure Monitor metriky](../../azure-monitor/platform/data-platform.md)
* [Umožňuje vytvářet, zobrazovat a spravovat výstrahy metrik pomocí Azure monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Další kroky

[Diagnostické protokoly](media-services-diagnostic-logs-howto.md)
