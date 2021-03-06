---
title: Vytvoření a šifrování virtuálního počítače s Windows s využitím webu Azure Portal
description: V tomto rychlém startu se dozvíte, jak pomocí Azure Portal vytvořit a zašifrovat virtuální počítač s Windows.
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 1327a2c621eca1cfadcf776ecd62f0899651f0bc
ms.sourcegitcommit: 374d1533ea2f2d9d3f8b6e6a8e65c6a5cd4aea47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/01/2020
ms.locfileid: "85807923"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-with-the-azure-portal"></a>Rychlý Start: vytvoření a šifrování virtuálního počítače s Windows pomocí Azure Portal

Virtuální počítače Azure je možné vytvářet na webu Azure Portal. Azure Portal je uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet virtuální počítače a související prostředky. V tomto rychlém startu použijete Azure Portal k nasazení virtuálního počítače s Windows, vytvoříte Trezor klíčů pro ukládání šifrovacích klíčů a zašifrujete virtuální počítač.

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), ještě než začnete.

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k [portálu Azure Portal](https://portal.azure.com).


## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

1. V levém horním rohu webu Azure Portal zvolte **Vytvořit prostředek**.
1. Na nové stránce v části Oblíbené vyberte **Windows Server 2016 Datacenter**.
1. Na kartě základy v části Project Details (podrobnosti projektu) Zkontrolujte, zda je vybráno správné předplatné a pak zvolte možnost **vytvořit novou skupinu prostředků**. Jako název zadejte *myResourceGroup* .
1. Jako **název virtuálního počítače**zadejte *MyVM*.
1. V poli **oblast**vyberte stejnou oblast, kterou jste použili při vytváření trezoru klíčů (například *východní USA*).
1. Ujistěte se, že je **Velikost** *standardní D2s V3*.
1. V části **účet správce**vyberte **heslo**. Zadejte uživatelské jméno a heslo.

    :::image type="content" source="../media/disk-encryption/portal-qs-windows-vm-creation.png" alt-text="Obrazovka pro vytvoření zdroje dat":::

    > [!WARNING]
    > Karta "disky" obsahuje pole "typ šifrování" v části **Možnosti disku**. Toto pole slouží k zadání možností šifrování pro [Managed disks](managed-disks-overview.md) + CMK, ne pro Azure Disk Encryption. 
    >
    > Aby nedocházelo k nejasnostem, doporučujeme při dokončení tohoto kurzu úplně přeskočit kartu *disky* . 

1. Vyberte kartu Správa a ověřte, že máte účet úložiště diagnostiky. Pokud nemáte žádné účty úložiště, vyberte vytvořit nový, zadejte název nového účtu a vyberte OK.

    :::image type="content" source="../media/disk-encryption/portal-qs-vm-creation-storage.png" alt-text="Obrazovka pro vytvoření zdroje dat":::

1. Klikněte na zkontrolovat + vytvořit.
1. Na stránce **Vytvoření virtuálního počítače** se zobrazí podrobnosti o virtuálním počítači, který se chystáte vytvořit. Až budete připraveni, vyberte **Vytvořit**.

Nasazení virtuálního počítače bude několik minut trvat. Po dokončení nasazení se přesuňte k další části.

## <a name="encrypt-the-virtual-machine"></a>Zašifrovat virtuální počítač

1. Po dokončení nasazení virtuálního počítače vyberte **Přejít k prostředku**.
1. Na levém bočním panelu vyberte **disky**.
1. Na obrazovce disky vyberte **šifrování**. 

    :::image type="content" source="../media/disk-encryption/portal-qs-disks-to-encryption.png" alt-text="Výběr disků a šifrování":::

1. Na obrazovce šifrování v části **disky k šifrování**vyberte **operační systém a datové disky**.
1. V části **nastavení šifrování**zvolte **Vybrat Trezor klíčů a klíč pro šifrování**.
1. Na obrazovce **Vybrat klíč z Azure Key Vault** vyberte **vytvořit novou**.

    :::image type="content" source="../media/disk-encryption/portal-qs-keyvault-create.png" alt-text="Výběr disků a šifrování":::

1. Na obrazovce **Vytvoření trezoru klíčů** se ujistěte, že skupina prostředků je stejná jako ta, kterou jste použili k vytvoření virtuálního počítače.
1. Zadejte název trezoru klíčů.  Každý Trezor klíčů v Azure musí mít jedinečný název.
1. Na kartě **zásady přístupu** zaškrtněte políčko **Azure Disk Encryption pro šifrování svazku** .

    :::image type="content" source="../media/disk-encryption/portal-qs-keyvault-enable.png" alt-text="Výběr disků a šifrování":::

1. Vyberte **Zkontrolovat a vytvořit**.  
1. Po úspěšném ověření trezoru klíčů vyberte **vytvořit**. Tím se vrátíte na Azure Key Vault obrazovce na **klíč pro výběr** .
1. Pole **klíče** nechejte prázdné a zvolte **Vybrat**.
1. V horní části obrazovky šifrování klikněte na **Uložit**. Automaticky otevírané okno vás upozorní, že se virtuální počítač restartuje. Klikněte na tlačítko **Ano**.


## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete odstranit skupinu prostředků, virtuální počítač a všechny související prostředky. Provedete to tak, že vyberete skupinu prostředků pro příslušný virtuální počítač, vyberete Odstranit a pak potvrdíte název skupiny prostředků, kterou chcete odstranit.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili Key Vault, která byla povolená pro šifrovací klíče, vytvořili jste virtuální počítač a povolili jste virtuální počítač pro šifrování.  

> [!div class="nextstepaction"]
> [Přehled Azure Disk Encryption](disk-encryption-overview.md)
