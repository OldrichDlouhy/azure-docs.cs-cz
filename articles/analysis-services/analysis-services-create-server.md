---
title: Rychlý Start – vytvoření serveru Analysis Services v Azure Portal | Microsoft Docs
description: Naučte se rychle vytvořit instanci Azure Analysis Services serveru pomocí Azure Portal.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 36899aa789f4947fe860621bc1795bfbe2ff1ecf
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697484"
---
# <a name="quickstart-create-a-server---portal"></a>Rychlý start: Vytvoření serveru – portál

Tento rychlý start popisuje, jak pomocí portálu vytvořit prostředek serveru služby Analysis Services v předplatném Azure.

## <a name="prerequisites"></a>Požadavky 

* **Předplatné Azure:** Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatný zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
* **Azure Active Directory:** Vaše předplatné musí být přidružené k tenantovi Azure Active Directory. Zároveň musíte být přihlášeni k Azure pod účtem v této službě Azure Active Directory. Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal 

[Přihlásit se k portálu](https://portal.azure.com)


## <a name="create-a-server"></a>Vytvoření serveru

1. Klikněte na **+ vytvořit Analysis Services pro**  >  **analýzu**prostředků  >  **Analysis Services**.

    ![Portál](./media/analysis-services-create-server/aas-create-server-portal.png)

2. V části **Analysis Services** vyplňte požadovaná pole a pak stiskněte **Vytvořit**.
   
   * **Název serveru:** Zadejte jedinečný název, který slouží jako odkaz na server. Název serveru musí začínat malým písmenem a musí být dlouhý 3 až 128 malých písmen a číslic. Prázdné znaky a speciální znaky nejsou povoleny.
   * **Předplatné:** Vyberte předplatné, ke kterému se tento server přidruží.
   * **Skupina prostředků:** Vytvořte novou skupinu prostředků nebo vyberte jednu z těch, které už máte. Skupiny prostředků jsou navržené tak, aby pomáhaly se správou kolekcí prostředků Azure. Další informace najdete v tématu věnovaném [skupinám prostředků](../azure-resource-manager/management/overview.md).
   * **Umístění:** Toto umístění datacentra Azure je hostitelem serveru. Zvolte umístění co nejblíže vaší největší uživatelské základně.
   * **Cenová úroveň:** Vyberte cenovou úroveň. Pokud provádíte testování a chcete nainstalovat ukázkovou modelovou databázi, vyberte bezplatnou úroveň **D1**. Další informace najdete v tématu [Ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/). 
   * **Správce:** Toto bude ve výchozím nastavení účet, pod kterým jste přihlášeni. Můžete zvolit jiný účet z vaší služby Azure Active Directory.
   * **Nastavení úložiště zálohování:** Volitelné. Pokud už máte [účet úložiště](../storage/common/storage-introduction.md), můžete ho zadat jako výchozí účet úložiště pro zálohování modelové databáze. Později můžete zadat také nastavení [zálohování a obnovení](analysis-services-backup.md).
   * **Vypršení platnosti klíče úložiště:** Volitelné. Zadejte dobu platnosti klíče úložiště.

Vytvoření serveru obvykle trvá necelou minutu. Pokud jste vybrali možnost **Přidat na portál**, přejděte na portál a prohlédněte si nový server. Případně můžete přejít na **všechny služby**  >  **Analysis Services** a zjistit, jestli je váš server připravený. Servery podporují tabelární modely na úrovni kompatibility 1200 a vyšší. Úroveň kompatibility modelu je určena v aplikaci Visual Studio nebo SSMS.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už server nepotřebujete, odstraňte ho. Na stránce **Přehled** vašeho serveru klikněte na **Odstranit**. 

 ![Vyčištění](./media/analysis-services-create-server/aas-create-server-cleanup.png)


## <a name="next-steps"></a>Další kroky
V tomto rychlém startu jste zjistili, jak vytvořit server v předplatném Azure. Když teď máte server, můžete ho zabezpečit nakonfigurováním (volitelné) brány firewall serveru. Na server také můžete přímo z portálu přidat základní ukázkový datový model. Na ukázkovém modelu se naučíte konfigurovat role modelové databáze a testovat připojení klientů. Ve výuce pokračujte kurzem, ve kterém přidáte ukázkový model.

> [!div class="nextstepaction"]
> [Rychlý Start: Konfigurace brány firewall serveru – portál](analysis-services-qs-firewall.md)   
> [!div class="nextstepaction"]
> [Kurz: Přidání ukázkového modelu na server](analysis-services-create-sample-model.md)
