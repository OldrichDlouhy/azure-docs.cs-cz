---
title: Uživatelé označení příznakem rizika na portálu Azure Active Directory | Microsoft Docs
description: Přečtěte si o sestavě zabezpečení Uživatelé označení příznakem rizika na portálu Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3037dd2e03062ebab0c48438aa4345a766a22fc8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608921"
---
# <a name="remediate-users-flagged-for-risk-in-the-azure-active-directory-portal"></a>Oprava uživatelů označených příznakem rizika na portálu Azure Active Directory

Pomocí sestav zabezpečení v Azure Active Directory (Azure AD) můžete posoudit pravděpodobnost napadení uživatelských účtů ve vašem prostředí. Uživatel označený příznakem rizika je indikátorem uživatelského účtu, který mohl být ohrožený.

Prioritou společnosti Microsoft je přispívat k udržení zabezpečení vašich prostředí. V rámci této snahy Microsoft neustále monitoruje aktivity, které jsou neobvyklé nebo odpovídají známým vzorcům útoku. 

Pokud se zjistí neobvyklé aktivity, které můžou označovat neoprávněný přístup k některým účtům vašich uživatelů, dostanete oznámení, která vám umožní provést akci. To neznamená, že došlo k ohrožení zabezpečení vlastních systémů společnosti Microsoft.

## <a name="access-the-users-flagged-for-risk-report"></a>Zpřístupnění sestavy uživatelů označených příznakem rizika

Můžete zkontrolovat uživatele označené příznakem rizika prostřednictvím [sestavy rizika pro uživatele](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RiskyUsers) v Azure Portal. Pokud službu Azure AD nemáte, můžete se zdarma zaregistrovat na adrese [https://aka.ms/AccessAAD](https://aka.ms/AccessAAD) . 

V sestavě uživatelů označených příznakem rizika můžete pro každého uživatele provést následující akce:

- Vygenerovat dočasné heslo
- Vyžádat, aby si uživatel při příštím přihlášení bezpečně resetoval heslo
- Zavřít riziko uživatele bez provedení nápravné akce

Další informace najdete v tématu [o sestavě zabezpečení pro uživatele označené příznakem rizika](concept-user-at-risk.md).

### <a name="azure-ad-subscription-for-office-365-customers"></a>Předplatné služby Azure AD pro zákazníky Office 365

Pro přístup k **centru pro správu Azure**můžete použít také přihlašovací údaje sady Office 365. Po aktivaci přístupu ke službě Azure AD budete přesměrováni na portál služby Azure AD. Na úrovni základního předplatného je v sestavách uvedené omezené množství podrobností. Pro předplatitele Azure na úrovni Premium jsou k dispozici další data a analýzy.

Přístup k sestavám **uživatelů označeným příznakem rizika** v centru pro správu Microsoft 365:

1.  V navigační nabídce na levé straně vyberte **centra pro správu**. 
2.  Vyberte **Azure AD**.
3.  Přihlaste se do **Centra pro správu Azure Active Directory**.
4.  Pokud se v horní části stránky zobrazí nápis, který uvádí **nový portál**, vyberte odkaz.
4.  V navigační nabídce na levé straně vyberte **Azure Active Directory**. 
5.  V navigačním podokně vyberte **uživatele označené příznakem rizika** v části **zabezpečení** .

## <a name="remediation-actions"></a>Nápravné akce

Provedení následujících akcí vám pomůže napravit ovlivněné účty a zabezpečit vaše prostředí:

1.  [Ověřte správné informace](https://aka.ms/MFAValid) pro Multi-Factor Authentication a Samoobslužné resetování hesla. 
2.  [Povolte službu Multi-Factor Authentication](https://aka.ms/MFAuth) pro všechny uživatele. 
3.  Tento [skript](https://aka.ms/remediate) pro napravení použijte pro každý ovlivněný účet a automaticky proveďte následující kroky: 

    a. Resetujte heslo pro zabezpečení účtu a ukončete aktivní relace.

    b. Odstranit delegáty poštovní schránky.

    c. Zakázat pravidla předávání e-mailů do externích domén.

    d. Odebrat z poštovní schránky globální vlastnost předávání e-mailů.

    e. Povolit MFA pro účet uživatele.

    f. Nastavit vysokou složitost hesla účtu.

    například Povolit auditování poštovní schránky.

    h. Vytvořte protokol auditu, který správce může zkontrolovat.

4. Prozkoumejte svého tenanta Office 365 a další IT infrastrukturu, včetně kontroly případných úprav všech nastavení tenanta, uživatelských účtů a nastavení konfigurace jednotlivých uživatelů. Zkontrolujte indikátory metod trvalosti a také indikátory, že útočník mohl k získání přihlašovacích údajů sítě VPN nebo přístupu k jiným prostředkům organizace využít počáteční základnu. 

5.  V rámci šetření zvažte, zda byste měli informovat státní úřady, včetně vynucování zákonů.

Kromě toho byste měli provést následující:

- Přečtěte si a implementujte tyto [pokyny k adresování neobvyklých aktivit](https://aka.ms/fixaccount). 
- [Povolte kanálu auditu](https://aka.ms/improvesecurity) , který vám umožní analyzovat aktivitu ve vašem tenantovi. Po dokončení se vaše úložiště auditu začne naplňovat pomocí protokolů aktivit. V tomto okamžiku můžete také využít [prostředek pro hledání a vyšetřování centra zabezpečení a dodržování předpisů](https://aka.ms/sccsearch). 
- Zkontrolovat oprávnění delegáta a pravidla předávání e-mailů pro všechny vaše poštovní schránky. Tuto úlohu můžete provést pomocí tohoto [skriptu PowerShellu](https://aka.ms/delegateforwardrules). 

## <a name="next-steps"></a>Další kroky

* [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
* [Uživatelé označení příznakem rizika](concept-user-at-risk.md)
