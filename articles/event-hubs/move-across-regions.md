---
title: Přesunutí oboru názvů Azure Event Hubs do jiné oblasti | Microsoft Docs
description: V tomto článku se dozvíte, jak přesunout obor názvů Azure Event Hubs z aktuální oblasti do jiné oblasti.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 51b02c34b0c28420a7e27da56b107ed3925a761b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537067"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Přesunutí oboru názvů Azure Event Hubs do jiné oblasti
Existují různé scénáře, ve kterých byste chtěli přesunout existující Event Hubs obor názvů z jedné oblasti do druhé. Například můžete chtít vytvořit obor názvů se stejnou konfigurací pro testování. V rámci [Plánování zotavení po havárii](event-hubs-geo-dr.md#setup-and-failover-flow)možná budete chtít vytvořit také sekundární obor názvů v jiné oblasti.

> [!NOTE]
> V tomto článku se dozvíte, jak exportovat šablonu Azure Resource Manager pro existující obor názvů Event Hubs a potom použít šablonu k vytvoření oboru názvů se stejným nastavením konfigurace v jiné oblasti. Tento proces však nepřesouvá události, které ještě nebyly zpracovány. Před odstraněním je třeba zpracovat události z původního oboru názvů.

## <a name="prerequisites"></a>Předpoklady

- Ujistěte se, že cílová oblast podporuje služby a funkce, které váš účet využívá.
- V případě funkcí Preview se ujistěte, že je vaše předplatné v cílové oblasti uvedené na seznamu povolených.
- Pokud jste povolili **funkci zachycení** pro centra událostí v oboru názvů, přesuňte účty [Azure Storage nebo Azure Data Lake Store gen 2](../storage/common/storage-account-move.md) nebo [Azure Data Lake Store 1.1](../data-lake-store/data-lake-store-migration-cross-region.md) . teprve potom přesuňte obor názvů Event Hubs. Můžete také přesunout skupinu prostředků, která obsahuje obory názvů úložiště i Event Hubs do jiné oblasti, a to pomocí následujících kroků, které jsou podobné těm, které jsou popsané v tomto článku. 
- Pokud je obor názvů Event Hubs v **clusteru Event Hubs**, vytvořte před provedením kroků v tomto článku [vyhrazený cluster](event-hubs-dedicated-cluster-create-portal.md) v **cílové oblasti** . Pomocí šablony pro rychlé zprovoznění [na GitHubu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) můžete také vytvořit cluster Event Hubs. V šabloně odeberte část s oborem názvů JSON, aby se vytvořil pouze cluster. 

## <a name="prepare"></a>Příprava
Začněte tím, že vyexportujete šablonu Správce prostředků. Tato šablona obsahuje nastavení, která popisují váš obor názvů Event Hubs.

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com).

2. Vyberte **všechny prostředky** a pak vyberte svůj obor názvů Event Hubs.

3. Vyberte > **Nastavení**  >  **Exportovat šablonu**.

4. Na stránce **Exportovat šablonu** vyberte **Stáhnout** .

    ![Stažení šablony Správce prostředků](./media/move-across-regions/download-template.png)

5. Vyhledejte soubor. zip, který jste stáhli z portálu, a rozbalte tento soubor do složky podle vašeho výběru.

   Tento soubor zip obsahuje soubory. JSON, které obsahují šablonu a skripty pro nasazení šablony.


## <a name="move"></a>Přesunout

Nasaďte šablonu pro vytvoření oboru názvů Event Hubs v cílové oblasti. 


1. V Azure Portal vyberte **vytvořit prostředek**.

2. V **části Hledat na Marketplace**zadejte **šablonu Deployment**a potom stiskněte **ENTER**.

3. Vyberte **template Deployment**.

4. Vyberte **Vytvořit**.

5. **V editoru vyberte vytvořit vlastní šablonu**.

6. Vyberte **načíst soubor**a potom podle pokynů načtěte **template.js** do souboru, který jste stáhli v poslední části.

7. Vyberte **Uložit** a šablonu uložte. 

8. Na stránce **vlastní nasazení** proveďte tyto kroky: 

    1. Vyberte **předplatné**Azure. 

    2. Vyberte existující **skupinu prostředků** nebo ji vytvořte. Pokud byl zdrojový obor názvů v clusteru Event Hubs, vyberte skupinu prostředků, která obsahuje cluster v cílové oblasti. 

    3. Vyberte cílové **umístění** nebo oblast. Pokud jste vybrali existující skupinu prostředků, toto nastavení je jen pro čtení. 

    4. V části **Nastavení** proveďte následující kroky:
    
        1. Zadejte nový **název oboru názvů**. 

            ![Nasazení šablony Správce prostředků](./media/move-across-regions/deploy-template.png)

        2. Pokud byl váš zdrojový obor názvů v **clusteru Event Hubs**, zadejte názvy **skupin prostředků** a **Event HUBS clusteru** jako součást **externího ID**. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Pokud centrum událostí v oboru názvů používá účet úložiště pro zachycení událostí, zadejte název skupiny prostředků a účet úložiště pro `StorageAccounts_<original storage account name>_external` pole. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Zaškrtněte políčko Souhlasím **s podmínkami a ujednáními uvedenými nahoře** . 
    
    6. Teď vyberte **Vybrat nákup** a zahajte proces nasazení. 

## <a name="discard-or-clean-up"></a>Zahození nebo vyčištění
Pokud po nasazení chcete začít znovu, můžete **cílový obor názvů Event Hubs**odstranit a postup opakovat postupem popsaným v části [Příprava](#prepare) a [Přesun](#move) v tomto článku.

Chcete-li potvrdit změny a dokončit přesun Event Hubs oboru názvů, odstraňte **zdrojový obor názvů Event Hubs**. Ujistěte se, že jste všechny události v oboru názvů před odstraněním oboru názvů zpracovali. 

Odstranění oboru názvů Event Hubs (zdroj nebo cíl) pomocí Azure Portal:

1. V okně hledání v horní části Azure Portal zadejte **Event Hubs**a vyberte **Event Hubs** z výsledků hledání. V seznamu se zobrazí Event Hubs obory názvů.

2. Vyberte cílový obor názvů, který chcete odstranit, a vyberte **Odstranit** z panelu nástrojů. 

    ![Odstranit obor názvů – tlačítko](./media/move-across-regions/delete-namespace-button.png)

3. Na stránce **Odstranit prostředky*** ověřte vybrané prostředky a potvrďte odstranění zadáním **Ano**a pak vyberte **Odstranit**. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste přesunuli obor názvů Azure Event Hubs z jedné oblasti na jiný a vyčistili zdrojové prostředky.  Další informace o přesouvání prostředků mezi oblastmi a zotavení po havárii v Azure najdete tady:


- [Přesun prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Přesun virtuálních počítačů Azure do jiné oblasti](../site-recovery/azure-to-azure-tutorial-migrate.md)
