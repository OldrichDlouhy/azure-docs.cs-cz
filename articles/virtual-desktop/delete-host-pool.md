---
title: Odstranit fond hostitelů virtuálních ploch Windows – Azure
description: Jak odstranit fond hostitelů na virtuálním počítači s Windows
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 07/11/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ced0d7513524758975919fad90f19581d15d95f4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87097566"
---
# <a name="delete-a-host-pool"></a>Odstranění fondu hostitelů

Všechny fondy hostitelů vytvořené ve virtuální ploše Windows jsou připojené k hostitelům relací a skupinám aplikací. Chcete-li odstranit fond hostitelů, je nutné odstranit jeho přidružené skupiny aplikací a hostitele relací. Odstranění skupiny aplikací je poměrně jednoduché, ale odstranění hostitele relace je složitější. Když odstraníte hostitele relace, musíte se ujistit, že nemá žádné aktivní uživatelské relace. Všechny uživatelské relace na hostiteli relace by se měly odhlásit, aby uživatelé nemohli ztratit data.

## <a name="delete-a-host-pool-with-powershell"></a>Odstranění fondu hostitelů pomocí PowerShellu

Pokud chcete odstranit fond hostitelů pomocí PowerShellu, musíte nejdřív odstranit všechny skupiny aplikací ve fondu hostitelů. Pokud chcete odstranit všechny skupiny aplikací, spusťte následující rutinu PowerShellu:

```powershell
Remove-AzWvdApplicationGroup -Name <appgroupname> -ResourceGroupName <resourcegroupname>
```

Potom spuštěním této rutiny odstraňte fond hostitelů:

```powershell
Remove-AzWvdHostPool -Name <hostpoolname> -ResourceGroupName <resourcegroupname> -Force:$true
```

Tato rutina odebere všechny existující uživatelské relace na hostiteli relace fondu hostitelů. Také zruší registraci hostitele relace z fondu hostitelů. V rámci vašeho předplatného budou pořád existovat všechny související virtuální počítače.

## <a name="delete-a-host-pool-with-the-azure-portal"></a>Odstranění fondu hostitelů pomocí Azure Portal

Postup odstranění fondu hostitelů v Azure Portal:

1. Přihlaste se na portál [Azure Portal](https://portal.azure.com/).

2. Vyhledejte a vyberte **virtuální počítač s Windows**.

3. V nabídce na levé straně stránky vyberte **fondy hostitelů** a pak vyberte název hostitelského fondu, který chcete odstranit.

4. V nabídce na levé straně stránky vyberte **skupiny aplikací**.

5. Vyberte všechny skupiny aplikací ve fondu hostitelů, který se chystáte odstranit, a pak vyberte **Odebrat**.

6. Po odebrání skupin aplikací přejděte do nabídky na levé straně stránky a vyberte **Přehled**.

7. Vyberte **Odebrat**.

8. Pokud jsou hostitelé relace ve fondu hostitelů, který odstraňujete, zobrazí se zpráva s dotazem, jestli má vaše oprávnění pokračovat. Vyberte **Ano**.

9. Azure Portal nyní odstraní všechny hostitele relací a odstraní fond hostitelů. Virtuální počítače týkající se hostitele relace se neodstraní a zůstanou ve vašem předplatném.

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak vytvořit fond hostitelů, najdete v těchto článcích:

- [Vytvoření fondu hostitelů s využitím webu Azure Portal](create-host-pools-azure-marketplace.md)
- [Vytvoření fondu hostitelů pomocí PowerShellu](create-host-pools-powershell.md)

Informace o tom, jak nakonfigurovat nastavení fondu hostitelů, najdete v těchto článcích:

- [Přizpůsobení vlastností protokol RDP (Remote Desktop Protocol) pro fond hostitelů](customize-rdp-properties.md)
- [Konfigurace metody vyrovnávání zatížení Windows Virtual Desktop](configure-host-pool-load-balancing.md)
- [Konfigurace typu přiřazení fondu hostitelů osobní plochy](configure-host-pool-personal-desktop-assignment-type.md)