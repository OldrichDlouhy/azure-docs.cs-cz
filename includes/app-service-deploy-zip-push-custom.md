---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 0019e50615f3e66778709ad8cb28f92967c66e2e
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050234"
---
## <a name="deployment-customization"></a>Přizpůsobení nasazení

Proces nasazení předpokládá, že soubor. zip, který vložíte, obsahuje aplikaci připravenou k běhu. Ve výchozím nastavení nejsou spuštěny žádné vlastní nastavení. Chcete-li povolit stejné procesy sestavení, které získáte s průběžnou integrací, přidejte do nastavení aplikace následující:

`SCM_DO_BUILD_DURING_DEPLOYMENT=true`

Při použití nabízeného nasazení. zip je toto nastavení ve výchozím nastavení **false** . Výchozí hodnota je **true** pro nasazení nepřetržité integrace. Při nastavení na **hodnotu true**se vaše nastavení související s nasazením použijí během nasazení. Tato nastavení můžete nakonfigurovat buď jako nastavení aplikace, nebo v konfiguračním souboru. nasazení, který se nachází v kořenovém adresáři souboru. zip. Další informace najdete v části [nastavení týkající se úložiště a nasazení](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) v tématu Reference k nasazení.