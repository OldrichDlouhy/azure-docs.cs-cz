---
title: Kurz spojený s logistikou IoT | Microsoft Docs
description: Kurz připojené šablony logistické aplikace pro IoT Central
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: eac43ae68b10436b3e45452c6b1d03bec3ae4c9c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "81000557"
---
# <a name="tutorial-deploy-and-walk-through-a-connected-logistics-application-template"></a>Kurz: nasazení a procházení prostřednictvím připojené šablony logistické aplikace



V tomto kurzu se dozvíte, jak začít nasazením IoT Central **připojené šablony logistické** aplikace. Naučíte se, jak nasadit šablonu, co je součástí okna a co byste chtěli udělat dál.

V tomto kurzu se naučíte,

* vytvořit připojenou logistickou aplikaci
* Procházení aplikací 

## <a name="prerequisites"></a>Požadavky

* K nasazení této aplikace nejsou nutné žádné konkrétní požadavky.
* Doporučuje se použít předplatné Azure, ale můžete to zkusit i bez něj.

## <a name="create-connected-logistics-application-template"></a>Vytvořit připojenou šablonu logistické aplikace

Aplikaci můžete vytvořit pomocí následujících kroků.

1. Přejděte na web Azure IoT Central Správce aplikací. V levém navigačním panelu vyberte **Build (sestavit** ) a pak klikněte na kartu **maloobchod** .

    > [!div class="mx-imgBorder"]
    > ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/iotc-retail-homepage.png)

2. Vybrat **vytvořit aplikaci** v rámci **připojené logistické aplikace**

3. Při **vytváření aplikace** se otevře formulář nové aplikace a vyplní se požadované podrobnosti, jak je vidět níže.
   * **Název aplikace**: můžete použít výchozí navrhovaný název nebo zadat popisný název aplikace.
   * **Adresa URL**: můžete použít navrhovanou výchozí adresu URL nebo zadat svou snadno jedinečnou adresu URL. V dalším kroku se doporučuje výchozí nastavení, pokud už předplatné Azure máte. Můžete začít s cenovým tarifem bezplatné zkušební verze na 7 dní a po dobu platnosti bezplatného záznamu můžete kdykoli převést na standardní cenový plán.
   * **Informace o fakturaci**: ke zřízení prostředků se vyžaduje adresář, předplatné Azure a podrobnosti o oblasti.
   * **Vytvořit**: v dolní části stránky vyberte vytvořit a nasaďte svoji aplikaci.

    > [!div class="mx-imgBorder"]
    > ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-app-create.png)

    > [!div class="mx-imgBorder"]
    > ![Informace o fakturaci pro Spojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-app-create-billinginfo.png)

## <a name="walk-through-the-application"></a>Procházení aplikací 

## <a name="dashboard"></a>Řídicí panel

Po úspěšném nasazení šablony aplikace je výchozím řídicím panelem připojený portál logistiky, který se zaměřuje na portál. Northwind obchodník je fiktivní logistický poskytovatel, který spravuje loďstvum nákladu v oceánu a na zemi. V tomto řídicím panelu uvidíte dvě různé brány, které poskytují telemetrii o dodávkách spolu s přidruženými příkazy, úlohami a akcemi, které můžete udělat. Tento řídicí panel je předem nakonfigurovaný tak, aby předvedl aktivitu důležité logistické operace zařízení.
Řídicí panel je logicky dělený mezi dvěma různými operacemi správy zařízení brány. 
   * Logistická trasa k expedici nákladní dopravy a podrobnosti umístění dodávek v oceánu jsou základním prvkem pro veškerou přepravu více modálních prvků.
   * Zobrazit stav brány & relevantní informace 

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard1.png)

   * Můžete snadno sledovat celkový počet bran, aktivních a neznámých značek.
   * Můžete provádět operace správy zařízení, jako je například firmware aktualizace, vypnout senzor, zapnout senzor, aktualizovat prahovou hodnotu senzoru aktualizace, aktualizovat intervaly telemetrie a & aktualizovat kontrakty služby zařízení.
   * Zobrazit spotřebu baterie zařízení

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard2.png)

## <a name="device-template"></a>Šablona zařízení

Klikněte na kartu šablony zařízení a zobrazí se model schopností brány. Model schopností je strukturovaný kolem dvou různých rozhraní **telemetrie brány & vlastností** a **příkazů brány** .

**Vlastnost & telemetrie brány** – toto rozhraní představuje veškerou telemetrii související s senzory, umístěním a informacemi o zařízení, jako jsou mezní hodnoty senzoru & intervaly aktualizací.

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate1.png)

**Příkazy brány** – toto rozhraní uspořádá všechny možnosti příkazů brány.

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate2.png)

## <a name="rules"></a>Pravidla
Vyberte kartu pravidla a podívejte se na dvě různá pravidla, která existují v této šabloně aplikace. Tato pravidla jsou nakonfigurovaná tak, aby se pro další šetření použila e-mailová oznámení pro operátory.
 
**Výstraha krádeže brány**: Toto pravidlo se aktivuje, když senzory v průběhu cesty neočekávaně detekuje světlo. Operátory je potřeba oznámit co nejdříve a prozkoumat možnou odcizení.
 
**Nereagující bránu**: Toto pravidlo se aktivuje, pokud brána nehlásí do cloudu po dlouhou dobu. Brána může přestat reagovat z důvodu nízkého stavu baterie, ztráty připojení a stavu zařízení.

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-rules.png)

## <a name="jobs"></a>Úlohy
Vyberte kartu úlohy, chcete-li zobrazit pět různých úloh, které existují jako součást této šablony aplikace:

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-jobs.png)

Funkce Jobs můžete použít k provádění operací v nejrůznějších řešeních. V rámci úloh se používají příkazy zařízení a funkce, které umožňují provádět úlohy, jako je například zakázání specifických senzorů napříč všemi branami nebo změna prahové hodnoty senzoru v závislosti na způsobu dodávek a trase. 
   * Jedná se o standardní operaci zakázání senzorů v průběhu expedice v oceánu za účelem úspory baterie nebo snížení prahové hodnoty teploty během přepravy studených řetězů. 
 
   * Úlohy umožňují provádět operace v rámci systému, jako je například aktualizace firmwaru v bránách nebo aktualizace kontraktu služby, aby zůstaly aktuální na aktivitách údržby.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud nebudete tuto aplikaci nadále používat, odstraňte šablonu aplikace na stránce**nastavení aplikace** **pro správu** > a klikněte na **Odstranit**.

> [!div class="mx-imgBorder"]
> ![Řídicí panel připojené logistiky](./media/tutorial-iot-central-connected-logistics/connected-logistics-cleanup.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [pojmu spojené logistiky](./architecture-connected-logistics.md)
* Další informace o jiných [šablonách IoT Central maloobchodních prodejů](./overview-iot-central-retail.md)
* Další informace o [IoT Central přehledu](../core/overview-iot-central.md)
