---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 138a9ef9d483ca0d460bab7185d646669650f83e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "81400022"
---
:::row:::
    :::column span="3":::
        Sada Speech SDK podporuje Windows 10 a Windows Server 2016 nebo novější verze. Starší verze se **oficiálně nepodporují.** Je možné použít části sady Speech SDK s dřívějšími verzemi Windows, i když se to nedoporučuje.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Windows" src="https://docs.microsoft.com/media/logos/logo_Windows.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

### <a name="system-requirements"></a>Požadavky na systém

Sada Speech SDK ve Windows vyžaduje <a href="https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads" target="_blank">Microsoft Visual C++ distribuovatelné pro Visual Studio 2019 <span class="docon docon-navigate-external x-hidden-focus"></span> </a> v systému.

- <a href="https://aka.ms/vs/16/release/vc_redist.x86.exe" target="_blank">Instalace pro x86<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.x64.exe" target="_blank">Instalace pro x64<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.arm64.exe" target="_blank">Nainstalovat pro ARMx64<span class="docon docon-navigate-external x-hidden-focus"></span></a>

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

V případě vstupu přes mikrofon musí být nainstalovány knihovny Media Foundation. Tyto knihovny jsou součástí systémů Windows 10 a Windows Server 2016. Je možné použít sadu Speech SDK bez těchto knihoven, pokud se jako vstupní zvukové zařízení nepoužívá mikrofon.

Požadované soubory sady Speech SDK lze nasadit do stejného adresáře jako vaše aplikace. Tímto způsobem může aplikace získat přímý přístup ke knihovnám. Ujistěte se, že jste vybrali správnou verzi (x86/x64), která odpovídá vaší aplikaci.

| Název                                            | Funkce                                             |
|-------------------------------------------------|------------------------------------------------------|
| `Microsoft.CognitiveServices.Speech.core.dll`   | Základní sada SDK požadovaná pro nativní a spravované nasazení |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Požadováno pro spravované nasazení                      |

> [!NOTE]
> Počínaje verzí 1.3.0 soubor `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (dodán v předchozích verzích) už není potřeba. Tato funkce je teď integrovaná v základní sadě SDK.

> [!IMPORTANT]
> Pro projekt aplikace model Windows Forms App (.NET Framework) se ujistěte, že jsou knihovny zahrnuté v nastavení nasazení vašeho projektu. Můžete to ověřit v části `Properties -> Publish Section`. Klikněte na `Application Files` tlačítko a v rozevíracím seznamu vyhledejte odpovídající knihovny. Ujistěte se, že je hodnota nastavená na `Included`. Visual Studio bude soubor zahrnovat při publikování nebo nasazení projektu.

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
