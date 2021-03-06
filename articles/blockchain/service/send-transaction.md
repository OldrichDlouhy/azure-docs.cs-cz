---
title: Kurz vytvoření, sestavení, & nasazení inteligentních kontraktů – Azure blockchain Service
description: Kurz týkající se použití rozšíření Azure blockchain Development Kit pro rozšíření Ethereem v Visual Studio Code k vytvoření, sestavení a nasazení inteligentních kontraktů ve službě Azure blockchain Service.
ms.date: 04/22/2020
ms.topic: tutorial
ms.reviewer: caleteet
ms.openlocfilehash: dc23c680dfb2ed33cae2a251af16e1b1f25c6ac7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "82086653"
---
# <a name="tutorial-create-buildanddeploysmartcontracts-on-azure-blockchain-service"></a>Kurz: vytvoření, sestavení a nasazení inteligentních smluv ve službě Azure blockchain

V tomto kurzu pomocí rozšíření Azure blockchain Development Kit pro Ethereem v Visual Studio Code vytvoříte, sestavíte a nasadíte inteligentní smlouvu na Azure blockchain Service. Pomocí vývojové sady také spustíte funkci inteligentního kontraktu prostřednictvím transakce.

Pomocí Azure blockchain Development Kit pro Ethereem:

> [!div class="checklist"]
> * Vytvoření inteligentního kontraktu
> * Nasazení inteligentního kontraktu
> * Provedení funkce inteligentního kontraktu prostřednictvím transakce

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

* Kompletní [rychlé zprovoznění: použití Visual Studio Code pro připojení k síti konsorcia Azure blockchain](connect-vscode.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Azure blockchain Development Kit pro rozšíření Ethereem](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node. js 10.15. x nebo vyšší](https://nodejs.org/download)
* [Git 2.10. x nebo vyšší](https://git-scm.com)
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/) Přidejte Python. exe do cesty. Pro Azure blockchain Development Kit se vyžaduje Python verze 2.7.15 ve vaší cestě.
* [Truffle 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI 6.0.0](https://github.com/trufflesuite/ganache-cli)

Ve Windows se pro modul gyp vyžaduje nainstalovaný kompilátor C++. Můžete použít nástroje MSBuild:

* Pokud je nainstalována aplikace Visual Studio 2017, nakonfigurujte npm pro použití nástrojů MSBuild s příkazem`npm config set msvs_version 2017 -g`
* Pokud je nainstalována sada Visual Studio 2019, nastavte cestu k nástrojům pro sestavení MS pro npm. Například `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`.
* V opačném případě nainstalujte samostatné nástroje VS Build Tools pomocí `npm install --global windows-build-tools` příkazu v příkazovém okně *Spustit jako správce* .

Další informace o Node-gyp najdete v části [úložiště Node-gyp na GitHubu](https://github.com/nodejs/node-gyp).

## <a name="create-a-smart-contract"></a>Vytvoření inteligentního kontraktu

Sada Azure blockchain Development Kit pro Ethereem používá projektové šablony a nástroje Truffle, které vám pomůžou vytvářet kontrakty pro generování a nasazení. Než začnete, dokončete potřebný [rychlý Start: pomocí Visual Studio Code se připojte k síti konsorcia služeb Azure blockchain](connect-vscode.md). Rychlý Start vás provede instalací a konfigurací sady Azure blockchain Development Kit pro Ethereem.

1. Z palety příkazů VS Code vyberte **Azure blockchain: nový projekt soliding**.
1. Vyberte **vytvořit základní projekt**.
1. Vytvořte novou složku s názvem `HelloBlockchain` a **Vyberte možnost Nová cesta k projektu**.

Sada Azure blockchain Development Kit vytvoří a inicializuje nový projekt Solider pro vás. Základní projekt obsahuje vzorový **HelloBlockchain** a všechny potřebné soubory pro sestavování a nasazování členů konsorcia ve službě Azure blockchain. Vytvoření projektu může trvat několik minut. Průběh můžete sledovat na panelu terminálu VS Code, když vyberete výstup pro Azure blockchain.

Struktura projektu vypadá jako v následujícím příkladu:

   ![Projekt solidosti](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Vytvoření inteligentního kontraktu

Inteligentní kontrakty se nacházejí v adresáři **kontraktů** projektu. Inteligentní smlouvy kompilujete před jejich nasazením do blockchain. Použijte příkaz **kontrakty sestavení** pro zkompilování všech inteligentních kontraktů v projektu.

1. Na bočním panelu Průzkumníka VS Code rozbalte složku **smlouvy** ve vašem projektu.
1. Klikněte pravým tlačítkem na **HelloBlockchain. Sol** a v nabídce vyberte **kontrakty sestavení** .

    ![Výběr nabídky smlouvy pro sestavení ](./media/send-transaction/build-contracts.png)

Azure blockchain Development Kit používá Truffle ke kompilaci inteligentních smluv.

![Výstup kompilátoru Truffle](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Nasazení inteligentního kontraktu

Truffle používá skripty pro migraci k nasazení svých smluv do sítě Ethereem. Migrace jsou soubory JavaScriptu, které se nacházejí v adresáři **migrace** projektu.

1. Pokud chcete nasadit vaši inteligentní kontrakt, klikněte pravým tlačítkem na **HelloBlockchain. Sol** a v nabídce vyberte **nasadit smlouvy** .
1. V paletě příkazů vyberte síť Azure blockchain Consortium. Při vytváření projektu byla do konfiguračního souboru Truffle projektu přidána síť blockchain Consortium.
1. Vyberte možnost **generovat klávesové zkratky**. Vyberte název souboru a uložte symbolický soubor do složky projektu. Například, `myblockchainmember.env`. K vygenerování Ethereem privátního klíče pro člena blockchain se použije symbolický soubor.

Azure blockchain Development Kit používá Truffle ke spouštění migračního skriptu k nasazení kontraktů do blockchain.

![Kontrakt se úspěšně nasadil.](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Volání funkce kontraktu

Funkce **SendRequest** kontraktu **HelloBlockchain** změní proměnnou stavu **RequestMessage** . Změna stavu blockchain sítě se provádí prostřednictvím transakce. Stránku interakce inteligentního kontraktu služby Azure blockchain Development Kit můžete použít k volání funkce **SendRequest** prostřednictvím transakce.

1. Pokud chcete s vaší inteligentní smlouvou pracovat, klikněte pravým tlačítkem myši na **HelloBlockchain. Sol** a v nabídce vyberte možnost **Zobrazit stránku interakce inteligentních kontraktů** .

    ![Zvolit zobrazení stránky interakce inteligentních kontraktů z nabídky](./media/send-transaction/contract-interaction.png)

1. Stránka interakce umožňuje zvolit nasazenou verzi kontraktu, funkce volání, zobrazit aktuální stav a zobrazit metadata.

    ![Příklad stránky interakce inteligentního kontraktu](./media/send-transaction/interaction-page.png)

1. Pokud chcete zavolat funkci inteligentního kontraktu, vyberte akci kontraktu a předejte své argumenty. Vyberte akci **SendRequest** kontrakt a zadejte **Hello, blockchain!** pro parametr **RequestMessage** . Vyberte možnost **Spustit** pro volání funkce **SendRequest** prostřednictvím transakce.

    ![Spustit SendRequest akci](./media/send-transaction/sendrequest-action.png)

Po zpracování transakce oddíl interakce odráží změny stavu.

![Změny stavu smlouvy](./media/send-transaction/contract-state.png)

Funkce SendRequest nastaví pole **RequestMessage** a **State** . Aktuální stav pro **RequestMessage** je argumentem, který jste předali **Hello, blockchain**. Hodnota pole **stav** zůstane **požadavek**.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete prostředky odstranit tak, že `myResourceGroup` odstraníte skupinu prostředků, kterou jste vytvořili v rychlém startu průvodce *vytvořením blockchain člena* .

Odstranění skupiny prostředků:

1. V Azure Portal přejděte do **skupiny prostředků** v levém navigačním podokně a vyberte skupinu prostředků, kterou chcete odstranit.
1. Vyberte **Odstranit skupinu prostředků**. Potvrďte odstranění zadáním názvu skupiny prostředků a vyberte **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili ukázkový projekt s ukázkou pomocí Azure blockchain Development Kit. Vystavíte a nasadili jste inteligentní kontrakt a pak jste volali funkci prostřednictvím transakce v síti konsorcia blockchain, která je hostovaná ve službě Azure blockchain.

> [!div class="nextstepaction"]
> [Vývoj aplikací blockchain pomocí služby Azure blockchain](develop.md)
