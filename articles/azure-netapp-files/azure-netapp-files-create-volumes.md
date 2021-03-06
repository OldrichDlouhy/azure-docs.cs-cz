---
title: Vytvoření svazku NFS pro Azure NetApp Files | Microsoft Docs
description: Popisuje, jak vytvořit svazek NFS pro Azure NetApp Files.
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
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: 7f14ac279f14feb3f83490ab96965d4355bed125
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87169446"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Vytvoření svazku NFS pro Azure NetApp Files

Azure NetApp Files podporuje systém souborů NFS (NFSv3 a NFSv 4.1) a svazky SMBv3. Spotřeba kapacity svazku se počítá proti zřízené kapacitě příslušného fondu. V tomto článku se dozvíte, jak vytvořit svazek NFS. Pokud chcete vytvořit svazek SMB, přečtěte si téma [vytvoření svazku SMB pro Azure NetApp Files](azure-netapp-files-create-volumes-smb.md). 

## <a name="before-you-begin"></a>Než začnete 
Musíte mít už nastavený fond kapacity.   
[Nastavení fondu kapacity](azure-netapp-files-set-up-capacity-pool.md)   
Podsíť musí být delegovaná na Azure NetApp Files.  
[Delegování podsítě na službu Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="considerations"></a>Požadavky 

* Rozhodnutí o tom, která verze systému souborů NFS se má použít  
  NFSv3 dokáže zvládnout širokou škálu případů použití a je běžně nasazená ve většině podnikových aplikací. Měli byste ověřit, jakou verzi (NFSv3 nebo NFSv 4.1) vaše aplikace vyžaduje, a vytvořit svazek s použitím příslušné verze. Pokud například používáte [Apache ActiveMQ](https://activemq.apache.org/shared-file-system-master-slave), doporučuje se zamykání souborů s nfsv 4.1 nad NFSv3. 

* Zabezpečení  
  Podpora pro režim systému UNIX (čtení, zápis a spouštění) je k dispozici pro NFSv3 a NFSv 4.1. Přístup na úrovni kořenového adresáře se vyžaduje v klientovi NFS, aby bylo možné připojit svazky systému souborů NFS.

* Podpora místního uživatele/skupiny a protokolu LDAP pro NFSv 4.1  
  NFSv 4.1 v současné době podporuje kořenový přístup pouze ke svazkům. Přečtěte si téma [Konfigurace výchozí domény nfsv 4.1 pro Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md). 

## <a name="best-practice"></a>Osvědčený postup

* Měli byste se ujistit, že pro tento svazek používáte správné pokyny pro připojení.  Viz [připojení nebo odpojení svazku pro virtuální počítače se systémem Windows nebo Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md).

* Klient NFS by měl být ve stejné virtuální síti nebo virtuální síti s partnerským vztahem jako svazek Azure NetApp Files. Připojení mimo virtuální síť je podporované; Tím se ale zavádí další latence a sníží se celkový výkon.

* Měli byste zajistit, aby byl klient systému souborů NFS aktuální a aby běžel nejnovější aktualizace operačního systému.

## <a name="create-an-nfs-volume"></a>Vytvoření svazku NFS

1.  V okně fondy kapacit klikněte na okno **svazky** . 

    ![Přejít na svazky](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Kliknutím na **+ Přidat svazek** vytvořte svazek.  
    Zobrazí se okno vytvořit svazek.

3.  V okně vytvořit svazek klikněte na **vytvořit** a zadejte informace pro následující pole:   
    * **Název svazku**      
        Zadejte název svazku, který vytváříte.   

        Název svazku musí být v rámci každého fondu kapacity jedinečný. Musí mít aspoň tři znaky dlouhé. Můžete použít jakékoli alfanumerické znaky.   

        `default`Název svazku nelze použít.

    * **Fond kapacit**  
        Zadejte fond kapacit, ve kterém chcete vytvořit svazek.

    * **Kvóta**  
        Určuje velikost logického úložiště, které je přidělené svazku.  

        Pole **Dostupná kvóta** zobrazuje množství nevyužitého místa ve zvoleném fondu kapacity, které můžete použít k vytvoření nového svazku. Velikost nového svazku nesmí překročit dostupnou kvótu.  

    * **Virtuální síť**  
        Zadejte službu Azure Virtual Network (VNet), ze které chcete získat přístup ke svazku.  

        Virtuální síť, kterou zadáte, musí mít přidělenou podsíť Azure NetApp Files. Služba Azure NetApp Files se dá použít jenom ze stejné virtuální sítě nebo z virtuální sítě, která se nachází ve stejné oblasti jako svazek prostřednictvím partnerského vztahu virtuálních sítí. Ke svazku z místní sítě se můžete dostat i přes Express Route.   

    * **Podsíť**  
        Zadejte podsíť, kterou chcete použít pro svazek.  
        Podsíť, kterou zadáte, musí být delegovaná na Azure NetApp Files. 
        
        Pokud jste nedelegovanou podsíť, můžete na stránce vytvořit svazek kliknout na **vytvořit novou** . Pak na stránce vytvořit podsíť zadejte informace o podsíti a vyberte možnost **Microsoft. NetApp/** Volumes pro delegování podsítě pro Azure NetApp Files. V každé virtuální síti je možné delegovat jenom jednu podsíť na Azure NetApp Files.   
 
        ![Vytvoření svazku](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Vytvoření podsítě](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Pokud chcete pro svazek použít existující zásadu snímku, rozbalte ji kliknutím na **Zobrazit Upřesnit oddíl** a v rozevírací nabídce vyberte zásadu snímku. 

        Informace o vytváření zásad snímku najdete v tématu [Správa snímků](azure-netapp-files-manage-snapshots.md).

        ![Zobrazit rozšířený výběr](../media/azure-netapp-files/volume-create-advanced-selection.png)

4. Klikněte na **protokol**a pak proveďte následující akce:  
    * Jako typ protokolu pro svazek vyberte **systém souborů NFS** .   
    * Zadejte **cestu k souboru** , která bude použita k vytvoření cesty pro export pro nový svazek. Cesta pro export slouží pro připojení svazku a přístup k němu.

        Název cesty k souboru může obsahovat pouze písmena, číslice a pomlčky („-“). Musí být dlouhý 16 až 40 znaků. 

        Cesta k souboru musí být jedinečná v rámci každého předplatného a každé oblasti. 

    * Vyberte verzi systému souborů NFS (**NFSv3** nebo **nfsv 4.1**) pro svazek.  
    * Volitelně můžete [nakonfigurovat zásady exportu pro svazek NFS](azure-netapp-files-configure-export-policy.md).

    ![Zadat protokol NFS](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Kliknutím na tlačítko **zkontrolovat + vytvořit** zkontrolujte podrobnosti o svazku.  Pak kliknutím na **vytvořit** vytvořte svazek NFS.

    Svazek, který jste vytvořili, se zobrazí na stránce svazky. 
 
    Svazek dědí atributy předplatného, skupiny prostředků a umístění z fondu kapacity. Stav nasazení svazku můžete monitorovat na kartě Oznámení.


## <a name="next-steps"></a>Další kroky  

* [Konfigurace výchozí domény NFSv4.1 pro Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Připojení nebo odpojení svazku pro virtuální počítače s Windows nebo Linuxem](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Konfigurace zásad exportu pro svazek NFS](azure-netapp-files-configure-export-policy.md)
* [Omezení prostředků pro službu Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Informace o integraci virtuální sítě pro služby Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
