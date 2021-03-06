---
title: Samoobslužné resetování hesla v Azure Active Directory B2C | Microsoft Docs
description: Ukazuje, jak nastavit Samoobslužné resetování hesla pro vaše zákazníky v rámci služby Azure Active Directory B2C
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 465643d21d3059cd41948ff682d7464b00f749c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85384001"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Nastavení samoobslužného resetování hesla pro vaše zákazníky

Díky funkci samoobslužného resetování hesla si vaši zákazníci, kteří si zaregistrovali místní účty, můžou svá hesla resetovat sami. To významně snižuje zatížení pracovníků podpory, zejména v případě, že vaše aplikace má v pravidelných intervalech miliony zákazníků. V současné době je jako jediná podporovaná metoda obnovení použita ověřená e-mailová adresa.

> [!NOTE]
> Tento článek se týká samoobslužného resetování hesla, které se používá v souvislosti s uživatelským tokem **přihlášení** V1, který jako zprostředkovatele identity používá **přihlášení pomocí místního účtu** . Pokud potřebujete plně přizpůsobitelné uživatelské toky pro resetování hesla vyvolané z vaší aplikace, přečtěte si [Tento článek](user-flow-overview.md).
>
>

Ve výchozím nastavení nemá váš adresář zapnuté Samoobslužné resetování hesla. K jeho zapnutí použijte následující postup:

1. Přihlaste se k [Azure Portal](https://portal.azure.com/) jako správce předplatného. Jedná se o stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k vytvoření adresáře.
2. Otevřete **Azure Active Directory** (v navigačním panelu na levé straně).
3. V okně Možnosti přejděte dolů a vyberte **resetování hesla**.
4. Nastavte **Samoobslužné resetování hesla povolené** na **vše**.
5. Klikněte na **Uložit** v horní části stránky. A je to hotové!

K otestování použijte funkci spustit nyní na jakémkoli uživatelském přihlašovacím toku, který má místní účty jako zprostředkovatele identity. Na přihlašovací stránce místního účtu (kde zadáváte e-mailovou adresu a heslo nebo uživatelské jméno a heslo), klikněte na **nepřístup k vašemu účtu** a ověřte si prostředí pro zákazníky.

> [!NOTE]
> Stránky samoobslužného resetování hesla je možné přizpůsobit pomocí [funkce Branding společnosti](../active-directory/fundamentals/customize-branding.md).
>
>

