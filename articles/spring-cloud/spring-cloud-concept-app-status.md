---
title: Principy stavu aplikací v Azure Spring Cloudu
description: Seznamte se s kategoriemi stavu aplikací v Azure jaře cloudu
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: e3ef202a1a98b8193b55bcc4c2cb616d4a2000d8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037759"
---
# <a name="understanding-app-status-in-azure-spring-cloud"></a>Principy stavu aplikací v Azure Spring Cloudu

Uživatelské rozhraní pro jarní cloudové prostředí Azure poskytuje informace o stavu spuštěných aplikací.  K dispozici je možnost **aplikace** pro každou skupinu prostředků v rámci předplatného, která zobrazuje obecný stav typů aplikací.  Pro každý typ aplikace jsou zobrazeny **instance aplikace**.

## <a name="apps-status"></a>Stav aplikací
Chcete-li zobrazit celkový stav typu aplikace, vyberte možnost **aplikace** v levém navigačním podokně skupiny prostředků. Výsledek zobrazí stav nasazené aplikace:

* **Stav zřizování** zobrazuje stav zřizování nasazení
* **Běžící instance** ukazuje, kolik instancí aplikace je spuštěno/kolik instancí aplikace je žádoucí. Pokud má být aplikace zastavena, zobrazí se v tomto sloupci Stav *Zastaveno*.
* **Registrovaná instance** ukazuje, kolik instancí aplikace je zaregistrované na Eureka/kolik instancí aplikace je žádoucí. Pokud má být aplikace zastavena, zobrazí se v tomto sloupci Stav *Zastaveno*.


 ![Stav aplikací](media/spring-cloud-concept-app-status/apps-ui-status.png)

**Stav nasazení je hlášen jako jedna z následujících hodnot:**

| Výčet | Definice |
|:--:|:----------------:|
| Spuštěný | Nasazení by mělo být spuštěné. |
| Zastaveno | Nasazení by se mělo zastavit. |

**Stav zřizování je přístupný pouze z rozhraní příkazového řádku.  Je hlášena jako jedna z následujících hodnot:**

| Výčet | Definice |
|:--:|:----------------:|
| Vytváření | Prostředek se vytváří. |
| Doplnění | Prostředek se aktualizuje. |
| Úspěšný | Úspěšně dodány prostředky a nasazují binární soubor. |
| Neúspěšný | Nepovedlo se dosáhnout cíle *úspěšného* dokončení. |
| odstraňování | Prostředek se odstraňuje. Tím se zabrání operace a prostředek není v tomto stavu k dispozici. |

## <a name="app-instances-status"></a>Stav instancí aplikace

Pokud chcete zobrazit stav konkrétní instance nasazené aplikace, klikněte na **název** aplikace v uživatelském rozhraní **aplikace** . Zobrazí se výsledky:
* **Stav**: zda instance běží nebo její stav
* **DiscoveryStatus**: registrovaný stav instance aplikace na serveru Eureka

 ![Stav instancí aplikace](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

**Stav instance je hlášen jako jedna z následujících hodnot:**

| Výčet | Definice |
|:--:|:----------------:|
| Spouštění | Binární soubor byl úspěšně nasazen do dané instance. Instance, která spouští soubor JAR, může selhat, protože jar nemůže správně fungovat. |
| Spuštěný | Instance funguje. |
| Neúspěšný | Instance aplikace nedokázala spustit binární soubor uživatele po několika opakovaných pokusech. |
| Ukončena | Instance aplikace se vypíná. |

**Stav zjišťování instance je hlášen jako jedna z následujících hodnot:**

| Výčet | Definice |
|:--:|:----------------:|
| NÁSLEDN | Instance aplikace je zaregistrovaná na Eureka a připravená na příjem provozu. |
| OUT_OF_SERVICE | Instance aplikace je zaregistrovaná v Eureka a schopná přijímat přenosy. ale vypíná se kvůli úmyslnému provozu. |
| DOLŮ | Instance aplikace není zaregistrovaná na Eureka nebo je zaregistrovaná, ale nemůže přijímat přenosy. |


## <a name="see-also"></a>Viz také
* [Příprava pružinové aplikace Java pro nasazení v jarním cloudu Azure](spring-cloud-tutorial-prepare-app-deployment.md)