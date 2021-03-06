---
title: Delegování podsítě na Azure NetApp Files | Microsoft Docs
description: Popisuje, jak delegovat podsíť na Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/04/2020
ms.author: b-juche
ms.openlocfilehash: 713a72b0a406d2038d56dc6fcc41e169d02c54eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483614"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Delegování podsítě do Azure NetApp Files 

Je nutné delegovat podsíť na Azure NetApp Files.   Při vytváření svazku je nutné zadat delegovanou podsíť.

## <a name="considerations"></a>Důležité informace
* Průvodce pro vytvoření nové podsítě je ve výchozím nastavení maska sítě/24, která poskytuje 251 dostupných IP adres. Pro službu je dostačující použití masky sítě/28, která poskytuje 16 použitelných IP adres.
* V každé službě Azure Virtual Network (VNet) je možné delegovat Azure NetApp Files jenom jednu podsíť.   
   Azure umožňuje vytvořit více delegovaných podsítí ve virtuální síti.  Pokud ale použijete víc než jednu delegovanou podsíť, všechny pokusy o vytvoření nového svazku selžou.  
   Ve virtuální síti může být jenom jedna delegovaná podsíť. Účet NetApp může nasazovat svazky do více virtuální sítě, z nichž každá má vlastní delegovanou podsíť.  
* V delegované podsíti nelze určit skupinu zabezpečení sítě ani koncový bod služby. Tím dojde k selhání delegování podsítě.
* Přístup k svazku z globálně partnerské virtuální sítě se momentálně nepodporuje.
* Vytváření [uživatelem definovaných vlastních tras](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes) na podsítích virtuálních počítačů s předponou adresy (cíl) do podsítě delegované pro Azure NetApp Files není podporováno. Tím dojde k ovlivnění připojení virtuálního počítače.

## <a name="steps"></a>Kroky 
1.  V okně **virtuální sítě** v Azure Portal vyberte virtuální síť, kterou chcete použít pro Azure NetApp Files.    

1. V okně virtuální síť vyberte **podsítě** a klikněte na tlačítko **+ podsíť** . 

1. Vytvořte novou podsíť, která se bude používat pro Azure NetApp Files, a to tak, že na stránce Přidat podsíť dokončíte následující povinná pole:
    * **Název**: zadejte název podsítě.
    * **Rozsah adres**: zadejte rozsah IP adres.
    * **Delegování podsítě**: vyberte **Microsoft. NetApp/svazky**. 

      ![Delegování podsítě](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Můžete také vytvořit a delegovat podsíť při [vytváření svazku pro Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Další kroky  
* [Vytvoření svazku pro Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Informace o integraci virtuální sítě pro služby Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


