---
title: Označení aplikace jako aplikace Publisher jako ověřené – Microsoft Identity Platform | Azure
description: Popisuje, jak označit aplikaci jako ověřený vydavatelem. Pokud je aplikace označena jako ověřená vydavatelem, znamená to, že vydavatel ověřil svoji identitu pomocí účtu Microsoft Partner Network, který dokončil proces ověření a přidružil tento účet MPN k registraci aplikace.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: d25a3831897ff8ad2e7dfb0c69910fee246aec21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85477681"
---
# <a name="mark-your-app-as-publisher-verified-preview"></a>Označení aplikace jako ověřené uživatelem (Preview)

Pokud je aplikace označena jako ověřená vydavatelem, znamená to, že vydavatel ověřil svoji identitu pomocí svého účtu Microsoft Partner Network (MPN) a přidružil tento účet MPN k registraci aplikace. Tento článek popisuje, jak dokončit proces [ověření vydavatele (Preview)](publisher-verification-overview.md) .

## <a name="quickstart"></a>Rychlé zprovoznění
Pokud už jste zaregistrovaní v Microsoft Partner Network (MPN) a splnili jste [požadavky](publisher-verification-overview.md#requirements), můžete začít hned: 

1. Přejděte na portál pro [registraci aplikace](https://aka.ms/PublisherVerificationPreview)ve verzi Preview.

1. Vyberte aplikaci a klikněte na **branding**. 

1. Kliknutím na **Přidat ID MPN ověřte vydavatele** a zkontrolujte uvedené požadavky.

1. Zadejte ID MPN a klikněte na **ověřit a uložit**.

Další podrobnosti o konkrétních výhodách, požadavcích a nejčastějších dotazech najdete v tématu [Přehled](publisher-verification-overview.md).


## <a name="mark-your-app-as-publisher-verified"></a>Označit aplikaci jako ověřenou vydavatelem
Ujistěte se, že jste splnili [požadavky](publisher-verification-overview.md#requirements), a pak postupujte podle těchto kroků a označte vaše aplikace jako vydavatel ověřen.  

1. Ujistěte se, že jste přihlášeni pomocí účtu organizace (Azure AD), který má oprávnění k provádění změn v aplikacích, které chcete označit jako vydavatel, a v účtu MPN v partnerském centru. 

    - V Azure AD musí být tento uživatel buď vlastníkem aplikace, nebo mít jednu z následujících rolí: správce aplikace, správce cloudové aplikace, globální správce. 

    - V partnerském centru musí mít tento uživatel tyto role: správce MPN, účty správce nebo globální správce (Toto je sdílená role spravovaná ve službě Azure AD). 

1. Přejděte na verzi Preview portálu pro registraci aplikací:  

1. Klikněte na aplikaci, kterou chcete označit jako vydavatel, a otevřete okno branding. 

1. Ujistěte se, že je správně nastavená doména vydavatele aplikace. Tato doména musí být: 

    - Dá se přidat do tenanta Azure AD jako vlastní doména ověřená DNS,  

    - Porovnává s doménou e-mailové adresy, která se používá během procesu ověřování pro váš účet MPN. 

1. Kliknutím na **Přidat ID MPN ověříte vydavatele** v dolní části stránky. 

1. Zadejte **ID MPN**. Toto ID MPN musí být pro: 

    - Platný účet Microsoft Partner Network, který dokončil proces ověření.  

    - Globální účet partnera (PGA) pro vaši organizaci. 

1. Klikněte na tlačítko **ověřit a uložit**. 

1. Počkejte na zpracování žádosti, což může trvat několik minut. 

1. Pokud bylo ověření úspěšné, okno ověření vydavatele se zavře a vrátí se do okna branding. Vedle ověřeného **zobrazovaného názvu vydavatele**se zobrazí modrý ověřený znak. 

1. Uživatelům, kteří se zobrazí výzva k vyjádření souhlasu s vaší aplikací, začne brzy zobrazovat výzvu, i když se tento proces úspěšně dokončí, i když to může trvat delší dobu, než se replika dokončí v celém systému. 

1. Otestujte tuto funkci tak, že se přihlásíte do vaší aplikace a ověříte, že se na obrazovce pro vyjádření souhlasu zobrazí ověřené označení. Pokud jste přihlášeni jako uživatel, který má aplikaci již udělený souhlas, můžete k vynucení výzvy k vyjádření souhlasu použít parametr *prompt = souhlasu* . 

1. Tento postup opakujte podle potřeby u všech dalších aplikací, pro které chcete zobrazit hvězdičku. Microsoft Graph můžete použít k rychlejšímu hromadnému provádění rutin PowerShellu, které budou brzy k dispozici. Další informace najdete v tématu [vytváření volání Microsoft API graphu](troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls) . 

A to je vše! Dejte nám vědět, pokud máte nějakou zpětnou vazbu k procesu, výsledkům nebo funkcím všeobecně. 

## <a name="next-steps"></a>Další kroky
Pokud narazíte na problémy, přečtěte si [informace o řešení potíží](troubleshoot-publisher-verification.md).