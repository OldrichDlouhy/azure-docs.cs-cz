---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d2df7388018c463ba58b57be46945915210b84b6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "67175205"
---
#### <a name="to-install-an-update-from-the-azure-portal"></a>Instalace aktualizace z webu Azure Portal

1. Na stránce služby StorSimple vyberte zařízení.

    ![Vybrat zařízení](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. Přejděte na **nastavení zařízení**  >  **aktualizace zařízení**.

    ![Klikněte na aktualizace zařízení.](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. Pokud jsou k dispozici nové aktualizace, zobrazí se oznámení. Případně můžete v okně **aktualizace zařízení** kliknout na **Vyhledat aktualizace**. Vytvoří se úloha vyhledávání dostupných aktualizací. Po úspěšném dokončení úlohy se zobrazí zpráva.

    ![Klikněte na aktualizace zařízení.](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. Doporučujeme, abyste si před instalací aktualizace na zařízení prošli poznámky k verzi. Chcete-li použít aktualizace, klikněte na tlačítko **instalovat aktualizace**. V okně **Potvrdit běžné aktualizace** si přečtěte předpoklady, které je potřeba provést před instalací aktualizací. Zaškrtnutím políčka označíte, že jste připraveni aktualizovat zařízení, a pak klikněte na **nainstalovat**.

    ![Klikněte na aktualizace zařízení.](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. Spustí se sada kontrol požadavků. Mezi tyto kontroly patří:
   
   * **Kontroly stavu kontrolerů** pro ověření, že oba kontrolery zařízení jsou v pořádku a online.
   * **Kontroly stavu hardwarových komponent** pro ověření, že všechny hardwarové komponenty v zařízení StorSimple jsou v pořádku.
   * **Kontroly rozhraní DATA 0** pro ověření, že je na zařízení povolené rozhraní DATA 0. Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.

     Aktualizace se stáhne a nainstaluje jenom v případě, že se všechny kontroly úspěšně dokončily. Na probíhání kontrol budete upozorněni. Pokud předkontroly selžou, budete mít k dispozici důvody pro selhání. Vyřešte tyto problémy a potom operaci opakujte. Pokud si s těmito problémy neporadíte sami, bude muset kontaktovat podporu Microsoftu.

7. Po úspěšném dokončení předkontrol se vytvoří úloha aktualizace. Po úspěšném vytvoření úlohy aktualizace se zobrazí upozornění.
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    Aktualizace se potom nainstaluje na vašem zařízení.

9. Dokončení aktualizace trvá několik hodin. Podrobnosti o úloze můžete kdykoli zobrazit výběrem úlohy aktualizace a kliknutím na **Podrobnosti**.

    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update8.png)

     Můžete také monitorovat průběh úlohy aktualizace z **nastavení zařízení > úloh**. V okně **úlohy** vidíte průběh aktualizace.

     ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. Po dokončení úlohy přejděte do **nastavení zařízení > aktualizace zařízení**. Verze softwaru by se teď měla aktualizovat.

