---
title: Správa prostředků Azure Cosmos DB pomocí Průzkumník služby Azure Storage
description: Naučte se, jak se připojit k Azure Cosmos DB a spravovat jeho prostředky pomocí Průzkumník služby Azure Storage.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/23/2019
ms.author: dech
ms.custom: seodec18, has-adal-ref
ms.openlocfilehash: a43e49ab29780e718139b677b780443e5e31e2ab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85262238"
---
# <a name="work-with-data-using-azure-storage-explorer"></a>Práce s daty s využitím Průzkumníka služby Azure Storage

Použití služby Azure Cosmos DB v Průzkumníku služby Azure Storage umožňuje uživatelům spravovat entity Azure Cosmos DB, manipulovat s daty, aktualizovat uložené procedury a triggery, stejně jako další entity Azure, jako jsou objekty blob a fronty služby Storage. Nyní můžete pomocí jednoho nástroje spravovat různé entity Azure na jednom místě. V tuto chvíli Průzkumník služby Azure Storage podporuje účty Cosmos nakonfigurované pro rozhraní API SQL, MongoDB, Graph a Table.


## <a name="prerequisites"></a>Požadavky

Účet Cosmos s rozhraním SQL API nebo rozhraním API Azure Cosmos DB pro MongoDB. Pokud účet nemáte, můžete si ho vytvořit na webu Azure Portal, jak je popsáno v tématu [Azure Cosmos DB: Vytvoření webové aplikace SQL API s využitím .NET a webu Azure Portal](create-sql-api-dotnet.md).

## <a name="installation"></a>Instalace

Nainstalujte nejnovější části Průzkumníka služby Azure Storage: [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/). V současné době podporujeme verze pro Windows, Linux a Mac.

## <a name="connect-to-an-azure-subscription"></a>Připojení k předplatnému Azure

1. Po instalaci **Průzkumník služby Azure Storage**klikněte na ikonu **modulu plug-in** na levé straně, jak je znázorněno na následujícím obrázku:

   :::image type="content" source="./media/storage-explorer/plug-in-icon.png" alt-text="Ikona modulu plug-in":::

2. Vyberte **Přidat účet Azure** a pak klikněte na **Přihlásit**.

   :::image type="content" source="./media/storage-explorer/connect-to-azure-subscription.png" alt-text="Připojení k předplatnému Azure":::

2. V dialogovém okně **Přihlášení k Azure** vyberte **Přihlásit** a zadejte své přihlašovací údaje Azure.

    :::image type="content" source="./media/storage-explorer/sign-in.png" alt-text="Přihlásit":::

3. Vyberte ze seznamu své předplatné a klikněte na **Použít**.

    :::image type="content" source="./media/storage-explorer/apply-subscription.png" alt-text="Vyrovnat":::

    Podokno Průzkumníka se aktualizuje a zobrazí účty ve vybraném předplatném.

    :::image type="content" source="./media/storage-explorer/account-list.png" alt-text="Seznam účtů":::

    Právě jste úspěšně propojili svůj **účet služby Cosmos DB** se svým předplatným Azure.

## <a name="connect-to-azure-cosmos-db-by-using-a-connection-string"></a>Připojení ke službě Azure Cosmos DB pomocí připojovacího řetězce

Alternativním způsobem, jak se připojit ke službě Azure Cosmos DB, je použít připojovací řetězec. Pomocí následujícího postupu se připojte pomocí připojovacího řetězce.

1. V levém stromě vyhledejte **Místní a připojené**, klikněte pravým tlačítkem na **Účty služby Cosmos DB** a zvolte **Připojit ke službě Cosmos DB...**

    :::image type="content" source="./media/storage-explorer/connect-to-db-by-connection-string.png" alt-text="Připojení ke službě Cosmos DB pomocí připojovacího řetězce":::

2. Aktuálně se podporují pouze rozhraní SQL a Table API. Zvolte rozhraní API, vložte **Připojovací řetězec**, zadejte **Popisek účtu**, klikněte na **Další** a zkontrolujte souhrn a pak se kliknutím na **Připojit** připojte k účtu služby Azure Cosmos DB. Informace o načtení primárního připojovacího řetězce najdete v tématu [získání připojovacího řetězce](manage-with-powershell.md#list-keys).

    :::image type="content" source="./media/storage-explorer/connection-string.png" alt-text="Připojovací řetězec":::

## <a name="connect-to-azure-cosmos-db-by-using-local-emulator"></a>Připojení ke službě Azure Cosmos DB pomocí místního emulátoru

Pomocí následujících kroků se připojte ke službě Azure Cosmos DB pomocí emulátoru, který aktuálně podporuje pouze účet SQL.

1. Nainstalujte emulátor a spusťte ho. Pokyny k instalaci emulátoru najdete v tématu [Emulátor služby Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

2. V levém stromě vyhledejte **Místní a připojené**, klikněte pravým tlačítkem na **Účty služby Cosmos DB** a zvolte **Připojit k emulátoru služby Cosmos DB...**

    :::image type="content" source="./media/storage-explorer/emulator-entry.png" alt-text="Připojení ke službě Cosmos DB pomocí emulátoru":::

3. Aktuálně se podporuje pouze rozhraní SQL API. Vložte **Připojovací řetězec**, zadejte **Popisek účtu**, klikněte na **Další** a zkontrolujte souhrn a pak se kliknutím na **Připojit** připojte k účtu služby Azure Cosmos DB. Informace o načtení primárního připojovacího řetězce najdete v tématu [získání připojovacího řetězce](manage-with-powershell.md#list-keys).

    :::image type="content" source="./media/storage-explorer/emulator-dialog.png" alt-text="Dialogové okno Připojení ke službě Cosmos DB pomocí emulátoru":::


## <a name="azure-cosmos-db-resource-management"></a>Správa prostředků Azure Cosmos DB

Účet služby Azure Cosmos DB můžete spravovat pomocí následujících operací:
* Otevření účtu na webu Azure Portal
* Přidání prostředku do seznamu Rychlý přístup
* Vyhledávání a aktualizace prostředků
* Vytvářet a odstraňovat databáze
* Vytvářet a odstraňovat kolekce
* Vytváření, úpravy, odstraňování a filtrování dokumentů
* Správa uložených procedur, triggerů a funkcí definovaných uživatelem

### <a name="quick-access-tasks"></a>Úlohy rychlého přístupu

Po kliknutí pravým tlačítkem na předplatné v podokně Průzkumníka můžete provádět řadu úloh rychlého přístupu:

* Po kliknutí pravým tlačítkem na účet nebo databázi Azure Cosmos DB můžete zvolit **Otevřít na portálu** a spravovat prostředek v prohlížeči na webu Azure Portal.

     :::image type="content" source="./media/storage-explorer/open-in-portal.png" alt-text="Otevřít na portálu":::

* Účet, databázi nebo kolekci Azure Cosmos DB můžete také přidat do seznamu **Rychlý přístup**.
* Možnost **Hledat odsud** umožňuje hledat klíčová slova v zadané cestě.

    :::image type="content" source="./media/storage-explorer/search-from-here.png" alt-text="Hledat odsud":::

### <a name="database-and-collection-management"></a>Správa databází a kolekcí
#### <a name="create-a-database"></a>Vytvoření databáze
-   Klikněte pravým tlačítkem na účet služby Azure Cosmos DB, zvolte **Vytvořit databázi**, zadejte název databáze a stisknutím klávesy **Enter** dokončete vytváření.

    :::image type="content" source="./media/storage-explorer/create-database.png" alt-text="Vytvoření databáze":::

#### <a name="delete-a-database"></a>Odstranění databáze
- Klikněte pravým tlačítkem na databázi, klikněte na **Odstranit databázi** a v automaticky otevíraném okně klikněte na **Ano**. Uzel databáze se odstraní a účet služby Azure Cosmos DB se automaticky aktualizuje.

    :::image type="content" source="./media/storage-explorer/delete-database1.png" alt-text="Odstranění databáze 1":::

    :::image type="content" source="./media/storage-explorer/delete-database2.png" alt-text="Odstranění databáze 2":::

#### <a name="create-a-collection"></a>Vytvoření kolekce
1. Klikněte pravým tlačítkem na databázi, zvolte možnost **vytvořit kolekci**a zadejte následující informace, jako je **ID kolekce**, **kapacita úložiště**atd. Dokončete kliknutím na **OK** .

    :::image type="content" source="./media/storage-explorer/create-collection.png" alt-text="Vytvoření kolekce 1":::

    :::image type="content" source="./media/storage-explorer/create-collection2.png" alt-text="Vytvoření kolekce 2":::

2. Vyberte **Neomezená**, abyste mohli zadat klíč oddílu, a kliknutím na **OK** vytváření dokončete.

    Pokud se při vytváření kolekce použije klíč oddílu, po vytvoření se hodnota klíče oddílu pro kolekci nedá změnit.

    :::image type="content" source="./media/storage-explorer/partitionkey.png" alt-text="Klíč oddílu":::

#### <a name="delete-a-collection"></a>Odstranění kolekce
- Klikněte pravým tlačítkem na kolekci, klikněte na **Odstranit kolekci** a v automaticky otevíraném okně pak klikněte na **Ano**.

    Uzel kolekce se odstraní a databáze se automaticky aktualizuje.

    :::image type="content" source="./media/storage-explorer/delete-collection.png" alt-text="Odstranění kolekce":::

### <a name="document-management"></a>Správa dokumentů

#### <a name="create-and-modify-documents"></a>Vytváření a úpravy dokumentů
- Pokud chcete vytvořit nový dokument, otevřete v levém okně **Dokumenty**, klikněte na **Nový dokument**, v pravém podokně upravte obsah a pak klikněte na **Uložit**. Můžete také aktualizovat existující dokument a pak kliknout na **Uložit**. Změny můžete zahodit kliknutím na **Zahodit**.

    :::image type="content" source="./media/storage-explorer/document.png" alt-text="Dokumentů":::

#### <a name="delete-a-document"></a>Odstranění dokumentu
- Kliknutím na tlačítko **Odstranit** odstraňte vybraný dokument.

#### <a name="query-for-documents"></a>Dotazování dokumentů
- Upravte filtr dokumentů zadáním [příkazu jazyka SQL](how-to-sql-query.md) a pak klikněte na **Použít**.

    :::image type="content" source="./media/storage-explorer/document-filter.png" alt-text="Filtr dokumentů":::



### <a name="graph-management"></a>Správa grafů

#### <a name="create-and-modify-vertex"></a>Vytvoření a úprava vrcholu
1. Pokud chcete vytvořit nový vrchol, v levém okně otevřete **Graph**, klikněte na **Nový vrchol**, upravte obsah a pak klikněte na **OK**.
2. Pokud chcete upravit existující vrchol, klikněte na ikonu pera v pravém podokně.

    :::image type="content" source="./media/storage-explorer/vertex.png" alt-text="Graph":::

#### <a name="delete-a-graph"></a>Odstranění grafu
- Pokud chcete odstranit vrchol, klikněte na ikonu koše vedle názvu vrcholu.

#### <a name="filter-for-graph"></a>Filtrování grafu
- Upravte filtr grafu zadáním [dotazu Gremlin](gremlin-support.md) a pak klikněte na **Použít filtr**.

    :::image type="content" source="./media/storage-explorer/graph-filter.png" alt-text="Filtr grafu":::

### <a name="table-management"></a>Správa tabulek

#### <a name="create-and-modify-table"></a>Vytvoření a úprava tabulky
1. Pokud chcete vytvořit novou tabulku, v levém okně otevřete **Entity**, klikněte na **Přidat**, upravte obsah dialogového okna **Přidat entitu**, přidejte vlastnost kliknutím na tlačítko **Přidat vlastnost** a pak klikněte na **Vložit**.
2. Pokud chcete upravit tabulku, klikněte na **Upravit**, upravte obsah a pak klikněte na **Aktualizovat**.

    :::image type="content" source="./media/storage-explorer/table.png" alt-text="Tabulka":::

#### <a name="import-and-export-table"></a>Import a export tabulky
1. Pokud chcete importovat tabulku, klikněte na tlačítko **Importovat** a zvolte existující tabulku.
2. Pokud chcete exportovat tabulku, klikněte na tlačítko **Exportovat** a zvolte cíl.

    :::image type="content" source="./media/storage-explorer/table-import-export.png" alt-text="Import a export tabulky":::

#### <a name="delete-entities"></a>Odstranění entit
- Vyberte entity a klikněte na tlačítko **Odstranit**.

    :::image type="content" source="./media/storage-explorer/table-delete.png" alt-text="Odstranění tabulky":::

#### <a name="query-table"></a>Dotaz na tabulku
- Klikněte na tlačítko **Dotaz**, zadejte podmínky dotazu a pak klikněte na tlačítko **Provést dotaz**. Zavřete podokno Dotaz kliknutím na tlačítko **Zavřít dotaz**.

    :::image type="content" source="./media/storage-explorer/table-query.png" alt-text="Dotaz na tabulku":::

### <a name="manage-stored-procedures-triggers-and-udfs"></a>Správa uložených procedur, triggerů a funkcí definovaných uživatelem
* Pokud chcete vytvořit uloženou proceduru, v levém stromu klikněte pravým tlačítkem na **Uložená procedura**, zvolte **Vytvořit uloženou proceduru**, v levém okně zadejte název, v pravém okně zadejte skripty uložené procedury a pak klikněte na **Vytvořit**.
* Můžete také upravovat existující uložené procedury tak, že na ně dvakrát kliknete, provedete aktualizaci a pak kliknutím na **Aktualizovat** uložíte změny nebo je kliknutím na **Zahodit** zrušíte.

    :::image type="content" source="./media/storage-explorer/stored-procedure.png" alt-text="Uložená procedura":::
* Operace týkající se **triggerů** a **funkcí definovaných uživatelem** jsou podobné **uloženým procedurám**.

## <a name="troubleshooting"></a>Řešení potíží

[Azure Cosmos DB v Průzkumníku služby Storage](https://docs.microsoft.com/azure/cosmos-db/storage-explorer) je samostatná aplikace umožňující připojení k účtům služby Azure Cosmos DB hostovaným v Azure a suverénních cloudech z Windows, macOS nebo Linuxu. Umožňuje správu entit Azure Cosmos DB, manipulaci s daty, aktualizace uložených procedur a triggerů, stejně jako dalších entit Azure, jako jsou objekty blob a fronty služby Storage.

Toto jsou řešení běžných problémů, ke kterým dochází ve službě Azure Cosmos DB v Průzkumníku služby Storage.

### <a name="sign-in-issues"></a>Problémy s přihlášením

Než budete pokračovat, zkuste svou aplikaci restartovat a podívejte se, jestli se tím problém nevyřeší.

#### <a name="self-signed-certificate-in-certificate-chain"></a>Certifikát podepsaný svým držitelem v řetězu certifikátů

Tato chyba se může zobrazit z několika důvodů, z nichž dva nejčastější jsou tyto:

+ Jste za *transparentní proxy*, což znamená, že někdo (například vaše IT oddělení) zachycuje provoz https, dešifruje ho a pak ho šifruje pomocí certifikátu podepsaného svým držitelem.

+ Spouštíte software, například antivirový software, který vkládá certifikáty TLS/SSL podepsané svým držitelem do zpráv HTTPS, které obdržíte.

Pokud Průzkumník služby Storage narazí na některý z těchto certifikátů podepsaných svým držitelem, už nemůže mít jistotu, že se s přijímanými zprávami protokolu HTTPS nemanipulovalo. Pokud však máte kopii příslušného certifikátu podepsaného svým držitelem, můžete Průzkumníku služby Storage sdělit, aby mu důvěřoval. Pokud si nejste jisti, kdo certifikát vkládá, můžete to sami zkusit zjistit provedením následujících kroků:

1. Nainstalovat OpenSSL
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (stačí jakákoli z odlehčených verzí)
     - Mac a Linux: Mělo by být součástí operačního systému.
2. Spustit OpenSSL
    - Windows: Přejděte do adresáře instalace, pak do složky **/bin/** a dvakrát klikněte na soubor **openssl.exe**.
    - Mac a Linux: V terminálu spusťte příkaz **openssl**.
3. Spusťte příkaz `s_client -showcerts -connect microsoft.com:443`.
4. Vyhledejte certifikáty podepsané svým držitelem. Pokud si nejste jisti, které certifikáty jsou podepsané svým držitelem, hledejte certifikáty, jejichž předmět (s:) je stejný jako vystavitel (i:).
5.  Jakmile najdete nějaké certifikáty podepsané svým držitelem, zkopírujte u jednotlivých certifikátů veškerý text začínající na **-----BEGIN CERTIFICATE-----** a končící na **-----END CERTIFICATE-----** (včetně) a zkopírujte ho do nového souboru .cer.
6.  Otevřete Průzkumník služby Storage a pak pokračujte v **úpravách**  >  **certifikátů SSL**  >  **Import certifikátů**. Pomocí nástroje pro výběr souborů vyhledejte, vyberte a otevřete soubory .cer, které jste vytvořili.

Pokud se vám podle výše uvedeného postupu nepodaří najít žádné certifikáty podepsané svým držitelem, můžete získat další nápovědu odesláním zpětné vazby.

#### <a name="unable-to-retrieve-subscriptions"></a>Nelze načíst předplatná

Pokud se vám po úspěšném přihlášení nedaří načíst vaše předplatná:

- Přihlaste se k webu [Azure Portal](https://portal.azure.com/) a ověřte, že má váš účet přístup k předplatným.
- Ujistěte se, že jste se přihlásili s použitím správného prostředí ([Azure](https://portal.azure.com/), [Azure (Čína)](https://portal.azure.cn/), [Azure (Německo)](https://portal.microsoftazure.de/), [Azure pro vládu USA](https://portal.azure.us/) nebo vlastní prostředí nebo Azure Stack).
- Pokud se nacházíte za proxy serverem, ujistěte se, že jste správně nakonfigurovali proxy Průzkumníka služby Storage.
- Zkuste účet odebrat a znovu přidat.
- Zkuste z domovského adresáře (např. C:\Users\ContosoUser) odstranit následující soubory a pak znovu přidat účet:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Při přihlašování sledujte případné chybové zprávy v konzole vývojářských nástrojů (F12).

:::image type="content" source="./media/storage-explorer/console.png" alt-text="stromu":::

#### <a name="unable-to-see-the-authentication-page"></a>Ověřovací stránka se nezobrazuje

Pokud se vám nezobrazuje ověřovací stránka:

- V závislosti na rychlosti vašeho připojení může načtení přihlašovací stránky nějakou dobu trvat. Počkejte alespoň minutu, než dialogové okno ověřování zavřete.
- Pokud se nacházíte za proxy serverem, ujistěte se, že jste správně nakonfigurovali proxy Průzkumníka služby Storage.
- Stisknutím klávesy F12 otevřete konzolu pro vývojáře. V konzole pro vývojáře sledujte odpovědi a podívejte se, jestli nenajdete nějaké vodítko k tomu, proč ověřování nefunguje.

#### <a name="cannot-remove-account"></a>Účet se nedá odebrat

Pokud nemůžete odebrat účet nebo pokud odkaz na opětovné ověření nic nedělá:

- Zkuste z domovského adresáře odstranit následující soubory a pak znovu přidat účet:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Pokud chcete odebrat prostředky služby Storage připojené sdíleným přístupovým podpisem, odstraňte:
  - Složku %AppData%/StorageExplorer ve Windows.
  - /Users/<your_name>/Library/Application Support podpora/StorageExplorer pro Mac
  - Složku ~/.config/StorageExplorer v Linuxu.
  - Pokud tyto soubory odstraníte, **budete muset zadat všechny své přihlašovací údaje znovu**.


### <a name="httphttps-proxy-issue"></a>Problém s proxy HTTP/HTTPS

Při konfiguraci proxy HTTP/HTTPS v Průzkumníku služby Azure Storage nemůžete v levém stromu vypsat uzly služby Azure Cosmos DB. Prozatím můžete jako alternativní řešení použít Průzkumníka dat služby Azure Cosmos DB na webu Azure Portal.

### <a name="development-node-under-local-and-attached-node-issue"></a>Problém s uzlem Vývoj v uzlu Místní a připojené

Po kliknutí na uzel Vývoj v uzlu Místní a připojené v levém stromu nedojde k žádné reakci.  Jedná se o očekávané chování. Místní emulátor služby Azure Cosmos DB bude podporovaný v další vydané verzi.

:::image type="content" source="./media/storage-explorer/development.png" alt-text="Uzel Vývoj":::

### <a name="attaching-azure-cosmos-db-account-in-local-and-attached-node-error"></a>Chyba připojení účtu služby Azure Cosmos DB v uzlu Místní a připojené

Pokud se po připojení účtu služby Azure Cosmos DB v uzlu Místní a připojené zobrazí následující chyba, zkontrolujte, jestli používáte správný připojovací řetězec.

:::image type="content" source="./media/storage-explorer/attached-error.png" alt-text="Chyba připojení účtu služby Azure Cosmos DB v uzlu Místní a připojené":::

### <a name="expand-azure-cosmos-db-node-error"></a>Chyba rozbalení uzlu služby Azure Cosmos DB

Při pokusu o rozbalení uzlů v levém stromu se může zobrazit následující chyba.

:::image type="content" source="./media/storage-explorer/expand-error.png" alt-text="Chyba rozbalení":::

Vyzkoušejte následující návrhy:

- Zkontrolujte, jestli neprobíhá zřizování účtu služby Azure Cosmos DB, a zkuste to znovu po úspěšném vytvoření účtu.
- Pokud je účet v uzlu Rychlý přístup nebo Místní a připojené, zkontrolujte, jestli se účet neodstranil. Pokud ano, musíte uzel odebrat ručně.

## <a name="contact-us"></a>Kontaktujte nás

Pokud žádná z řešení nefunguje za vás, pošlete e-mail pro Azure Cosmos DB týmu nástrojů pro vývoj ( [cosmosdbtooling@microsoft.com](mailto:cosmosdbtooling@microsoft.com) ) s podrobnostmi o problému a opravte problémy.

## <a name="next-steps"></a>Další kroky

* Podívejte se na následující video a zjistěte, jak používat službu Azure Cosmos DB v Průzkumníku služby Azure Storage: [Použití služby Azure Cosmos DB v Průzkumníku služby Azure Storage](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be).
* Další informace o Průzkumníku služby Storage a připojení dalších služeb najdete v tématu [Začínáme s Průzkumníkem služby Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).
