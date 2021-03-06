---
title: Převod vlastnictví fakturace předplatného Azure
description: Popisuje, jak převést vlastnictví fakturace předplatného Azure na jiný účet, a obsahuje některé nejčastější dotazy týkající se tohoto procesu.
keywords: převod předplatného azure, azure převod předplatného, přesun předplatného azure do jiného účtu, azure změna vlastníka předplatného, převod předplatného azure na jiný účet, azure převod fakturace
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 07/01/2020
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 722d1bca7f983c124c85e6d675f51d29c5357522
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854937"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Převod vlastnictví fakturace předplatného Azure na jiný účet

Vlastnictví fakturace vašeho předplatného Azure můžete chtít převést, když opouštíte organizaci, nebo pokud chcete, aby se vaše předplatné účtovalo k jinému účtu. Převod vlastnictví fakturace na jiný účet poskytuje správcům v novém účtu oprávnění pro fakturační úkoly. Mohou změnit způsob platby, zobrazit si poplatky a předplatné zrušit.

Pokud chcete vlastnictví fakturace zachovat, ale chcete změnit typ vašeho předplatného, přečtěte si téma [Přepnutí předplatného Azure na jinou nabídku](switch-azure-offer.md). Pokud chcete řídit, kdo má přístup k prostředkům v předplatném, přečtěte si téma věnované [předdefinovaným rolím v Azure](../../role-based-access-control/built-in-roles.md).

Pokud jste zákazníkem se smlouvou Enterprise (EA), můžou správci vaší organizace převádět vlastnictví fakturace vašich předplatných mezi účty. Další informace najdete v tématu [Převod vlastnictví fakturace předplatných se smlouvou Enterprise (EA)](#EA).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Převod vlastnictví fakturace předplatného Azure

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) jako správce fakturačního účtu obsahujícího předplatné, které chcete převést. Pokud chcete zjistit, jestli jste správce, přečtěte si [Nejčastější dotazy.](#faq)

1. Vyhledejte **Cost Management a fakturace**.

   ![Snímek obrazovky znázorňující hledání na webu Azure Portal](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. V levém podokně vyberte **Předplatná**. V závislosti na vašem přístupu možná budete muset vybrat rozsah fakturace a pak **Předplatná** nebo **Předplatná Azure**.

1. U předplatného, které chcete převést, vyberte **Převést vlastnictví fakturace**.

   ![Výběr předplatného pro převod](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Zadejte e-mailovou adresu uživatele, který je správcem fakturace účtu, který bude novým vlastníkem předplatného.

1. Pokud předplatné převádíte na účet v jiném tenantovi Azure AD, vyberte, jestli chcete předplatné do tenanta nového účtu přesunout. Další informace najdete v tématu [Převod předplatného na účet v jiném tenantovi služby Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account).

    > [!IMPORTANT]
    >
    > Pokud se rozhodnete přesunout předplatné do tenanta Azure AD nového účtu, všechna [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md) pro přístup k prostředkům v předplatném se trvale odstraní. Přístup ke správě prostředků v předplatném bude mít pouze uživatel v novém účtu, který přijme žádost o převod. Další informace najdete další části v tématu [Převod předplatného na jiný účet tenanta Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account). Případně můžete zrušit zaškrtnutí políčka u **tenanta Azure AD pro předplatné**, abyste vlastnictví fakturace převedli bez přesunu předplatného do tenanta nového účtu. Pokud to uděláte, zachovají se stávající přiřazení rolí Azure pro přístup k prostředkům Azure.

    ![Stránka odeslání žádosti o převod](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. Vyberte možnost **Poslat žádost o převod**.

1. Tento uživatel obdrží e-mail s pokyny ke kontrole vaší žádosti o převod.

   ![E-mail pro převod předplatného odeslaný příjemci](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Uživatel může žádost o převod schválit tím, že vybere odkaz v e-mailu a bude postupovat podle zobrazených pokynů. Uživatel potom zvolí způsob platby, který se bude pro platbu za předplatné používat. Pokud uživatel nemá účet Azure, musí si zaregistrovat nový účet.

   ![První webová stránka převodu předplatného](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![Druhá webová stránka převodu předplatného](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![Druhá webová stránka převodu předplatného](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Výborně! Předplatné je teď převedené.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Převod předplatného na jiný účet tenanta Azure AD

Tenant Azure Active Directory (AD) se vám vytvoří při registraci do Azure. Tenant představuje váš účet. Tenant se používá ke správě přístupu k předplatným a prostředkům.

Když vytvoříte nové předplatné, hostuje se v tenantovi Azure AD vašeho účtu. Pokud chcete přístup k vašemu předplatnému nebo jeho prostředkům poskytnout jiným uživatelům, musíte je pozvat, aby se připojili k vašemu tenantovi. Takto můžete řídit přístup k předplatným a prostředkům.

Když převádíte vlastnictví fakturace vašeho předplatného na účet v jiném tenantovi Azure AD, můžete předplatné do tenanta nového účtu přesunout. Pokud to uděláte, všichni uživatelé, skupiny nebo instanční objekty, které měly [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md) pro správu předplatných a jejich prostředků, tento přístup ztratí. Přístup ke správě prostředků bude mít pouze uživatel v novém účtu, který přijme žádost o převod. Pokud nový vlastník chce poskytnout přístup uživatelům, kteří ho ztratili, musí je do předplatného ručně přidat. Další informace najdete v tématu věnovaném [převodu předplatných Azure do jiného adresáře Azure AD (Preview)](../../role-based-access-control/transfer-subscription.md).


## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Převod předplatných sady Visual Studio a programu Partner Network

Předplatná sady Visual Studio a programu Microsoft Partner Network mají přidružený měsíční opakující se kredit Azure. Při převodu těchto předplatných není kredit v cílovém fakturačním účtu dostupný. Předplatné používá kredit v cílovém fakturačním účtu. Robert například 9. září převede předplatné sady Visual Studio Enterprise do Janina účtu a Jana tento převod přijme. Jakmile se převod dokončí, začne předplatné používat kredit v Janině účtu. Kredit se resetuje vždy devátého v měsíci.


<a id="EA"></a>

## <a name="transfer-ea-subscription-billing-ownership"></a>Přenést vlastnictví fakturace předplatných EA

Podnikový správce může převádět vlastnictví předplatných mezi účty v rámci registrace. Další informace najdete v tématu o [změně vlastníka účtu](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-get-started#change-account-owner) na portálu EA.

## <a name="next-steps-after-accepting-billing-ownership"></a>Další postup po přijetí vlastnictví fakturace

Pokud jste přijali vlastnictví fakturace předplatného Azure, doporučujeme vám projít si tyto další kroky:

1. Zkontrolujte a aktualizujte správce služby, spolusprávce a další přiřazení rolí Azure. Pokud se chcete dozvědět víc, přečtěte si téma [Přidání nebo změna správců předplatného Azure](add-change-subscription-administrator.md) a téma věnované [přidání nebo odebrání přiřazení rolí Azure s využitím webu Azure Portal](../../role-based-access-control/role-assignments-portal.md).
1. Aktualizujte přihlašovací údaje přidružené službám tohoto předplatného, mezi které patří:
   1. Certifikáty pro správu, které udělují uživateli oprávnění správce prostředků předplatného. Další informace najdete v tématu o [vytvoření a nahrání certifikátu pro správu Azure](../../cloud-services/cloud-services-certs-create.md).
   1. Přístupové klíče pro služby, jako je Storage. Další informace najdete v článku o [účtech Azure Storage](../../storage/common/storage-create-storage-account.md).
   1. Oprávnění pro vzdálený přístup ke službám, jako je Azure Virtual Machines.
1. Pokud pracujete s partnerem, zvažte u tohoto předplatného aktualizaci ID partnera. ID partnera můžete aktualizovat na webu [Azure Portal](https://portal.azure.com). Další informace najdete v článku [Propojení ID partnera s účty Azure](link-partner-id.md).

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Podporované typy předplatného

Převod předplatného na webu Azure Portal je k dispozici pro níže uvedené typy předplatného. V současné době se převod nepodporuje pro [bezplatné zkušební verze](https://azure.microsoft.com/offers/ms-azr-0044p/) nebo předplatná [systému Azure v rámci licenčního programu Open (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/). Alternativní řešení najdete v článku [Přesunutí prostředků do nové skupiny prostředků nebo předplatného](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Pokud chcete převést další předplatná, jako jsou plány podpory, [obraťte se na podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Smlouva Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Předplatitelé sady Visual Studio Enterprise (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)
- [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Průběžné platby dle aktuálního využití pro vývoj/testování](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Plán Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\* [Prostřednictvím portálu EA](#EA).

\*\* Podporuje se jen u účtů vytvořených během registrace na webu Azure.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Nejčastější dotazy pro odesílatele

Tyto nejčastější dotazy se týkají uživatelů, kteří převádějí vlastnictví fakturace předplatného Azure na jiný účet.

### <a name="who-is-a-billing-administrator-of-an-account"></a><a name="whoisaa"></a> Kdo je správce fakturace účtu?

Správce fakturace je osoba, která má oprávnění pro správu fakturace účtu. Má oprávnění přistupovat k fakturaci na webu [Azure Portal](https://portal.azure.com) a provádět různé fakturační úkoly, jako je vytváření předplatných, zobrazení a platby faktur nebo aktualizace způsobů platby.

K identifikaci účtů, u kterých jste správcem fakturace, použijte následující postup:

1. Navštivte stránku [Správa nákladů a fakturace na webu Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. V levém podokně vyberte **Všechny obory fakturace**.
1. Stránka s předplatnými obsahuje seznam všech předplatných, pro která jste správcem fakturace.

Pokud si nejste jisti, kdo je správcem účtu předplatného, zjistíte to následujícím postupem.

1. Přejděte na [stránku Předplatná na webu Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Vyberte předplatné, které chcete zkontrolovat, a pak se podívejte pod **Nastavení**.
1. Vyberte **Vlastnosti**. Správce účtu předplatného se zobrazí v poli **Správce účtu**.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Převádí se všechno? Včetně skupin prostředků, virtuálních počítačů, disků a dalších spuštěných služeb?

Všechny prostředky, jako jsou virtuální počítače, disky a weby, se převedou na nový účet. Pokud ale převádíte předplatné do účtu v jiném tenantovi Azure AD, žádné [role správce](add-change-subscription-administrator.md) ani [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md) předplatného se [nepřevedou](#transfer-a-subscription-to-another-azure-ad-tenant-account). Dále se společně s předplatným nepřevádí [registrace aplikací](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) a jiné služby specifické pro tenanta.

### <a name="can-i-transfer-ownership-to-an-account-in-another-countryregion"></a>Můžu převést vlastnictví na účet v jiné zemi nebo oblasti?
Převody mezi zeměmi nebo oblastmi bohužel na webu Azure Portal nejdou provádět. Pokud chcete předplatné převést mezi zeměmi nebo oblastmi, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>Jsem správce dvou účtů. Můžu převést předplatné z jednoho svého účtu do jiného?
Ano, předplatné můžete převádět mezi vašimi účty. Vaše účty jsou koncepčně považované za účty dvou různých uživatelů, proto můžete použít výše uvedené kroky a převádět předplatná mezi těmito účty.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Způsobí převod předplatného nějaké výpadky služeb?

Pokud převádíte předplatné na účet ve stejném tenantovi Azure AD, nemá to žádný vliv na prostředky spuštěné v tomto předplatném. Informace o kontextu uložené v PowerShellu se ale neaktualizují, takže je možná budete muset vymazat nebo změnit nastavení. Pokud převádíte předplatné na účet v jiném tenantovi a rozhodnete se předplatné do tohoto tenanta přesunout, všichni uživatelé, skupiny a instanční objekty, které měly [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md) pro přístup k prostředkům v předplatném, tento přístup ztratí. Následkem toho může dojít k výpadku služeb.

### <a name="can-users-in-new-account-access-usage-and-billing-history"></a>Mají uživatelé v novém účtu přístup k historii využití a fakturace?

Jediné informace, které budou mít uživatelé v novém účtu k dispozici, budou informace o nákladech na předplatné za poslední měsíc. Zbytek historie využití a fakturace se společně s předplatným nepřevede.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Jak můžu migrovat data a služby mého předplatného Azure do nového předplatného?

Pokud nemůžete převést vlastnictví předplatného, můžete prostředky migrovat ručně. Přečtěte si článek [Přesunutí prostředků do nové skupiny prostředků nebo předplatného](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Když převádím předplatné sady Visual Studio nebo programu Microsoft Partner Network, převede se můj kredit do nového účtu společně s předplatným?

Ne, váš kredit nebude v novém účtu k dispozici. Uživatel, který přijímá žádost o převod, musí mít licenci sady Visual Studio, aby žádost o převod mohl přijmout. Předplatné používá kredit sady Visual Studio, který je k dispozici v účtu uživatele. Další informace najdete v tématu věnovaném [převodu předplatných sady Visual Studio a programu Partner Network](#transfer-visual-studio-and-partner-network-subscriptions).


## <a name="frequently-asked-questions-faq-for-recipients"></a>Nejčastější dotazy pro příjemce

Tyto nejčastější dotazy se týkají uživatelů, kteří přijímají vlastnictví fakturace předplatného Azure z jiného účtu.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Pokud převezmu vlastnictví fakturace předplatného z jiného účtu, budou mít uživatelé tohoto účtu i nadále přístup k mým prostředkům?

Ano. Může ale dojít k odebrání [rolí správce](add-change-subscription-administrator.md) a [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md). Ke ztrátě přístupu dojde, když je váš účet v jiném tenantovi Azure AD, než je tenant předplatného, a uživatel, který odeslal požadavek na převod, přesune předplatné do tenanta vašeho účtu. Pokud chcete zobrazit uživatele, kteří mají přiřazení rolí Azure pro přístup k prostředkům v předplatném, použijte následující postup:

1. Přejděte na [stránku Předplatná na webu Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Vyberte předplatné, které chcete zkontrolovat, a pak v levém podokně vyberte **Řízení přístupu (IAM)** .
1. V horní části stránky vyberte **Přiřazení rolí**. Na stránce přiřazení rolí se zobrazí seznam všech uživatelů, kteří mají k předplatnému přístup.

I když během převodu dojde k odebrání [přiřazení rolí Azure](../../role-based-access-control/role-assignments-portal.md), uživatelé v účtu původního vlastníka mohou mít k předplatnému stále přístup prostřednictvím jiných mechanismů zabezpečení, mezi které patří:

* Certifikáty pro správu, které udělují uživateli oprávnění správce prostředků předplatného. Další informace najdete v tématu o [vytvoření a nahrání certifikátu pro správu Azure](../../cloud-services/cloud-services-certs-create.md).
* Přístupové klíče pro služby, jako je Storage. Další informace najdete v článku o [účtech Azure Storage](../../storage/common/storage-create-storage-account.md).
* Oprávnění pro vzdálený přístup ke službám, jako je Azure Virtual Machines.

Pokud příjemce potřebuje omezit přístup k prostředkům, měl by zvážit aktualizaci tajných kódů přidružených ke službě. Většinu prostředků můžete aktualizovat takto:

  1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
  2. V nabídce centra vyberte **Všechny prostředky**.
  3. Vyberte prostředek.
  4. Na stránce prostředku vyberte **Nastavení**. Tady můžete zobrazit a aktualizovat existující tajné kódy.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Pokud převezmu vlastnictví fakturace předplatného uprostřed fakturačního cyklu, musím platit za celý fakturační cyklus?

Váš účet zodpovídá za platbu za jakékoliv využití, které je hlášené od okamžiku převodu. Může dojít k využití, které se uskutečnilo před převodem, ale bylo nahlášeno až poté. Takové využití je součástí faktury vašeho účtu.

### <a name="can-i-use-a-different-payment-method"></a>Můžu použít jiný způsob platby?

Ano. Při přijímání žádosti o převod můžete vybrat stávající způsob platby, který je propojený s vaším účtem, nebo přidat nový způsob platby.

### <a name="how-can-i-transfer-ownership-of-my-enterprise-agreement-ea-subscription-account-ownership-if-the-original-account-owner-is-no-longer-with-the-organization"></a>Jak můžu převést vlastnictví účtu předplatného se smlouvou Enterprise (EA) v případě, že původní vlastník účtu v organizaci již nepracuje?

Podnikový správce může aktualizovat vlastnictví jakéhokoli účtu i po odchodu původního vlastníka účtu z organizace. Může to provést podle pokynů pro [Převod vlastnictví účtu pro všechna předplatná](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) na portálu EA.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="why-dont-i-see-the-transfer-subscription-button"></a><a id="no-button"></a> Proč se mi nezobrazuje tlačítko pro převod předplatného?

Pro váš fakturační účet není k dispozici samoobslužný převod předplatného. V současné době nepodporujeme převod vlastnictví fakturace předplatných v účtech smlouvy Enterprise (EA) na webu Azure Portal. Účty smluv se zákazníkem Microsoftu vytvořené během práce se zástupcem společnosti Microsoft také nepodporují převod vlastnictví fakturace.

### <a name="why-doesnt-my-subscription-type-support-transfer"></a><a id="no-button"></a> Proč můj typ předplatného nepodporuje převod?

Převod vlastnictví fakturace nepodporují všechny typy předplatného. Seznam typů předplatného, které podporují převody, najdete v tématu [Podporované typy předplatného](#supported-subscription-types).

### <a name="why-am-i-receiving-an-access-denied-error-when-i-try-to-transfer-billing-ownership-of-a-subscription"></a><a id="no-button"></a> Proč se mi při pokusu o převod vlastnictví předplatného zobrazuje chyba odepření přístupu?

Tato chyba se zobrazí, pokud se pokoušíte převést předplatné plánu Microsoft Azure a nemáte potřebná oprávnění. Pokud chcete převést předplatné plánu Microsoft Azure, musíte být vlastníkem nebo přispěvatelem v části faktury, ke které se předplatné účtuje. Další informace najdete v tématu [Správa předplatných pro části faktury](understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Potřebujete pomoc? Kontaktujte nás.

Pokud máte dotazy nebo potřebujete pomoc, [vytvořte žádost o podporu](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Další kroky

- Zkontrolujte a aktualizujte správce služby, spolusprávce a další přiřazení rolí Azure. Pokud se chcete dozvědět víc, přečtěte si téma [Přidání nebo změna správců předplatného Azure](add-change-subscription-administrator.md) a téma věnované [přidání nebo odebrání přiřazení rolí Azure s využitím webu Azure Portal](../../role-based-access-control/role-assignments-portal.md).
