---
title: Správa pravidel brány firewall – Citus (-Scale) – Azure Database for PostgreSQL
description: Vytváření a Správa pravidel brány firewall pro Azure Database for PostgreSQL – Citus (škálovatelné) pomocí Azure Portal
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 9/12/2019
ms.openlocfilehash: c84616e8a9b9ff9722f5a104175c80c37dbcbcc3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116909"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>Správa pravidel brány firewall pro Azure Database for PostgreSQL – Citus (škálování)
Pravidla brány firewall na úrovni serveru se dají použít ke správě přístupu k uzlu koordinátoru Citus () ze zadané IP adresy nebo rozsahu IP adres.

## <a name="prerequisites"></a>Požadavky
Pokud chcete projít tento průvodce, budete potřebovat:
- Skupina serverů [vytvoří skupinu serverů Azure Database for PostgreSQL – Citus (– škálovatelný rozsah)](quickstart-create-hyperscale-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal

> [!NOTE]
> Tato nastavení jsou k dispozici také při vytváření skupiny serverů Azure Database for PostgreSQL Citus (). Na kartě **síť** klikněte na **veřejný koncový bod**.
> ![Azure Portal – karta síť](./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png)

1. Na stránce skupiny serverů PostgreSQL pod záhlavím zabezpečení kliknutím na **sítě** otevřete pravidla brány firewall.

   ![Azure Portal – kliknutí na sítě](./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png)

2. Klikněte na **Přidat IP adresu klienta**, a to buď na panelu nástrojů (A níže), nebo v odkazu (možnost B). Buď způsob automaticky vytvoří pravidlo brány firewall s veřejnou IP adresou vašeho počítače, jak je znázorněno v systému Azure.

   ![Azure Portal klikněte na Přidat IP adresu klienta.](./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png)

Alternativně můžete kliknutím na **+ Přidat 0.0.0.0-255.255.255.255** (napravo od možnosti B) použít nejen vaši IP adresu, ale celý Internet pro přístup k portu 5432 pro uzel koordinátora. V takové situaci se klienti pořád musí přihlásit se správným uživatelským jménem a heslem, aby mohli cluster používat. Nicméně doporučujeme povolit celosvětový přístup jenom po krátkou dobu a jenom pro neprodukční databáze.

3. Před uložením konfigurace ověřte svoji IP adresu. V některých situacích se IP adresa zjištěná Azure Portal liší od IP adresy používané při přístupu k Internetu a k serverům Azure. Proto může být nutné změnit počáteční IP adresu a koncovou IP adresu, aby funkce pravidla fungovala podle očekávání.
   Použijte vyhledávací modul nebo jiný online nástroj ke kontrole vlastní IP adresy. Vyhledejte například "Co je moje IP adresa".

   ![Vyhledávání Bingu pro co je moje IP adresa](./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Přidejte další rozsahy adres. V pravidlech brány firewall můžete zadat jednu IP adresu nebo rozsah adres. Pokud chcete pravidlo omezit na jednu IP adresu, zadejte do pole stejnou adresu jako počáteční IP adresa a koncová IP adresa. Otevření brány firewall umožňuje správcům, uživatelům a aplikacím přístup k uzlu koordinátora na portu 5432.

5. Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru. Počkejte, až se potvrdí, že aktualizace pravidel firewallu proběhla úspěšně.

## <a name="connecting-from-azure"></a>Připojení z Azure

Existuje snadný způsob, jak udělit přístup k databázi pomocí technologie Hyper-v, která je hostovaná v Azure (například aplikace Azure Web Apps, nebo ty, které běží na virtuálním počítači Azure). Jednoduše nastavte možnost **Povolení služeb a prostředků Azure pro přístup k této skupině serverů** na **Ano** v portálu v podokně **síť** a stiskněte **Uložit**.

> [!IMPORTANT]
> Touto možností se brána firewall nakonfiguruje tak, aby povolovala všechna připojení z Azure, včetně připojení z předplatných ostatních zákazníků. Když vyberete tuto možnost, ujistěte se, že vaše přihlašovací a uživatelská oprávnění omezují přístup pouze na autorizované uživatele.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Správa stávajících pravidel brány firewall na úrovni serveru na webu Azure Portal
Opakováním kroků spravujte pravidla brány firewall.
* Chcete-li přidat aktuální počítač, klikněte na tlačítko a **přidejte IP adresu klienta**. Kliknutím na **Uložit** uložte změny.
* Pro přidání dalších IP adres zadejte Název pravidla, Počáteční IP adresu a Koncovou IP adresu. Kliknutím na **Uložit** uložte změny.
* Pokud chcete upravit stávající pravidlo, klikněte na libovolné pole pravidla a upravte ho. Kliknutím na **Uložit** uložte změny.
* Chcete-li odstranit stávající pravidlo, klikněte na tlačítko se třemi tečkami [...] a odstraňte pravidlo kliknutím na tlačítko **Odstranit** . Kliknutím na **Uložit** uložte změny.

## <a name="next-steps"></a>Další kroky
- Přečtěte si další informace o [konceptu pravidel brány firewall](concepts-hyperscale-firewall-rules.md), včetně toho, jak řešit problémy s připojením.
