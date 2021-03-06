---
title: Co je Custom Vision?
titleSuffix: Azure Cognitive Services
description: Zjistěte, jak pomocí služby Custom Vision vytvářet vlastní klasifikátory obrázků v cloudu Azure.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 79ecb801e1b4d0fa96ca7ae06223fc231cbf12e6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2020
ms.locfileid: "82129872"
---
# <a name="what-is-custom-vision"></a>Co je Custom Vision?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Custom Vision je služba Cognitive Services, která umožňuje vytvářet, nasazovat a vylepšovat vaše vlastní klasifikátory obrázků. Klasifikátor obrázku je služba AI, která v závislosti na jejich vizuálních vlastnostech používá popisky (které reprezentují _třídy_) k obrázkům. Na rozdíl od služby [Počítačové zpracování obrazu](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) Custom Vision umožňuje zadat popisky, které se mají použít.

## <a name="what-it-does"></a>Co dělá

Služba Custom Vision používá algoritmus strojového učení pro použití popisků na obrázky. Vy, vývojář, musíte odeslat skupiny imagí, které mají funkci, a chybějící příslušné charakteristiky. Obrázky můžete označovat sami v době odeslání. Pak algoritmus navede na tato data a vypočte svou vlastní přesnost tím, že se na stejných obrázcích otestuje sám. Jakmile je algoritmus vyučený, můžete ho otestovat, revlakovat a nakonec ho použít ke klasifikaci nových imagí podle potřeb vaší aplikace. Samotný model můžete také exportovat pro offline použití.

### <a name="classification-and-object-detection"></a>Klasifikace a detekce objektů

Funkce služby Custom Vision je možné rozdělit do dvou funkcí. **Klasifikace obrázku** používá jeden nebo více štítků na obrázek. **Detekce objektu** je podobná, ale také vrací souřadnice v obrázku, kde lze nalézt použité popisky.

### <a name="optimization"></a>Optimalizace

Služba Custom Vision je optimalizovaná tak, aby rychle rozpoznala hlavní rozdíly mezi obrázky, takže můžete začít s vytvářením prototypů modelu s malým množstvím dat. 50 obrázků na popisek jsou obecně dobrým startem. Služba ale není optimální pro detekci drobných rozdílů v obrázcích (například rozpoznávání menších prasklin nebo odsazení ve scénářích zabezpečování kvality).

Kromě toho si můžete vybrat z několika variant Custom Vision algoritmu, které jsou optimalizované pro obrázky s určitým materiálem&mdash;předmětu, například orientačních bodů nebo maloobchodních položek. Další informace naleznete v tématu [sestavování Průvodce tříděním](getting-started-build-a-classifier.md) .

## <a name="what-it-includes"></a>Co zahrnuje

Služba Custom Vision je dostupná jako sada nativních sad SDK i prostřednictvím webového rozhraní na [domovské stránce služby Custom Vision](https://customvision.ai/). Můžete vytvořit, otestovat a naučit model prostřednictvím obou rozhraní nebo obojího použít společně.

![Domovská stránka služby Custom Vision v okně prohlížeče Chrome](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Ochrana osobních údajů a zabezpečení dat

Stejně jako u všech Cognitive Services by měli vývojáři, kteří používají Custom Vision službu, znát zásady Microsoftu pro zákaznická data. Další informace najdete na [stránce Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) v centru zabezpečení Microsoftu.

## <a name="next-steps"></a>Další kroky

Postupujte podle pokynů průvodce [sestavením klasifikátoru](getting-started-build-a-classifier.md) a začněte používat Custom Vision na webu, nebo dokončete [kurz pro klasifikaci imagí](quickstarts/image-classification.md) , který implementuje základní scénář v kódu.
