---
title: Přidat vrstvu obrázku do mapy | Mapy Microsoft Azure
description: V tomto článku se dozvíte, jak překrýt obrázek na mapě pomocí webové sady SDK Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: 6ffec1afc44c6b920a64b4b92b384b6ce3829488
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286957"
---
# <a name="add-an-image-layer-to-a-map"></a>Přidat vrstvu obrázku do mapy

V tomto článku se dozvíte, jak překrýt obrázek do pevné sady souřadnic. Tady je několik příkladů různých typů imagí, které je možné v mapách překrývají:

* Obrázky zachycené z DRONY zachraňují životy
* Sestavování floorplans
* Historické nebo jiné specializované image map
* Modrotisky webů úloh
* Obrázky paprsků s počasí

> [!TIP]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) je jednoduchý způsob, jak překrýt obrázek na mapě. Všimněte si, že prohlížeče mohou mít potíže při načítání velkého obrázku. V takovém případě zvažte rozdělení obrázku do dlaždic a jejich načtení do mapy jako [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

Vrstva image podporuje následující formáty obrázků:

- JPEG
- PNG
- BMP
- GIF (žádné animace)

## <a name="add-an-image-layer"></a>Přidání vrstvy obrázků

Následující kód překrývá obrázek [mapy newyorském, New Jersey, z 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) na mapě. [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) se vytvoří PŘEDÁNÍM adresy URL obrázku a souřadnicemi pro čtyři rohy ve formátu `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]` .

```javascript
//Create an image layer and add it to the map.
map.layers.add(new atlas.layer.ImageLayer({
    url: 'newark_nj_1922.jpg',
    coordinates: [
        [-74.22655, 40.773941], //Top Left Corner
        [-74.12544, 40.773941], //Top Right Corner
        [-74.12544, 40.712216], //Bottom Right Corner
        [-74.22655, 40.712216]  //Bottom Left Corner
    ]
}));
```

Zde je ukázka kompletního běžícího kódu předchozího kódu.

<br/>

<iframe height='500' scrolling='no' title='Jednoduchá vrstva obrázku' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>V CodePen () na se podívejte na Azure Maps <a href='https://codepen.io/azuremaps/pen/eQodRo/'>vrstvu obrázku jednoduchého</a> pera <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="import-a-kml-file-as-ground-overlay"></a>Import souboru KML jako překryvné desky

Tento příklad ukazuje, jak přidat informace o překrytí základní desky KML jako vrstvu obrázku na mapě. KML překryvná deska poskytují souřadnice sever, jih, východní a západní délky a rotace po směru hodinových ručiček. Ale vrstva obrázku očekává souřadnice pro každý roh obrázku. Překrytí základní desky KML v této ukázce je pro Cathedral Chartres a je nasource z [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

Kód používá statickou `getCoordinatesFromEdges` funkci ze třídy [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) . Vypočítá čtyři rohy obrázku s použitím informací o Severní, Jižní, východní, západní a rotační překrytí základní desky (KML).

<br/>

<iframe height='500' scrolling='no' title='Základní překrytí KML jako vrstva obrázku' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Podívejte se na <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>překrytí KML pera jako obrázek</a> pomocí Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-an-image-layer"></a>Přizpůsobení vrstvy obrázku

Vrstva obrázku má mnoho možností stylů. Tady je nástroj pro jejich vyzkoušení.

<br/>

<iframe height='700' scrolling='no' title='Možnosti vrstvy obrázku' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>V CodePen () na se podívejte na Azure Maps <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>Možnosti vrstvy obrázku</a> pera <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Další kroky

Další informace o třídách a metodách, které se používají v tomto článku:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Další ukázky kódu pro přidání do vašich map najdete v následujících článcích:

> [!div class="nextstepaction"]
> [Přidání vrstvy dlaždic](./map-add-tile-layer.md)