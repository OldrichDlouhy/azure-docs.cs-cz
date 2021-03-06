---
title: Nasazení kontejneru LUIS ve službě Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Nasaďte kontejner LUIS do instance kontejneru Azure a otestujte ho ve webovém prohlížeči.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: 08af17106846a0f5f7a0ccc2b01da1b2e15c1143
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80879194"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Nasazení kontejneru Language Understanding (LUIS) do služby Azure Container Instances

Naučte se, jak nasadit kontejner Cognitive Services [Luis](luis-container-howto.md) do služby Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Tento postup ukazuje vytvoření prostředku detektoru anomálií. Pak se podíváme na navýšení přidružené image kontejneru. Nakonec zvýrazníme možnost cvičení těchto dvou z prohlížeče. Pomocí kontejnerů můžete před správou infrastruktury místo toho, aby se zaměřily na vývoj aplikací, posunout pozornost vývojářů.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="create-an-azure-file-share"></a>Vytvoření sdílené složky Azure

Kontejner LUIS vyžaduje soubor `.gz` modelu, který je načítán za běhu. Kontejner musí být schopný získat přístup k tomuto souboru modelu prostřednictvím připojení svazku z instance kontejneru. Informace o tom, jak vytvořit sdílenou složku Azure, najdete v tématu [Vytvoření sdílení souborů](../../storage/files/storage-how-to-create-file-share.md). Poznamenejte si název účtu Azure Storage, klíč a název sdílené složky, abyste je mohli později potřebovat.

### <a name="export-and-upload-packaged-luis-app"></a>Export a nahrání zabalených aplikací v LUIS

Aby bylo možné nahrát LUIS model (zabalenou aplikaci) do sdílené složky Azure, budete <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener">ho muset nejdřív <span class="docon docon-navigate-external x-hidden-focus"> </span>exportovat z portálu Luis </a>. V Azure Portal přejděte na stránku **Přehled** prostředku účtu úložiště a vyberte **sdílené složky**. Vyberte název sdílené složky, který jste nedávno vytvořili, a pak vyberte tlačítko **nahrát** .

> [!div class="mx-imgBorder"]
> ![Odeslat do sdílení souborů](media/luis-how-to-deploy-to-aci/upload-file-share.png)

Nahrajte soubor modelu LUIS.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
