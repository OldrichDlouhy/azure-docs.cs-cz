---
title: zahrnout soubor
description: zahrnout soubor
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/20/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ace42278269ff6af31902dbecead81329815af12
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "67175243"
---
## <a name="create-a-topic-using-the-azure-portal"></a>Vytvoření tématu pomocí webu Azure Portal
1. Na stránce **Service Bus obor názvů** vyberte v nabídce vlevo možnost **témata** .
2. Na panelu nástrojů vyberte **+ téma** . 
4. Zadejte **název** tématu. U ostatních možností ponechte jejich výchozí hodnoty.
5. Vyberte **Vytvořit**.

    ![Vytvořit téma](./media/service-bus-create-topics-subscriptions-portal/create-topic.png)

## <a name="create-subscriptions-to-the-topic"></a>Vytvoření předplatných pro téma
1. Vyberte **téma** , které jste vytvořili v předchozí části. 
    
    ![Vybrat téma](./media/service-bus-create-topics-subscriptions-portal/select-topic.png)
2. Na stránce **Service Bus téma** vyberte v nabídce vlevo možnost **odběry** a pak na panelu nástrojů vyberte **+ předplatné** . 
    
    ![Tlačítko Přidat předplatné](./media/service-bus-create-topics-subscriptions-portal/add-subscription-button.png)
3. Na stránce **vytvořit odběr** zadejte **S1** pro **název** předplatného a pak vyberte **vytvořit**. 

    ![Vytvořit stránku předplatného](./media/service-bus-create-topics-subscriptions-portal/create-subscription-page.png)
4. Opakujte předchozí krok dvakrát, abyste vytvořili odběry s názvem **S2** a **S3**.