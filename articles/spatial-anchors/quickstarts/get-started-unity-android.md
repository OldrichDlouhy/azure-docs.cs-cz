---
title: 'Rychlý Start: Vytvoření aplikace Unity pro Android'
description: V tomto rychlém startu se dozvíte, jak vytvořit aplikaci pro Android s využitím prostorových ukotvení.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7acff7f0249cdedcebd367fc315be92cafb9ab78
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77615441"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Rychlý Start: Vytvoření aplikace Unity pro Android pomocí prostorových kotev Azure

Tento rychlý Start popisuje, jak vytvořit aplikaci Unity pro Android pomocí [prostorových kotev Azure](../overview.md). Prostorové kotvy Azure je služba pro vývojáře napříč platformami, která umožňuje vytvářet hybridní prostředí realit pomocí objektů, které v průběhu času trvale uchovávají jejich umístění v rámci zařízení. Až budete hotovi, budete mít ARCore aplikaci pro Android vytvořenou v Unity, která může ukládat a odvolat prostorovou kotvu.

Dozvíte se, jak provést tyto akce:

> [!div class="checklist"]
> * Vytvoření účtu prostorových kotev
> * Příprava nastavení sestavení Unity
> * Konfigurace identifikátoru účtu prostorových kotev a klíče účtu
> * Exportovat Android Studio projekt
> * Nasazení a spuštění na zařízení s Androidem

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý start, ujistěte se, že máte následující:

- Počítač s Windows nebo macOS s <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019,1 nebo 2019,2</a> včetně modulů podpory sestavení pro Android a Android SDK & NDK nástrojů.
  - Pokud používáte systém Windows, budete také potřebovat <a href="https://git-scm.com/download/win" target="_blank">Git pro Windows</a> a <a href="https://git-lfs.github.com/">Git LFS</a>.
  - Pokud používáte macOS, načtěte Git prostřednictvím HomeBrew. Do jednoho řádku terminálu zadejte následující příkaz: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Pak spusťte `brew install git` příkaz a `brew install git-lfs`.
- Zařízení s Androidem podporující <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">vývojáře</a> a <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore</a> .
  - Počítač může pro komunikaci se zařízením s Androidem vyžadovat další ovladače zařízení. Další informace a pokyny najdete [tady](https://developer.android.com/studio/run/device.html) .

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Stáhněte a otevřete vzorový projekt Unity.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Konfigurace identifikátoru a klíče účtu

V podokně **projekt** přejděte na `Assets/AzureSpatialAnchors.Examples/Scenes` a otevřete soubor `AzureSpatialAnchorsBasicDemo.unity` scény.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Scénu uložte výběrem Uložit **soubor** -> **Save**.

## <a name="export-the-android-studio-project"></a>Exportovat Android Studio projekt

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

V nabídce **Spustit zařízení** vyberte zařízení a klikněte na **Sestavit a spustit**. Zobrazí se výzva k uložení `.apk` souboru, pro který můžete vybrat libovolný název.

Podle pokynů v aplikaci založte a odvoláte kotvu.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="rendering-issues"></a>Problémy vykreslování

Pokud při spuštění aplikace nevidíte kameru jako pozadí (například místo toho, aby se zobrazily prázdné, modré nebo jiné textury), pravděpodobně budete muset znovu naimportovat prostředky v Unity. Zastavte aplikaci. V horní nabídce v Unity vyberte **assets – > znovu importovat vše**. Pak znovu spusťte aplikaci.

### <a name="unity-20193"></a>Unity 2019,3

V důsledku zásadních změn se Unity 2019,3 aktuálně nepodporuje. Použijte prosím Unity 2019,1 nebo 2019,2.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Kurz: sdílení prostorových ukotvení napříč zařízeními](../tutorials/tutorial-share-anchors-across-devices.md)
