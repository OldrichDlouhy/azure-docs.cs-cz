---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "67174702"
---
### <a name="access-tokens"></a>Přístupové tokeny

Přístupové tokeny představují robustnější metodu ověřování pomocí prostorových kotev Azure. Obzvláště při přípravě aplikace pro produkční nasazení. Souhrn tohoto přístupu je nastavit back-end službu, se kterou může klientská aplikace bezpečně ověřit. Rozhraní back-end služby s AAD za běhu a službu tokenů prostorových kotev Azure pro vyžádání přístupového tokenu. Tento token se pak doručí do klientské aplikace a použije se v sadě SDK k ověřování pomocí prostorových kotev Azure.
