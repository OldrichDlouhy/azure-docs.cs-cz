---
title: Restart serveru – Azure Portal-Azure Database for MariaDB
description: Tento článek popisuje, jak můžete restartovat server Azure Database for MariaDB pomocí webu Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: 369d19d98946f8309c7f2053f4453e09a7ed902f
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121057"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Restartování serveru Azure Database for MariaDB pomocí Azure Portal
Toto téma popisuje, jak můžete restartovat server Azure Database for MariaDB. Možná budete muset restartovat server z důvodů údržby, což způsobí krátký výpadek, protože server tuto operaci provede.

Pokud je služba zaneprázdněná, restart serveru se zablokuje. Například služba může zpracovávat dříve požadovanou operaci, jako je například škálování virtuální jádra.

Čas potřebný k dokončení restartování závisí na procesu obnovení MariaDB. Chcete-li zkrátit dobu restartování, doporučujeme, abyste minimalizovali množství aktivity, ke kterým došlo na serveru před restartováním.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto průvodce budete potřebovat:
- [Server Azure Database for MariaDB](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Provést restart serveru

Následující kroky restartují Server MariaDB:

1. V Azure Portal vyberte server Azure Database for MariaDB.

2. Na panelu nástrojů na stránce **Přehled** serveru klikněte na **restartovat**.

   ![Azure Database for MariaDB – přehled – tlačítko restartovat](./media/howto-restart-server-portal/2-server.png)

3. Kliknutím na **Ano** potvrďte restartování serveru.

   ![Azure Database for MariaDB – potvrzení restartování](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Všimněte si, že se stav serveru změní na restart.

   ![Azure Database for MariaDB – stav restartování](./media/howto-restart-server-portal/4-restarting-status.png)

5. Potvrzení restartování serveru bylo úspěšné.

   ![Azure Database for MariaDB – úspěšné restartování](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Další kroky

[Rychlý Start: vytvoření serveru Azure Database for MariaDB pomocí Azure Portal](./quickstart-create-mariadb-server-database-using-azure-portal.md)