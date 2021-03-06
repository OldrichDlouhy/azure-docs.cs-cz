---
title: zahrnout soubor
description: zahrnout soubor
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9648f4c16e8d266bbdd504d4a7599b67a5c846b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "73522551"
---
1. V nabídce [Azure Portal](https://portal.azure.com) vyberte **vytvořit prostředek**. 

   ![Vytvoření prostředku v Azure Portal](./media/vpn-gateway-add-local-network-gateway-portal-include/azure-portal-create-resource.png)
2. Do pole **Hledat na Marketplace** zadejte **Brána místní sítě**a potom stiskněte klávesu **ENTER** pro hledání. Zobrazí se seznam výsledků. Klikněte na **Brána místní sítě** a pak kliknutím na **Vytvořit** otevřete stránku **Vytvořit bránu místní sítě**.

   ![Vytvoření brány místní sítě](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Vytvoření brány místní sítě")

3. Na stránce **Vytvořit bránu místní sítě** zadejte hodnoty brány místní sítě.

   - **Název**: Zadejte název objektu brány místní sítě.
   - **IP adresa**: Toto je veřejná IP adresa zařízení VPN, ke kterému se má Azure připojit. Zadejte platnou veřejnou IP adresu. Pokud momentálně nemáte IP adresu, můžete použít hodnoty uvedené v příkladu, ale budete se muset vrátit zpět a nahradit zástupnou IP adresu veřejnou IP adresou svého zařízení VPN. V opačném případe se Azure nebude moci připojit.
   - **Adresní prostor** odkazuje na rozsahy adres sítě, kterou tato místní síť představuje. Můžete přidat více různých rozsahů adres. Zkontrolujte, že se zadané rozsahy nepřekrývají s rozsahy jiných sítí, ke kterým se budete chtít připojit. Azure bude směrovat zadaný rozsah adres na místní IP adresu zařízení VPN. *Pokud se chcete připojit ke své místní lokalitě, použijte tady vlastní hodnoty, a ne hodnoty uvedené v příkladu*.
   - **Konfigurovat nastavení protokolu BGP:** Používejte jenom při konfiguraci BGP. V jiných případech tuto možnost nevybírejte.
   - **Předplatné**: Zkontrolujte, že se zobrazuje správné předplatné.
   - **Skupina prostředků**: Vyberte skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili.
   - **Umístění:** Umístění je stejné jako **oblast** v jiném nastavení. Vyberte umístění, ve kterém bude tento objekt vytvořen. Můžete vybrat stejné umístění, ve kterém se nachází vaše virtuální síť, ale není to povinné.

4. Až zadáte všechny hodnoty, klikněte na tlačítko **Vytvořit** v dolní části stránky a vytvořte bránu místní sítě.
