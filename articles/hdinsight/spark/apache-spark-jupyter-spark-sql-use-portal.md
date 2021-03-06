---
title: 'Rychlý Start: Vytvoření clusteru Spark ve službě HDInsight pomocí Azure Portal'
description: V tomto rychlém startu se dozvíte, jak pomocí Azure Portal vytvořit cluster Apache Spark ve službě Azure HDInsight a spustit dotaz Spark SQL.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/25/2020
ms.openlocfilehash: 1d816a84dc8062890633661716cf78aa5ba58527
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2020
ms.locfileid: "82888849"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-portal"></a>Rychlý Start: Vytvoření clusteru Apache Spark ve službě Azure HDInsight pomocí Azure Portal

V tomto rychlém startu použijete Azure Portal k vytvoření clusteru Apache Spark ve službě Azure HDInsight. Pak vytvoříte Poznámkový blok Jupyter a použijete ho ke spouštění dotazů Spark SQL pro Apache Hive tabulek. Azure HDInsight je spravovaná opensourcová analytická služba určená pro podniky. Rozhraní Apache Spark Framework for HDInsight umožňuje rychlé analýzy dat a výpočetní výkon clusteru pomocí zpracování v paměti. Poznámkový blok Jupyter vám umožňuje pracovat s daty, kombinovat kód s textem Markdownu a provádět jednoduché vizualizace.

Podrobné vysvětlení dostupných konfigurací najdete v tématu [Nastavení clusterů v HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Další informace o použití portálu k vytváření clusterů najdete v tématu [Vytvoření clusterů na portálu](../hdinsight-hadoop-create-linux-clusters-portal.md).

Pokud používáte více clusterů společně, budete chtít vytvořit virtuální síť a pokud používáte cluster Spark, budete také chtít použít konektor pro skladiště z podregistru. Další informace najdete v tématu [plánování virtuální sítě pro Azure HDInsight](../hdinsight-plan-virtual-network-deployment.md) a [integrace Apache Spark a Apache Hive pomocí konektoru skladu s podregistru](../interactive-query/apache-hive-warehouse-connector.md).

> [!IMPORTANT]  
> Clustery HDInsight se fakturují za minutu bez ohledu na to, jestli je používáte, nebo ne. Až přestanete cluster používat, nezapomeňte ho odstranit. Další informace najdete v části [Vyčištění prostředků](#clean-up-resources) tohoto článku.

## <a name="prerequisites"></a>Požadavky

Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="create-an-apache-spark-cluster-in-hdinsight"></a>Vytvoření clusteru Apache Spark v HDInsight

Pomocí Azure Portal vytvoříte cluster HDInsight, který jako úložiště clusteru používá objekty blob Azure Storage. Další informace o použití Data Lake Storage Gen2 najdete v tématu [Rychlý start: Nastavení clusterů ve službě HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

1. V horní nabídce vyberte **+ vytvořit prostředek**.

    ![Azure Portal vytvoření prostředku](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create-resource.png "Vytvořit prostředek na webu Azure Portal")

1. Vyberte **Analytics** > **Azure HDInsight** a přejdete na stránku **vytvořit cluster HDInsight** .

1. Na kartě **základy** zadejte následující informace:

    |Vlastnost  |Popis  |
    |---------|---------|
    |Předplatné  | V rozevíracím seznamu vyberte předplatné Azure, které se používá pro cluster. |
    |Skupina prostředků | V rozevíracím seznamu vyberte existující skupinu prostředků nebo vyberte **vytvořit novou**.|
    |Název clusteru | Zadejte globálně jedinečný název.|
    |Oblast   | V rozevíracím seznamu vyberte oblast, ve které se cluster vytvoří. |
    |Typ clusteru| Vyberte vybrat typ clusteru a otevřete seznam. V seznamu vyberte možnost **Spark**.|
    |Verze clusteru|Po výběru typu clusteru bude toto pole automaticky vyplněno výchozí verzí.|
    |Uživatelské jméno přihlášení clusteru| Zadejte uživatelské jméno přihlášení clusteru.  Výchozí název je **admin**. Tento účet použijete k přihlášení do poznámkového bloku Jupyter později v rychlém startu. |
    |Heslo přihlášení clusteru| Zadejte přihlašovací heslo clusteru. |
    |Uživatelské jméno Secure Shell (SSH)| Zadejte uživatelské jméno SSH. V tomto rychlém startu se používá uživatelské jméno SSH **sshuser**. Ve výchozím nastavení má tento účet stejné heslo jako účet *Uživatelské jméno přihlášení clusteru*. |

    ![Vytvoření základních konfigurací clusteru HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-basics-spark.png "Vytvoření clusteru Spark v HDInsight základní konfigurace")

    Vyberte **Další: >>úložiště** pro pokračování na stránku **úložiště** .

1. V části **Úložiště** zadejte tyto hodnoty:

    |Vlastnost  |Popis  |
    |---------|---------|
    |Typ primárního úložiště|Použijte výchozí hodnotu **Azure Storage**.|
    |Metoda výběru|Použijte výchozí hodnotu **vybrat ze seznamu**.|
    |Účet primárního úložiště|Použijte automaticky vyplněnou hodnotu.|
    |Kontejner|Použijte automaticky vyplněnou hodnotu.|

    ![Vytvoření základních konfigurací clusteru HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-storage.png "Vytvoření clusteru Spark v HDInsight základní konfigurace")

    Pokračujte výběrem **Zobrazit + vytvořit** .

1. V nabídce **Revize + vytvořit**vyberte **vytvořit**. Vytvoření clusteru trvá přibližně 20 minut. Než budete moct pokračovat k další relaci, musí se cluster nejdříve vytvořit.

Pokud narazíte na problém s vytvářením clusterů HDInsight, může to být tím, že nemáte správná oprávnění k tomu. Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="create-a-jupyter-notebook"></a>Vytvoření poznámkového bloku Jupyter

Jupyter Notebook je interaktivní prostředí poznámkového bloku, které podporuje různé programovací jazyky. Poznámkový blok umožňuje pracovat s daty, kombinovat kód s textem markdownu a provádět jednoduché vizualizace.

1. Z webového prohlížeče přejděte do `https://CLUSTERNAME.azurehdinsight.net/jupyter`umístění, kde `CLUSTERNAME` je název vašeho clusteru. Po zobrazení výzvy zadejte přihlašovací údaje clusteru.

1. Vyberte **Nový** > **PySpark** a vytvořte Poznámkový blok.

   ![Vytvoření Jupyter Notebook pro spuštění interaktivního dotazu Spark SQL](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Vytvoření Jupyter Notebook pro spuštění interaktivního dotazu Spark SQL")

   Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).

## <a name="run-apache-spark-sql-statements"></a>Spustit Apache Spark příkazy SQL

Jazyk SQL (Structured Query Language) je nejběžnějším a široce používaným jazykem pro dotazování a definování dat. Spark SQL funguje jako rozšíření Apache Spark pro zpracování strukturovaných dat a používá známou syntaxi jazyka SQL.

1. Ověřte, že je jádro připravené. Jádro bude připravené, až se vedle názvu jádra v poznámkovém bloku zobrazí prázdný kroužek. Plný kruh označuje, že je jádro zaneprázdněno.

    ![Apache Hive dotazování v HDInsight](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Dotaz na podregistr v HDInsight")

    Při prvním spuštění poznámkového bloku jádro provede některé úlohy na pozadí. Počkejte, až bude jádro připravené.

1. Do prázdné buňky vložte následující kód a stisknutím **SHIFT + ENTER** kód spusťte. Příkaz vypíše tabulky Hive v clusteru:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Když použijete Jupyter Notebook s clusterem HDInsight, získáte předvolbu `sqlContext` , kterou můžete použít ke spouštění dotazů na podregistr pomocí Spark SQL. `%%sql` říká poznámkovému bloku Jupyter, aby ke spuštění dotazu Hive použil přednastavený kontext `sqlContext`. Dotaz načte prvních 10 řádků z tabulky Hive (**hivesampletable**), která je ve výchozím nastavení k dispozici na všech clusterech HDInsight. Získání výsledků trvá přibližně 30 sekund. Výstup bude vypadat následovně:

    ![Apache Hive dotazování v HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query.png "Dotaz na podregistr v HDInsight")

    Při každém spuštění dotazu v Jupyter se v názvu okna webového prohlížeče zobrazí stav **(Busy)** (Zaneprázdněn) společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu.

1. Spuštěním dalšího dotazu zobrazíte data v tabulce `hivesampletable`.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Obrazovka by se měla aktualizovat a zobrazit výstup dotazu.

    ![Výstup dotazů na podregistr v HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazů na podregistr v HDInsight")

1. V nabídce **Soubor** poznámkového bloku vyberte **Zavřít a zastavit**. Ukončením poznámkového bloku se uvolní prostředky clusteru.

## <a name="clean-up-resources"></a>Vyčištění prostředků

HDInsight ukládá vaše data do Azure Storage nebo Azure Data Lake Storage, takže můžete cluster bezpečně odstranit, pokud se nepoužívá. Účtují se vám také poplatky za cluster HDInsight, a to i v případě, že se už nepoužívá. Vzhledem k tomu, že se poplatky za cluster mnohokrát účtují rychleji než poplatky za úložiště, má ekonomický smysl odstraňovat clustery, když se nepoužívají. Pokud se chystáte hned začít pracovat na kurzu uvedeném v části [Další kroky](#next-steps), měli byste cluster zachovat.

Přepněte zpět na web Azure Portal a vyberte **Odstranit**.

![Azure Portal odstranit cluster HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-azure-portal-delete-cluster.png "Odstranit cluster HDInsight")

Můžete také výběrem názvu skupiny prostředků otevřít stránku skupiny prostředků a pak vybrat **Odstranit skupinu prostředků**. Odstraněním skupiny prostředků odstraníte cluster HDInsight i výchozí účet úložiště.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak vytvořit cluster Apache Spark v HDInsight a spustit základní dotaz Spark SQL. Přejděte k dalšímu kurzu, kde se dozvíte, jak používat cluster HDInsight ke spouštění interaktivních dotazů na ukázkových datech.

> [!div class="nextstepaction"]
> [Spouštění interaktivních dotazů na Apache Spark](./apache-spark-load-data-run-query.md)
