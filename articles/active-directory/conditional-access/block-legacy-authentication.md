---
title: Blokovat starší verze ověřování – Azure Active Directory
description: Přečtěte si, jak vylepšit stav zabezpečení blokováním staršího ověřování pomocí podmíněného přístupu Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d3df4eee14e5ce2f0638058efde0f80d0e5b051
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87275475"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Postupy: blokování staršího ověřování ve službě Azure AD pomocí podmíněného přístupu   

Aby měli uživatelé snadný přístup k vašim cloudovým aplikacím, Azure Active Directory (Azure AD) podporuje širokou škálu ověřovacích protokolů, včetně starších verzí ověřování. Starší protokoly ale nepodporují službu Multi-Factor Authentication (MFA). Vícefaktorové ověřování je v mnoha prostředích běžným požadavkem na krádež identity. 

Alex Weinert, ředitel pro zabezpečení identity v Microsoftu, v jeho 12. března 2020 blogové příspěvky [nové nástroje, které zablokují starší verze ověřování ve vaší organizaci](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#) , zdůrazňují, proč by organizace měli blokovat starší verze ověřování a jaké další nástroje Microsoftu pro splnění této úlohy poskytují:

> Aby MFA bylo účinné, musíte také blokovat starší verze ověřování. Důvodem je to, že starší protokoly ověřování, jako jsou POP, SMTP, IMAP a MAPI, nemůžou vymáhat MFA, takže jim preferované vstupní body pro nežádoucí osoby útočí vaší organizaci...
> 
>... Čísla pro ověřování starší verze z analýzy provozu služby Azure Active Directory (Azure AD) jsou naprostém:
> 
> - Více než 99 procent útoků na postřik hesla používá starší protokoly ověřování.
> - Více než 97 procent útoků s přihlašovacími údaji používá starší verze ověřování.
> - Účty Azure AD v organizacích, které mají zakázané starší verze ověřování 67% méně ohrožení než u těch, kde je povolené starší verze ověřování
>

Pokud je vaše prostředí připravené k blokování starší verze ověřování, aby se zlepšila ochrana vašeho tenanta, můžete tento cíl dosáhnout pomocí podmíněného přístupu. Tento článek vysvětluje, jak můžete nakonfigurovat zásady podmíněného přístupu, které blokují ověřování starší verze pro vašeho tenanta.

## <a name="prerequisites"></a>Požadavky

V tomto článku se předpokládá, že máte zkušenosti s nástrojem: 

- [Základní koncepty](overview.md) podmíněného přístupu Azure AD 
- [Osvědčené postupy](best-practices.md) pro konfiguraci zásad podmíněného přístupu v Azure Portal

## <a name="scenario-description"></a>Popis scénáře

Azure AD podporuje několik nejčastěji používaných ověřovacích a autorizačních protokolů, včetně starších verzí ověřování. Starší verze ověřování odkazuje na protokoly, které používají základní ověřování. Obvykle tyto protokoly nemůžou vyhovět žádnému typu druhého faktoru ověřování. Příklady pro aplikace, které jsou založené na starších verzích ověřování:

- Starší aplikace systém Microsoft Office
- Aplikace používající poštovní protokoly jako POP, IMAP a SMTP

Jeden faktor ověřování (například uživatelské jméno a heslo) není dostatečný počet dnů. Hesla jsou chybná, protože je lze snadno uhodnout a my (lidi) jsou při volbě dobrých hesel špatná. Hesla jsou taky zranitelná u různých útoků, jako je phishing a sprej hesla. Jedním z nejjednodušších věcí, které můžete chránit před hrozbami s heslem, je implementace MFA. V případě vícefaktorového ověřování, a to i v případě, že útočník získá heslo uživatele, nestačí k úspěšnému ověření a přístupu k datům použít heslo samotné.

Jak můžete zabránit tomu, aby aplikace používaly starší ověřování pro přístup k prostředkům vašeho tenanta? Doporučení je jednoduše zablokované zásadou podmíněného přístupu. V případě potřeby povolujete použití aplikací, které jsou založené na starších verzích ověřování, jenom určitým uživatelům a konkrétním síťovým umístěním.

Zásady podmíněného přístupu se vynutily po dokončení prvního faktoru ověřování. Proto podmíněný přístup není určený jako ochrana prvního řádku pro scénáře, jako jsou útoky DoS (Denial-of-Service), ale může využít signály z těchto událostí (například úroveň rizika přihlašování, umístění žádosti atd.) a určit tak přístup.

## <a name="implementation"></a>Implementace

V této části se dozvíte, jak nakonfigurovat zásady podmíněného přístupu pro blokování starších verzí ověřování. 

### <a name="legacy-authentication-protocols"></a>Starší protokoly pro ověřování

Následující možnosti jsou považovány za starší protokoly ověřování.

- Ověřený protokol SMTP, pomocí kterého klient POP a IMAP odesílá e-mailové zprávy.
- Automatická konfigurace – používá klienti Outlooku a EAS k vyhledání a připojení k poštovním schránkám v Exchangi Online.
- Protokol Exchange ActiveSync (EAS) – používá se pro připojení k poštovním schránkám v Exchangi Online.
- Exchange Online PowerShell – slouží k připojení k Exchangi Online pomocí vzdáleného prostředí PowerShell. Pokud zablokujete základní ověřování pro prostředí Exchange Online PowerShell, musíte k připojení použít modul prostředí Exchange Online PowerShell. Pokyny najdete v tématu [připojení k prostředí PowerShell pro Exchange Online pomocí služby Multi-Factor Authentication](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
- Webové služby Exchange (EWS) – programovací rozhraní, které používá Outlook, Outlook pro Mac a aplikace třetích stran.
- IMAP4 – používá e-mailové klienty IMAP.
- Rozhraní MAPI přes protokol HTTP (MAPI/HTTP) – používá Outlook 2010 a novější.
- Adresář v režimu offline (OAB) – kopii kolekcí seznamu adres, které jsou staženy a používány aplikací Outlook.
- Outlook odkudkoli (RPC over HTTP) – používá Outlook 2016 a starší.
- Služba Outlook – používaná aplikací pro poštu a kalendář pro Windows 10
- POP3 – používá e-mailové klienty POP.
- Webové služby vytváření sestav – slouží k načtení dat sestavy v Exchangi Online.
- Ostatní klienti – jiné protokoly identifikované jako s využitím starší verze ověřování.

Další informace o těchto ověřovacích protokolech a službách najdete v tématu [sestavy aktivit přihlašování na portálu Azure Active Directory](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities).

### <a name="identify-legacy-authentication-use"></a>Identifikovat použití starší verze ověřování

Než budete moct v adresáři zablokovat starší verze ověřování, musíte nejdřív pochopit, jestli mají vaši uživatelé aplikace, které používají starší verze ověřování a jak ovlivňují celkový adresář. Protokoly přihlášení k Azure AD se dají použít k pochopení, jestli používáte starší verze ověřování.

1. Přejděte do **Azure Portal**  >  **Azure Active Directory**  >  **přihlášení**.
1. Pokud není zobrazený, klikněte na **sloupce**  >  **klientská aplikace**a přidejte sloupec aplikace klienta.
1. **Přidat filtry**  >  **Klientská aplikace** > vybrat všechny starší protokoly pro ověřování. Vyberte mimo dialogové okno filtrování Blox a použijte výběr a zavřete dialogové okno.

Filtrování zobrazí jenom ty pokusy o přihlášení, které udělaly starší protokoly ověřování. Kliknutím na každý jednotlivý pokus o přihlášení zobrazíte další podrobnosti. V poli **klientská aplikace** na kartě **základní informace** se určí, který starší verze ověřovacího protokolu se použil.

Tyto protokoly určují, kteří uživatelé pořád závisejí na starší verzi ověřování a které aplikace používají starší protokoly k provádění požadavků na ověření. Pro uživatele, kteří se v těchto protokolech nezobrazují a které se potvrzují, že nepoužívají starší verze ověřování, implementujte zásady podmíněného přístupu jenom pro tyto uživatele.

### <a name="block-legacy-authentication"></a>Blokování starší verze ověřování 

V zásadách podmíněného přístupu můžete nastavit podmínky, které jsou svázané s klientskými aplikacemi, které se používají pro přístup k prostředkům. Podmínka klientské aplikace umožňuje zúžit rozsah aplikací na aplikace pomocí starší verze ověřování výběrem **klientů Exchange ActiveSync** a **dalších klientů** v části **mobilní aplikace a klienti klasické pracovní plochy**.

![Ostatní klienti](./media/block-legacy-authentication/01.png)

Pokud chcete pro tyto aplikace blokovat přístup, musíte vybrat možnost **blokovat přístup**.

![Blokování přístupu](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Výběr uživatelů a cloudových aplikací

Pokud chcete pro vaši organizaci zablokovat starší verze ověřování, pravděpodobně si myslíte, že to můžete provést výběrem možnosti:

- Všichni uživatelé
- Všechny cloudové aplikace
- Blokování přístupu

![Přiřazení](./media/block-legacy-authentication/03.png)

Azure má funkci zabezpečení, která vám brání v vytváření zásad, protože tato konfigurace je v rozporu s [osvědčenými postupy](best-practices.md) pro zásady podmíněného přístupu.
 
![Konfigurace zásad není podporovaná.](./media/block-legacy-authentication/04.png)

Funkce zabezpečení je nezbytná, protože *zablokuje všem uživatelům a všem cloudovým aplikacím* možnost blokovat vaší celé organizaci přihlášení k vašemu tenantovi. Musíte vyloučit alespoň jednoho uživatele, aby splňoval požadavek minimálního osvědčeného postupu. Můžete také vyloučit roli adresáře.

![Konfigurace zásad není podporovaná.](./media/block-legacy-authentication/05.png)

Tuto bezpečnostní funkci můžete vyhovět vyloučením jednoho uživatele ze zásad. V ideálním případě byste měli [v Azure AD definovat několik účtů pro správu pro nouzový přístup](../users-groups-roles/directory-emergency-access.md) a vyloučit je ze zásad.

Použití [režimu pouze pro sestavy](concept-conditional-access-report-only.md) při povolování zásad pro blokování starších verzí ověřování poskytuje vaší organizaci možnost sledovat, jaký bude mít dopad na zásadu.

## <a name="policy-deployment"></a>Nasazení zásad

Před umístěním zásad do produkčního prostředí se postará:
 
- **Účty služeb** – Identifikujte uživatelské účty používané jako účty služeb nebo zařízení, jako jsou telefony konferenční místnosti. Ujistěte se, že tyto účty mají silná hesla a přidejte je do Vyloučené skupiny.
- **Sestavy přihlášení** – zkontrolujte sestavu přihlášení a vyhledejte **Další přenosy klientů** . Identifikujte nejdůležitější využití a zjistěte, proč se používá. Obvykle se provoz generuje pomocí starších klientů Office, kteří nepoužívají moderní ověřování ani některé e-mailové aplikace třetích stran. Vytvořte plán pro přesun využití těchto aplikací, nebo pokud je tento dopad nízký, upozorněte uživatele, že už tyto aplikace nemůžou používat.
 
Další informace najdete v tématu [Jak byste měli nasadit novou zásadu?](best-practices.md#how-should-you-deploy-a-new-policy).

## <a name="what-you-should-know"></a>Co byste měli vědět

Blokování přístupu pomocí **jiných klientů** také blokuje Exchange Online PowerShell a Dynamics 365 pomocí základního ověřování.

Konfigurace zásad pro **ostatní klienty** zablokuje celou organizaci od určitých klientů, jako je SPConnect. K tomuto bloku dochází, protože se starší klienti Ověřují neočekávaným způsobem. Tento problém se nevztahuje na hlavní aplikace Office, jako jsou starší klienti Office.

Může trvat až 24 hodin, než se zásada dostanou platit.

Pro podmínky **ostatních klientů** můžete vybrat všechny dostupné ovládací prvky pro udělení oprávnění. činnost koncového uživatele je ale vždycky zablokovaný přístup.

Pokud zablokujete starší ověřování pomocí podmínky **ostatních klientů** , můžete také nastavit platformu zařízení a podmínku umístění. Pokud třeba chcete pro mobilní zařízení blokovat jenom starší verze ověřování, nastavte podmínku pro **platformy zařízení** tak, že vyberete:

- Android
- iOS
- telefon se systémem Windows

![Konfigurace zásad není podporovaná.](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Další kroky

- [Určení dopadu pomocí režimu pouze sestavy podmíněného přístupu](howto-conditional-access-report-only.md)
- Pokud ještě nejste obeznámeni s konfigurací zásad podmíněného přístupu, přečtěte si téma [vyžádání MFA pro konkrétní aplikace s Azure Active Directory podmíněný přístup](app-based-mfa.md) pro příklad.
- Další informace o podpoře moderního ověřování najdete v článku [jak moderní ověřování funguje pro klientské aplikace office 2013 a office 2016](/office365/enterprise/modern-auth-for-office-2013-and-2016) . 
- [Jak nastavit multifunkční zařízení nebo aplikaci pro odesílání e-mailů pomocí Office 365 a Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
