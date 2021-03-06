---
title: Konfigurace rozšíření Azure MFA NPS – Azure Active Directory
description: Po instalaci rozšíření serveru NPS použijte tyto kroky pro pokročilou konfiguraci, jako je přidávání do seznamu povolených IP adres a nahrazení UPN.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34d92af88106151e7efba679c53c5b5bd1c07dcd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80653791"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Rozšířené možnosti konfigurace rozšíření NPS pro službu Multi-Factor Authentication

Rozšíření serveru NPS (Network Policy Server) rozšiřuje vaše cloudové funkce Azure Multi-Factor Authentication do vaší místní infrastruktury. V tomto článku se předpokládá, že už máte nainstalované rozšíření, a teď chcete zjistit, jak se rozšíření přizpůsobí vašim potřebám. 

## <a name="alternate-login-id"></a>Alternativní ID přihlášení

Vzhledem k tomu, že se rozšíření serveru NPS připojuje k místním i cloudovým adresářům, může dojít k potížím, když místní hlavní názvy uživatelů (UPN) se neshodují s názvy v cloudu. Pokud chcete tento problém vyřešit, použijte alternativní přihlašovací ID. 

V rámci rozšíření serveru NPS můžete určit atribut služby Active Directory, který bude použit místo hlavního názvu uživatele (UPN) pro Azure Multi-Factor Authentication. Díky tomu můžete chránit místní prostředky pomocí dvoustupňového ověřování bez nutnosti úprav místních názvů UPN. 

Pokud chcete nakonfigurovat alternativní přihlašovací ID, použijte `HKLM\SOFTWARE\Microsoft\AzureMfa` a upravte následující hodnoty registru:

| Name | Typ | Výchozí hodnota | Description |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | řetězec | Obsahovat | Místo hlavního názvu uživatele (UPN) určete název atributu služby Active Directory, který chcete použít. Tento atribut se používá jako atribut AlternateLoginId. Pokud je tato hodnota registru nastavena na [platný atribut služby Active Directory](https://msdn.microsoft.com/library/ms675090.aspx) (například mail nebo DisplayName), pak se hodnota atributu používá místo hlavního názvu uživatele (UPN) pro ověřování. Pokud je tato hodnota registru prázdná nebo není nakonfigurovaná, je AlternateLoginId zakázaná a k ověřování se používá hlavní název uživatele (UPN). |
| LDAP_FORCE_GLOBAL_CATALOG | Boolean | False | Pomocí tohoto příznaku vynutíte při vyhledávání AlternateLoginId použití globálního katalogu pro hledání LDAP. Nakonfigurujte řadič domény jako globální katalog, přidejte do globálního katalogu atribut AlternateLoginId a pak tento příznak povolte. <br><br> Pokud je nakonfigurováno LDAP_LOOKUP_FORESTS (není prázdné), **bude tento příznak vynutil jako true**bez ohledu na hodnotu nastavení registru. V takovém případě rozšíření serveru NPS vyžaduje, aby byl globální katalog nakonfigurovaný s atributem AlternateLoginId pro jednotlivé doménové struktury. |
| LDAP_LOOKUP_FORESTS | řetězec | Obsahovat | Zadejte středníkem oddělený seznam doménových struktur, které se mají hledat. Například *contoso. com; panel. com*. Pokud je tato hodnota registru nakonfigurovaná, rozšíření serveru NPS provede iterativní hledání všech doménových struktur v pořadí, ve kterém byly uvedené, a vrátí první úspěšnou hodnotu AlternateLoginId. Pokud tato hodnota registru není nakonfigurovaná, vyhledávání AlternateLoginId je omezené na aktuální doménu.|

Pokud chcete řešit problémy s alternativními přihlašovacími ID, použijte doporučené kroky pro [alternativní chyby ID přihlášení](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>Výjimky protokolu IP

Pokud potřebujete monitorovat dostupnost serveru, třeba když nástroje pro vyrovnávání zatížení ověřují, které servery jsou spuštěné před odesláním úloh, nechcete, aby tyto kontroly byly zablokované požadavky na ověření. Místo toho vytvořte seznam IP adres, které víte, že účty služeb používají, a zakažte pro tento seznam požadavky na Multi-Factor Authentication.

Pokud chcete nakonfigurovat seznam povolených IP adres, použijte `HKLM\SOFTWARE\Microsoft\AzureMfa` a nakonfigurujte tuto hodnotu registru:

| Name | Typ | Výchozí hodnota | Description |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | řetězec | Obsahovat | Zadejte středníkem oddělený seznam IP adres. Zahrňte IP adresy počítačů, ve kterých pochází žádosti o služby, jako je třeba server NAS/VPN. Rozsahy IP adres a podsítě se nepodporují. <br><br> Například *10.0.0.1; 10.0.0.2; 10.0.0.3*.

> [!NOTE]
> Tento klíč registru není ve výchozím nastavení vytvořen instalačním programem a při restartování služby se v protokolu AuthZOptCh zobrazí chyba. Tato chyba v protokolu se může ignorovat, ale pokud se tento klíč registru vytvoří a zůstane prázdný, pokud není potřeba, chybová zpráva se nevrátí.

Pokud se žádost dostane z IP adresy, která existuje v rámci `IP_WHITELIST` , bude dvoustupňové ověřování vynecháno. Seznam IP adres se porovnává s IP adresou, která je k dispozici v atributu *ratNASIPAddress* žádosti RADIUS. Pokud žádost RADIUS přichází bez atributu ratNASIPAddress, zapíše se následující upozornění: "P_WHITE_LIST_WARNING:: IP seznam povolených adres se ignoruje, protože v žádosti RADIUS v atributu NasIpAddress chybí zdrojová IP adresa."

## <a name="next-steps"></a>Další kroky

[řešení chybových zpráv z rozšíření NPS pro Azure Multi-Factor Authentication](howto-mfa-nps-extension-errors.md)
