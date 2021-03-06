---
title: Připojení partnerského vztahu pro partnery Exchange se serverem směrování pomocí portálu
titleSuffix: Azure
description: Vytvoření nebo úprava partnerského vztahu serveru Exchange pomocí serveru Směrování pomocí Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: 1eaf7413b01bceacbcbf3640bfe654fdad026672
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700661"
---
# <a name="create-or-modify-an-exchange-peering-with-route-server-in-azure-portal"></a>Vytvoření nebo úprava partnerského vztahu systému Exchange pomocí serveru Směrování v Azure Portal

Tento článek popisuje, jak vytvořit partnerský vztah Microsoft Exchange s směrovacím serverem pomocí Azure Portal. Tento článek také ukazuje, jak kontrolovat stav prostředku, aktualizovat ho nebo odstranit a zrušit jeho zřízení.


## <a name="before-you-begin"></a>Než začnete
* Před zahájením konfigurace si Projděte návod [požadavky](prerequisites.md) a [partnerský vztah Exchange](walkthrough-exchange-all.md) .
* Pokud už máte partnerské vztahy Exchange s Microsoftem, které se nepřevádějí na prostředky Azure, přečtěte si téma [Převod staršího partnerského vztahu Exchange na prostředek Azure pomocí portálu](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Vytvoření a zřízení partnerského vztahu Exchange

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Přihlaste se k portálu a vyberte své předplatné.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering-with-route-server"></a><a name=create></a>Vytvoření partnerského vztahu systému Exchange pomocí serveru Směrování


Jako poskytovatel internetového Exchange můžete vytvořit partnerský vztah Exchange vytvořením [partnerského]( https://go.microsoft.com/fwlink/?linkid=2129593)vztahu.

1. Na stránce **vytvořit partnerský vztah** na kartě **základy** vyplňte pole, jak je znázorněno zde:

    > [!div class="mx-imgBorder"] 
    > ![Registrace služby Peering Service](./media/setup-basics-tab.png)

* Vyberte své předplatné Azure.

* V případě skupiny prostředků můžete buď zvolit existující skupinu prostředků z rozevíracího seznamu, nebo vytvořit novou skupinu výběrem možnosti vytvořit nový. V tomto příkladu vytvoříme novou skupinu prostředků.

* Název odpovídá názvu prostředku a může to být cokoli, co si zvolíte.

* Oblast se vybere automaticky, pokud jste zvolili existující skupinu prostředků. Pokud se rozhodnete vytvořit novou skupinu prostředků, musíte taky zvolit oblast Azure, ve které chcete prostředek umístit.

    >[!NOTE]
    >Oblast, ve které se nachází skupina prostředků, je nezávislá na umístění, kde chcete vytvořit partnerský vztah s Microsoftem. Je ale osvědčeným postupem uspořádání prostředků partnerského vztahu do skupin prostředků, které se nacházejí v nejbližších oblastech Azure. Například pro partnerské vztahy v Ashburn můžete vytvořit skupinu prostředků v Východní USA nebo východní USA 2.

* V poli **PeerASN** vyberte své ASN.

    >[!IMPORTANT] 
    >Před odesláním žádosti o vytvoření partnerského vztahu můžete zvolit jenom číslo ASN s ValidationState jako schválenou. Pokud jste právě odeslali žádost PeerAsn, počkejte 12 hodin, nebo tak, aby bylo možné schválit přidružení ASN. Pokud vybrané číslo ASN čeká na ověření, zobrazí se chybová zpráva. Pokud nevidíte ASN, které potřebujete vybrat, zkontrolujte, jestli jste vybrali správné předplatné. Pokud ano, ověřte, jestli jste už vytvořili PeerAsn pomocí **[přidružení partnerského čísla ASN k předplatnému Azure](https://go.microsoft.com/fwlink/?linkid=2129592)**.

* Chcete-li pokračovat, vyberte možnost **Další: Konfigurace** .

#### <a name="configure-connections-and-submit"></a>Konfigurace připojení a odeslání

1. Na stránce vytvořit partnerský vztah na kartě Konfigurace vyplňte pole, jak je znázorněno zde:

    > [!div class="mx-imgBorder"]
    > ![Konfigurace serveru Směrování](./media/setup-exchange-conf-tab-routeserver.png)
 
    * V případě typu partnerského vztahu vyberte **přímý** .
    * V případě sítě Microsoft vyberte **AS8075 se serverem Exchange Route Server**. 
    * Vyberte SKU jako **základní zdarma**. Nevybírejte Premium zdarma, protože jsou vyhrazené pro speciální aplikace.
    * Vyberte umístění **Metro** , kde chcete nastavit partnerský vztah.

1. V části **připojení partnerských vztahů**vyberte **vytvořit novou** .

1.  V části **přímé připojení partnerského vztahu**zadejte následující podrobnosti o relaci protokolu BGP:

    > [!div class="mx-imgBorder"]
    > ![Konfigurovat přímý partnerský server směrování](./media/setup-exchange-conf-tab-direct-route.png)


     * Možnost partnerského vztahu vyberte příslušné fyzické umístění pro partnerský vztah.
     * Zprostředkovatel adres relace, výběr partnerského vztahu
     * Partnerovi poskytovatele Exchange poskytne předponu IPv4 relace.
     * Adresa IPv4 partnerské relace bude vybrána serverem Exchange partner pro server tras z rozsahu předpon IP adres.
     * Adresa IPv4 relace společnosti Microsoft bude tato IP adresa směrovače přidělená z rozsahu předpon IP adres.
     * Protokol IPv6 relace je v tuto chvíli volitelný.
     * Maximální předpona inzerovaných IPv4 adres může být až 20000. 
     * Použití pro službu partnerského vztahu je ve výchozím nastavení zakázáno. Dá se povolit, když poskytovatel Exchange podepíše smlouvu partnerské služby s Microsoftem.

1. Po dokončení klikněte na **Uložit**. 

1. V části vytvoření partnerského vztahu se zobrazí úspěšné ověření. Po úspěšném ověření klikněte na **vytvořit** .

    > [!div class="mx-imgBorder"]
    > ![Ověření nastavení](./media/setup-exchange-conf-tab-validation.png)

    >[!NOTE]
    >Pro normální poskytovatele internetových služeb (ISP), kteří jsou partnerem služby partnerského vztahu Microsoftu, se vyžaduje registrace předpony IP adres zákazníků. V případě partnerů Exchange s směrovacím serverem je však nutné zaregistrovat zákazníka čísla ASN a nikoli předpony. Stejný klíč ASN by byl platný pro registraci předpony zákazníka.

1. V části nastavení vyberte **zaregistrované čísla ASN** .

    > [!div class="mx-imgBorder"]
    > ![Konfigurovat přímý partnerský server směrování](./media/setup-exchange-registered-asn.png)

1. Vyberte **Přidat registrované číslo ASN** a v rámci vašeho předplatného Exchange vytvořte nové číslo ASN zákazníka.

    > [!div class="mx-imgBorder"]
    > ![Konfigurovat přímý partnerský server směrování](./media/setup-exchange-register-new-asn.png)

1. V části zaregistrovat číslo ASN vyberte název, vyplňte číslo ASN zákazníka a klikněte na Uložit.

1. V rámci registrovaného čísla ASN se ke každému ASN přiřadí přiřazený klíč předpony. Jako poskytovatel Exchange budete muset zadat tento klíč předpony pro zákazníka, aby mohli zaregistrovat službu partnerského vztahu v rámci svého předplatného.

    > [!div class="mx-imgBorder"]
    > ![Konfigurovat přímý partnerský server směrování](./media/setup-exchange-register-asn-prefixkey.png)




### <a name="verify-an-exchange-peering"></a><a name=get></a>Ověření partnerského vztahu Exchange
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Úprava partnerského vztahu Exchange
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Zrušení zřízení partnerského vztahu Exchange
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Další kroky

* [Vytvoření nebo úprava přímého partnerského vztahu pomocí portálu](howto-direct-portal.md)
* [Převod starší verze přímého partnerského vztahu na prostředek Azure pomocí portálu](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Další zdroje

Další informace najdete v tématu [Nejčastější dotazy k internetovým partnerům](faqs.md).
