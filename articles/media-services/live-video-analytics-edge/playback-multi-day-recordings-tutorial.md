---
title: Přehrávání nahrávek na více dní – Azure
description: V tomto kurzu se naučíte používat rozhraní API služby Azure Media Service k přehrání nepřetržitého nahrávání videa.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: 52ef33e8c4380e9c21e99c4ba45b7f25f7c57780
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2020
ms.locfileid: "84433655"
---
# <a name="tutorial-playback-of-multi-day-recordings"></a>Kurz: přehrávání vícedenních nahrávek  

Tento kurz sestaví kurz [nepřetržitého nahrávání videa](continuous-video-recording-concept.md) (CVR). V tomto kurzu se naučíte používat rozhraní API služby Azure Media Service k přehrávání nepřetržitého záznamu videa z cloudu na více dní. 

To je užitečné ve scénářích, jako je třeba veřejná bezpečnost, kde je potřeba udržovat záznam o záběrech z kamery po dobu více dní (nebo týdnů) a je příležitostně potřebovat sledovat určité části těchto záběrů.

V tomto kurzu získáte informace o následujících postupech:

> [!div class="checklist"]
> * Spusťte ukázkovou aplikaci, která demonstruje, jak můžete procházet obsah pro nahrávání na více dní.
> * Zobrazit vybrané části daného záznamu

## <a name="suggested-pre-reading"></a>Navrhované před čtením  

Doporučujeme si přečíst následující stránky dokumentace:

* [Přehled živé analýzy videí na IoT Edge](overview.md)
* [Živá analýza videí na IoT Edge terminologii](terminology.md)
* [Koncept Media graphu](media-graph-concept.md)
* [Nepřetržité nahrávání videa](continuous-video-recording-concept.md) 
* [Návod: přehrávání nahrávek](playback-recordings-how-to.md)
* [Kurz: nepřetržité nahrávání videa](continuous-video-recording-tutorial.md)

## <a name="prerequisites"></a>Požadavky

* Dokončete [kurz CVR](continuous-video-recording-tutorial.md). V tomto kurzu a v relevantních rozhraních API popisovaných v [kurzu se průběžné zaznamenávání videa](continuous-video-recording-tutorial.md) vztahuje na nahrávky, které jsou 5 minut nebo déle, a pokud ne, doporučujeme vám nahrávat 5 hodin na video. Rozhraní API používaná k procházení záznamů jsou nejlépe znázorněná s dlouhými nahrávkami.
* Doporučujeme, abyste spustili tento kurz v průběhu tohoto [kurzu: nepřetržité nahrávání videa](continuous-video-recording-tutorial.md) je pořád spuštěné – to znamená, že pořád budete nahrávat video do cloudu.

## <a name="run-the-sample"></a>Spuštění ukázky 

V rámci [kurzu CVR](continuous-video-recording-tutorial.md)jste vytvořili účet Media Service. Pro tento kurz budete muset mít k tomuto účtu úplný přístup k rozhraní API. Pomocí kroků v části [získání přihlašovacích údajů můžete získat přístup k Media Services rozhraní API](../latest/access-api-howto.md#use-the-azure-portal) k vytvoření instančního objektu. Měli byste být schopni získat blok JSON z Azure Portal, který vypadá takto:

```
{
    "AadClientId": "<<INSERT_AZURE_AD_APP_ID_HERE>>",
    "AadSecret": "<<INSERT_AZURE_AD_APP_SECRET_HERE>>",
    "AadTenantDomain": "<<YOUR_TENANT_DOMAIN>>",
    "AadTenantId": "<<YOUR_TENANT_ID>>",
    "AccountName": "<<YOUR_ACCOUNT_NAME>>",
    "Location": "<<YOUR_REGION_NAME>>",
    "ResourceGroup": "<<YOUR_RESOURCE_GROUP>>",
    "SubscriptionId": "<<YOUR_SUBSCRIPTION_ID>>",
    "ArmAadAudience": "https://management.core.windows.net",
    "ArmEndpoint": "https://management.azure.com"
}
```

Dále v nástroji Visual Studio Code otevřete src/AMS – Asset-Player. Tato složka obsahuje potřebné soubory pro tento kurz. Otevřete soubor appSettings. JSON a zkopírujte jeho obsah do nového souboru appSettings. Development. JSON. Proveďte následující úpravy pro druhý soubor:

```
  "AMS" : {
    "subscriptionId" : "Use value of SubscriptionId above",
    "resourceGroup" : "Use value of ResourceGroup above",
    "accountId" : "Use value of AccountName above",
    "aadTenantId" : "Use value of AadTenantId above",
    "aadClientId" : "Use value of AadClientId above",
    "aadSecret" : "Use value of AadSecret above"
} 
```

V Visual Studio Code můžete kliknout na ikonu spustit na levé straně (nebo CTRL + SHIFT + D) a spustit tak dostupné aplikace:

![Spustit](./media/playback-multi-day-recordings-tutorial/run.png)
 
V rozevíracím seznamu vyberte aplikaci přehrávač assetů AMS, jak vidíte níže, a stisknutím klávesy F5 spusťte ladění.

![Ladění](./media/playback-multi-day-recordings-tutorial/debug.png)

Ukázková aplikace sestaví a spustí výchozí prohlížečovou aplikaci a otevře stránku přehrávače assetů pro AMS.

> [!NOTE]
> V závislosti na nastavení zabezpečení v prohlížeči se může zobrazit varovná zpráva. Vzhledem k tomu, že je webová stránka spuštěna místně, můžete ignorovat upozornění.

Přehrávač assetů AMS vás vyzve k zadání názvu prostředku Media Service. Měli byste použít název Assetu, který jste použili pro nahrávání videa v [kurzu: nepřetržité nahrávání videa](continuous-video-recording-tutorial.md).

Po zadání názvu assetu a jeho odeslání bude kód přehrávače načíst adresu URL streamování. Další informace najdete v tématu [Návod: přehrávání nahrávek](playback-recordings-how-to.md). Pokud se v takovém případě stále záznam do assetu zaznamená, přehrávač ho detekuje a pokusí se o přehrání přehrávání do poslední části zaznamenaného videa. Můžete vidět časové razítko (v UTC) v levém horním rohu přehrávače. Na následujícím snímku obrazovky si všimněte, jak je vybráno tlačítko "živé".

![Stream](./media/playback-multi-day-recordings-tutorial/assetplayer1.png)
 
Na pravé straně přehrávače uvidíte ovládací prvky pro procházení archivu. Roky, měsíce a kalendářní data v tomto ovládacím prvku jsou vyplněny pomocí rozhraní availableMedia API dokumentovaného v tématu [Návod: přehrávání nahrávek](playback-recordings-how-to.md).
Pokud po rozbalení dne necháte kurz CVR běžet několik hodin, zobrazí se výsledek podobný tomuto:

![Procházet archiv](./media/playback-multi-day-recordings-tutorial/results.png)

Zdrojem informačního kanálu videa v tomto kurzu je soubor MKV. Když simulátor RSTP (viz [Live555 Media Server](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)) dosáhne konce souboru, ukončí datový proud. Uzel zdroje RTSP v mediálním grafu zjistí toto a znovu naváže připojení a video se obnoví. V mezi každým tímto koncem souboru a opětovným připojením existuje mezera v zaznamenaném archivu, který se zobrazí jako nová položka v availableMedia výsledcích.

![Výsledky](./media/playback-multi-day-recordings-tutorial/assetplayer2.png)
 
Když kliknete na libovolnou položku v seznamu, aplikace vytvoří adresu URL streamování s příslušným filtrem, například https://{hostname}/{locatorId}/Content. ISM/manifest (Format = MPD-Time-CSF, Čas_spuštění = RRRR-MM-DDTHH: MM: SS). Přehrávač načte tuto adresu URL a přehrávání začne na požadované Čas_spuštění.

Alternativně můžete použít konkrétní počáteční a koncové časy prostřednictvím ovládacích prvků v dolní části stránky. Můžete použít výsledky volání availableMedia, jak je uvedeno na pravé straně, jako vodítko k povoleným hodnotám počátečního a koncového času. Podrobná omezení pro filtry startTime a čas_ukončení jsou popsána v tématu [Návod: přehrávání záznamů](playback-recordings-how-to.md). Po výběru hodnot času po kliknutí na Odeslat aplikace nahraje přehrávač s adresou URL pro streamování: https://{hostname}/{locatorId}/Content. ISM/manifest (Format = MPD-Time-CSF, Čas_spuštění = RRRR-MM-DDTHH: MM: SS, čas_ukončení = RRRR-MM-DDTHH: MM: SS) přehrávání bude zahájeno na požadovaném Čas_spuštění.

## <a name="next-steps"></a>Další kroky

[Nahrávání videa na základě událostí do cloudu a přehrávání z cloudu](event-based-video-recording-tutorial.md)
