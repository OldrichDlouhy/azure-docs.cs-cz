---
title: Najít ID objektů identity pro ověřování – Azure API pro FHIR
description: Tento článek vysvětluje, jak najít ID objektů identity potřebné ke konfiguraci ověřování pro Azure API pro FHIR.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 8e1fdcbbdefb9c36d84b345d422d49f088077954
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86133488"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Vyhledat ID objektů identity pro konfiguraci ověřování

V tomto článku se naučíte, jak najít ID objektů identity potřebná při konfiguraci rozhraní API Azure pro FHIR, aby [používala externí nebo sekundární tenanta služby Active Directory](configure-local-rbac.md) pro rovinu dat.

## <a name="find-user-object-id"></a>Najít ID objektu uživatele

Pokud máte uživatele s uživatelským jménem `myuser@consoso.com` , můžete uživatele vyhledat `ObjectId` pomocí následujícího příkazu PowerShellu:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

nebo můžete použít rozhraní příkazového řádku Azure:

```azurecli-interactive
az ad user show --id myuser@consoso.com --query objectId --out tsv
```

## <a name="find-service-principal-object-id"></a>Najít ID instančního objektu služby

Předpokládejme, že jste zaregistrovali [klientskou aplikaci služby](register-service-azure-ad-client-app.md) a chcete, aby tento klient služby povolil přístup k rozhraní Azure API pro FHIR, a to pomocí následujícího příkazu prostředí PowerShell, kde můžete najít ID objektu pro instanční objekt služby.

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

kde `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` je ID klientské aplikace služby. Alternativně můžete použít `DisplayName` klienta služby:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Pokud používáte Azure CLI, můžete použít:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX --query objectId --out tsv
```

## <a name="find-a-security-group-object-id"></a>Najít ID objektu skupiny zabezpečení

Pokud chcete najít ID objektu skupiny zabezpečení, můžete použít následující příkaz prostředí PowerShell:

```azurepowershell-interactive
$(Get-AzureADGroup -Filter "DisplayName eq 'mygroup'").ObjectId
```
Kde `mygroup` je název skupiny, které vás zajímá.

Pokud používáte Azure CLI, můžete použít:

```azurecli-interactive
az ad group show --group "mygroup" --query objectId --out tsv
```

## <a name="next-steps"></a>Další kroky

V tomto článku jste se naučili, jak najít ID objektů identity, která jsou potřeba ke konfiguraci rozhraní API Azure pro FHIR k použití externího nebo sekundárního tenanta Azure Active Directory. Další informace o tom, jak používat ID objektů ke konfiguraci místních nastavení RBAC:
 
>[!div class="nextstepaction"]
>[Konfigurovat nastavení místních RBAC](configure-local-rbac.md)
