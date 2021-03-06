---
title: Škálování COMPUTE pro synapse fond SQL (Azure Portal)
description: Výpočetní prostředky pro synapse fond SQL (datový sklad) můžete škálovat pomocí Azure Portal.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 04/28/2020
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: d944ff86cdc11c30142792c6fd15d03e3cfd4fa9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212934"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-the-azure-portal"></a>Rychlý Start: škálování COMPUTE pro synapse fond SQL pomocí Azure Portal

Výpočetní prostředky pro synapse fond SQL (datový sklad) můžete škálovat pomocí Azure Portal. Kapacitu výpočetních prostředků můžete [horizontálně navýšit](sql-data-warehouse-manage-compute-overview.md), abyste získali lepší výkon, nebo snížit, abyste dosáhli nižších nákladů. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný](https://azure.microsoft.com/free/) účet před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k [portálu Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Než začnete

Můžete škálovat fond SQL, který už máte, nebo můžete použít [rychlý Start: vytvoření a připojení – portál](create-data-warehouse-portal.md) pro vytvoření fondu SQL s názvem **mySampleDataWarehouse**. Tento rychlý start škáluje **mySampleDataWarehouse**.

>[!IMPORTANT] 
>Váš fond SQL musí být online, aby se mohlo škálovat. 

## <a name="scale-compute"></a>Škálování výpočetního výkonu

Výpočetní prostředky fondu SQL se dají škálovat zvýšením nebo snížením jednotek datového skladu. [Rychlý Start: vytvoření a připojení – portál](create-data-warehouse-portal.md) vytvořil **mySampleDataWarehouse** a inicializoval ho pomocí 400 DWU. V následujícím postupu upravíte jednotky datového skladu pro **mySampleDataWarehouse**.

Změna jednotek datového skladu:

1. Na levé straně Azure Portal klikněte na **Azure synapse Analytics (dřív SQL DW)** .
2. Vyberte **mySampleDataWarehouse** ze stránky **Azure synapse Analytics (dříve SQL DW)** . Otevře se fond SQL.
3. Klikněte na **Škálovat**.

    ![Kliknutí na Škálovat](./media/quickstart-scale-compute-portal/click-scale.png)

2. Na panelu škálování přesuňte posuvník doleva nebo doprava. Tím změníte nastavení jednotek datového skladu. Pak vyberte škálovat.

    ![Přesunutí posuvníku](./media/quickstart-scale-compute-portal/scale-dwu.png)

## <a name="next-steps"></a>Další kroky
Pokud se chcete dozvědět víc o fondu SQL, přejděte k kurzu [načíst data do fondu SQL](load-data-from-azure-blob-storage-using-polybase.md) . 
