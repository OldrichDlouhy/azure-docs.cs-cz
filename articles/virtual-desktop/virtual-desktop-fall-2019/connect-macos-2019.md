---
title: Připojení k virtuálnímu počítači s Windows (Classic) z macOS – Azure
description: Jak se připojit k virtuálnímu počítači s Windows (Classic) pomocí klienta macOS.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a4aac80f7e4ef93b6503398c225b2aeffe566dbe
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87270562"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-macos-client"></a>Připojení k virtuálnímu počítači s Windows (Classic) pomocí klienta macOS

> Platí pro: macOS 10,12 nebo novější

>[!IMPORTANT]
>Tento obsah se vztahuje na virtuální plochu Windows (Classic), která nepodporuje Azure Resource Manager objektů virtuálních klientů Windows. Pokud se snažíte spravovat Azure Resource Manager objektů virtuálních klientů Windows, přečtěte si [Tento článek](../connect-macos.md).

K prostředkům virtuálních klientů s Windows můžete přistupovat ze svých zařízení macOS pomocí našeho klienta ke stažení. V této příručce se dozvíte, jak nastavit klienta.

## <a name="install-the-client"></a>Instalace klienta

Začněte tím, že [si stáhnete](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) a nainstalujete klienta na zařízení MacOS.

## <a name="subscribe-to-a-feed"></a>Přihlášení k odběru informačního kanálu

Přihlaste se k odběru informačního kanálu, který vám správce umožnil získat seznam spravovaných prostředků, které máte k dispozici na vašem zařízení macOS.

Přihlášení k odběru informačního kanálu:

1. Na hlavní stránce vyberte **Přidat pracovní prostor** pro připojení ke službě a načtení prostředků.
2. Zadejte adresu URL informačního kanálu. Může se jednat o adresu URL nebo e-mailovou adresu:
   - Pokud použijete adresu URL, použijte tu, kterou vám správce poskytl. Obvykle je adresa URL <https://rdweb.wvd.microsoft.com> .
   - Pokud chcete používat e-mail, zadejte svou e-mailovou adresu. To klientovi oznamuje, aby vyhledal adresu URL přidruženou k vaší e-mailové adrese, pokud váš správce nakonfiguroval server tímto způsobem.
3. Vyberte možnost **Přidat**.
4. Po zobrazení výzvy se přihlaste pomocí svého uživatelského účtu.

Po přihlášení by se měl zobrazit seznam dostupných prostředků.

Jakmile se přihlásíte k odběru informačního kanálu, obsah informačního kanálu se pravidelně aktualizuje v pravidelných intervalech. Prostředky je možné přidat, změnit nebo odebrat na základě změn provedených správcem.

## <a name="next-steps"></a>Další kroky

Další informace o klientovi macOS najdete v dokumentaci ke [klientskému MacOS](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/) v části Začínáme.