---
title: zahrnout soubor
description: zahrnout soubor
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ea97e2dbee87f7ab129c4295276c9024c0212c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80116922"
---
Novou funkcí klienta sítě VPN s Windows 10, která je vždycky zapnutá, je možnost udržovat připojení VPN. Díky službě Always On se aktivní profil sítě VPN může připojit automaticky a zůstat připojený na triggerech, jako jsou přihlášení uživatele, změna stavu sítě nebo obrazovka zařízení aktivní.

Brány se systémem Windows 10, které mají trvalé tunelové propojení a tunely zařízení v Azure, můžete používat vždycky. Tento článek vám pomůže nakonfigurovat tunelové propojení uživatelů VPN typu Always On.

Připojení k síti VPN Always On zahrnuje jeden ze dvou typů tunelů:

* **Tunelové propojení zařízení**: připojí se k zadaným serverům sítě VPN, než se uživatelé přihlásí k zařízení. Scénáře připojení před přihlášením a Správa zařízení používají tunelové zařízení.

* **Tunelové propojení uživatelů**: připojuje se až po přihlášení uživatelů k zařízení. Pomocí tunelů uživatelů můžete získat přístup k prostředkům organizace prostřednictvím serverů VPN.

Tunely zařízení a tunely uživatelů fungují nezávisle na jejich profilech sítě VPN. Můžou být připojené ve stejnou dobu a můžou podle potřeby používat jiné metody ověřování a další nastavení konfigurace VPN.
