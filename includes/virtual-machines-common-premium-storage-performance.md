---
title: zahrnout soubor
description: zahrnout soubor
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 60053f24aa4231f1100d0b00cb6cf70b851b1939
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526026"
---
## <a name="application-performance-indicators"></a>Indikátory výkonu aplikace

Vyhodnocujeme, jestli aplikace funguje dobře nebo nepoužívá indikátory výkonu, jako je třeba, jak rychle aplikace zpracovává požadavky uživatelů, kolik dat aplikace zpracovává na žádost, kolik požadavků je zpracování aplikace v konkrétní době, jak dlouho musí uživatel po odeslání žádosti počkat na odpověď. Technické údaje o těchto ukazatelích výkonu jsou, IOPS, propustnost nebo šířka pásma a latence.

V této části se podíváme na běžné ukazatele výkonu v souvislosti s Premium Storage. V následující části se seznámíte s požadavky na aplikace, se naučíte změřit tyto ukazatele výkonu pro vaši aplikaci. Později v rámci optimalizace výkonu aplikace se dozvíte o faktorech, které mají vliv na tyto ukazatele výkonu a doporučení pro jejich optimalizaci.

## <a name="iops"></a>IOPS

Počet vstupně-výstupních operací za sekundu za sekundu je počet požadavků, které aplikace posílá na disky úložiště za sekundu. Vstupně-výstupní operace může být přečtena nebo zapsána, sekvenční nebo náhodná. Aplikace OLTP (online Transaction Processing), jako je online prodejní web, potřebují okamžitě zpracovat mnoho souběžných uživatelských požadavků. Požadavky uživatelů jsou vkládání a aktualizace náročných databázových transakcí, které musí aplikace zpracovat rychle. Proto aplikace OLTP vyžadují velmi vysoký IOPS. Tyto aplikace zpracovávají miliony malých a náhodných vstupně-výstupních požadavků. Máte-li takovou aplikaci, je nutné navrhnout infrastrukturu aplikace, která bude optimalizována pro IOPS. V pozdější části, které *optimalizují výkon aplikace*, se podrobněji podíváme na podrobné informace o všech faktorech, které je potřeba vzít v úvahu, abyste získali vysoké IOPS.

Když ke svému virtuálnímu počítači s vysokou kapacitou připojíte disk úložiště úrovně Premium, Azure pro vás zřídí zaručenou hodnotu IOPS podle specifikace disku. Například disk P50 zřídí 7 500 IOPS. Každý virtuální počítač s vysokou kapacitou má také konkrétní limit IOPS, který dokáže plnit. Například standardní virtuální počítač GS5 má limit 80 000 IOPS.

## <a name="throughput"></a>Propustnost

Propustnost nebo šířka pásma je množství dat, které aplikace odesílá na disky úložiště v zadaném intervalu. Pokud vaše aplikace provádí vstupní a výstupní operace s velkými velikostmi jednotek v/v, vyžaduje vysokou propustnost. Aplikace datového skladu mají za následek vystavování operací náročných na kontrolu, které přistupují k velkým objemům dat v čase, a běžně provádějí hromadné operace. Jinými slovy, takové aplikace vyžadují vyšší propustnost. Máte-li takovou aplikaci, je nutné navrhnout infrastrukturu pro optimalizaci propustnosti. V další části se podrobněji podíváme na faktory, které musíte ladit, abyste to dosáhli.

Když k virtuálnímu počítači s vysokým rozsahem připojíte disk služby Premium Storage, Azure zřídí propustnost podle konkrétního disku. Například disk P50 zřídí propustnost 250 MB za sekundu. Každý virtuální počítač s vysokou kapacitou má také konkrétní limit propustnosti, který dokáže plnit. Například virtuální počítač Standard GS5 má maximální propustnost 2000 MB za sekundu.

Existuje vztah mezi propustností a IOPS, jak je znázorněno ve vzorci níže.

![Vztah IOPS a propustnosti](../articles/virtual-machines/linux/media/premium-storage-performance/image1.png)

Proto je důležité určit optimální propustnost a hodnoty IOPS, které vaše aplikace vyžaduje. Při pokusu o optimalizaci se druhá taky ovlivní. V pozdější části, která *optimalizuje výkon aplikace*, probereme další podrobnosti o optimalizaci IOPS a propustnosti.

## <a name="latency"></a>Latence

Latence je doba, kterou aplikace potřebuje k přijetí jediné žádosti, odeslání na disky úložiště a odeslání odpovědi klientovi. Toto je kritická míra výkonu aplikace kromě vstupně-výstupních operací a propustnosti. Latence disku služby Premium Storage je čas potřebný k načtení informací o požadavku a jeho předání zpět do vaší aplikace. Premium Storage poskytuje konzistentní nízkou latenci. Disky Premium jsou navržené tak, aby pro většinu vstupně-výstupních operací poskytovaly jednorázové latence milisekund. Pokud povolíte ukládání do mezipaměti hostitele jen pro čtení na disky Premium Storage, můžete získat mnohem nižší latenci čtení. V další části o *optimalizaci výkonu aplikace*probereme další podrobnosti o ukládání disku do mezipaměti.

Při optimalizaci aplikace za účelem získání vyššího počtu vstupně-výstupních operací a propustnosti bude ovlivněna latence vaší aplikace. Po optimalizaci výkonu aplikace vždy vyhodnoťte latenci aplikace, aby nedošlo k neočekávanému chování vysoké latence.

Následující operace správy roviny na Managed Disks můžou zahrnovat přesun disku z jednoho umístění úložiště do druhého. Tato aplikace se orchestruje prostřednictvím kopie dat na pozadí, která může trvat několik hodin, obvykle v závislosti na množství dat na discích. Během této doby může aplikace docházet k vyšší latenci při čtení, protože některé čtení se může přesměrovat na původní umístění a dokončení může trvat déle. Během této doby není nijak ovlivněna latence zápisu.

- Aktualizujte typ úložiště.
- Odpojení a připojení disku z jednoho virtuálního počítače k druhému.
- Vytvoření spravovaného disku z virtuálního pevného disku.
- Vytvoření spravovaného disku ze snímku.
- Převod nespravovaných disků na Managed disks

## <a name="performance-application-checklist-for-disks"></a>Kontrolní seznam pro aplikace výkonu pro disky

Prvním krokem při navrhování vysoce výkonných aplikací běžících na Azure Premium Storage je porozumění požadavkům na výkon vaší aplikace. Po shromáždění požadavků na výkon můžete optimalizovat aplikaci, abyste dosáhli optimálního výkonu.

V předchozí části jsme vysvětlili běžné ukazatele výkonu, IOPS, propustnost a latence. Musíte určit, které z těchto ukazatelů výkonu jsou pro vaši aplikaci důležité, aby poskytovaly požadované uživatelské prostředí. Například vysoce náročné otázky IOPS na OLTP aplikace zpracovávající miliony transakcí za sekundu. Vzhledem k tomu, že vysoká propustnost je zásadní pro aplikace datového skladu, které zpracovávají velké objemy dat za sekundu. Extrémně nízká latence je zásadní pro aplikace v reálném čase, jako jsou websites streamování videí.

Dále změřte maximální požadavky na výkon vaší aplikace během své životnosti. Jako začátek použijte ukázkový kontrolní seznam. Zaznamenejte maximální požadavky na výkon během období normálního, špičky a doby běhu. Tím, že identifikujete požadavky na úrovně všech úloh, budete moci určit celkový požadavek na výkon vaší aplikace. Například běžným zatížením webu elektronického obchodování budou transakce, které obsluha zahrnuje během většiny dnů v roce. Úlohou ve špičce webu budou transakce, které slouží během svátečních období nebo zvláštních prodejních událostí. Úloha špičky se obvykle používá pro omezené období, ale může vyžadovat, aby vaše aplikace mohla škálovat dvě nebo více času na jejich normální fungování. Přečtěte si 50. percentilu, 90 percentil a 99 percentily. To pomáhá vyfiltrovat všechny odlehlé podmínky v požadavcích na výkon a můžete se zaměřit na optimalizaci pro správné hodnoty.

## <a name="application-performance-requirements-checklist"></a>Kontrolní seznam požadavků na výkon aplikace

| **Požadavky na výkon** | **50. percentil** | **90. percentil** | **99. percentil** |
| --- | --- | --- | --- |
| Max. Transakcí za sekundu | | | |
| % Operací čtení | | | |
| % Operací zápisu | | | |
| % Náhodných operací | | | |
| % Sekvenčních operací | | | |
| Velikost žádosti v/v | | | |
| Průměrná propustnost | | | |
| Max. Propustnost | | | |
| Dlouhé. Latence | | | |
| Průměrná latence | | | |
| Max. Procesor | | | |
| Průměrný procesor | | | |
| Max. Paměť | | | |
| Průměrná paměť | | | |
| Hloubka fronty | | | |

> [!NOTE]
> Měli byste zvážit škálování těchto čísel na základě očekávaného budoucího růstu vaší aplikace. Je vhodné naplánovat nárůst před časem, protože by mohlo být obtížnější změnit infrastrukturu na zvýšení výkonu později.

Pokud máte existující aplikaci a chcete přejít na Premium Storage, nejprve Sestavte kontrolní seznam pro existující aplikaci. Pak Sestavte prototyp své aplikace na Premium Storage a návrh aplikace podle pokynů popsaných v tématu *optimalizace výkonu aplikace* v pozdější části tohoto dokumentu. Další článek popisuje nástroje, které můžete použít ke shromáždění měření výkonu.

### <a name="counters-to-measure-application-performance-requirements"></a>Čítače pro měření požadavků na výkon aplikace

Nejlepším způsobem, jak změřit požadavky na výkon vaší aplikace, je používat nástroje pro monitorování výkonu poskytované operačním systémem serveru. Můžete použít PerfMon pro Windows a iostat pro Linux. Tyto nástroje zachycují čítače odpovídající každé míře vysvětlené v předchozí části. Hodnoty těchto čítačů je nutné zachytit, pokud vaše aplikace běží normálně, ve špičce nebo mimo pracovní vytížení.

Čítače výkonu jsou k dispozici pro procesor, paměť a každý logický disk a fyzický disk serveru. Když použijete disky Premium Storage s virtuálním počítačem, čítače fyzického disku jsou pro každý disk Storage úrovně Premium a čítače logických disků jsou pro každý svazek vytvořený na discích úložiště úrovně Premium. Je nutné zachytit hodnoty pro disky, které hostují zatížení vaší aplikace. Pokud existuje mapování mezi logickými a fyzickými disky, můžete se podívat na čítače fyzického disku. v opačném případě se podívejte na čítače logických disků. V systému Linux příkaz iostat vygeneruje sestavu využití procesoru a disku. Sestava využití disku poskytuje statistiku pro každé fyzické zařízení nebo oddíl. Pokud máte databázový server s daty a protokoly na samostatných discích, shromážděte tato data pro oba disky. Následující tabulka popisuje čítače pro disky, procesory a paměť:

| Čítač | Popis | PerfMon | Iostat |
| --- | --- | --- | --- |
| **Počet vstupně-výstupních operací za sekundu** |Počet vstupně-výstupních požadavků vydaných na disk úložiště za sekundu. |Čtení z disku/s <br> Zápisy na disk/s |TPS <br> r/s <br> w/s |
| **Čtení a zápisy na disk** |% operací čtení a zápisu provedených na disku. |% Doby čtení disku <br> % Času zápisu na disk |r/s <br> w/s |
| **Propustnost** |Množství dat čtených nebo zapsaných na disk za sekundu. |Bajty čtení z disku/s <br> Bajty zápisu na disk/s |kB_read/s <br> kB_wrtn/s |
| **Latence** |Celková doba, po kterou se má dokončit požadavek na vstupně-výstupní operace disku |Střední doba disku/čtení <br> Střední doba disku/zápis |await <br> svctm |
| **Velikost v/v** |Velikost vstupně-výstupních požadavků vydává problémy diskům úložiště. |Průměrný počet bajtů disku/čtení <br> Průměrný počet bajtů disku/zápis |avgrq – SZ |
| **Hloubka fronty** |Počet nezpracovaných vstupně-výstupních požadavků, které čekají na čtení nebo zapisování na disk úložiště. |Aktuální délka fronty disku |avgqu – SZ |
| **Počet. Rezident** |Množství paměti vyžadované pro plynulé spuštění aplikace |% Používaných potvrzených bajtů |Použití vmstat |
| **Počet. VČETNĚ** |Množství CPU vyžadované pro plynulé spuštění aplikace |% Času procesoru |% util |

Přečtěte si další informace o [iostat](https://linux.die.net/man/1/iostat) a [perfmon](https://msdn.microsoft.com/library/aa645516.aspx).



## <a name="optimize-application-performance"></a>Optimalizace výkonu aplikace

Hlavní faktory ovlivňující výkon aplikace běžící na Premium Storage jsou povahou vstupně-výstupních požadavků, velikosti virtuálních počítačů, velikosti disků, počtu disků, ukládání do mezipaměti disku, multithreading a hloubka fronty. Některé z těchto faktorů můžete řídit pomocí knoflíků poskytovaných systémem. Většina aplikací vám nemusí dát možnost změnit přímo velikost vstupně-výstupních operací a hloubku fronty. Pokud například používáte SQL Server, nemůžete zvolit velikost vstupně-výstupních operací a hloubku fronty. Pro dosažení maximálního výkonu SQL Server zvolí optimální velikost vstupně-výstupních operací a hodnoty hloubky fronty. Je důležité pochopit účinky obou typů faktorů na výkon aplikace, abyste mohli zřídit vhodné prostředky pro splnění požadavků na výkon.

V této části najdete kontrolní seznam požadavků aplikace, který jste vytvořili, abyste identifikovali, kolik potřebujete k optimalizaci výkonu aplikace. V závislosti na tom budete moct určit, které faktory z této části budete muset ladit. Chcete-li nastavit důsledky pro vliv každého faktoru na výkon aplikace, spusťte nástroje srovnávacích testů v nastavení aplikace. Postup pro spuštění běžných nástrojů pro srovnávací testy na virtuálních počítačích s Windows a Linux najdete v článku o srovnávacích testech propojených na konci.

### <a name="optimize-iops-throughput-and-latency-at-a-glance"></a>Rychlé optimalizace vstupně-výstupních operací, propustnosti a latence

Následující tabulka shrnuje faktory výkonu a kroky potřebné k optimalizaci IOPS, propustnosti a latence. Oddíly uvedené v tomto souhrnu popisují, že každý faktor bude mnohem větší hloubka.

Další informace o velikostech virtuálních počítačů a počtu vstupně-výstupních operací, propustnosti a latence dostupných pro každý typ virtuálního počítače najdete v tématu [velikosti virtuálních počítačů Linux](../articles/virtual-machines/linux/sizes.md) nebo [velikosti virtuálních počítačů s Windows](../articles/virtual-machines/windows/sizes.md).

| | **IOPS** | **Propustnost** | **Latence** |
| --- | --- | --- | --- |
| **Ukázkový scénář** |Podniková aplikace OLTP vyžadující velmi vysoký počet transakcí za sekundu. |Aplikace pro podnikové datové sklady zpracovávající velké objemy dat. |Aplikace prakticky v reálném čase, které vyžadují okamžitou odezvu na požadavky uživatelů, jako jsou online hry. |
| **Faktory výkonu** | &nbsp; | &nbsp; | &nbsp; |
| **Velikost v/v** |Menší velikost vstupně-výstupních operací má vyšší IOPS. |Větší množství vstupně-výstupních operací, které poskytuje vyšší propustnost. | &nbsp;|
| **Velikost virtuálního počítače** |Použijte velikost virtuálního počítače, která nabízí IOPS větší než požadavek vaší aplikace. |Použijte velikost virtuálního počítače s limitem propustnosti větším, než je požadavek vaší aplikace. |Použijte velikost virtuálního počítače, která nabízí limity škálování větší než požadavek vaší aplikace. |
| **Velikost disku** |Použijte velikost disku, která nabízí IOPS větší než požadavek vaší aplikace. |Použijte velikost disku s limitem propustnosti větším, než je požadavek vaší aplikace. |Použijte velikost disku, která nabízí limity škálování větší než požadavek vaší aplikace. |
| **Omezení škálování virtuálního počítače a disku** |Limit IOPS zvolené velikosti virtuálního počítače by měl být větší než celkový počet IOPS, který je založený na discích úložiště, které jsou k němu připojené. |Limit propustnosti zvolené velikosti virtuálního počítače by měl být větší než celková propustnost řízená disky Premium Storage, které jsou k ní připojené. |Limity škálování zvolené velikosti virtuálního počítače musí být větší než celková omezení škálování připojených disků úložiště úrovně Premium. |
| **Ukládání disku do mezipaměti** |Povolte mezipaměť jen pro čtení na discích úložiště úrovně Premium s operacemi čtení těžkých operací, abyste získali vyšší počet IOPS. | &nbsp; |Povolte mezipaměť jen pro čtení na discích úložiště úrovně Premium s připravenými náročnými operacemi, abyste dosáhli velmi nízké latence čtení. |
| **Diskové svazky** |Použijte více disků a propojte je dohromady, abyste získali kombinovaný limit s vyšším počtem vstupně-výstupních operací a propustnosti. Kombinovaný limit na virtuální počítač musí být vyšší než kombinované limity připojených prémiových disků. | &nbsp; | &nbsp; |
| **Velikost pruhu** |Menší velikost obložení pro náhodný vzor malých vstupně-výstupních operací, který se zobrazuje v aplikacích OLTP. Například použijte velikost Stripe 64 KB pro aplikaci SQL Server OLTP. |Větší velikost proložení pro sekvenční velký vstupně-výstupní vzor, který se zobrazuje v aplikacích datového skladu. Například použijte velikost pruhu 256 KB pro SQL Server aplikaci datového skladu. | &nbsp; |
| **Multithreading** |Použijte multithreading k nabízení většího počtu požadavků na Premium Storage, které vedou k vyšším vstupně-výstupním operacím a propustnosti. Například na SQL Server nastavte vysokou hodnotu MAXDOP pro přidělení dalších procesorů k SQL Server. | &nbsp; | &nbsp; |
| **Hloubka fronty** |Větší hloubka fronty poskytuje vyšší IOPS. |Větší hloubka fronty poskytuje vyšší propustnost. |Menší hloubka fronty poskytuje nižší latenci. |

## <a name="nature-of-io-requests"></a>Povaha vstupně-výstupních požadavků

Vstupně-výstupní operace je jednotka vstupně-výstupních operací, kterou bude vaše aplikace provádět. Určení povahy požadavků na vstupně-výstupní operace, náhodných nebo sekvenčních, čtení nebo zápisu, malých nebo velkých, vám pomůže určit požadavky na výkon vaší aplikace. Je důležité pochopit charakter požadavků v/v, aby se při návrhu infrastruktury aplikace zajistila správná rozhodnutí. IOs se musí rovnoměrně distribuovat, aby se dosáhlo co nejlepšího výkonu.

Velikost v/v je jedním z důležitějších faktorů. Velikost vstupně-výstupních operací je velikost žádosti o vstupně-výstupní operace vygenerované vaší aplikací. Velikost vstupně-výstupních operací má významný dopad na výkon hlavně na základě IOPS a šířky pásma, které aplikace dokáže dosáhnout. Následující vzorec znázorňuje vztah mezi vstupně-výstupními operacemi, velikostí vstupně-výstupních operací a šířkou pásma a propustností  
    ![Diagram znázorňující rovnici I O-t, kolikrát se mění propustnost.](media/premium-storage-performance/image1.png)

Některé aplikace umožňují změnit jejich vstupně-výstupní operace, zatímco některé aplikace ne. SQL Server například určuje optimální velikost vstupně-výstupních operací a neposkytuje uživatelům žádné ovladače ke změně. Na druhé straně Oracle poskytuje parametr s názvem [ \_ \_ velikost bloku DB](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) , pomocí kterého můžete nakonfigurovat velikost vstupně-výstupních požadavků databáze.

Pokud používáte aplikaci, která vám neumožňuje změnit velikost vstupně-výstupních operací, použijte pokyny v tomto článku k optimalizaci klíčového ukazatele výkonu, který je pro vaši aplikaci nejrelevantnější. Příklad:

* Aplikace OLTP generuje miliony malých a náhodných vstupně-výstupních požadavků. Chcete-li tyto typy požadavků na vstupně-výstupní operace zpracovat, je nutné navrhnout infrastrukturu aplikace a získat vyšší IOPS.  
* Aplikace datového skladu generuje velké a sekvenční vstupně-výstupní požadavky. Chcete-li tyto typy požadavků na vstupně-výstupní operace zpracovat, je nutné navrhnout infrastrukturu vaší aplikace, abyste dosáhli vyšší šířky pásma nebo propustnosti.

Pokud používáte aplikaci, která umožňuje změnit velikost vstupně-výstupních operací, použijte toto pravidlo pro vstupně-výstupní operace kromě dalších pokynů pro výkon.

* Menší velikost vstupně-výstupních operací k získání vyššího počtu IOPS. Například 8 KB pro aplikaci OLTP.  
* Větší vstupně-výstupní velikost pro dosažení vyšší šířky pásma a propustnosti. Například 1024 KB pro aplikaci datového skladu.

Tady je příklad, jak můžete vypočítat vstupně-výstupní operace a propustnost/šířku pásma pro vaši aplikaci. Zvažte použití disku P30. Maximální IOPS a propustnost/šířka pásma, které může P30 disk dosáhnout, jsou 5000 vstupně-výstupních operací za sekundu a 200 MB v uvedeném pořadí. Pokud teď vaše aplikace vyžaduje maximální IOPS z disku P30 a používáte menší velikost vstupně-výstupních operací, jako je 8 KB, výsledná šířka pásma bude možné získat 40 MB za sekundu. Pokud ale vaše aplikace vyžaduje maximální propustnost a šířku pásma z disku P30 a použijete větší vstupně-výstupní velikost jako 1024 KB, výsledný IOPS bude menší, 200 IOPS. Proto vylaďte velikost vstupně-výstupních operací tak, aby splňovala požadavky na vstupně-výstupní operace vaší aplikace a propustnost/šířka pásma. Následující tabulka shrnuje různé velikosti v/v a jejich odpovídající IOPS a propustnost pro P30 disk.

| Požadavek na aplikaci | I/O velikost | IOPS | Propustnost a šířka pásma |
| --- | --- | --- | --- |
| Maximální počet vstupně-výstupních operací za sekundu |8 kB |5 000 |40 MB za sekundu |
| Maximální propustnost |1024 KB |200 |200 MB za sekundu |
| Maximální propustnost + horní IOPS |64 kB |3 200 |200 MB za sekundu |
| Max. IOPS + vysoká propustnost |32 KB |5 000 |160 MB za sekundu |

Pokud chcete získat vstupně-výstupní operace a šířku pásma větší než maximální hodnota jediného úložného disku Premium, použijte několik prémiových disků. Proložením dvou P30 disků můžete například získat kombinované IOPS 10 000 IOPS nebo kombinovanou propustnost 400 MB za sekundu. Jak je vysvětleno v další části, je nutné použít velikost virtuálního počítače, která podporuje počet vstupně-výstupních operací a propustnosti disku v kombinaci.

> [!NOTE]
> Při navýšení počtu IOPS nebo propustnosti se druhá taky zvýší a ujistěte se, že při zvyšování počtu vstupně-výstupních operací disku nebo virtuálního počítače nejste dosáhli limitu propustnosti nebo IOPS.

Chcete-li nastavit vliv velikosti vstupně-výstupních operací na výkon aplikace, můžete spustit nástroje pro srovnávací testy na virtuálních počítačích a discích. Vytvoření více testovacích běhů a použití jiné velikosti v/v pro každý běh pro zobrazení dopadu. Další podrobnosti najdete v článku o srovnávacích testech propojených na konci.

## <a name="high-scale-vm-sizes"></a>Velikosti virtuálních počítačů s vysokou škálou

Když začnete navrhovat aplikaci, jednou z nich, kterou je třeba udělat, je, že chcete hostovat svoji aplikaci, vyberte virtuální počítač. Premium Storage přináší vysoce škálovatelné velikosti virtuálních počítačů, které můžou spouštět aplikace vyžadující vyšší výpočetní výkon a vysoký výkon vstupně-výstupních operací na místním disku. Tyto virtuální počítače poskytují rychlejší procesory, vyšší poměr paměti k jádrům a jednotku SSD (Solid-State Drive) pro místní disk. Příklady virtuálních počítačů s vysokým škálováním, které podporuje Premium Storage, jsou virtuální počítače řady DS a GS.

Virtuální počítače s vysokým rozsahem jsou k dispozici v různých velikostech s různými počty PROCESORových jader, paměti, operačním systémem a dočasné velikosti disku. Každá velikost virtuálního počítače má také maximální počet datových disků, které můžete připojit k virtuálnímu počítači. Vybraná velikost virtuálního počítače proto bude mít vliv na to, kolik je pro vaši aplikaci k dispozici zpracování, paměť a kapacita úložiště. Ovlivňuje také náklady na výpočetní prostředky a úložiště. Níže jsou uvedené například specifikace největšího počtu virtuálních počítačů v řadě DS a řady GS:

| Velikost virtuálního počítače | Procesorová jádra | Paměť | Velikosti disků virtuálních počítačů | Max. datové disky | Velikost mezipaměti | IOPS | Omezení v/v mezipaměti šířky pásma |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |OS = 1023 GB <br> Místní SSD = 224 GB |32 |576 GB |50 000 IOPS <br> 512 MB za sekundu |4 000 IOPS a 33 MB za sekundu |
| Standard_GS5 |32 |448 GB |OS = 1023 GB <br> Místní SSD = 896 GB |64 |4224 GB |80 000 IOPS <br> 2 000 MB za sekundu |5 000 IOPS a 50 MB za sekundu |

Úplný seznam dostupných velikostí virtuálních počítačů Azure najdete v tématu velikosti virtuálních počítačů s [Windows](../articles/virtual-machines/windows/sizes.md) nebo [velikosti virtuálních počítačů](../articles/virtual-machines/linux/sizes.md)se systémem Linux. Vyberte velikost virtuálního počítače, která může splňovat požadavky na výkon požadované aplikace a škálovat je. Kromě toho vezměte v úvahu následující důležité informace při volbě velikostí virtuálních počítačů.

*Omezení škálování*  
Maximální počet IOPS na virtuální počítač a na disk se liší a nezávisle na sobě navzájem. Ujistěte se, že aplikace řídí IOPS v rámci limitů virtuálního počítače a taky připojených prémiových disků. V opačném případě výkon aplikace zaznamená omezení.

Předpokládejme například, že požadavek na aplikaci je maximálně 4 000 IOPS. K tomu je potřeba zřídit P30 disk na virtuálním počítači s DS1. Disk P30 může doručovat až 5 000 IOPS. Virtuální počítač DS1 je ale omezený na 3 200 IOPS. V důsledku toho bude výkon aplikace omezen omezením počtu virtuálních počítačů na 3 200 IOPS a dojde ke snížení výkonu. Pokud chcete této situaci zabránit, vyberte virtuální počítač a velikost disku, které budou vyhovovat požadavkům aplikace.

*Náklady na operaci*  
V mnoha případech je možné, že celkové náklady na provoz pomocí Premium Storage jsou nižší než používání služby Storage úrovně Standard.

Představte si třeba aplikaci vyžadující 16 000 IOPS. K dosažení tohoto výkonu budete potřebovat standardní \_ virtuální počítač Azure IaaS s D14, který může poskytnout maximální IOPS 16 000 s použitím disků 32 úrovně Standard úložiště 1 TB. Každý standardní disk úložiště o velikosti 1 TB může dosáhnout maximálně 500 IOPS. Odhadované náklady na tento virtuální počítač za měsíc budou $1 570. Měsíční náklady na disky úložiště úrovně Standard 32 budou $1 638. Odhadované celkové měsíční náklady budou $3 208.

Pokud však používáte stejnou aplikaci na Premium Storage, budete potřebovat menší velikost virtuálního počítače a méně disků služby Premium Storage, čímž se sníží celkové náklady. Standardní \_ virtuální počítač DS13 může splňovat požadavky 16 000 IOPS pomocí čtyř disků P30. Virtuální počítač DS13 má maximální IOPS 25 600 a každý disk P30 má maximální počet IOPS 5 000. Celková Tato konfigurace může dosáhnout 5 000 x 4 = 20 000 IOPS. Odhadované náklady na tento virtuální počítač za měsíc budou $1 003. Měsíční náklady na čtyři disky P30 Premium Storage budou $544,34. Odhadované celkové měsíční náklady budou $1 544.

Následující tabulka shrnuje rozpis nákladů tohoto scénáře pro Standard a Premium Storage.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Náklady na virtuální počítač za měsíc** |$1 570,58 (standardní \_ D14) |$1 003,66 (standardní \_ DS13) |
| **Náklady na disky za měsíc** |$1 638,40 (32 × 1 TB disků) |$544,34 (4 x P30 disky) |
| **Celkové náklady za měsíc** |$3 208,98 |$1 544,34 |

*Linux distribuce*  

V případě Azure Premium Storage získáte stejnou úroveň výkonu pro virtuální počítače s Windows a Linux. Podporujeme spoustu různých typů Linux distribuce a [tady](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)si můžete prohlédnout úplný seznam. Je důležité si uvědomit, že různé distribuce jsou lépe vhodné pro různé typy úloh. V závislosti na distribuce, na kterém běží vaše zatížení, se zobrazí různé úrovně výkonu. Otestujte distribuce pro Linux s vaší aplikací a vyberte tu, která nejlépe funguje.

Pokud používáte systém Linux se Premium Storage, přečtěte si nejnovější aktualizace požadovaných ovladačů, abyste zajistili vysoký výkon.

## <a name="premium-storage-disk-sizes"></a>Velikosti disků úložiště úrovně Premium

Azure Premium Storage nabízí celou řadu velikostí, takže si můžete vybrat, který nejlépe vyhovuje vašim potřebám. Velikost každého disku má jiný limit škálování pro IOPS, šířku pásma a úložiště. Vyberte správnou Premium Storage velikost disku v závislosti na požadavcích aplikace a velikosti virtuálního počítače s vysokým rozsahem. V následující tabulce jsou uvedeny velikosti disků a jejich možnosti. Velikosti P4, P6, P15, P60, P70 a P80 se aktuálně podporují jenom pro Managed Disks.

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Počet disků, které zvolíte, závisí na zvolené velikosti disku. K splnění požadavku vaší aplikace můžete použít jeden disk s P50 nebo více disků P10. Při rozhodování Vezměte v úvahu níže uvedené otázky.

*Omezení škálování (IOPS a propustnost)*  
Omezení počtu vstupně-výstupních operací a propustnosti jednotlivých disků Premium se liší a nezávisí na limitech škálování virtuálního počítače. Ujistěte se, že celkový počet vstupně-výstupních operací a propustnosti z disků jsou v rámci omezení velikosti zvolené velikosti virtuálních počítačů.

Například pokud je požadavek aplikace v propustnosti maximálně 250 MB/s a používáte virtuální počítač DS4 s jedním diskem P30. Virtuální počítač DS4 může poskytovat propustnost až 256 MB/s. Jeden P30 disk však má omezení propustnosti 200 MB/s. V důsledku toho bude aplikace omezena v 200 MB/s kvůli limitu disku. Pokud chcete tento limit překonat, zřiďte na VIRTUÁLNÍm počítači více než jeden datový disk nebo změňte velikost disků na P40 nebo P50.

> [!NOTE]
> Čtení poskytované mezipamětí nejsou součástí vstupně-výstupních operací disku a propustnosti, a proto nepodléhají omezením disku. Mezipaměť má samostatný vstupně-výstupní operace a omezení propustnosti pro každý virtuální počítač.
>
> Například počáteční čtení a zápisy jsou 60MB/s a 40MB/s. V průběhu času se mezipaměť zahřívá a obsluhuje více a více čtení z mezipaměti. Pak můžete z disku získat vyšší propustnost zápisu.

*Počet disků*  
Určete počet disků, které budete potřebovat, pomocí vyhodnocení požadavků aplikace. Velikost každého virtuálního počítače má taky omezení počtu disků, které se dají připojit k virtuálnímu počítači. Obvykle se jedná o dvojnásobek počtu jader. Ujistěte se, že velikost virtuálního počítače, kterou zvolíte, může podporovat potřebný počet disků.

Pamatujte, že Premium Storage disky mají vyšší možnosti výkonu v porovnání se standardními disky úložiště. Proto pokud migrujete aplikaci z virtuálního počítače Azure IaaS pomocí služby Storage úrovně Standard na Premium Storage, budete pravděpodobně potřebovat méně prémiových disků, abyste dosáhli stejného nebo vyššího výkonu vaší aplikace.

## <a name="disk-caching"></a>Mezipaměť disku

Vysoce škálovatelné virtuální počítače, které využívají Azure Premium Storage, mají vícevrstvou technologii ukládání do mezipaměti s názvem BlobCache. BlobCache používá kombinaci paměti RAM hostitele a místního disku SSD pro ukládání do mezipaměti. Tato mezipaměť je k dispozici pro Premium Storage Trvalé disky a místní disky virtuálních počítačů. Ve výchozím nastavení je nastavení této mezipaměti nastaveno na čtení/zápis pro disky s operačním systémem a jen pro čtení pro datové disky hostované v Premium Storage. Díky ukládání do mezipaměti disku na Premium Storage discích může virtuální počítače s vysokým rozsahem dosáhnout extrémně vysoké úrovně výkonu, které překračují základní diskový výkon.

> [!WARNING]
> Disková mezipaměť není podporovaná pro disky 4 TiB a větší. Pokud je k VIRTUÁLNÍmu počítači připojeno více disků, bude každý disk, který je menší než 4 TiB, podporovat ukládání do mezipaměti.
>
> Při změně nastavení mezipaměti disku Azure se cílový disk odpojí a znovu připojí. Pokud se jedná o disk s operačním systémem, virtuální počítač se restartuje. Před změnou nastavení mezipaměti disku zastavte všechny aplikace nebo služby, které by mohly být tímto přerušením ovlivněny. Žádná z těchto doporučení by mohla vést k poškození dat.

Další informace o tom, jak BlobCache funguje, najdete v blogovém příspěvku v rámci služby [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) .

Je důležité povolit mezipaměť na pravé sadě disků. Bez ohledu na to, jestli byste měli povolit ukládání do mezipaměti disku na disk Premium, nebo ne, bude záviset na způsobu, jakým bude disk zpracovávat. Následující tabulka ukazuje výchozí nastavení mezipaměti pro operační systém a datové disky.

| **Typ disku** | **Výchozí nastavení mezipaměti** |
| --- | --- |
| Disk OS |ReadWrite |
| Datový disk |ReadOnly |

Pro datové disky se doporučuje nastavení pro diskovou mezipaměť:

| **Nastavení ukládání do mezipaměti disku** | **doporučení, kdy použít toto nastavení** |
| --- | --- |
| Žádný |Nakonfigurujte mezipaměť hosta jako žádná pro disky jen pro zápis a zápis s velkým množstvím. |
| ReadOnly |Nakonfigurujte mezipaměť hosta jako ReadOnly pro disky jen pro čtení a pro čtení i zápis. |
| ReadWrite |Nakonfigurujte mezipaměť hosta jako jen pro čtení, pokud vaše aplikace správně zpracovává zápis dat uložených v mezipaměti na trvalé disky v případě potřeby. |

*ReadOnly*  
Když nakonfigurujete ukládání do mezipaměti Premium Storage na datových discích jen pro čtení, můžete dosáhnout nízké latence čtení a u vaší aplikace získat velmi vysoký počet vstupně-výstupních operací a propustnosti. Důvodem je dva z následujících důvodů.

1. Čtení prováděné z mezipaměti, které je na paměti virtuálního počítače a místní disk SSD, je mnohem rychlejší než čtení z datového disku, který je v úložišti objektů BLOB v Azure.  
1. Premium Storage nepočítá čtení poskytované z mezipaměti k vstupně-výstupním operacím disku a propustnosti. Proto vaše aplikace může dosáhnout vyššího celkového počtu vstupně-výstupních operací a propustnosti.

*ReadWrite*  
Ve výchozím nastavení mají disky s operačním systémem povoleno ukládání do mezipaměti. Nedávno jsme do datových disků přidali podporu ukládání do mezipaměti pro čtení a zápis. Pokud používáte ukládání do mezipaměti pro čtení a zápis, musíte mít správný způsob, jak zapisovat data z mezipaměti do trvalých disků. SQL Server například zpracovává zápis dat uložených v mezipaměti na trvalé disky úložiště sami. Použití mezipaměti s podporou přečtení z aplikace, která nezpracovává trvalá potřebná data, může způsobit ztrátu dat, pokud dojde k chybě virtuálního počítače.

*Žádný*  
V současné době se **žádná** podpora na datových discích nepodporuje. Na discích s operačním systémem se nepodporuje. Pokud jste na disku s operačním systémem nastavili **možnost žádné** , přepíše se to interně a nastaví se na **jen pro čtení**.

V takovém případě můžete tyto pokyny použít k SQL Server spuštění na Premium Storage provedením následujících kroků:

1. Nakonfigurujte mezipaměť "ReadOnly" na discích úložiště úrovně Premium, které hostují datové soubory.  
   a.  Rychlá operace čtení z mezipaměti snižuje dobu SQL Server dotazu, protože se z mezipaměti v porovnání s přímo z datových disků načítá mnohem rychleji velikost datových stránek.  
   b.  Obsluha čtení z mezipaměti znamená, že je k dispozici další propustnost z datových disků Premium. SQL Server může tuto dodatečnou propustnost využít k načítání dalších datových stránek a dalších operací, jako je zálohování a obnovení, dávkové načtení a opětovné sestavení indexu.  
1. Nakonfigurujte mezipaměť None na discích úložiště úrovně Premium, které hostují soubory protokolů.  
   a.  Soubory protokolu mají hlavně operace náročné na zápis. Proto nevyužívají mezipaměť jen pro čtení.

## <a name="optimize-performance-on-linux-vms"></a>Optimalizace výkonu pro virtuální počítače se systémem Linux

U všech disků úrovně Premium SSD nebo Ultra s mezipamětí nastavenou na **ReadOnly** nebo **žádné**je při připojování systému souborů nutné zakázat "překážky". V tomto scénáři nepotřebujete překážky, protože zápisy na disky Premium Storage jsou pro tato nastavení mezipaměti trvalé. Po úspěšném dokončení žádosti o zápis se data zapisují do trvalého úložiště. Pokud chcete zakázat "překážky", použijte jednu z následujících metod. Vyberte jednu z těchto souborů v systému souborů:
  
* Pro **reiserFS**zakažte překážky pomocí `barrier=none` Možnosti připojit. (Pokud chcete povolit bariéry, použijte `barrier=flush` .)
* V případě **ext3/ext4**zakažte překážky pomocí `barrier=0` Možnosti připojit. (Pokud chcete povolit bariéry, použijte `barrier=1` .)
* Pro **XFS**zakažte překážky pomocí `nobarrier` Možnosti připojit. (Pokud chcete povolit bariéry, použijte `barrier` .)
* U disků služby Premium Storage s mezipamětí nastavenou **na hodnotu**nepoužívat jako mezipaměť povolte překážky při zápisu.
* Aby jmenovky svazků po restartování virtuálního počítače zůstaly zachované, je nutné aktualizovat/etc/fstab s použitím univerzálně jedinečného identifikátoru (UUID) na disky. Další informace najdete v tématu [Přidání spravovaného disku do virtuálního počítače se systémem Linux](../articles/virtual-machines/linux/add-disk.md).

Pro prémiové SSD byly ověřeny následující distribuce systému Linux. Pro zajištění lepšího výkonu a stability pomocí Premium SSD doporučujeme, abyste provedli upgrade virtuálních počítačů na některou z těchto verzí nebo novější. 

Některé verze vyžadují nejnovější služby Linux Integration Services (LIS), v 4.0 pro Azure. Chcete-li stáhnout a nainstalovat distribuci, postupujte podle odkazu uvedeného v následující tabulce. Do seznamu přidáme obrázky, protože jsme dokončili ověřování. Naše ověřování ukazují, že se výkon u jednotlivých imagí liší. Výkon závisí na charakteristikách úloh a na nastaveních imagí. Různé obrázky jsou vyladěny pro různé druhy úloh.

| Distribuce | Verze | Podporované jádro | Podrobnosti |
| --- | --- | --- | --- |
| Ubuntu | 12,04 nebo novější| 3.2.0 – 75.110 + | &nbsp; |
| Ubuntu | 14,04 nebo novější| 3.13.0 – 44.73 +  | &nbsp; |
| Debian | 7. x, 8. x nebo novější| 3.16.7-ckt4-1 + | &nbsp; |
| SUSE | SLES 12 nebo novější| 3.12.36 – 38.1 + | &nbsp; |
| SUSE | SLES 11 SP4 nebo novější| 3.0.101 – 0.63.1 + | &nbsp; |
| CoreOS | 584.0.0 + nebo novější| 3.18.4 + | &nbsp; |
| CentOS | 6,5, 6,6, 6,7, 7,0 nebo novější| &nbsp; | [Požadováno LIS4](https://www.microsoft.com/download/details.aspx?id=55106) <br> *Viz Poznámka v následující části.* |
| CentOS | 7.1 + nebo novější| 3.10.0-229.1.2. el7 + | [LIS4 Doporučené](https://www.microsoft.com/download/details.aspx?id=55106) <br> *Viz Poznámka v následující části.* |
| Red Hat Enterprise Linux (RHEL) | 6.8 +, 7.2 + nebo novější | &nbsp; | &nbsp; |
| Oracle | 6.0 +, 7.2 + nebo novější | &nbsp; | UEK4 nebo RHCK |
| Oracle | 7.0-7.1 nebo novější | &nbsp; | UEK4 nebo RHCK s[LIS4](https://www.microsoft.com/download/details.aspx?id=55106) |
| Oracle | 6.4 – 6.7 nebo novější | &nbsp; | UEK4 nebo RHCK s[LIS4](https://www.microsoft.com/download/details.aspx?id=55106) |

### <a name="lis-drivers-for-openlogic-centos"></a>Ovladače LIS pro OpenLogic CentOS

Pokud používáte virtuální počítače s OpenLogic CentOS, spusťte následující příkaz, který nainstaluje nejnovější ovladače:

```
sudo yum remove hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
sudo reboot
```

V některých případech příkaz výše provede upgrade jádra i. Pokud se vyžaduje aktualizace jádra, možná budete muset po restartování znovu spustit výše uvedené příkazy, abyste mohli plně nainstalovat balíček Microsoft-Hyper-v.


## <a name="disk-striping"></a>Diskové svazky

Když je virtuální počítač s vysokým škálováním připojený k několika trvalým diskům Premium Storage, můžou se disky rozložit dohromady a agregovat tak jejich IOPs, šířku pásma a kapacitu úložiště.

Ve Windows můžete pomocí prostorů úložiště prokládat disky společně. Pro každý disk ve fondu musíte nakonfigurovat jeden sloupec. V opačném případě může být celkový výkon prokládaného svazku nižší, než bylo očekáváno, vzhledem k nerovnoměrné distribuci provozu mezi disky.

Důležité: pomocí Správce serveru uživatelského rozhraní můžete pro prokládaný svazek nastavit celkový počet sloupců o velikosti až 8. Při připojování více než osmi disků použijte PowerShell k vytvoření svazku. Pomocí prostředí PowerShell můžete nastavit počet sloupců, které se rovnají počtu disků. Například pokud je v jedné sadě Stripe nastavený 16 disků; v parametru *NumberOfColumns* rutiny *New-VirtualDisk* prostředí PowerShell zadejte 16 sloupců.

V systému Linux pomocí nástroje MDADM propojte disky společně. Podrobný postup pro proložení disků v systému Linux najdete v tématu [Konfigurace softwarového pole RAID v systému Linux](../articles/virtual-machines/linux/configure-raid.md).

*Velikost pruhu*  
Důležitou konfigurací při proložení disku je velikost pruhu. Velikost nebo velikost bloku je nejmenší datový blok, který aplikace může adresovat na prokládaný svazek. Velikost pruhu, kterou nakonfigurujete, závisí na typu aplikace a jeho vzoru požadavků. Pokud zvolíte špatnou velikost pruhu, může to vést k chybnému zarovnání v/v, což vede ke snížení výkonu aplikace.

Pokud je například požadavek v/v generovaný vaší aplikací větší než velikost diskového pruhu, systém úložiště ho zapisuje přes hranice prokládaných jednotek na více než jednom disku. Pokud je čas na přístup k těmto datům, bude nutné vyhledat v rámci více jednotek Stripe, aby bylo možné požadavek dokončit. Kumulativní účinek takového chování může vést k výraznému snížení výkonu. Na druhou stranu platí, že pokud je velikost vstupně-výstupních operací menší než velikost pruhu, a pokud je náhodná, můžou požadavky na vstupně-výstupní operace přidat na stejný disk, který způsobuje kritické body, a nakonec zpomalit výkon v/v.

V závislosti na typu zatížení, na kterém je aplikace spuštěná, vyberte vhodnou velikost pruhu. V případě náhodných malých vstupně-výstupních požadavků použijte menší velikost pruhu. Vzhledem k tomu, že velké sekvenční vstupně-výstupní požadavky používají větší velikost pruhu. Zjistěte doporučení pro velikost stripe pro aplikaci, kterou budete používat na Premium Storage. Pro SQL Server nakonfigurujte velikost Stripe 64 KB pro úlohy OLTP a 256 KB pro úlohy datových skladů. Další informace najdete v tématu [osvědčené postupy výkonu pro SQL Server na virtuálních počítačích Azure](../articles/azure-sql/virtual-machines/windows/performance-guidelines-best-practices.md#disks-guidance) .

> [!NOTE]
> Na virtuálním počítači řady DS 64 a na discích úrovně Premium úložiště na VIRTUÁLNÍm počítači řady GS můžete prokládat maximálně 32 disků úložiště úrovně Premium.

## <a name="multi-threading"></a>Vícevláknové zpracování

Azure navrhl, aby se Premium Storage platforma hromadně paralelní. Aplikace s více vlákny proto dosahují mnohem vyššího výkonu než aplikace s jedním vláknem. Vícevláknová aplikace rozdělí své úkoly napříč více vlákny a zvýší efektivitu jejího spuštění díky využití prostředků virtuálního počítače a disku na maximum.

Například pokud vaše aplikace běží na jednom jádru virtuálního počítače pomocí dvou vláken, může procesor přepínat mezi těmito dvěma vlákny a dosáhnout tak efektivity. Zatímco jedno vlákno čeká na dokončení operace v/v disku, může se procesor přepnout do druhého vlákna. Tímto způsobem mohou dvě vlákna dosáhnout více než jednoho vlákna. Pokud má virtuální počítač více než jedno jádro, tím se zkrátí doba běhu, protože každý jádro může spouštět úlohy paralelně.

Je možné, že nebudete moct změnit způsob, jakým aplikace vypnula z jednoho vlákna nebo více vláken. SQL Server je například schopný zpracovávat více PROCESORů a vícejádrových jader. SQL Server se však rozhodne za jakých podmínek bude využití jednoho nebo více vláken pro zpracování dotazu. Může spouštět dotazy a sestavovat indexy pomocí vícevláknového dělení. Pro dotaz, který zahrnuje spojování velkých tabulek a řazení dat před vrácením uživateli, SQL Server nejspíš použít více vláken. Uživatel však nemůže určit, zda SQL Server spustí dotaz pomocí jednoho nebo více podprocesů.

Existuje nastavení konfigurace, které můžete změnit, aby se ovlivnilo toto multithreading nebo paralelní zpracování aplikace. Například v případě SQL Server se jedná o maximální stupeň konfigurace paralelismu. Toto nastavení s názvem MAXDOP umožňuje nakonfigurovat maximální počet procesorů, které může SQL Server použít při paralelním zpracování. MAXDOP můžete nakonfigurovat pro jednotlivé dotazy nebo operace indexu. To je užitečné v případě, že chcete vyvážit prostředky systému pro kritickou aplikaci s výkonem.

Řekněme například, že aplikace pomocí SQL Server spouští velký dotaz a operaci indexu ve stejnou dobu. Můžeme předpokládat, že jste chtěli, aby operace indexu byla v porovnání s velkým dotazem větší. V takovém případě můžete nastavit hodnotu MAXDOP operace indexu tak, aby byla vyšší než hodnota MAXDOP dotazu. Tímto způsobem SQL Server více procesorů, které může využít pro operaci indexu v porovnání s počtem procesorů, které může vyhradit velký dotaz. Nezapomeňte, že neřídíte počet vláken, SQL Server budou použity pro každou operaci. Můžete řídit maximální počet procesorů vyhrazených pro multithreading.

Další informace o [stupních paralelismu](https://technet.microsoft.com/library/ms188611.aspx) v SQL Server. Zjistěte taková nastavení, která mají vliv na více vláken ve vaší aplikaci a jejich konfigurace pro optimalizaci výkonu.

## <a name="queue-depth"></a>Hloubka fronty

Hloubka fronty nebo délka fronty nebo velikost fronty je počet čekajících vstupně-výstupních požadavků v systému. Hodnota hloubky fronty určuje, kolik vstupně-výstupních operací, které může vaše aplikace obsahovat, se budou zpracovávat disky úložiště. Má vliv na všechny tři indikátory výkonu aplikací, které jsme provedli v tomto článku viz., IOPS, propustnost a latence.

Hloubka fronty a Multithreading s více vlákny úzce souvisejí. Hodnota hloubky fronty určuje, kolik více vláken může aplikace dosáhnout. Pokud je hloubka fronty velká, aplikace může provádět více operací souběžně, jinými slovy, více vlákny. Pokud je hloubka fronty malá, a to i v případě, že je aplikace Vícevláknová, nebude mít dostatek požadavků pro souběžné provádění.

U aplikací v poli poličky se obvykle nepovoluje Změna hloubky fronty, protože pokud je nastavena nesprávně, bude to mít větší vliv. Aplikace nastaví správnou hodnotu hloubky fronty pro dosažení optimálního výkonu. Je ale důležité pochopit tento koncept, abyste mohli řešit problémy s výkonem ve vaší aplikaci. Vlivem hloubky fronty můžete také sledovat spuštěním nástrojů srovnávacích testů na vašem systému.

Některé aplikace poskytují nastavení pro ovlivnění hloubky fronty. Například nastavení MAXDOP (maximální stupeň paralelismu) v SQL Server vysvětleno v předchozím oddílu. MAXDOP je způsob, jak ovlivnit hloubku fronty a multithreading, i když se nemění hodnota hloubky fronty SQL Server.

*Vysoká hloubka fronty*  
Vysoká hloubka fronty na disku přechází více operací. Disk ještě před časem ví o další požadavek ve frontě. V důsledku toho může disk plánovat operace předem a zpracovávat je v optimální posloupnosti. Vzhledem k tomu, že aplikace posílá více požadavků na disk, může disk zpracovávat více paralelních IOs. Nakonec bude aplikace moci dosáhnout vyššího počtu vstupně-výstupních operací. Vzhledem k tomu, že aplikace zpracovává více požadavků, zvyšuje celková propustnost aplikace také.

Aplikace obvykle může dosáhnout maximální propustnosti s 8 až 16 a nezpracovanými IOs na připojený disk. Pokud je hloubka fronty jedna, aplikace nemá dostatek systému IOs pro systém a v daném období zpracuje méně místa. Jinými slovy, menší propustnost.

Například v SQL Server se nastaví hodnota MAXDOP pro dotaz na "4" SQL Server tím, že může použít až čtyři jádra pro spuštění dotazu. SQL Server určí, co je nejlepší hodnota hloubky fronty a počet jader pro provedení dotazu.

*Optimální hloubka fronty*  
Velmi vysoká hodnota hloubky fronty má také své nevýhody. Pokud je hodnota hloubky fronty příliš vysoká, pokusí se aplikace zařídit velmi vysoký IOPS. Pokud aplikace nemá trvalé disky s dostatečným zřízeným IOPS, může to negativně ovlivnit latenci aplikace. Následující vzorec ukazuje vztah mezi vstupně-výstupními operacemi za sekundu, latencí a hloubkou fronty.  
    ![Diagram znázorňující, že se v rovnici v době, kdy se jedná O latenci, rovná hloubka fronty.](media/premium-storage-performance/image6.png)

Hloubku fronty byste neměli konfigurovat na žádnou vysokou hodnotu, ale na optimální hodnotu, která může poskytovat dostatek IOPS pro aplikaci, aniž by to mělo vliv na latenci. Pokud například latence aplikace musí být 1 milisekunda, hloubka fronty potřebná k dosažení 5 000 IOPS je, hloubka fronty = 5000 x 0,001 = 5.

*Hloubka fronty pro prokládaný svazek*  
U prokládaného svazku Udržujte dostatečně velkou hloubku fronty, takže každý disk má nejvyšší hloubku fronty ve špičce. Představte si například aplikaci, která nahraje hloubku fronty 2 a v pruzích jsou čtyři disky. Dvě vstupně-výstupní požadavky budou přijít na dva disky a zbývající dva disky budou nečinné. Proto nakonfigurujte hloubku fronty tak, aby všechny disky mohly být zaneprázdněné. Vzorec níže ukazuje, jak určit hloubku fronty prokládaných svazků.  
    ![Diagram znázorňující, že se rovnice Q D na disk vynásobí počtem sloupců na jeden svazek, který se rovná Q D prokládaného svazku.](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Omezování

Azure Premium Storage zřídí zadaný počet vstupně-výstupních operací za sekundu v závislosti na velikosti virtuálních počítačů a velikosti disků, které si zvolíte. Kdykoli se vaše aplikace pokusí o zpracování IOPS nebo propustnosti nad rámec těchto limitů, které může virtuální počítač nebo disk zvládnout, Premium Storage ho omezí. Tyto manifesty ve formě sníženého výkonu ve vaší aplikaci. To může znamenat vyšší latenci, nižší propustnost nebo nižší IOPS. Pokud Premium Storage neomezuje, vaše aplikace by mohla být zcela neúspěšná, protože by se překročilo, jaké prostředky je možné dosáhnout. Aby se zabránilo problémům s výkonem kvůli omezení, vždy pro vaši aplikaci zajistěte dostatek prostředků. Vezměte v úvahu, co jsme probrali v oddílech velikosti virtuálních počítačů a velikosti disků výše. Srovnávací testy je nejlepším způsobem, jak zjistit, jaké prostředky budete potřebovat k hostování vaší aplikace.

## <a name="next-steps"></a>Další kroky

