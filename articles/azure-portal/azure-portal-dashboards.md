---
title: Vytváření a sdílení řídicích panelů na webu Azure Portal
description: Tento článek popisuje, jak vytvářet, přizpůsobovat, publikovat a sdílet řídicí panely v Azure Portal.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/23/2020
ms.author: mblythe
ms.openlocfilehash: 06cf0a5692b3d80a07c305f53c9e3801cbdb5110
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84764005"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Vytváření a sdílení řídicích panelů na webu Azure Portal

Řídicí panely jsou cílené a organizované zobrazení vašich cloudových prostředků v Azure Portal. Řídicí panely slouží jako pracovní prostor, kde můžete rychle spouštět úlohy pro každodenní operace a monitorovat prostředky. Můžete například vytvářet vlastní řídicí panely založené na projektech, úkolech nebo rolích uživatelů.

Azure Portal jako výchozí bod poskytuje výchozí řídicí panel. Můžete upravit výchozí řídicí panel. Vytváření a přizpůsobení dalších řídicích panelů a publikování a sdílení řídicích panelů, aby je uživatelé mohli zpřístupnit ostatním uživatelům. Tento článek popisuje postup vytvoření nového řídicího panelu, přizpůsobení rozhraní a publikování a sdílení řídicích panelů.

## <a name="create-a-new-dashboard"></a>Vytvoření nového řídicího panelu

V tomto příkladu vytvoříme nový privátní řídicí panel a přiřadíme mu název. Začněte těmito kroky:

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com).

1. V nabídce Azure Portal vyberte možnost **řídicí panel**. Výchozí zobrazení již může být nastaveno na řídicí panel.

    ![Otevření řídicího panelu](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. Vyberte **nový řídicí panel**.

    ![Snímek obrazovky s novým řídicím panelem](./media/azure-portal-dashboards/create-new-dashboard.png)

    Tato akce otevře **galerii dlaždic**, ze které se budou vybírat dlaždice, a prázdnou mřížku, ve které uspořádáte dlaždice.

    ![Snímek obrazovky Galerie dlaždic a prázdná mřížka](./media/azure-portal-dashboards/dashboard-name.png)

1. V popisku řídicího panelu vyberte text **Můj řídicí panel** a zadejte název, který vám pomůže snadno identifikovat vlastní řídicí panel.

1. Chcete-li ukončit režim úprav, vyberte možnost **Hotovo přizpůsobení** v záhlaví stránky.

V zobrazení řídicího panelu se teď zobrazí nový řídicí panel. Výběrem šipky vedle názvu řídicího panelu zobrazte řídicí panely, které máte k dispozici. Seznam může obsahovat řídicí panely, které vytvořili a sdíleli jiní uživatelé.

## <a name="edit-a-dashboard"></a>Úprava řídicího panelu

Teď tento řídicí panel upravíte tak, aby bylo možné přidat, změnit jeho velikost a uspořádat dlaždice, které reprezentují vaše prostředky Azure.

### <a name="add-tiles-from-the-dashboard"></a>Přidat dlaždice z řídicího panelu

Chcete-li přidat dlaždice na řídicí panel, použijte následující postup:

1. ![V záhlaví stránky vyberte Upravit úpravu ikony ](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Upravit** .

    ![Snímek obrazovky s úpravou zvýraznění řídicího panelu](./media/azure-portal-dashboards/dashboard-edit.png)

1. Procházením **Galerie dlaždic** nebo pomocí vyhledávacího pole najděte požadovanou dlaždici.

1. Pokud chcete dlaždici přidat na řídicí panel s výchozí velikostí a umístěním, vyberte **Přidat** . Nebo přetáhněte dlaždici do mřížky a umístěte ji tam, kde chcete.

> [!TIP]
> Pokud pracujete s více než jednou organizací, přidejte na řídicí panel dlaždici **identity organizace** a jasně Zobrazte, do které organizace prostředky patří.

### <a name="add-tiles-from-a-resource-page"></a>Přidat dlaždice ze stránky prostředků

Existuje alternativní způsob, jak přidat dlaždice na řídicí panel. Mnohé stránky prostředků obsahují ikonu připínáčku na panelu příkazů. Vyberete-li ikonu, bude na řídicím panelu, který je aktuálně aktivní, připojena dlaždice představující zdrojovou stránku. 

![Snímek obrazovky s panelem příkazů stránky s ikonou připnutí](./media/azure-portal-dashboards/dashboard-pin-blade.png)

### <a name="resize-or-rearrange-tiles"></a>Změna velikosti nebo změny uspořádání dlaždic

Chcete-li změnit velikost dlaždice nebo změnit uspořádání dlaždic na řídicím panelu, postupujte podle následujících kroků:

1. ![V záhlaví stránky vyberte Upravit úpravu ikony ](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Upravit** .

1. Vyberte kontextovou nabídku v pravém horním rohu dlaždice. Pak zvolte velikost dlaždice. Dlaždice, které podporují jakoukoli velikost, zahrnují také "popisovač" v pravém dolním rohu, který umožňuje přetáhnout dlaždici na požadovanou velikost.

    ![Snímek obrazovky řídicího panelu s otevřenou nabídkou velikost dlaždice](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Vyberte dlaždici a přetáhněte ji na nové místo v mřížce a uspořádejte tak řídicí panel.

### <a name="additional-tile-configuration"></a>Další konfigurace dlaždice

Některé dlaždice můžou vyžadovat další konfiguraci, aby se zobrazily požadované informace. Například dlaždice **grafu metriky** musí být nastavená tak, aby zobrazila metriku z **Azure monitor**. Můžete také přizpůsobit data dlaždice a přepsat tak výchozí nastavení času řídicího panelu.

Všechny dlaždice, které je třeba nastavit, zobrazí banner s **ikonou pro konfiguraci** , dokud dlaždici nepřizpůsobíte. Přizpůsobení dlaždice:

1. Chcete-li ukončit režim úprav, vyberte možnost **Hotovo přizpůsobení** v záhlaví stránky.

1. Vyberte hlavičku a pak proveďte požadovanou instalaci.

    ![Snímek obrazovky dlaždice, která vyžaduje konfiguraci](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Dlaždice Markdownu vám umožní zobrazit na řídicím panelu vlastní a statický obsah. Může se jednat o základní pokyny, obrázek, sadu hypertextových odkazů nebo dokonce kontaktní informace. Další informace o použití dlaždice Markdownu najdete v tématu [použití dlaždice Markdownu na řídicích panelech Azure k zobrazení vlastního obsahu](azure-portal-markdown-tile.md).

### <a name="customize-tile-data"></a>Přizpůsobení dat dlaždic

Data na řídicím panelu automaticky zobrazují aktivitu za posledních 24 hodin. Chcete-li zobrazit pro tuto dlaždici jiný časový rozsah, postupujte podle následujících kroků:

1. V místní nabídce nebo v levém horním rohu dlaždice vyberte **přizpůsobit data dlaždice** nebo ![ Filtr ikony filtru ](./media/azure-portal-dashboards/dashboard-filter.png) .

    ![Obrazovka kontextové nabídky dlaždice](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. Zaškrtnutím políčka **potlačíte nastavení času řídicího panelu na úrovni dlaždice**.

    ![Snímek obrazovky dialogového okna pro konfiguraci nastavení času dlaždice](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Vyberte časový rozsah, který se má zobrazit pro tuto dlaždici. Můžete si vybrat z posledních 30 minut za posledních 30 dní nebo definovat vlastní rozsah.

1. Vyberte časové rozlišení, které se má zobrazit. Můžete se zobrazit kdekoli od 1 minuty po jeden měsíc.

1. Vyberte **Použít**.

## <a name="delete-a-tile"></a>Odstranění dlaždice

Pokud chcete z řídicího panelu odebrat dlaždici, postupujte následovně:

* Vyberte kontextovou nabídku v pravém horním rohu dlaždice a pak vyberte **Odebrat z řídicího panelu**. Nebo:

* Vyberte ![ upravit ikonu ](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Upravit** a zadejte režim přizpůsobení. Najeďte myší v pravém horním rohu dlaždice a pak vyberte ![ ikonu Odstranit ikona odstranit ](./media/azure-portal-dashboards/dashboard-delete-icon.png) pro odebrání dlaždice z řídicího panelu.

   ![Snímek obrazovky ukazující, jak odebrat dlaždici z řídicího panelu](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Klonování řídicího panelu

Pokud chcete použít existující řídicí panel jako šablonu pro nový řídicí panel, postupujte takto:

1. Ujistěte se, že se v zobrazení řídicího panelu zobrazuje řídicí panel, který chcete zkopírovat.

1. V záhlaví stránky vyberte klonovat ![ ikonu klonu ](./media/azure-portal-dashboards/dashboard-clone.png) **Clone**.

1. Kopie řídicího panelu, pojmenovaný **klon** *názvu řídicího panelu* , se otevře v režimu úprav. Pomocí předchozích kroků v tomto článku přejmenujete a přizpůsobíte řídicí panel.

## <a name="publish-and-share-a-dashboard"></a>Publikování a sdílení řídicího panelu

Když vytváříte řídicí panel, je ve výchozím nastavení privátní, což znamená, že jste ten, kdo ho uvidí. Aby byly řídicí panely dostupné ostatním uživatelům, můžete je publikovat a sdílet. Další informace najdete v tématu [sdílení řídicích panelů Azure pomocí Access Control na základě rolí](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Otevření sdíleného řídicího panelu

Pokud chcete najít a otevřít sdílený řídicí panel, postupujte takto:

1. Vyberte šipku vedle názvu řídicího panelu.

1. Vyberte ze zobrazeného seznamu řídicích panelů. Pokud řídicí panel, který chcete otevřít, není uvedený:

    1. Vyberte **Procházet všechny řídicí panely**.

        ![Snímek nabídky výběru řídicího panelu](./media/azure-portal-dashboards/dashboard-browse.png)

    1. V poli **typ** vyberte **sdílené řídicí panely**.

        ![Snímek obrazovky nabídky výběr všech řídicích panelů](./media/azure-portal-dashboards/dashboard-browse-all.png)

    1. Vyberte jedno nebo více předplatných. Můžete také zadat text pro filtrování řídicích panelů podle názvu.

    1. Vyberte řídicí panel ze seznamu sdílených řídicích panelů.

## <a name="delete-a-dashboard"></a>Odstranění řídicího panelu

K trvalému odstranění privátního nebo sdíleného řídicího panelu použijte následující postup:

1. V seznamu vedle názvu řídicího panelu vyberte řídicí panel, který chcete odstranit.

1. ![V záhlaví stránky vyberte odstranit ikonu ](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Odstranit** .

1. V případě privátního řídicího panelu v potvrzovacím dialogovém okně vyberte **OK** , aby se řídicí panel odebral. U sdíleného řídicího panelu kliknutím na zaškrtávací políčko v potvrzovacím dialogovém okně potvrďte, že publikovaný řídicí panel již nebude možné zobrazit jiní uživatelé. Pak vyberte **OK**.

    ![Snímek obrazovky s potvrzením odstranění](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Další kroky

* [Sdílení řídicích panelů Azure prostřednictvím Řízení přístupu na základě role](azure-portal-dashboard-share-access.md)
* [Vytváření řídicích panelů Azure prostřednictvím kódu programu](azure-portal-dashboards-create-programmatically.md)
