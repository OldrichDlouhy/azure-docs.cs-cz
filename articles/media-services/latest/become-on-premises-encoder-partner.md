---
title: Staňte se místním partnerem kodéru – Azure Media Services
description: Tento článek popisuje, jak ověřit vaše místní kodéry živého streamování.
services: media-services
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 03/02/2020
ms.topic: article
ms.service: media-services
ms.openlocfilehash: f98d9942f8c30f06b0144503b056c1e8a393ae52
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "79298627"
---
# <a name="how-to-verify-your-on-premises-live-streaming-encoder"></a>Jak ověřit místní kodér živého streamování

Jako Azure Media Services on-premises Encoder partner Media Services propaguje váš produkt tím, že doporučí kodér zákazníkům pro podniky. Pokud se chcete stát místním partnerem kodéru, musíte ověřit kompatibilitu místního kodéru s Media Services. Provedete to tak, že provedete následující ověření.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="pass-through-live-event-verification"></a>Předávací ověření události za provozu

1. Ujistěte se, že je v účtu Media Services spuštěný **koncový bod streamování** . 
2. Vytvořte a spusťte **předávací** živou událost. <br/> Další informace najdete v tématu [stavy událostí Live a fakturace](live-event-states-billing.md).
3. Získejte adresy URL pro příjem a nakonfigurujte místní kodér tak, aby používal adresu URL k odeslání živého datového proudu s více přenosovými rychlostmi do Media Services.
4. Získejte adresu URL náhledu a použijte ji k ověření, že vstup z kodéru je skutečně přijatý.
5. Vytvoří nový objekt **assetu** .
6. Vytvořte **živý výstup** a použijte název assetu, který jste vytvořili.
7. Vytvořte **Lokátor streamování** s integrovanými typy **zásad streamování** .
8. Vypište cesty na **lokátoru streamování** a vraťte tak adresy URL, které se mají použít.
9. Získejte název hostitele **koncového bodu streamování** , ze kterého chcete streamovat datový proud.
10. Kombinací adresy URL z kroku 8 s názvem hostitele v kroku 9 získáte úplnou adresu URL.
11. Spusťte živý kodér po dobu přibližně 10 minut.
12. Zastaví živou událost. 
13. Pomocí přehrávače, jako je například [Azure Media Player](https://aka.ms/azuremediaplayer) , Sledujte archivovaný Asset a ujistěte se, že přehrávání nemá žádné viditelné histogramu na všech úrovních kvality. Nebo sledujte a ověřte pomocí adresy URL náhledu během živé relace.
14. Poznamenejte si ID assetu, publikovanou adresu URL streamování pro živý archiv a nastavení a verzi používanou z kodéru Live Encoder.
15. Po vytvoření každého vzorku resetujte stav živé události.
16. Opakujte kroky 5 až 15 pro všechny konfigurace, které kodér podporuje (s signalizací a bez něj, popisky nebo jiné rychlosti kódování).

## <a name="live-encoding-live-event-verification"></a>Live Encoding – ověření události Live

1. Ujistěte se, že je v účtu Media Services spuštěný **koncový bod streamování** . 
2. Vytvořte a spusťte živou událost **živého kódování** . <br/> Další informace najdete v tématu [stavy událostí Live a fakturace](live-event-states-billing.md).
3. Získejte adresy URL pro příjem a nakonfigurujte kodér tak, aby do Media Services načetl živý datový proud s jednou přenosovou rychlostí.
4. Získejte adresu URL náhledu a použijte ji k ověření, že vstup z kodéru je skutečně přijatý.
5. Vytvoří nový objekt **assetu** .
6. Vytvořte **živý výstup** a použijte název assetu, který jste vytvořili.
7. Vytvořte **Lokátor streamování** s integrovanými typy **zásad streamování** .
8. Vypište cesty na **lokátoru streamování** a vraťte tak adresy URL, které se mají použít.
9. Získejte název hostitele **koncového bodu streamování** , ze kterého chcete streamovat datový proud.
10. Kombinací adresy URL z kroku 8 s názvem hostitele v kroku 9 získáte úplnou adresu URL.
11. Spusťte živý kodér po dobu přibližně 10 minut.
12. Zastaví živou událost.
13. Pomocí přehrávače, jako je například [Azure Media Player](https://aka.ms/azuremediaplayer) , Sledujte archivovaný Asset a ujistěte se, že přehrávání nemá žádné viditelné histogramu pro všechny úrovně kvality. Nebo sledujte a ověřte pomocí adresy URL náhledu během živé relace.
14. Poznamenejte si ID assetu, publikovanou adresu URL streamování pro živý archiv a nastavení a verzi používanou z kodéru Live Encoder.
15. Po vytvoření každého vzorku resetujte stav živé události.
16. Opakujte kroky 5 až 15 pro všechny konfigurace, které kodér podporuje (s signalizací a bez něj, popisky nebo jiné rychlosti kódování).

## <a name="longevity-verification"></a>Longevity ověřování

Použijte stejný postup jako v rámci [předávacího ověřování pro živé události](#pass-through-live-event-verification) s výjimkou kroku 11. <br/>Místo 10 minut spusťte živý kodér po dobu jednoho týdne nebo déle. Pomocí přehrávače, jako je například [Azure Media Player](https://aka.ms/azuremediaplayer) , Sledujte živé streamování v čase (nebo archivovaný Asset), abyste zajistili, že přehrávání nemá žádné viditelné histogramu.

## <a name="email-your-recorded-settings"></a>Poslat zaznamenaná nastavení e-mailem

Nakonec odešlete e-mailem zaznamenaná nastavení a parametry živého archivu do Azure Media Services amshelp@microsoft.com jako oznámení, že prošly všechny kontroly pro vlastní ověření. Také zahrňte kontaktní informace pro jakékoliv následné zprávy. Můžete se obrátit na tým Azure Media Services s případnými dotazy týkajícími se tohoto procesu.

## <a name="see-also"></a>Viz také

[Testované místní kodéry](recommended-on-premises-live-encoders.md)

## <a name="next-steps"></a>Další kroky

[Živé streamování s Media Services V3](live-streaming-overview.md)
