---
title: Nastavení připojení ke zdroji dat pomocí spravované identity (Preview)
titleSuffix: Azure Cognitive Search
description: Naučte se, jak nastavit připojení indexeru ke zdroji dat pomocí spravované identity (Preview).
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 48b94b8cd047f62ea13bf4e062254088ea11840e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83664897"
---
# <a name="set-up-an-indexer-connection-to-a-data-source-using-a-managed-identity-preview"></a>Nastavení připojení indexeru ke zdroji dat pomocí spravované identity (Preview)

> [!IMPORTANT] 
> Podpora nastavení připojení ke zdroji dat pomocí spravované identity je aktuálně v ověřované veřejné verzi Preview. Funkce Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy.
> Vyplněním [tohoto formuláře](https://aka.ms/azure-cognitive-search/mi-preview-request)můžete požádat o přístup k verzi Preview.

[Indexer](search-indexer-overview.md) v Azure kognitivní hledání je prohledávací modul, který poskytuje způsob, jak načíst data ze zdroje dat do Azure kognitivní hledání. Indexer získá připojení ke zdroji dat z objektu zdroje dat, který vytvoříte. Objekt zdroje dat obvykle obsahuje přihlašovací údaje pro cílový zdroj dat. Objekt zdroje dat může například zahrnovat klíč účtu Azure Storage, pokud chcete indexovat data z kontejneru úložiště objektů BLOB.

V mnoha případech poskytnutí přihlašovacích údajů přímo v objektu zdroje dat není problém, ale vyskytly se problémy, které mohou nastat:
* Návody zachovat přihlašovací údaje v kódu, který vytvoří objekt zdroje dat?
* Pokud je váš klíč nebo heslo napadený a potřebuji ho změnit, teď je potřeba aktualizovat své zdrojové objekty dat pomocí nového klíče účtu nebo hesla, aby se indexer mohl znovu připojit ke zdroji dat.

Tyto otázky je možné vyřešit nastavením připojení pomocí spravované identity.

## <a name="using-managed-identities"></a>Použití spravovaných identit

[Spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) jsou funkce, která poskytuje služby Azure s automaticky spravovanou identitou v Azure Active Directory (Azure AD). Pomocí této funkce v Azure Kognitivní hledání můžete vytvořit objekt zdroje dat s připojovacím řetězcem, který neobsahuje žádné přihlašovací údaje. Místo toho bude službě Search udělen přístup ke zdroji dat prostřednictvím řízení přístupu na základě role (RBAC).

Při nastavování zdroje dat pomocí spravované identity můžete změnit přihlašovací údaje ke zdroji dat a indexery se pořád budou moct připojit ke zdroji dat. Můžete také vytvořit objekty zdroje dat ve vašem kódu, aniž byste museli zahrnout klíč účtu, nebo použít Key Vault k načtení klíče účtu.

## <a name="limitations"></a>Omezení

Následující zdroje dat podporují nastavení připojení indexeru pomocí spravovaných identit. 

* [Azure Blob Storage, Azure Data Lake Storage Gen2 (Preview), Azure Table Storage](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)

Následující funkce aktuálně nepodporují použití spravovaných identit k nastavení připojení:
* Knowledge Store
* Vlastní dovednosti
 
## <a name="next-steps"></a>Další kroky

Další informace o tom, jak nastavit připojení indexeru pomocí spravovaných identit:

* [Azure Blob Storage, Azure Data Lake Storage Gen2 (Preview), Azure Table Storage](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)