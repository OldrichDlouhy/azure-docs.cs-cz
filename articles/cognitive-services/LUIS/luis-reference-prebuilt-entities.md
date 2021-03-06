---
title: Všechny předem připravené entity – LUIS
titleSuffix: Azure Cognitive Services
description: Tento článek obsahuje seznam předem vytvořených entit, které jsou součástí Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/20/2020
ms.author: diberry
ms.openlocfilehash: 7ec9cd8aade1edb7d2df2bab5cec8de445af7e44
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337972"
---
# <a name="entities-per-culture-in-your-luis-model"></a>Entity na jazykovou verzi v modelu LUIS

Language Understanding (LUIS) poskytuje předem připravené entity.

## <a name="entity-resolution"></a>Řešení entit
Když je do vaší aplikace zahrnutá předem sestavená entita, LUIS zahrnuje odpovídající řešení entity v odpovědi koncového bodu. Všechny příklady projevy jsou také označeny entitou.

Chování předem sestavených entit se nedá změnit, ale pokud chcete řešení zlepšit, [přidejte předem vytvořenou entitu jako funkci do entity strojového učení nebo subentity](luis-concept-entity-types.md#effective-prebuilt-entities).

## <a name="availability"></a>Dostupnost
Není-li uvedeno jinak, jsou předem připravené entity k dispozici ve všech národních prostředích aplikace LUIS (kultury). V následující tabulce jsou uvedeny předem připravené entity, které jsou podporovány pro každou jazykovou verzi.

|Kultura|Subkultury|Poznámky|
|--|--|--|
|Čínština|[zh-CN](#chinese-entity-support)||
|Nizozemština|[nl-NL](#dutch-entity-support)||
|Angličtina|[EN-US (USA)](#english-american-entity-support)||
|Francouzština|[fr – CA (Kanada)](#french-canadian-entity-support), [fr-FR (Francie)](#french-france-entity-support), ||
|Němčina|[de-DE](#german-entity-support)||
|Italština|[IT oddělení IT](#italian-entity-support)||
|Japonština|[ja-JP](#japanese-entity-support)||
|Korejština|[ko-KR](#korean-entity-support)||
|Portugalština|[pt-BR (Brazílie)](#portuguese-brazil-entity-support)||
|Španělština|[ES-ES (Španělsko)](#spanish-spain-entity-support), [ES-MX (Mexiko)](#spanish-mexico-entity-support)||
|Turečtina|[Turečtina](#turkish-entity-support)|V turečtině nejsou podporované předem připravené entity|

## <a name="prediction-endpoint-runtime"></a>Modul runtime koncového bodu předpovědi

Dostupnost předem připravené entity v konkrétním jazyce je určena verzí modulu runtime koncového bodu předpovědi.

## <a name="chinese-entity-support"></a>Podpora čínských entit

Podporovány jsou následující entity:

| Předem vytvořená entita | zh-CN |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    V2, V3   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="dutch-entity-support"></a>Podpora pro nizozemskou entitu

Podporovány jsou následující entity:

| Předem vytvořená entita | nl-NL |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[Hodnotu](luis-reference-prebuilt-deprecated.md)   |    -   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="english-american-entity-support"></a>Podpora entit v angličtině (USA)

Podporovány jsou následující entity:

| Předem vytvořená entita | cs-CZ |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    V2, V3   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    V2, V3   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    V2, V3   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="french-france-entity-support"></a>Podpora entit francouzština (Francie)

Podporovány jsou následující entity:

| Předem vytvořená entita | fr-FR |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |   -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="french-canadian-entity-support"></a>Podpora pro francouzštinu (kanadskou) entitu

Podporovány jsou následující entity:

| Předem vytvořená entita | fr – CA |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="german-entity-support"></a>Podpora pro německé entity

Podporovány jsou následující entity:

|Předem vytvořená entita | de-DE |
| -------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="italian-entity-support"></a>Podpora pro italskou entitu

Předem sestavený věk v italštině, měna, dimenze, číslo a procento _rozlišení_ bylo změněno z v2 a V3 Preview.

Podporovány jsou následující entity:

| Předem vytvořená entita | it-IT |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[Hodnotu](luis-reference-prebuilt-deprecated.md)   |    -   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="japanese-entity-support"></a>Podpora japonských entit

Podporovány jsou následující entity:

|Předem vytvořená entita | ja-JP |
| -------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2,-   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2,-   |
[Hodnotu](luis-reference-prebuilt-deprecated.md)   |    -   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2,-   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2,-   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2,-   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2,-   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2,-   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="korean-entity-support"></a>Podpora pro korejštinu entit

Podporovány jsou následující entity:

| Předem vytvořená entita | ko-KR |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    -   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    -   |
[Hodnotu](luis-reference-prebuilt-deprecated.md)   |    -   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    -   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    -   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    -   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    -   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    -   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="portuguese-brazil-entity-support"></a>Podpora pro portugalské (Brazílie) entity

Podporovány jsou následující entity:

| Předem vytvořená entita | pt-BR |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="spanish-spain-entity-support"></a>Španělština (Španělsko) podpora entit

Podporovány jsou následující entity:

| Předem vytvořená entita | es-ES |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    V2, V3   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    V2, V3   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    V2, V3   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    V2, V3   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="spanish-mexico-entity-support"></a>Španělština (Mexiko) podpora entit

Podporovány jsou následující entity:

| Předem vytvořená entita | ES – MX |
| --------------- | :---: |
[Stáří](luis-reference-prebuilt-age.md):<br>year<br>month<br>týden<br>day   |    -   |
[Měna (peníze)](luis-reference-prebuilt-currency.md):<br>měny<br>zlomková jednotka (např. haléře)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   |
[Dimenze](luis-reference-prebuilt-dimension.md):<br>svazků<br>oblast<br>hmotnost<br>informace (např. bitů/Byte)<br>Délka (např. měřič)<br>rychlost (např. km za hodinu)  |    -   |
[Elektron](luis-reference-prebuilt-email.md)   |    V2, V3   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Číselná](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Řadový](luis-reference-prebuilt-ordinal.md)   |    -   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Procento](luis-reference-prebuilt-percentage.md)   |    -   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Teplota](luis-reference-prebuilt-temperature.md):<br>Fahrenheita<br>Kelvin<br>rankine<br>delisle<br>Celsia   |    -   |
[Adresa URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

Zobrazit poznámky u [zastaralých předem definovaných entit](luis-reference-prebuilt-deprecated.md)

KeyPhrase není k dispozici ve všech podkulturách portugalštiny (Brazílie) – ```pt-BR``` .

## <a name="turkish-entity-support"></a>Podpora pro tureckou entitu

**V turečtině nejsou podporovány žádné předem připravené entity.**

<!--

| Prebuilt entity | tr-tr |
| --------------- | :---: |
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    -   |
[Currency (money)](luis-reference-prebuilt-currency.md):<br>dollar<br>fractional unit (ex: penny)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>volume<br>area<br>weight<br>information (ex: bit/byte)<br>length (ex: meter)<br>speed (ex: mile per hour)  |    -   |
[Email](luis-reference-prebuilt-email.md)   |    -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   |
[Number](luis-reference-prebuilt-number.md)   |    -   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    -   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    -   |

See notes on [Deprecated prebuilt entities](luis-reference-prebuilt-deprecated.md)


KeyPhrase is not available.
-->

## <a name="contribute-to-prebuilt-entity-cultures"></a>Přispět k předem vytvořeným kulturám entit
Předem připravené entity se vypracovávají v projektu pro rozpoznávání – text open source. [Přispět](https://github.com/Microsoft/Recognizers-Text) k projektu. Tento projekt obsahuje příklady měny na jazykovou verzi.

GeographyV2 a person nejsou zahrnuté do textového projektu pro rozpoznávání. V případě problémů s těmito předem vytvořenými entitami prosím otevřete [žádost o podporu](../../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Další kroky

Seznamte se s entitami [Number](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md)a [Currency](luis-reference-prebuilt-currency.md) .
