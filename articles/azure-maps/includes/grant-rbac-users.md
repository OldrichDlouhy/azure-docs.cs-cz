---
title: Udělení přístupu na základě role uživatelům
titleSuffix: Azure Maps
description: Udělení autorizace uživatelům pomocí řízení přístupu na základě role Azure Maps
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2b138dab2a97537a93b8d873f79b6ee9c00b4af4
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126737"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Udělení přístupu na základě role uživatelům Azure Maps

*Řízení přístupu na základě role* (RBAC) udělíte přiřazením skupiny nebo objektů zabezpečení Azure AD k jedné nebo více definicím role řízení přístupu Azure Maps. Chcete-li zobrazit definice rolí RBAC, které jsou k dispozici pro Azure Maps, přejděte na **řízení přístupu (IAM)**. Vyberte **role**a potom vyhledejte role, které začínají na *Azure Maps*.

* Pokud chcete efektivně spravovat velké množství přístupu uživatelů k Azure Maps, přečtěte si téma [skupiny Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).
* Aby se uživatelé mohli k aplikaci ověřit, musí se v Azure AD vytvořit. Viz téma [Přidání nebo odstranění uživatelů pomocí Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory).

Další informace najdete v tématu [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/) , abyste mohli efektivně spravovat adresář pro uživatele.

1. Přejít na **účet Azure Maps**. Vyberte přiřazení role **řízení přístupu (IAM)**  >  **Role assignment**.

    ![Udělit RBAC](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Na kartě **přiřazení rolí** v části **Role**vyberte předdefinované Azure Maps definice role, jako je **Azure Maps data Reader** nebo **Azure Maps Přispěvatel dat**. V části **přiřadit přístup k**vyberte možnost **uživatel, skupina nebo instanční objekt služby Azure AD**. Vyberte objekt zabezpečení podle názvu. Potom vyberte **Uložit**.

   * Podívejte se na podrobnosti o [Přidání nebo odebrání přiřazení rolí](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).

> [!WARNING]
> Azure Maps předdefinované definice rolí poskytují velmi velký přístup k mnoha Azure Mapsám rozhraní REST API. Chcete-li omezit rozhraní API pro uživatele na minimum, přečtěte si téma [Vytvoření vlastní definice role a přiřazení uživatelů](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) k definici vlastní role. Umožníte tak uživatelům, aby pro aplikaci měli k dispozici nejnižší oprávnění.