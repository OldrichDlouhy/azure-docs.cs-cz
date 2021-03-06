---
title: Použití modulu pro model image skóre
titleSuffix: Azure Machine Learning
description: Naučte se používat modul pro model image skóre v Azure Machine Learning k vygenerování předpovědi pomocí modelu vyškolených imagí.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: b949603b3e6ee51311f9c54f3e1326217f00c82d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87039106"
---
# <a name="score-image-model"></a>Model určení skóre obrázků

Tento článek popisuje modul v Návrháři Azure Machine Learning (Preview).

Pomocí tohoto modulu můžete vygenerovat předpovědi s využitím modelu vyškolených imagí pro data vstupních imagí.

## <a name="how-to-configure-score-image-model"></a>Jak nakonfigurovat model obrázku skóre

1. Přidejte do svého kanálu modul pro **model obrazu skóre** .

2. Připojte model trained image a datovou sadu obsahující data vstupních imagí. 

    Data by měla být typu ImageDirectory. Další informace o tom, jak získat adresář imagí, najdete v tématu [převod na modul adresáře imagí](convert-to-image-directory.md) . Schéma vstupní datové sady by mělo také obecně odpovídat schématu dat použitých pro výuku modelu.

3. Odešlete kanál.

## <a name="results"></a>Výsledky

Po vygenerování sady výsledků pomocí [modelu obrázku skóre](score-image-model.md)pro vygenerování sady metrik používané pro vyhodnocení přesnosti modelu (výkon) můžete připojit tento modul a datovou sadu s skóre k [vyhodnocení modelu](evaluate-model.md), 

### <a name="publish-scores-as-a-web-service"></a>Publikování skóre jako webové služby

Běžné použití bodování je vrácení výstupu v rámci prediktivní webové služby. Další informace najdete v [tomto kurzu](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) o nasazení koncového bodu v reálném čase na základě kanálu v Návrháři Azure Machine Learning.

## <a name="next-steps"></a>Další kroky

Podívejte se na [sadu modulů, které jsou k dispozici](module-reference.md) pro Azure Machine Learning. 
