---
title: 'Azure HDInsight: ukázky .NET'
description: Příklady C# .NET na GitHubu pro běžné úlohy s využitím sady HDInsight SDK pro .NET.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: sample
ms.date: 12/06/2019
ms.openlocfilehash: 097323b4566daeda737428121bee4d64e9f7248b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "74951560"
---
# <a name="azure-hdinsight-net-samples"></a>Azure HDInsight: ukázky .NET

> [!div class="op_single_selector"]
> * [Příklady v .NET](hdinsight-sdk-dotnet-samples.md)
> * [Příklady v Pythonu](hdinsight-sdk-python-samples.md)
> * [Příklady v Javě](hdinsight-sdk-java-samples.md)
<!-- * [Go Examples](hdinsight-sdk-go-samples.md)-->

Tento článek obsahuje:

* Obsahuje odkazy na ukázky pro úlohy vytváření clusterů.
* Odkazy na referenční obsah pro jiné úlohy správy.

Můžete si [aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Za své předplatné sady Visual Studio každý měsíc získáváte kredity, které můžete použít k placení za služby Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisite"></a>Požadavek

[Sada Azure HDInsight SDK pro .NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>Správa clusteru – vytvoření

* [Vytvoření clusteru Kafka](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateKafkaClusterSample.cs)
* [Vytvoření clusteru Spark](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateSparkClusterSample.cs)
* [Vytvoření clusteru Spark pomocí Azure Data Lake Storage Gen2](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateHadoopClusterWithAdlsGen2Sample.cs)
* [Vytvoření clusteru Spark pomocí Balíček zabezpečení podniku (ESP)](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateEspClusterSample.cs)

Tyto ukázky pro .NET můžete získat klonováním úložiště GitHub pro [HDInsight-dotnet-SDK-Samples](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples) .

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

Fragmenty kódu pro tyto další funkce sady SDK najdete v [referenční dokumentaci k sadě HDInsight SDK pro .NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet).
