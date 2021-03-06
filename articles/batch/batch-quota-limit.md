---
title: Kvóty a omezení služeb
description: Přečtěte si o výchozích Azure Batch kvótách, omezeních a omezeních a o zvýšení kvóty.
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 4c13df8b537d701400a22cd2871e7f8362f02455
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84417272"
---
# <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch

Stejně jako u jiných služeb Azure platí omezení pro určité prostředky přidružené ke službě Batch. Mnohé z těchto omezení jsou výchozími kvótami, které Azure používá na úrovni předplatného nebo účtu.

Při návrhu a navýšení kapacity úloh služby Batch mějte na paměti tyto kvóty. Například pokud váš fond nedosahuje cílového počtu výpočetních uzlů, které jste zadali, možná jste dosáhli limitu základní kvóty pro účet Batch.

Můžete spouštět více úloh služby Batch v jednom účtu Batch nebo distribuovat úlohy mezi účty Batch, které jsou ve stejném předplatném, ale v různých oblastech Azure.

Pokud máte v plánu provozovat produkční úlohy ve službě Batch, možná budete muset zvýšit jednu nebo více kvót nad výchozí hodnotou. Pokud chcete vygenerovat kvótu, můžete zdarma otevřít online [žádost o zákaznickou podporu](#increase-a-quota) .

## <a name="resource-quotas"></a>Kvóty prostředků

Kvóta je úvěrový limit, nikoli záruka na kapacitu. Pokud máte velké nároky na kapacitu, obraťte se prosím na podporu Azure.

Všimněte si také, že kvóty nejsou Garantované hodnoty. Kvóty se můžou lišit v závislosti na změnách ze služby Batch nebo na žádost uživatele o změnu hodnoty kvóty.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kvóty jader v režimu předplatného uživatele

Pokud jste vytvořili [účet Batch](accounts.md) s režimem přidělování fondů nastaveným na **předplatné uživatele**, kvóty se uplatní odlišně. V tomto režimu se virtuální počítače a další prostředky služby Batch vytvářejí přímo ve vašem předplatném při vytvoření fondu. Kvóty Azure Batchch jader se nevztahují na účet vytvořený v tomto režimu. Místo toho se použijí kvóty ve vašem předplatném pro regionální výpočetní jádra a další prostředky.

Další informace o těchto kvótách najdete v tématu [limity, kvóty a omezení předplatného a služeb Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Omezení velikosti fondu

Omezení velikosti fondu nastavuje služba Batch. Na rozdíl od [kvót prostředků](#resource-quotas)se tyto hodnoty nedají změnit. Omezení odlišná od standardní kvóty mají jenom fondy, které mají komunikaci mezi uzly a vlastní image.

| **Prostředek** | **Maximální limit** |
| --- | --- |
| **Výpočetní uzly ve [fondu s povolenými komunikacemi mezi uzly](batch-mpi.md)**  ||
| Režim přidělování fondů služby Batch | 100 |
| Režim přidělování fondů předplatných dávky | 80 |
| **Výpočetní uzly ve [fondu vytvořené pomocí prostředku spravované image](batch-custom-images.md)**<sup>1</sup> ||
| Vyhrazené uzly | 2000 |
| Uzly s nízkou prioritou | 1000 |

<sup>1</sup> pro fondy, které nejsou povolené komunikace mezi uzly.

## <a name="other-limits"></a>Další omezení

Další omezení nastavená službou Batch. Na rozdíl od [kvót prostředků](#resource-quotas)nelze tyto hodnoty změnit.

| **Prostředek** | **Maximální limit** |
| --- | --- |
| [Souběžné úkoly](batch-parallel-node-tasks.md) na výpočetní uzel | 4 x počet jader uzlů |
| Počet [aplikací](batch-application-packages.md) na účet Batch | 20 |
| Balíčky aplikací na aplikaci | 40 |
| Balíčky aplikací na fond | 10 |
| Maximální doba života úlohy | 180 dnů<sup>1</sup> |
| Počet [připojení](virtual-file-mount.md) na výpočetní uzel | 10 |

<sup>1</sup> maximální doba života úkolu, od kdy se přidá do úlohy po jeho dokončení, je 180 dní. Dokončené úlohy trvají po dobu sedmi dnů; data pro úlohy, které nejsou dokončená v rámci maximální doby života, nejsou dostupná.

## <a name="view-batch-quotas"></a>Zobrazit kvóty dávky

Zobrazení kvót účtu Batch v [Azure Portal](https://portal.azure.com):

1. Vyberte **účty Batch**a pak vyberte účet Batch, na který vás zajímáte.
1. V nabídce účtu Batch vyberte **kvóty** .
1. Zobrazit kvóty, které jsou aktuálně aplikovány na účet Batch.

    ![Kvóty účtu Batch][account_quotas]

## <a name="increase-a-quota"></a>Zvýšení kvóty

Můžete požádat o zvýšení kvóty pro účet Batch nebo předplatné pomocí [Azure Portal](https://portal.azure.com). Typ zvýšení kvóty závisí na režimu přidělování fondů účtu Batch. Chcete-li požádat o zvýšení kvóty, je nutné zahrnout řadu virtuálních počítačů, pro kterou chcete kvótu zvýšit. Při použití zvýšení kvóty se tato kvóta použije na všechny řady virtuálních počítačů.

1. Na řídicím panelu portálu vyberte dlaždici **Nápověda a podpora** nebo otazník (**?**) v pravém horním rohu portálu.
1. Vyberte **nové základy žádosti o podporu**  >  **Basics**.
1. **Základní informace**:
   
    1. **Typ problému**  >  **Omezení služeb a předplatného (kvóty)**
   
    1. Vyberte své předplatné.
   
    1. **Typ kvóty**  >  **Batch**
      
       Vyberte **Další**.
    
1. Oblast **Details** (Podrobnosti) obsahuje:
      
    1. V části **Zadejte podrobnosti**zadejte umístění, typ kvóty a účet Batch.
    
       ![Zvýšení kvóty dávky][quota_increase]

       Mezi typy kvót patří:

       * **Na účet Batch**  
         Hodnoty specifické pro jeden účet Batch, včetně základních a s nízkou prioritou a počtu úloh a fondů.
        
       * **Na oblast**  
         Hodnoty, které se vztahují na všechny účty Batch v oblasti a zahrnují počet účtů Batch na oblast a předplatné.

       Kvóta s nízkou prioritou je jednou hodnotou v rámci všech řad virtuálních počítačů. Pokud potřebujete omezené skladové jednotky, musíte vybrat **jádra s nízkou prioritou** a zahrnout do žádosti rodiny virtuálních počítačů.

    1. Vyberte **závažnost** podle svého [obchodního dopadu](https://aka.ms/supportseverity).

       Vyberte **Další**.

1. V **kontaktní údaje**:
   
    1. Vyberte **upřednostňovanou metodu kontaktu**.
   
    1. Ověřte a zadejte požadované kontaktní údaje.
   
       Vyberte **vytvořit** a odešlete žádost o podporu.

Po odeslání žádosti o podporu vás bude kontaktovat podpora Azure. Žádosti o kvótu se můžou dokončit během několika minut nebo do dvou pracovních dnů.

## <a name="related-quotas-for-vm-pools"></a>Související kvóty pro fondy virtuálních počítačů

Fondy služby Batch v konfiguraci virtuálního počítače nasazené ve virtuální síti Azure automaticky přidělují další síťové prostředky Azure. Pro každý uzel fondu 50 ve virtuální síti jsou potřeba tyto prostředky:

- Jedna [Skupina zabezpečení sítě](../virtual-network/security-overview.md#network-security-groups)
- Jedna [Veřejná IP adresa](../virtual-network/public-ip-addresses.md)
- Jeden [Nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md)

Tyto prostředky se přidělují v rámci předplatného, které obsahuje virtuální síť, která se zadala při vytváření fondu Batch. Pro tyto prostředky platí omezení [kvót prostředků](../azure-resource-manager/management/azure-subscription-service-limits.md) předplatného. Pokud plánujete nasazení rozsáhlých fondů ve virtuální síti, Prohlédněte si kvóty předplatného pro tyto prostředky. V případě potřeby požádejte o zvýšení Azure Portal tím, že vyberete **help + podpora**.

## <a name="next-steps"></a>Další kroky

* [Vytvořte účet Azure Batch pomocí Azure Portal](batch-account-create-portal.md).
* Přečtěte si o [pracovních postupech služby Batch a primárních prostředcích](batch-service-workflow-features.md) , jako jsou fondy, uzly, úlohy a úkoly.
* Přečtěte si o [omezeních, kvótách a omezeních předplatného a služeb Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
