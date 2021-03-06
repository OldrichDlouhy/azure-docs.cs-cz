---
title: 'Rychlý Start: vytvoření zóny a záznamu DNS – Azure Portal'
titleSuffix: Azure DNS
description: Pomocí tohoto podrobného průvodce rychlým startem se dozvíte, jak vytvořit zónu Azure DNS a záznam pomocí Azure Portal.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: quickstart
ms.date: 3/11/2019
ms.author: rohink
ms.openlocfilehash: 3b34b45cba805d748dcc8a0c31340cd8116e1c80
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82160762"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-the-azure-portal"></a>Rychlý Start: vytvoření zóny Azure DNS a záznamu pomocí Azure Portal

Azure DNS můžete nakonfigurovat k překladu názvů hostitelů ve veřejné doméně. Pokud jste například koupili název domény *contoso. xyz* z registrátora názvu domény, můžete nakonfigurovat Azure DNS pro hostování domény *contoso. xyz* a přeložit *`www.contoso.xyz`* na IP adresu vašeho webového serveru nebo webové aplikace.

V tomto rychlém startu vytvoříte testovací doménu a pak vytvoříte záznam adresy pro překlad *webové* služby na IP adresu *10.10.10.10*.

>[!IMPORTANT]
>Všechny názvy a IP adresy v tomto rychlém startu jsou příklady, které nepředstavují scénáře reálného světa.

<!---
You can also perform these steps using [Azure PowerShell](dns-getstarted-powershell.md) or the cross-platform [Azure CLI](dns-getstarted-cli.md).
--->

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Pro všechny kroky portálu se přihlaste k [Azure Portal](https://portal.azure.com).

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS obsahuje záznamy DNS pro doménu. Pokud chcete začít hostovat vaši doménu v Azure DNS, vytvořte pro tento název domény zónu DNS. 

**Postup vytvoření zóny DNS:**

1. V levém horním rohu vyberte **vytvořit prostředek**, pak **sítě**a **zónu DNS**.

1. Na stránce **vytvořit ZÓNU DNS** zadejte nebo vyberte následující hodnoty:

   - **Název**: zadejte *contoso. xyz* pro tento příklad rychlého startu. Název zóny DNS může být libovolná hodnota, která ještě není nakonfigurovaná na Azure DNSch serverech. Ve skutečné situaci byste použili doménu zakoupenou od registrátora názvu domény.
   - **Skupina prostředků**: vyberte **vytvořit novou**, zadejte *MyResourceGroup*a vyberte **OK**. Název skupiny prostředků musí být v rámci předplatného Azure jedinečný. 

1. Vyberte **Vytvořit**.

   ![Zóna DNS](./media/dns-getstarted-portal/openzone650.png)

Vytvoření zóny může trvat několik minut.

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Položky DNS nebo záznamy pro vaši doménu vytvoříte v zóně DNS. Vytvořte nový záznam adresy nebo záznam A k překladu názvu hostitele na adresu IPv4.

**Vytvoření záznamu A:**

1. V Azure Portal v části **všechny prostředky**ve skupině prostředků **MyResourceGroup** otevřete zónu DNS **contoso. xyz** . V poli **filtrovat podle názvu** můžete zadat *contoso. xyz* , aby bylo snazší ho najít.

1. V horní části stránky **zóny DNS** vyberte **+ Sada záznamů**.

1. Na stránce **Přidat sadu záznamů** zadejte nebo vyberte následující hodnoty:

   - **Název**: zadejte text *www*. Název záznamu je název hostitele, který chcete přeložit na zadanou IP adresu.
   - **Typ** **: vyberte.** Záznamy a jsou nejběžnější, ale existují i jiné typy záznamů pro poštovní servery (MX), adresy IP V6 ("AAAA") atd. 
   - **TTL**: typ *1*. *Doba do živého* požadavku DNS určuje, jak dlouhé servery a klienti DNS můžou odpověď ukládat do mezipaměti.
   - **Jednotka TTL**: vyberte **hodiny**. Toto je časová jednotka pro hodnotu **TTL** . 
   - **IP adresa**: pro tento příklad rychlého startu zadejte *10.10.10.10*. Tato hodnota je IP adresa, na kterou se název záznamu překládá. Ve scénáři reálného světa byste zadali veřejnou IP adresu svého webového serveru.

Vzhledem k tomu, že je tento rychlý Start jenom pro účely rychlého testování, není potřeba konfigurovat Azure DNS názvové servery v registrátoru názvů domén. S skutečnou produkční doménou budete chtít, aby kdokoli na internetu přeložil název hostitele pro připojení k webovému serveru nebo aplikaci. V rámci svého registrátora názvu domény budete moct nahradit záznamy názvového serveru Azure DNS názvové servery. Další informace najdete v tématu [kurz: Hostování vaší domény v Azure DNS](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="test-the-name-resolution"></a>Testování překladu IP adres

Teď, když máte testovací zónu DNS s testovacím záznamem "A", můžete otestovat překlad IP adres pomocí nástroje s názvem *nslookup*. 

**Testování překladu názvů DNS:**

1. V Azure Portal v části **všechny prostředky**ve skupině prostředků **MyResourceGroup** otevřete zónu DNS **contoso. xyz** . V poli **filtrovat podle názvu** můžete zadat *contoso. xyz* , aby bylo snazší ho najít.

1. Zkopírujte název názvového serveru ze seznamu názvového serveru na stránce **Přehled** . 

   ![zóna](./media/dns-getstarted-portal/viewzonens500.png)

1. Otevřete příkazový řádek a spusťte následující příkaz:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Příklad:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Mělo by se zobrazit něco podobného jako na následující obrazovce:

   ![nslookup](media/dns-getstarted-portal/nslookup.PNG)

Název hostitele **www\.contoso. xyz** se přeloží na **10.10.10.10**, stejně jako jste nakonfigurovali. Tento výsledek ověří, že překlad názvů funguje správně. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Když už nepotřebujete prostředky, které jste vytvořili v tomto rychlém startu, odstraňte je odstraněním skupiny prostředků **MyResourceGroup** . Otevřete skupinu prostředků **MyResourceGroup** a vyberte **Odstranit skupinu prostředků**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření záznamů DNS pro webovou aplikaci ve vlastní doméně](./dns-web-sites-custom-domain.md)