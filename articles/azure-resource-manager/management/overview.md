---
title: Přehled Azure Resource Manageru
description: Popisuje, jak Azure Resource Manager využívat k nasazení, správě a řízení přístupu k prostředkům v Azure.
ms.topic: overview
ms.date: 04/21/2020
ms.openlocfilehash: 089919e227b33859dbeabd98ecd75845a28a3f42
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087023"
---
# <a name="what-is-azure-resource-manager"></a>Co je Azure Resource Manager?

Azure Resource Manager je služba nasazování a správy pro Azure. Poskytuje vrstvu pro správu, která umožňuje vytvářet, aktualizovat a odstraňovat prostředky v účtu Azure. Pomocí funkcí správy, jako jsou řízení přístupu, zámky a značky, můžete zabezpečit a organizovat prostředky po nasazení.

Další informace o šablonách Azure Resource Manager najdete v tématu [template Deployment Overview](../templates/overview.md).

## <a name="consistent-management-layer"></a>Konzistentní vrstva správy

Když uživatel odešle žádost ze všech nástrojů, rozhraní API nebo sad Azure, Správce prostředků obdrží požadavek. Ověřuje a autorizuje požadavek. Správce prostředků odešle požadavek službě Azure, která provede požadovanou akci. Vzhledem k tomu, že všechny požadavky jsou zpracovávány přes stejné rozhraní API, zobrazí se konzistentní výsledky a možnosti ve všech různých nástrojích.

Následující obrázek ukazuje, Azure Resource Manager role hraje při zpracování požadavků Azure.

![Model požadavku Resource Manageru](./media/overview/consistent-management-layer.png)

Všechny funkce, které jsou k dispozici na portálu, jsou také dostupné prostřednictvím PowerShellu, rozhraní příkazového řádku Azure CLI, rozhraní REST API a klientských sad SDK. Funkce původně vydané prostřednictvím rozhraní API budou na portálu k dispozici do 180 dnů od počátečního vydání.

## <a name="terminology"></a>Terminologie

Pokud s Azure Resource Managerem začínáte, existuje několik termínů, které možná neznáte.

* **prostředek** - Spravovatelná položka, která je k dispozici prostřednictvím služby Azure. Příklady prostředků jsou virtuální počítače, účty úložiště, webové aplikace, databáze a virtuální sítě. Příklady prostředků jsou také skupiny prostředků, předplatná, skupiny pro správu a značky.
* **Skupina prostředků** – kontejner, který obsahuje související prostředky pro řešení Azure. Skupina prostředků zahrnuje ty prostředky, které chcete spravovat jako skupinu. O tom, které prostředky do skupiny prostředků patří, rozhodujete vy na základě toho, co je pro vaši organizaci nejvhodnější. Viz [Skupiny prostředků](#resource-groups).
* **poskytovatel prostředků** – služba poskytující prostředky Azure. Například běžný poskytovatel prostředků je Microsoft. COMPUTE, který poskytuje prostředek virtuálního počítače. Microsoft. Storage je další společný poskytovatel prostředků. Viz téma [poskytovatelé a typy prostředků](resource-providers-and-types.md).
* **Správce prostředků šablonu** – soubor JavaScript Object Notation (JSON), který definuje jeden nebo více prostředků pro nasazení do skupiny prostředků, předplatného, skupiny pro správu nebo tenanta. Šablony lze použít k nasazení prostředků konzistentně a opakovaně. Další informace najdete v tématu [přehled Template Deployment](../templates/overview.md).
* **deklarativní syntaxe** – Syntaxe, která umožňuje prohlásit „Toto mám v úmyslu vytvořit“, aniž by k tomu bylo nutné psát sekvence programových příkazů. Šablona Resource Manageru je příkladem deklarativní syntaxe. V souboru definujete vlastnosti pro infrastrukturu k nasazení do Azure.  Další informace najdete v tématu [přehled Template Deployment](../templates/overview.md).

## <a name="the-benefits-of-using-resource-manager"></a>Výhody použití Resource Manageru

Pomocí Správce prostředků můžete:

* Spravujte svoji infrastrukturu prostřednictvím deklarativních šablon místo skriptů.

* Nasaďte, spravujte a monitorujte všechny prostředky pro vaše řešení jako skupinu, místo toho, aby se tyto prostředky nemusely zpracovávat jednotlivě.

* Znovu nasaďte řešení v průběhu životního cyklu vývoje a měli byste mít jistotu, že se prostředky nasazují v konzistentním stavu.

* Definujte závislosti mezi prostředky tak, aby byly nasazeny ve správném pořadí.

* Použijte řízení přístupu ke všem službám, protože Access Control na základě rolí je nativně integrovaná do platformy pro správu.

* Použijte značky pro prostředky k logickému uspořádání všech prostředků v rámci vašeho předplatného.

* Vyjasnění fakturace vaší organizace zobrazením nákladů na skupinu prostředků, které sdílejí stejnou značku.

## <a name="understand-scope"></a>Orientace v oborech

Azure poskytuje čtyři úrovně rozsahu: [skupiny pro správu](../../governance/management-groups/overview.md), předplatná, [skupiny prostředků](#resource-groups)a prostředky. Následující obrázek ukazuje příklad těchto vrstev.

![Úrovně správy](./media/overview/scope-levels.png)

Nastavení správy můžete použít na jakékoli z těchto úrovní rozsahu. Vybraná úroveň určuje rozsah použití nastavení. Nižší úrovně dědí nastavení z vyšších úrovní. Když například použijete [zásadu](../../governance/policy/overview.md) pro předplatné, zásada se použije na všechny skupiny prostředků a prostředky v rámci vašeho předplatného. Když použijete zásadu pro skupinu prostředků, tato zásada se použije pro skupinu prostředků a všechny její prostředky. U jiné skupiny prostředků ale tato přiřazení zásad neplatí.

Šablony můžete nasadit do klientů, skupin pro správu, předplatných nebo skupin prostředků.

## <a name="resource-groups"></a>Skupiny prostředků

Při definování skupin prostředků byste měli vzít v úvahu některé důležité faktory:

* Všechny prostředky ve skupině by měly sdílet stejný životní cyklus. Nasazujete, aktualizujete a odstraňujete je společně. Pokud jeden prostředek, třeba server, musí existovat v jiném cyklu nasazení, měl by být v jiné skupině prostředků.

* Každý prostředek může být jenom v jedné skupině prostředků.

* Některé prostředky můžou existovat mimo skupinu prostředků. Tyto prostředky se nasazují do [předplatného](../templates/deploy-to-subscription.md), [skupiny pro správu](../templates/deploy-to-management-group.md)nebo [tenanta](../templates/deploy-to-tenant.md). V těchto oborech jsou podporovány pouze konkrétní typy prostředků.

* Prostředky je možné do skupiny prostředků kdykoli přidat nebo naopak odebrat.

* Prostředky je možné přesouvat mezi skupinami. Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](move-resource-group-and-subscription.md).

* Skupina prostředků může obsahovat prostředky, které se nacházejí v různých oblastech.

* Skupinu prostředků lze využít k určení rozsahu řízení přístupu pro akce správy.

* Prostředek může interagovat s prostředky v dalších skupinách prostředků. Tato interakce je běžná v případě, že spolu tyto dva prostředky souvisejí, ale nesdílejí stejný životní cyklus (například webové aplikace, které se připojují k databázi).

Při vytváření skupiny prostředků pro ni musíte zadat umístění. Asi vás zajímá, proč skupina prostředků potřebuje umístění. A proč vůbec záleží na umístění skupiny prostředků, pokud prostředky mohou mít jiná umístění než skupina prostředků. Skupina prostředků ukládá metadata o prostředcích. Když zadáte umístění pro skupinu prostředků, určíte, kde jsou tato metadata uložená. Z důvodu dodržování předpisů může být nutné zajistit, aby se data ukládala v určité oblasti.

Pokud je oblast skupiny prostředků dočasně nedostupná, nemůžete aktualizovat prostředky ve skupině prostředků, protože metadata nejsou k dispozici. Prostředky v jiných oblastech budou pořád fungovat podle očekávání, ale nemůžete je aktualizovat. Další informace o vytváření spolehlivých aplikací najdete v tématu [navrhování spolehlivých aplikací Azure](/azure/architecture/checklist/resiliency-per-service).

## <a name="resiliency-of-azure-resource-manager"></a>Odolnost Azure Resource Manager

Služba Azure Resource Manager je navržena pro zajištění odolnosti a nepřetržité dostupnosti. Správce prostředků a řídící operace roviny (požadavky odeslané na management.azure.com) v REST API jsou:

* Distribuované napříč oblastmi. Některé služby jsou regionální.

* Distribuováno mezi Zóny dostupnosti (jako v oblastech) v umístěních, která mají více Zóny dostupnosti.

* Nezávislá na jednom logickém datovém centru.

* Pro aktivity údržby se nikdy neukončí.

Tato odolnost se vztahuje na služby, které přijímají požadavky prostřednictvím Správce prostředků. Například Key Vault výhody z této odolnosti.

## <a name="next-steps"></a>Další kroky

* Další informace o přesouvání prostředků najdete v tématu [Přesunutí prostředků do nové skupiny prostředků nebo předplatného](move-resource-group-and-subscription.md).

* Další informace o označování prostředků najdete v tématu [použití značek k uspořádání prostředků Azure](tag-resources.md).

* Další informace o uzamykání prostředků najdete v tématu [uzamčení prostředků, aby nedocházelo k neočekávaným změnám](lock-resources.md).
