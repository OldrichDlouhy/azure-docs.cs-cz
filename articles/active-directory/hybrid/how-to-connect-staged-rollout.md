---
title: 'Azure AD Connect: cloudové ověřování prostřednictvím připraveného zavedení | Microsoft Docs'
description: Tento článek vysvětluje, jak migrovat z federovaného ověřování na cloudové ověřování pomocí dvoufázové implementace.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/03/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2f547aa900c1b8dbea27eceff7ac7ebc86a83e33
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019824"
---
# <a name="migrate-to-cloud-authentication-using-staged-rollout-preview"></a>Migrace na cloudové ověřování pomocí připraveného zavedení (Preview)

Příprava na přípravu umožňuje selektivně testovat skupiny uživatelů s možnostmi cloudového ověřování, jako je Azure Multi-Factor Authentication (MFA), podmíněný přístup, ochrana identity pro nevrácená pověření, řízení identit a další, a to ještě před vyjmutím z domén.  Tento článek popisuje, jak provést tento přepínač. Než začnete postupovat podle fáze zavedení, měli byste zvážit důsledky, pokud je splněna jedna nebo více z následujících podmínek:
    
-  Aktuálně používáte místní Multi-Factor Authentication Server. 
-  Pro ověřování používáte čipové karty. 
-  Váš aktuální server nabízí určité jenom federační funkce.

Než se pustíte do této funkce, doporučujeme, abyste si přečtěte náš průvodce pro výběr správné metody ověřování. Další informace najdete v tabulce "porovnání metod" v tématu [Volba správné metody ověřování pro Azure Active Directory řešení hybridní identity](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn#comparing-methods).

Přehled této funkce najdete v tomto tématu Azure Active Directory: co je postupné zavedení?. obrazový

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3inQJ]



## <a name="prerequisites"></a>Předpoklady

-   Máte tenanta Azure Active Directory (Azure AD) se federovanémi doménami.

-   Rozhodli jste se přesunout jednu ze dvou možností:
    - **Možnost A**  -  *synchronizace hodnot hash hesel (synchronizace)*  +  *bezproblémové jednotné přihlašování (SSO)*.  Další informace najdete v tématu [co je synchronizace hodnot hash hesel](whatis-phs.md) a [co je bezproblémové jednotné přihlašování](how-to-connect-sso.md) .
    - **Možnost B**  -  *předávací ověřování*  +  *bezproblémové jednotné přihlašování*  Další informace najdete v tématu [co je předávací ověřování](how-to-connect-pta.md) .  
    
    I když je *snadné jednotné přihlašování* volitelné, doporučujeme, aby mu zajistili tiché přihlašování pro uživatele, kteří používají počítače připojené k doméně v rámci podnikové sítě.

-   Pro uživatele, kteří se migrují do cloudového ověřování, jste nakonfigurovali všechny příslušné zásady klienta a zásad podmíněného přístupu, které potřebujete.

-   Pokud plánujete použít Azure Multi-Factor Authentication, doporučujeme použít [kombinovanou registraci pro Samoobslužné resetování hesla (SSPR) a Multi-Factor Authentication](../authentication/concept-registration-mfa-sspr-combined.md) , aby vaši uživatelé mohli své metody ověřování registrovat jednou.

-   Pokud chcete použít funkci dvoufázové zavedení, musíte být globálním správcem vašeho tenanta.

-   Pokud chcete povolit *bezproblémové přihlašování* v konkrétní doménové struktuře služby Active Directory, musíte být správce domény.


## <a name="supported-scenarios"></a>Podporované scénáře

Následující scénáře jsou podporovány pro připravené zavedení. Funkce funguje pouze pro:

- Uživatele, kteří jsou zřízeni ve službě Azure AD pomocí Azure AD Connect. Netýká se pouze cloudových uživatelů.

- Přihlašovací data uživatelů v prohlížečích a v *moderních ověřovacích* klientech. Aplikace nebo cloudové služby, které používají starší verze ověřování, se vrátí do toků federovaného ověřování. Příkladem může být Exchange Online se moderním ověřováním vypnuto nebo Outlook 2010, které nepodporuje moderní ověřování.
- Velikost skupiny je aktuálně omezená na 50 000 uživatelů.  Pokud máte skupiny větší než 50 000 uživatelů, doporučujeme tuto skupinu rozdělit do několika skupin pro připravené zavedení.

## <a name="unsupported-scenarios"></a>Nepodporované scénáře

Následující scénáře nejsou podporovány pro fáze zavedení:

- Určité aplikace odesílají parametr dotazu "domain_hint" do služby Azure AD během ověřování. Tyto toky budou pokračovat a uživatelé, kteří jsou povoleni pro dvoufázové zavedení, budou pro ověřování nadále používat federaci.

<!-- -->

- Správci mohou zavést cloudové ověřování pomocí skupin zabezpečení. Abyste se vyhnuli latenci při použití místních skupin zabezpečení služby Active Directory, doporučujeme, abyste používali skupiny zabezpečení cloudu. Platí následující podmínky:

    - Na jednu funkci můžete použít maximálně 10 skupin. To znamená, že můžete použít 10 skupin pro každou *synchronizaci hodnot hash hesel*, *předávací ověřování*a *bezproblémové jednotné přihlašování*.
    - Vnořené skupiny se *nepodporují*. Tento obor platí i pro Public Preview.
    - Pro fáze zavedení se *nepodporují* dynamické skupiny.
    - Objekty kontaktu uvnitř skupiny zablokují přidávání skupiny.

- Ke konečnému přímou migraci je potřeba z federovaných na cloudové ověřování vytvořit pomocí Azure AD Connect nebo PowerShellu. Při přípravě na přípravu se nemění doména ze federované na spravovanou.  Další informace o přímou migraci domény najdete v článku [migrace z federace na synchronizaci hodnoty hash hesla](plan-migrate-adfs-password-hash-sync.md) a [migrace z federace na předávací ověřování](plan-migrate-adfs-pass-through-authentication.md) .



- Když poprvé přidáte skupinu zabezpečení pro dvoufázové zavedení, budete omezeni na 200 uživatelů, aby nedocházelo k vypršení časového limitu uživatelského prostředí. Po přidání skupiny můžete podle potřeby přidat do ní další uživatele přímo.

- I když jsou uživatelé ve fázi zavedení, jsou zásady vypršení platnosti hesla nastavené na 90 dnů bez možnosti přizpůsobení. 


## <a name="get-started-with-staged-rollout"></a>Začínáme s fází uvedení do provozu

Pokud chcete otestovat přihlášení k *synchronizaci hodnot hash hesel* pomocí připraveného zavedení, postupujte podle pokynů v následující části.

Informace o tom, které rutiny prostředí PowerShell použít, najdete v tématu [Azure AD 2,0 Preview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout).

## <a name="pre-work-for-password-hash-sync"></a>Předběžná práce pro synchronizaci hodnot hash hesel

1. Povolte *synchronizaci hodnot hash hesel*na   stránce [volitelné funkce](how-to-connect-install-custom.md#optional-features)   v Azure AD Connect. 

   ![Snímek obrazovky se stránkami volitelné funkce v Azure Active Directory Connect](media/how-to-connect-staged-rollout/sr1.png)

1. Zajistěte, aby byl spuštěn cyklus *synchronizace hodnot hash celého hesla* , aby všechny hodnoty hash hesel uživatelů byly synchronizovány do služby Azure AD. Ke kontrole stavu *synchronizace hodnot hash hesel*můžete použít diagnostiku PowerShellu při [řešení potíží se synchronizací hodnot hash hesel pomocí Azure AD Connect synchronizace](tshoot-connect-password-hash-synchronization.md).

   ![Snímek obrazovky s protokolem pro řešení potíží s AADConnect](./media/how-to-connect-staged-rollout/sr2.png)

Pokud chcete testovat *předávací ověřování* při přihlášení pomocí připraveného zavedení, povolte ho podle pokynů v následující části.

## <a name="pre-work-for-pass-through-authentication"></a>Předběžná práce pro předávací ověřování

1. Identifikujte Server se systémem Windows Server 2012 R2 nebo novějším, kde chcete spustit agenta *předávacího ověřování* . 

   *Nevybírejte server* Azure AD Connect.Ujistěte se, že je server připojený k doméně, umožňuje ověřovat vybrané uživatele pomocí služby Active Directory a může komunikovat se službou Azure AD na odchozích portech a adresách URL. Další informace najdete v části "krok 1: ověření požadavků" v tématu [rychlý Start: Azure AD bezproblémové jednotné přihlašování](how-to-connect-sso-quick-start.md).

1. [Stáhněte si Azure AD Connect ověřovacího agenta](https://aka.ms/getauthagent)a nainstalujte ho na server. 

1. Pokud chcete povolit [vysokou dostupnost](how-to-connect-sso-quick-start.md), nainstalujte další ověřovací agenty na jiné servery.

1. Ujistěte se, že jste správně nakonfigurovali [Nastavení inteligentního uzamčení](../authentication/howto-password-smart-lockout.md) . Díky tomu je možné zajistit, aby účty uživatelů místní služby Active Directory nebyly uzamčeny nesprávnými aktéry.

Doporučujeme, abyste povolili *bezproblémové přihlašování* bez ohledu na způsob přihlašování (*synchronizace hodnot hash hesel* nebo *předávací ověřování*), který jste vybrali pro připravené zavedení. Pokud chcete povolit *bezproblémové jednotné přihlašování*, postupujte podle pokynů v následující části.

## <a name="pre-work-for-seamless-sso"></a>Předběžná práce pro bezproblémové přihlašování

Pomocí prostředí PowerShell povolte *bezproblémové jednotné přihlašování*   v doménových strukturách služby Active Directory. Pokud máte více než jednu doménovou strukturu služby Active Directory, povolte ji pro každou doménovou strukturu jednotlivě.  *Bezproblémové jednotné přihlašování* se aktivuje jenom pro uživatele, kteří jsou vybráni pro připravené zavedení. Nemá vliv na stávající nastavení federace.

Pomocí následujícího postupu povolte *bezproblémové přihlašování* :

1. Přihlaste se k serveru Azure AD Connect.

2. Přejít do složky *% ProgramFiles% \\ Microsoft Azure Active Directory Connect*   .

3. Pomocí následujícího příkazu Importujte modul prostředí PowerShell pro *bezproblémové přihlašování* : 

   `Import-Module .\AzureADSSO.psd1`

4. Spusťte PowerShell jako správce. V prostředí PowerShell volejte  `New-AzureADSSOAuthenticationContext` . Tento příkaz otevře podokno, kde můžete zadat přihlašovací údaje globálního správce vašeho tenanta.

5. Volání  `Get-AzureADSSOStatus | ConvertFrom-Json` . Tento příkaz zobrazí seznam doménových struktur služby Active Directory (viz seznam domény), na kterém je tato funkce povolená. Ve výchozím nastavení je nastavena na hodnotu false na úrovni tenanta.

   ![Příklad výstupu Windows PowerShellu](./media/how-to-connect-staged-rollout/sr3.png)

6. Volání  `$creds = Get-Credential` . Na příkazovém řádku zadejte přihlašovací údaje správce domény pro požadovanou doménovou strukturu služby Active Directory.

7. Volání `Enable-AzureADSSOForest -OnPremCredentials $creds` . Tento příkaz vytvoří účet počítače AZUREADSSOACC z místního řadiče domény pro doménovou strukturu služby Active Directory, která je potřebná pro *bezproblémové jednotné přihlašování*.

8. *Bezproblémové jednotné přihlašování* vyžaduje, aby adresy URL byly v zóně intranetu. Pokud chcete tyto adresy URL nasadit pomocí zásad skupiny, přečtěte si [rychlý Start: Azure AD bezproblémové jednotné přihlašování](how-to-connect-sso-quick-start.md#step-3-roll-out-the-feature).

9. Úplný návod vám umožní také stáhnout [plány nasazení](https://aka.ms/SeamlessSSODPDownload) pro *bezproblémové přihlašování*.

## <a name="enable-staged-rollout"></a>Povolit připravené zavedení

Pokud chcete zavést konkrétní funkci (*předávací ověřování*, *synchronizaci hodnot hash hesel*nebo *bezproblémové přihlašování*) k výběru skupiny uživatelů ve skupině, postupujte podle pokynů v dalších částech.

### <a name="enable-a-staged-rollout-of-a-specific-feature-on-your-tenant"></a>Povolit dvoufázové zavedení konkrétní funkce ve vašem tenantovi

Můžete zavést jednu z těchto možností:

- **Možnost A**  -  synchronizace hodnot hash *hesel*  +  *bezproblémové jednotné přihlašování*
- **Možnost B**  -  *předávací ověřování*  +  *bezproblémové jednotné přihlašování*
- **Nepodporováno**  -  synchronizace hodnot hash *hesel*  +  *předávací ověřování*  +  *bezproblémové jednotné přihlašování*

Postupujte následovně:

1. Pokud chcete získat přístup k UŽIVATELSKÉmu rozhraní verze Preview, přihlaste se k [portálu Azure AD](https://aka.ms/stagedrolloutux).

2. Vyberte odkaz **povolit dvoufázové zavedení pro přihlášení ke spravovanému uživateli (Preview)** .

   Chcete-li například povolit *možnost a*, posuňte **synchronizaci hodnot hash hesel** a **bezproblémové jednotné přihlašování** na **zapnuto**, jak je znázorněno na následujících obrázcích.

   ![Stránka Azure AD Connect](./media/how-to-connect-staged-rollout/sr4.png)

   ![Stránka "povolit funkce pro přípravu na více verzí (Preview)"](./media/how-to-connect-staged-rollout/sr5.png)

3. Přidejte skupiny do funkce, aby se povolilo *předávací ověřování* a *bezproblémové jednotné přihlašování*. Aby nedošlo k vypršení časového limitu uživatelského rozhraní, ujistěte se, že skupiny zabezpečení neobsahují nejdříve 200 členů.

   ![Stránka "Správa skupin pro synchronizaci hodnot hash hesel (Preview)"](./media/how-to-connect-staged-rollout/sr6.png)

   >[!NOTE]
   >Členové ve skupině jsou automaticky povoleni pro připravené zavedení. Vnořené a dynamické skupiny se pro připravené zavedení nepodporují.
   >Při přidávání nové skupiny budou uživatelé v této skupině (až 200 uživatelů pro novou skupinu) aktualizováni tak, aby používali spravované ověřování immidiatly. Úprava skupiny (přidávání nebo odebírání uživatelů) může trvat až 24 hodin, než se změny projeví.

## <a name="auditing"></a>Auditování

Povolili jsme události auditu pro různé akce, které provedeme pro připravené zavedení:

- Událost auditu, když povolíte dvoufázové zavedení pro *synchronizaci hodnot hash hesel*, *předávací ověřování*nebo *bezproblémové jednotné přihlašování*.

  >[!NOTE]
  >Událost auditu je zaznamenána v případě, že je zapnuté *bezproblémové jednotné přihlašování* pomocí připraveného zavedení.

  ![Podokno vytvořit zásady zavedení pro funkci – karta aktivita](./media/how-to-connect-staged-rollout/sr7.png)

  ![Karta vlastnosti pro vytvoření zásady zavedení pro funkci – změněné vlastnosti](./media/how-to-connect-staged-rollout/sr8.png)

- Událost auditu při přidání skupiny do *synchronizace hodnot hash hesel*, *předávacího ověřování*nebo *bezproblémového přihlašování*

  >[!NOTE]
  >Událost auditu se zaprotokoluje, když se do *synchronizace hodnot hash hesel* pro připravené zavedení přidá skupina.

  ![Podokno přidat skupinu do funkce – karta aktivita](./media/how-to-connect-staged-rollout/sr9.png)

  ![Podokno přidat skupinu do zavedení funkcí – Změna vlastností – karta vlastnosti](./media/how-to-connect-staged-rollout/sr10.png)

- Událost auditu, pokud je uživatel, který byl přidán do skupiny, povolen pro připravené zavedení.

  ![Podokno pro přidání uživatele do funkce – karta aktivita](media/how-to-connect-staged-rollout/sr11.png)

  ![Karta cíle v podokně Přidat uživatele do funkce – cíl (s)](./media/how-to-connect-staged-rollout/sr12.png)

## <a name="validation"></a>Ověřování

Pokud chcete otestovat přihlášení pomocí *synchronizace hodnot hash hesel* nebo *předávacího ověřování* (uživatelské jméno a heslo), udělejte toto:

1. V extranetu přejděte na [stránku aplikace](https://myapps.microsoft.com) v privátní relaci prohlížeče a pak zadejte hodnotu USERPRINCIPALNAME (UPN) uživatelského účtu, který je vybraný pro připravené zavedení.

   Uživatelé, na které bylo cíleno na přípravu, nejsou přesměrováni na vaši federované přihlašovací stránku. Místo toho se zobrazí výzva, abyste se přihlásili na přihlašovací stránce klienta Azure AD – branding.

1. Zajistěte, aby se přihlášení úspěšně zobrazilo v [sestavě aktivit přihlášení Azure AD](../reports-monitoring/concept-sign-ins.md) pomocí filtrování pomocí třídy userPrincipalName.

Testování přihlášení pomocí *bezproblémového jednotného přihlašování*:

1. V intranetu přejděte na [stránku aplikace](https://myapps.microsoft.com) v privátní relaci prohlížeče a pak zadejte hodnotu USERPRINCIPALNAME (UPN) uživatelského účtu, který je vybraný pro připravené zavedení.

   Uživatelům, kteří byli cíleni na dvoufázové zavedení *bezproblémového PŘIhlašování* , se zobrazí zpráva "Probíhá pokus o přihlášení..." zpráva před tím, než se tiše přihlásí.

1. Zajistěte, aby se přihlášení úspěšně zobrazilo v [sestavě aktivit přihlášení Azure AD](../reports-monitoring/concept-sign-ins.md) pomocí filtrování pomocí třídy userPrincipalName.

   Pokud chcete sledovat přihlašování uživatelů, ke kterým stále dochází na Active Directory Federation Services (AD FS) (AD FS) pro vybrané uživatele se zahrnutými fázemi, postupujte podle pokynů v tématu [AD FS řešení potíží: události a protokolování](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging#types-of-events). Podívejte se na dokumentaci výrobce, kde najdete informace o tom, jak tuto kontrolu u poskytovatelů federačních třetích stran vyzkoušet.

## <a name="remove-a-user-from-staged-rollout"></a>Odebrání uživatele z připraveného zavedení

Odebráním uživatele ze skupiny se pro tohoto uživatele zakáže fáze zavedení. Pokud chcete funkci dvoufázové zavedení zakázat, přesuňte ovládací prvek zpět na **off**.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Otázka: je možné tuto funkci použít v produkčním prostředí?**

Odpověď: Ano, tuto funkci můžete použít ve vašem provozním tenantovi, ale doporučujeme, abyste si ji nejprve vyzkoušeli ve vašem tenantovi test.

**Otázka: Tato funkce se dá použít k údržbě trvalé "společné existence", kde někteří uživatelé používají federované ověřování a jiní používají cloudové ověřování?**

Odpověď: Ne, tato funkce je navržena pro migraci z federovaných do cloudového ověřování ve fázích a pak na jejich následné vyjmutí do cloudového ověřování. Nedoporučujeme používat trvalý smíšený stav, protože tento přístup by mohl vést k neočekávaným tokům ověřování.

**Otázka: mohu použít PowerShell k provedení připraveného zavedení?**

Odpověď: Ano. Další informace o tom, jak používat PowerShell k provádění připraveného zavedení, najdete v tématu [Azure AD Preview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout).

## <a name="next-steps"></a>Další kroky
- [Azure AD 2,0 Preview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout )
