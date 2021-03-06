---
title: 'Rychlý Start: propojení pracovního prostoru Power BI k pracovnímu prostoru synapse'
description: Pomocí kroků v této příručce můžete propojit pracovní prostor Power BI k pracovnímu prostoru Azure synapse Analytics.
services: synapse-analytics
author: jocaplan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/29/2020
ms.author: jocaplan
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: f9ae7b74c17bb330c2c7aa99903c62d4701f0a52
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274182"
---
# <a name="quickstart-linking-a-power-bi-workspace-to-a-synapse-workspace"></a>Rychlý Start: propojení pracovního prostoru Power BI k pracovnímu prostoru synapse

V tomto rychlém startu se dozvíte, jak připojit Power BI pracovní prostor k pracovnímu prostoru analýzy synapse a vytvořit nové sestavy Power BI a datové sady z synapse studia (Preview).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet před tím, než začnete](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Předpoklady

- [Vytvoření pracovního prostoru Azure synapse a přidruženého účtu úložiště](quickstart-create-workspace.md)
- [Pracovní prostor Power BI Professional nebo Premium](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces)

## <a name="link-power-bi-workspace-to-your-synapse-workspace"></a>Propojení pracovního prostoru Power BI k pracovnímu prostoru synapse

1. Od synapse studia klikněte na **Spravovat**.

    ![Synapse Studio klikněte na spravovat.](media/quickstart-link-powerbi/synapse-studio-click-manage.png)

2. V části **externí připojení**klikněte na **propojené služby**.

    ![Propojené služby jsou zvýrazněné.](media/quickstart-link-powerbi/manage-click-linked-services.png)

3. Klikněte na **+ Nový**.

    ![A zvýrazní se nová propojená služba.](media/quickstart-link-powerbi/new-highlighted.png)

4. Klikněte **Power BI** a klikněte na **pokračovat**.

    ![Zobrazuje se Power BI propojená služba.](media/quickstart-link-powerbi/powerbi-linked-service.png)

5. Zadejte název propojené služby a v rozevíracím seznamu vyberte pracovní prostor.

    ![Zobrazují se nastavení propojené služby Power BI.](media/quickstart-link-powerbi/workspace-link-dialog.png)

6. Klikněte na **Create** (Vytvořit).

## <a name="view-power-bi-workspace-in-synapse-studio"></a>Zobrazit Power BI pracovní prostor v synapse studiu

Po propojení pracovních prostorů můžete procházet Power BI datové sady, upravovat nebo vytvářet nové sestavy Power BI z synapse studia.

1. Klikněte na tlačítko **vývoj**.

    ![Synapse Studio klikněte na vývoj.](media/quickstart-link-powerbi/synapse-studio-click-develop.png)

2. Rozbalte Power BI a pracovní prostor, který chcete použít.

    ![Rozbalte položku Power BI a pracovní prostor.](media/quickstart-link-powerbi/develop-expand-powerbi.png)

Nové sestavy lze vytvořit kliknutím v **+** horní části karty **vývoj** . existující sestavy lze upravovat kliknutím na název sestavy. Všechny uložené změny se zapíší zpátky do Power BI pracovního prostoru.

![Zobrazení a úprava sestavy Power BI.](media/quickstart-link-powerbi/powerbi-report.png)


## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [vytváření sestav Power BI u souborů uložených v Azure Storage](sql/tutorial-connect-power-bi-desktop.md).
