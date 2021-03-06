---
title: Omezení rozsahu nasazení Update Management Azure Automation
description: V tomto článku se dozvíte, jak používat konfigurace oboru k omezení rozsahu nasazení Update Management.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 8770762fa2d2ae6bc0584d75397829298a62e8c0
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185887"
---
# <a name="limit-update-management-deployment-scope"></a>Omezení rozsahu nasazení Update Management

Tento článek popisuje, jak pracovat s konfiguracemi oboru při použití funkce [Update Management](automation-update-management.md) k nasazení aktualizací a oprav do vašich virtuálních počítačů. Další informace najdete v tématu [cílení řešení monitorování v Azure monitor (Preview)](../azure-monitor/insights/solution-targeting.md). 

## <a name="about-scope-configurations"></a>O konfiguracích oboru

Konfigurace oboru je skupina jednoho nebo několika uložených hledání (dotazů) používaných k omezení rozsahu Update Management na konkrétní počítače. Konfigurace oboru se používá v pracovním prostoru Log Analytics pro cílení na počítače, které chcete povolit. Když přidáte počítač pro příjem aktualizací z Update Management, počítač se přidá i do uloženého hledání v pracovním prostoru.

## <a name="set-the-scope-limit"></a>Nastavení omezení rozsahu

Omezení rozsahu nasazení Update Management:

1. V účtu Automation vyberte v části **související prostředky**možnost **propojený pracovní prostor** .

2. Klikněte na **Přejít k pracovnímu prostoru**.

3. V části **zdroje dat pracovního prostoru**vyberte **Konfigurace oboru (Preview)** .

4. Vyberte tři tečky napravo od `MicrosoftDefaultScopeConfig-Updates` Konfigurace oboru a klikněte na **Upravit**. 

5. V podokně úpravy rozbalte položku **Vybrat skupiny počítačů**. V podokně skupiny počítačů se zobrazí uložená hledání, která slouží k vytvoření konfigurace oboru. Uložené hledání, které používá Update Management, je:

    |Název     |Kategorie  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     | Aktualizace        | Updates__MicrosoftDefaultComputerGroup         |

6. Vyberte uložené hledání, které chcete zobrazit, a upravte dotaz, který jste použili k naplnění skupiny. Následující obrázek znázorňuje dotaz a jeho výsledky:

    ![Uložená hledání](media/automation-scope-configurations-update-management/logsearch.png)

## <a name="next-steps"></a>Další kroky

* Informace o tom, jak používat tuto funkci, najdete v tématu [Správa aktualizací a oprav pro virtuální počítače Azure](automation-tutorial-update-management.md).
* Informace o řešení chyb funkcí najdete v tématu [řešení potíží s Update Management](troubleshoot/update-management.md).
* Problémy s chybami agenta Windows Update najdete v tématu řešení potíží s [agentem pro Windows Update](troubleshoot/update-agent-issues.md).
* Problémy s chybami agenta aktualizací pro Linux najdete v tématu [řešení potíží s aktualizacemi agenta pro Linux](troubleshoot/update-agent-issues-linux.md).
