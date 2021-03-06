---
title: Použití Apache Beeline s Apache Hive – Azure HDInsight
description: Naučte se používat klienta Beeline ke spouštění dotazů na podregistr pomocí Hadoop v HDInsight. Beeline je nástroj pro práci s HiveServer2 nad JDBC.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: 3614fac027dd32ab5f5d70f5835432ac3b9b512d
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86207744"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Použití klienta Apache Beeline s Apache Hivem

Naučte se používat [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) ke spouštění dotazů Apache Hive v HDInsight.

Beeline je klient podregistru, který je součástí hlavních uzlů clusteru HDInsight. Pokud se chcete připojit ke klientovi Beeline nainstalovanému v clusteru HDInsight nebo místně nainstalovat Beeline, přečtěte si článek [připojení k Apache Beeline nebo](connect-install-beeline.md)jeho instalace. Beeline používá JDBC pro připojení k HiveServer2, službě hostované v clusteru HDInsight. Beeline můžete použít také k vzdálenému přístupu k podregistru v HDInsight přes Internet. V následujících příkladech jsou uvedeny nejběžnější připojovací řetězce používané pro připojení ke službě HDInsight z Beeline.

## <a name="prerequisites-for-examples"></a>Předpoklady pro příklady

* Cluster Hadoop ve službě HDInsight. Viz Začínáme [se službou HDInsight v systému Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Všimněte si schématu identifikátoru URI pro primární úložiště vašeho clusteru. Například `wasb://` pro Azure Storage pro `abfs://` Azure Data Lake Storage Gen2 nebo `adl://` pro Azure Data Lake Storage Gen1. Pokud je pro Azure Storage povolený zabezpečený přenos, je identifikátor URI `wasbs://` . Další informace najdete v tématu [zabezpečený přenos](../../storage/common/storage-require-secure-transfer.md).

* Možnost 1: klient SSH. Další informace najdete v tématu [připojení ke službě HDInsight (Apache Hadoop) pomocí SSH](../hdinsight-hadoop-linux-use-ssh-unix.md). Většina kroků v tomto dokumentu předpokládá, že používáte Beeline z relace SSH do clusteru.

* Možnost 2: místní klient Beeline.

## <a name="run-a-hive-query"></a>Spuštění dotazu Hive

Tento příklad je založený na použití klienta Beeline z připojení SSH.

1. Otevřete připojení SSH ke clusteru pomocí následujícího kódu. Místo `sshuser` použijte jméno uživatele SSH pro váš cluster a místo `CLUSTERNAME` zadejte název clusteru. Po zobrazení výzvy zadejte heslo pro uživatelský účet SSH.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Připojte se k HiveServer2 pomocí klienta Beeline z otevřené relace SSH zadáním následujícího příkazu:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Příkazy Beeline začínají `!` znakem, například `!help` zobrazí nápovědu. `!`U některých příkazů ale může být vynecháno. Například `help` funguje také.

    K dispozici `!sql` je, který se používá ke spouštění příkazů HiveQL. HiveQL je ale často používaný, takže můžete vynechat předchozí `!sql` . Následující dva příkazy jsou ekvivalentní:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    V novém clusteru je uvedena pouze jedna tabulka: **hivesampletable**.

4. K zobrazení schématu pro hivesampletable použijte následující příkaz:

    ```hiveql
    describe hivesampletable;
    ```

    Tento příkaz vrátí následující informace:

    ```output
    +-----------------------+------------+----------+--+
    |       col_name        | data_type  | comment  |
    +-----------------------+------------+----------+--+
    | clientid              | string     |          |
    | querytime             | string     |          |
    | market                | string     |          |
    | deviceplatform        | string     |          |
    | devicemake            | string     |          |
    | devicemodel           | string     |          |
    | state                 | string     |          |
    | country               | string     |          |
    | querydwelltime        | double     |          |
    | sessionid             | bigint     |          |
    | sessionpagevieworder  | bigint     |          |
    +-----------------------+------------+----------+--+
    ```

    Tyto informace popisují sloupce v tabulce.

5. Zadáním následujících příkazů vytvořte tabulku s názvem **log4jLogs** pomocí ukázkových dat, která jsou součástí clusteru HDInsight: (podle potřeby podle schématu identifikátoru URI Proveďte revizi podle potřeby).

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log'
        GROUP BY t4;
    ```

    Tyto příkazy provedou následující akce:

    |Příkaz |Popis |
    |---|---|
    |ODKLÁDACÍ TABULKA|Pokud tabulka existuje, je odstraněna.|
    |VYTVOŘIT EXTERNÍ TABULKU|Vytvoří **externí** tabulku v podregistru. Externí tabulky ukládají pouze definici tabulky v podregistru. Data zůstanou v původním umístění.|
    |FORMÁT ŘÁDKU|Způsob formátování dat. V tomto případě jsou pole v každém protokolu oddělená mezerou.|
    |ULOŽENO JAKO UMÍSTĚNÍ TEXTFILE|Kde jsou data uložena a v jakém formátu souboru.|
    |SELECT|Vybere počet všech řádků, ve kterých sloupec **T4** obsahuje hodnotu **[Chyba]**. Tento dotaz vrátí hodnotu **3** , protože jsou tři řádky, které obsahují tuto hodnotu.|
    |INPUT__FILE__NAME jako je%. log|Podregistr se pokusí použít schéma pro všechny soubory v adresáři. V tomto případě adresář obsahuje soubory, které neodpovídají schématu. Aby se zabránilo uvolňování dat ve výsledcích, tento příkaz oznamuje podregistru, že by měl vracet pouze data ze souborů končících log. log.|

   > [!NOTE]  
   > Externí tabulky by měly být použity, pokud očekáváte, že budou zdrojová data aktualizována externím zdrojem. Například automatizovaný proces odesílání dat nebo operace MapReduce.
   >
   > Vyřazení externí tabulky **neodstraní data** , pouze definici tabulky.

    Výstup tohoto příkazu je podobný následujícímu textu:

    ```output
    INFO  : Tez session hasn't been created yet. Opening session
    INFO  :

    INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

    INFO  : Map 1: -/-      Reducer 2: 0/1
    INFO  : Map 1: 0/1      Reducer 2: 0/1
    INFO  : Map 1: 0/1      Reducer 2: 0/1
    INFO  : Map 1: 0/1      Reducer 2: 0/1
    INFO  : Map 1: 0/1      Reducer 2: 0/1
    INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
    INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
    INFO  : Map 1: 1/1      Reducer 2: 0/1
    INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
    INFO  : Map 1: 1/1      Reducer 2: 1/1
    +----------+--------+--+
    |   sev    | count  |
    +----------+--------+--+
    | [ERROR]  | 3      |
    +----------+--------+--+
    1 row selected (47.351 seconds)
    ```

6. Konec Beeline:

    ```bash
    !exit
    ```

## <a name="run-a-hiveql-file"></a>Spuštění souboru HiveQL

Tento příklad je pokračování z předchozího příkladu. Pomocí následujících kroků vytvořte soubor a pak ho spusťte pomocí Beeline.

1. Pomocí následujícího příkazu vytvořte soubor s názvem **Query. HQL**:

    ```bash
    nano query.hql
    ```

1. Jako obsah souboru použijte následující text. Tento dotaz vytvoří novou interní **tabulku s názvem**protokolu chyb:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Tyto příkazy provedou následující akce:

    |Příkaz |Popis |
    |---|---|
    |CREATE TABLE, POKUD NEEXISTUJE|Pokud tabulka ještě neexistuje, vytvoří se. Vzhledem k tomu, že se klíčové slovo **External** nepoužívá, vytvoří tento příkaz interní tabulku. Interní tabulky jsou uložené v datovém skladu podregistru a jsou plně spravované podregistrem.|
    |ULOŽENO JAKO ORC|Ukládá data ve formátu optimalizovaného řádku (ORC). Formát ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat z podregistru.|
    |VLOŽIT PŘEPSÁNÍ... VYBRALI|Vybere řádky z tabulky **log4jLogs** , které obsahují **[Error]**, a pak data vloží **do tabulky chyb** .|

    > [!NOTE]  
    > Na rozdíl od externích tabulek odstraní interní tabulka také podkladová data.

1. Pokud chcete soubor uložit, použijte **CTRL +** + **X**, zadejte **Y**a nakonec **ENTER**.

1. K spuštění souboru pomocí Beeline použijte následující:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > `-i`Parametr spustí Beeline a spustí příkazy v `query.hql` souboru. Po dokončení dotazu se zobrazí `jdbc:hive2://headnodehost:10001/>` výzva. Můžete také spustit soubor pomocí `-f` parametru, který ukončí Beeline po dokončení dotazu.

1. Chcete-li ověřit, zda byla **vytvořena tabulka chyb** protokolu chyb, použijte následující příkaz, který vrátí všechny řádky z chyb protokolu **chyb:**

    ```hiveql
    SELECT * from errorLogs;
    ```

    Měly by se vracet tři řádky dat, všechny obsahující **[Error]** v sloupci T4:

    ```output
    +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
    | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
    +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
    | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
    | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
    | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
    +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
    3 rows selected (0.813 seconds)
    ```

## <a name="next-steps"></a>Další kroky

* Obecnější informace o podregistru v HDInsight najdete v tématu [použití Apache Hive s Apache Hadoop v HDInsight](hdinsight-use-hive.md) .

* Další informace o dalších způsobech práce se systémem Hadoop ve službě HDInsight najdete v tématu [použití MapReduce s Apache Hadoop v HDInsight](hdinsight-use-mapreduce.md) .
