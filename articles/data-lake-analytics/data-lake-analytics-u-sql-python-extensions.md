---
title: Rozšiřování skriptů U-SQL pomocí Pythonu v Azure Data Lake Analytics
description: Naučte se spouštět kód Pythonu ve skriptech U-SQL pomocí Azure Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/20/2017
ms.custom: tracking-python
ms.openlocfilehash: 89c974f6a2a6374370ff33940515b2a9f7904ce7
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129759"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Rozšiřování skriptů U-SQL pomocí kódu Pythonu v Azure Data Lake Analytics

## <a name="prerequisites"></a>Předpoklady

Než začnete, ujistěte se, že jsou ve vašem Azure Data Lake Analytics účtu nainstalované rozšíření Pythonu.

* Přejděte na Data Lake Analytics účtu v Azure Portal
* V nabídce vlevo v části **Začínáme** klikněte na **ukázkové skripty** .
* Klikněte na **nainstalovat rozšíření U-SQL** a pak **OK** .

## <a name="overview"></a>Přehled

Rozšíření Pythonu pro jazyk U-SQL umožňují vývojářům provádět výkonné paralelní spouštění kódu Pythonu. Následující příklad ilustruje základní kroky:

* Použití `REFERENCE ASSEMBLY` příkazu pro povolení rozšíření Pythonu pro skript U-SQL
* Použití `REDUCE` operace k rozdělení vstupních dat na klíč
* Rozšíření Pythonu pro jazyk U-SQL zahrnují vestavěný snižovalo ( `Extension.Python.Reducer` ), který spouští kód Pythonu na každém vrcholu přiřazeném k tomuto nástroji.
* Skript U-SQL obsahuje vložený kód Pythonu, který obsahuje funkci s názvem `usqlml_main` , která přijímá jako vstup PANDAS dataframe a jako výstup vrátí PANDAS dataframe.

```usql
REFERENCE ASSEMBLY [ExtPython];
DECLARE @myScript = @"
def get_mentions(tweet):
    return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )
def usqlml_main(df):
    del df['time']
    del df['author']
    df['mentions'] = df.tweet.apply(get_mentions)
    del df['tweet']
    return df
";
@t  =
    SELECT * FROM
       (VALUES
           ("D1","T1","A1","@foo Hello World @bar"),
           ("D2","T2","A2","@baz Hello World @beer")
       ) AS date, time, author, tweet );
@m  =
    REDUCE @t ON date
    PRODUCE date string, mentions string
    USING new Extension.Python.Reducer(pyScript:@myScript);
OUTPUT @m
    TO "/tweetmentions.csv"
    USING Outputters.Csv();
```

## <a name="how-python-integrates-with-u-sql"></a>Jak se Python integruje s U-SQL

### <a name="datatypes"></a>Datové typy

* Řetězcové a číselné sloupce z U-SQL se převádějí mezi PANDAS a U-SQL.
* Hodnoty null u-SQL jsou převáděny na a z `NA` hodnot PANDAS

### <a name="schemas"></a>Schémata

* Vektory indexu v PANDAS nejsou podporovány v U-SQL. Všechny vstupní datové rámce ve funkci Pythonu mají vždy 64 bitů číselného indexu od 0 do počtu řádků minus 1.
* Datové sady U-SQL nemůžou mít duplicitní názvy sloupců.
* Datové sady U-SQL jsou názvy sloupců, které nejsou řetězci.

### <a name="python-versions"></a>Verze Pythonu

Podporuje se jenom Python 3.5.1 (kompilované pro Windows).

### <a name="standard-python-modules"></a>Standardní moduly Pythonu

Jsou zahrnuty všechny standardní moduly Pythonu.

### <a name="additional-python-modules"></a>Další moduly Pythonu

Kromě standardních knihoven Pythonu jsou k dispozici několik běžně používaných knihoven Python:

* pandas
* numpy
* numexpr

### <a name="exception-messages"></a>Zprávy výjimek

V současné době se v kódu Pythonu zobrazuje výjimka jako obecná chyba vrcholu. V budoucnu chybové zprávy U-SQL úlohy zobrazí zprávu o výjimce Pythonu.

### <a name="input-and-output-size-limitations"></a>Omezení velikosti vstupu a výstupu

Ke každému vrcholu je přiřazena omezená velikost paměti. V současné době je tento limit 6 GB pro AU. Vzhledem k tomu, že vstupní a výstupní datový rámec musí existovat v paměti v kódu Pythonu, celková velikost vstupu a výstupu nemůže být větší než 6 GB.

## <a name="next-steps"></a>Další kroky

* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Použití funkcí okna U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-use-window-functions.md)
* [Použití nástrojů Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
