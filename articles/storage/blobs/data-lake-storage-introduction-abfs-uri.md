---
title: Azure Data Lake Storage Gen2 URI
description: Azure Data Lake Storage Gen2 URI
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: fa0f67e0d72ee5710a42b6de744ddae98e20220a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "80437132"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Azure Data Lake Storage Gen2 URI

Ovladač [systému Hadoop](https://www.aosabook.org/en/hdfs.html) , který je kompatibilní s Azure Data Lake Storage Gen2, je známý identifikátorem schématu `abfs` (systém souborů Azure BLOB). Ovladače ABFS v souladu s jinými ovladači systému souborů Hadoop využívají formát identifikátoru URI k adresování souborů a adresářů v rámci účtu s možností Data Lake Storage Gen2.

## <a name="uri-syntax"></a>Syntaxe identifikátoru URI

Syntaxe identifikátoru URI pro Data Lake Storage Gen2 závisí na tom, jestli je váš účet úložiště nastavený tak, aby měl Data Lake Storage Gen2 jako výchozí systém souborů.

Pokud účet s možností Data Lake Storage Gen2, který chcete adresovat, **není** nastaven jako výchozí systém souborů během vytváření účtu, je syntaxe zkrácený identifikátor URI:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Identifikátor schématu**: `abfs` protokol se používá jako identifikátor schématu. Máte možnost se připojit pomocí protokolu TLS (Transport Layer Security), dříve označovaného jako SSL (Secure Sockets Layer) (SSL), připojení. Použijte `abfss` pro připojení s připojením TLS.

2. **Systém souborů**: nadřazené umístění, které obsahuje soubory a složky. To je stejné jako u kontejnerů ve službě Azure Storage BLOBs.

3. **Název účtu**: název přidělený vašemu účtu úložiště během vytváření.

4. **Cesty**: reprezentace adresářové struktury s lomítkem ( `/` ).

5. **Název souboru**: název jednotlivého souboru. Tento parametr je nepovinný, pokud adresování adresáře.

Pokud je však účet, který chcete adresovat, nastaven jako výchozí systém souborů během vytváření účtu, zkrácený identifikátor URI syntaxe je:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Cesta**: reprezentace adresářové struktury s lomítkem ( `/` ).

2. **Název souboru**: název jednotlivého souboru.


## <a name="next-steps"></a>Další kroky

- [Použití služby Azure Data Lake Storage Gen2 s clustery Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
