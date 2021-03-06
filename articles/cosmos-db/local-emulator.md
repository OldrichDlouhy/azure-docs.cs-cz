---
title: Vývoj místně pomocí emulátoru Azure Cosmos
description: Pomocí emulátoru Azure Cosmos můžete svou aplikaci místně vyvíjet a testovat bez vytváření předplatného Azure.
ms.service: cosmos-db
ms.topic: how-to
author: markjbrown
ms.author: mjbrown
ms.date: 01/31/2020
ms.openlocfilehash: e06a2eac5387cd02e95d8252ae04edc356683ed9
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86028248"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Použití emulátoru Azure Cosmos pro místní vývoj a testování

Emulátor Azure Cosmos poskytuje místní prostředí, které emuluje službu Azure Cosmos DB pro účely vývoje. Pomocí emulátoru Azure Cosmos můžete svou aplikaci vyvíjet a testovat místně, aniž byste museli vytvářet předplatné Azure nebo náklady. Až budete spokojeni s tím, jak vaše aplikace funguje v emulátoru Azure Cosmos, můžete přejít na používání účtu Azure Cosmos v cloudu.

Můžete vyvíjet pomocí emulátoru Azure Cosmos s využitím účtů [SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api)a [Table](local-emulator.md#table-api) API. V současnosti ale zobrazení Průzkumník dat v emulátoru plně podporuje klienty pouze pro rozhraní SQL API. 

## <a name="how-the-emulator-works"></a>Jak emulátor funguje

Emulátor Azure Cosmos zajišťuje emulaci Azure Cosmos DB služby s vysokou přesností. Podporuje stejné funkce jako Azure Cosmos DB, včetně podpory pro vytváření a dotazování dat, zřizování a škálování kontejnerů a spouštění uložených procedur a triggerů. Můžete vyvíjet a testovat aplikace pomocí emulátoru Azure Cosmos a nasazovat je do Azure v globálním měřítku jenom tak, že se na koncový bod připojení pro Azure Cosmos DB nasadí jedna změna konfigurace.

Přestože je emulace služby Azure Cosmos DB věrná, implementace emulátoru se od služby liší. Emulátor například používá standardní součásti operačního systému, jako je místní systém souborů pro trvalost a sada protokolů HTTPS pro připojení. Funkce, které se spoléhají na infrastrukturu Azure, jako je globální replikace, latence milisekund pro čtení a zápis a přizpůsobitelné úrovně konzistence se nevztahují.

Data mezi emulátorem Azure Cosmos a službou Azure Cosmos DB můžete migrovat pomocí [nástroje Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).

Emulátor Azure Cosmos můžete spustit v kontejneru Docker systému Windows. Další informace najdete v [Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) pro příkaz Docker Pull a na [GitHubu](https://github.com/Azure/azure-cosmos-db-emulator-docker) `Dockerfile` .

## <a name="differences-between-the-emulator-and-the-service"></a>Rozdíly mezi emulátorem a službou

Vzhledem k tomu, že emulátor Azure Cosmos poskytuje emulované prostředí běžící na místní pracovní stanici pro vývojáře, je mezi emulátorem a účtem Azure Cosmos v cloudu několik rozdílů:

* Aktuálně Průzkumník dat v emulátoru podporuje klienty pro rozhraní SQL API. Zobrazení a operace Průzkumník dat pro rozhraní API Azure Cosmos DB, jako jsou MongoDB, Table, Graph a Cassandra API, nejsou plně podporované.
* Emulátor Azure Cosmos podporuje jenom jeden pevný účet a dobře známý hlavní klíč. Vygenerování klíče není možné v emulátoru Azure Cosmos, ale výchozí klíč se dá změnit pomocí možnosti příkazového řádku.
* Emulátor Azure Cosmos není škálovatelná služba a nebude podporovat velký počet kontejnerů.
* Emulátor Azure Cosmos nenabízí různé [Azure Cosmos dB úrovně konzistence](consistency-levels.md).
* Emulátor Azure Cosmos nenabízí replikaci ve [více oblastech](distribute-data-globally.md).
* Protože vaše kopie emulátoru Azure Cosmos nemusí být vždycky aktuální s nejnovějšími změnami ve službě Azure Cosmos DB, měli byste se podívat na [Azure Cosmos DB Capacity Planneru](https://www.documentdb.com/capacityplanner) a přesně odhadnout, jaké jsou požadavky vaší aplikace v produkční propustnosti (ru).
* Při použití emulátoru Azure Cosmos ve výchozím nastavení můžete vytvořit až 25 kontejnerů s pevnou velikostí (podporované jenom pomocí sad SDK pro Azure Cosmos DB) nebo 5 neomezených kontejnerů pomocí emulátoru Azure Cosmos. Další informace o změně této hodnoty najdete v části [Nastavení hodnoty PartitionCount](#set-partitioncount).
* Emulátor podporuje maximální velikost vlastností ID 254 znaků.

## <a name="system-requirements"></a>Požadavky na systém

Emulátor Azure Cosmos má následující požadavky na hardware a software:

* Požadavky na software
  * Windows Server 2012 R2, Windows Server 2016 nebo Windows 10
  * 64bitový operační systém
* Minimální požadavky na hardware
  * 2 GB RAM
  * 10 GB volného místa na disku

## <a name="installation"></a>Instalace

Emulátor Azure Cosmos můžete stáhnout a nainstalovat z webu [Microsoft Download Center](https://aka.ms/cosmosdb-emulator) nebo můžete spustit emulátor na Docker for Windows. Pokyny k používání emulátoru v Docker for Windows najdete v tématu [spuštění v Docker](#running-on-docker).

> [!NOTE]
> Pokud chcete nainstalovat, nakonfigurovat a spustit emulátor Azure Cosmos, musíte mít v počítači oprávnění správce. Emulátor vytvoří nebo přidá certifikát a také nastaví pravidla brány firewall, aby bylo možné provozovat jeho služby. Proto je nutné, aby emulátor mohl provádět tyto operace.

## <a name="running-on-windows"></a>Spuštění v systému Windows

Emulátor Azure Cosmos spustíte tak, že vyberete tlačítko Start nebo stisknete klávesu Windows. Začněte psát **emulátor Azure Cosmos**a vyberte ze seznamu aplikací emulátor.

:::image type="content" source="./media/local-emulator/database-local-emulator-start.png" alt-text="Vyberte tlačítko Start nebo stiskněte klávesu Windows, začněte psát emulátor Azure Cosmos a vyberte ze seznamu aplikací emulátor.":::

Po spuštění emulátoru se v oznamovací oblasti hlavního panelu Windows zobrazí jeho ikona. 

:::image type="content" source="./media/local-emulator/database-local-emulator-taskbar.png" alt-text="Oznámení o Azure Cosmos DB hlavním emulátoru hlavního emulátoru":::

Emulátor Azure Cosmos ve výchozím nastavení běží na místním počítači (localhost), který naslouchá na portu 8081.

Emulátor Azure Cosmos je `C:\Program Files\Azure Cosmos DB Emulator` ve výchozím nastavení nainstalovaný. Emulátor můžete také spustit a zastavit z příkazového řádku. Další informace najdete v [referenčních informacích k nástroji příkazového řádku](#command-line).

## <a name="start-data-explorer"></a>Spuštění Průzkumníka dat

Když se emulátor Azure Cosmos spustí, automaticky se v prohlížeči otevře Průzkumník dat Azure Cosmos. Adresa se zobrazí jako `https://localhost:8081/_explorer/index.html`. Pokud zavřete Průzkumníka a chcete ho znovu otevřít později, můžete buď otevřít adresu URL v prohlížeči, nebo ji spustit z emulátoru Azure Cosmos v ikoně na hlavním panelu Windows, jak je znázorněno níže.

:::image type="content" source="./media/local-emulator/database-local-emulator-data-explorer-launcher.png" alt-text="Spouštěč Průzkumníka dat místního emulátoru Azure Cosmos":::

## <a name="checking-for-updates"></a>Kontrola aktualizací

Průzkumník dat zjistí, zda je k dispozici nová aktualizace ke stažení.

> [!NOTE]
> Data vytvořená v jedné verzi emulátoru Azure Cosmos (viz%LOCALAPPDATA%\CosmosDBEmulator nebo volitelná nastavení cesty k datům) nejsou zaručená, že budou přístupná, pokud používáte jinou verzi. Pokud potřebujete zachovat data po dlouhou dobu, doporučujeme, abyste tato data ukládali v účtu Azure Cosmos, nikoli v emulátoru Azure Cosmos.

## <a name="authenticating-requests"></a>Ověřování požadavků

Stejně jako u Azure Cosmos DB v cloudu musí být ověřeny všechny požadavky, které provedete v emulátoru Azure Cosmos. Emulátor Azure Cosmos podporuje jeden pevný účet a známý ověřovací klíč pro ověřování pomocí hlavního klíče. Tento účet a klíč jsou jediné přihlašovací údaje povolené pro použití s emulátorem Azure Cosmos. Jsou to tyto:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Hlavní klíč podporovaný emulátorem Azure Cosmos je určený jenom pro použití s emulátorem. V emulátoru Azure Cosmos nemůžete použít svůj účet a klíč produkčního Azure Cosmos DB.

> [!NOTE]
> Pokud jste spustili emulátor s možností/Key, pak použijte vygenerovaný klíč místo `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` . Další informace o možnosti/Key naleznete v tématu [Reference k nástroji příkazového řádku.](#command-line)

Jako u Azure Cosmos DB emulátor Azure Cosmos podporuje jenom zabezpečenou komunikaci pomocí protokolu TLS.

## <a name="running-on-a-local-network"></a>Spuštění v místní síti

Emulátor můžete spustit v místní síti. Pokud chcete povolit přístup k síti, zadejte na `/AllowNetworkAccess` [příkazovém řádku](#command-line-syntax)možnost, která také vyžaduje, abyste zadali `/Key=key_string` nebo `/KeyFile=file_name` . Můžete použít `/GenKeyFile=file_name` k vygenerování souboru s náhodným klíčem. Pak můžete předat to `/KeyFile=file_name` nebo `/Key=contents_of_file` .

Pro povolení přístupu k síti uživateli při prvním spuštění emulátoru a odstranění datového adresáře emulátoru (%LOCALAPPDATA%\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Vývoj v emulátoru

### <a name="sql-api"></a>SQL API

Jakmile na ploše spustíte emulátor Azure Cosmos, můžete k interakci s emulátorem použít libovolnou podporovanou [sadu Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) nebo [Azure Cosmos DB REST API](/rest/api/cosmos-db/) . Emulátor Azure Cosmos obsahuje taky integrovaný Průzkumník dat, který umožňuje vytvářet kontejnery pro rozhraní SQL API nebo Cosmos DB pro rozhraní API Mongo DB a zobrazovat a upravovat položky bez psaní kódu.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Rozhraní API služby Azure Cosmos DB pro MongoDB

Jakmile na ploše spustíte emulátor Azure Cosmos, můžete k interakci s emulátorem použít [rozhraní API Azure Cosmos DB pro MongoDB](mongodb-introduction.md) . Spusťte emulátor z příkazového řádku jako správce s názvem "/EnableMongoDbEndpoint". Pak použijte následující připojovací řetězec pro připojení k účtu rozhraní MongoDB API:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>Rozhraní Table API

Jakmile na ploše spustíte emulátor Azure Cosmos, můžete k interakci s emulátorem použít [sadu SDK pro rozhraní API pro tabulky Azure Cosmos DB](table-storage-how-to-use-dotnet.md) . Spusťte emulátor z příkazového řádku jako správce s názvem "/EnableTableEndpoint". V dalším kroku spusťte následující kód pro připojení k účtu rozhraní Table API:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Rozhraní Cassandra API

Spusťte emulátor z příkazového řádku správce s názvem "/EnableCassandraEndpoint". Případně můžete také nastavit proměnnou prostředí `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true` .

* [Instalace Pythonu 2,7](https://www.python.org/downloads/release/python-2716/)

* [Instalace rozhraní příkazového řádku Cassandra/CQLSH](https://cassandra.apache.org/download/)

* V běžném okně příkazového řádku spusťte následující příkazy:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* V prostředí CQLSH spusťte následující příkazy pro připojení ke koncovému bodu Cassandra:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>Rozhraní Gremlin API

Spusťte emulátor z příkazového řádku správce s názvem "/EnableGremlinEndpoint". Případně můžete také nastavit proměnnou prostředí.`AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

* [Nainstalujte Apache-tinkerpop-Gremlin-Console-3.3.4](https://archive.apache.org/dist/tinkerpop/3.3.4).

* V Průzkumník dat emulátoru vytvořte databázi "DB1" a kolekci "coll1"; pro klíč oddílu vyberte "/Name".

* V běžném okně příkazového řádku spusťte následující příkazy:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* V prostředí Gremlin spusťte následující příkazy pro připojení ke koncovému bodu Gremlin:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-tlsssl-certificate"></a>Export certifikátu TLS/SSL

Jazyky a moduly runtime rozhraní .NET používají k bezpečnému připojení k místnímu emulátoru služby Azure Cosmos DB úložiště certifikátů systému Windows. Další jazyky mají vlastní metody správy a použití certifikátů. Java používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), zatímco Python používá [obálky soketu](https://docs.python.org/2/library/ssl.html).

Pokud chcete získat certifikát pro použití s jazyky a moduly runtime, které se neintegrují s úložištěm certifikátů systému Windows, budete ho muset exportovat pomocí Správce certifikátů systému Windows. Můžete ji spustit spuštěním Certlm. msc nebo podle podrobných pokynů v tématu [export certifikátů emulátoru Azure Cosmos](./local-emulator-export-ssl-certificates.md). Jakmile je správce certifikátů spuštěn, otevřete osobní certifikáty, jak je zobrazeno níže, a exportujte certifikát s popisným názvem „DocumentDBEmulatorCertificate“ jako soubor X.509 (.cer) s kódováním BASE-64.

:::image type="content" source="./media/local-emulator/database-local-emulator-ssl_certificate.png" alt-text="Azure Cosmos DB certifikát TLS/SSL místního emulátoru":::

Certifikát X.509 lze importovat do úložiště certifikátů Javy podle pokynů v tématu o [přidání certifikátu do úložiště certifikátů certifikační autority Javy](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Až se certifikát naimportuje do úložiště certifikátů, klienti pro SQL a Azure Cosmos DB API pro MongoDB se budou moct připojit k emulátoru Azure Cosmos.

Při připojování k emulátoru z Pythonu a Node.js SDK je ověřování TLS zakázané.

## <a name="command-line-tool-reference"></a><a id="command-line"></a>Reference nástroje příkazového řádku
Z umístění instalace můžete pomocí příkazového řádku spustit a zastavit emulátor, nakonfigurovat možnosti a provést jiné operace.

### <a name="command-line-syntax"></a>Syntaxe příkazového řádku

```cmd
Microsoft.Azure.Cosmos.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]
```

Pokud chcete zobrazit seznam možností, na příkazovém řádku zadejte `Microsoft.Azure.Cosmos.Emulator.exe /?`.

|**Nastavení** | **Popis** | **Příkaz**| **Arguments**|
|---|---|---|---|
|[Žádné argumenty] | Spustí emulátor Azure Cosmos s výchozími nastaveními. |Microsoft.Azure.Cosmos.Emulator.exe| |
|[Nápověda] |Zobrazí seznam podporovaných argumentů příkazového řádku.|Microsoft.Azure.Cosmos.Emulator.exe/? | |
| GetStatus |Získá stav emulátoru Azure Cosmos. Stav je indikován ukončovacím kódem: 1 = spouštění, 2 = spuštěno, 3 = zastaveno. Záporný ukončovací kód označuje, že došlo k chybě. Žádný jiný výstup neexistuje. | Microsoft.Azure.Cosmos.Emulator.exe/GetStatus| |
| Vypnutí| Ukončí emulátor Azure Cosmos.| Microsoft.Azure.Cosmos.Emulator.exe/Shutdown | |
|DataPath | Určuje cestu, do které chcete uložit datové soubory. Výchozí hodnota je%LocalAppdata%\CosmosDBEmulator. | Microsoft.Azure.Cosmos.Emulator.exe/DataPath =\<datapath\> | \<datapath\>: Přístupná cesta |
|Port | Určuje číslo portu pro emulátor. Výchozí hodnota je 8081. |Microsoft.Azure.Cosmos.Emulator.exe/port =\<port\> | \<port\>: Jedno číslo portu |
| ComputePort | Určuje číslo portu, které se má použít pro službu COMPUTE Interop Gateway. Port testu koncového bodu HTTP brány se počítá jako ComputePort + 79. Proto musí být ComputePort a ComputePort + 79 otevřené a dostupné. Výchozí hodnota je 8900. | Microsoft.Azure.Cosmos.Emulator.exe/ComputePort =\<computeport\> | \<computeport\>: Jedno číslo portu |
| EnableMongoDbEndpoint = 3.2 | Povolí rozhraní MongoDB API 3,2. | Microsoft.Azure.Cosmos.Emulator.exe/EnableMongoDbEndpoint = 3.2 | |
| EnableMongoDbEndpoint = 3.6 | Povolí rozhraní MongoDB API 3,6. | Microsoft.Azure.Cosmos.Emulator.exe/EnableMongoDbEndpoint = 3.6 | |
| MongoPort | Určuje číslo portu, který chcete použít pro rozhraní API kompatibility MongoDB. Výchozí hodnota je 10255. |Microsoft.Azure.Cosmos.Emulator.exe/MongoPort =\<mongoport\>|\<mongoport\>: Jedno číslo portu|
| EnableCassandraEndpoint | Povolí rozhraní API Cassandra | Microsoft.Azure.Cosmos.Emulator.exe/EnableCassandraEndpoint | |
| CassandraPort | Určuje číslo portu, který se má použít pro koncový bod Cassandra. Výchozí hodnota je 10350. | Microsoft.Azure.Cosmos.Emulator.exe/CassandraPort =\<cassandraport\> | \<cassandraport\>: Jedno číslo portu |
| EnableGremlinEndpoint | Povolí rozhraní Gremlin API. | Microsoft.Azure.Cosmos.Emulator.exe/EnableGremlinEndpoint | |
| GremlinPort | Číslo portu, které se má použít pro koncový bod Gremlin Výchozí hodnota je 8901. | Microsoft.Azure.Cosmos.Emulator.exe/GremlinPort =\<port\> | \<port\>: Jedno číslo portu |
|EnableTableEndpoint | Povolí Azure rozhraní API pro tabulky | Microsoft.Azure.Cosmos.Emulator.exe/EnableTableEndpoint | |
|TablePort | Číslo portu, které se má použít pro koncový bod tabulky Azure Výchozí hodnota je 8902. | Microsoft.Azure.Cosmos.Emulator.exe/TablePort =\<port\> | \<port\>: Jedno číslo portu|
| KeyFile | Načte autorizační klíč ze zadaného souboru. Pro vytvoření souboru klíče použijte možnost/GenKeyFile | Microsoft.Azure.Cosmos.Emulator.exe/KeyFile =\<file_name\> | \<file_name\>: Cesta k souboru |
| ResetDataPath | Rekurzivně odstraní všechny soubory v zadané cestě. Pokud cestu nezadáte, použije se výchozí hodnota%LOCALAPPDATA%\CosmosDbEmulator. | Microsoft.Azure.Cosmos.Emulator.exe/ResetDataPath =\<path> | \<path\>: Cesta k souboru  |
| StartTraces  |  Spusťte shromažďování protokolů trasování ladění pomocí programu LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe/StartTraces | |
| StopTraces     | Zastavte shromažďování protokolů trasování ladění pomocí programu LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe/StopTraces  | |
| StartWprTraces  |  Spusťte shromažďování protokolů trasování ladění pomocí nástroje pro záznam výkonu systému Windows. | Microsoft.Azure.Cosmos.Emulator.exe/StartWprTraces | |
| StopWprTraces     | Zastavte shromažďování protokolů trasování ladění pomocí nástroje pro záznam výkonu systému Windows. | Microsoft.Azure.Cosmos.Emulator.exe/StopWprTraces  | |
|FailOnSslCertificateNameMismatch | Ve výchozím nastavení emulátor znovu vygeneruje svůj certifikát TLS/SSL podepsaný svým držitelem, pokud síť SAN certifikátu neobsahuje název domény hostitele emulátoru, místní adresu IPv4, localhost a adresu 127.0.0.1. Tato možnost způsobí, že emulátor při spuštění selže. Pak použijte možnost/GenCert k vytvoření a instalaci nového certifikátu TLS/SSL podepsaného svým držitelem. | Microsoft.Azure.Cosmos.Emulator.exe/FailOnSslCertificateNameMismatch  | |
| GenCert | Vygenerujte a nainstalujte nový certifikát TLS/SSL podepsaný svým držitelem. Volitelně můžete zahrnout čárkami oddělený seznam dalších názvů DNS pro přístup k emulátoru přes síť. | Microsoft.Azure.Cosmos.Emulator.exe/GenCert =\<dns-names\> |\<dns-names\>: Volitelný čárkami oddělený seznam dalších názvů DNS  |
| DirectPorts |Určuje porty, které chcete použít pro přímé připojení. Výchozí hodnoty jsou 10251,10252,10253,10254. | Microsoft.Azure.Cosmos.Emulator.exe/DirectPorts:\<directports\> | \<directports\>: Seznam 4 portů oddělených čárkami |
| Klíč |Autorizační klíč pro emulátor. Klíč musí být 64bajtový vektor s kódováním base-64. | Microsoft.Azure.Cosmos.Emulator.exe/Key:\<key\> | \<key\>: Klíč musí být kódování Base-64 pro vektor 64-byte.|
| EnableRateLimiting | Určuje, že je povoleno chování omezující četnost požadavků. |Microsoft.Azure.Cosmos.Emulator.exe/EnableRateLimiting | |
| DisableRateLimiting |Určuje, že je zakázáno chování omezující četnost požadavků. |Microsoft.Azure.Cosmos.Emulator.exe/DisableRateLimiting | |
| NoUI | Nezobrazuje uživatelské rozhraní emulátoru. | Microsoft.Azure.Cosmos.Emulator.exe/NoUI | |
| NoExplorer | Nezobrazuje Průzkumníka dat při spuštění. |Microsoft.Azure.Cosmos.Emulator.exe/NoExplorer | | 
| PartitionCount | Určuje maximální počet kontejnerů rozdělený na oddíly. Další informace najdete v tématu [Změna počtu kontejnerů](#set-partitioncount) . | Microsoft.Azure.Cosmos.Emulator.exe/PartitionCount =\<partitioncount\> | \<partitioncount\>: Maximální počet povolených kontejnerů s jedním oddílem. Výchozí hodnota je 25. Maximální povolený počet je 250.|
| DefaultPartitionCount| Určuje výchozí počet oddílů pro kontejner rozdělený na oddíly. | Microsoft.Azure.Cosmos.Emulator.exe/DefaultPartitionCount =\<defaultpartitioncount\> | \<defaultpartitioncount\>Výchozí hodnota je 25.|
| AllowNetworkAccess | Povolí přístup k emulátoru přes síť. \<key_string\> \<file_name\> Abyste mohli povolit přístup k síti, musíte taky předat/Key = nebo/keyfile =. | Microsoft.Azure.Cosmos.Emulator.exe/AllowNetworkAccess/Key = \<key_string\> nebo Microsoft.Azure.Cosmos.Emulator.exe/AllowNetworkAccess/keyfile =\<file_name\>| |
| NoFirewall | Neupravujte pravidla brány firewall, pokud se používá možnost/AllowNetworkAccess. |Microsoft.Azure.Cosmos.Emulator.exe/NoFirewall | |
| GenKeyFile | Vygeneruje nový autorizační klíč a uloží ho do zadaného souboru. Generovaný klíč lze použít s možností /Key nebo/KeyFile. | Microsoft.Azure.Cosmos.Emulator.exe/GenKeyFile =\<path to key file\> | |
| Konzistence | Nastaví výchozí úroveň konzistence pro účet. | Microsoft.Azure.Cosmos.Emulator.exe/Consistency =\<consistency\> | \<consistency\>: Hodnota musí být jedna z následujících [úrovní konzistence](consistency-levels.md): Session, Strong, případný nebo BoundedStaleness. Výchozí hodnota je Session. |
| ? | Zobrazí zprávu nápovědy.| | |

## <a name="change-the-number-of-containers"></a><a id="set-partitioncount"></a>Změna počtu kontejnerů

Ve výchozím nastavení můžete vytvořit až 25 kontejnerů s pevnou velikostí (podporované jenom pomocí sad Azure Cosmos DB SDK) nebo 5 neomezených kontejnerů pomocí emulátoru Azure Cosmos. Změnou hodnoty **PartitionCount** můžete vytvořit až 250 kontejnerů pevné velikosti nebo 50 neomezených kontejnerů, případně jakoukoli kombinaci dvou, která nepřekračuje 250 kontejnerů pevné velikosti (kde jeden neomezený kontejner = 5 kontejnerů pevné velikosti). Nedoporučuje se však nastavit emulátor pro spuštění s více než 200 kontejnery s pevnou velikostí. Z důvodu režie, kterou přidává k diskovým operacím v/v, což vede k nepředvídatelným časovým limitům při použití rozhraní API koncových bodů.

Pokud se pokusíte vytvořit kontejner po překročení aktuálního počtu oddílů, emulátor vyvolá výjimku ServiceUnavailable s následující zprávou.

Omlouváme se, ale v tuto chvíli máme vysokou poptávku a v tuto chvíli nemůže váš požadavek splnit. Průběžně pracujeme na zajištění více a větší kapacity online a pomůžeme vám to zkusit znovu.
ActivityId: 12345678-1234-1234-1234-123456789ABC "

Pokud chcete změnit počet kontejnerů dostupných v emulátoru Azure Cosmos, spusťte následující postup:

1. Kliknutím pravým tlačítkem na ikonu **emulátoru Azure Cosmos DB** na hlavním panelu a kliknutím na **resetovat data**odstraňte všechna místní data emulátoru Azure Cosmos.
2. Odstraní všechna data emulátoru v této složce `%LOCALAPPDATA%\CosmosDBEmulator` .
3. Ukončete všechny otevřené instance tak, že kliknete pravým tlačítkem myši na ikonu **emulátoru služby Azure Cosmos DB** na hlavním panelu systému a potom kliknete na **Exit** (Konec). Ukončení všech instancí může chvíli trvat.
4. Nainstalujte nejnovější verzi [emulátoru Azure Cosmos](https://aka.ms/cosmosdb-emulator).
5. Spusťte emulátor s příznakem PartitionCount nastaveným na hodnotu < = 250. Například: `C:\Program Files\Azure Cosmos DB Emulator> Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Řízení emulátoru

Emulátor se dodává s modulem PowerShellu pro spuštění, zastavení, odinstalaci a načtení stavu služby. Spuštěním následující rutiny použijte modul prostředí PowerShell:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

nebo umístěte `PSModules` adresář na svůj `PSModulesPath` a naimportujte ho, jak je znázorněno v následujícím příkazu:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Zde je uveden seznam příkazů pro řízení emulátoru z PowerShellu:

### `Get-CosmosDbEmulatorStatus`

**Syntax**

`Get-CosmosDbEmulatorStatus`

**Poznámky**

Vrátí jednu z těchto hodnot ServiceControllerStatus: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running nebo ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Syntax**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Poznámky**

Spustí emulátor. Ve výchozím nastavení tento příkaz čeká, dokud emulátor nebude připraven přijímat požadavky. Pokud chcete, aby se rutina vrátila, jakmile spustí emulátor, použijte možnost -NoWait.

### `Stop-CosmosDbEmulator`

**Syntax**

 `Stop-CosmosDbEmulator [-NoWait]`

**Poznámky**

Zastaví emulátor. Ve výchozím nastavení tento příkaz čeká, až emulátor je zcela vypnutý. Pokud chcete, aby se rutina vrátila, jakmile se emulátor začne vypínat, použijte možnost -NoWait.

### `Uninstall-CosmosDbEmulator`

**Syntax**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Poznámky**

Odinstaluje emulátor a volitelně odstraní úplný obsah $env:LOCALAPPDATA\CosmosDbEmulator.
Rutina zajišťuje zastavení emulátoru před jeho odinstalací.

## <a name="running-on-docker"></a>Spuštění v Dockeru

Emulátor Azure Cosmos můžete spustit na Docker for Windows. Emulátor nefunguje v aplikaci Docker for Oracle Linux.

Jakmile [Docker for Windows](https://www.docker.com/docker-windows) nainstalujete, přepněte na kontejnery Windows tak, že kliknete pravým tlačítkem myši na ikonu Dockeru na panelu nástrojů a vyberete **Switch to Windows containers** (Přepnout na kontejnery Windows).

Potom si spuštěním následujícího příkazu z vašeho oblíbeného prostředí stáhněte image emulátoru z Centra Dockeru.

```bash
docker pull mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```
Pokud chcete image spustit, spusťte následující příkazy.

Z příkazového řádku:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```

> [!NOTE]
> Pokud se zobrazí chyba konfliktu portů (zadaný port se už používá) při spuštění příkazu Docker Run, můžete předat vlastní port změnou čísel portů. Můžete například změnit "-p 8081:8081" na "-p 443:8081"

Z PowerShellu:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator

```

Odpověď vypadá podobně jako následující zpráva:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Nyní použijte koncový bod a hlavní klíč z odpovědi v klientovi a importujte certifikát TLS/SSL do hostitele. Certifikát TLS/SSL naimportujete z příkazového řádku správce následujícím způsobem:

Z příkazového řádku:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

Z PowerShellu:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Po spuštění emulátoru se zavře interaktivní prostředí, aby se ukončil kontejner emulátoru.

Pokud chcete otevřít Průzkumníka dat, přejděte v prohlížeči na následující adresu URL. Koncový bod emulátoru je uveden ve zprávě s odpovědí uvedené výše.

**protokol \: // https** \<emulator endpoint provided in response> **/_explorer/index.html**

Pokud máte klientskou aplikaci .NET spuštěnou v kontejneru Docker platformy Linux a pokud používáte emulátor Azure Cosmos na hostitelském počítači, použijte prosím následující oddíl pro systém Linux k importu certifikátu do kontejneru Docker pro Linux.

## <a name="running-on-mac-or-linux"></a>Spuštění v systému Mac nebo Linux<a id="mac"></a>

V současné době se emulátor Cosmos dá spustit jenom ve Windows. Uživatelé se systémem Mac nebo Linux můžou spustit emulátor na virtuálním počítači s Windows, který hostuje hypervisor, jako je Parallel nebo VirtualBox. Tady je postup, jak to povolit.

Ve virtuálním počítači s Windows spusťte níže uvedený příkaz a poznamenejte si adresu IPv4.

```cmd
ipconfig.exe
```

V rámci aplikace potřebujete změnit identifikátor URI, který se používá jako koncový bod pro použití adresy IPv4 vrácené funkcí `ipconfig.exe` místo `localhost` .

V dalším kroku z na virtuálním počítači s Windows spustíte emulátor Cosmos z příkazového řádku pomocí následujících možností.

```cmd
Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

Nakonec musíme importovat certifikát CA emulátoru do prostředí Linux nebo Mac.

### <a name="linux"></a>Linux

Pokud pracujete na platformě Linux, aplikace .NET Relay na OpenSSL provede ověření:

1. [Exportujte certifikát ve formátu PFX](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-tlsssl-certificate) (při volbě exportu privátního klíče je k dispozici PFX). 

1. Zkopírujte tento soubor PFX do svého prostředí Linux.

1. Převést soubor PFX na soubor CRT

   ```bash
   openssl pkcs12 -in YourPFX.pfx -clcerts -nokeys -out YourCTR.crt
   ```

1. Zkopírujte soubor CRT do složky, která obsahuje vlastní certifikáty v distribuci systému Linux. V Debian distribucích se obvykle nachází na `/usr/local/share/ca-certificates/` .

   ```bash
   cp YourCTR.crt /usr/local/share/ca-certificates/
   ```

1. Aktualizujte certifikáty certifikační autority, které budou aktualizovat `/etc/ssl/certs/` složku.

   ```bash
   update-ca-certificates
   ```

### <a name="macos"></a>macOS

Pokud pracujete na Macu, použijte následující postup:

1. [Exportujte certifikát ve formátu PFX](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-tlsssl-certificate) (při volbě exportu privátního klíče je k dispozici PFX).

1. Zkopírujte tento soubor PFX do prostředí Mac.

1. Otevřete aplikaci pro *přístup k řetězci klíčů* a IMPORTUJTE soubor PFX.

1. Otevřete seznam certifikátů a Identifikujte ho názvem `localhost` .

1. Otevřete kontextovou nabídku pro tuto konkrétní položku, vyberte možnost *získat položku* a v části *důvěřovat*  >  *při použití tohoto certifikátu* vyberte možnost *vždy důvěřovat*. 

   :::image type="content" source="./media/local-emulator/mac-trust-certificate.png" alt-text="Otevřete kontextovou nabídku pro tuto konkrétní položku, vyberte získat položku a v části důvěřovat – při použití tohoto certifikátu vyberte možnost vždy důvěřovat.":::

Po provedení tohoto postupu bude vaše prostředí důvěřovat certifikátu používanému emulátorem při připojování k IP adrese, kterou zveřejňuje `/AllowNetworkAccess` .

## <a name="troubleshooting"></a>Řešení potíží

Následující tipy vám pomůžou při řešení problémů, ke kterým dochází v emulátoru Azure Cosmos:

- Pokud jste nainstalovali novou verzi emulátoru a dochází k chybám, proveďte obnovení dat. Data můžete obnovit kliknutím pravým tlačítkem na ikonu emulátoru Azure Cosmos na hlavním panelu systému a kliknutím na resetovat data.... Pokud to neopraví chyby, můžete emulátor a všechny starší verze emulátoru odinstalovat, odebrat adresář "C:\Program files\Azure Cosmos DB emulátor" a přeinstalovat emulátor. Pokyny najdete v části [Odinstalace místního emulátoru](#uninstall).

- Pokud dojde k selhání emulátoru Azure Cosmos, shromážděte soubory s výpisem paměti ze složky '%LOCALAPPDATA%\CrashDumps ', Zkomprimujte je a otevřete lístek podpory z [Azure Portal](https://portal.azure.com).

- Pokud dojde k chybě `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe` , může to být příznak, ve kterém jsou čítače výkonu v poškozeném stavu. Obvykle se při spuštění následujícího příkazu z příkazového řádku správce vyřeší problém:

  ```cmd
  lodctr /R
   ```

- Pokud narazíte na problém s připojením, [Shromážděte trasovací soubory](#trace-files), Zkomprimujte je a otevřete lístek podpory v [Azure Portal](https://portal.azure.com).

- Pokud se zobrazí zpráva **Služba není dostupná**, pravděpodobně se emulátoru nedaří inicializovat sadu síťových protokolů. Zkontrolujte, zda máte nainstalovaného klienta Pulse Secure nebo klienta Juniper Networks, protože potíže mohou způsobovat jejich ovladače síťových filtrů. Odinstalace ovladačů síťových filtrů třetích stran obvykle potíže vyřeší. Případně můžete spustit emulátor pomocí/DisableRIO, který přepne síťovou komunikaci emulátoru na normální rozhraní Winsock. 

- Pokud emulátor běží, když počítač přechází do režimu spánku nebo instaluje nějaké aktualizace operačního systému, může se zobrazit zpráva, že **služba momentálně není dostupná**. Obnovte data emulátoru tak, že kliknete pravým tlačítkem na ikonu, která se zobrazuje v oznamovacím panelu Windows, a vyberete **resetovat data**.

### <a name="collect-trace-files"></a><a id="trace-files"></a>Shromažďování trasovacích souborů

Pokud chcete shromažďovat trasovací soubory pro ladění, spusťte z příkazového řádku pro správu následující příkazy:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `Microsoft.Azure.Cosmos.Emulator.exe /shutdown`. Sledujte hlavní panel systému a ujistěte se, že program je vypnutý. Může to chvíli trvat. V uživatelském rozhraní emulátoru Azure Cosmos můžete taky jenom kliknout na **konec** .
3. `Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces`
4. `Microsoft.Azure.Cosmos.Emulator.exe`
5. Reprodukujte problém. Pokud Průzkumník dat nefunguje, stačí několik sekund čekat na otevření prohlížeče a zachytit chybu.
6. `Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces`
7. Přejděte do složky `%ProgramFiles%\Azure Cosmos DB Emulator` a vyhledejte soubor docdbemulator_000001.etl.
8. Otevřete lístek podpory v [Azure Portal](https://portal.azure.com) a přidejte soubor. ETL spolu s postupem reprodukci.

### <a name="uninstall-the-local-emulator"></a><a id="uninstall"></a>Odinstalace místního emulátoru

1. Zavřete všechny otevřené instance místního emulátoru tak, že kliknete pravým tlačítkem na ikonu emulátoru Azure Cosmos na hlavním panelu a pak kliknete na Zavřít. Ukončení všech instancí může chvíli trvat.
2. Do vyhledávacího pole ve Windows zadejte **Programy a funkce** a klikněte na výsledek **Programy a funkce (nastavení systému)**.
3. V seznamu aplikací se posuňte na položku **Azure Cosmos DB Emulator**, vyberte ji, klikněte na **Odinstalovat**, potvrďte a znovu klikněte na **Odinstalovat**.
4. Když je aplikace odinstalovaná, přejděte do složky `%LOCALAPPDATA%\CosmosDBEmulator` a odstraňte ji.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak používat místní emulátor pro bezplatný místní vývoj. Teď můžete přejít k dalšímu kurzu a Naučte se exportovat certifikáty TLS/SSL emulátoru.

> [!div class="nextstepaction"]
> [Export certifikátů emulátoru Azure Cosmos](local-emulator-export-ssl-certificates.md)
