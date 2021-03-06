---
title: zahrnout soubor
description: zahrnout soubor
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 01/27/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: a6cfafd41d1632d99b7ca03bce2316ec679d7579
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77061780"
---
1. V novém okně prohlížeče se přihlaste k webu [Azure Portal](https://portal.azure.com/).

2. V nabídce vlevo vyberte **vytvořit prostředek**.
   
   ![Vytvoření prostředku v Azure Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-0.png)
   
3. Na stránce **Nový** vyberte **databáze**  >  **Azure Cosmos DB**.
   
   ![Podokno databází portálu Azure Portal](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)
   
3. Na stránce **vytvořit Azure Cosmos DB účet** zadejte nastavení pro nový účet Azure Cosmos DB. 
 
    Nastavení|Hodnota|Popis
    ---|---|---
    Předplatné|Vaše předplatné|Vyberte předplatné Azure, které chcete pro tento účet služby Azure Cosmos DB použít. 
    Skupina prostředků|Vytvořit nový<br><br>Pak zadejte stejný název jako název účtu.|Vyberte, že chcete **vytvořit novou** IP adresu. Pak zadejte nový název skupiny prostředků pro váš účet. Pro jednoduchost použijte stejný název jako název účtu Azure Cosmos DB. 
    Account Name|Zadejte jedinečný název.|Zadejte jedinečný název, který identifikuje váš účet služby Azure Cosmos DB. Identifikátor URI vašeho účtu se *Gremlin.Azure.com* připojí k jedinečnému názvu účtu.<br><br>Název účtu může obsahovat jenom malá písmena, číslice a spojovníky (-) a musí mít délku 3 až 31 znaků.
    Rozhraní API|Gremlin (graf)|Rozhraní API určuje typ účtu, který se má vytvořit. Azure Cosmos DB poskytuje pět rozhraní API: Core (SQL) pro databáze dokumentů, Gremlin pro databáze grafů, MongoDB pro databáze dokumentů, tabulku Azure a Cassandra. Pro každé rozhraní API musíte vytvořit samostatný účet. <br><br>Vyberte **Gremlin (Graph)**, protože v tomto rychlém startu vytváříte tabulku, která funguje s rozhraním API Gremlin. <br><br>[Přečtěte si další informace o rozhraní Gremlin API](../articles/cosmos-db/graph-introduction.md).|
    Umístění|Vyberte oblast nejbližší vašim uživatelům.|Vyberte zeměpisné umístění, ve kterém chcete účet služby Azure Cosmos DB hostovat. Použijte umístění, které je vašim uživatelům nejbližší, abyste jim zajistili nejrychlejší přístup k datům.

    Vyberte **zkontrolovat + vytvořit**. Můžete přeskočit část **síť** a **značky** . 

    ![Stránka nového účtu pro službu Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. Vytvoření účtu trvá několik minut. Počkejte, až se na portálu zobrazí **blahopřání. Váš účet Azure Cosmos DB byl vytvořen na** stránce.
   
   ![Stránka vytvořená Azure Cosmos DB účtu](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)

