---
title: Vytváření incidentů z upozornění v ověřovacích událostech Azure | Microsoft Docs
description: Naučte se vytvářet incidenty z výstrah v Azure Sentinel.
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
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 1593b96ae8412632120e8977635a4193996ca88d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "82025114"
---
# <a name="automatically-create-incidents-from-microsoft-security-alerts"></a>Automatické vytváření incidentů z výstrah zabezpečení společnosti Microsoft

Výstrahy aktivované v řešeních zabezpečení Microsoftu, která jsou připojená ke službě Azure Sentinel, jako je Microsoft Cloud App Security a Rozšířená ochrana před internetovými útoky, nevytvářejí v Azure Sentinel automaticky incidenty. Ve výchozím nastavení platí, že když připojíte řešení Microsoftu ke službě Azure Sentinel, všechna upozornění vygenerovaná v této službě se uloží jako nezpracovaná data v Azure Sentinel v tabulce výstrahy zabezpečení v pracovním prostoru Azure Sentinel. Tato data pak můžete použít stejně jako jakákoli jiná nezpracovaná data, ke kterým se připojíte pomocí ověřovacích dat.

Pomocí pokynů v tomto článku můžete snadno nakonfigurovat službu Azure Sentinel tak, aby automaticky vytvářela incidenty pokaždé, když se aktivuje výstraha v rámci připojeného řešení zabezpečení Microsoftu.

## <a name="prerequisites"></a>Požadavky
Aby bylo možné vytvářet incidenty z výstrah služby zabezpečení, je nutné [připojit řešení zabezpečení společnosti Microsoft](connect-data-sources.md#data-connection-methods) .

## <a name="using-microsoft-security-incident-creation-analytic-rules"></a>Pomocí analytických pravidel pro vytváření incidentů zabezpečení Microsoftu

Pomocí předdefinovaných pravidel dostupných v Azure Sentinel můžete zvolit, která připojená řešení zabezpečení Microsoftu by měla v reálném čase vytvářet incidenty Sentinel Azure automaticky. Můžete také upravit pravidla a definovat konkrétnější možnosti pro filtrování, které výstrahy vygenerované řešením zabezpečení Microsoftu by měly vytvářet incidenty v Azure Sentinel. Můžete se třeba rozhodnout vytvořit incidenty Sentinel Azure automaticky jenom z Azure Security Center výstrahy s vysokou závažností.

1. V Azure Portal v části Azure Sentinel vyberte **Analytics**.

1. Vyberte kartu **šablony pravidel** a zobrazte všechna předdefinovaná analytická pravidla.

    ![Šablony pravidel](media/incidents-from-alerts/rule-templates.png)

1. Vyberte šablonu pravidla **Microsoft Security** Analytics, kterou chcete použít, a klikněte na **vytvořit pravidlo**.

    ![Pravidlo analýzy zabezpečení](media/incidents-from-alerts/security-analytics-rule.png)

1. Můžete upravit podrobnosti pravidla a vybrat, jestli se mají filtrovat výstrahy, které vytvoří incidenty podle závažnosti výstrahy nebo podle textu obsaženého v názvu výstrahy.  
      
    Pokud například **v poli** filtr podle závažnosti zvolíte možnost **Azure Security Center** a v poli **filtrovat podle závažnosti** zvolíte možnost **Vysoká** , budou automaticky ve službě Azure Sentinel automaticky vytvořeny pouze vysoké závažnost Azure Security Center výstrahy.  

    ![Průvodce vytvořením pravidla](media/incidents-from-alerts/create-rule-wizard.png)

1. Můžete také vytvořit nové pravidlo **zabezpečení společnosti Microsoft** , které filtruje výstrahy z různých služeb zabezpečení společnosti Microsoft kliknutím na **+ vytvořit** a vybrat **pravidlo pro vytvoření incidentu Microsoft**.

    ![Pravidlo vytvoření incidentu](media/incidents-from-alerts/incident-creation-rule.png)

  Pro každý typ **služby zabezpečení Microsoft** můžete vytvořit více než jedno pravidlo analytického **zabezpečení společnosti Microsoft** . Tím se nevytvoří duplicitní incidenty, protože každé pravidlo se používá jako filtr. I v případě, že se výstraha shoduje s více než jedním pravidlem analytického řešení **společnosti Microsoft** , vytvoří se jenom jeden incident Sentinel Azure.

## <a name="enable-incident-generation-automatically-during-connection"></a>Povolit generování incidentů automaticky během připojení
 Když se připojíte k řešení zabezpečení Microsoftu, můžete vybrat, jestli chcete, aby výstrahy z řešení zabezpečení automaticky generovaly incidenty v Azure Sentinel automaticky.

1. Připojte zdroj dat řešení Microsoft Security. 

   ![Generovat incidenty zabezpečení](media/incidents-from-alerts/generate-security-incidents.png)

1. V části **vytvořit incidenty** vyberte **Povolit** , pokud chcete povolit výchozí analytické pravidlo, které automaticky vytvoří incidenty z výstrah vygenerovaných v připojené službě zabezpečení. Toto pravidlo pak můžete upravit v části **Analýza** a pak na **aktivní pravidla**.

## <a name="next-steps"></a>Další kroky

- Abyste mohli začít používat službu Azure Sentinel, potřebujete Microsoft Azure předplatné. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
- Naučte se, jak začlenit [data do Azure Sentinel](quickstart-onboard.md)a [získat přehled o vašich datech a potenciálních hrozbách](quickstart-get-visibility.md).
