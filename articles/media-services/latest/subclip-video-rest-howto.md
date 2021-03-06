---
title: Rozříznout video při kódování pomocí Azure Media Services REST
description: Toto téma popisuje, jak rozříznout video při kódování pomocí Azure Media Services pomocí REST.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: b112b3844ddb3758b79a086b624a227127e65ce2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091874"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Rozstřihnout video při kódování pomocí Media Services-REST

Video můžete při kódování pomocí [úlohy](/rest/api/media/jobs)oříznout nebo vystřihnout. Tato funkce funguje s libovolnou [transformací](/rest/api/media/transforms) , která je sestavená buď pomocí přednastavení [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) , nebo z [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset) předvoleb. 

V příkladu v tomto tématu se vytvoří úloha, která ořízne video při odesílání úlohy kódování. 

## <a name="prerequisites"></a>Předpoklady

K dokončení kroků popsaných v tomto tématu je třeba provést následující kroky:

- [Vytvořte účet Azure Media Services](./create-account-howto.md).
- [Nakonfigurujte metodu post pro Azure Media Services REST API volání](media-rest-apis-with-postman.md).
    
    Nezapomeňte postupovat podle posledního kroku v tématu [získání tokenu Azure AD](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Vytvořte transformaci a výstupní prostředky. Můžete si prohlédnout, jak vytvořit transformaci a výstupní prostředky v [kódování vzdáleného souboru na základě adresy URL a streamu v kurzu pro video – REST](stream-files-tutorial-with-rest.md) .
- Přečtěte si téma o [pojmu kódování](encoding-concept.md) .

## <a name="create-a-subclipping-job"></a>Vytvoření úlohy dílčího výstřižku

1. V stažené kolekci po výběru vyberte **Transformers a úlohy**  ->  **vytvořit úlohu s dílčím oříznutím**.
    
    Požadavek **Put** vypadá takto:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Aktualizujte hodnotu proměnné prostředí "transformovat" s názvem transformace. 
1. Vyberte kartu **tělo** a aktualizujte "myOutputAsset" s názvem výstupního prostředku.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. Stiskněte **Odeslat**.

    Zobrazí se **odpověď** s informacemi o úloze, která byla vytvořena a odeslána a stavu úlohy. 

## <a name="next-steps"></a>Další kroky

[Kódování s vlastní transformací](custom-preset-rest-howto.md) 
