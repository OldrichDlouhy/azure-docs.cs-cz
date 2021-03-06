---
title: StylesObject pro dynamickou Azure Maps
description: Referenční příručka ke schématu a syntaxi JSON pro StylesObject, která se používá při vytváření v dynamickém Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/19/2020
ms.topic: reference
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4b085fbc6e330d38b59fce0c494f672b00c712b7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85120514"
---
# <a name="stylesobject-schema-reference-guide-for-dynamic-maps"></a>Referenční příručka schématu StylesObject pro dynamická mapování

Tento článek představuje referenční příručku ke schématu a syntaxi JSON pro `StylesObject` . `StylesObject`Je `StyleObject` pole reprezentující styly stateset. Pomocí [služby stavu funkce](https://docs.microsoft.com/rest/api/maps/featurestate) Azure Maps Creator můžete použít styly stateset pro funkce dat mapy v interiéru. Jakmile vytvoříte styly stateset a přidružíte je k funkcím vnitřních map, můžete je použít k vytvoření dynamické mapy vnitřních oken. Další informace o vytváření dynamických map vnitřníchy najdete v tématu [Implementace dynamického stylu pro mapy vnitřních vnitřních](indoor-map-dynamic-styling.md)přípon.

## <a name="styleobject"></a>StyleObject

A `StyleObject` je buď jako [`BooleanTypeStyleRule`](#booleantypestylerule) nebo [`NumericTypeStyleRule`](#numerictypestylerule) .

Následující kód JSON znázorňuje `BooleanTypeStyleRule` pojmenované `occupied` a `NumericTypeStyleRule` pojmenované `temperature` .

```json
 "styles": [
     {
        "keyname": "occupied",
        "type": "boolean",
        "rules": [
            {
                "true": "#FF0000",
                "false": "#00FF00"
            }
        ]
    },
    {
        "keyname": "temperature",
        "type": "number",
        "rules": [
             {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "maximum": 70,
                "exclusiveMinimum": 30
              },
              "color": "#eba834"
            }
        ]
    }
]
```

## <a name="numerictypestylerule"></a>NumericTypeStyleRule

 A `NumericTypeStyleRule` je [`StyleObject`](#styleobject) a skládá se z následujících vlastností:

| Vlastnost | Typ | Description | Vyžadováno |
|-----------|----------|-------------|-------------|
| `keyName` | řetězec | Název *stavu* nebo dynamické vlastnosti. `keyName`Element by měl být v `StyleObject` poli jedinečný.| Yes |
| `type` | řetězec | Hodnota je "číselná hodnota". | Yes |
| `rules` | [`NumberRuleObject`](#numberruleobject)[]| Pole číselných stylů a rozsahů s přidruženými barvami. Každý rozsah definuje barvu, která se má použít, když hodnota *stavu* splní rozsah.| Yes |

### <a name="numberruleobject"></a>NumberRuleObject

`NumberRuleObject`Sestává z [`RangeObject`](#rangeobject) `color` vlastností a. Pokud hodnota *stavu* spadá do rozsahu, jeho barva pro zobrazení bude barva zadaná ve `color` Vlastnosti.

Pokud definujete více překrývajících se rozsahů, vybraná barva bude barva, která je definována v prvním rozsahu, který je splněn.

V následujícím příkladu JSON budou oba rozsahy platit, pokud je hodnota *stavu* mezi 50-60. Barva, která se bude používat, ale je `#343deb` v seznamu první oblast, která byla splněná.

```json

    {
        "rules":[
            {
                "range": {
                "minimum": 50,
                "exclusiveMaximum": 70
                },
                "color": "#343deb"
            },
            {
                "range": {
                "minimum": 50,
                "maximum": 60
                },
                "color": "#eba834"
            }
        ]
    }
]
```

| Vlastnost | Typ | Description | Vyžadováno |
|-----------|----------|-------------|-------------|
| `range` | [RangeObject](#rangeobject) | [RangeObject](#rangeobject) definuje sadu logických podmínek rozsahu, které v případě `true` změny barvy zobrazení *stavu* na barvu určenou `color` vlastností. Pokud `range` parametr není zadán, bude vždy použita barva definovaná ve `color` Vlastnosti.   | No |
| `color` | řetězec | Barva, která se má použít, pokud hodnota stavu spadá do rozsahu. `color`Vlastnost je řetězec formátu JSON v jednom z následujících formátů: <ul><li> Šestnáctkové hodnoty ve stylu HTML </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, 50%, 50%)")</li><li> HSLA ("HSLA (100; 50%; 50%; 1)")</li><li> Předdefinované názvy barev HTML, jako je žlutá a modrá.</li></ul> | Yes |

### <a name="rangeobject"></a>RangeObject

`RangeObject`Definuje hodnotu číselného rozsahu [`NumberRuleObject`](#numberruleobject) . Aby hodnota *stavu* mohla být do rozsahu, všechny definované podmínky musí držet hodnotu true. 

| Vlastnost | Typ | Description | Vyžadováno |
|-----------|----------|-------------|-------------|
| `minimum` | double | Celé číslo x, které x ≥ `minimum` .| No |
| `maximum` | double | Vše číslo x, které x ≤ `maximum` . | No |
| `exclusiveMinimum` | double | Číslo x > `exclusiveMinimum` .| No |
| `exclusiveMaximum` | double | Číslo x < `exclusiveMaximum` .| No |

### <a name="example-of-numerictypestylerule"></a>Příklad NumericTypeStyleRule

Následující kód JSON ukazuje `NumericTypeStyleRule` *stav* s názvem `temperature` . V tomto příkladu [`NumberRuleObject`](#numberruleobject) obsahuje dva definované rozsahy teplot a jejich přidružené styly barev. Pokud je rozsah teploty 50-69, displej by měl používat barvu `#343deb` .  Pokud je rozsah teploty 31-70, displej by měl používat barvu `#eba834` .

```json
{
    "keyname": "temperature",
    "type": "number",
    "rules":[
        {
            "range": {
            "minimum": 50,
            "exclusiveMaximum": 70
            },
            "color": "#343deb"
        },
        {
            "range": {
            "maximum": 70,
            "exclusiveMinimum": 30
            },
            "color": "#eba834"
        }
    ]
}
```

## <a name="booleantypestylerule"></a>BooleanTypeStyleRule

A `BooleanTypeStyleRule` je [`StyleObject`](#styleobject) a skládá se z následujících vlastností:

| Vlastnost | Typ | Description | Vyžadováno |
|-----------|----------|-------------|-------------|
| `keyName` | řetězec |  Název *stavu* nebo dynamické vlastnosti.  `keyName`Element by měl být jedinečný uvnitř Style Array.| Yes |
| `type` | řetězec |Hodnota je "Boolean". | Yes |
| `rules` | [`BooleanRuleObject`](#booleanruleobject)první| Logický pár s barvami pro `true` a `false` hodnoty *stavu* .| Yes |

### <a name="booleanruleobject"></a>BooleanRuleObject

`BooleanRuleObject`Definuje barvy pro `true` hodnoty a `false` .

| Vlastnost | Typ | Description | Vyžadováno |
|-----------|----------|-------------|-------------|
| `true` | řetězec | Barva, která se má použít, pokud je hodnota *stavu* `true` . `color`Vlastnost je řetězec formátu JSON v jednom z následujících formátů: <ul><li> Šestnáctkové hodnoty ve stylu HTML </li><li> RGB ("#ff0", "#ffff00", "RGB (255, 255, 0)")</li><li> RGBA ("RGBA (255, 255, 0, 1)")</li><li> HSL ("HSL (100, 50%, 50%)")</li><li> HSLA ("HSLA (100; 50%; 50%; 1)")</li><li> Předdefinované názvy barev HTML, jako je žlutá a modrá.</li></ul>| Yes |
| `false` | řetězec | Barva, která se má použít, pokud je hodnota *stavu* `false` . | Yes |

### <a name="example-of-booleantypestylerule"></a>Příklad BooleanTypeStyleRule

Následující kód JSON ukazuje `BooleanTypeStyleRule` *stav* s názvem `occupied` . [`BooleanRuleObject`](#booleanruleobject)Definuje barvy pro `true` hodnoty a `false` .

```json
{
    "keyname": "occupied",
    "type": "boolean",
    "rules": [
    {
        "true": "#FF0000",
        "false": "#00FF00"
    }
    ]
}
```
