---
title: Připojení počítačů s Windows ke službě Azure Security Center
description: V tomto rychlém startu se dozvíte, jak zřídit agenta Log Analytics v počítači s Windows.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: ecbdb6dc73082fc0fab04c8802c43219cabb6a79
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519758"
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Rychlý start: Připojení počítačů s Windows ke službě Azure Security Center
Po připojení předplatných Azure můžete povolit Security Center pro prostředky běžící mimo Azure, například místně nebo v jiných cloudech, zřizováním agenta Log Analytics.

V tomto rychlém startu se dozvíte, jak nainstalovat agenta Log Analytics do počítače s Windows.

## <a name="prerequisites"></a>Předpoklady
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Pokud nemáte předplatné, můžete si vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

Než začnete s tímto rychlým startem, musíte být v Security Center cenové úrovně Standard. Pokyny k upgradu najdete v tématu [Připojení předplatného Azure ke službě Security Center úrovně Standard](security-center-get-started.md). Můžete vyzkoušet Standard Security Center na žádné náklady. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="add-new-windows-computer"></a>Přidání nového počítače s Windows

1. Přihlaste se k webu [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. V nabídce **Microsoft Azure** vyberte možnost **Security Center**. **Security Center –** zobrazí se přehled.

   ![Přehled služby Security Center][2]

3. V hlavní nabídce služby Security Center vyberte **Začínáme**.
4. Vyberte kartu **Začínáme**.

   ![Začínáme][3]

5. V části **Přidat počítače umístěné mimo Azure** klikněte na **Konfigurovat**. Zobrazí se seznam vašich pracovních prostorů Log Analytics. Seznam obsahuje i výchozí pracovní prostor, který pro vás vytvořila služba Security Center při povolení automatického zřizování, pokud existuje. Vyberte tento nebo jiný pracovní prostor, který chcete použít.

    ![Přidání počítače umístěného mimo Azure](./media/quick-onboard-windows-computer/non-azure.png)

   Otevře se okno **Přímý agent** s odkazem na stažení agenta pro Windows a klíči pro ID vašeho pracovního prostoru, které použijete při konfiguraci agenta.

6. Vyberte odkaz **Stáhnout agenta pro Windows** odpovídající typu procesoru vašeho počítače a stáhněte instalační soubor.

7. Napravo od **ID pracovního prostoru** vyberte ikonu kopírování a vložte ID do Poznámkového bloku.

8. Napravo od **Primárního klíče** vyberte ikonu kopírování a vložte klíč do Poznámkového bloku.

## <a name="install-the-agent"></a>Instalace agenta
Teď je potřeba stažený soubor nainstalovat na cílový počítač.

1. Zkopírujte soubor do cílového počítače a spusťte instalaci.
2. Na **úvodní** stránce vyberte **Další**.
3. Na stránce **Licenční podmínky** si přečtěte licenční podmínky a pak vyberte **Souhlasím**.
4. Na stránce **Cílová složka** změňte nebo ponechte výchozí instalační složku a pak vyberte **Další**.
5. Na stránce **Možnosti instalace agenta** zvolte připojení agenta k Azure Log Analytics a pak vyberte **Další**.
6. Na stránce **Azure Log Analytics** vložte **ID pracovního prostoru** a **Klíč pracovního prostoru (Primární klíč)**, které jste zkopírovali do Poznámkového bloku v rámci předchozího postupu.
7. Pokud se má počítač hlásit do pracovního prostoru služby Log Analytics v cloudu Azure Government, vyberte z rozevíracího seznamu **Cloud Azure** možnost **Azure US Government**. Pokud počítač potřebuje komunikovat se službou Log Analytics přes proxy server, vyberte **Upřesnit** a zadejte adresu URL a číslo portu proxy serveru.
8. Jakmile dokončíte zadávání nezbytných nastavení konfigurace, vyberte **Další**.

   ![Instalace agenta][5]

9. Na stránce **Připraveno k instalaci** zkontrolujte zvolené volby a pak vyberte **Nainstalovat**.
10. Na stránce **Konfigurace byla úspěšně dokončena** vyberte **Dokončit**.

Po dokončení se **agent Log Analytics** zobrazí v **Ovládacích panelech**. Tam můžete zkontrolovat svou konfiguraci a ověřit připojení agenta.

Další informace o instalaci a konfiguraci agenta najdete v tématu popisujícím [připojení počítačů s Windows](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Teď můžete na jednom místě monitorovat své virtuální počítače Azure i počítače umístěné mimo Azure. V části **Compute** najdete přehled všech virtuálních počítačů a počítačů i s doporučeními. Každý sloupec představuje jednu sadu doporučení. Barva doporučení představuje aktuální stav zabezpečení virtuálního počítače nebo počítače. Security Center navíc zobrazí všechny detekce pro tyto počítače ve výstrahách upozornění.

  ![Okno Compute][6]

V okně **Compute** se zobrazují dva typy ikon:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Počítač umístěný mimo Azure

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Virtuální počítač Azure

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud už ho nepotřebujete, můžete agenta z počítače s Windows odebrat.

Odebrání agenta:

1. Otevřete **Ovládací panely**.
2. Otevřete **Programy a funkce**.
3. V nabídce **programy a funkce**vyberte **Log Analytics agent** a klikněte na **odinstalovat**.

## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste zřídili agenta Log Analytics v počítači se systémem Windows. Další informace o tom, jak používat Security Center, najdete v následném kurzu o konfiguraci zásad zabezpečení a o vyhodnocení zabezpečení vašich prostředků.

> [!div class="nextstepaction"]
> [Kurz: Definování a vyhodnocení zásad zabezpečení](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
