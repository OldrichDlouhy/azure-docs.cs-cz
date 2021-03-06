---
title: Použití Apache Hive s PowerShellem ve službě HDInsight – Azure
description: Použití PowerShellu ke spouštění dotazů Apache Hive v Apache Hadoop ve službě Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/24/2019
ms.openlocfilehash: 327a8a0de0d144a5c1d8494a6dd22a8b89a7bd93
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87081045"
---
# <a name="run-apache-hive-queries-using-powershell"></a>Spouštění dotazů Apache Hive pomocí PowerShellu

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Tento dokument poskytuje příklad použití Azure PowerShell ke spouštění dotazů Apache Hive v Apache Hadoop clusteru HDInsight.

> [!NOTE]  
> Tento dokument neposkytuje podrobný popis toho, co HiveQL příkazy, které jsou používány v příkladech. Informace o HiveQL, které se používají v tomto příkladu, najdete v tématu [použití Apache Hive s Apache Hadoop ve službě HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Předpoklady

* Cluster Apache Hadoop v HDInsight. Viz Začínáme [se službou HDInsight v systému Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Prostředí PowerShell [AZ Module](https://docs.microsoft.com/powershell/azure/) installed.

## <a name="run-a-hive-query"></a>Spuštění dotazu Hive

Azure PowerShell poskytuje *rutiny* , které umožňují vzdáleně spouštět dotazy na podregistry v HDInsight. Rutiny interně zavedou volání REST do [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) v clusteru HDInsight.

Při spouštění dotazů na podregistr ve vzdáleném clusteru HDInsight se používají následující rutiny:

* `Connect-AzAccount`: Ověřuje Azure PowerShell předplatného Azure.
* `New-AzHDInsightHiveJobDefinition`: Vytvoří *definici úlohy* pomocí zadaných příkazů HiveQL.
* `Start-AzHDInsightJob`: Odešle definici úlohy do HDInsight a spustí úlohu. Vrátí se objekt *úlohy* .
* `Wait-AzHDInsightJob`: Používá objekt úlohy ke kontrole stavu úlohy. Čeká na dokončení úlohy nebo překročení doby čekání.
* `Get-AzHDInsightJobOutput`: Používá se k načtení výstupu úlohy.
* `Invoke-AzHDInsightHiveJob`: Používá se ke spouštění příkazů HiveQL. Tato rutina zablokuje dotaz na dokončeno a pak vrátí výsledky.
* `Use-AzHDInsightCluster`: Nastaví aktuální cluster, který se má použít pro `Invoke-AzHDInsightHiveJob` příkaz.

Následující kroky ukazují, jak pomocí těchto rutin spustit úlohu v clusteru HDInsight:

1. Pomocí editoru uložte následující kód jako `hivejob.ps1` .

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Otevřete nový příkazový řádek **Azure PowerShell** . Změňte adresáře na umístění `hivejob.ps1` souboru a potom spusťte skript pomocí následujícího příkazu:

    ```azurepowershell
    .\hivejob.ps1
    ```

    Po spuštění skriptu se zobrazí výzva k zadání názvu clusteru a přihlašovacích údajů účtu správce HTTPS/cluster. Může se také zobrazit výzva, abyste se přihlásili ke svému předplatnému Azure.

3. Po dokončení úlohy vrátí informace podobné následujícímu textu:

    ```output
    Display the standard output...
    2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
    2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
    2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id
    ```

4. Jak bylo zmíněno dříve, `Invoke-Hive` lze použít ke spuštění dotazu a čekání na odpověď. Pomocí následujícího skriptu můžete zjistit, jak funguje příkaz Invoke-podregistr:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Výstup bude vypadat jako následující text:

    ```output
    2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
    2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
    2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id
    ```

   > [!NOTE]  
   > Pro delší dotazy HiveQL můžete použít rutinu Azure PowerShell **tady-Strings** nebo soubory skriptu HiveQL. Následující fragment kódu ukazuje, jak použít `Invoke-Hive` rutinu ke spuštění souboru skriptu HiveQL. Soubor skriptu HiveQL musí být nahrán do wasbs://.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Další informace o **řetězcích**najdete v tématu <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">použití řetězců v prostředí Windows PowerShell</a>.

## <a name="troubleshooting"></a>Poradce při potížích

Pokud se po dokončení úlohy nevrátí žádné informace, podívejte se na protokoly chyb. Chcete-li zobrazit informace o chybě pro tuto úlohu, přidejte na konec `hivejob.ps1` souboru, uložte ho a pak ho znovu spusťte.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Tato rutina vrátí informace, které jsou během zpracování úlohy zapisovány do STDERR.

## <a name="summary"></a>Shrnutí

Jak vidíte, Azure PowerShell poskytuje snadný způsob, jak spustit dotazy na podregistr v clusteru HDInsight, jak sledovat stav úlohy a načíst výstup.

## <a name="next-steps"></a>Další kroky

Obecné informace o podregistru v HDInsight:

* [Použití Apache Hive s Apache Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o dalších způsobech, jak můžete pracovat se systémem Hadoop ve službě HDInsight:

* [Použití MapReduce s Apache Hadoop v HDInsight](hdinsight-use-mapreduce.md)
