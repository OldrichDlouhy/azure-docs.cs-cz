---
title: Přidání uživatelů do dynamické skupiny – kurz – Azure AD | Microsoft Docs
description: V tomto kurzu použijete skupiny s pravidly členství uživatelů k automatickému přidávání nebo odebírání uživatelů.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: tutorial
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ee5fa52f59ea2ef3332fe66c81c24ff44c64e81
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582875"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Kurz: Automatické přidávání nebo odebírání členů skupin

V Azure Active Directory (Azure AD) můžete automaticky přidávat uživatele do skupin zabezpečení nebo skupin Office 365 nebo je z nich odebírat, abyste to pokaždé nemuseli dělat ručně. Při každé změně vlastností uživatele nebo zařízení vyhodnotí Azure AD všechna dynamická pravidla skupin ve vaší organizaci Azure AD, aby viděli, jestli by tato změna měla přidat nebo odebrat členy.

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Vytvoření automaticky vyplněné skupiny uživatelů typu Host od partnerské společnosti
> * Přiřazení licencí ke skupině pro funkce specifické pro partnera, ke kterým budou mít uživatelé typu host přístup
> * Bonus: Zabezpečení skupiny **Všichni uživatelé** odebráním uživatelů typu host, abyste členům mohli například udělit přístup k výhradně interním webům

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Tato funkce vyžaduje jednu licenci Azure AD Premium pro vás jako globální správce organizace. Pokud ho nemáte, v Azure AD vyberte **licence** > **produkty** > **vyzkoušet/koupit**.

Uživatelům, kteří mají být členy dynamických skupin, licence přiřazovat nemusíte. K pokrytí všech takových uživatelů potřebujete jenom minimální počet dostupných licencí Azure AD Premium P1 v organizaci. 

## <a name="create-a-group-of-guest-users"></a>Vytvoření skupiny uživatelů typu host

Nejprve vytvoříte skupinu pro uživatele typu host, kteří jsou všichni z jedné partnerské společnosti. Potřebují zvláštní licencování, takže je efektivnější pro tento účel vytvořit skupinu.

1. Přihlaste se k Azure Portalhttps://portal.azure.com) (pomocí účtu, který je globálním správcem vaší organizace.
2. Vyberte **Azure Active Directory** > **skupiny** > **Nová skupina**.
   ![výběrem příkazu zahájíte novou skupinu.](./media/groups-dynamic-tutorial/new-group.png)
3. V okně **Skupina**:
  
   * Jako typ skupiny vyberte **zabezpečení** .
   * Jako `Guest users Contoso` název a popis skupiny zadejte.
   * Změňte **typ členství** na **dynamický uživatel**.
   
4. Vyberte **vlastníci** a v okně **Přidat vlastníky** vyhledejte požadované vlastníky. Klikněte na požadované vlastníky a přidejte je do výběru.
5. Kliknutím na **Vybrat** zavřete okno **Přidat vlastníky** .  
6. V poli **dynamické uživatelské členy** vyberte **Upravit dynamický dotaz** .
7. V okně**Pravidla dynamického členství**:

   * V poli **vlastnost** klikněte na existující hodnotu a vyberte možnost **userType**. 
   * Ověřte, zda je pole **operátoru** **stejné** jako vybrané.  
   * Vyberte pole **hodnota** a zadejte **Host**. 
   * Kliknutím na hypertextový odkaz **přidat výraz** přidáte další řádek.
   * V poli **a/nebo** vyberte **a**.
   * V poli **vlastnost** vyberte **CompanyName**.
   * Ověřte, zda je pole **operátoru** **stejné** jako vybrané.
   * Do pole **hodnota** zadejte **Contoso**.
   * Kliknutím na **Uložit** zavřete okno **pravidla dynamického členství** .
   
8. V okně **Skupina** výběrem možnosti **Vytvořit** vytvořte skupinu.

## <a name="assign-licenses"></a>Přiřazení licencí

Teď, když máte novou skupinu, můžete použít licence, které tito uživatelé z partnerské společnosti potřebují.

1. V Azure AD vyberte **Licence**, vyberte jednu nebo několik licencí a pak vyberte **Přiřadit**.
2. Vyberte **Uživatelé a skupiny**, vyberte skupinu **Uživatelé typu host – Contoso** a uložte provedené změny.
3. **Možnosti přiřazení** umožňují zapnout nebo vypnout plány služeb zahrnuté v licencích, které jste vybrali. Když provedete nějaké změny, nezapomeňte je uložit kliknutím na **OK**.
4. Přiřazení dokončíte tak, že v dolní části podokna **Přiřadit licenci** kliknete na **Přiřadit**.

## <a name="remove-guests-from-all-users-group"></a>Odebrání hostů ze skupiny Všichni uživatelé

Možná je vaším konečným správním plánem přiřadit všechny uživatele typu host k vlastním skupinám podle společnosti. Nyní můžete změnit skupinu **Všichni uživatelé** tak, aby byla vyhrazena pouze pro uživatele, kteří jsou ve vaší organizaci. Pak ji můžete použít k přiřazování aplikací a licencí specifických pro vaši domovskou organizaci.

   ![Změna skupiny Všichni uživatelé tak, aby byla pouze pro členy](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

**Odebrání skupiny uživatelů typu host**

1. Přihlaste se k [Azure Portal](https://portal.azure.com) pomocí účtu, který je globálním správcem vaší organizace.
2. Vyberte **Azure Active Directory** > **skupiny**. Vyberte skupinu **Uživatelé typu host – Contoso**, vyberte tři tečky (...) a pak vyberte **Odstranit**. Když odstraníte skupinu, odeberou se všechny přiřazené licence.

**Obnovení skupiny Všichni uživatelé**
1. Vyberte **Azure Active Directory** > **skupiny**. Výběrem názvu skupiny **Všichni uživatelé** otevřete skupinu.
1. Vyberte **Pravidla dynamického členství**, vymažte veškerý text pravidla a vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:
> [!div class="checklist"]
> * Vytvoření skupiny uživatelů typu host
> * Přiřazení licencí k nové skupině
> * Změna skupiny Všichni uživatelé tak, aby byla pouze pro členy

Přejděte k dalšímu článku, kde najdete další základní informace o licencování na základě skupin.
> [!div class="nextstepaction"]
> [Základy licencování na základě skupin](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



