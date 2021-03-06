---
title: 'Rychlý Start: Vytvoření aplikace pro Xamarin Android'
description: V tomto rychlém startu se dozvíte, jak vytvořit aplikaci pro Android pomocí prostorových ukotvení.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 8c27a50f823dcda74d735e6314c9004ec36769d4
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2020
ms.locfileid: "83870889"
---
# <a name="quickstart-create-a-xamarin-android-app-with-azure-spatial-anchors"></a>Rychlý Start: Vytvoření aplikace pro Xamarin Android pomocí prostorových kotev Azure

Tento rychlý Start popisuje, jak vytvořit aplikaci pro Android s použitím [prostorových kotev Azure](../overview.md). Prostorové kotvy Azure je služba pro vývojáře napříč platformami, která umožňuje vytvářet hybridní prostředí realit pomocí objektů, které v průběhu času trvale uchovávají jejich umístění v rámci zařízení. Až budete hotovi, budete mít aplikaci pro Android, která může uložit a odvolat prostorovou kotvu.

Dozvíte se, jak provést tyto akce:

> [!div class="checklist"]
> * Vytvoření účtu prostorových kotev
> * Konfigurace identifikátoru účtu prostorových kotev a klíče účtu
> * Nasazení a spuštění na zařízení s Androidem

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý start, ujistěte se, že máte následující:
- Počítač s Windows nebo macOS:
  - Pokud používáte Windows:
    - Aktuální verze sady <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019 16.2 +</a>.
    - <a href="https://git-scm.com/download/win" target="_blank">Git pro Windows</a>.
    - <a href="https://git-lfs.github.com/">Git LFS</a>.
  - Pokud používáte macOS:
    - Aktuální verze <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio pro Mac 8.1 +</a>.
    - <a href="https://git-scm.com/download/mac" target="_blank">Git pro MacOS</a>.
    - <a href="https://git-lfs.github.com/">Git LFS</a>.
- Na vámi zvolenou platformu je nainstalovaná a spuštěná nejnovější verze Xamarin. Androidu. Průvodce instalací Xamarin. Android najdete v příručkách k [instalaci Xamarin. Android](https://docs.microsoft.com/xamarin/android/get-started/installation/index) .
- Zařízení s Androidem podporující <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">vývojáře</a> a <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore</a> .
  - Počítač může pro komunikaci se zařízením s Androidem vyžadovat další ovladače zařízení. Další informace najdete [tady](https://developer.android.com/studio/run/device.html).
- Vaše aplikace musí cílit na ARCore **1,8**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Otevřete vzorový projekt

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Otevřete `Xamarin/SampleXamarin.sln` v aplikaci Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Konfigurace identifikátoru a klíče účtu

Dalším krokem je konfigurace aplikace tak, aby používala identifikátor účtu a klíč účtu. Při [nastavování prostředku prostorových ukotvení](#create-a-spatial-anchors-resource)jste je zkopírovali do textového editoru.

Otevřete `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Vyhledejte `SpatialAnchorsAccountKey` pole a nahraďte ho `Set me` klíčem účtu.

Vyhledejte `SpatialAnchorsAccountId` pole a nahraďte ho `Set me` identifikátorem účtu.

Otevřete `Xamarin/SampleXamarin.Android/AzureSpatialAnchorsManager.cs`.

Vyhledejte `AzureSpatialAnchorsManager(Session arCoreSession)` a přidejte následující řádek, ve kterém nahradíte doménu účtu v minulosti: `this.spatialAnchorsSession.Configuration.AccountDomain = "MyAccountDomain";` .

## <a name="deploy-the-app-to-your-android-device"></a>Nasazení aplikace na zařízení s Androidem

Zapněte zařízení se systémem Android, přihlaste se a připojte ho k počítači pomocí kabelu USB.

Nastavte projekt po spuštění na **SampleXamarin. Android**, změňte **konfiguraci řešení** na **release**a v rozevíracím seznamu pro výběr zařízení vyberte zařízení, do kterého chcete nasadit.

# <a name="windows"></a>[Windows](#tab/deploy-windows)

![Konfigurace sady Visual Studio](./media/get-started-xamarin-android/visual-studio-windows-configuration.png)

Vyberte **ladit**  >  **Spustit ladění** a nasaďte a spusťte svoji aplikaci.

# <a name="macos"></a>[macOS](#tab/deploy-macos)

![Konfigurace sady Visual Studio](./media/get-started-xamarin-android/visual-studio-macos-configuration.jpg)

Pokud **Run**  >  chcete nasadit a spustit aplikaci, vyberte spustit**Spustit bez ladění** .

---

V aplikaci vyberte **základní** , aby se spustila ukázka, a postupujte podle pokynů k umístění a odvolání kotvy.

> ![Snímek obrazovky 1 ](./media/get-started-xamarin-android/screenshot-1.jpg)
>  ![ snímek obrazovky 2 snímek ](./media/get-started-xamarin-android/screenshot-2.jpg)
>  ![ obrazovky 3](./media/get-started-xamarin-android/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Kurz: sdílení prostorových ukotvení napříč zařízeními](../tutorials/tutorial-share-anchors-across-devices.md)
