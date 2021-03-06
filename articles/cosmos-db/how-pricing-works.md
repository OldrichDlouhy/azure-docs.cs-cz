---
title: Cenový model Azure Cosmos DB
description: V tomto článku se dozvíte o cenovém modelu Azure Cosmos DB a o tom, jak zjednodušuje správu nákladů a plánování nákladů.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: d36b4fd433af716ebd97d88d05922d94bd74c309
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523532"
---
# <a name="pricing-model-in-azure-cosmos-db"></a>Cenový model ve službě Azure Cosmos DB

Cenový model služby Azure Cosmos DB zjednodušuje správu a plánování nákladů. V případě služby Azure Cosmos DB platíte za zřízenou propustnost a spotřebované úložiště.

* **Zajištěná propustnost**: [zajištěná](how-to-choose-offer.md) propustnost (označovaná také jako rezervovaná propustnost) zaručuje vysoký výkon v jakémkoli měřítku. Určete propustnost (RU/s), kterou potřebujete, a Azure Cosmos DB vyhradit prostředky potřebné k zajištění nakonfigurované propustnosti. Za danou hodinu se účtuje po hodinách maximální zajištěné propustnosti. Můžete ručně zřídit propustnost nebo použít [Automatické škálování](provision-throughput-autoscale.md).

   > [!NOTE]
   > Vzhledem k tomu, že model zřízené propustnosti vyhradí prostředky pro váš kontejner nebo databázi, bude se vám za zřízenou propustnost účtovat i v případě, že nespustíte žádné úlohy.

* **Spotřebované úložiště**: za určitou hodinu se účtuje paušální sazba za celkové množství úložiště (GB) spotřebované za data a indexy.

Zřízená propustnost, zadaná jako [jednotky žádosti](request-units.md) za sekundu (ru/s), umožňuje čtení nebo zápis dat do kontejnerů nebo databází. Propustnost můžete [zřídit buď pro databázi, nebo pro kontejner](set-throughput.md). Na základě potřeb vašich úloh můžete kdykoli škálovat a snížit propustnost. Ceny Azure Cosmos DB jsou elastické a jsou úměrné propustnosti, kterou konfigurujete v databázi nebo kontejneru. Minimální propustnost a hodnoty úložiště a zvýšení měřítka poskytují celou škálu cen a spektra pružnosti pro všechny segmenty zákazníků, od malých až po velké kontejnery. Každá databáze nebo kontejner se fakturuje po hodinách na základě propustnosti zřízené v jednotkách 100 RU/s, minimálně 400 RU/s a úložiště spotřebovaného v GB. Na rozdíl od zřízené propustnosti se úložiště účtuje na základě spotřeby. To znamená, že nemusíte rezervovat žádné úložiště předem. Účtuje se vám jenom úložiště, které spotřebováváte.

Další informace najdete na stránce s [cenami za Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) a v článku o [Azure Cosmos DB fakturaci](understand-your-bill.md).

Cenový model v Azure Cosmos DB je konzistentní napříč všemi rozhraními API. Další informace najdete v tématu [jak Azure Cosmos DB cenového modelu pro zákazníky](total-cost-ownership.md)cenově výhodné. Pro databázi nebo kontejner je vyžadována minimální propustnost, která zajišťuje SLA a můžete zvýšit nebo snížit zřízenou propustnost pro každé 100 RU/s.

Pokud účet Azure Cosmos DB nasadíte do oblasti mimo státní správu v USA, je v současné době minimální cena pro obě databáze i propustnost na základě kontejneru přibližně 24 USD měsíčně. Ceny se liší v závislosti na oblasti, kterou používáte, na nejnovější informace o cenách najdete na [stránce s cenami Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) . Pokud vaše úloha používá více kontejnerů, je možné ji optimalizovat pro náklady pomocí propustnosti na úrovni databáze, protože propustnost na úrovni databáze umožňuje mít v databázi, která sdílí propustnost mezi kontejnery, libovolný počet kontejnerů. Následující tabulka shrnuje zřízenou propustnost a náklady na různé entity:

|**Entita**  | **Minimální propustnost** |**Přírůstky stupnice** |**Rozsah zřizování** |
|---------|---------|---------|-------|
|databáze    | 400 RU/s    | 100 RU/s   |Propustnost je vyhrazena pro databázi a je sdílena kontejnery v rámci databáze. |
|Kontejner     | 400 RU/s   | 100 RU/s  |Propustnost je vyhrazena pro konkrétní kontejner. |

Jak je znázorněno v předchozí tabulce, minimální propustnost v Azure Cosmos DB začíná v ceně přibližně za měsíc 24 měsíčně. Pokud začnete s minimální propustností a v průběhu času potřebujete podporovat provozní zatížení, vaše náklady budou plynule růst, a to v přírůstcích po dobu přibližně 6 USD měsíčně. Ceny se liší v závislosti na oblasti, kterou používáte, na nejnovější informace o cenách najdete na [stránce s cenami Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) . Cenový model v Azure Cosmos DB je elastický a při horizontálním navýšení nebo snížení kapacity dojde k hladkému zvýšení nebo snížení ceny.

## <a name="try-azure-cosmos-db-for-free"></a>Vyzkoušejte si Azure Cosmos DB zdarma

Azure Cosmos DB nabízí zdarma mnoho možností pro vývojáře. K těmto možnostem patří:

* **Azure Cosmos DB úroveň Free**: Azure Cosmos dB úrovně Free usnadňuje začátek, vývoj a testování aplikací nebo dokonce i spouštění malých produkčních úloh zdarma. Pokud je na účtu povolená úroveň Free, získáte po dobu životnosti účtu prvních 400 RU/s a 5 GB úložiště v účtu Free. Můžete mít až jeden účet bezplatné úrovně na jedno předplatné Azure a při vytváření účtu musí být výslovný souhlas. Pokud chcete začít, [vytvořte nový účet v Azure Portal s povolenou úrovní Free](create-cosmosdb-resources-portal.md) nebo použijte [šablonu ARM](manage-sql-with-resource-manager.md#free-tier).

* **Bezplatný účet Azure**: Azure nabízí [bezplatnou úroveň](https://azure.microsoft.com/free/) , která vám poskytne $200 kredity Azure během prvních 30 dnů a omezené množství bezplatných služeb na 12 měsíců. Další informace najdete na stránce [bezplatného účtu Azure](../cost-management-billing/manage/avoid-charges-free-account.md). Azure Cosmos DB je součástí bezplatného účtu Azure. Tento bezplatný účet Azure Cosmos DB konkrétně nabízí 5 GB úložiště a 400 RU/s zřízené propustnost po celý rok.

* **Vyzkoušejte si Azure Cosmos DB zdarma**: Azure Cosmos DB nabízí časově omezené prostředí pomocí funkce vyzkoušet Azure Cosmos DB pro bezplatné účty. Můžete vytvořit účet Azure Cosmos DB, vytvořit databázi a kolekce a spustit ukázkovou aplikaci pomocí rychlých startů a kurzů. Ukázkovou aplikaci můžete spustit bez přihlášení k odběru účtu Azure nebo pomocí platební karty. [Vyzkoušejte si Azure Cosmos DB zdarma](https://azure.microsoft.com/try/cosmosdb/) nabízených nabídek Azure Cosmos DB po dobu jednoho měsíce, s možností obnovit účet několikrát.

* **Emulátor Azure Cosmos DB**: Azure Cosmos DB emulátor poskytuje místní prostředí, které emuluje službu Azure Cosmos DB pro účely vývoje. Emulátor se nabízí zdarma a vysoká věrnost cloudové služby. Pomocí emulátoru Azure Cosmos DB můžete své aplikace vyvíjet a testovat v místním prostředí, aniž byste museli vytvářet předplatné Azure ani náklady. Své aplikace můžete vyvíjet pomocí místního emulátoru před tím, než budete pokračovat v produkci. Až budete spokojeni s funkcemi aplikace proti emulátoru, můžete přepnout na použití účtu Azure Cosmos DB v cloudu a významně ušetřit náklady. Další informace o emulátoru najdete v článku [použití Azure Cosmos DB pro vývoj a testování](local-emulator.md) .

## <a name="pricing-with-reserved-capacity"></a>Ceny s rezervovanou kapacitou

Azure Cosmos DB [Rezervovaná kapacita](cosmos-db-reserved-capacity.md) vám pomůže ušetřit peníze tím, že se předem platíte za prostředky Azure Cosmos DB po dobu jednoho roku nebo tří let. V porovnání s běžnými cenami můžete významně snížit náklady za jednoleté nebo tříleté závazky a ušetřit 20-65% slev. Azure Cosmos DB Rezervovaná kapacita pomáhá snížit náklady tím, že se předem platíte za zřízenou propustnost (RU/s) po dobu jednoho roku nebo tři roky a získáte slevu na zřízenou propustnost. 

Rezervovaná kapacita poskytuje slevu z faktury a neovlivňuje běhový stav prostředků Azure Cosmos DB. Rezervovaná kapacita je jednotně dostupná pro všechna rozhraní API, která zahrnují MongoDB, Cassandra, SQL, Gremlin a Azure a všechny oblasti po celém světě. Další informace o rezervované kapacitě najdete v tématu [platba za Azure Cosmos DB prostředky s využitím rezervované kapacity](cosmos-db-reserved-capacity.md) a zakoupení rezervované kapacity z [Azure Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Další kroky

Další informace o optimalizaci nákladů na prostředky Azure Cosmos DB najdete v následujících článcích:

* Informace o [optimalizaci pro vývoj a testování](optimize-dev-test.md)
* Další informace o [Azure Cosmos DB vyúčtování](understand-your-bill.md)
* Další informace o [optimalizaci nákladů na propustnost](optimize-cost-throughput.md)
* Další informace o [optimalizaci nákladů na úložiště](optimize-cost-storage.md)
* Další informace o [optimalizaci nákladů na čtení a zápisy](optimize-cost-reads-writes.md)
* Další informace o [optimalizaci nákladů na dotazy](optimize-cost-queries.md)
* Další informace o [optimalizaci nákladů na účty Cosmos s více oblastmi](optimize-cost-regions.md)
* Další informace o [Azure Cosmos DB rezervované kapacity](cosmos-db-reserved-capacity.md)
* Další informace o [emulátoru Azure Cosmos DB](local-emulator.md)
