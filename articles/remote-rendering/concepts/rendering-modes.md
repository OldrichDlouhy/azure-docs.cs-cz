---
title: Režimy vykreslování
description: Popisuje různé režimy vykreslování na straně serveru.
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 7f2b1031659864ae338bb0aa320c048ea23c21f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80681698"
---
# <a name="rendering-modes"></a>Režimy vykreslování

Vzdálené vykreslování nabízí dva hlavní režimy operací, režim **TileBasedComposition** a režim **DepthBasedComposition** . Tyto režimy určují, jak je zatížení distribuováno mezi více procesorů GPU na serveru. Režim musí být zadán v době připojení a nelze jej změnit během běhu.

Oba režimy se dodávají s výhodami, ale také se základními omezeními funkcí, takže výběr nejvhodnějšího režimu je specifický pro případ použití.

## <a name="modes"></a>Druzí

Tyto dva režimy jsou nyní popsány podrobněji.

### <a name="tilebasedcomposition-mode"></a>Režim TileBasedComposition

V režimu **TileBasedComposition** všechny příslušné procesory GPU vykreslují konkrétní dílčí obdélníky (dlaždice) na obrazovce. Hlavní GPU vytvoří finální obrázek z dlaždic předtím, než se odešle jako snímek videa pro klienta. Všechny procesory GPU proto musí mít stejnou sadu prostředků pro vykreslování, takže načtené prostředky musí odpovídat do paměti jednoho GPU.

Kvalita vykreslování v tomto režimu je mírně lepší než v režimu **DepthBasedComposition** , protože rozhraní MSAA může pracovat na plné sadě geometrie pro každý grafický procesor. Následující snímek obrazovky ukazuje, že antialiasing správně funguje pro obě hrany, podobně jako:

![Rozhraní MSAA v TileBasedComposition](./media/service-render-mode-quality.png)

V tomto režimu je navíc možné každou část přepnout na transparentní materiál nebo přepnout na režim **zobrazení** přes [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md) .

### <a name="depthbasedcomposition-mode"></a>Režim DepthBasedComposition

V režimu **DepthBasedComposition** se každý procesor GPU vykreslí při rozlišení celé obrazovky, ale jenom v podmnožině sítí. Konečné složení obrazu na hlavním GPU se stará o to, aby se součásti správně sloučily podle informací o jejich hloubce. Přirozeně je datová část paměti distribuována napříč GPU, což umožňuje vykreslování modelů, které se nevejdou do paměti jednoho GPU.

Každý samostatný grafický procesor používá MSAA k vyhlazení místního obsahu. Mezi hranami z různých procesorů GPU ale může docházet z vlastního aliasu. Tím se snižuje dopad postprocessing finální image, ale kvalita rozhraní MSAA je stále horší než v režimu **TileBasedComposition** .

Artefakty rozhraní MSAA jsou znázorněné na následujícím obrázku: ![ MSAA v DepthBasedComposition](./media/service-render-mode-balanced.png)

Antialiasing funguje správně mezi Sculpture a zákulisí, protože obě části jsou vykresleny na stejném GPU. Na druhé straně hrana mezi zákulisí a stěnou zobrazuje některé aliasy, protože tyto dvě části se skládají z různých procesorů GPU.

Největším omezením tohoto režimu je, že části geometrie nelze dynamicky přepnout na průhledné materiály, ani režim **zobrazení** v režimu pro [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md). Další funkce přepisu stavu (osnova, barevný nádech,...) fungují, ale. Také materiály, které byly označeny jako transparentní v době převodu, fungují v tomto režimu správně.

### <a name="performance"></a>Výkon

Charakteristiky výkonu pro oba režimy se liší v závislosti na případu použití a jsou obtížné nebo poskytují obecná doporučení. Pokud nejste omezeni omezeními uvedenými nahoře (paměť nebo transparentnost/viz-through), doporučuje se vyzkoušet oba režimy a monitorovat výkon pomocí různých poloh kamery.

## <a name="setting-the-render-mode"></a>Nastavení režimu vykreslování

Režim vykreslování použitý na virtuálním počítači pro vzdálené vykreslování je určený `AzureSession.ConnectToRuntime` prostřednictvím `ConnectToRuntimeParams` .

```cs
async void ExampleConnect(AzureSession session)
{
    ConnectToRuntimeParams parameters = new ConnectToRuntimeParams();

    // Connect with one rendering mode
    parameters.mode = ServiceRenderMode.TileBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();

    session.DisconnectFromRuntime();

    // Wait until session.IsConnected == false

    // Reconnect with a different rendering mode
    parameters.mode = ServiceRenderMode.DepthBasedComposition;
    await session.ConnectToRuntime(parameters).AsTask();
}
```

## <a name="next-steps"></a>Další kroky

* [Relace](../concepts/sessions.md)
* [Součást přepisu hierarchického stavu](../overview/features/override-hierarchical-state.md)
