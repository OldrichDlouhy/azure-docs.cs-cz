---
title: Aktivní a neaktivní události – Přizpůsobte si
description: Tento článek popisuje použití aktivních a neaktivních událostí v rámci služby přizpůsobeného modulu.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: a8f27542208965e2b820b9fc45cfcc5353a7f193
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77624250"
---
# <a name="active-and-inactive-events"></a>Aktivní a neaktivní události

**Aktivní** událost je jakékoli volání do pořadí, ve kterém víte, že se vám zobrazí výsledek pro zákazníka, a určí se skóre odměny. Toto je výchozí chování.

**Neaktivní** událost je volání k zařazení, kde si nejste jistí, jestli se uživatel někdy na základě obchodní logiky uvidí doporučenou akci. To vám umožní tuto událost zrušit, takže přizpůsobování není školené s výchozí záměna. Neaktivní události by neměly volat rozhraní API pro odměnu.

Je důležité, aby studijní smyčka znala skutečný typ události. Neaktivní událost nebude mít volání odměna. Aktivní událost by měla být volána, ale pokud není volání rozhraní API nikdy provedeno, použije se výchozí skóre pro odměňování. Změnit stav události z neaktivní na aktivní, jakmile budete znát, bude mít vliv na uživatelské prostředí.

## <a name="typical-active-events-scenario"></a>Scénář typických aktivních událostí

Když vaše aplikace volá rozhraní API pro řazení, dostanete akci, kterou by měla aplikace zobrazit v poli **rewardActionId** .  V takovém případě očekává, že přizpůsobuje volání odměna s skóre odměňování, které má stejné ID události. Skóre odměňování se používá ke školení modelu pro budoucí volání pořadí. Pokud se pro ID události nepřijme žádné volání, použije se výchozí měna. [Výchozí ceny](how-to-settings.md#configure-rewards-for-the-feedback-loop) jsou nastaveny na prostředku přizpůsobeného v Azure Portal.

## <a name="other-event-type-scenarios"></a>Další scénáře typu události

V některých scénářích může aplikace před tím, než bude vědět, zda bude výsledek použit nebo zobrazen uživateli, volat funkci Rank. K tomu může dojít v situacích, kdy například vykreslování stránky propagovaného obsahu přepíše marketingová kampaň. Pokud výsledek volání řazení nebyl nikdy použit a uživatel ho nikdy neviděl, neodešlete odpovídající volání odměna.

K těmto scénářům obvykle dochází v následujících případech:

* Předvedete si předvykreslování uživatelského rozhraní, které uživatel může nebo nemusí zobrazit.
* Vaše aplikace provádí prediktivní přizpůsobení, ve kterém se provádí volání pořadí s malým kontextem v reálném čase a aplikace může nebo nemusí používat výstup.

V těchto případech použijte přizpůsobení pro volání pořadí, které vyžaduje, aby událost byla _neaktivní_. Přizpůsobování neočekává pro tuto událost odměňování a nepoužije výchozí odměnu.

Pokud aplikace používá informace z volání pořadí, použijte později v obchodní logice, stačí událost _aktivovat_ . Jakmile je událost aktivní, přizpůsobování očekává nějakou odměnu události. Pokud neprovede žádné explicitní volání rozhraní API pro odměnu, přizpůsobuje se výchozí odměna.

## <a name="inactive-events"></a>Neaktivní události

Chcete-li zakázat školení pro událost, zakažte pořadí `learningEnabled = False`volání pomocí.

V případě neaktivní události se učení implicitně aktivuje, pokud odešlete nějakou odměnu pro ID události nebo `activate` zavoláte rozhraní API pro daný ID události.

## <a name="next-steps"></a>Další kroky

* Naučte [se určit skóre a jaká data zvážit](concept-rewards.md).
