---
title: Správa pravidel brány firewall – Azure CLI – Azure Database for MariaDB
description: Tento článek popisuje, jak vytvořit a spravovat Azure Database for MariaDB pravidla brány firewall pomocí příkazového řádku Azure CLI.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: ae15b98f593dc71c8ed63e867f224aad93f13a43
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86101320"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-cli"></a>Vytváření a Správa Azure Database for MariaDB pravidel brány firewall pomocí Azure CLI
Pravidla brány firewall na úrovni serveru se dají použít ke správě přístupu k serveru Azure Database for MariaDB z konkrétní IP adresy nebo rozsahu IP adres. Pomocí pohodlných příkazů rozhraní příkazového řádku Azure můžete vytvářet, aktualizovat, odstraňovat, zobrazovat a zobrazovat pravidla brány firewall pro správu serveru. Přehled Azure Database for MariaDB bran firewall najdete v tématu [pravidla brány firewall pro Azure Database for MariaDB serveru](./concepts-firewall-rules.md).

Pravidla Virtual Network (VNet) je možné použít také k zabezpečení přístupu k serveru. Přečtěte si další informace o [vytváření a správě koncových bodů a pravidel služby Virtual Network pomocí Azure CLI](howto-manage-vnet-cli.md).

## <a name="prerequisites"></a>Požadavky
* [Nainstalovat rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
* [Server a databáze Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md).

## <a name="firewall-rule-commands"></a>Příkazy pravidla brány firewall:
Pomocí příkazu **AZ MariaDB server firewall-Rule** se v Azure CLI použijí pravidla firewallu pro vytvoření, odstranění, vypsání, zobrazení a aktualizaci.

Příkaz
- **vytvořit**: Vytvořte pravidlo brány firewall serveru Azure MariaDB.
- **Odstranit**: odstranění pravidla brány firewall serveru Azure MariaDB.
- **seznam**: vypíše pravidla brány firewall serveru Azure MariaDB.
- **Zobrazit**: zobrazí podrobnosti pravidla brány firewall serveru Azure MariaDB.
- **aktualizace**: aktualizujte pravidlo brány firewall serveru Azure MariaDB.

## <a name="sign-in-to-azure-and-list-your-azure-database-for-mariadb-servers"></a>Přihlaste se k Azure a seznamte se s Azure Database for MariaDB servery.
Pomocí příkazu **AZ Login** připojte Azure CLI k účtu Azure bezpečně.

1. Z příkazového řádku spusťte následující příkaz:
   ```azurecli
   az login
   ```
   Tento příkaz vytvoří výstup kódu pro použití v dalším kroku.

2. Pomocí webového prohlížeče otevřete stránku [https://aka.ms/devicelogin](https://aka.ms/devicelogin) a potom zadejte kód.

3. Na příkazovém řádku se přihlaste pomocí svých přihlašovacích údajů Azure.

4. Po autorizaci přihlašovacích údajů se v konzole vytiskne seznam předplatných. Zkopírujte ID požadovaného předplatného a nastavte aktuální předplatné na použití. Použijte příkaz [AZ Account set](/cli/azure/account#az-account-set) .
   ```azurecli-interactive
   az account set --subscription <your subscription id>
   ```

5. Pokud si nejste jisti názvy, Seznamte se se servery Azure Database for MariaDB pro vaše předplatné a skupinu prostředků. Použijte příkaz [AZ MariaDB server list](/cli/azure/mariadb/server#az-mariadb-server-list) .

   ```azurecli-interactive
   az mariadb server list --resource-group myresourcegroup
   ```

   Poznamenejte si atribut Name v seznamu, ve kterém je třeba určit server MariaDB, na kterém chcete pracovat. V případě potřeby potvrďte podrobnosti tohoto serveru a použijte atribut Name, aby bylo zajištěno, že je správný. Použijte příkaz [AZ MariaDB server show](/cli/azure/mariadb/server#az-mariadb-server-show) .

   ```azurecli-interactive
   az mariadb server show --resource-group myresourcegroup --name mydemoserver
   ```

## <a name="list-firewall-rules-on-azure-database-for-mariadb-server"></a>Výpis pravidel brány firewall na serveru Azure Database for MariaDB 
Pomocí názvu serveru a názvu skupiny prostředků uveďte existující pravidla brány firewall serveru na serveru. Použijte příkaz [AZ MariaDB server firewall list](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-list) .  Všimněte si, že atribut název serveru je zadán v přepínači **--Server** a nikoli v přepínači **--Name** . 
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
Výstup obsahuje seznam pravidel (pokud existuje) ve formátu JSON (ve výchozím nastavení). Můžete použít přepínač **--Output Table** pro výstup výsledků v čitelnějším formátu tabulky.
```azurecli-interactive
az mariadb server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Vytvoření pravidla brány firewall na serveru Azure Database for MariaDB
Pomocí názvu serveru Azure MariaDB a názvu skupiny prostředků vytvořte nové pravidlo brány firewall na serveru. Použijte příkaz [AZ MariaDB server firewall Create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) . Zadejte název pravidla a také počáteční IP adresu a koncovou IP adresu (pro poskytnutí přístupu k rozsahu IP adres) pro dané pravidlo.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Chcete-li umožnit přístup pro jednu IP adresu, zadejte stejnou IP adresu jako počáteční IP adresa a koncovou IP adresu, jak je uvedeno v tomto příkladu.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```

Pokud chcete aplikacím z IP adres Azure umožnit připojení k vašemu Azure Database for MariaDB serveru, zadejte IP adresu 0.0.0.0 jako počáteční IP adresu a koncovou IP adresu, jako v tomto příkladu.
```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mariadb --name "AllowAllWindowsAzureIps" --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Touto možností se brána firewall nakonfiguruje tak, aby povolovala všechna připojení z Azure, včetně připojení z předplatných ostatních zákazníků. Když vyberete tuto možnost, ujistěte se, že vaše přihlašovací a uživatelská oprávnění omezují přístup pouze na autorizované uživatele.
> 

Po úspěšném vytvoření výstupního příkazu pro vytváření se zobrazí podrobnosti o vytvořeném pravidlu brány firewall ve formátu JSON (ve výchozím nastavení). Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

## <a name="update-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Aktualizace pravidla brány firewall na serveru Azure Database for MariaDB 
Pomocí názvu serveru Azure MariaDB a názvu skupiny prostředků aktualizujte stávající pravidlo brány firewall na serveru. Použijte příkaz [AZ MariaDB server firewall Update](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-update) . Zadejte název existujícího pravidla brány firewall jako vstup a také počáteční IP adresy a atributy koncové IP adresy, které chcete aktualizovat.
```azurecli-interactive
az mariadb server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1 --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Po úspěchu příkaz zobrazí výstup pravidla brány firewall, které jste aktualizovali, ve formátu JSON (ve výchozím nastavení). Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

> [!NOTE]
> Pokud pravidlo brány firewall neexistuje, pravidlo se vytvoří pomocí příkazu Update.

## <a name="show-firewall-rule-details-on-azure-database-for-mariadb-server"></a>Zobrazit podrobnosti pravidla brány firewall na serveru Azure Database for MariaDB
Pomocí názvu serveru Azure MariaDB a názvu skupiny prostředků zobrazte existující Podrobnosti pravidla brány firewall ze serveru. Použijte příkaz [AZ MariaDB server firewall show](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-show) . Zadejte název existujícího pravidla brány firewall jako vstup.
```azurecli-interactive
az mariadb server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Po úspěšném provedení příkazu zobrazí výstup příkazu Podrobnosti pravidla brány firewall, které jste zadali, ve formátu JSON (ve výchozím nastavení). Pokud dojde k selhání, výstup zobrazí text chybové zprávy.

## <a name="delete-a-firewall-rule-on-azure-database-for-mariadb-server"></a>Odstranění pravidla brány firewall na serveru Azure Database for MariaDB
Pomocí názvu serveru Azure MariaDB a názvu skupiny prostředků odeberte stávající pravidlo brány firewall ze serveru. Použijte příkaz [AZ MariaDB server firewall Delete](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-delete) . Zadejte název existujícího pravidla brány firewall.
```azurecli-interactive
az mariadb server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name FirewallRule1
```
Po úspěšném provedení neproběhne žádný výstup. Po selhání se text chybové zprávy zobrazí.

## <a name="next-steps"></a>Další kroky
- Další informace o [pravidlech brány firewall serveru Azure Database for MariaDB](./concepts-firewall-rules.md).
- [Vytvářejte a spravujte Azure Database for MariaDB pravidla brány firewall pomocí Azure Portal](./howto-manage-firewall-portal.md).
- [Vytvořením a správou koncových bodů a pravidel služby Virtual Network pomocí rozhraní příkazového řádku Azure](howto-manage-vnet-cli.md)získáte další zabezpečený přístup k serveru.
