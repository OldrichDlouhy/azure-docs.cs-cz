---
title: Připojení k virtuálnímu počítači se systémem Linux pomocí Azure bastionu
description: V tomto článku se dozvíte, jak se připojit k virtuálnímu počítači se systémem Linux pomocí Azure bastionu.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 98f1f3d02d8433dd88fed844a587a961ea9f8c82
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744285"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>Připojení pomocí protokolu SSH k virtuálnímu počítači se systémem Linux pomocí Azure bastionu

V tomto článku se dozvíte, jak bezpečně a bezproblémově popracovat s virtuálními počítači Linux ve službě Azure Virtual Network. K virtuálnímu počítači se můžete připojit přímo z Azure Portal. Při použití Azure bastionu virtuální počítače nevyžadují klienta, agenta ani další software. Další informace o Azure bastionu najdete v tématu [Přehled](bastion-overview.md).

Pomocí služby Azure bastionu se můžete připojit k virtuálnímu počítači se systémem Linux pomocí protokolu SSH. Pro ověřování můžete použít jak uživatelské jméno/heslo, tak klíče SSH. K VIRTUÁLNÍmu počítači se můžete připojit pomocí klíčů SSH pomocí těchto akcí:

* Soukromý klíč, který zadáte ručně
* Soubor, který obsahuje informace o privátním klíči

Privátní klíč SSH musí být ve formátu, který začíná `"-----BEGIN RSA PRIVATE KEY-----"` a končí na `"-----END RSA PRIVATE KEY-----"` .

## <a name="before-you-begin"></a>Než začnete

Ujistěte se, že jste nastavili hostitele Azure bastionu pro virtuální síť, ve které se virtuální počítač nachází. Další informace najdete v tématu [Vytvoření hostitele Azure bastionu](bastion-create-host-portal.md). Jakmile se služba bastionu zřídí a nasadí ve vaší virtuální síti, můžete ji použít pro připojení k libovolnému virtuálnímu počítači v této virtuální síti. 

Když použijete bastionu k připojení, předpokládá se, že používáte protokol RDP pro připojení k virtuálnímu počítači s Windows, a SSH pro připojení k virtuálním počítačům Linux. Informace o připojení k virtuálnímu počítači s Windows najdete v tématu [připojení k virtuálnímu počítači s](bastion-connect-vm-rdp.md)Windows.

### <a name="required-roles"></a>Požadované role

Aby bylo možné vytvořit připojení, jsou vyžadovány následující role:

* Role čtenář na virtuálním počítači
* Role čtecího zařízení na síťové kartě s privátní IP adresou virtuálního počítače
* Role čtenář v prostředku Azure bastionu

### <a name="ports"></a>Porty

Aby bylo možné se připojit k virtuálnímu počítači se systémem Linux přes SSH, je nutné, abyste na svém VIRTUÁLNÍm počítači otevřeli následující porty:

* Port pro příchozí spojení: SSH (22)

## <a name="connect-using-username-and-password"></a><a name="username"></a>Připojení: použití uživatelského jména a hesla

1. Otevřete [Azure Portal](https://portal.azure.com). Přejděte k virtuálnímu počítači, ke kterému se chcete připojit, a pak klikněte na **připojit** a v rozevíracím seznamu vyberte **bastionu** .

   ![Připojit](./media/bastion-connect-vm-ssh/connect.png)
1. Po kliknutí na bastionu se zobrazí postranní panel se třemi kartami – RDP, SSH a bastionu. Pokud byl pro virtuální síť zřízen bastionu, karta bastionu je ve výchozím nastavení aktivní. Pokud jste nezřídili bastionu pro virtuální síť, přečtěte si téma [Konfigurace bastionu](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Zadejte uživatelské jméno a heslo pro SSH k virtuálnímu počítači.
1. Po zadání klíče klikněte na tlačítko **připojit** .

## <a name="connect-manually-enter-a-private-key"></a><a name="privatekey"></a>Connect: Ruční zadání privátního klíče

1. Otevřete [Azure Portal](https://portal.azure.com). Přejděte k virtuálnímu počítači, ke kterému se chcete připojit, a pak klikněte na **připojit** a v rozevíracím seznamu vyberte **bastionu** .

   ![Připojit](./media/bastion-connect-vm-ssh/connect.png)
1. Po kliknutí na bastionu se zobrazí postranní panel se třemi kartami – RDP, SSH a bastionu. Pokud byl pro virtuální síť zřízen bastionu, karta bastionu je ve výchozím nastavení aktivní. Pokud jste nezřídili bastionu pro virtuální síť, přečtěte si téma [Konfigurace bastionu](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Zadejte uživatelské jméno a vyberte **privátní klíč SSH**.
1. Zadejte privátní klíč do textové oblasti **SSH privátního klíče** (nebo ho vložte přímo).
1. Po zadání klíče klikněte na tlačítko **připojit** .

## <a name="connect-using-a-private-key-file"></a><a name="ssh"></a>Připojit: použití souboru privátního klíče

1. Otevřete [Azure Portal](https://portal.azure.com). Přejděte k virtuálnímu počítači, ke kterému se chcete připojit, a pak klikněte na **připojit** a v rozevíracím seznamu vyberte **bastionu** .

   ![Připojit](./media/bastion-connect-vm-ssh/connect.png)
1. Po kliknutí na bastionu se zobrazí postranní panel se třemi kartami – RDP, SSH a bastionu. Pokud byl pro virtuální síť zřízen bastionu, karta bastionu je ve výchozím nastavení aktivní. Pokud jste nezřídili bastionu pro virtuální síť, přečtěte si téma [Konfigurace bastionu](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Zadejte uživatelské jméno a vyberte **privátní klíč SSH z místního souboru**.
1. Klikněte na tlačítko **Procházet** (ikona složky v místním souboru).
1. Vyhledejte soubor a pak klikněte na **otevřít**.
1. Kliknutím na **připojit** se připojte k virtuálnímu počítači. Po kliknutí na připojit se SSH k tomuto virtuálnímu počítači přímo otevře v Azure Portal. Toto připojení je přes HTML5 pomocí portu 443 ve službě bastionu přes soukromou IP adresu vašeho virtuálního počítače.

## <a name="next-steps"></a>Další kroky

Přečtěte si [Nejčastější dotazy k bastionu](bastion-faq.md)
