---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 945d701a2a7dffc259c601b4dab9fa1333ccc066
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "74896609"
---
Před zahájením konfigurace ověřte, že splňujete následující kritéria:

* Pokud už máte virtuální síť, ke které se chcete připojit, ověřte, že se s ní nepřekrývá žádná z podsítí vaší místní sítě. Vaše virtuální síť nevyžaduje podsíť brány a nemůže mít žádné brány virtuální sítě. Pokud nemáte virtuální síť, můžete ji vytvořit pomocí kroků v tomto článku.
* Zařiďte rozsah IP adres pro oblast vašeho rozbočovače. Centrum je virtuální síť a rozsah adres, který zadáte pro oblast centra, se nemůže překrývat s existující virtuální sítí, ke které se připojujete. Také se nemůže překrývat s rozsahy adres, ke kterým se připojujete místně. Pokud neznáte rozsahy IP adres nacházející se v konfiguraci vaší místní sítě, zajistěte koordinaci s někým, kdo vám poskytne tyto podrobnosti.
* Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.