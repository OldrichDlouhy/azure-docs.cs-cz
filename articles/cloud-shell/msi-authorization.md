---
title: Použití spravovaných identit pro prostředky v Azure Cloud Shell
description: Ověřování kódu pomocí MSI v Azure Cloud Shell
services: azure
author: maertendMSFT
ms.author: damaerte
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 04/14/2018
ms.openlocfilehash: a5d49a16324a5a97f4a0507f9abf47ea602ea072
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "72328712"
---
# <a name="use-managed-identities-for-azure-resources-in-azure-cloud-shell"></a>Použití spravovaných identit pro prostředky Azure v Azure Cloud Shell

Azure Cloud Shell podporuje autorizaci se spravovanými identitami pro prostředky Azure. Využijte tento postup k získání přístupových tokenů pro zabezpečenou komunikaci se službami Azure.

## <a name="about-managed-identities-for-azure-resources"></a>Informace o spravovaných identitách pro prostředky Azure
Běžným problémem při sestavování cloudových aplikací je způsob, jak bezpečně spravovat přihlašovací údaje, které se musí nacházet v kódu pro ověřování ve službě Cloud Services. V Cloud Shell možná budete muset ověřit načtení Key Vault pro přihlašovací údaje, které může skript potřebovat.

Spravované identity prostředků Azure usnadňují řešení tohoto problému tím, že poskytuje službám Azure automaticky spravovanou identitu ve službě Azure Active Directory (Azure AD). Tuto identitu můžete použít k ověření pro jakoukoli službu, která podporuje ověřování Azure AD, včetně služby Key Vault, aniž byste ve vašem kódu museli mít přihlašovací údaje.

## <a name="acquire-access-token-in-cloud-shell"></a>Získání přístupového tokenu v Cloud Shell

Spusťte následující příkazy pro nastavení přístupového tokenu MSI jako proměnné prostředí `access_token` .
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Doba platnosti tokenu zpracování

Místní subsystém MSI ukládá tokeny do mezipaměti. Proto ji můžete zavolat tak často, jak budete chtít, a volání služby Azure AD za běhu pouze v případě, že:
- k mezipaměti nedochází v důsledku neúspěšného tokenu v mezipaměti.
- platnost tokenu vypršela.

Pokud token ukládáte do mezipaměti v kódu, měli byste se připravit na zpracování scénářů, kde prostředek indikuje, že platnost tokenu vypršela.

Chcete-li zpracovat chyby tokenu, navštivte [stránku MSI s přístupovými tokeny MSI pro oblé](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling)služby.

## <a name="next-steps"></a>Další kroky
[Další informace o MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[Získání přístupových tokenů z virtuálních počítačů MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
