---
title: Vytvoření centra oznámení Azure pomocí Azure Portal | Microsoft Docs
description: V tomto kurzu se naučíte vytvořit centrum oznámení Azure pomocí Azure Portal.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 3aeeb989d15dc74849c85fa58cbefa891809f3c5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "80347089"
---
# <a name="quickstart-create-an-azure-notification-hub-in-the-azure-portal"></a>Rychlý Start: vytvoření centra oznámení Azure v Azure Portal 
Služba Azure Notification Hubs nabízí snadno použitelné, škálovatelné zasílací jádro, které umožňuje posílat oznámení libovolné platformě (iOS, Android, Windows, Kindle, Baidu atd.) z libovolného back-endu (cloudový nebo místní). Další informace o této službě najdete v tématu [co je Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

V tomto rychlém startu vytvoříte centrum oznámení v Azure Portal. První část obsahuje kroky pro vytvoření oboru názvů Notification Hubs a centra v daném oboru názvů. Druhá část obsahuje postup vytvoření centra oznámení v existujícím oboru názvů Notification Hubs. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Vytvoření oboru názvů a centra oznámení
V této části vytvoříte obor názvů a centrum v oboru názvů. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Vytvoření centra oznámení v existujícím oboru názvů
V této části vytvoříte centrum oznámení v existujícím oboru názvů. 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo vyberte **všechny služby** , vyhledejte **Centrum oznámení**, vyberte **hvězdičku** (`*`) vedle položky **obory názvů centra oznámení** a přidejte je do části **Oblíbené** v nabídce vlevo. Vyberte **obory názvů centra oznámení**. 

      ![Azure Portal – výběr oborů názvů centra oznámení](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Na stránce **obory názvů centra oznámení** vyberte ze seznamu svůj obor názvů. 

      ![Ze seznamu vyberte svůj obor názvů.](./media/create-notification-hub-portal/select-namespace.png)
1. Na stránce **obor názvů centra oznámení** vyberte **Přidat centrum** na panelu nástrojů. 

      ![Obory názvů centra oznámení – tlačítko Přidat centrum](./media/create-notification-hub-portal/add-hub-button.png)
4. Na stránce **nové centrum oznámení** zadejte název centra oznámení a vyberte **OK**.

      ![Nová stránka centra oznámení – > zadejte název vašeho centra.](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. V horní části vyberte **oznámení** (ikona zvonku) a zobrazte stav nasazení nového centra. Výběrem **X** v pravém horním rohu zavřete okno oznámení. 

      ![Oznámení o nasazení](./media/create-notification-hub-portal/deployment-notification.png)
5. Aktualizujte webovou stránku **obory názvů centra oznámení** , abyste viděli nové centrum v seznamu. 

      ![Portál Azure Portal – oznámení -> Přejít k prostředku](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Vyberte **Centrum oznámení** , kde se zobrazí domovská stránka centra oznámení. 

      ![Portál Azure Portal – oznámení -> Přejít k prostředku](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste vytvořili centrum oznámení. Informace o tom, jak nakonfigurovat nastavení centra pro PNS (Platform Notification System), najdete v tématu [Konfigurace centra oznámení s nastavením PNS](configure-notification-hub-portal-pns-settings.md). 