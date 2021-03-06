---
title: Import a spuštění poznámkových bloků z úložiště GitHub do Azure Cosmos DB
description: Naučte se připojit k GitHubu a importovat poznámkové bloky z úložiště GitHub do svého účtu Azure Cosmos. Po importu je můžete spustit, upravit a uložit změny zpátky do GitHubu.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/19/2020
ms.openlocfilehash: d85f020152fa3cadb1d437c125d327f5e895e14e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85262884"
---
# <a name="import-notebooks-from-a-github-repo-into-azure-cosmos-db"></a>Import poznámkových bloků z úložiště GitHub do Azure Cosmos DB

Po [Povolení podpory poznámkových bloků](enable-notebooks.md) pro účty Azure Cosmos můžete vytvářet nové poznámkové bloky, nahrávat nové poznámkové bloky z místního počítače nebo importovat stávající poznámkové bloky z vašich účtů GitHubu. Tento článek ukazuje, jak připojit pracovní prostor vašich poznámkových bloků k GitHubu a importovat poznámkové bloky z úložiště GitHub do svého účtu Azure Cosmos. Po importu je můžete spustit, provést změny a uložit změny zpátky do GitHubu.

## <a name="get-notebooks-from-github"></a>Získat poznámkové bloky z GitHubu

Můžete se připojit k vlastním úložištím GitHubu nebo k jiným veřejným úložištím GitHubu, abyste mohli číst, vytvářet a sdílet poznámkové bloky v Azure Cosmos DB. K připojení k účtu GitHubu použijte následující postup:

1. Přihlaste se [Azure Portal](https://portal.azure.com/) a přejděte k účtu Azure Cosmos.

1. Otevřete kartu **Průzkumník dat** . Na této kartě se zobrazí všechny vaše existující databáze, kontejnery a poznámkové bloky.

1. Vyberte položku nabídky **připojit k GitHubu** .

1. Otevře se karta, kde se můžete rozhodnout, že se chcete připojit pouze k **veřejným** úložištím nebo **veřejným a soukromým**úložištím.  Po výběru požadované možnosti vyberte **autorizovat přístup**. Pro Azure Cosmos DB přístupu k úložištím ve vašem účtu GitHub se vyžaduje autorizace.

   :::image type="content" source="./media/import-github-notebooks/authorize-access-github.png" alt-text="Autorizovat Azure Cosmos DB k přístupu k úložištím GitHubu":::

1. Budete přesměrováni na webovou stránku "github.com", kde můžete potvrdit autorizaci. Vyberte tlačítko **autorizovat AzureCosmosDBNotebooks** a do příkazového řádku zadejte heslo účtu GitHub.

1. Po úspěšném ověření přejdete zpátky na svůj účet Azure Cosmos. Pak můžete zobrazit všechna úložiště veřejného a soukromého úložiště z vašeho účtu GitHub. Můžete vybrat úložiště ze seznamu k dispozici nebo přidat úložiště přímo pomocí jeho adresy URL.

1. Jakmile vyberete požadované úložiště, položka úložiště se přesune z oddílu **nepřipnutých úložišť** do **připnutých úložišť** . V případě potřeby můžete také zvolit konkrétní větev úložiště, ze které se mají importovat poznámkové bloky.

   :::image type="content" source="./media/import-github-notebooks/choose-repo-branch.png" alt-text="Výběr úložiště a větve":::

1. Kliknutím na **tlačítko OK** dokončete operaci importu. Všechny poznámkové bloky dostupné ve vybrané větvi úložiště se importují do svého účtu Azure Cosmos.

Po integraci s účtem GitHubu uvidíte jenom seznam úložišť a poznámkových bloků v účtu Azure Cosmos. Tento příkaz platí i v případě, že se k účtu Azure Cosmos DB přihlašuje více uživatelů a přidáte vlastní účty. Jinými slovy, více uživatelů může použít stejný účet Azure Cosmos k připojení pracovního prostoru poznámkového bloku k GitHubu. Každý uživatel ale uvidí jenom seznam úložišť a poznámkových bloků, které naimportovali. Poznámkové bloky importované jinými uživateli nejsou pro vás vidět.

Pokud chcete odpojit svůj účet GitHubu z pracovního prostoru poznámkových bloků, otevřete kartu **Průzkumník dat** , vyberte možnost `…` vedle **úložišť GitHub** a vyberte **Odpojit od GitHubu**.

## <a name="edit-a-notebook-and-push-changes-to-github"></a>Úprava poznámkového bloku a vložení změn do GitHubu

Můžete upravit existující Poznámkový blok nebo přidat nový Poznámkový blok do úložiště a uložit změny zpátky do GitHubu.

Po úpravě existujícího poznámkového bloku vyberte **Save (Uložit**). Otevře se dialogové okno, kde můžete zadat potvrzovací zprávu pro změny, které jste udělali. Vyberte **Potvrdit** a Poznámkový blok na GitHubu se aktualizuje. Aktualizace můžete ověřit tak, že se přihlásíte k účtu GitHub a ověříte historii potvrzení změn.

V normálním toku GitHub po potvrzení změn se obvykle dokončí změny do vzdáleného. V takovém případě ale možnost potvrzení slouží jako "fázování, potvrzování a" doručování "vašich aktualizací do GitHubu.

:::image type="content" source="./media/import-github-notebooks/commit-changes-github.png" alt-text="Upravit poznámkové bloky a potvrdit změny do GitHubu":::

## <a name="next-steps"></a>Další kroky

* Přečtěte si o výhodách [Azure Cosmos DB poznámkových blocích Jupyter.](cosmosdb-jupyter-notebooks.md)

