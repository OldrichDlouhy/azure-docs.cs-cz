---
title: Jak provést samočinnou diagnostiku a řešení problémů v Azure jaře cloudu
description: Naučte se, jak sami diagnostikovat a řešit problémy v Azure jarním cloudu.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 05/29/2020
ms.custom: devx-track-java
ms.openlocfilehash: 1f48cd185f2aa25954627a9ff26f3e59b8e76e7a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037308"
---
# <a name="how-to-self-diagnose-and-solve-problems-in-azure-spring-cloud"></a>Jak provést samočinnou diagnostiku a řešení problémů v Azure jaře cloudu
Diagnostika Azure jaře Cloud je interaktivní prostředí, které vám může pomoct při odstraňování potíží s aplikací. Není nutná žádná konfigurace. Když najdete problémy, diagnostika cloudu Azure vyhledá chybu a provede vás s informacemi, které vám pomůžou problém vyřešit a vyřešit.

## <a name="prerequisites"></a>Předpoklady
Pro absolvování tohoto kurzu potřebujete:

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.
* Nasazená instance služby jarní cloudová služba Azure Pokud chcete začít, postupujte podle našeho [rychlého startu na webu nasazení aplikace prostřednictvím rozhraní příkazového řádku Azure CLI](spring-cloud-quickstart-launch-app-cli.md) .
* V instanci služby už je vytvořená aspoň jedna aplikace.

## <a name="navigate-to-the-diagnostics-page"></a>Přejít na stránku Diagnostika
1. Přihlaste se k webu Azure Portal.
2. Přejít na stránku s **přehledem** jarního cloudu Azure.
3. Otevřete **diagnostiku a řešení problémů** v nabídce na levé straně stránky.

 ![Diagnostika, dialogové okno řešení](media/spring-cloud-diagnose/diagnose-solve-dialog.png)

 ## <a name="search-logged-issues"></a>Hledat zaznamenané problémy
Chcete-li zjistit problém, můžete hledat zadáním klíčového slova nebo kliknutím na možnost Skupina řešení a prozkoumat vše v této kategorii.

 ![Problémy vyhledávání](media/spring-cloud-diagnose/search-detectors.png)

Po výběru **kontroly stavu konfiguračního serveru**, **stavu konfiguračního serveru**nebo **Historie aktualizací serveru** se zobrazí různé výsledky.

![Možnosti problémů](media/spring-cloud-diagnose/detectors-options.png)

Najděte svůj cílový detektor a kliknutím ho spusťte. Po spuštění detektoru se zobrazí souhrn diagnostiky. Můžete vybrat **Zobrazit úplnou sestavu** pro kontrolu diagnostických podrobností nebo kliknutím na tlačítko **Zobrazit dlaždici** přejít zpátky na seznam detektorů.

 ![Souhrnná Diagnostika](media/spring-cloud-diagnose/summary-diagnostics.png)

Na stránce diagnostické podrobnosti můžete změnit časový rozsah diagnostiky pomocí kontroleru v pravém horním rohu. Chcete-li zobrazit další metriky nebo protokoly, přepněte jednotlivé diagnostiky. Pro metriky a protokoly může trvat 15 minut.

 ![Podrobnosti diagnostiky](media/spring-cloud-diagnose/diagnostics-details.png)

Některé výsledky obsahují související dokumentaci.

 ![Související podrobnosti](media/spring-cloud-diagnose/related-details.png)

## <a name="next-steps"></a>Další kroky
* [Monitorování prostředků Spring Cloudu s využitím výstrah a skupin akcí](spring-cloud-tutorial-alerts-action-groups.md)
* [Ovládací prvky zabezpečení pro službu Azure Spring Cloud](spring-cloud-concept-security-controls.md)