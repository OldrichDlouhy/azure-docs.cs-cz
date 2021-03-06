---
title: Nabídky spravovaných služeb na Azure Marketplace
description: Spravovaná služba umožňuje poskytovatelům služeb prodávat nabídky správy prostředků zákazníkům v Azure Marketplace.
ms.date: 05/04/2020
ms.topic: conceptual
ms.openlocfilehash: 119ecc8d15ef93a265cb5419404840496aaa1572
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121584"
---
# <a name="managed-service-offers-in-azure-marketplace"></a>Nabídky spravovaných služeb na Azure Marketplace

Tento článek popisuje typ nabídky **spravované služby** v [Azure Marketplace](https://azuremarketplace.microsoft.com). Nabídky spravované služby umožňují zákazníkům nabízet služby správy prostředků prostřednictvím [Azure Lighthouse](../overview.md). Tyto nabídky můžete zpřístupnit všem potenciálním zákazníkům nebo pouze jednomu nebo více konkrétním zákazníkům. Vzhledem k tomu, že se zákazníci fakturují přímo na náklady související s těmito spravovanými službami, neúčtují žádné poplatky za vás Microsoft.

## <a name="understand-managed-service-offers"></a>Vysvětlení nabídek spravovaných služeb

Spravovaná služba nabízí zjednodušení procesu připojování zákazníků do Azure Lighthouse. Když zákazník koupí nabídku v Azure Marketplace, bude moct určit, které předplatné nebo skupiny prostředků by se měly připojit.

Potom budou uživatelé ve vaší organizaci moci pracovat na těchto prostředcích v rámci tenanta vaší organizace prostřednictvím [správy delegovaných prostředků Azure](azure-delegated-resource-management.md)podle přístupu, který jste definovali při vytváření nabídky. To se provádí pomocí manifestu, který určuje Azure Active Directory (Azure AD) uživatelů, skupin a instančních objektů, které budou mít přístup k zákaznickým prostředkům, spolu s rolemi, které definují jejich úroveň přístupu. Když pro skupinu Azure AD přiřadíte oprávnění místo řady individuálních účtů uživatelů nebo aplikací, můžete přidat nebo odebrat jednotlivé uživatele, když se vaše požadavky na přístup změní.

## <a name="public-and-private-offers"></a>Veřejné a soukromé nabídky

Každá nabídka spravovaných služeb obsahuje jeden nebo více plánů. Plány můžou být buď soukromé, nebo veřejné.

Pokud chcete omezit nabídku na konkrétní zákazníky, můžete publikovat soukromý plán. V takovém případě lze plán zakoupit pouze pro konkrétní ID předplatného, které zadáte. Další informace najdete v tématu [soukromé nabídky](../../marketplace/private-offers.md).

Veřejné plány umožňují propagovat vaše služby na nové zákazníky. To je obvykle vhodnější, pokud požadujete pouze omezený přístup k tenantovi zákazníka. Po navázání vztahu se zákazníkem, pokud se rozhodne udělit vaší organizaci dodatečný přístup, můžete to udělat buď publikováním nového privátního plánu jenom pro tohoto zákazníka, nebo jeho [zprovozněním pro další přístup pomocí šablon Azure Resource Manager](../how-to/onboard-customer.md).

V případě potřeby můžete do stejné nabídky zahrnout jak veřejné, tak i privátní plány.

> [!IMPORTANT]
> Jakmile je plán publikovaný jako veřejný, nemůžete ho změnit na privátní. Chcete-li určit, kteří zákazníci mohou přijmout vaši nabídku a delegovat prostředky, použijte privátní plán. S veřejným plánem nemůžete omezit dostupnost na konkrétní zákazníky nebo dokonce na určitý počet zákazníků (i když se rozhodnete, že můžete plán úplně zastavit). [Přístup k delegování můžete odebrat](../how-to/remove-delegation.md) poté, co zákazník přijme nabídku, pouze pokud jste při publikování této nabídky zazahrnuli **autorizaci** s **definicí role** nastavenou na možnost [přiřazení registrace spravovaných služeb](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) . Můžete se také obrátit na zákazníka a požádat ho, aby [Váš přístup odebral](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Publikování nabídek spravovaných služeb

Informace o tom, jak publikovat nabídku spravovaných služeb, najdete v tématu [publikování nabídky spravovaných služeb na Azure Marketplace](../how-to/publish-managed-services-offers.md).

## <a name="next-steps"></a>Další kroky

- Přečtěte si o [správě prostředků delegovaných v Azure](azure-delegated-resource-management.md) a o [prostředích pro správu mezi klienty](cross-tenant-management-experience.md).
- [Publikujte nabídky spravovaných služeb](../how-to/publish-managed-services-offers.md) pro Azure Marketplace.
