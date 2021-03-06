---
title: Nastavení připojení k účtu úložiště pomocí spravované identity (Preview)
titleSuffix: Azure Cognitive Search
description: Naučte se, jak nastavit připojení indexeru k účtu Azure Storage pomocí spravované identity (Preview).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 073a92f07d17614cb386c5c33a8058af9b59aaea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084071"
---
# <a name="set-up-a-connection-to-an-azure-storage-account-using-a-managed-identity-preview"></a>Nastavení připojení k účtu Azure Storage pomocí spravované identity (Preview)

> [!IMPORTANT] 
> Podpora nastavení připojení ke zdroji dat pomocí spravované identity je aktuálně v ověřované veřejné verzi Preview. Funkce Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy.
> Vyplněním [tohoto formuláře](https://aka.ms/azure-cognitive-search/mi-preview-request)můžete požádat o přístup k verzi Preview.

Tato stránka popisuje, jak nastavit připojení indexeru k účtu služby Azure Storage pomocí spravované identity namísto zadání přihlašovacích údajů do připojovacího řetězce objektu zdroje dat.

Než se dozvíte víc o této funkci, doporučujeme vám pochopit, co indexer je a jak nastavit indexer pro zdroj dat. Další informace najdete na následujících odkazech:
* [Přehled indexeru](search-indexer-overview.md)
* [Indexer Azure Blob](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2 indexer](search-howto-index-azure-data-lake-storage.md)
* [Indexer tabulek Azure](search-howto-indexing-azure-tables.md)

## <a name="set-up-the-connection"></a>Nastavení připojení

### <a name="1---turn-on-system-assigned-managed-identity"></a>1 – zapnout spravovanou identitu přiřazenou systémem

Když je povolená spravovaná identita přiřazená systémem, Azure vytvoří identitu pro vaši vyhledávací službu, která se dá použít k ověření dalších služeb Azure v rámci stejného tenanta a předplatného. Tuto identitu pak můžete použít v přiřazeních řízení přístupu na základě role (RBAC), které umožňuje přístup k datům během indexování.

![Zapnout spravovanou identitu přiřazenou systémem](./media/search-managed-identities/turn-on-system-assigned-identity.png "Zapnout spravovanou identitu přiřazenou systémem")

Po výběru možnosti **Uložit** se zobrazí ID objektu, které bylo přiřazeno k vaší vyhledávací službě.

![ID objektu](./media/search-managed-identities/system-assigned-identity-object-id.png "ID objektu")
 
### <a name="2---add-a-role-assignment"></a>2. přidání přiřazení role

V tomto kroku udělíte službě Azure Kognitivní hledání oprávnění číst data z vašeho účtu úložiště.

1. V Azure Portal přejděte k účtu úložiště, který obsahuje data, která chcete indexovat.
2. Výběr **řízení přístupu (IAM)**
3. Vyberte **Přidat** a pak **Přidat přiřazení role** .

    ![Přidat přiřazení role](./media/search-managed-identities/add-role-assignment-storage.png "Přidat přiřazení role")

4. Vyberte příslušné role na základě typu účtu úložiště, který chcete indexovat:
    1. Úložiště objektů BLOB v Azure vyžaduje, abyste přidali vyhledávací službu do role **čtečky dat objektů BLOB úložiště** .
    1. Azure Data Lake Storage Gen2 vyžaduje, abyste přidali vyhledávací službu do role **čtečky dat objektů BLOB úložiště** .
    1. Azure Table Storage vyžaduje přidání vyhledávací služby do role **Čtenář a přístup k datům** .
5.  Ponechat **přiřazení přístupu k** **uživateli, skupině nebo instančnímu objektu Azure AD**
6.  Vyhledejte vyhledávací službu, vyberte ji a pak vyberte **Uložit** .

    Příklad pro Azure Blob Storage a Azure Data Lake Storage Gen2:

    ![Přidat přiřazení role čtečky dat objektů BLOB úložiště](./media/search-managed-identities/add-role-assignment-storage-blob-data-reader.png "Přidat přiřazení role čtečky dat objektů BLOB úložiště")

    Příklad pro Azure Table Storage:

    ![Přidat přiřazení čtecího zařízení a role přístup k datům](./media/search-managed-identities/add-role-assignment-reader-and-data-access.png "Přidat přiřazení čtecího zařízení a role přístup k datům")

### <a name="3---create-the-data-source"></a>3. vytvoření zdroje dat

Při indexování z účtu úložiště musí mít zdroj dat následující požadované vlastnosti:

* **název** je jedinečný název zdroje dat v rámci vyhledávací služby.
* **textový**
    * Úložiště objektů BLOB v Azure:`azureblob`
    * Úložiště tabulek Azure:`azuretable`
    * Azure Data Lake Storage Gen2: **typ** se poskytne po registraci ve verzi Preview pomocí [tohoto formuláře](https://aka.ms/azure-cognitive-search/mi-preview-request).
* **přihlašovací údaje**
    * Při ověřování pomocí spravované identity se formát **přihlašovacích údajů** liší od použití spravované identity. Tady poskytnete ResourceId, které nemá klíč účtu ani heslo. ResourceId musí zahrnovat ID předplatného účtu úložiště, skupinu prostředků účtu úložiště a název účtu úložiště.
    * Spravovaný formát identity: 
        * *ResourceId =/Subscriptions/**ID vašeho předplatného**/resourceGroups/název vaší**skupiny prostředků**/Providers/Microsoft.Storage/storageAccounts/**název vašeho účtu úložiště**/;*
* **kontejner** Určuje název kontejneru nebo tabulky v účtu úložiště. Ve výchozím nastavení jsou všechny objekty BLOB v kontejneru navýšené. Pokud chcete indexovat objekty blob pouze v konkrétním virtuálním adresáři, můžete tento adresář zadat pomocí volitelného parametru **dotazu** .

Příklad vytvoření objektu zdroje dat objektů BLOB pomocí [REST API](https://docs.microsoft.com/rest/api/searchservice/create-data-source):

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "blob-datasource",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name/;" },
    "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
}   
```

Azure Portal a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) podporují také připojovací řetězec spravovaných identit. Azure Portal vyžaduje příznak funkce, který vám poskytne při registraci ve verzi Preview pomocí odkazu v horní části této stránky. 

### <a name="4---create-the-index"></a>4. vytvoření indexu

Index určuje pole v dokumentu, atributech a dalších konstrukcích, které prohledají možnosti vyhledávání.

Tady je postup, jak vytvořit index s `content` polem, které lze prohledávat a který ukládá text extrahovaný z objektů BLOB:   

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

Další informace o vytváření indexů najdete v tématu [vytvoření indexu](https://docs.microsoft.com/rest/api/searchservice/create-index) .

### <a name="5---create-the-indexer"></a>5. vytvoření indexeru

Indexer připojuje zdroj dat k cílovému vyhledávacímu indexu a poskytuje plán pro automatizaci aktualizace dat.

Po vytvoření indexu a zdroje dat jste připraveni vytvořit indexer.

Příklad definice indexeru pro indexer objektů BLOB:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Tento indexer se spustí každé dvě hodiny (časový interval je nastaven na "PT2H"). Pokud chcete indexer spustit každých 30 minut, nastavte interval na "PT30M". Nejkratší podporovaný interval je 5 minut. Plán je nepovinný – Pokud je vynechaný, indexer se při vytvoření spustí jenom jednou. Můžete ale kdykoli spustit indexer na vyžádání.   

Další informace o rozhraní API Create indexeru najdete v části [Vytvoření indexeru](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Další informace o definování plánů indexerů najdete v tématu [postup plánování indexerů pro Azure kognitivní hledání](search-howto-schedule-indexers.md).

## <a name="see-also"></a>Viz také

Další informace o Azure Storage indexerech:
* [Indexer Azure Blob](search-howto-indexing-azure-blob-storage.md)
* [Azure Data Lake Storage Gen2 indexer](search-howto-index-azure-data-lake-storage.md)
* [Indexer tabulek Azure](search-howto-indexing-azure-tables.md)
