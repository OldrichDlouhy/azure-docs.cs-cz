---
title: Deaktivace a odstranění zařízení řady StorSimple 8000 | Microsoft Docs
description: Popisuje, jak odebrat zařízení StorSimple ze služby tím, že ji nejdřív deaktivujete a pak ji odstraníte.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2018
ms.author: alkohli
ms.openlocfilehash: 825a10bec7a9d415bdcf76e5b6f28f04060bb411
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514025"
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktivace a odstranění zařízení StorSimple

## <a name="overview"></a>Přehled

Tento článek popisuje, jak deaktivovat a odstranit zařízení StorSimple, které je připojené ke službě StorSimple Device Manager. Pokyny v tomto článku se vztahují jenom na zařízení řady StorSimple 8000, včetně cloudových zařízení StorSimple. Pokud používáte virtuální pole StorSimple, použijte příkaz [deaktivovat a odstranit virtuální pole StorSimple](storsimple-virtual-array-deactivate-and-delete-device.md).

Deaktivace: naruší spojení mezi zařízením a odpovídající službou StorSimple Device Manager. Možná si budete chtít vzít zařízení StorSimple mimo službu (například Pokud nahrazujete nebo upgradujete vaše zařízení nebo pokud už nepoužíváte StorSimple). Pokud ano, musíte zařízení deaktivovat, aby ho bylo možné odstranit.

Když zařízení deaktivujete, všechna data uložená místně na zařízení už nebudou dostupná. Obnovit lze pouze data přidružená k zařízení, které bylo uloženo v cloudu.

> [!WARNING]
> Deaktivace je TRVALá operace a nelze ji vrátit zpět. Deaktivované zařízení se nedá zaregistrovat ve službě StorSimple Device Manager, pokud se neobnoví na výchozí tovární nastavení.
>
> Proces obnovení továrního nastavení odstraní všechna data uložená místně na vašem zařízení. Proto musíte před deaktivací zařízení pořídit snímek všech vašich dat v cloudu. Tento snímek v cloudu umožňuje obnovit všechna data v pozdější fázi.

> [!NOTE]
>
> - Před deaktivací fyzického zařízení StorSimple nebo cloudového zařízení se ujistěte, že se ze zařízení skutečně odstraní data z kontejneru odstraněných svazků. Můžete monitorovat grafy pro cloudovou spotřebu a když se vynechává využití cloudu, protože jste odstranili zálohy, můžete pokračovat v deaktivaci zařízení. Pokud zařízení deaktivujete před tím, než dojde k tomuto přerušení, budou data v účtu úložiště a budou účtovány poplatky.
>
> - Před deaktivací fyzického zařízení StorSimple nebo cloudového zařízení zastavte nebo odstraňte klienty a hostitele, kteří na tomto zařízení závisejí.
>
> - Pokud se už před odstraněním dat ze zařízení odstranily účty úložiště nebo kontejnery v účtu úložiště, které jsou přidružené k kontejnerům svazků, dojde k chybě a data možná nebude možné odstranit. Doporučujeme, abyste data ze zařízení odstranili předtím, než odstraníte účet úložiště nebo kontejnery v něm. V takové situaci ale budete muset pokračovat v deaktivaci a odstranění zařízení za předpokladu, že data už jsou z účtu úložiště odebraná.

Po přečtení tohoto kurzu budete moct:

- Deaktivuje zařízení a odstraní data.
- Deaktivuje zařízení a zachová data.

## <a name="deactivate-and-delete-data"></a>Deaktivace a odstranění dat

Pokud vás zajímá, že jste zařízení zcela odstranili a nechcete uchovávat data na zařízení, proveďte následující kroky.

### <a name="to-deactivate-the-device-and-delete-the-data"></a>Deaktivace zařízení a odstranění dat

1. Před deaktivací zařízení musíte odstranit všechny kontejnery svazků (a svazky) přidružené k zařízení. Kontejnery svazků můžete odstranit až po odstranění přidružených záloh. Před deaktivací fyzického zařízení StorSimple nebo cloudového zařízení si přečtěte poznámku v předchozím přehledu.

2. Deaktivujte zařízení následujícím způsobem:

   1. Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**. V okně **zařízení** vyberte zařízení, které chcete deaktivovat, klikněte na něj pravým tlačítkem myši a potom klikněte na **deaktivovat**.

        ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. V okně **deaktivovat** zadejte název zařízení, který chcete potvrdit, a pak klikněte na **deaktivovat**. Proces deaktivace se spustí a dokončení bude trvat několik minut.

        ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Po deaktivaci můžete zařízení úplně odstranit. Odstraněním zařízení se odebere ze seznamu zařízení připojených ke službě. Služba pak už nebude moct odstraněné zařízení spravovat. Zařízení odstraníte pomocí následujících kroků:
   
   1. Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**. V okně **zařízení** vyberte deaktivované zařízení, které chcete odstranit, klikněte pravým tlačítkem myši a pak klikněte na **Odstranit**.

        ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. V okně **Odstranit** zadejte název zařízení, který chcete potvrdit, a pak klikněte na **Odstranit**. Dokončení odstranění trvá několik minut.

        ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Po úspěšném dokončení odstranění budete upozorněni. Seznam zařízení se také aktualizuje, aby odrážel odstranění.

## <a name="deactivate-and-retain-data"></a>Deaktivace a uchovávání dat

Pokud vás zajímá odstranění zařízení, ale chcete zachovat data, proveďte následující kroky:

### <a name="to-deactivate-a-device-and-retain-the-data"></a>Deaktivace zařízení a zachování dat

1. Deaktivuje zařízení. Všechny kontejnery svazků a snímky zařízení zůstanou.
   
   1. Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**. V okně **zařízení** vyberte zařízení, které chcete deaktivovat, klikněte na něj pravým tlačítkem myši a potom klikněte na **deaktivovat**.

         ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. V okně **deaktivovat** zadejte název zařízení, který chcete potvrdit, a pak klikněte na **deaktivovat**. Proces deaktivace se spustí a dokončení bude trvat několik minut.

         ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Nyní můžete převzít kontejnery svazků a přidružené snímky. Postupy získáte, když přejdete na [převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-8000-device-failover-disaster-recovery.md).
3. Po deaktivaci a převzetí služeb při selhání můžete zařízení úplně odstranit. Odstraněním zařízení se odebere ze seznamu zařízení připojených ke službě. Služba pak už nebude moct odstraněné zařízení spravovat. Pokud chcete zařízení odstranit, proveďte následující kroky:
   
   1. Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**. V okně **zařízení** vyberte deaktivované zařízení, které chcete odstranit, klikněte pravým tlačítkem myši a pak klikněte na **Odstranit**.

       ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. V okně **Odstranit** zadejte název zařízení, který chcete potvrdit, a pak klikněte na **Odstranit**. Dokončení odstranění trvá několik minut.

       ![Deaktivovat zařízení StorSimple](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. Po úspěšném dokončení odstranění budete upozorněni. Seznam zařízení se také aktualizuje, aby odrážel odstranění.

## <a name="deactivate-and-delete-a-cloud-appliance"></a>Deaktivace a odstranění cloudového zařízení

U StorSimple Cloud Appliance deaktivace z portálu zruší přidělení a odstraní virtuální počítač a prostředky, které byly vytvořeny při zřízení. Po deaktivaci cloudového zařízení není možné ho obnovit do předchozího stavu.

![Deaktivovat StorSimple Cloud Appliance](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Při deaktivaci dojde k následujícím akcím:

* StorSimple Cloud Appliance odebrána ze služby.
* Virtuální počítač pro StorSimple Cloud Appliance se odstraní.
* Disk s operačním systémem a datové disky, které byly vytvořeny pro StorSimple Cloud Appliance, jsou zachovány. Pokud tyto entity nepoužíváte, měli byste je odstranit ručně.
* Hostovaná služba a Virtual Network, které byly vytvořeny během zřizování, jsou zachovány. Pokud tyto entity nepoužíváte, měli byste je odstranit ručně.
* Cloudové snímky vytvořené StorSimple Cloud Appliance jsou zachovány.

Po deaktivaci cloudového zařízení ho můžete ze seznamu zařízení odstranit. Vyberte deaktivované zařízení, klikněte pravým tlačítkem myši a pak klikněte na **Odstranit**. StorSimple vás upozorní, jakmile se zařízení odstraní a seznam zařízení se aktualizuje.

## <a name="next-steps"></a>Další kroky

* Pokud chcete deaktivované zařízení obnovit do výchozího továrního nastavení, použijte možnost [resetovat zařízení do výchozího továrního nastavení](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Pokud potřebujete technickou pomoc, obraťte se na [Podpora Microsoftu](storsimple-8000-contact-microsoft-support.md).
* Další informace o tom, jak používat službu StorSimple Device Manager, najdete v článku [použití služby StorSimple Device Manager ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).

