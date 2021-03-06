---
title: Referenční příručka Azure Active Directory operací zásad správného řízení
description: Referenční příručka operací popisuje kontroly a akce, které byste měli provést při zabezpečení správy zásad správného řízení.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: 4826bcdc85e0c6189c51aa262014fe154bb479b1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "74535454"
---
# <a name="azure-active-directory-governance-operations-reference-guide"></a>Referenční příručka Azure Active Directory operací zásad správného řízení

Tato část [Referenční příručka k operacím Azure AD](active-directory-ops-guide-intro.md) popisuje kontroly a akce, které byste měli provést k vyhodnocení a ověření přístupu k neprivilegovaným a privilegovaným identitám, auditům a ovládacím prvkům pro změny v prostředí.

> [!NOTE]
> Tato doporučení jsou aktuální k datu publikování, ale mohou se v průběhu času měnit. Organizace by měly průběžně vyhodnocovat své postupy zásad správného řízení, jak se v průběhu času vyvíjí produkty a služby Microsoftu.

## <a name="key-operational-processes"></a>Klíčové provozní procesy

### <a name="assign-owners-to-key-tasks"></a>Přiřazení vlastníků ke klíčovým úlohám

Správa Azure Active Directory vyžaduje průběžné provádění klíčových provozních úloh a procesů, které nemusí být součástí projektu zavedení. Stále je důležité, abyste nastavili tyto úlohy pro optimalizaci prostředí. Mezi klíčové úlohy a jejich Doporučené vlastníky patří:

| Úkol | Vlastník |
| :- | :- |
| Archivace protokolů auditu Azure AD v systému SIEM | Provozní tým InfoSec |
| Zjišťování aplikací, které jsou spravované mimo dodržování předpisů | Provozní tým IAM |
| Pravidelně kontrolujte přístup k aplikacím. | Tým architektury InfoSec |
| Pravidelně kontrolujte přístup k externím identitám. | Tým architektury InfoSec |
| Pravidelně kontrolujte, kdo má privilegované role. | Tým architektury InfoSec |
| Definování bran zabezpečení pro aktivaci privilegovaných rolí | Tým architektury InfoSec |
| Pravidelně kontrolují udělení souhlasu. | Tým architektury InfoSec |
| Navrhněte katalogy a balíčky přístupu pro aplikace a prostředky na základě zaměstnanců v organizaci. | Vlastníci aplikace |
| Definování zásad zabezpečení, které uživatelům přiřadí přístup k balíčkům | Vlastníci aplikace InfoSec Team + |
| Pokud zásady zahrnují schvalovací pracovní postupy, pravidelně kontrolujte schválení pracovních postupů. | Vlastníci aplikace |
| Kontrola výjimek v zásadách zabezpečení, jako jsou zásady podmíněného přístupu, pomocí kontrol přístupu | Provozní tým InfoSec |

Při revizi seznamu se můžete setkat s tím, že budete muset buď přiřadit vlastníka pro úlohy, u kterých chybí vlastník, nebo upravit vlastnictví pro úlohy s vlastníky, které nejsou zarovnané na výše uvedená doporučení.

#### <a name="owner-recommended-reading"></a>Čtení Doporučené vlastníkem

- [Přiřazení rolí správce v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Zásady správného řízení v Azure](https://docs.microsoft.com/azure/security/governance-in-azure)

### <a name="configuration-changes-testing"></a>Testování změn konfigurace

Existují změny, které vyžadují zvláštní okolnosti při testování, od jednoduchých technik, jako je například zavedení cílové podmnožiny uživatelů pro nasazení změny v rámci paralelního testovacího tenanta. Pokud jste neimplementovali testovací strategii, měli byste definovat testovací přístup na základě pokynů v následující tabulce:

| Scénář| Doporučení |
|-|-|
|Změna typu ověřování z federovaných na KOSMETICE/PTA nebo naopak| Pomocí [připraveného zavedení](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-staged-rollout) otestujte dopad změny typu ověřování.|
|Zavedení nové zásady podmíněného přístupu (CA) nebo zásady ochrany identit|Vytvořte nové zásady certifikační autority a přiřaďte je testovacím uživatelům.|
|Připojování testovacího prostředí aplikace|Přidejte aplikaci do produkčního prostředí, skryjte ji z panelu MyApp a přiřaďte ji k testovacím uživatelům během fáze kontroly kvality (QA).|
|Změna pravidel synchronizace|Proveďte změny v testovacím Azure AD Connect se stejnou konfigurací, která je aktuálně v produkčním režimu, označovanou také jako pracovní režim a analyzujte výsledky CSExport. Pokud je to splněné, Proměňte do produkčního prostředí, až budete připraveni.|
|Změna brandingu|Otestujte v samostatném testovacím tenantovi.|
|Zavedení nové funkce|Pokud funkce podporuje zavedení do cílové sady uživatelů, identifikujte uživatele zkušebního nasazení a vystavíte je. Například Samoobslužné resetování hesla a služba Multi-Factor Authentication může cílit na konkrétní uživatele nebo skupiny.|
|Přímou migraci aplikaci od místního zprostředkovatele identity (IdP), třeba služby Active Directory, do Azure AD|Pokud aplikace podporuje více konfigurací IdP, například Salesforce, nakonfigurujte v rámci okna změny a otestujte službu Azure AD (v případě, že aplikace zavádí stránku HRD). Pokud aplikace nepodporuje více zprostředkovatelů identity, naplánujte testování během výpadku okna řízení změn a výpadku programu.|
|Aktualizovat pravidla dynamických skupin|Vytvořte paralelní dynamickou skupinu pomocí nového pravidla. Porovnejte na vypočtený výsledek, například spusťte PowerShell se stejnou podmínkou.<br>Pokud je test Pass, Proměňte místo, kde se stará Skupina použila (je-li to možné).|
|Migrace licencí na produkty|Odkaz na [změnu licence pro jednoho uživatele v licencované skupině v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-change-licenses).|
|Změna pravidel AD FS, jako je například autorizace, vystavení, MFA|K cílové podmnožině uživatelů použijte deklaraci skupiny uživatelů.|
|Změna možností AD FS ověřování nebo podobných změn v rámci farmy|Vytvořte paralelní farmu se stejným názvem hostitele, implementujte změny konfigurace, otestujte klienty pomocí souboru Hosts, pravidla směrování vyrovnávání zatížení sítě nebo podobného směrování.<br>Pokud cílová platforma nepodporuje soubory hostitelů (například mobilní zařízení), změna ovládacího prvku.|

## <a name="access-reviews"></a>Kontroly přístupu

### <a name="access-reviews-to-applications"></a>Kontroly přístupu k aplikacím

V průběhu času můžou uživatelé shromažďovat přístup k prostředkům při jejich přesunu do různých týmů a pozic. Je důležité, aby vlastníci prostředků zkontrolovali přístup k aplikacím v pravidelných intervalech a odebrali oprávnění, která už nejsou v průběhu životního cyklu uživatelů potřeba. Kontroly [přístupu](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) Azure AD umožňují organizacím efektivně spravovat členství ve skupinách, přístup k podnikovým aplikacím a přiřazování rolí. Vlastníci prostředků by měli pravidelně kontrolovat přístup uživatelů, aby měli jistotu, že budou mít přístup jenom přípravní uživatelé. V ideálním případě byste měli zvážit použití kontrol přístupu služby Azure AD pro tuto úlohu.

![Úvodní stránka kontroly přístupu](./media/active-directory-ops-guide/active-directory-ops-img15.png)

> [!NOTE]
> Každý uživatel, který komunikuje s recenzemi kontroly přístupu, musí mít placenou Azure AD Premiumovou licenci P2.

### <a name="access-reviews-to-external-identities"></a>Kontroly přístupu k externím identitám

Je důležité zajistit, aby byl přístup k externím identitám omezený jenom na prostředky, které jsou potřeba, a to v době, kdy je potřeba. Navažte běžný proces automatizovaného procesu kontroly přístupu pro všechny externí identity a přístup k aplikacím pomocí kontrol [přístupu](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)Azure AD. Pokud už proces existuje v místním prostředí, zvažte použití kontrol přístupu Azure AD. Jakmile je aplikace Vyřazená nebo už nepoužívaná, odeberte všechny externí identity, které měly přístup k aplikaci.

> [!NOTE]
> Každý uživatel, který komunikuje s recenzemi kontroly přístupu, musí mít placenou Azure AD Premiumovou licenci P2.

## <a name="privileged-account-management"></a>Správa privilegovaných účtů

### <a name="privileged-account-usage"></a>Využití privilegovaného účtu

Hackeři často cílí na účty správců a další prvky privilegovaného přístupu, aby rychle získali přístup k citlivým datům a systémům.Vzhledem k tomu, že uživatelé s privilegovanými rolemi se v průběhu času často shromažďují, je důležité pravidelně kontrolovat a spravovat přístup správců a poskytovat přístup k prostředkům Azure AD a k prostředkům Azure pomocí privilegovaného přístupu za běhu.

Pokud ve vaší organizaci neexistují žádné procesy pro správu privilegovaných účtů nebo v současnosti máte správce, kteří používají své běžné uživatelské účty ke správě služeb a prostředků, měli byste hned začít používat samostatné účty, například jeden pro běžné každodenní aktivity. druhý pro privilegovaný přístup a nakonfigurovaný s MFA. V případě, že vaše organizace má Azure AD Premium předplatné P2, je ještě lepší, měli byste hned nasadit [Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure#license-requirements) (PIM). Ve stejném tokenu byste si měli projít také tyto privilegované účty a v případě potřeby [přiřadit méně privilegované role](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure) .

Další aspektem správy privilegovaných účtů, které by se měly implementovat, je definování kontrol [přístupu](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) pro tyto účty, a to buď ručně, nebo [automaticky prostřednictvím PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-perform-security-review).

#### <a name="privileged-account-management-recommended-reading"></a>Doporučené čtení pro správu privilegovaných účtů

- [Role v Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-roles)

### <a name="emergency-access-accounts"></a>Účty pro nouzový přístup

Organizace musí vytvořit [Nouzový účet](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access) , který se bude připravovat pro správu Azure AD v případech, jako je například výpadek ověřování, například:

- Výpadek komponent infrastruktury ověřování (AD FS, místní AD, služba MFA)
- Obrat administrativních zaměstnanců

Aby nedošlo k nechtěnému uzamknutí vašeho tenanta, protože se nemůžete přihlásit nebo aktivovat existující účet individuálního uživatele jako správce, měli byste vytvořit dva nebo více mimořádných účtů a zajistit jejich implementaci a zarovnání k [osvědčeným](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure) [postupům Microsoftu a postupům přerušení](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#break-glass-what-to-do-in-an-emergency).

### <a name="privileged-access-to-azure-ea-portal"></a>Privilegovaný přístup k portálu Azure EA

[Portál azure smlouva Enterprise (Azure EA)](https://azure.microsoft.com/blog/create-enterprise-subscription-experience-in-azure-portal-public-preview/) umožňuje vytvářet předplatná Azure pro hlavní smlouva Enterprise, což je účinná role v rámci podniku. Je běžné začít vytvářet tento portál ještě před tím, než se služba Azure AD zavede, takže je nutné použít identity Azure AD k jeho uzamčení, odebrat osobní účty z portálu, zajistit správné delegování a zmírnit riziko uzamčení.

Aby bylo jasné, že úroveň autorizace portálu EA je aktuálně nastavená na smíšený režim, musíte odebrat všechny [účty Microsoft](https://support.skype.com/en/faq/FA12059/what-is-a-microsoft-account) ze všech privilegovaných přístupů na portálu EA a nakonfigurovat portál EA tak, aby používal jenom účty Azure AD. Pokud nejsou nakonfigurované delegované role portálu EA, měli byste také vyhledat a implementovat delegované role pro oddělení a účty.

#### <a name="privileged-access-recommended-reading"></a>Čtení Doporučené privilegovaného přístupu

- [Oprávnění role správce v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

## <a name="entitlement-management"></a>Správa nároků

[Správa oprávnění (em)](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) umožňuje vlastníkům aplikací seskupit prostředky a přiřadit je ke konkrétním osoby v organizaci (interní i externí). EM umožňuje samoobslužné registrace a delegování pro vlastníky podniku a přitom zachovává zásady správného řízení k udělení přístupu, nastavení doby trvání přístupu a povolení pracovních postupů schvalování. 

> [!NOTE]
> Správa nároků Azure AD vyžaduje Azure AD Premium licence P2.

## <a name="summary"></a>Souhrn

Existuje osm aspektů zabezpečení správného řízení identity. Tento seznam vám pomůže identifikovat akce, které byste měli provést při posuzování a ověřování přístupu k neprivilegovaným a privilegovaným identitám, auditům a řízení změn v prostředí.

- Přiřaďte vlastníky ke klíčovým úlohám.
- Implementujte strategii testování.
- Pomocí kontrol přístupu ke službě Azure AD můžete efektivně spravovat členství ve skupinách, přístup k podnikovým aplikacím a přiřazování rolí.
- Navažte pravidelný automatizovaný proces kontroly přístupu pro všechny typy externích identit a přístupu k aplikacím.
- Navažte proces kontroly přístupu, který umožňuje pravidelně kontrolovat a spravovat přístup správců, a poskytuje přístup k prostředkům Azure AD a prostředkům Azure pomocí privilegovaného přístupu za běhu.
- Zřizování účtů v nouzi pro správu Azure AD za neočekávané výpadky
- Odblokujte přístup k portálu Azure EA.
- Implementujte správu nároků k poskytnutí řízení přístupu ke kolekci prostředků.

## <a name="next-steps"></a>Další kroky

Začněte s [provozními kontrolami a akcemi služby Azure AD](active-directory-ops-guide-ops.md).
