---
title: Vytvoření kontroly přístupu skupin & aplikací – Azure AD
description: Naučte se, jak vytvořit kontrolu přístupu pro členy skupiny nebo přístup k aplikacím v Azure Active Directory kontroly přístupu.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/29/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cf82080a54075353d2ff1d98a2ea19fc6a96dc7
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86204008"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Vytvoření kontroly přístupu skupin a aplikací v prohlídekch Azure AD Access

Přístup k skupinám a aplikacím pro zaměstnance a hosty se v průběhu času mění. Aby se snížilo riziko spojené s nezastaralým přiřazením přístupu, můžou správci pomocí služby Azure Active Directory (Azure AD) vytvářet kontroly přístupu pro členy skupiny nebo přístup k aplikacím. Pokud potřebujete rutinní kontrolu přístupu, můžete také vytvořit opakované recenze přístupu. Další informace o těchto scénářích najdete v tématech [Správa přístupu uživatelů](manage-user-access-with-access-reviews.md) a [Správa přístupu hostů](manage-guest-access-with-access-reviews.md).

Tento článek popisuje, jak vytvořit jednu nebo více kontrol přístupu pro členy skupiny nebo přístup k aplikaci.

## <a name="prerequisites"></a>Požadavky

- Azure AD Premium P2
- Globální správce nebo Správce uživatelů

Další informace najdete v tématu [licenční požadavky](access-reviews-overview.md#license-requirements).

## <a name="create-one-or-more-access-reviews"></a>Vytvoření jedné nebo více kontrol přístupu

1. Přihlaste se k Azure Portal a otevřete [stránku zásad správného řízení identity](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. V nabídce vlevo klikněte na **recenze přístupů**.

1. Kliknutím na **Nová kontrola přístupu** vytvořte novou kontrolu přístupu.

    ![Podokno kontroly přístupu v nástroji pro řízení identit](./media/create-access-review/access-reviews.png)

1. Pojmenujte kontrolu přístupu. Volitelně můžete zadat popis revize. Název a popis se zobrazí kontrolorům.

    ![Vytvoření kontroly přístupu – kontrola názvu a popisu](./media/create-access-review/name-description.png)

1. Nastavte **počáteční datum**. Ve výchozím nastavení proběhne kontrola přístupu jednou, začíná ve stejnou dobu, kdy je vytvořena, a končí v jednom měsíci. Počáteční a koncové datum můžete změnit tak, aby byla kontrola přístupu zahájena v budoucnosti a poslední, ale kolik dní potřebujete.

    ![Vytvoření kontroly přístupu – počáteční a koncové datum](./media/create-access-review/start-end-dates.png)

1. Chcete-li provést opakované kontroly přístupu, změňte nastavení **frekvence** od **jednoho** na **týdně**, **měsíčně**, **čtvrtletně**, **částečně ročně**nebo **ročně**. Pomocí posuvníku **Trvání** nebo textového pole můžete definovat, kolik dní bude každá revize opakujících se řad otevřená pro vstup od revidujících. Například maximální doba, kterou můžete nastavit pro měsíční revizi, je 27 dní, aby se předešlo překrývání recenzí.

1. Pomocí nastavení **konec** určete, jak se má ukončit řada kontroly opakovaného přístupu. Série může končit třemi způsoby: průběžně spouští recenze na neomezenou dobu, až do konkrétního data nebo po dokončení definovaného počtu výskytů. Vy, jiný správce nebo jiný globální správce může série po vytvoření zastavit změnou data v **Nastavení**, takže skončí k tomuto datu.

1. V části **Uživatelé** Určete uživatele, pro které platí tato kontrola přístupu. Kontroly přístupu mohou být pro členy skupiny nebo pro uživatele, kteří byli přiřazeni k aplikaci. Můžete dále určit rozsah kontroly přístupu a zkontrolovat pouze uživatele typu Host, kteří jsou členy (nebo jsou přiřazeni k aplikaci), a ne kontrolovat všechny uživatele, kteří jsou členy nebo kteří mají k aplikaci přístup.

    ![Vytvoření kontroly přístupu – uživatelé](./media/create-access-review/users.png)

1. V části **Skupina** vyberte jednu nebo více skupin, u kterých chcete zkontrolovat členství.

    > [!NOTE]
    > Výběr více než jedné skupiny vytvoří více kontrol přístupu. Když například vyberete pět skupin, vytvoří se pět samostatných kontrol přístupu.
    
    ![Vytvoření kontroly přístupu – výběr skupiny](./media/create-access-review/select-group.png)

1. V části **aplikace** (Pokud jste v kroku 8 vybrali možnost **přiřazeno k aplikaci** ) vyberte aplikace, pro které chcete zkontrolovat přístup.

    > [!NOTE]
    > Když vyberete víc než jednu aplikaci, vytvoří se víc kontrol přístupu. Když například vyberete pět aplikací, vytvoří se pět samostatných revizí přístupu.
    
    ![Vytvoření kontroly přístupu – výběr aplikace](./media/create-access-review/select-application.png)

1. V části **revidující** vyberte jednu nebo více lidí pro kontrolu všech uživatelů v oboru. Nebo můžete vybrat, aby členové zkontrolovali svůj vlastní přístup. Pokud je prostředek skupinou, můžete požádat vlastníka skupiny, aby si je zkontrolovali. Také můžete vyžadovat, aby kontroloři při schvalování přístupu zadali důvod.

    ![Vytvoření kontroly přístupu – kontroloři](./media/create-access-review/reviewers.png)

1. V části **programy** vyberte program, který chcete použít. **Výchozí program** je vždy přítomen.

    ![Vytvoření kontroly přístupu – programy](./media/create-access-review/programs.png)

    Můžete zjednodušit sledování a shromažďování kontrol přístupu pro různé účely jejich uspořádáním do programů. Každou kontrolu přístupu můžete propojit s programem. Po přípravě sestav pro auditora se můžete soustředit na kontroly přístupu v oboru pro konkrétní iniciativu. Programy a výsledky kontroly přístupu jsou viditelné uživatelům v roli globální správce, správce uživatele, správce zabezpečení nebo čtenáře zabezpečení.

    Chcete-li zobrazit seznam programů, přejděte na stránku kontroly přístupu a vyberte **programy**. Pokud se nacházíte v roli globálního správce nebo Správce uživatelů, můžete vytvořit další programy. Můžete například zvolit, aby měl jeden program pro každou iniciativu dodržování předpisů nebo obchodní cíl. Pokud už program nepotřebujete a k němu nejsou připojené žádné ovládací prvky, můžete ho odstranit.

### <a name="upon-completion-settings"></a>Nastavení po dokončení

1. Chcete-li určit, co se stane po dokončení kontroly, rozbalte část **nastavení po dokončení** .

    ![Vytvoření kontroly přístupu s nastavením při dokončování](./media/create-access-review/upon-completion-settings.png)

1. Pokud chcete automaticky odebrat přístup pro uživatele, kterým bylo odepřeno, nastavte **automatické použití výsledků na prostředek** , aby **bylo možné povolit**. Pokud chcete výsledky použít ručně po dokončení kontroly, nastavte přepínač na **Zakázat**.

1. Seznam **by měl kontrolor bez odpovědi** použít k určení toho, co se stane pro uživatele, kteří kontrolor v rámci období revize nekontroloval. Toto nastavení nemá vliv na uživatele, kteří byli zkontrolováni ručně. Pokud je posledním rozhodnutím kontrolora zamítnutí, bude přístup uživatele odebrán.

    - **Žádná změna** – opuštění přístupu uživatele nezměněné
    - **Odebrání přístupu** – odebrání přístupu uživatele
    - **Schválit přístup** – schválení přístupu uživatele
    - **Využijte doporučení** – Vezměte v úvahu doporučení systému při odepření nebo schvalování trvalého přístupu uživatele.

### <a name="advanced-settings"></a>Upřesnit nastavení

1. Chcete-li zadat další nastavení, rozbalte oddíl **Upřesnit nastavení** .

    ![Vytvoření kontroly přístupu – Pokročilá nastavení](./media/create-access-review/advanced-settings-preview.png)

1. Nastavením **Zobrazit doporučení** **umožníte, aby se** recenzenti zobrazovala doporučení k systému na základě informací o přístupu uživatele.

1. Nastavte **vyžadovat důvod schválení** , aby mohl uživatel vyžadovat **, aby kontrolor** zadal důvod schválení.

1. Nastavte e- **mailová oznámení** , která **umožní** , aby služba Azure AD odesílala e-mailová oznámení kontrolorům při zahájení kontroly přístupu a správcům, když se kontrola dokončí.

1. Nastavením **připomenutí** **umožníte** , aby služba Azure AD odesílala připomenutí kontrol přístupu, která nedokončila jejich kontrolu. 

    >[!NOTE]
    > Ve výchozím nastavení Azure AD automaticky pošle připomenutí kontrolorům, kteří ještě neodpověděli, do koncového data.

1. Tisk Obsah e-mailu odeslaného revidujícím se automaticky generuje na základě podrobností o kontrole, jako je například název revize, název prostředku, datum splatnosti atd. Pokud potřebujete způsob, jak sdělit další informace, jako jsou například další pokyny nebo kontaktní údaje, můžete tyto podrobnosti zadat v dalším obsahu pro e-mail kontrolora, který bude zahrnut v e-mailech pozvánky a připomenutí odeslaných přiřazeným kontrolorům. Zvýrazněná část je místo, kde se tyto informace zobrazí.

    ![Kontrola přístupu uživatelů ke skupině](./media/create-access-review/review-users-access-group.png)

## <a name="start-the-access-review"></a>Spustit kontrolu přístupu

Po zadání nastavení pro kontrolu přístupu klikněte na **Spustit**. Kontrola přístupu se zobrazí v seznamu s indikátorem jeho stavu.

![Seznam revizí přístupu a jejich stav](./media/create-access-review/access-reviews-list.png)

Ve výchozím nastavení Azure AD pošle e-mail kontrolorům krátce po zahájení kontroly. Pokud se rozhodnete Neodesílat e-maily Azure AD, nezapomeňte informovat kontrolory, které čekají na dokončení kontroly přístupu. Můžete jim Ukázat pokyny, jak [zkontrolovat přístup ke skupinám nebo aplikacím](perform-access-review.md). Pokud je vaše kontrola pro hosty, aby zkontrolovala svůj vlastní přístup, zobrazte si pokyny, jak [kontrolovat přístup pro vlastní skupiny nebo aplikace](review-your-access.md).

Pokud jste přidělili hosty jako kontroloři a nepřijali pozvánku, neobdrží e-mail z kontroly přístupu, protože před přezkoumáním musí nejdřív pozvání přijmout.

## <a name="access-review-status-table"></a>Tabulka stavů kontroly přístupu

| Status | Definice |
|--------|------------|
|NotStarted | Byla vytvořena recenze, zjišťování uživatelů čeká na spuštění. |
|Initializing   | Zjišťování uživatelů probíhá při identifikaci všech uživatelů, kteří jsou součástí kontroly. |
|Spouštění | Probíhá spuštění kontroly. Pokud jsou povolená e-mailová oznámení, posílá recenzentům e-maily. |
|InProgress | Kontrola byla spuštěna. Pokud se e-mailová oznámení povolí recenzentům, pošlou jim e-mail. Recenzenti mohou odesílat rozhodnutí do data splatnosti. |
|Absolv | Kontrola je dokončena a e-maily jsou odesílány vlastníkovi revize. |
|Autorevize | Revize je ve fázi kontroly systému. Systém zaznamenává rozhodnutí pro uživatele, kteří nebyli prověřeni na základě doporučení nebo předem nakonfigurovaných rozhodnutí. |
|Přezkoumat | Systém zaznamenal rozhodnutí pro všechny uživatele, kteří nebyli zkontrolováni. Kontrola je připravena pokračovat v **použití** , pokud je povoleno automatické použití. |
|Použije | Pro uživatele, kteří se schválili, nebude přístup nijak změněn. |
|Použito | Zamítnutí uživatelé (pokud existují) byly odebrány z prostředku nebo adresáře. |

## <a name="create-reviews-via-apis"></a>Vytváření recenzí prostřednictvím rozhraní API

Můžete také vytvořit kontroly přístupu pomocí rozhraní API. K tomu, jak spravovat kontroly přístupu skupin a uživatelů aplikací v Azure Portal lze také použít rozhraní API Microsoft Graph. Další informace najdete v referenčních informacích k [rozhraní API kontroly přístupu Azure AD](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Ukázku kódu najdete v tématu [příklad načtení kontrol přístupu služby Azure AD prostřednictvím Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Další kroky

- [Kontrola přístupu ke skupinám nebo aplikacím](perform-access-review.md)
- [Kontrola přístupu pro vlastní skupiny nebo aplikace](review-your-access.md)
- [Dokončení kontroly přístupu skupin nebo aplikací](complete-access-review.md)
