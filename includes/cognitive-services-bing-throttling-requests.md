---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: 1d8340054ace25a0cdc36eef9c3d5b4238a6f99b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67175291"
---
Služba a typ vašeho předplatného určuje počet dotazů, které můžete provést za sekundu (QPS). Ujistěte se, že vaše aplikace obsahuje logiku potřebnou k nepřekročení vaší kvóty. Při dosažení nebo překročení limitu QPS požadavek selže a vrátí se stavový kód HTTP 429. Odpověď zahrnuje hlavičku `Retry-After`, která označuje, jak dlouho je nutné počkat před odesláním dalšího požadavku.

## <a name="denial-of-service-versus-throttling"></a>Odepření služby a omezování

Služba rozlišuje mezi útokem s cílem odepření služby (DoS) a porušením QPS. Pokud služba má podezření na útok DoS, požadavek bude úspěšný (stavový kód HTTP je 200 OK). Text odpovědi však bude prázdný.