---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "67175005"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problém: vlastní image; chyby zřizování
K chybám zřizování dojde, když nahrajete nebo zachytíte zobecněnou image virtuálního počítače jako specializovanou image virtuálního počítače nebo naopak. V bývalém důsledku dojde k chybě časového limitu zřizování a ta by způsobila chybu zřizování. Chcete-li nasadit vlastní image bez chyb, je nutné zajistit, aby se typ obrázku během procesu zachycení nezměnil.

V následující tabulce jsou uvedeny možné kombinace zobecněných a specializovaných imagí, typ chyby, který se zobrazí, a informace o tom, co je třeba udělat k opravě chyb.

