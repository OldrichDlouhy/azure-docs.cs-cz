---
title: Ověřování Windows a Azure MFA Server – Azure Active Directory
description: Nasazení ověření Windows a serveru Azure Multi-Factor Authentication.
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
ms.openlocfilehash: 4abfb970ca322724adb0f8919b7509bc8a641378
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80652802"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Ověření Windows a server Azure Multi-Factor Authentication

Část ověření Windows serveru Azure Multi-Factor Authentication umožňuje povolit a konfigurovat ověřování Windows pro aplikace. Před nastavením ověřování Windows mějte na paměti následující seznam:

* Po nastavení je nutné provést aktivaci restartováním Azure Multi-Factor Authentication pro Terminálové služby.
* Pokud je zaškrtnuté políčko vyžadovat shodu uživatele se službou Azure Multi-Factor Authentication a nejste v seznamu uživatelů, po restartování se nebudete moct přihlásit k počítači.
* Důvěryhodné IP adresy jsou závislé na tom, zda aplikace může zajistit IP adresu klienta s ověřením. Momentálně jsou podporovány pouze terminálové služby.  

> [!IMPORTANT]
> Od 1. července 2019 už Microsoft nenabídne MFA Server pro nová nasazení. Noví zákazníci, kteří chtějí vyžadovat službu Multi-Factor Authentication od uživatelů, by měli používat cloudové Multi-Factor Authentication Azure. Stávající zákazníci, kteří mají aktivovaný MFA Server před 1. července, budou moci stáhnout nejnovější verzi, budoucí aktualizace a generovat přihlašovací údaje pro aktivaci obvyklým způsobem.

> [!NOTE]
> Tato funkce není podporována pro zabezpečené terminálové služby v systému Windows Server 2012 R2.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Chcete-li zabezpečit aplikaci pomocí ověřování systému Windows, použijte následující postup.

1. Na serveru Azure Multi-Factor Authentication klikněte na ikonu ověřování systému Windows.
   ![Ověřování systému Windows na serveru MFA](./media/howto-mfaserver-windows/windowsauth.png)
2. Zaškrtněte políčko **Povolit ověřování systému Windows**. Ve výchozím nastavení je toto políčko zaškrtnuté.
3. Karta aplikace umožňuje správci konfigurovat jednu nebo více aplikací pro ověřování systému Windows.
4. Vyberte server nebo aplikaci – určete, zda je povolen server/aplikace. Klikněte na **OK**.
5. Klikněte na tlačítko **Přidat...**
6. Karta Důvěryhodné IP umožňuje přeskočit ověřování Azure Multi-Factor Authentication pro relace systému Windows pocházející z konkrétních IP adres. Například pokud zaměstnanci používají aplikace z kanceláře a z domova, můžete rozhodnout, že nechcete, aby jejich telefony vyzváněly pro ověřování Azure Multi-Factor Authentication, zatímco jsou v kanceláři. V takovém případě zadáte podsíť kanceláře jako položku důvěryhodných IP adres.
7. Klikněte na tlačítko **Přidat...**
8. Chcete-li přeskočit jednu IP adresu, vyberte možnost **jediná IP** adresa.
9. Vyberte **Rozsah IP adres**, pokud chcete přeskočit celý rozsah IP adres. Příklad 10.63.193.1–10.63.193.100.
10. Pokud chcete zadat rozsahu IP adres pomocí zápisu podsítě, vyberte **Podsíť**. Zadejte počáteční IP adresu podsítě a vyberte příslušnou síťovou masku z rozevíracího seznamu.
11. Klikněte na **OK**.

## <a name="next-steps"></a>Další kroky

- [Konfigurace zařízení sítě VPN třetích stran pro Azure MFA Server](howto-mfaserver-nps-vpn.md)

- [Posílení vaší stávající infrastruktury pro ověřování pomocí rozšíření NPS pro Azure MFA](howto-mfa-nps-extension.md)
