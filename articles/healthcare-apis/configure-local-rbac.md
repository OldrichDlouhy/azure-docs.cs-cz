---
title: Konfigurace místního Access Control na základě rolí (RBAC) pro Azure API pro FHIR
description: Tento článek popisuje, jak nakonfigurovat rozhraní API Azure pro FHIR, aby používalo externí tenanta Azure AD pro rovinu dat.
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: mihansen
ms.openlocfilehash: a8c1b36d6a439297dfb0bbcb34efe059349fc5a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871907"
---
# <a name="configure-local-rbac-for-fhir"></a>Konfigurace místní RBAC pro FHIR 

Tento článek vysvětluje, jak nakonfigurovat rozhraní API Azure pro FHIR, aby používalo externího sekundárního tenanta Azure Active Directory pro správu přístupu k rovině dat. Tento režim použijte pouze v případě, že nemůžete použít klienta Azure Active Directory přidruženého k vašemu předplatnému.

> [!NOTE]
> Pokud je vaše datová plocha služby FHIR nakonfigurovaná tak, aby používala vašeho primárního klienta Azure Active Directory přidruženého k vašemu předplatnému, [přiřaďte role roviny dat pomocí Azure RBAC](configure-azure-rbac.md).

## <a name="add-service-principal"></a>Přidání instančního objektu

Místní RBAC umožňuje použití externího tenanta Azure Active Directory se serverem FHIR. Aby systém RBAC mohl v tomto tenantovi kontrolovat členství ve skupinách, musí mít rozhraní API Azure pro FHIR v tenantovi instanční objekt. Tento instanční objekt se vytvoří automaticky v klientech vázaných na předplatná, která nasadila rozhraní Azure API pro FHIR, ale v případě, že se k tomuto tenantovi neváže žádné předplatné, správce klienta bude muset tento instanční objekt vytvořit pomocí jednoho z následujících příkazů:

Pomocí `Az` modulu PowerShellu:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

nebo můžete použít `AzureAd` modul prostředí PowerShell:

```azurepowershell-interactive
New-AzureADServicePrincipal -AppId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

nebo můžete použít rozhraní příkazového řádku Azure:

```azurecli-interactive
az ad sp create --id 3274406e-4e0a-4852-ba4f-d7226630abb7
```

## <a name="configure-local-rbac"></a>Konfigurace místní RBAC

Rozhraní Azure API pro FHIR můžete nakonfigurovat tak, aby používalo externího nebo sekundárního tenanta Azure Active Directory v okně **ověřování** :

![Místní přiřazení RBAC](media/rbac/local-rbac-guids.png).

Do pole autorita zadejte platného klienta Azure Active Directory. Po ověření tenanta by se mělo aktivovat pole **povolené ID objektu** a můžete zadat seznam ID objektů identity. Těmito identifikátory můžou být ID objektů identity:

* Uživatel Azure Active Directory.
* Objekt Azure Active Directory služby.
* Skupina zabezpečení Azure Active Directory.

Další podrobnosti najdete v článku o tom, jak [najít ID objektů identity](find-identity-object-ids.md) .

Po zadání požadovaných ID objektů klikněte na **Uložit** a počkejte, než se změny uloží, než se pokusíte o přístup k rovině dat pomocí přiřazených uživatelů, objektů služby nebo skupin.

## <a name="caching-behavior"></a>Chování při ukládání do mezipaměti

Rozhraní Azure API pro FHIR zapíše do mezipaměti rozhodnutí po dobu až 5 minut. Pokud udělíte uživateli přístup k FHIR serveru tím, že je přidáte do seznamu povolených ID objektů nebo je odeberete ze seznamu, měli byste očekávat, že může trvat až pět minut, než se změny v oprávněních šíří.

## <a name="next-steps"></a>Další kroky

V tomto článku jste zjistili, jak přiřadit přístup k rovině FHIR dat pomocí externího (sekundárního) Azure Active Directory tenanta. Další informace o dalších nastaveních pro Azure API pro FHIR:
 
>[!div class="nextstepaction"]
>[Další nastavení rozhraní Azure API pro FHIR](azure-api-for-fhir-additional-settings.md)

