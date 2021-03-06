---
title: Nastavení samoobslužné správy skupin – Azure Active Directory | Microsoft Docs
description: Vytváření a správa skupin zabezpečení nebo skupin Office 365 ve službě Azure Active Directory a žádosti o členství ve skupině zabezpečení nebo skupině Office 365
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 03/10/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ce5d96d3ca65efb69bf322cf4a5f5563b83d8ce
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84727870"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Nastavení samoobslužné správy skupin v Azure Active Directory 

Uživatelům můžete povolit vytváření a správu vlastních skupin zabezpečení nebo skupin Office 365 ve službě Azure Active Directory (Azure AD). Vlastník skupiny může schvalovat nebo zamítnout žádosti o členství a může delegovat řízení členství ve skupině. Funkce Samoobslužné správy skupin nejsou k dispozici pro skupiny zabezpečení s povoleným e-mailem ani pro distribuční seznamy.

## <a name="self-service-group-membership-defaults"></a>Výchozí nastavení členství ve skupině samoobslužných služeb

Když se v Azure Portal nebo pomocí Azure AD PowerShellu vytvoří skupiny zabezpečení, můžou členství aktualizovat jenom vlastníci skupiny. Skupiny zabezpečení vytvořené samoobslužnými službami na [přístupovém panelu](https://account.activedirectory.windowsazure.com/r#/joinGroups) a všechny skupiny Office 365 jsou k dispozici pro připojení pro všechny uživatele, ať už bylo schváleno nebo automaticky schváleno. Na přístupovém panelu můžete změnit možnosti členství při vytváření skupiny.

Skupiny vytvořené v | Výchozí chování skupiny zabezpečení | Výchozí chování skupiny Office 365
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Členy můžou přidávat jenom vlastníci.<br>Viditelná, ale není dostupná pro připojení na přístupovém panelu | Otevřít pro připojení pro všechny uživatele
[Azure Portal](https://portal.azure.com) | Členy můžou přidávat jenom vlastníci.<br>Viditelná, ale není dostupná pro připojení na přístupovém panelu<br>Vlastník není automaticky přiřazen při vytváření skupiny. | Otevřít pro připojení pro všechny uživatele
[Přístupový panel](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Otevřít pro připojení pro všechny uživatele<br>Možnosti členství lze změnit při vytvoření skupiny. | Otevřít pro připojení pro všechny uživatele<br>Možnosti členství lze změnit při vytvoření skupiny.

## <a name="self-service-group-management-scenarios"></a>Scénáře správy skupin samoobslužných služeb

* **Delegovaná správa skupin** – příkladem je správce, který spravuje přístup k aplikaci SaaS, kterou používá jeho společnost. Správa těchto přístupových práv je čím dál náročnější, takže správce požádá majitele firmy, aby vytvořil novou skupinu. Správce přiřadí aplikaci k nové skupině a přidá je do skupiny všichni lidé, kteří už přistupují k aplikaci. Majitel firmy potom může přidat další uživatele a tito uživatelé budou automaticky přiřazeni k aplikaci. Kvůli správě přístupu pro uživatele tak majitel firmy nemusí čekat na správce. Pokud správce udělí stejné oprávnění správci v jiné obchodní skupině, pak může tato osoba také spravovat přístup vlastních členů skupiny. Obchodní vlastník ani správce nemůže zobrazit ani spravovat členství ve skupině ostatních. Správce může stále vidět všechny uživatele, kteří mají přístup k aplikaci, a v případě potřeby je může zablokovat.
* **Samoobslužná správa skupin** – příkladem tohoto scénáře jsou dva uživatelé, kteří mají nezávisle nastavené weby SharePoint Online. Chtějí svým týmům udělit přístup ke svým webům. Aby toho dosáhli, můžou vytvořit jednu skupinu v Azure AD a potom v SharePointu Online každý z nich vybere tu skupinu, které chce poskytnout přístup na svůj web. Pokud chce uživatel získat přístup, požádá o něj na přístupovém panelu a po schválení získá přístup k oběma webům SharePoint Online automaticky. Později se jeden z nich rozhodne, že všichni uživatelé s přístupem k webu by měli získat přístup i k určité aplikaci SaaS. Správce aplikace SaaS může přidat přístupová práva k aplikacím na web SharePoint Online. Od toho okamžiku získají veškeré jím schválené žádosti přístup na oba weby SharePoint Online a také k této aplikaci SaaS.

## <a name="make-a-group-available-for-user-self-service"></a>Zpřístupnění skupiny pro uživatelskou samoobsluhu

1. Přihlaste se k [centru pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který má k adresáři oprávnění globálního správce.
1. Vyberte **skupiny**a pak vyberte **Obecné** nastavení.
1. Možnost nastavit **vlastníky mohou spravovat požadavky na členství ve skupinách na přístupovém panelu** na **Ano**.
1. Nastavte **omezení přístupu ke skupinám na přístupovém panelu** na **ne**.
1. Pokud nastavíte **možnost Uživatelé můžou vytvářet skupiny zabezpečení na portálech Azure** nebo **Uživatelé můžou vytvářet skupiny Office 365 na portálech Azure** na

    - **Ano**: všichni uživatelé v organizaci Azure AD můžou vytvářet nové skupiny zabezpečení a přidávat do těchto skupin členy. Tyto nové skupiny se zobrazí také všem ostatním uživatelům na přístupovém panelu. Pokud to nastavení zásad skupiny umožňuje, můžou jiní uživatelé vytvářet žádosti o připojení k těmto skupinám.
    - **Ne**: uživatelé nemůžou vytvářet skupiny a nemůžou měnit existující skupiny, pro které jsou vlastníci. Můžou však stále spravovat členství v těchto skupinách a schvalovat žádosti o členství od jiných uživatelů.

Můžete také použít **vlastníky, kteří můžou přiřadit členy jako vlastníky skupin na portálech Azure** a **vlastníky, kteří můžou členy přiřadit jako vlastníci skupin na portálech Azure** a dosáhnout tak podrobnějšího řízení přístupu pro uživatele samoobslužné správy skupin.

Když uživatelé můžou vytvářet skupiny, všichni uživatelé ve vaší organizaci můžou vytvářet nové skupiny a potom můžou jako výchozí vlastník přidávat členy do těchto skupin. Nemůžete určit jednotlivce, kteří můžou vytvářet své vlastní skupiny. Můžete určit jednotlivce jenom pro případ, že bude mít jiný člen skupiny jako vlastník skupiny.

> [!NOTE]
> Pro uživatele, kteří žádají o připojení ke skupině zabezpečení nebo skupině Office 365, se vyžaduje licence Azure Active Directory Premium (P1 nebo P2) a vlastníkům schvalovat nebo odmítat žádosti o členství. Bez licence Azure Active Directory Premium můžou uživatelé dál spravovat své skupiny na přístupovém panelu, ale nemůžou vytvořit skupinu, která vyžaduje schválení vlastníka na přístupovém panelu, a nemůže požádat o připojení ke skupině. 

## <a name="next-steps"></a>Další kroky

Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Rutiny Azure Active Directory pro konfiguraci nastavení skupiny](groups-settings-cmdlets.md)
* [Správa aplikací ve službě Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Představení služby Azure Active Directory](../fundamentals/active-directory-whatis.md)
* [Integrace místních identit do služby Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
