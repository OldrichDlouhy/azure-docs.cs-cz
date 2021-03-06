---
title: Instalace aktualizace na zařízení s Azure Data Box Gateway Series | Microsoft Docs
description: Popisuje, jak použít aktualizace pomocí Azure Portal a místního webového uživatelského rozhraní pro zařízení Azure Data Box Gateway Series.
services: databox
author: priestlg
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: v-grpr
ms.openlocfilehash: 4c17488a875484b2d3dc0e7e8e1045ce8ea75cf0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85802133"
---
# <a name="update-your-azure-data-box-gateway"></a>Aktualizace Azure Data Box Gateway

Tento článek popisuje kroky potřebné k instalaci aktualizace do Azure Data Box Gateway prostřednictvím místního webového uživatelského rozhraní a prostřednictvím Azure Portal. Aktualizace softwaru a opravy hotfix můžete použít, chcete-li udržovat Data Box Gateway zařízení v aktuálním stavu.

> [!IMPORTANT]
>
> - Aktualizace **1911** odpovídá verzi **1.6.1049.786** softwaru na vašem zařízení. Další informace o této aktualizaci najdete v [poznámkách k verzi](data-box-gateway-1911-release-notes.md).
>
> - Mějte na paměti, že při instalaci aktualizace nebo opravy hotfix se zařízení restartuje. Vzhledem k tom, že Data Box Gateway je zařízení s jedním uzlem, dojde k přerušení všech vstupně-výstupních operací a v zařízení dojde k výpadku až 30 minut od aktualizace softwaru zařízení.

Každý z těchto kroků je popsaný v následujících částech.

## <a name="use-the-azure-portal"></a>Použití webu Azure Portal

Doporučujeme nainstalovat aktualizace prostřednictvím Azure Portal. Zařízení automaticky vyhledává aktualizace jednou denně. Až budou aktualizace k dispozici, zobrazí se na portálu oznámení. Pak můžete stáhnout a nainstalovat aktualizace.

> [!NOTE]
> Než budete pokračovat v instalaci aktualizací, ujistěte se, že je zařízení v pořádku a zobrazuje se stav **online** .

1. Až budou aktualizace k dispozici pro vaše zařízení, zobrazí se oznámení. Vyberte oznámení nebo na horním panelu příkazů **aktualizujte zařízení**. To vám umožní použít aktualizace softwaru zařízení.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. V okně **aktualizace zařízení** ověřte, že jste si přečetli licenční smlouvy spojené s novými funkcemi v poznámkách k verzi.

    Můžete si **Stáhnout a nainstalovat** aktualizace nebo **Stáhnout** aktualizace. Pak se můžete rozhodnout nainstalovat tyto aktualizace později.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Pokud chcete stáhnout a nainstalovat aktualizace, ověřte možnost, že se aktualizace po dokončení stahování automaticky nainstalují.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Spustí se stahování aktualizací. Zobrazí se oznámení o tom, že probíhá stahování.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    V Azure Portal se zobrazí také nápis s oznámením. To indikuje průběh stahování.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    Můžete vybrat toto oznámení nebo vybrat **aktualizovat zařízení** a zobrazit podrobný stav aktualizace.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. Po dokončení stahování se banner oznámení aktualizuje, aby označoval dokončení. Pokud jste se rozhodli stáhnout a nainstalovat aktualizace, bude instalace automaticky zahájena.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Pokud se rozhodnete stahovat pouze aktualizace, vyberte oznámení a otevřete okno **aktualizace zařízení** . Vyberte **Nainstalovat**.
  
    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. Zobrazí se oznámení o tom, že probíhá instalace.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    Portál také zobrazí informační výstrahu, která indikuje, že instalace probíhá. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Vzhledem k tomu, že se jedná o zařízení s jedním uzlem, po instalaci aktualizací se zařízení restartuje. Kritická výstraha při restartování bude znamenat, že dojde ke ztrátě prezenčního signálu zařízení.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    Vyberte výstrahu, aby se zobrazila odpovídající událost zařízení.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. Po instalaci aktualizací se stav zařízení aktualizuje na **online** .

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    V horním panelu příkazů vyberte **aktualizace zařízení**. Ověřte, že se aktualizace úspěšně nainstalovala a že verze softwaru zařízení bude odpovídat.

    ![Verze softwaru po aktualizaci](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>Použití místního webového uživatelského rozhraní

Existují dva kroky při použití místního webového uživatelského rozhraní:

- Stáhnout aktualizaci nebo opravu hotfix
- Instalace aktualizace nebo opravy hotfix

Každý z těchto kroků je podrobně popsán v následujících částech.

### <a name="download-the-update-or-the-hotfix"></a>Stáhnout aktualizaci nebo opravu hotfix

Chcete-li stáhnout aktualizaci, proveďte následující kroky. Aktualizaci si můžete stáhnout z umístění dodaného společností Microsoft nebo z katalogu Microsoft Update.

Chcete-li stáhnout aktualizaci z katalogu Microsoft Update, proveďte následující kroky.

1. Spusťte prohlížeč a přejděte na adresu [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

   ![Prohledávání katalogu](./media/data-box-gateway-apply-updates/download-update-1.png)

2. Do vyhledávacího pole katalogu Microsoft Update zadejte číslo opravy hotfix nebo podmínek pro aktualizaci, kterou chcete stáhnout, do znalostní báze (KB). Zadejte například **Azure Data box Gateway**a pak klikněte na **Hledat**.

   Výpis aktualizace se zobrazí jako **Azure Data Box Gateway 1911**.

   ![Prohledávání katalogu](./media/data-box-gateway-apply-updates/download-update-2.png)

3. Vyberte **Download** (Stáhnout). Existuje jeden soubor ke stažení s názvem *SoftwareUpdatePackage.exe* , který odpovídá aktualizaci softwaru zařízení. Stáhněte soubor do složky v místním systému. Můžete také zkopírovat složku do síťové sdílené složky, která je dosažitelná ze zařízení.

   ![Prohledávání katalogu](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>Instalace aktualizace nebo opravy hotfix

Před instalací aktualizace nebo oprav hotfix se ujistěte, že:

- Máte aktualizaci nebo opravu hotfix staženou místně na svém hostiteli nebo přístupnou přes sdílenou síťovou složku.
- Stav zařízení je v pořádku, jak je znázorněno na stránce **Přehled** místního webového uživatelského rozhraní.

   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Dokončení tohoto postupu trvá přibližně 20 minut. Provedením následujících kroků nainstalujete aktualizaci nebo opravu hotfix.

1. V místním webovém uživatelském rozhraní přejdete na **Údržba**  >  **aktualizace softwaru**. Poznamenejte si verzi softwaru, kterou používáte.

   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Zadejte cestu k souboru aktualizace. Můžete také přejít k instalačnímu souboru aktualizace, pokud je umístěn ve sdílené síťové složce. Vyberte soubor aktualizace softwaru s příponou *SoftwareUpdatePackage.exe* .

   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. Vyberte **Použít**.

   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. Po zobrazení výzvy k potvrzení vyberte **Ano** a pokračujte. Když je zařízení s jedním uzlem, po použití aktualizace se zařízení restartuje a dojde k výpadku.
   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. Spustí se aktualizace. Po úspěšné aktualizaci zařízení se restartuje. Místní uživatelské rozhraní není v tuto dobu k dispozici.

6. Po dokončení restartování přejdete na **přihlašovací** stránku. Chcete-li ověřit, zda byl software zařízení aktualizován, v místním webovém uživatelském rozhraní, **Maintenance**navštivte web  >  **aktualizace softwaru**údržba. Zobrazená verze softwaru v tomto příkladu je **1.6.1049.786**.

   ![aktualizace zařízení](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [správě Azure Data box Gateway](data-box-gateway-manage-users.md).
