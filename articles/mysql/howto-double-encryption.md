---
title: Šifrování v infrastruktuře – Azure Portal-Azure Database for MySQL
description: Naučte se nastavit a spravovat dvojité šifrování infrastruktury pro vaše Azure Database for MySQL.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: d3076f2591718931bdab4dba9510d25fe07b2d02
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118756"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-mysql"></a>Dvojité šifrování infrastruktury pro Azure Database for MySQL

Naučte se, jak nastavit a spravovat dvojité šifrování infrastruktury pro váš Azure Database for MySQL.

## <a name="prerequisites"></a>Požadavky

* Musíte mít předplatné Azure a mít oprávnění správce k tomuto předplatnému.

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---portal"></a>Vytvoření serveru Azure Database for MySQL s využitím šifrování infrastruktury – portál

Pomocí těchto kroků můžete vytvořit Azure Database for MySQL server s šifrováním s dvojitou infrastrukturou v Azure Portal:

1. V levém horním rohu portálu vyberte **vytvořit prostředek** (+).

2. Vyberte **databáze**  >  **Azure Database for MySQL**. Službu můžete vyhledat také zadáním **MySQL** do vyhledávacího pole.

   ![Možnost Azure Database for MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Zadejte základní informace o serveru. Chcete-li nastavit parametr, vyberte **Další nastavení** a povolit zaškrtávací políčko **infrastruktura – dvojité šifrování** .

    ![Výběry Azure Database for MySQL](./media/howto-double-encryption/infrastructure-encryption-selected.png)

4. Vyberte **zkontrolovat + vytvořit** a zřiďte Server.

    ![Souhrn Azure Database for MySQL](./media/howto-double-encryption/infrastructure-encryption-summary.png)

5. Po vytvoření serveru můžete ověřit šifrované šifrování infrastruktury tím, že zkontrolujete stav v okně **datový šifrovací** Server.

    ![Ověřování Azure Database for MySQL](./media/howto-double-encryption/infrastructure-encryption-validation.png)

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---cli"></a>Vytvoření serveru Azure Database for MySQL s použitím šifrování infrastruktury – rozhraní příkazového řádku

Pomocí těchto kroků vytvořte Azure Database for MySQL server s dvojitým šifrováním infrastruktury z CLI:

Tento příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v `westus` umístění.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Následující příklad vytvoří server MySQL 5.7 v umístění USA – západ, `mydemoserver` ve vaší skupině prostředků `myresourcegroup` a s přihlašovacím jménem správce serveru `myadmin`. Toto je **pro obecné účely** server **Gen 4** s **2 virtuální jádra**. Tím se taky povolí dvojité šifrování infrastruktury pro vytvořený server. Nahraďte položku `<server_admin_password>` vlastní hodnotou.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 5.7 --infrastructure-encryption <Enabled/Disabled>
```

## <a name="next-steps"></a>Další kroky

 Další informace o šifrování dat najdete v tématu [Azure Database for MySQL dvojité šifrování infrastruktury dat](concepts-Infrastructure-double-encryption.md).
