---
title: Použití dat služby Azure Blockchain Workbench v Power BI
description: Zjistěte, jak načíst a zobrazit data z databáze SQL DB služby Azure Blockchain Workbench v Power BI.
ms.date: 04/22/2020
ms.topic: how-to
ms.reviewer: sunri
ms.openlocfilehash: 2a5e67a8416c57c1a0cb039733a93608a8919be6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214226"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Použití dat služby Azure Blockchain Workbench v Power BI

Microsoft Power BI poskytuje možnost snadno generovat výkonné sestavy z databází SQL DB pomocí Power BI Desktop a pak je publikovat do [https://www.powerbi.com](https://www.powerbi.com) .

Tento článek obsahuje podrobný návod, jak se připojit k databázi SQL Database služby Azure Blockchain Workbench z Power BI Desktopu, vytvořit sestavu a nasadit ji na stránku powerbi.com.

## <a name="prerequisites"></a>Požadavky

* Stáhněte si [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

## <a name="connecting-power-bi-to-data-in-azure-blockchain-workbench"></a>Připojení Power BI k datům v Azure blockchain Workbench

1.  Otevřete Power BI Desktop.
2.  Vyberte **Načíst data**.

    ![Získat data](./media/data-powerbi/get-data.png)
3.  Ze seznamu typů zdrojů dat vyberte **SQL Server**.

4.  Do dialogového okna zadejte server a název databáze. Určete, jestli chcete data importovat nebo použít **DirectQuery**. Vyberte **OK**.

    ![Výběr SQL Serveru](./media/data-powerbi/select-sql.png)

5.  Zadejte přihlašovací údaje databáze, abyste získali přístup ke službě Azure Blockchain Workbench. Vyberte **Databáze** a zadejte přihlašovací údaje.

    Pokud používáte přihlašovací údaje vytvořené při nasazení služby Azure Blockchain Workbench, pak uživatelským jménem je **dbadmin** a heslo je stejné jako to, které jste při zadali nasazení.

    ![Nastavení databáze SQL DB](./media/data-powerbi/db-settings.png)

6.  Jakmile se připojíte k databázi, dialogové okno **Navigátor** zobrazí tabulky a zobrazení, která jsou v databázi k dispozici. Zobrazení jsou navržená pro generování sestav a mají předponu **vw**.

    ![Navigátor](./media/data-powerbi/navigator.png)

7.  Vyberte zobrazení, které chcete zahrnout. Pro demonstrační účely jsme zahrnuli **vwContractAction**, která poskytuje podrobné informace o akcích, které byly provedeny ve smlouvě.

    ![Výběr zobrazení](./media/data-powerbi/select-views.png)

Nyní můžete v Power BI vytvářet a publikovat sestavy běžným způsobem.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Zobrazení databáze ve službě Azure Blockchain Workbench](database-views.md)