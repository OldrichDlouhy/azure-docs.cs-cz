---
title: Řešení potíží s atributem, který se nesynchronizuje v Azure AD Connect | Microsoft Docs
description: V tomto tématu najdete postup řešení potíží se synchronizací atributů pomocí úlohy řešení potíží.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a16e989a6da8daa4a290c7eaa4363eef09c9749
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85356334"
---
# <a name="troubleshoot-an-attribute-not-synchronizing-in-azure-ad-connect"></a>Řešení potíží s atributem, který se nesynchronizuje v Azure AD Connect

## <a name="recommended-steps"></a>**Doporučené kroky**

Než prozkoumáte problémy se synchronizací atributů, Podívejme se na **Azure AD Connect** proces synchronizace:

  ![Proces synchronizace Azure AD Connect](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminologie**

* **Cs:** Místo konektoru, tabulka v databázi.
* **MV:** Metaverse, tabulka v databázi.
* **Služba AD:** Služba Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Kroky synchronizace**

* Import ze služby AD: objekty služby Active Directory se přenesou do služby AD CS.

* Import z AAD: objekty Azure Active Directory se přenesou do služby AAD CS.

* Synchronizace: **pravidla pro příchozí synchronizaci** a **pravidla odchozí synchronizace** se spouštějí v pořadí podle priority od nižších po vyšší. Chcete-li zobrazit pravidla synchronizace, můžete přejít na **Editor synchronizačních pravidel** z aplikací klasické pracovní plochy. **Pravidla pro příchozí synchronizaci** přináší data z cs do MV. **Pravidla odchozí synchronizace** přesouvá data z MV do cs.

* Export do AD: po spuštění synchronizace se objekty exportují ze služby AD CS do **Active Directory**.

* Export do AAD: po spuštění synchronizace se objekty exportují z AAD CS do **Azure Active Directory**.

### <a name="step-by-step-investigation"></a>**Krok za krokem šetření**

* Začneme hledat v **úložišti Metaverse** a podíváme se na mapování atributů ze zdroje do cíle.

* Spusťte **Synchronization Service Manager** z aplikací klasické pracovní plochy, jak je znázorněno níže:

  ![Spustit Synchronization Service Manager](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/startmenu.png)

* V **Synchronization Service Manager**vyberte **hledání Metaverse**, vyberte **Rozsah podle typu objektu**, vyberte objekt pomocí atributu a klikněte na tlačítko **Hledat** .

  ![Hledání v úložišti Metaverse](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvsearch.png)

* Dvojím kliknutím na objekt, který najdete v **úložišti Metaverse** , zobrazte všechny jeho atributy. Kliknutím na kartu **konektory** se můžete podívat na odpovídající objekt ve všech **prostorech konektoru**.

  ![Konektory objektů úložiště metaverse](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvattributes.png)

* Dvojitým kliknutím na **konektor služby Active Directory** zobrazíte atributy **prostoru konektoru** . Klikněte na tlačítko **Náhled** . v následujícím dialogovém okně klikněte na tlačítko **vygenerovat náhled** .

  ![Atributy prostoru konektoru](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/csattributes.png)

* Nyní klikněte na **tok importovaného atributu**, který ukazuje tok atributů z **prostoru konektoru služby Active Directory do úložiště** **Metaverse**. Sloupec **pravidlo synchronizace** zobrazuje, které **pravidlo synchronizace** přispělo k tomuto atributu. Sloupec **zdroj dat** zobrazuje atributy z **prostoru konektoru**. Sloupec **atributu Metaverse** zobrazuje atributy v **úložišti Metaverse**. Zde můžete vyhledat atribut, který zde není synchronizován. Pokud zde atribut nenajdete, není to namapováno a je třeba vytvořit nové vlastní **synchronizační pravidlo** pro mapování atributu.

  ![Atributy prostoru konektoru](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/cstomvattributeflow.png)

* Kliknutím na **tok atributů exportu** v levém podokně zobrazíte tok atributů z úložiště **Metaverse** zpátky do **prostoru konektoru služby Active Directory** pomocí **pravidel odchozí synchronizace**.

  ![Atributy prostoru konektoru](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvtocsattributeflow.png)

* Podobně můžete zobrazit objekt **prostoru konektoru Azure Active Directory** a můžete vygenerovat **Náhled** pro zobrazení toku atributů z úložiště **Metaverse** do **prostoru konektoru** a naopak, což vám umožní zjistit, proč se atribut nesynchronizuje.

## <a name="recommended-documents"></a>**Doporučené dokumenty**
* [Azure AD Connect synchronizace: technické koncepty](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect synchronizace: Principy architektury](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect synchronizace: principy deklarativního zřizování](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect synchronizace: principy deklarativních zřizovacích výrazů](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Synchronizace služby Azure AD Connect: Principy výchozí konfigurace](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect synchronizace: principy uživatelů, skupin a kontaktů](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect Sync: atributy stínů](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Další kroky

- [Azure AD Connect synchronizace](how-to-connect-sync-whatis.md).
- [Co je hybridní identita?](whatis-hybrid-identity.md).
