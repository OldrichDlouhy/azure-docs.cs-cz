---
title: Soubory a adresáře v Azure Batch
description: Přečtěte si o souborech a adresářích a o tom, jak se používají v Azure Batch pracovním postupu z hlediska vývoje.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: e7babb7e2cfdbbe78f61be766c549c1e80cacf98
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83791117"
---
# <a name="files-and-directories-in-azure-batch"></a>Soubory a adresáře v Azure Batch

V Azure Batch každý úkol má pracovní adresář, pod kterým se vytvoří nula nebo více souborů a adresářů. Tento pracovní adresář lze použít pro ukládání programu, který je spuštěn úkolem, dat, která zpracovává, a výstupu zpracování, které provádí. Všechny soubory a adresáře úkolu jsou vlastněné uživatelem úkolu.

Služba Batch zveřejňuje část systému souborů na uzlu jako *kořenový adresář*. Úkoly mohou získat přístup do kořenového adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_ROOT_DIR`. Další informace o používání proměnných prostředí naleznete v tématu [Nastavení prostředí pro úkoly](jobs-and-tasks.md#environment-settings-for-tasks).

Kořenový adresář obsahuje následující adresářovou strukturu:

! [Adresářová struktura výpočetního uzlu] [media\files-and-directories\node-folder-structure.png]

- **aplikace**: obsahuje informace o podrobnostech balíčků aplikací nainstalovaných na výpočetním uzlu. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_APP_PACKAGE`.

- **fsmounts**: adresář obsahuje všechny systémy souborů, které jsou připojeny na výpočetním uzlu. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_MOUNTS_DIR`. Další informace najdete v tématu [připojení virtuálního systému souborů ve fondu služby Batch](virtual-file-mount.md).

- **shared**: Tento adresář poskytuje přístup pro čtení a zápis pro *všechny* úkoly, které jsou spouštěny na uzlu. Každý úkol spuštěný na uzlu může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_SHARED_DIR`.

- **startup**: Tento adresář je využíván spouštěcím úkolem jako jeho pracovní adresář. Jsou sem uloženy všechny soubory, které byly staženy do uzlu spouštěcím úkolem. Spouštěcí úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_STARTUP_DIR`.

- **volatile**: Tento adresář slouží pro interní účely. Není nijak zaručeno, že všechny soubory v tomto adresáři nebo samotný adresář budou v budoucnu existovat.

- **pracovní položky**: Tento adresář obsahuje adresáře pro úlohy a jejich úkoly na výpočetním uzlu.

    V adresáři **pracovní položky** se vytvoří adresář **úkoly** pro každý úkol, který běží na uzlu. K tomuto adresáři se dá dostat odkazem na `AZ_BATCH_TASK_DIR` proměnnou prostředí.
    
    Ve všech adresářích **úloh** vytvoří služba Batch pracovní adresář (), `wd` jehož jedinečná cesta je určena `AZ_BATCH_TASK_WORKING_DIR` proměnnou prostředí. Tento adresář poskytuje přístup pro čtení a zápis pro úkol. Úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Tento adresář je zachován podle pravidel omezení *RetentionTime*, které je zadáno pro úkol.

    `stdout.txt`Soubory a `stderr.txt` jsou zapsány do složky **úkoly** během provádění úlohy.

> [!IMPORTANT]
> Pokud uzel je odebrán z fondu, jsou odebrány všechny soubory, které jsou uloženy na uzlu.

## <a name="next-steps"></a>Další kroky

- Přečtěte si o [zpracování chyb a detekci](error-handling.md) v Azure Batch.