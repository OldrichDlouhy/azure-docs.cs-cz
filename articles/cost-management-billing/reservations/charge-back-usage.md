---
title: Zpětné vyúčtování na rezervace Azure
description: Přečtěte si, jak zobrazovat náklady na rezervace Azure pro zpětné vyúčtování.
author: yashesvi
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 05/14/2020
ms.author: banders
ms.openlocfilehash: 22e2ee897236a2e079f0ce6e230502d5242a02e8
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714338"
---
# <a name="charge-back-azure-reservation-costs"></a>Zpětné vyúčtování na rezervace Azure

Čtenáři fakturace pro smlouvu Enterprise a Smlouvu se zákazníkem Microsoftu si mohou projít údaje o amortizovaných nákladech na rezervace. Tyto údaje o nákladech mohou využít ke zpětnému vyúčtování finanční hodnoty předplatného, skupiny prostředků, prostředku nebo značky pro svoje partnery. V rámci údajů s využitím amortizace jsou platnou cenou náklady na rezervace s poměrným přepočítáním po hodinách. Náklady jsou celkové náklady na využití rezervací prostředku v daném dni.

Uživatelé s individuálním předplatným mohou údaje o amortizovaných nákladech zjistit ze souboru využití. Když prostředek získá slevu za rezervaci, sekce *AdditionalInfo* souboru využití obsahuje podrobnosti o příslušné rezervaci. Další informace najdete v tématu [Stažení informací o využití z webu Azure Portal](https://docs.microsoft.com/azure/cost-management-billing/understand/download-azure-daily-usage#download-usage-from-the-azure-portal-csv).

## <a name="get-reservation-charge-back-data-for-chargeback"></a>Získání údajů dat pro zpětné vyúčtování

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
1. Přejděte do části **Správa nákladů a fakturace**.
1. V části **Skutečné náklady** vyberte metriku **Amortizované náklady**.
1. Pokud chcete zjistit, které prostředky využily konkrétní rezervace, použijte filtr pro **rezervace** a potom vyberte příslušné rezervace.
1. **Úroveň podrobností** nastavte na **Měsíční** nebo **Denní**.
1. Nastavte typ grafu na **Tabulkový**.
1. Možnost **Seskupit podle** nastavte na **Prostředek**.

[![Příklad ukazující náklady na prostředky, které se dají použít pro zpětné vyúčtování](./media/charge-back-usage/amortized-reservation-costs.png)](./media/charge-back-usage/amortized-reservation-costs.png#lightbox)

Tady je video, které ukazuje, jak zobrazit náklady na využití rezervací na webu Azure Portal.

 > [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4sQOw] 

## <a name="need-help-contact-us"></a>Potřebujete pomoc? Kontaktujte nás.

Pokud máte dotazy nebo potřebujete pomoc, [vytvořte žádost o podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Další kroky

- Informace o tom, jak spravovat rezervace, najdete v tématu [Správa rezervací Azure](manage-reserved-vm-instance.md).
- Další informace o rezervacích Azure najdete v následujících článcích:
  - [Co jsou rezervace Azure?](save-compute-costs-reservations.md)
  - [Správa rezervací v Azure](manage-reserved-vm-instance.md)
