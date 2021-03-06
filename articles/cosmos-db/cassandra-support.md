---
title: Funkce Apache Cassandra, které podporuje rozhraní API Cassandra pro Azure Cosmos DB
description: Informace o podpoře funkcí Apache Cassandra v rozhraní API Cassandra pro Azure Cosmos DB
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: e7384237f91bf3af8ccad1a97b27fb62a1845a88
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2020
ms.locfileid: "85118980"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Funkce Apache Cassandra, které podporuje rozhraní API Cassandra pro Azure Cosmos DB 

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. S rozhraním API Cassandra pro Azure Cosmos DB můžete komunikovat prostřednictvím [přenosového protokolu](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) jazyka Cassandra Query Language (CQL) verze 4 kompatibilního s open source [ovladači](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) klienta Cassandra. 

Když použijete rozhraní API Cassandra pro Azure Cosmos DB, získáte nejen výhody rozhraní API pro Apache Cassandra, ale i podnikové funkce, které poskytuje Azure Cosmos DB. Mezi podnikové funkce patří [globální distribuce](distribute-data-globally.md), [automatické dělení škálování na více systémů](partition-data.md), záruky dostupnosti a latence, šifrování při nečinnosti, zálohování a mnoho dalšího.

## <a name="cassandra-protocol"></a>Protokol Cassandra 

Rozhraní API Cassandra pro Azure Cosmos DB je kompatibilní s CQL **verze v4**. Podporované příkazy CQL, nástroje, omezení a výjimky najdete níže. Jakýkoli ovladač klienta, který podporuje tyto protokoly, by se měl dokázat připojit k rozhraní API Cassandra pro Azure Cosmos DB.

## <a name="cassandra-driver"></a>Ovladač Cassandra

Rozhraní API Cassandra pro Azure Cosmos DB podporuje tyto verze ovladačů Cassandra:

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [NodeJS 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>Datové typy CQL 

Rozhraní API Cassandra pro Azure Cosmos DB podporuje následující datové typy CQL:

* ascii  
* bigint  
* blob  
* Boolean  
* counter  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* text  
* time  
* časové razítko  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* mapa  

## <a name="cql-functions"></a>Funkce CQL

Rozhraní API Cassandra pro Azure Cosmos DB podporuje tyto funkce CQL:

* Token  
* Agregační funkce
  * minimum, maximum, prům. Count
* Funkce pro převod objektů blob 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID a funkce timeuuid 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toTimestamp(timeuuid)  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toTimestamp(date)  
  * toTimestamp(date) 
  


## <a name="cassandra-api-limits"></a>Omezení rozhraní API Cassandra

Rozhraní API Cassandra pro Azure Cosmos DB nemá žádná omezení velikosti dat uložených v tabulce. Když se dodrží limity klíče oddílu, je možné uložit stovky terabajtů nebo petabajtů dat. Podobně všechny ekvivalenty entit nebo řádků nemají omezení počtu sloupců. Celková velikost entity však nesmí překročit 2 MB. Data na klíč oddílu nesmí být větší než 20 GB jako u všech ostatních rozhraní API.

## <a name="tools"></a>nástroje 

Rozhraní API Cassandra pro Azure Cosmos DB je platforma pro spravované služby. Ke správě clusteru nevyžaduje žádnou režii ani nástroje řízení, jako jsou například systém uvolňování paměti, Java Virtual Machine (JVM) a nodetool. Podporuje nástroje, jako je cqlsh, které využívá kompatibilitu Binary CQLv4. 

* Průzkumník dat Azure Portal, metriky, diagnostika protokolů, PowerShell a rozhraní příkazového řádku jsou dalšími podporovanými mechanismy pro správu účtu.

## <a name="hosted-cql-shell-preview"></a>Hostované prostředí CQL (Preview)

Hosted Native Cassandra Shell (CQLSH v 5.0.1) můžete otevřít přímo z Průzkumník dat v [Azure Portal](data-explorer.md) nebo v [Průzkumníku Azure Cosmos](https://cosmos.azure.com/). Než povolíte prostředí CQL, musíte ve svém účtu [Povolit funkci poznámkových blocích](enable-notebooks.md) (Pokud ještě není povolená, zobrazí se vám výzva při kliknutí `Open Cassandra Shell` ). Podívejte se na zvýrazněnou poznámku v části [Povolení poznámkových bloků pro účty Azure Cosmos DB](enable-notebooks.md) pro podporované oblasti Azure.

:::image type="content" source="./media/cassandra-support/cqlsh.png" alt-text="CQLSH":::

Můžete se také připojit k rozhraní API Cassandra v Azure Cosmos DB pomocí CQLSH nainstalovaného na místním počítači. Dodává se s Apache Cassandra 3.1.1 a funguje v poli nastavením proměnných prostředí. Následující části obsahují pokyny k instalaci, konfiguraci a připojení k rozhraní API Cassandra v Azure Cosmos DB, v systému Windows nebo Linux pomocí CQLSH.

**Systému**

Pokud používáte systém Windows, doporučujeme povolit [systém souborů systému Windows pro Linux](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux). Pak můžete postupovat podle níže uvedených příkazů pro Linux.

**Unix/Linux/Mac:**

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

## <a name="cql-commands"></a>Příkazy CQL

Azure Cosmos DB podporuje u účtů rozhraní API Cassandra následující databázové příkazy.

* VYTVOŘIT prostor (nastavení replikace pro tento příkaz se ignoruje)
* CREATE TABLE 
* Vytvoření indexu (bez zadání názvu indexu a úplných zmrazených indexů, které ještě nejsou podporované)
* POVOLENÍ FILTROVÁNÍ
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH – podporují se jen neprotokolované příkazy. 
* DELETE

Všechny operace CRUD, které jsou spouštěny prostřednictvím sady SDK kompatibilní s CQL v4, vrátí další informace o tom, jaké chybové a využívané jednotky žádosti se vybírají. Příkazy DELETE a UPDATE by se měly zpracovat s ohledem na dodržování zásad správného řízení prostředků, aby se zajistilo nejúčinnější využívání zřízené propustnosti.

* Poznámka: Pokud je zadaná hodnota gc_grace_seconds, musí být nulová.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## <a name="consistency-mapping"></a>Mapování konzistence 

Rozhraní API Cassandra pro Azure Cosmos DB poskytuje volbu konzistence pro operace čtení.  Mapování konzistence je popsáno [zde](consistency-levels-across-apis.md#cassandra-mapping).

## <a name="permission-and-role-management"></a>Správa oprávnění a rolí

Azure Cosmos DB podporuje řízení přístupu na základě role (RBAC) pro zřizování, otáčení klíčů, zobrazení metrik a hesla pro čtení a zápis a klíče nebo klíče, které lze získat prostřednictvím [Azure Portal](https://portal.azure.com). Azure Cosmos DB nepodporuje role pro aktivity CRUD.

## <a name="keyspace-and-table-options"></a>Možnosti prostoru klíčů a tabulek

Možnosti pro název oblasti, třídu, replication_factor a datacentrum v příkazu pro vytvoření prostoru klíčů se aktuálně ignorují. Systém používá k přidání oblastí základní metodu replikace [globální distribuce](global-dist-under-the-hood.md) Azure Cosmos DB. Pokud potřebujete data mezi oblastmi, můžete ji povolit na úrovni účtu pomocí PowerShellu, rozhraní příkazového řádku nebo portálu. Další informace najdete v článku [Přidání oblastí](how-to-manage-database-account.md#addremove-regions-from-your-database-account) . Durable_writes nejde zakázat, protože Azure Cosmos DB zajišťuje, že každý zápis je trvalý. V každé oblasti Azure Cosmos DB replikuje data napříč sadou replik, která je tvořená čtyřmi replikami a tuto [konfiguraci](global-dist-under-the-hood.md) sady replik nelze upravit.
 
Při vytváření tabulky se ignorují všechny možnosti, kromě gc_grace_seconds, které by měly být nastavené na hodnotu nula.
V prostoru a tabulce se nachází další možnost s názvem "cosmosdb_provisioned_throughput" s minimální hodnotou 400 RU/s. Propustnost místa na disku umožňuje sdílení propustnosti napříč několika tabulkami a je užitečná pro scénáře, kdy všechny tabulky nevyužívají zřízenou propustnost. Příkaz ALTER TABLE umožňuje změnit zřízenou propustnost napříč oblastmi. 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```


## <a name="usage-of-cassandra-retry-connection-policy"></a>Používání zásad opakování připojení Cassandra

Azure Cosmos DB je systém spravovaný systémem prostředků. To znamená, že v dané druhé operaci můžete provést určitý počet operací na základě jednotek žádostí spotřebovaných operacemi. Pokud aplikace během dané sekundy toto omezení překročí, požadavky jsou omezené na frekvenci a vyvolají se výjimky. Rozhraní API Cassandra v Azure Cosmos DB překládá tyto výjimky na přetížené chyby v nativním protokolu Cassandra. Chcete-li zajistit, aby vaše aplikace mohla zachytit a opakovat požadavky v případě omezení přenosové rychlosti, jsou k dispozici rozšíření [Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) a [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) . Pokud se připojujete k rozhraní API Cassandra v Azure Cosmos DB, přečtěte si také ukázky kódu Java pro ovladače [verze 3](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) a [verze 4](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4) DataStax. Pokud k přístupu k rozhraní API Cassandra v Azure Cosmos DB používáte jiné sady SDK, vytvořte zásadu připojení a zkuste to znovu s těmito výjimkami.

## <a name="next-steps"></a>Další kroky

- Začínáme s [vytvořením účtu rozhraní API Cassandra, databáze a tabulky](create-cassandra-api-account-java.md) prostřednictvím aplikace Java
