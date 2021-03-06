---
title: Seznam funkcí Preview
titleSuffix: Azure Cognitive Search
description: Funkce verze Preview se uvolňují, aby zákazníci mohli poskytovat zpětnou vazbu ke svým návrhům a nástrojům. Tento článek obsahuje úplný seznam všech funkcí, které jsou v současnosti ve verzi Preview.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: b952d6b6fec9a1ec0dcd8af1a9566e67d3301d77
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496702"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Funkce ve verzi Preview v Azure Kognitivní hledání

Tento článek obsahuje úplný seznam všech funkcí, které jsou ve verzi Public Preview. Funkce Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro produkční úlohy. Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Funkce ve verzi Preview, které se převádějí do všeobecné dostupnosti, se z tohoto seznamu odeberou. Pokud funkce není uvedená níže, můžete předpokládat, že je všeobecně dostupná. Oznámení týkající se obecné dostupnosti najdete v tématu [aktualizace služby](https://azure.microsoft.com/updates/?product=search) nebo [novinky](whats-new.md).

|Zapnut&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Kategorie | Popis | Dostupnost  |
|---------|------------------|-------------|---------------|
| [**Azure Machine Learning (AML) – dovednost**](cognitive-search-aml-skill.md) | Obohacení AI| Nový typ dovednosti pro integraci koncového bodu Inferencing z Azure Machine Learning. Začněte s [tímto kurzem](cognitive-search-tutorial-aml-custom-skill.md). | Použijte [Search REST API 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/) nebo 2019-05-06-Preview. K dispozici také na portálu v návrhu dovednosti, za předpokladu, že Kognitivní hledání a služby Azure ML se nasazují ve stejném předplatném. |
| [**parametr featuresMode**](https://docs.microsoft.com/rest/api/searchservice/search-documents#featuresmode) | Relevance (bodování) | Rozšíření skóre relevance tak, aby zahrnovalo detaily: skóre podle pole, frekvence termínu pro pole a počet jedinečných tokenů, které se shodují. Tyto datové body můžete využívat ve [vlastních řešeních pro bodování](https://github.com/Azure-Samples/search-ranking-tutorial). | Přidejte tento parametr dotazu pomocí [vyhledávacích dokumentů (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) s rozhraním API-Version = 2020-06 -30-preview nebo 2019-05-06-Preview. |
| [**Identita spravované služby**](search-howto-managed-identities-data-sources.md) | Indexery, zabezpečení| Zaregistrujte vyhledávací službu s Azure Active Directory, abyste ji načetli jako důvěryhodnou službu, a pak v Azure Data Sources použijte oprávnění RBAC, aby bylo možné v indexeru umožnit přístup jen pro čtení. | Přístup k této funkci při použití portálu nebo [Vytvoření zdroje dat (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**Relace ladění**](cognitive-search-debug-session.md) | Portál, obohacení AI (dovednosti) | Editor dovednosti v relaci, který se používá k prozkoumání a řešení problémů s dovednosti. Opravy, které se použijí během relace ladění, se dají uložit do dovednosti ve službě. | Pouze portál, pomocí odkazů na střední stránku na stránce Přehled otevřete relaci ladění. |
| [**Obnovitelné odstranění nativního objektu BLOB**](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection) | Indexery, objekty blob Azure| Indexovací člen služby Azure Blob Storage v Azure Kognitivní hledání rozpozná objekty blob, které jsou ve stavu undeleteded, a během indexování odebere odpovídající hledaný dokument. | Přidejte toto nastavení konfigurace pomocí [Create indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**Vlastní dovednosti při vyhledávání entit**](cognitive-search-skill-custom-entity-lookup.md ) | Rozšíření AI (dovednosti) | Vnímání dovedností, která hledá text z vlastního uživatelsky definovaného seznamu slov a frází. Pomocí tohoto seznamu jsou všechny dokumenty označeny všemi vyhovujícími entitami. Dovednost také podporuje stupeň přibližné shody, které lze použít pro hledání shod, které jsou podobné, ale nejsou zcela přesné. | Na tuto dovednost ve verzi Preview se odkazuje pomocí editoru dovednosti na portálu nebo [Vytvoření dovednosti (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**Dovednost pro detekci PII**](cognitive-search-skill-pii-detection.md) | Rozšíření AI (dovednosti) | Vnímání odbornosti, která se používá při indexování, která extrahuje osobní údaje ze vstupního textu a poskytuje možnost jejich maskování z tohoto textu různými způsoby. | Na tuto dovednost ve verzi Preview se odkazuje pomocí editoru dovednosti na portálu nebo [Vytvoření dovednosti (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**Přírůstkové obohacení**](cognitive-search-incremental-indexing-conceptual.md) | Konfigurace indexeru| Přidá do kanálu pro rozšíření ukládání do mezipaměti, což vám umožní znovu použít stávající výstup, pokud cílené změny, jako je například aktualizace dovednosti nebo jiného objektu, nezmění obsah. Ukládání do mezipaměti se týká pouze obohacených dokumentů vyprodukovaných dovednosti.| Přidejte toto nastavení konfigurace pomocí [Create indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**Cosmos DB indexer: rozhraní API MongoDB, rozhraní Gremlin API rozhraní API Cassandra**](search-howto-index-cosmosdb.md) | Zdroj dat indexeru | Pro Cosmos DB je SQL API všeobecně dostupné, ale rozhraní API pro MongoDB, Gremlin a Cassandra jsou ve verzi Preview. | Jenom pro Gremlin a Cassandra se [nejdřív Zaregistrujte](https://aka.ms/azure-cognitive-search/indexer-preview) , aby bylo možné povolit podporu pro vaše předplatné na back-endu. Zdroje dat MongoDB můžete nakonfigurovat na portálu. V opačném případě je konfigurace zdroje dat pro všechna tři rozhraní API podporovaná pomocí funkcí [vytvořit zdroj dat (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
|  [**Azure Data Lake Storage Gen2 indexer**](search-howto-index-azure-data-lake-storage.md) | Zdroj dat indexeru | Indexuje obsah a metadata z Data Lake Storage Gen2.| Vyžaduje se [registrace](https://aka.ms/azure-cognitive-search/indexer-preview) , aby bylo možné povolit podporu pro vaše předplatné na back-endu. Přístup k tomuto zdroji dat pomocí [Vytvoření zdroje dat (REST)](https://docs.microsoft.com/rest/api/searchservice/create-data-source) s rozhraním API-Version = 2020-06 -30-Preview nebo API-Version = 2019-05 -06-Preview. |
| [**moreLikeThis**](search-more-like-this.md) | Dotazy | Vyhledá dokumenty, které jsou relevantní pro určitý dokument. Tato funkce je ve starších verzích Preview. | Tento parametr dotazu přidejte do volání [vyhledávacích dokumentů (REST)](https://docs.microsoft.com/rest/api/searchservice/search-documents) pomocí API-Version = 2020-06 -30-preview, 2019-05-06-preview, 2016-09-01-preview nebo 2017-11-11-Preview. |

## <a name="calling-preview-rest-apis"></a>Volání rozhraní REST API pro náhled

Azure Kognitivní hledání vždy předem vydává experimentální funkce přes REST API a pak prostřednictvím předprodejní verze sady .NET SDK.

Funkce ve verzi Preview jsou k dispozici pro testování a experimentování s cílem shromažďovat zpětnou vazbu k návrhu a implementaci funkcí. Z tohoto důvodu se funkce verze Preview můžou v průběhu času měnit, možná způsobem, který přeruší zpětnou kompatibilitu. To je na rozdíl od funkcí ve verzi GA, které jsou stabilní a nepravděpodobné, že se nemění s výjimkou malých a nekompatibilních oprav a vylepšení. Funkce ve verzi Preview se také nedělají vždy do verze GA.

I když některé funkce verze Preview můžou být dostupné na portálu a .NET SDK, má REST API vždycky funkce ve verzi Preview.

+ Pro operace vyhledávání [**`2020-06-30-Preview`**](https://docs.microsoft.com/rest/api/searchservice/index-preview) je aktuální verze Preview.

+ Pro operace správy [**`2019-10-01-Preview`**](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) je aktuální verze Preview.

Starší verze Preview jsou pořád funkční, ale v průběhu času se stanou zastaralé. Pokud váš kód volá `api-version=2019-05-06-Preview` nebo `api-version=2016-09-01-Preview` nebo `api-version=2017-11-11-Preview` , jsou tato volání stále platná. Jenom nejnovější verze Preview se ale aktualizuje s vylepšeními. 

Následující příklad syntaxe znázorňuje volání rozhraní API verze Preview.

```HTTP
GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2020-06-30-Preview
```

Služba Azure Kognitivní hledání je dostupná ve více verzích. Další informace najdete v tématu [verze rozhraní API](search-api-versions.md).

## <a name="next-steps"></a>Další kroky

Přečtěte si referenční dokumentaci k rozhraní API pro hledání REST Preview. Pokud narazíte na problémy, požádejte nás, abychom vám pomohli [Stack Overflow](https://stackoverflow.com/) nebo [kontaktujte podporu](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Odkaz na REST API vyhledávací služby (Preview)](https://docs.microsoft.com/rest/api/searchservice/index-preview)