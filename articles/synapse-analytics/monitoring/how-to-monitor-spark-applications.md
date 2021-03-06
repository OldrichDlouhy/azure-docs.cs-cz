---
title: Jak monitorovat aplikace Apache Spark v synapse studiu
description: Pomocí synapse studia můžete monitorovat aplikace Apache Spark.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: a4dc2604dbd62da1baa4278ff3463f41337886bf
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87052497"
---
# <a name="use-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Použití synapse studia (Preview) k monitorování aplikací Apache Spark

Pomocí služby Azure synapse Analytics můžete pomocí Sparku spouštět poznámkové bloky, úlohy a jiné druhy aplikací ve vašich fondech Spark ve vašem pracovním prostoru.

Tento článek vysvětluje, jak monitorovat aplikace Apache Spark, což vám umožní sledovat nejnovější stav, problémy a průběh.

## <a name="accessing-the-list-of-apache-spark-applications"></a>Přístup k seznamu Apache Spark aplikací

Pokud chcete zobrazit seznam aplikací Apache Spark ve vašem pracovním prostoru, nejdřív [otevřete synapse Studio](https://web.azuresynapse.net/) a vyberte svůj pracovní prostor.

![Přihlášení k pracovnímu prostoru](./media/common/login-workspace.png)

Po otevření pracovního prostoru vyberte na levé straně část **monitorování** .

![Výběr centra monitorování](./media/common/left-nav.png)

Pokud chcete zobrazit seznam aplikací Apache Spark, vyberte **Apache Spark aplikace** .

 ![Vybrat aplikace Spark](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-apache-spark-applications"></a>Filtrování aplikací Apache Spark

Seznam aplikací Apache Spark můžete filtrovat do těch, které vás zajímají. Filtry v horní části obrazovky umožňují zadat pole, podle kterého chcete filtrovat.

Můžete například filtrovat zobrazení a zobrazit pouze aplikace Apache Spark, které obsahují název "prodej":

![Tlačítko Filtr](./media/common/filter-button.png)

![Vzorový filtr](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-apache-spark-application"></a>Zobrazení podrobností o konkrétní Apache Spark aplikaci

Chcete-li zobrazit podrobnosti o jedné z aplikací Apache Spark, vyberte aplikaci Apache Spark a Prohlédněte si podrobnosti. Pokud je aplikace Apache Spark stále spuštěná, můžete monitorovat průběh. [Další informace](apache-spark-applications.md).

## <a name="next-steps"></a>Další kroky

Další informace o monitorování spuštění kanálu najdete v článku [monitorování kanálu spuštění synapse studia](how-to-monitor-pipeline-runs.md) . 

Další informace o ladění Apache Spark aplikace najdete v článku [monitorování Apache Spark aplikace v synapse studiu](apache-spark-applications.md) .