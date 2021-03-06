---
title: Přidání vrstvy vysunutí mnohoúhelníku na mapu | Mapy Microsoft Azure
description: Postup přidání vrstvy vysunutí mnohoúhelníku do webové sady SDK Microsoft Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: cd12242367c6f3b5b2c64df28f7f5e52cf1a4f97
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282870"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Přidání vrstvy vysunutí mnohoúhelníku na mapu

V tomto článku se dozvíte, jak použít vrstvu vysunutí mnohoúhelníku k vykreslení oblastí `Polygon` a `MultiPolygon` funkcí geometrií jako vytlačených tvarů. Sada Azure Maps Web SDK podporuje vykreslování geometrií kroužků, jak je definováno v [rozšířeném schématu](extend-geojson.md#circle)pro zobrazení. JSON. Tyto kroužky je možné transformovat na mnohoúhelníky při vykreslování na mapě. Všechny funkce geometrií se můžou snadno aktualizovat při zabalení pomocí nástroje [Atlas. Třída Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)

## <a name="use-a-polygon-extrusion-layer"></a>Použití vrstvy vysunutí mnohoúhelníku

Připojte [vrstvu vysunutí mnohoúhelníku](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) ke zdroji dat. Pak ho načtete na mapě. Vrstva vysunutí mnohoúhelníku vykresluje oblasti `Polygon` `MultiPolygon` funkcí a jako vytlačené obrazce. `height`Vlastnosti a `base` vrstvy vysunutí mnohoúhelníku definují základní vzdálenost od základu a výšky vytlačeného obrazce v **měřičích**. Následující kód ukazuje, jak vytvořit mnohoúhelník, přidat ho do zdroje dat a vykreslit pomocí třídy vrstev vytlačení mnohoúhelníku.

> [!Note]
> `base`Hodnota definovaná ve vrstvě vysunutí mnohoúhelníku by měla být menší než nebo rovna hodnotě `height` .

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Vytlačený mnohoúhelník" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Podívejte se na pero <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>vytlačené mnohoúhelníkem</a> Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-polygons"></a>Přidat mnohoúhelníky řízené daty

Mapu kartogramy lze vykreslit pomocí vrstvy vysunutí mnohoúhelníku. Nastavte `height` vlastnosti a `fillColor` vrstvy vytlačení na měření statistické proměnné v `Polygon` `MultiPolygon` geometrií funkcí a. Následující ukázka kódu ukazuje vytlačenou mapu kartogramyu U. S na základě měření hustoty populace podle stavu.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Vytlačená mapa kartogramy" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Podívejte se na <a href='https://codepen.io/azuremaps/pen/eYYYNox'>mapu kartogramy vytlačený</a> perem Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Přidání kruhu k mapě

Azure Maps používá rozšířenou verzi schématu injson, která poskytuje definici pro kruhy, jak je uvedeno [zde](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle). Vytlačený kruh lze na mapě vykreslit vytvořením `point` funkce s `subType` vlastností `Circle` a číslovanou vlastností, která `Radius` představuje poloměr v **metrech**. Například:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Azure Maps webová sada SDK převede tyto `Point` funkce na `Polygon` funkce v digestoři. Tyto `Point` funkce lze vykreslit na mapě pomocí vrstvy vytlačení mnohoúhelníku, jak je znázorněno v následujícím příkladu kódu.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mnohoúhelník prostorů pro pomocí dronů" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Podívejte se na pero <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>pomocí dronů pro vzdušný Azure Maps prostor</a> <a href='https://codepen.io/azuremaps'>@azuremaps</a> na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Přizpůsobení vrstvy vysunutí mnohoúhelníku

Vrstva vytlačení mnohoúhelníku má několik možností stylů. Tady je nástroj pro jejich vyzkoušení.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Podívejte se na Azure Maps <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> pera <a href='https://codepen.io/azuremaps'>@azuremaps</a> na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Další kroky

Další informace o třídách a metodách, které se používají v tomto článku:

> [!div class="nextstepaction"]
> [Mnohoúhelník](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [vrstva vysunutí mnohoúhelníku](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Další prostředky:

> [!div class="nextstepaction"]
> [Azure Maps rozšíření pro injson Specification](extend-geojson.md#circle)
