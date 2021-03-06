---
title: Řešení potíží s připojením synapse Studio (Preview) pomocí PowerShellu
description: Řešení potíží s připojením ke službě Azure synapse Studio pomocí PowerShellu
author: julieMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: f4afaf536a9c65758ad030e5cdeeee5fb97074d7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87046672"
---
# <a name="diagnose-azure-synapse-studio-preview-connectivity-issues-with-powershell-script"></a>Diagnostika potíží s připojením ke službě Azure synapse Studio (Preview) pomocí skriptu PowerShellu

Azure synapse Studio (Preview) závisí na sadě koncových bodů webového rozhraní API, aby fungovaly správně. Tato příručka vám pomůže identifikovat příčiny potíží s připojením, když máte tyto možnosti:
- Konfigurace místní sítě (například sítě za podnikovou bránou firewall) pro přístup k Azure synapse studiu.
- nastávají problémy s připojením pomocí Azure synapse studia.

## <a name="prerequisite"></a>Požadavek

* PowerShell 5,0 nebo vyšší verze ve Windows nebo
* PowerShell Core 6,0 nebo vyšší verze v systému Windows nebo Linux.

## <a name="troubleshooting-steps"></a>Postup při řešení potíží

Klikněte pravým tlačítkem na následující odkaz a klikněte na Uložit cíl jako:

- [Test-AzureSynapse.ps1](https://go.microsoft.com/fwlink/?linkid=2119734)

Alternativně můžete odkaz otevřít přímo a uložit otevřený soubor skriptu. Neuloží adresu odkazu výše, jak se může v budoucnu změnit.

V Průzkumníku souborů klikněte pravým tlačítkem na stažený soubor skriptu a klikněte na spustit s prostředím PowerShell.

![Spustit stažený soubor skriptu pomocí PowerShellu](media/troubleshooting-synapse-studio-powershell/run-with-powershell.png)

Po zobrazení výzvy zadejte název pracovního prostoru Azure synapse, který aktuálně má problém nebo který chcete otestovat pro připojení, a stiskněte klávesu ENTER.

![Zadejte název pracovního prostoru.](media/troubleshooting-synapse-studio-powershell/enter-workspace-name.png)

Spustí se diagnostická relace. Počkejte, než se dokončí.

![Počkat na dokončení diagnostiky](media/troubleshooting-synapse-studio-powershell/wait-for-diagnosis.png)

Na konci se zobrazí souhrn diagnostiky. Pokud se Váš počítač nemůže připojit k jednomu nebo více koncovým bodům, zobrazí se v části Shrnutí některé návrhy.

![Přehled diagnostiky](media/troubleshooting-synapse-studio-powershell/diagnosis-summary.png)

Kromě toho se soubor diagnostického protokolu pro tuto relaci vygeneruje ve stejné složce jako skript pro odstraňování potíží. Jeho umístění se zobrazuje v části Obecné tipy ( `D:\TestAzureSynapse_2020....log` ). V případě potřeby můžete tento soubor poslat technické podpoře.

Pokud jste správcem sítě a vyladěním konfigurace brány firewall pro Azure synapse Studio, může vám pomáhat technické podrobnosti zobrazené výše v části "Souhrn".

* Všechny položky testu (požadavky) označené "Pass" znamenají, že prošly testy připojení bez ohledu na stavový kód HTTP.
 V případě neúspěšných požadavků je důvod zobrazen žlutě, například `NamedResolutionFailure` nebo `ConnectFailure` . Tyto důvody vám pomůžou zjistit, jestli se v síťovém prostředí vyskytly neúspěšné konfigurace.


## <a name="next-steps"></a>Další kroky
Pokud vám předchozí kroky nepomohly vyřešit problém s [vytvořením lístku podpory](../../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md).
