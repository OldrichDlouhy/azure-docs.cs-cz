---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 046bd8fcbb8fab50269c8d35da0956bdc63f2304
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298595"
---
Funkce umožňují používat klíče k tomu, aby během vývoje měly přístup k koncovým bodům funkce HTTP. Pokud není úroveň přístupu HTTP u funkce aktivované protokolem HTTP nastavená na `anonymous` , požadavky musí v žádosti zahrnovat přístupový klíč rozhraní API. 

Zatímco klíče poskytují výchozí mechanismus zabezpečení, možná budete chtít zvážit další možnosti zabezpečení koncového bodu HTTP v produkčním prostředí. Obvykle není dobrým zvykem distribuovat sdílený tajný klíč ve veřejných aplikacích. Pokud je vaše funkce volána z veřejného klienta, je vhodné zvážit implementaci jiného mechanismu Secrity. Další informace najdete v tématu [zabezpečení koncového bodu http v produkčním prostředí](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#secure-an-http-endpoint-in-production).

Při obnovování hodnot klíčů funkce je nutné ručně znovu distribuovat aktualizované hodnoty klíčů všem klientům, kteří volají vaši funkci.  

#### <a name="authorization-scopes-function-level"></a>Obory autorizace (na úrovni funkcí)

Existují dva obory přístupu pro klíče na úrovni funkcí:

* **Funkce**: tyto klíče se vztahují pouze na konkrétní funkce, ve kterých jsou definovány. Když se použije jako klíč rozhraní API, povolí přístup k této funkci.

* **Hostitel**: klíče s oborem hostitele lze použít pro přístup ke všem funkcím v aplikaci Function App. Při použití jako klíč rozhraní API umožňují tyto funkce přístup k libovolné funkci v aplikaci Function App. 

Každý klíč má název pro referenci a výchozí klíč (s názvem "výchozí") na úrovni funkce a hostitele. Klíče funkcí mají přednost před klíči hostitele. Pokud jsou definovány dva klíče se stejným názvem, je klíč funkce vždy použit.

#### <a name="master-key-admin-level"></a>Hlavní klíč (úroveň správce) 

Každá aplikace Function App má také klíč hostitele na úrovni správce s názvem `_master` . Kromě poskytování přístupu na úrovni hostitele ke všem funkcím v aplikaci poskytuje hlavní klíč také přístup správce k rozhraním REST API pro modul runtime. Tento klíč nelze odvolat. Když nastavíte úroveň přístupu `admin` , musí žádosti používat hlavní klíč. všechny ostatní klíče mají za následek selhání přístupu.

> [!CAUTION]  
> Vzhledem ke zvýšeným oprávněním v aplikaci Function App udělené hlavním klíčem byste tento klíč neměli sdílet s třetími stranami nebo ho distribuovat v nativních klientských aplikacích. Při volbě úrovně přístupu správce buďte opatrní.
