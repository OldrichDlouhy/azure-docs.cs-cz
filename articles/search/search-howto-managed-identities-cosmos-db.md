---
title: Nastavení připojení k účtu Cosmos DB pomocí spravované identity (Preview)
titleSuffix: Azure Cognitive Search
description: Naučte se, jak nastavit připojení indexeru k účtu Cosmos DB pomocí spravované identity (Preview).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 107cd113645a2cbd4b452f9350fa67d734ee6df8
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86143648"
---
# <a name="set-up-an-indexer-connection-to-a-cosmos-db-database-using-a-managed-identity-preview"></a>Nastavení připojení indexeru k databázi Cosmos DB pomocí spravované identity (Preview)

> [!IMPORTANT] 
> Podpora nastavení připojení ke zdroji dat pomocí spravované identity je aktuálně v ověřované veřejné verzi Preview. Funkce Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy.
> Vyplněním [tohoto formuláře](https://aka.ms/azure-cognitive-search/mi-preview-request)můžete požádat o přístup k verzi Preview.

Tato stránka popisuje, jak nastavit připojení indexeru k databázi Azure Cosmos DB pomocí spravované identity namísto zadání přihlašovacích údajů do připojovacího řetězce objektu zdroje dat.

Než se dozvíte víc o této funkci, doporučujeme vám pochopit, co indexer je a jak nastavit indexer pro zdroj dat. Další informace najdete na následujících odkazech:
* [Přehled indexeru](search-indexer-overview.md)
* [Indexer pro Azure Cosmos DB](search-howto-index-cosmosdb.md)

## <a name="set-up-a-connection-using-a-managed-identity"></a>Nastavení připojení pomocí spravované identity

### <a name="1---turn-on-system-assigned-managed-identity"></a>1 – zapnout spravovanou identitu přiřazenou systémem

Když je povolená spravovaná identita přiřazená systémem, Azure vytvoří identitu pro vaši vyhledávací službu, která se dá použít k ověření dalších služeb Azure v rámci stejného tenanta a předplatného. Tuto identitu pak můžete použít v přiřazeních řízení přístupu na základě role (RBAC), které umožňuje přístup k datům během indexování.

![Zapnout spravovanou identitu přiřazenou systémem](./media/search-managed-identities/turn-on-system-assigned-identity.png "Zapnout spravovanou identitu přiřazenou systémem")

Po výběru možnosti **Uložit** se zobrazí ID objektu, které bylo přiřazeno k vaší vyhledávací službě.

![ID objektu](./media/search-managed-identities/system-assigned-identity-object-id.png "ID objektu")
 
### <a name="2---add-a-role-assignment"></a>2. přidání přiřazení role

V tomto kroku udělíte službě Azure Kognitivní hledání oprávnění ke čtení dat z databáze Cosmos DB.

1. V Azure Portal přejděte na účet Cosmos DB obsahující data, která chcete indexovat.
2. Výběr **řízení přístupu (IAM)**
3. Vyberte **Přidat** a pak **Přidat přiřazení role** .

    ![Přidat přiřazení role](./media/search-managed-identities/add-role-assignment-cosmos-db.png "Přidat přiřazení role")

4. Vyberte **roli Čtenář účtu Cosmos DB** .
5. Ponechat **přiřazení přístupu k** **uživateli, skupině nebo instančnímu objektu Azure AD**
6. Vyhledejte vyhledávací službu, vyberte ji a pak vyberte **Uložit** .

    ![Přidat přiřazení čtecího zařízení a role přístup k datům](./media/search-managed-identities/add-role-assignment-cosmos-db-account-reader-role.png "Přidat přiřazení čtecího zařízení a role přístup k datům")

### <a name="3---create-the-data-source"></a>3. vytvoření zdroje dat

**Zdroj dat** určuje data, která mají být indexována, pověření a zásady pro identifikaci změn v datech (například upravené nebo odstraněné dokumenty v kolekci). Zdroj dat je definován jako nezávislý prostředek, aby jej bylo možné použít více indexery.

Při ověřování zdroje dat pomocí spravovaných identit nebudou **přihlašovací údaje** obsahovat klíč účtu.

Příklad vytvoření Cosmos DB objekt zdroje dat pomocí [REST API](https://docs.microsoft.com/rest/api/searchservice/create-data-source):

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [Search service admin key]

{
    "name": "cosmos-db-datasource",
    "type": "cosmosdb",
    "credentials": {
        "connectionString": "Database=sql-test-db;ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/cosmos-db-resource-group/providers/Microsoft.DocumentDB/databaseAccounts/my-cosmos-db-account/;"
    },
    "container": { "name": "myCollection", "query": null },
    "dataChangeDetectionPolicy": {
        "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName": "_ts"
    }
}
```    

Tělo požadavku obsahuje definici zdroje dat, která by měla obsahovat následující pole:

| Pole   | Popis |
|---------|-------------|
| **Jméno** | Povinná hodnota. Vyberte libovolný název, který bude představovat váš objekt zdroje dat. |
|**textový**| Povinná hodnota. Musí být `cosmosdb` . |
|**přihlašovací údaje** | Povinná hodnota. <br/><br/>Při připojování pomocí spravované identity by měl mít formát **pověření** : *Database = [název databáze]; ResourceId = [Resource-ID-String];(ApiKind = [typ rozhraní API];)*<br/> <br/>Formát ResourceId: *ResourceID =/Subscriptions/**ID vašeho předplatného**/resourceGroups/**název vaší skupiny prostředků**/Providers/Microsoft.DocumentDB/databaseAccounts/**název vašeho účtu služby Cosmos DB**/;*<br/><br/>V případě kolekcí SQL nevyžaduje připojovací řetězec ApiKind.<br/><br/>Pro kolekce MongoDB přidejte **ApiKind = MongoDB** do připojovacího řetězce. <br/><br/>V případě grafů Gremlin a tabulek Cassandra si zaregistrujte si ve [verzi Preview služby gated indexer](https://aka.ms/azure-cognitive-search/indexer-preview) , abyste získali přístup k verzi Preview a informace o tom, jak tato pověření naformátovat.<br/>|
| **vnitřního** | Obsahuje následující prvky: <br/>**název**: povinné. Zadejte ID kolekce databází, která se má indexovat.<br/>**dotaz**: volitelné. Můžete zadat dotaz pro sloučení libovolného dokumentu JSON do plochého schématu, které může Azure Kognitivní hledání indexovat.<br/>Pro rozhraní MongoDB API, rozhraní Gremlin API a rozhraní API Cassandra se dotazy nepodporují. |
| **dataChangeDetectionPolicy** | Doporučeno |
|**dataDeletionDetectionPolicy** | Volitelné |

Azure Portal a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) podporují také připojovací řetězec spravovaných identit. Azure Portal vyžaduje příznak funkce, který vám poskytne při registraci ve verzi Preview pomocí odkazu v horní části této stránky. 

### <a name="4---create-the-index"></a>4. vytvoření indexu

Index určuje pole v dokumentu, atributech a dalších konstrukcích, které prohledají možnosti vyhledávání.

Tady je postup vytvoření indexu s prohledávatelným `booktitle` polem:

```
POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "my-target-index",
    "fields": [
    { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    { "name": "booktitle", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
    ]
}
```

Další informace o vytváření indexů najdete v tématu [vytvoření indexu](https://docs.microsoft.com/rest/api/searchservice/create-index) .

### <a name="5---create-the-indexer"></a>5. vytvoření indexeru

Indexer připojuje zdroj dat k cílovému vyhledávacímu indexu a poskytuje plán pro automatizaci aktualizace dat.

Po vytvoření indexu a zdroje dat jste připraveni vytvořit indexer.

Příklad definice indexeru:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "cosmos-db-indexer",
      "dataSourceName" : "cosmos-db-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Tento indexer se spustí každé dvě hodiny (časový interval je nastaven na "PT2H"). Pokud chcete indexer spustit každých 30 minut, nastavte interval na "PT30M". Nejkratší podporovaný interval je 5 minut. Plán je nepovinný – Pokud je vynechaný, indexer se při vytvoření spustí jenom jednou. Můžete ale kdykoli spustit indexer na vyžádání.   

Další informace o rozhraní API Create indexeru najdete v části [Vytvoření indexeru](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Další informace o definování plánů indexerů najdete v tématu [postup plánování indexerů pro Azure kognitivní hledání](search-howto-schedule-indexers.md).

## <a name="see-also"></a>Viz také

Další informace o Cosmos DB indexerech:
* [Indexer pro Azure Cosmos DB](search-howto-index-cosmosdb.md)
