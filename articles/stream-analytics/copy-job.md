---
title: Kopírování nebo zálohování úloh Azure Stream Analytics
description: Tento článek popisuje, jak zkopírovat nebo zálohovat úlohu Azure Stream Analytics.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/11/2019
ms.openlocfilehash: 2c6b6af46ae89f794e05c3aa80716250c566257e
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037218"
---
# <a name="copy-or-back-up-azure-stream-analytics-jobs"></a>Kopírování nebo zálohování úloh Azure Stream Analytics

Nasazené Azure Stream Analytics úlohy můžete kopírovat nebo zálohovat pomocí Visual Studio Code nebo sady Visual Studio. Zkopírování úlohy do jiné oblasti nekopíruje poslední výstupní čas. Proto nemůžete použít, pokud se při spuštění zkopírované úlohy používá možnost [**naposledy zastaveno**](https://docs.microsoft.com/azure/stream-analytics/start-job#start-options) .

## <a name="before-you-begin"></a>Než začnete
* Pokud nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/).

* Přihlaste se k [portálu Azure Portal](https://portal.azure.com/).

* Nainstalujte [Azure Stream Analytics rozšíření pro Visual Studio Code](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension) nebo [Azure Stream Analytics nástroje pro Visual Studio](quick-create-vs-code.md#install-the-azure-stream-analytics-tools-extension).  

## <a name="visual-studio-code"></a>Visual Studio Code

1. Klikněte na ikonu **Azure** na řádku Visual Studio Code aktivity a potom rozbalte uzel **Stream Analytics** . Vaše úlohy by se měly zobrazit v rámci vašich předplatných.

   ![Otevřít Stream Analytics Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Chcete-li exportovat úlohu do místního projektu, vyhledejte úlohu, kterou chcete exportovat, v **průzkumníkovi Stream Analytics** v Visual Studio Code. Pak vyberte složku pro svůj projekt.

    ![Exportovat úlohu ASA v Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

    Projekt se vyexportuje do vybrané složky a přidá se do vašeho aktuálního pracovního prostoru.

    ![Exportovat úlohu ASA v Visual Studio Code](./media/stream-analytics-manage-job/copy-backup-stream-analytics-jobs.png)

3. Pokud chcete úlohu publikovat do jiné oblasti nebo zálohy s použitím jiného názvu, vyberte v editoru dotazů (. asaql) **možnost vybrat ze svých předplatných** \* a postupujte podle pokynů.

    ![Publikování do Azure v Visual Studio Code](./media/quick-create-vs-code/submit-job.png)

## <a name="visual-studio"></a>Visual Studio

1. Postupujte podle [pokynů k projektu export Azure Stream Analytics nasazené úlohy](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-vs-tools#export-jobs-to-a-project).

2. Otevřete \* soubor. asaql v editoru dotazů, v editoru skriptů vyberte **Odeslat do Azure** a postupujte podle pokynů pro publikování úlohy do jiné oblasti nebo zálohy pomocí nového názvu.

## <a name="next-steps"></a>Další kroky

* [Rychlý Start: vytvoření úlohy Stream Analytics pomocí Visual Studio Code](quick-create-vs-code.md)
* [Rychlý Start: vytvoření úlohy Stream Analytics pomocí sady Visual Studio](stream-analytics-quick-create-vs.md)
* [Nasazení úlohy Azure Stream Analytics s CI/CD pomocí Azure Pipelines](stream-analytics-tools-visual-studio-cicd-vsts.md)
