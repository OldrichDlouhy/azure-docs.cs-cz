---
title: 'Kurz: Začínáme s analýzou Sparku'
description: V tomto kurzu se naučíte základní kroky pro nastavení a používání Azure synapse Analytics.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 5c6b35c1d9f00cae8fc688569e3a491679900995
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101482"
---
# <a name="analyze-with-apache-spark"></a>Analýza pomocí Apache Spark

V tomto kurzu se seznámíte se základními kroky pro načtení a analýzu dat pomocí Apache Spark pro Azure synapse.

## <a name="load-the-nyc-taxi-data-into-the-spark-nyctaxi-database"></a>Načtení dat taxislužby NYC do databáze Spark nyctaxi

K dispozici jsou data v tabulce v **SQLDB1**. Načtěte ho do databáze Spark s názvem **nyctaxi**.

1. V synapse studiu přejdete do centra pro **vývoj** .
1. Vyberte **+**  >  **Poznámkový blok**.
1. V horní části poznámkového bloku nastavte hodnotu **připojit k** **Spark1**.
1. Vyberte **přidat kód** pro přidání buňky kódu poznámkového bloku a vložte následující text:

    ```scala
    %%spark
    spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
    val df = spark.read.sqlanalytics("SQLDB1.dbo.Trip") 
    df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
    ```

1. Přejděte do centra **dat** , klikněte pravým tlačítkem na **databáze**a pak vyberte **aktualizovat**. Tyto databáze by se měly zobrazit:

    - **SQLDB1** (fond SQL)
    - **nyctaxi** (Spark)

## <a name="analyze-the-nyc-taxi-data-using-spark-and-notebooks"></a>Analýza dat taxislužby NYC pomocí Sparku a poznámkových bloků

1. Vraťte se do poznámkového bloku.
1. Vytvořte novou buňku kódu a zadejte následující text. Pak spuštěním buňky zobrazte data NYC taxislužby, která jsme načetli do databáze **nyctaxi** Spark.

   ```py
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```

1. Spusťte následující kód, který provede stejnou analýzu, jakou jsme provedli dříve s fondem SQL **SQLDB1**. Tento kód uloží výsledky analýzy do tabulky s názvem **nyctaxi. passengercountstats** a vizualizuje výsledky.

   ```py
   %%pyspark
   df = spark.sql("""
      SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE TripDistanceMiles > 0 AND PassengerCount > 0
      GROUP BY PassengerCount
      ORDER BY PassengerCount
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```

1. Ve výsledcích buňky vyberte možnost **graf** , aby se zobrazila data vizuálů.

## <a name="customize-data-visualization-with-spark-and-notebooks"></a>Přizpůsobení vizualizace dat pomocí Sparku a poznámkových bloků

Můžete řídit, jak se grafy vykreslují pomocí poznámkových bloků. Následující kód ukazuje jednoduchý příklad. Používá oblíbené knihovny **matplotlib** a **Seaborn**. Kód vykresluje stejný druh spojnicového grafu jako příkazy jazyka SQL, které byly dříve spuštěny.

```py
%%pyspark
import matplotlib.pyplot
import seaborn

seaborn.set(style = "whitegrid")
df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df = df.toPandas()
seaborn.lineplot(x="PassengerCount", y="SumTripDistance" , data = df)
seaborn.lineplot(x="PassengerCount", y="AvgTripDistance" , data = df)
matplotlib.pyplot.show()
```



## <a name="load-data-from-a-spark-table-into-a-sql-pool-table"></a>Načtení dat z tabulky Spark do tabulky fondu SQL

Dříve jsme zkopírovali data z tabulky fondu SQL **SQLDB1. dbo. Trip** do tabulky Spark **nyctaxi. Trip**. Pomocí Sparku jsme data agreguje do tabulky Spark **nyctaxi. passengercountstats**. Nyní zkopírujeme data z **nyctaxi. passengercountstats** do tabulky fondu SQL s názvem **SQLDB1. dbo. passengercountstats**.

Spusťte následující buňku v poznámkovém bloku. Nakopíruje agregovanou tabulku Spark zpátky do tabulky fondu SQL.

```scala
%%spark
val df = spark.sql("SELECT * FROM nyctaxi.passengercountstats")
df.write.sqlanalytics("SQLDB1.dbo.PassengerCountStats", Constants.INTERNAL )
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Analýza dat v úložišti](get-started-analyze-storage.md)


