---
title: O konektorech rozhraní API v samoobslužných registračních tocích – Azure AD
description: Použijte konektory rozhraní API pro Azure Active Directory (Azure AD) k přizpůsobení a rozšíření uživatelských toků samoobslužné registrace pomocí webových rozhraní API.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c5e546c6eac77c4952a0d32d360f49d4251d49d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905173"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Použití konektorů rozhraní API k přizpůsobení a rozšiřování samoobslužné registrace 

## <a name="overview"></a>Přehled 
Jako vývojář nebo správce IT můžete použít konektory rozhraní API k integraci [uživatelských toků samoobslužné registrace](self-service-sign-up-overview.md) k externím systémům pomocí webových rozhraní API. Konektory rozhraní API můžete například použít k těmto akcím:

- Proveďte [**integraci s vlastním pracovním postupem schválení**](self-service-sign-up-add-approvals.md). Připojte se k vlastnímu systému schvalování pro správu vytváření účtů.
- [**Proveďte ověření identity**](code-samples-self-service-sign-up.md#identity-verification). Pomocí služby ověřování identity přidejte další úroveň zabezpečení pro rozhodování o vytváření účtů.
- **Ověřte vstupní data uživatele**. Ověřte proti poškozeným nebo neplatným uživatelským datům. Můžete například ověřit uživatelem zadaná data pro stávající data v externím úložišti dat nebo seznamu povolených hodnot. Pokud je neplatná, můžete požádat uživatele, aby poskytl platná data, nebo zablokovat, aby uživatel pokračoval v procesu registrace.
- **Přepsat atributy uživatele**. Přeformátujte nebo přiřaďte hodnotu atributu shromážděnému od uživatele. Pokud například uživatel zadá křestní jméno do všech malých a velkých písmen, můžete název naformátovat pouze na první písmeno. 
<!-- - **Enrich user data**. Integrate with your external cloud systems that store user information to integrate them with the sign-up flow. For example, your API can receive the user's email address, query a CRM system, and return the user's loyalty number. Returned claims can be used to pre-fill form fields or return additional data in the application token.  -->
- **Spusťte vlastní obchodní logiku**. Můžete aktivovat navazující události v cloudových systémech a odesílat nabízená oznámení, aktualizovat podnikové databáze, spravovat oprávnění, auditovat databáze a provádět další vlastní akce.

Konektor API představuje kontrakt mezi Azure AD a koncovým bodem rozhraní API tím, že definuje koncový bod HTTP, ověřování, požadavek a očekávanou odpověď. Po nakonfigurování konektoru API ho můžete povolit pro konkrétní krok v toku uživatele. Když uživatel dosáhne tohoto kroku v procesu registrace, vyvolá se konektor API a materializuje se jako požadavek HTTP POST a posílá vybrané deklarace identity jako páry klíč-hodnota v těle JSON. Reakce rozhraní API může ovlivnit spuštění toku uživatele. Odpověď rozhraní API může například zablokovat uživateli registraci, požádat uživatele o opětovné zadání informací, nebo přepsat a připojit atributy uživatele.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Kde můžete povolit konektor API v toku uživatele

Tok uživatele obsahuje dvě místa, kde můžete povolit konektor API:

- Po přihlášení pomocí zprostředkovatele identity
- Před vytvořením uživatele

V obou těchto případech jsou konektory rozhraní API vyvolány během registrace, nikoli při přihlášení.

### <a name="after-signing-in-with-an-identity-provider"></a>Po přihlášení pomocí zprostředkovatele identity

Konektor API v tomto kroku v procesu registrace se vyvolá hned po ověření uživatele u poskytovatele identity (Google, Facebook, Azure AD). Tento krok předchází ***stránku kolekce atributů***, což je formulář prezentovaný uživateli ke shromáždění atributů uživatele. Následují příklady scénářů konektoru rozhraní API, které můžete povolit v tomto kroku:

- Použijte e-mailovou nebo federované identitu, kterou uživatel zadal k vyhledání deklarací v existujícím systému. Vrátí tyto deklarace ze stávajícího systému, předem vyplní stránku kolekce atributů a zpřístupní je k vrácení v tokenu.
- Ověřte, zda je uživatel součástí seznamu povolených nebo odepřených dat, a určete, zda mohou pokračovat v procesu registrace.

### <a name="before-creating-the-user"></a>Před vytvořením uživatele

Konektor rozhraní API v tomto kroku v procesu registrace je vyvolán po stránce kolekce atributů, pokud je jeden zahrnutý. Tento krok se vždycky vyvolá před vytvořením uživatelského účtu ve službě Azure AD. Níže jsou uvedené příklady scénářů, které můžete v tomto okamžiku povolit během registrace:

- Ověřte vstupní data uživatele a požádejte uživatele, aby data znovu odeslal.
- Zablokuje registraci uživatele na základě dat zadaných uživatelem.
- Proveďte ověření identity.
- Dotaz na externí systémy pro stávající data o uživateli, který se vrátí v tokenu aplikace nebo ho uloží do Azure AD.

<!-- > [!IMPORTANT]
> If an invalid response is returned or another error occurs (for example, a network error), the user will be redirected to the app with the error re -->

## <a name="next-steps"></a>Další kroky
- Naučte se [Přidat konektor API do toku uživatele](self-service-sign-up-add-api-connector.md) .
- Přečtěte si, jak [Přidat vlastní schvalovací systém do samoobslužné registrace](self-service-sign-up-add-approvals.md) .