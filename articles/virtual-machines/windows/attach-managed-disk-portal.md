---
title: Připojení spravovaného datového disku k virtuálnímu počítači s Windows – Azure
description: Postup připojení spravovaného datového disku k virtuálnímu počítači s Windows pomocí Azure Portal.
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: e8fd330fdb6e0c7d857ea99f6d51b05436c17d1a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84657166"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Připojení spravovaného datového disku k virtuálnímu počítači s Windows pomocí Azure Portal

V tomto článku se dozvíte, jak připojit nový spravovaný datový disk k virtuálnímu počítači s Windows (VM) pomocí Azure Portal. Velikost virtuálního počítače určuje, kolik datových disků můžete připojit. Další informace najdete v tématu [velikosti pro virtuální počítače](sizes.md).


## <a name="add-a-data-disk"></a>Přidat datový disk

1. Chcete-li přidat datový disk, otevřete [Azure Portal](https://portal.azure.com) . Vyhledejte a vyberte **virtuální počítače**.
2. Vyberte virtuální počítač ze seznamu.
3. Na stránce **virtuální počítač** vyberte **disky**.
4. Na stránce **disky** vyberte **přidat datový disk**.
5. V rozevíracím seznamu pro nový disk vyberte **vytvořit disk**.
6. Na stránce **vytvořit spravovaný disk** zadejte název disku a podle potřeby upravte ostatní nastavení. Po dokončení vyberte **Vytvořit**.
7. Na stránce **disky** vyberte **Uložit** a uložte novou konfiguraci disku pro virtuální počítač.
8. Když Azure vytvoří disk a připojí ho k virtuálnímu počítači, nový disk se zobrazí v nastavení disku virtuálního počítače v části **datové disky**.


## <a name="initialize-a-new-data-disk"></a>Inicializovat nový datový disk

1. Připojte se k virtuálnímu počítači.
1. V běžícím virtuálním počítači vyberte nabídku **Start** systému Windows a do vyhledávacího pole zadejte **diskmgmt. msc** . Otevře se konzola **pro správu disků** .
2. Správa disků rozpozná, že máte nový, Neinicializovaný disk a zobrazí se okno **inicializovat disk** .
3. Ověřte, že je vybraný nový disk, a pak ho inicializujte kliknutím na **OK** .
4. Nový disk se zobrazí jako **nepřidělený**. Klikněte pravým tlačítkem na libovolné místo na disku a vyberte **Nový jednoduchý svazek**. Otevře se okno **Průvodce vytvořením jednoduchého svazku** .
5. Pokračujte v průvodci, ponechte všechny výchozí hodnoty a až skončíte, vyberte **Dokončit**.
6. Zavřete **správu disků**.
7. Zobrazí se automaticky otevírané okno s informacemi o tom, že je potřeba naformátovat nový disk předtím, než ho budete moct použít. Vyberte **formátovat disk**.
8. V okně **Formátovat nový disk** zkontrolujte nastavení a pak vyberte **Spustit**.
9. Zobrazí se upozornění oznamující, že formátování disků vymaže všechna data. Vyberte **OK**.
10. Až se formátování dokončí, vyberte **OK**.

## <a name="next-steps"></a>Další kroky

- [Datový disk můžete připojit také pomocí prostředí PowerShell](attach-disk-ps.md).
- Pokud vaše aplikace potřebuje k ukládání dat použít jednotku *D:* , můžete [změnit písmeno jednotky pro dočasný disk se systémem Windows](change-drive-letter.md).
