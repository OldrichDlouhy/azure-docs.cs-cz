---
title: 'Rychlý Start: Vytvoření aplikace pro Xamarin iOS'
description: V tomto rychlém startu se dozvíte, jak vytvořit aplikaci pro iOS pomocí prostorových ukotvení.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 625a1a256ec5f1668c97e74c37e567a4ceb3612f
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2020
ms.locfileid: "83871157"
---
# <a name="quickstart-create-a-xamarin-ios-app-with-azure-spatial-anchors"></a>Rychlý Start: Vytvoření aplikace Xamarin iOS pomocí prostorových kotev Azure

V tomto rychlém startu se dozvíte, jak vytvořit aplikaci pro iOS pomocí [prostorových kotev Azure](../overview.md). Prostorové kotvy Azure je služba pro vývojáře napříč platformami, která umožňuje vytvářet hybridní prostředí realit pomocí objektů, které v průběhu času trvale uchovávají jejich umístění v rámci zařízení. Až budete hotovi, budete mít aplikaci pro iOS, která může uložit a odvolat prostorovou kotvu.

Dozvíte se, jak provést tyto akce:

> [!div class="checklist"]
> * Vytvoření účtu prostorových kotev
> * Konfigurace identifikátoru účtu prostorových kotev a klíče účtu
> * Nasazení a spuštění na zařízení s iOS

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý start, ujistěte se, že máte následující:
- Mac s macOS vysokým Sierra (10,13) nebo novějším:
  - Nejnovější verze Xcode a sady iOS SDK nainstalované z [App Storu](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  - Aktuální verze <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio pro Mac 8.1 +</a>.
  - <a href="https://git-scm.com/download/mac" target="_blank">Git pro MacOS</a>.
  - <a href="https://git-lfs.github.com/">Git LFS</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Otevřete vzorový projekt

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Otevřete `Xamarin/SampleXamarin.sln` v aplikaci Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Konfigurace identifikátoru a klíče účtu

Dalším krokem je konfigurace aplikace tak, aby používala identifikátor účtu a klíč účtu. Při [nastavování prostředku prostorových ukotvení](#create-a-spatial-anchors-resource)jste je zkopírovali do textového editoru.

Otevřete `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Vyhledejte `SpatialAnchorsAccountKey` pole a nahraďte ho `Set me` klíčem účtu.

Vyhledejte `SpatialAnchorsAccountId` pole a nahraďte ho `Set me` identifikátorem účtu.

Otevírají`Xamarin/SampleXamarin.iOS/ViewControllers/DemoViewControllerBase.cs`

Vyhledejte `StartSession()` a přidejte následující řádek, ve kterém nahradíte doménu účtu v minulosti: `this.cloudSession.Configuration.AccountDomain = "MyAccountDomain";` .

## <a name="deploy-the-app-to-your-ios-device"></a>Nasazení aplikace do zařízení s iOS

Zapněte zařízení s iOS, přihlaste se a připojte ho k počítači pomocí kabelu USB.

Nastavte projekt po spuštění na **SampleXamarin. iOS**, změňte **konfiguraci řešení** na **release**a v rozevíracím seznamu pro výběr zařízení vyberte zařízení, do kterého chcete nasadit.

![Konfigurace sady Visual Studio](./media/get-started-xamarin-iOS/visual-studio-macos-configuration.jpg)

Pokud **Run**  >  chcete nasadit a spustit aplikaci, vyberte spustit**Spustit bez ladění** .

V aplikaci vyberte **základní** , aby se spustila ukázka, a postupujte podle pokynů k umístění a odvolání kotvy.

> ![Snímek obrazovky 1 ](./media/get-started-xamarin-ios/screenshot-1.jpg)
>  ![ snímek obrazovky 2 snímek ](./media/get-started-xamarin-ios/screenshot-2.jpg)
>  ![ obrazovky 3](./media/get-started-xamarin-ios/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Kurz: sdílení prostorových ukotvení napříč zařízeními](../tutorials/tutorial-share-anchors-across-devices.md)
