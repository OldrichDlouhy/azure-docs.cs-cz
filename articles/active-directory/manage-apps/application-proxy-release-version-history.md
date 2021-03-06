---
title: 'Azure Proxy aplikací služby AD: Historie vydání verze | Microsoft Docs'
description: V tomto článku jsou uvedené všechny verze Azure Proxy aplikací služby AD a popisuje nové funkce a opravené problémy.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/22/2020
ms.subservice: app-mgmt
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042509240eb2b88446d3ac1956d9056d5c39dfc8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019382"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure Proxy aplikací služby AD: Historie verzí
V tomto článku jsou uvedeny verze a funkce služby Azure Active Directory (Azure AD) proxy aplikací, které byly vydány. Tým Azure AD pravidelně aktualizuje proxy aplikace s novými funkcemi a funkcemi. Konektory proxy aplikací se aktualizují automaticky, když se uvolní nová verze. 

Doporučujeme, abyste se ujistili, že pro vaše konektory jsou povolené automatické aktualizace, abyste měli jistotu, že máte nejnovější funkce a opravy chyb. Společnost Microsoft poskytuje přímou podporu pro poslední verzi konektoru a jednu verzi.

Tady je seznam souvisejících prostředků:

Prostředek |  Podrobnosti
--------- | --------- |
Jak povolit proxy aplikace | V tomto [kurzu](application-proxy-add-on-premises-application.md)jsou popsány předpoklady pro povolení proxy aplikací a instalace a registrace konektoru.
Vysvětlení konektorů Azure Proxy aplikací služby AD | Přečtěte si další informace o [správě konektorů](application-proxy-connectors.md) a o tom, jak konektory [automaticky upgradují](application-proxy-connectors.md#automatic-updates).
Stažení konektoru služby Azure Proxy aplikací služby AD |  [Stáhněte si nejnovější konektor](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download).

## <a name="1519750"></a>1.5.1975.0

### <a name="release-status"></a>Stav verze

22. července 2020: vydáno ke stažení Tato verze je k dispozici pouze pro instalaci prostřednictvím stránky pro stažení. Vydání této verze se automaticky aktualizuje na pozdější dobu.

### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení
-   Vylepšená podpora pro Azure Government cloudová prostředí. Postup, jak správně nainstalovat konektor pro Azure Government Cloud, najdete v části [požadavky](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-government-cloud#allow-access-to-urls) a [kroky instalace](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-government-cloud#install-the-agent-for-the-azure-government-cloud).
- Podpora používání webového klienta služby Vzdálená plocha s proxy aplikací. Další podrobnosti najdete v tématu věnovaném [publikování vzdálené plochy pomocí Azure proxy aplikací služby AD](application-proxy-integrate-with-remote-desktop-services.md) .
- Vylepšená vyjednávání rozšíření protokolu WebSocket. 

### <a name="fixed-issues"></a>Opravené problémy
- Opravili jsme problém protokolu WebSocket, který vynutil řetězce v malých písmenech.
- Opravili jsme problém, který způsobil, že konektory occassionally nereagující.

## <a name="1516260"></a>1.5.1626.0

### <a name="release-status"></a>Stav verze

17. července 2020: vydáno ke stažení. Tato verze je k dispozici pouze pro instalaci prostřednictvím stránky pro stažení. Vydání této verze se automaticky aktualizuje na pozdější dobu.

### <a name="fixed-issues"></a>Opravené problémy
- Problém vyřešené nevracení paměti v předchozí verzi
- Obecná vylepšení podpory protokolu WebSocket

## <a name="1515260"></a>1.5.1526.0

### <a name="release-status"></a>Stav verze

7. dubna 2020: vydáno ke stažení

### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení
-   Konektory používají pouze TLS 1,2 pro všechna připojení. Další podrobnosti najdete v tématu [požadavky konektoru](application-proxy-add-on-premises-application.md#before-you-begin) .
- Vylepšené signalizace mezi konektorem a službami Azure. To zahrnuje podporu spolehlivých relací pro komunikaci WCF mezi konektorem a službami Azure a vylepšení mezipaměti protokolu DNS pro komunikaci pomocí protokolu WebSocket.
- Podpora konfigurace proxy serveru mezi konektorem a aplikací back-end. Další informace najdete v tématu [práce se stávajícími místními proxy servery](application-proxy-configure-connectors-with-proxy-servers.md).

### <a name="fixed-issues"></a>Opravené problémy
- Odebral se návrat k portu 8080 pro komunikaci z konektoru se službami Azure.
- Přidání trasování ladění pro komunikaci protokolu WebSocket. 
- Bylo vyřešeno zachování atributu SameSite při nastavení v souborech cookie aplikace back-endu.

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Stav verze

20. září 2018: vydáno ke stažení

### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení

- Přidání podpory protokolu WebSocket pro aplikaci QlikSense Další informace o tom, jak integrovat QlikSense s proxy aplikací, najdete v tomto [návodu](application-proxy-qlik.md). 
- Vylepšený průvodce instalací, který usnadňuje konfiguraci odchozího proxy serveru. 
- Jako výchozí protokol pro konektory nastavte TLS 1,2. 
- Přidala se nová licenční smlouva s koncovým uživatelem (EULA).  

### <a name="fixed-issues"></a>Opravené problémy

- Opravili jsme chybu, která způsobila nevracení paměti v konektoru.
- Aktualizace verze Azure Service Bus, která zahrnuje opravu chyby pro problémy s časovým limitem konektoru.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Stav verze

19. ledna 2018: vydáno ke stažení

### <a name="fixed-issues"></a>Opravené problémy

- Přidání podpory pro vlastní domény, které potřebují překlad domény v souboru cookie.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Stav verze 

25. května 2017: vydáno ke stažení 

### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení 

Vylepšená kontrola nad omezeními odchozího připojení konektorů. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Stav verze

15. dubna 2017: vydáno ke stažení

### <a name="new-features-and-improvements"></a>Nové funkce a vylepšení

- Zjednodušená připojování a správa s menším počtem požadovaných portů. Proxy aplikace nyní vyžaduje otevření pouze dvou standardních odchozích portů: 443 a 80. Proxy aplikace i nadále používá pouze odchozí připojení, takže stále nepotřebujete žádné součásti v DMZ. Podrobnosti najdete v naší [dokumentaci ke konfiguraci](application-proxy-add-on-premises-application.md).  
- Pokud to vaše externí proxy server nebo brána firewall podporuje, můžete teď síť otevřít pomocí DNS místo rozsahu IP adres. Služba proxy aplikací vyžaduje připojení pouze k *. msappproxy.net a *. servicebus.windows.net.


## <a name="earlier-versions"></a>Starší verze

Pokud používáte aplikační konektor proxy verze starší než 1.5.36.0, aktualizujte na nejnovější verzi, abyste měli jistotu, že máte nejnovější plně podporované funkce.

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o [vzdáleném přístupu k místním aplikacím prostřednictvím Azure proxy aplikací služby AD](application-proxy.md).
- Pokud chcete začít používat proxy aplikace, přečtěte si téma [kurz: Přidání místní aplikace pro vzdálený přístup prostřednictvím proxy aplikací](application-proxy-add-on-premises-application.md).
