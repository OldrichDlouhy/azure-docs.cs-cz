---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "76545196"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Konfigurace relace prostorové kotvy cloudu

V dalším kroku se postará o konfiguraci relace prostorového kotva cloudu. Na prvním řádku nastavíme v relaci poskytovatele senzorů. Od této chvíle budou všechny kotvy, které během relace vytvoříte, přidruženy k sadě čtení snímačů. V dalším kroku vytvoříme pro každé zařízení kritéria hledání a inicializujete je, aby odpovídala požadavkům aplikace. Nakonec připravujeme relaci, aby používala data senzorů při hledání kotev tím, že vytvoří sledovací proces z našich kritérií téměř pro zařízení.