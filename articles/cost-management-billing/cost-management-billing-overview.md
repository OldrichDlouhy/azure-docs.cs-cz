---
title: Přehled správy nákladů a fakturace v Azure | Microsoft Docs
description: Pomocí funkcí správy nákladů a fakturace v Azure můžete provádět úlohy správy fakturace a spravovat přístup k fakturám a nákladům. Pomůžou vám také sledovat a řídit útraty za Azure a optimalizovat využití prostředků Azure.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: overview
ms.service: cost-management-billing
ms.custom: ''
ms.openlocfilehash: 2f96208ff3f9664d82bfc1d9ddf9bc5b9aec37c3
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879084"
---
# <a name="what-is-azure-cost-management-and-billing"></a>Co je správa nákladů a fakturace v Azure?

S produkty a službami Azure platíte jenom za to, co využijete. S tím, jak vytváříte a používáte prostředky Azure, se vám za ně účtují poplatky. Pomocí funkcí správy nákladů a fakturace v Azure můžete provádět úlohy správy fakturace a spravovat přístup k fakturám a nákladům. Pomohou vám také sledovat a řídit útraty za Azure a optimalizovat využití prostředků Azure.

## <a name="understand-azure-billing"></a>Principy fakturace v Azure

Funkce fakturace v Azure slouží ke kontrole fakturovaných nákladů a ke správě přístupu k fakturačním údajům. Ve velkých organizacích se o fakturační úlohy obvykle starají zásobovací a finanční týmy.

Když se zaregistrujete do Azure, vytvoří se vám fakturační účet. Ten slouží ke správě faktur a plateb a sledování nákladů. Přístup můžete mít k více fakturačním účtům. Může to být třeba v situaci, kdy se zaregistrujete do Azure, abyste mohli pracovat na svých osobních projektech, takže budete mít individuální předplatné Azure s fakturačním účtem, ale současně máte přístup i prostřednictvím smlouvy Enterprise vaší organizace nebo smlouvy se zákazníkem Microsoftu. Pro každý scénář byste měli samostatný fakturační účet.

### <a name="billing-accounts"></a>Fakturační účty

Web Azure Portal aktuálně podporuje následující typy fakturačních účtů:

- **Program MOSP (Microsoft Online Services Program):** Individuální fakturační účet pro program MOSP (Microsoft Online Services Program) se vytvoří, když se zaregistrujete do Azure prostřednictvím webu Azure. Když si například zaregistrujete [bezplatný účet Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), [účet s průběžnými platbami](https://azure.microsoft.com/offers/ms-azr-0003p/) nebo jste [předplatitelem sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

- **Smlouva Enterprise:** Fakturační účet pro smlouvu Enterprise se vytvoří, když vaše organizace uzavře [smlouvu Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) na používání Azure.

- **Smlouva se zákazníkem Microsoftu:** Fakturační účet pro smlouvu se zákazníkem Microsoftu se vytvoří tehdy, když vaše organizace uzavře smlouvu se zákazníkem Microsoftu prostřednictvím zástupce Microsoftu. Fakturační účet pro smlouvu se zákazníkem Microsoftu můžou mít i někteří zákazníci ve vybraných oblastech, kteří si prostřednictvím webu Azure zaregistrují [účet s průběžnými platbami](https://azure.microsoft.com/offers/ms-azr-0003p/) nebo upgradují svůj [bezplatný účet Azure](https://azure.microsoft.com/offers/ms-azr-0044p/). Další informace najdete v tématu [Začínáme s fakturačními účty pro smlouvu se zákazníkem Microsoftu](./understand/mca-overview.md).

### <a name="scopes-for-billing-accounts"></a>Rozsahy pro fakturační účty
Obor je uzel v rámci fakturačního účtu, pomocí kterého zobrazujete a spravujete fakturaci. Právě tady spravujete fakturační údaje, platby a faktury a provádíte všeobecnou správu účtu.

#### <a name="microsoft-online-services-program"></a>Program MOSP (Microsoft Online Services Program)

|Rozsah  |Definice  |
|---------|---------|
|Fakturační účet     | Představuje jednoho vlastníka (správce účtu) pro jedno nebo více předplatných Azure. Správce účtu má oprávnění provádět různé úkony spojené s fakturací, jako je vytváření předplatných, zobrazení faktur nebo změna fakturace pro předplatná.  |
|Předplatné     |  Představuje seskupení prostředků Azure. Faktura se vygeneruje v oboru předplatného. Má vlastní způsoby platby, pomocí kterých se hradí příslušné faktury.|


#### <a name="enterprise-agreement"></a>Smlouva Enterprise

|Rozsah  |Definice  |
|---------|---------|
|Fakturační účet    | Představuje registraci smlouvy Enterprise. Faktura se vygeneruje v oboru fakturačního účtu. Je strukturovaná na základě oddělení a registračních účtů.  |
|Oddělení     |  Volitelné seskupení registračních účtů.      |
|Registrační účet     |  Představuje vlastníka jednoho účtu. Předplatná Azure se vytvářejí v rámci oboru registračního účtu.  |


#### <a name="microsoft-customer-agreement"></a>Smlouva se zákazníkem Microsoftu

|Rozsah  |Úlohy  |
|---------|---------|
|Fakturační účet     |   Představuje zákaznickou smlouvu na více produktů a služeb Microsoftu. Fakturační účet je strukturovaný pomocí fakturačních profilů a oddílů faktury.   |
|Fakturační profil     |  Představuje fakturu a související způsoby platby. V tomto rozsahu se generují faktury. Fakturační profil může obsahovat více oddílů faktury.      |
|Oddíl faktury     |   Představuje skupinu nákladů na faktuře. K oboru oddílu faktury jsou přidružená předplatná a další nákupy.    |


## <a name="understand-azure-cost-management"></a>Principy správy nákladů v Azure
Správa nákladů je proces, při kterém efektivně plánujete a řídíte náklady své firmy. Úlohy správy nákladů obvykle provádějí finanční týmy, týmy správy účtů a týmy aplikací. Správa nákladů a fakturace v Azure pomáhá organizacím plánovat s ohledem na náklady. Pomáhá také efektivně analyzovat náklady a optimalizovat útratu za cloud. Další informace o tom, jak organizace mohou využít správu nákladů, najdete v článku, který se věnuje [osvědčeným postupům pro Azure Cost Management](./costs/cost-mgt-best-practices.md).

Rychlý přehled o tom, jak vám funkce správy nákladů v Azure můžou pomoct ušetřit v Azure peníze, najdete ve [videu s přehledem správy nákladů v Azure](https://www.youtube.com/watch?v=el4yN5cHsJ0). Další videa najdete v [kanálu služby Cost Management na YouTube](https://www.youtube.com/c/AzureCostManagement).

>[!VIDEO https://www.youtube.com/embed/el4yN5cHsJ0]

Přestože spolu souvisejí, fakturace se od správy nákladů liší. Fakturace je proces, při kterém vystavujete faktury za zboží a služby zákazníkům a spravujete komerční vztahy.

Pomocí pokročilých analýz služba Cost Management zobrazuje schémata nákladů a využití na úrovni organizace. Sestavy ve službě Cost Management ukazují náklady na základě využití pro služby Azure a nabídky třetích stran z Marketplace. Náklady jsou založené na vyjednaných cenách a zohledňují slevy za rezervace a Zvýhodněné hybridní využití Azure. Společně tyto sestavy zobrazují interní a externí náklady na využití a poplatky za Azure Marketplace. Ostatní poplatky, například za nákupy rezervací, podporu a daně, se zatím v sestavách nezobrazují. Tyto sestavy vám pomohou vyznat se v útratách a využití prostředků a zjistit neobvyklé výdaje. K dispozici máte také prediktivní analýzu. Služba Cost Management využívá skupiny pro správu, rozpočty a doporučení Azure, aby přehledně zobrazila, jak máte uspořádány výdaje a jak byste mohli snížit náklady.

K automatizaci exportu můžete využít web Azure Portal nebo různá rozhraní API, abyste mohli integrovat data nákladů s externími systémy a procesy. K dispozici máte také automatizovaný export dat fakturace a naplánované sestavy.

### <a name="plan-and-control-expenses"></a>Plánování a řízení nákladů

Mezi způsoby, jak vám služba Cost Management pomáhá plánovat a řídit náklady, patří: analýza nákladů, rozpočty, doporučení a exportování údajů pro správu nákladů.

Prostřednictvím analýzy nákladů můžete prozkoumat a analyzovat výdaje organizace. Můžete se podívat na agregované náklady na úrovni organizace, abyste porozuměli tomu, kde se náklady generují, a mohli identifikovat trendy útrat. Můžete si také zobrazit souhrnné náklady v průběhu času, abyste mohli odhadnout měsíční, čtvrtletní a dokonce i roční trendy nákladů oproti rozpočtu.

Rozpočty pomáhají plánovat a plnit finanční odpovědnost v organizaci. Pomáhají vám, abyste nepřekročili mezní hodnoty nebo limity nákladů. Prostřednictvím rozpočtů také můžete informovat ostatní o jejich útratách a proaktivně tak spravovat náklady. Díky nim se také můžete podívat na to, jak se útraty během času vyvíjejí.

Doporučení znázorňují, jak můžete optimalizovat a zvýšit efektivitu pomocí identifikace nečinných a nedostatečně využitých prostředků. Mohou také zobrazit levnější možnosti prostředků. Pokud se budete řídit doporučeními, můžete změnit způsob, jakým využíváte prostředky, a ušetřit tak peníze. Pokud chcete postupovat podle doporučení, nejprve si zobrazte doporučení k optimalizaci nákladů a podívejte se na možné neefektivní využití prostředků. Dále změňte využití prostředku Azure na nákladově efektivnější možnost. Potom ověřte akci, abyste měli jistotu, že provedená změna je úspěšná.

Pokud pro přístup k datům správy nákladů nebo jejich kontrole používáte externí systémy, můžete z Azure data snadno exportovat. Můžete také naplánovat denní export ve formátu CSV a soubory dat ukládat v úložišti Azure. Potom budete mít přístup k datům v externím systému.

### <a name="cloudyn-deprecation"></a>Vyřazení služby Cloudyn z provozu

[Cloudyn](./cloudyn/overview.md) je služba Azure související s Cost Managementem, do konce roku 2020 vyřazuje z provozu. Stávající funkce Cloudynu se tam, kde je to možné, integrují přímo do webu Azure Portal. V tuto chvíli se neonboardují žádní noví zákazníci, ale podpora pro tento produkt zůstane zachovaná, dokud se úplně nevyřadí z provozu.
 
Podívejte se na [video o službách Azure Cost Management a Cloudyn](https://www.youtube.com/watch?v=15DzKPMBRxM), ve kterém se dozvíte, kdy byste v závislosti na potřebách vaší firmy měli používat Azure Cost Management nebo Cloudyn. Další videa najdete v [kanálu služby Cost Management na YouTube](https://www.youtube.com/c/AzureCostManagement).
 
>[!VIDEO https://www.youtube.com/embed/15DzKPMBRxM]

### <a name="additional-azure-tools"></a>Další nástroje Azure

Azure má další nástroje, které nejsou součástí sady funkcí pro správu nákladů a fakturaci v Azure. Hrají ale důležitou roli v procesu správy nákladů. Další informace o těchto nástrojích si můžete přečíst po kliknutí na následující odkazy.

- [Cenová kalkulačka Azure](https://azure.microsoft.com/pricing/calculator/) – tento nástroj slouží k odhadování počátečních nákladů na cloud.
- [Azure Migrate](../migrate/migrate-overview.md) – umožňuje zhodnotit aktuální úlohy datacentra a zjistit, co je třeba využít z náhradního řešení Azure.
- [Azure Advisor](../advisor/advisor-overview.md) – umožňuje identifikovat nevyužité virtuální počítače a získat doporučení k nákupům rezervovaných instancí Azure.
- [Zvýhodněné hybridní využití Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) – umožňuje využít aktuální místní licence k Windows Serveru nebo SQL Serveru pro virtuální počítače v Azure a ušetřit tak peníze.


## <a name="next-steps"></a>Další kroky

Seznámili jste se s funkcemi pro správu nákladů a fakturaci, takže dalším krokem je začít využívat službu Cost Management.

- Začněte s použitím služby Cost Management k [analýze nákladů](./costs/quick-acm-cost-analysis.md).
- Můžete si také přečíst další informace o [osvědčených postupech pro Azure Cost Management](./costs/cost-mgt-best-practices.md).
