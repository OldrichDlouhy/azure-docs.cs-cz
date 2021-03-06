---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: b2dec95e0258933b50d4437f1cb317639b62883d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "67175050"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Upgradujte SharePoint 2010 na SharePoint 2013 a pak nainstalujte adaptér StorSomple pro SharePoint.
> [!IMPORTANT]
> Všechny soubory, které byly dříve přesunuty do externího úložiště prostřednictvím kódu RBS, nebudou k dispozici, dokud nebude upgrade dokončen a funkce RBS bude znovu povolena. Chcete-li omezit dopad na uživatele, proveďte upgrade nebo přeinstalaci během plánovaného časového období údržby.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Upgrade SharePoint 2010 na SharePoint 2013 a pak nainstalujte adaptér.
1. V serverové farmě SharePoint 2010 si poznamenejte cestu k úložišti objektů BLOB pro externě uložené objekty BLOB a databáze obsahu, pro které je kód RBS povolený. 
2. Nainstalujte a nakonfigurujte novou farmu SharePoint 2013. 
3. Přesuňte databáze, aplikace a kolekce webů z farmy služby SharePoint 2010 na novou farmu služby SharePoint 2013. Pokyny najdete v tématu [Přehled procesu upgradu na SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Nainstalujte adaptér StorSimple pro službu SharePoint na novou farmu. Postup najdete v části [Instalace adaptéru StorSimple pro službu SharePoint](#install-the-storsimple-adapter-for-sharepoint) .
5. Pomocí informací, které jste si poznamenali v kroku 1, povolte RBS pro stejnou sadu databází obsahu a poskytněte stejnou cestu k úložišti objektů BLOB, která byla použita v instalaci SharePoint 2010. Pro postupy použijte [konfiguraci RBS](#configure-rbs) . Po dokončení tohoto kroku by měly být dříve externě dostupné soubory z nové farmy. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade adaptéru StorSimple pro SharePoint
> [!IMPORTANT]
> Měli byste naplánovat, aby se tento upgrade nacházet během plánovaného časového období údržby z následujících důvodů:
> 
> * Dříve externě vydaný obsah nebude k dispozici, dokud adaptér nebude znovu nainstalován.
> * Veškerý obsah nahraný na lokalitu po odinstalaci předchozí verze adaptéru StorSimple pro službu SharePoint, ale před instalací nové verze, bude uložen v databázi obsahu. Po instalaci nového adaptéru budete muset tento obsah přesunout do zařízení StorSimple. `RBS Migrate()`K migraci obsahu můžete použít rutinu prostředí Microsoft PowerShell, která je součástí SharePointu. Další informace najdete v tématu [migrace obsahu do RBS nebo](https://technet.microsoft.com/library/ff628255.aspx)z něj. 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>Upgrade adaptéru StorSimple pro službu SharePoint
1. Odinstalujte předchozí verzi adaptéru StorSimple pro službu SharePoint.
   
   > [!NOTE]
   > Tato akce automaticky zakáže RBS v databázích obsahu. Existující objekty blob zůstanou ale na zařízení StorSimple. Vzhledem k tomu, že je kód RBS zakázaný a objekty BLOB se nemigrují zpátky do databází obsahu, nebudou všechny požadavky na tyto objekty blob úspěšné. 
   > 
   > 
2. Nainstalujte nový adaptér StorSimple pro službu SharePoint. Nový adaptér automaticky rozpoznává databáze obsahu, které byly dříve povoleny nebo zakázány pro RBS, a použije předchozí nastavení.

