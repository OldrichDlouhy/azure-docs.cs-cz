---
title: 'Kurz: hlasy umožňují robotům používat sadu Speech SDK – Speech Service'
titleSuffix: Azure Cognitive Services
description: V tomto kurzu vytvoříte robota s odezvou s použitím Microsoft bot-Frameworku, nasadíte ho do Azure a zaregistrujete ho pomocí kanálu s přímým odkazem na rozhraní robota. Potom nakonfigurujete ukázkovou klientskou aplikaci pro Windows, která vám umožní mluvit na robota a slyšet na vás odpověď.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: trbye
ms.openlocfilehash: a96ddfe2023fbddd6a4a25c97001875e0dddc7f3
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/12/2020
ms.locfileid: "84753189"
---
# <a name="tutorial-voice-enable-your-bot-using-the-speech-sdk"></a>Kurz: hlas – povolení robota pomocí sady Speech SDK

Nyní můžete použít sílu služby řeči ke snadnému hlasu – povolte robota chatu.

V tomto kurzu vytvoříte robota s odezvou s použitím Microsoft bot-Frameworku, nasadíte ho do Azure a zaregistrujete ho pomocí kanálu s přímým odkazem na rozhraní robota. Potom nakonfigurujete ukázkovou klientskou aplikaci pro Windows, která vám umožní mluvit na robota a slyšet na vás odpověď.

Tento kurz je určený pro vývojáře, kteří právě spouštějí svou cestu pomocí Azure, robot-Framework roboty, Direct line Speech nebo sady Speech SDK a chcete rychle vytvořit funkční systém s omezeným kódováním. Tyto služby nevyžadují žádné zkušenosti ani obeznámení s nimi.

Na konci tohoto cvičení jste nastavili systém, který bude fungovat následujícím způsobem:

1. Ukázková klientská aplikace je nakonfigurovaná tak, aby se připojovala k přímému line kanálu a robotovi s odezvou
1. Zvuk se zaznamenává z výchozího mikrofonu při stisknutí tlačítka (nebo nepřetržitě zaznamenaný, pokud je aktivované vlastní klíčové slovo).
1. Volitelně se detekce vlastního klíčového slova stane, když se do cloudu zavede audio streamování.
1. Pomocí sady Speech SDK se aplikace připojuje k přímému line kanálu řeči a zvuk streamů.
1. Volitelně se u služby stane ověření klíčového slova s vyšší přesností.
1. Zvuk se předává službě rozpoznávání řeči a přepisu se na text.
1. Rozpoznaný text se předává funkci echo-bot jako aktivity bot Frameworku. 
1. Text odpovědi je převedený na zvuk ve službě převod textu na mluvené slovo (TTS) a streamuje se zpět do klientské aplikace pro přehrávání.

![Diagram – značka](media/tutorial-voice-enable-your-bot-speech-sdk/diagram.png "Tok kanálu řeči")

> [!NOTE]
> Kroky v tomto kurzu nevyžadují placenou službu. Jako nový uživatel Azure budete moct k dokončení tohoto kurzu použít kredity ze bezplatného zkušebního předplatného Azure a bezplatné úrovně služby Speech.

V tomto kurzu se dozvíte, co tento kurz popisuje:
> [!div class="checklist"]
> * Vytvoření nových prostředků Azure
> * Sestavování, testování a nasazování vzorku echo bot na Azure App Service
> * Zaregistrujte si robot s přímým line Speech Channel.
> * Sestavte a spusťte klienta Windows Voice Assistant pro interakci s vaším robotem ozvěny.
> * Přidat vlastní aktivaci klíčového slova
> * Naučte se změnit jazyk rozpoznaného a mluveného řeči.

## <a name="prerequisites"></a>Požadavky

Tady je postup, který budete potřebovat k dokončení tohoto kurzu:

- POČÍTAČ s Windows 10 s pracovním mikrofonem a reproduktory (nebo sluchátka)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) nebo vyšší
- [.NET Framework runtime 4.6.1](https://dotnet.microsoft.com/download) nebo vyšší
- Účet Azure. [Zaregistrujte se zdarma](https://azure.microsoft.com/free/ai/).
- Účet [GitHub](https://github.com/)
- [Git pro Windows](https://git-scm.com/download/win)

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Klientská aplikace, kterou vytvoříte v tomto kurzu, používá několik služeb Azure. Aby se snížila doba odezvy pro odpovědi z bot, měli byste se ujistit, že tyto služby jsou umístěné ve stejné oblasti Azure. V této části vytvoříte skupinu prostředků v oblasti **západní USA** . Tato skupina prostředků se použije při vytváření individuálních prostředků pro robot-Framework, kanál přímého zpracování řeči a službu pro rozpoznávání řeči.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.ResourceGroup" target="_blank">Vytvoření skupiny prostředků<span class="docon docon-navigate-external x-hidden-focus"></span></a>
1. Zobrazí se výzva k zadání některých informací:
   * Nastavte **předplatné** na **bezplatnou zkušební verzi** (můžete také použít stávající předplatné).
   * Zadejte název vaší **skupiny prostředků**. Doporučujeme **SpeechEchoBotTutorial-Resource**.
   * V rozevíracím seznamu **oblast** vyberte možnost **západní USA**.
1. Klikněte na **Zkontrolovat a vytvořit**. Měla by se zobrazit informační zpráva s **potvrzením**, že čtení bylo úspěšné.
1. Klikněte na **Vytvořit**. Vytvoření skupiny prostředků může trvat několik minut.
1. Stejně jako u prostředků, které vytvoříte později v tomto kurzu, je vhodné Připnout tuto skupinu prostředků na řídicí panel a získat tak snadný přístup. Pokud chcete tuto skupinu prostředků připnout, klikněte na ikonu připnutí v pravém horním rohu řídicího panelu.

### <a name="choosing-an-azure-region"></a>Výběr oblasti Azure

Pokud chcete pro tento kurz použít jinou oblast, můžou tyto faktory omezit vaše možnosti:

* Ujistěte se, že používáte [podporovanou oblast Azure](regions.md#voice-assistants).
* Kanál Direct line Speech používá službu pro převod textu na mluvené slovo, která má standardní a neuronové hlasy. Hlasy neuronové jsou [omezené na konkrétní oblasti Azure](regions.md#standard-and-neural-voices).
* Bezplatné zkušební klíče můžou být omezené na konkrétní oblast.

Další informace o oblastech najdete v tématu věnovaném [umístěním Azure](https://azure.microsoft.com/global-infrastructure/locations/).

## <a name="create-resources"></a>Vytvoření prostředků

Teď, když máte skupinu prostředků v podporované oblasti, je dalším krokem vytvoření jednotlivých prostředků pro každou službu, kterou použijete v tomto kurzu.

### <a name="create-a-speech-service-resource"></a>Vytvoření prostředku služby Speech

Pomocí těchto pokynů vytvořte prostředek řeči:

1. <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Vytvoření prostředku služby Speech<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Zobrazí se výzva k zadání některých informací:
   * Zadejte **název**prostředku. Doporučujeme **SpeechEchoBotTutorial** řeč.
   * U **předplatného**se ujistěte, že je vybraná možnost **bezplatná zkušební verze** .
   * V **oblasti umístění**vyberte **západní USA**.
   * V případě **cenové úrovně**vyberte **F0**. Toto je úroveň Free.
   * V případě **skupiny prostředků**vyberte **SpeechEchoBotTutorial-Resource**.
5. Po zadání všech požadovaných informací klikněte na **vytvořit**. Vytvoření prostředku může trvat několik minut.
6. Později v tomto kurzu budete pro tuto službu potřebovat klíče předplatného. K těmto klíčům můžete kdykoli přistupovat z **přehledu** svého prostředku (Správa klíčů) nebo **klíčů**.

V tomto okamžiku ověřte, že skupina prostředků (**SpeechEchoBotTutorial-resourceName**) má prostředek řeči:

| Name | Typ  | Umístění |
|------|-------|----------|
| SpeechEchoBotTutorial – řeč | Cognitive Services | USA – západ |

### <a name="create-an-azure-app-service-plan"></a>Vytvoření plánu služby Azure App Service

Dalším krokem je vytvoření plánu App Service. Plán služby App Service definuje sadu výpočetních prostředků pro provozování webové aplikace.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.AppServicePlanCreate" target="_blank">Vytvoření plánu Azure App Service<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Zobrazí se výzva k zadání některých informací:
   * Nastavte **předplatné** na **bezplatnou zkušební verzi** (můžete také použít stávající předplatné).
   * V případě **skupiny prostředků**vyberte **SpeechEchoBotTutorial-Resource**.
   * Zadejte **název**prostředku. Doporučujeme **SpeechEchoBotTutorial-AppServicePlan** .
   * V případě **operačního systému**vyberte možnost **Windows**.
   * V **oblasti oblast**vyberte **západní USA**.
   * V případě **cenové úrovně**se ujistěte, že je vybraná **úroveň Standard S1** . Mělo by se jednat o výchozí hodnotu. Pokud ne, ujistěte se, že jste nastavili **operační systém** na **Windows** , jak je popsáno výše.
5. Klikněte na **Zkontrolovat a vytvořit**. Měla by se zobrazit informační zpráva s **potvrzením**, že čtení bylo úspěšné.
6. Klikněte na **Vytvořit**. Vytvoření skupiny prostředků může trvat několik minut.

V tomto okamžiku ověřte, že vaše skupina prostředků (**SpeechEchoBotTutorial-resourceName**) má dva prostředky:

| Name | Typ  | Umístění |
|------|-------|----------|
| SpeechEchoBotTutorial-AppServicePlan | Plán služby App Service | USA – západ |
| SpeechEchoBotTutorial – řeč | Cognitive Services | USA – západ |

## <a name="build-an-echo-bot"></a>Sestavení robota s odezvou

Teď, když jste vytvořili nějaké prostředky, pojďme vytvořit robota. Budeme začít s ukázkou pro echo bot, který jako název napovídá, vrátí text, který jste zadali jako odpověď. Nedělejte si starosti, vzorový kód je připravený k použití bez jakýchkoli změn. Je nakonfigurovaná tak, aby fungovala s přímým kanálem pro rozpoznávání řeči, ke kterému se připojíme po nasazení robota do Azure.

> [!NOTE]
> Následující pokyny a další informace o službě echo bot jsou k dispozici v [souboru Readme ukázky na GitHubu](https://github.com/microsoft/BotBuilder-Samples/blob/master/samples/csharp_dotnetcore/02.echo-bot/README.md).

### <a name="run-the-bot-sample-on-your-machine"></a>Spuštění ukázky robota na počítači

1. Naklonujte úložiště ukázek:

   ```bash
   git clone https://github.com/Microsoft/botbuilder-samples.git
   ```

2. Spusťte Visual Studio.
3. Na panelu nástrojů vyberte **soubor**  >  **otevřít**  >  **projekt/řešení**a otevřete řešení pro odezvu na aplikaci echo bot:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

4. Po načtení projektu stiskněte klávesu <kbd>F5</kbd> a sestavte a spusťte projekt.
5. Prohlížeč by se měl spustit a zobrazí se obrazovka podobná této.
    > [!div class="mx-imgBorder"]
    > [![echobot – běžící – v místním počítači](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png "EchoBot běžící na místním hostiteli")](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png#lightbox)

### <a name="test-the-bot-sample-with-the-bot-framework-emulator"></a>Test ukázky robota pomocí emulátoru bot Framework

[Emulátor systému robot Framework](https://github.com/microsoft/botframework-emulator) je desktopová aplikace, která umožňuje vývojářům robotům testovat a ladit roboty místně nebo vzdáleně prostřednictvím tunelu. Emulátor podporuje zadaný text jako vstup (ne hlas). Robot bude reagovat na text. Postupujte podle těchto kroků a pomocí emulátoru robot Framework otestujte vaši aplikaci echo robot spuštěnou lokálně s textovým vstupem a textovým výstupem. Po nasazení robota do Azure ho budeme testovat pomocí hlasového vstupu a hlasového výstupu.

1. Nainstalujte [emulátor systému bot Framework](https://github.com/Microsoft/BotFramework-Emulator/releases/latest) verze 4.3.0 nebo novější.
2. Spusťte emulátor rozhraní bot Framework a otevřete robota:
   * **Soubor**  ->  **Otevřete robot**.
3. Zadejte adresu URL pro robota. Například:

   ```
   http://localhost:3978/api/messages
   ```
   a stiskněte klávesu "připojit".
4. Robot by vás měl okamžitě popozdravit "Hello a Welcome!" zpráva. Zadejte jakoukoli textovou zprávu a potvrďte, že od robota dostanete odpověď.
5. To znamená, že výměna komunikace s instancí robota s odezvou může vypadat jako: [ ![robot-Framework-emulátor](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png "Emulátor pro robot Framework")](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png#lightbox)

## <a name="deploy-your-bot-to-an-azure-app-service"></a>Nasazení robota na Azure App Service

Dalším krokem je nasazení nástroje echo bot do Azure. Existuje několik způsobů, jak nasadit robot, ale v tomto kurzu se zaměříme na publikování přímo ze sady Visual Studio.

> [!NOTE]
> Alternativně můžete nasadit robota pomocí [Azure CLI](https://docs.microsoft.com/azure/bot-service/bot-builder-deploy-az-cli) a [šablon nasazení](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/adaptive-dialog/03.core-bot).

1. V aplikaci Visual Studio otevřete robota s odezvou, která je nakonfigurovaná pro použití s přímým linkovým kanálem řeči:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

1. V **Průzkumník řešení**klikněte pravým tlačítkem na projekt **EchoBot** a vyberte **publikovat...**
1. Otevře se nové okno s názvem **Vyberte cíl publikování** .
1. Z navigace **služeb Azure** vyberte **App Service** , vyberte **vytvořit novou**a potom klikněte na **vytvořit profil**.
1. Po zobrazení okna **vytvořit App Service** :
   * Klikněte na **Přidat účet**a přihlaste se pomocí svých přihlašovacích údajů k účtu Azure. Pokud jste už přihlášení, v rozevíracím seznamu vyberte požadovaný účet.
   * Pro **název aplikace**budete muset pro robot zadat globálně jedinečný název. Tento název se používá k vytvoření jedinečné adresy URL robota. Bude naplněna výchozí hodnota včetně data a času (například: "EchoBot20190805125647"). Pro tento kurz můžete použít výchozí název.
   * U **předplatného**nastavte na **bezplatnou zkušební verzi** .
   * V případě **skupiny prostředků**vyberte **SpeechEchoBotTutorial-Resource** .
   * V případě **plánu hostování**vyberte **SpeechEchoBotTutorial-AppServicePlan**
   * Pro **Application Insights**ponechat jako **žádné**
1. Klikněte na **vytvořit** .
1. Klikněte na **publikovat** na pravé straně nově vytvořeného profilu.
1. V aplikaci Visual Studio by se měla zobrazit zpráva o úspěchu, která vypadá takto:

   ```
   Publish Succeeded.
   Web App was published successfully https://EchoBot20190805125647.azurewebsites.net/
   ```

1. Výchozí prohlížeč by měl otevřít a zobrazit stránku, která čte: "vaše robot je připravený!".
1. V tomto okamžiku zkontrolujte skupinu prostředků **SpeechEchoBotTutorial-source** ve Azure Portal a potvrďte, že existují tři prostředky:

| Name | Typ  | Umístění |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA – západ |
| SpeechEchoBotTutorial-AppServicePlan | Plán služby App Service | USA – západ |
| SpeechEchoBotTutorial – řeč | Cognitive Services | USA – západ |

## <a name="enable-web-sockets"></a>Povolit webové sokety

Budete muset udělat malou změnu konfigurace, aby robot mohl komunikovat s přímým řádkem řeči pomocí webových soketů. Pokud chcete povolit webové sokety, postupujte podle těchto kroků:

1. Přejděte do [Azure Portal](https://portal.azure.com)a vyhledejte App Service. Prostředek by měl mít název podobný jako **EchoBot20190805125647** (jedinečný název aplikace).
2. V navigaci **služeb Azure** v části **Nastavení**klikněte na **Konfigurace**.
3. Vyberte kartu **Obecné nastavení** .
4. Vyhledejte přepínač pro **webové sokety** a nastavte jej na **zapnuto**.
5. Klikněte na **Uložit**.

> [!TIP]
> Pomocí ovládacích prvků v horní části stránky Azure App Service můžete službu zastavit nebo restartovat. To může být užitečné při řešení potíží.

## <a name="create-a-channel-registration"></a>Vytvoření registrace kanálu

Teď, když jste vytvořili Azure App Service pro hostování robota, je dalším krokem vytvoření **registrace kanálů robota**. Vytvoření registrace kanálu je předpokladem pro registraci robota pomocí kanálů robot-Framework, včetně kanálu Direct line Speech.

> [!NOTE]
> Pokud se chcete dozvědět víc o tom, jak roboty využívají kanály, přečtěte si téma [připojení robota k kanálům](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0).


1. <a href="https://ms.portal.azure.com/#create/Microsoft.BotServiceConnectivityGalleryPackage" target="_blank">Vytvoření registrace kanálů pro Azure bot<span class="docon docon-navigate-external x-hidden-focus"></span></a>
2. Zobrazí se výzva k zadání některých informací:
   * Jako **popisovač robota**zadejte **SpeechEchoBotTutorial-BotRegistration-# # # #** a nahraďte **####** je číslem podle vaší volby. Poznámka: popisovač robota musí být globálně jedinečný. Pokud zadáte popisovač robota, ale zobrazí se chybová zpráva _požadované ID robota není k dispozici_, vyberte jiné číslo. V níže uvedených příkladech jsme použili 8726.
   * V případě **předplatného**vyberte **bezplatná zkušební verze**.
   * V případě **skupiny prostředků**vyberte **SpeechEchoBotTutorial-Resource**.
   * V **oblasti umístění**vyberte **západní USA**.
     * V případě **cenové úrovně**vyberte **F0**.
     * Do pole **koncový bod pro zasílání zpráv**zadejte adresu URL webové aplikace s cestou, která je `/api/messages` připojena na konci. Příklad: Pokud byl váš globálně jedinečný název aplikace **EchoBot20190805125647**, koncový bod pro zasílání zpráv by byl: `https://EchoBot20190805125647.azurewebsites.net/api/messages/` .
     * V případě služby **Application Insights**můžete tuto možnost nastavit na **vypnuto**. Další informace najdete v tématu věnovaném [analýze robotů](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0).
     * Ignoruje **Automatické vytvoření ID a heslo aplikace**.
5. V dolní části okna **registrace kanálů robota** klikněte na **vytvořit**.

V tomto okamžiku ověřte skupinu prostředků **SpeechEchoBotTutorial-Resource** v Azure Portal. Teď by se měly zobrazit čtyři prostředky:

| Name | Typ  | Umístění |
|------|-------|----------|
| EchoBot20190805125647 | App Service | USA – západ |
| SpeechEchoBotTutorial-AppServicePlan | Plán služby App Service | USA – západ |
| SpeechEchoBotTutorial-BotRegistration-8726 | Registrace kanálů robota | global |
| SpeechEchoBotTutorial – řeč | Cognitive Services | USA – západ |

> [!IMPORTANT]
> Prostředek registrace kanálů robota zobrazí globální oblast, i když jste vybrali Západní USA. To se očekává.

## <a name="optional-test-in-web-chat"></a>Volitelné: test ve webovém chatu

Registrační stránka kanálů robota Azure bot má v rámci **správy robotů**možnost **otestovat webový chat** . Ve výchozím nastavení nebude s robotem fungovat, protože webový chat potřebuje ověřit na robotovi. Pokud chcete otestovat nasazenou robota pomocí textového vstupu, postupujte podle následujících kroků. Všimněte si, že tyto kroky jsou volitelné a nejsou vyžadovány, aby bylo možné pokračovat v dalších krocích tohoto kurzu. 

1. Vyhledejte a otevřete svůj prostředek **EchoBotTutorial-BotRegistration-# #** # # v [Azure Portal](https://portal.azure.com)
1. V navigaci **pro správu bot** vyberte **Nastavení**. Zkopírujte hodnotu v části **ID aplikace Microsoftu** .
1. Otevřete řešení Visual Studio EchoBot. V Průzkumníku řešení Najděte **appsettings.js** a dvakrát klikněte na
1. Nahraďte prázdný řetězec vedle **MicrosoftAppId** v souboru JSON hodnotou ZKOPÍROVANÉho ID.
1. Vraťte se do Azure Portal, v navigaci **pro správu robota** vyberte **Nastavení**a klikněte na **(spravovat)** vedle **ID aplikace Microsoft** .
1. Klikněte na **nový tajný klíč klienta**. Přidejte popis (např. "webový chat") a klikněte na **Přidat**. Kopírování nového tajného klíče
1. Nahraďte prázdný řetězec vedle **MicrosoftAppPassword** v souboru JSON se zkopírovanou tajnou hodnotou.
1. Uložte soubor JSON. Výsledek by měl vypadat přibližně takto:
```json
{
  "MicrosoftAppId": "3be0abc2-ca07-475e-b6c3-90c4476c4370",
  "MicrosoftAppPassword": "-zRhJZ~1cnc7ZIlj4Qozs_eKN.8Cq~U38G"
}
```
9. Znovu publikujte aplikaci (klikněte pravým tlačítkem na projekt **EchoBot** v Průzkumníku řešení sady Visual Studio, vyberte **publikovat...** a klikněte na tlačítko **publikovat** ).
10. Teď jste připraveni otestovat robota ve webové konverzaci!

## <a name="register-the-direct-line-speech-channel"></a>Registrace kanálu Direct line Speech

Teď je čas zaregistrovat robota pomocí kanálu Direct line Speech. Tento kanál slouží k vytvoření propojení mezi robotem ozvěny a klientskou aplikací zkompilovanou sadou Speech SDK.

1. Na [Azure Portal](https://portal.azure.com)Najděte a otevřete svůj prostředek **SpeechEchoBotTutorial-BotRegistration-#** # # #.
1. Z navigace **pro správu bot** vyberte **kanály**.
   * Podívejte se na **Další kanály**, najděte a klikněte na **Direct line Speech**.
   * Projděte si text na stránce s názvem **Konfigurace přímého řádku řeč**a pak rozbalte rozevírací nabídku s názvem "účet služby pro rozpoznávání".
   * Vyberte prostředek řeči, který jste předtím vytvořili (např. **SpeechEchoBotTutorial-Speech**) z nabídky a přidružte svůj robot k vašemu klíči předplatného řeči.
   * Ignorovat zbývající volitelná pole
   * Klikněte na **Uložit**.

1. V navigaci **pro správu bot** klikněte na **Nastavení**.
   * Zaškrtněte políčko s popiskem **Povolit koncový bod streamování**. Tato možnost je nutná k povolení komunikačního protokolu postaveného na webových soketech mezi robotem a kanálem Direct line Speech.
   * Klikněte na **Uložit**.

> [!TIP]
> Pokud se chcete dozvědět víc, přečtěte si téma [připojení robota k přímému line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0). Tato stránka obsahuje další informace a známé problémy.

## <a name="run-the-windows-voice-assistant-client"></a>Spustit klienta Windows Voice Assistant

V tomto kroku budete spouštět klienta Windows Voice Assistant. Klient je aplikace Windows Presentation Foundation (WPF) v jazyce C#, která používá [sadu Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk) ke správě komunikace s robotem pomocí kanálu Direct line Speech. Použijte ho k interakci s a otestujte bot před psaním vlastní klientské aplikace. Je to open source, takže můžete stáhnout spustitelný soubor a spustit ho nebo si ho sestavit sami.

Klient hlasového asistenta systému Windows má jednoduché uživatelské rozhraní, které umožňuje nakonfigurovat připojení k robotovi, zobrazit textovou konverzaci, zobrazit aktivity robota v rámci formátu JSON a zobrazit adaptivní karty. Podporuje také použití vlastních klíčových slov. Tohoto klienta použijete ke mluvit s robotem a dostanete hlasovou reakci.

Než začneme, ujistěte se, že mikrofon a reproduktory jsou povolené a fungují.

1. Přejděte do úložiště GitHub pro [klienta Windows Voice Assistant](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/README.md).
1. Postupujte podle pokynů, které jsou k dispozici v aplikaci.
   * Stáhněte balíček ZIP obsahující spustitelný soubor, který chcete spustit, nebo
   * Sestavte spustitelný soubor sami tak, že naklonujte úložiště a sestavíte projekt.

1. Spusťte klientskou aplikaci a nakonfigurujte ji tak, aby se připojila k robotovi na základě pokynů v úložišti GitHubu.
1. Klikněte na **znovu připojit** a ujistěte se, že se zobrazí zpráva **stiskněte tlačítko mikrofonu, nebo napište, abyste mohli začít mluvit s robotem**.
1. Pojďme si to vyzkoušet. Klikněte na tlačítko mikrofon a mluvte několik slov v angličtině. Rozpoznaný text se zobrazí při mluvení. Až budete hotovi, robot odpoví ve svém vlastním hlasu a říká "echo" následovaný rozpoznanými slovy.
1. K komunikaci s robotem můžete také použít text. Stačí zadat text na dolním panelu. 

### <a name="troubleshooting-errors-in-windows-voice-assistant-client"></a>Řešení chyb v klientovi Windows Voice Assistant

Pokud se zobrazí chybová zpráva v hlavním okně aplikace, použijte tuto tabulku k identifikaci a řešení potíží s chybou:

| Chyba | Co byste měli udělat? |
|-------|----------------------|
|Chyba (AuthenticationFailure): upgrade protokolu WebSocket se nezdařil s chybou ověřování (401). Vyhledat správný klíč předplatného (nebo autorizační token) a název oblasti| Na stránce nastavení aplikace se ujistěte, že jste zadali správný klíč předplatného pro rozpoznávání řeči a jeho oblast.<br>Ujistěte se, že klíč řeči a klíčová oblast byly zadány správně. |
|Chyba (ConnectionFailure): připojení bylo ukončeno vzdáleným hostitelem. Kód chyby: 1011. Podrobnosti o chybě: před odesláním zprávy se nepovedlo připojit k robotovi. | Ujistěte se, že jste [zaškrtli políčko Povolit koncový bod streamování](#register-the-direct-line-speech-channel) nebo na zapnuté [ **webové zásuvky** ](#enable-web-sockets) .<br>Ujistěte se, že je spuštěný Azure App Service. Pokud je to, zkuste restartovat App Service.|
|Chyba (ConnectionFailure): připojení bylo ukončeno vzdáleným hostitelem. Kód chyby: 1002. Podrobnosti o chybě: Server vrátil stavový kód "503", pokud byl očekáván stavový kód "101" | Ujistěte se, že jste [zaškrtli políčko Povolit koncový bod streamování](#register-the-direct-line-speech-channel) nebo na zapnuté [ **webové zásuvky** ](#enable-web-sockets) .<br>Ujistěte se, že je spuštěný Azure App Service. Pokud je to, zkuste restartovat App Service.|
|Chyba (ConnectionFailure): připojení bylo ukončeno vzdáleným hostitelem. Kód chyby: 1011. Podrobnosti o chybě: kód stavu odpovědi neindikuje úspěch: 500 (Nenalezeno)| Vaše robot zadal neuronové hlas do výstupního pole [speaking](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) Activity, ale oblast Azure přidružená k vašemu klíči předplatného pro rozpoznávání řeči nepodporuje hlasy neuronové. Viz [hlasy Standard a neuronové](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).|

Pokud se problém nevyřeší v tabulce, přečtěte si téma [asistenti hlasu: nejčastější dotazy](faq-voice-assistants.md). Pokud vaše potíže ještě nepůjde vyřešit po splnění všech kroků v tomto kurzu, zadejte prosím nový problém na [stránce GitHubu hlasového pomocníka](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/issues).

#### <a name="a-note-on-connection-time-out"></a>Poznámka k vypršení časového limitu připojení

Pokud jste připojení ke robotovi a za posledních 5 minut nedošlo k žádné aktivitě, služba automaticky ukončí připojení protokolu WebSocket k klientovi a s robotem. Toto chování je úmyslné. Na dolním panelu se zobrazí zpráva: *vypršel časový limit aktivního připojení, ale jeho Příprava na vyžádání znovu připojit*. Nemusíte stisknout tlačítko "znovu připojit" – Stačí stisknout tlačítko mikrofonu a začít mluvit, zadat textovou zprávu nebo vyslovit klíčové slovo (Pokud je povolené). Připojení se automaticky znovu vytvoří.  
### <a name="view-bot-activities"></a>Zobrazit aktivity robota

Každý robot odesílá a přijímá zprávy o **aktivitách** . V okně **Protokol aktivit** klienta Windows Voice Assistant uvidíte protokoly s časovým razítkem s každou aktivitou, kterou klient přijal od robota. Můžete také zobrazit aktivity, které klient poslal do robota pomocí [`DialogServiceConnector.SendActivityAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.sendactivityasync) metody. Když vyberete položku protokolu, zobrazí se podrobnosti související aktivity jako JSON.

Tady je ukázkový JSON aktivity, kterou klient přijal:

```json
{
    "attachments":[],
    "channelData":{
        "conversationalAiData":{
             "requestInfo":{
                 "interactionId":"8d5cb416-73c3-476b-95fd-9358cbfaebfa",
                 "version":"0.2"
             }
         }
    },
    "channelId":"directlinespeech",
    "conversation":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79",
        "isGroup":false
    },
    "entities":[],
    "from":{
        "id":"SpeechEchoBotTutorial-BotRegistration-8726"
    },
    "id":"89841b4d-46ce-42de-9960-4fe4070c70cc",
    "inputHint":"acceptingInput",
    "recipient":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79|0000"
    },
    "replyToId":"67c823b4-4c7a-4828-9d6e-0b84fd052869",
    "serviceUrl":"urn:botframework:websocket:directlinespeech",
    "speak":"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'><voice name='Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)'>Echo: Hello and welcome.</voice></speak>",
    "text":"Echo: Hello and welcome.",
    "timestamp":"2019-07-19T20:03:51.1939097Z",
    "type":"message"
}
```

Další informace o tom, co se vrátilo ve výstupu JSON, najdete v tématu [pole v aktivitě](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md). Pro účely tohoto kurzu se můžete zaměřit na pole [text](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#text) a [Speak](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) .

### <a name="view-client-source-code-for-calls-to-the-speech-sdk"></a>Zobrazit zdrojový kód klienta pro volání sady Speech SDK

Klient Windows Voice Assistant používá balíček NuGet [Microsoft. cognitiveservices Account. Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), který obsahuje sadu Speech SDK. Dobrým místem, kde začít kontrolovat vzorový kód, je metoda InitSpeechConnector () v souboru [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs) , která vytváří tyto dva objekty sady Speech SDK:
- [`DialogServiceConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconfig)– Pro konfigurační nastavení (například klíč předplatného pro rozpoznávání řeči, klíčová oblast)
- [`DialogServiceConnector`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.-ctor)– Ke správě připojení kanálu a událostí odběru klienta pro zpracování rozpoznaných odpovědí na řeč a robotů.

## <a name="add-custom-keyword-activation"></a>Přidat vlastní aktivaci klíčového slova

Sada Speech SDK podporuje aktivaci vlastního klíčového slova. Podobně jako "Hey Cortana" pro pomocníka společnosti Microsoft můžete napsat aplikaci, která bude průběžně naslouchat klíčovému slovu podle vašeho výběru. Mějte na paměti, že klíčové slovo může být jedno slovo nebo fráze více slov.

> [!NOTE]
> *Klíčové slovo* Term se často používá k zaznamenání na *probouzení slova*a může se zobrazit v dokumentaci Microsoftu.

Rozpoznávání klíčových slov se provádí v klientské aplikaci. Při použití klíčového slova je zvuk streamování do kanálu s přímým přístupem, pokud je klíčové slovo rozpoznáno. Kanál Direct line Speech zahrnuje komponentu s názvem *ověřování klíčovým slovem (KWV)*, která provádí složitější zpracování v cloudu, aby se ověřilo, že klíčové slovo, které jste zvolili, je na začátku zvukového streamu. Pokud se ověření pomocí klíčového slova zdaří, kanál bude komunikovat s robotem.

Pomocí těchto kroků můžete vytvořit model klíčových slov, nakonfigurovat klienta Windows Voice Assistant tak, aby používal tento model, a nakonec ho otestovat pomocí robota.

1. Podle těchto pokynů [vytvořte vlastní klíčové slovo pomocí služby Speech](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
2. Rozbalte soubor modelu, který jste stáhli v předchozím kroku. Měla by být pojmenována pro vaše klíčové slovo. Hledáte soubor s názvem `kws.table` .
3. V klientovi pomocníka pro Windows hlas Vyhledejte nabídku **Nastavení** (v pravém horním rohu vyhledejte ikonu ozubeného kolečka). Vyhledejte **cestu k souboru modelu** a zadejte úplný název cesty k `kws.table` souboru z kroku 2.
4. Nezapomeňte zaškrtnout políčko označený jako **povolené**. Tato zpráva by se měla zobrazit vedle zaškrtávacího políčka: "při dalším připojení naslouchat klíčové slovo." Pokud jste zadali nesprávný soubor nebo neplatnou cestu, měla by se zobrazit chybová zpráva.
5. Zadejte **klíč předplatného**pro rozpoznávání řeči, **oblast klíče předplatného**a potom kliknutím na tlačítko **OK** zavřete nabídku **Nastavení** .
6. Klikněte na **znovu připojit**. Měla by se zobrazit zpráva, že "nová konverzace byla spuštěna – typ, stiskněte tlačítko mikrofonu, nebo řekněte klíčové slovo". Aplikace teď neustále naslouchá.
7. Mluvte všechny fráze začínající vaším klíčovým slovem. Například: "**{Your klíčové slovo}**, jaký je čas?". Po umístění klíčového slova se nemusíte porušit. Po dokončení budou provedeny dvě věci:
   * Uvidíte si, co jste paprskový
   * Krátce po vás uslyšíte odpověď robota.
8. Pokračujte v experimentování se třemi vstupními typy, které vaše robot podporuje:
   * Zadaný text na dolním panelu
   * Stisknutí ikony mikrofonu a mluvení
   * Říká se fráze začínající vaším klíčovým slovem.

### <a name="view-the-source-code-that-enables-keyword"></a>Zobrazit zdrojový kód, který povoluje klíčové slovo

Ve zdrojovém kódu klienta Windows Voice Assistant si prohlédněte tyto soubory a zkontrolujte kód, který se používá k povolení rozpoznávání klíčových slov:

1. [`VoiceAssistantClient\Models.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/Models.cs)obsahuje volání metody sady Speech SDK [`KeywordRecognitionModel.fromFile()`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/keywordrecognitionmodel?view=azure-node-latest#fromfile-string-) , která se používá k vytvoření instance modelu z místního souboru na disku.
1. [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs)obsahuje volání metody sady Speech SDK [`DialogServiceConnector.StartKeywordRecognitionAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.startkeywordrecognitionasync) , které aktivuje funkci zjišťování nepřetržitého klíčového slova.

## <a name="optional-change-the-language-and-bot-voice"></a>Volitelné Změna jazyka a hlasu robota

Robot, který jste vytvořili, bude naslouchat a reagovat v angličtině s výchozím hlasem pro AMERICKou angličtinu pro text na řeč. Nejste však omezeni na použití angličtiny nebo výchozího hlasu. V této části se dozvíte, jak změnit jazyk, ve kterém bude robot naslouchat a reagovat. Naučíte se také, jak vybrat jiný hlas pro daný jazyk.

### <a name="change-the-language"></a>Změnit jazyk

Můžete zvolit některý z jazyků uvedených v tabulce [Převod řeči na text](language-support.md#speech-to-text) . V níže uvedeném příkladu změníme jazyk na němčina.

1. Otevřete klientskou aplikaci pro Windows Voice Assistant, klikněte na tlačítko nastavení (ikona vpravo nahoře) a zadejte `de-de` hodnotu do pole jazyk (Toto je hodnota národního prostředí uvedená v tabulce [Převod řeči na text](language-support.md#speech-to-text) ). Tím se nastaví mluvený jazyk, který se má rozpoznat, přepsání výchozího `en-us` . Tato funkce také dává přímým linkovým kanálům pro odpověď robota použít výchozí německý hlas.
2. Zavřete stránku nastavení a klikněte na tlačítko znovu připojit a vytvořte nové připojení k robotovi ozvěny.
3. Klikněte na tlačítko mikrofon a vyslovením fráze v němčině. Zobrazí se rozpoznaný text a odpověď robota pro ozvěnu s výchozím německým hlasem.

### <a name="change-the-default-bot-voice"></a>Změnit výchozí hlas robota

Výběr hlasu pro převod textu na řeč a řízení výslovnosti je možné provést v případě, že robot určí odpověď ve formě jazyka SSML ( [Speech syntézy Language](speech-synthesis-markup.md) ) místo jednoduchého textu. Robot ozvěny nepoužívá SSML, ale můžeme kód snadno upravit. V následujícím příkladu přidáme SSML do odpovědi robota echo, aby se místo výchozího ženského hlasu použil německý hlas Stefan Apollo (kolík hlasu). Seznamte se se [standardními hlasy](language-support.md#standard-voices) a [neuronové hlasy](language-support.md#neural-voices) , které podporuje váš jazyk.

1. Pojďme začít otevřením `samples\csharp_dotnetcore\02.echo-bot\echo-bot.cs` .
2. Vyhledejte tyto dva řádky:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
    ```
3. Nahraďte je:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    var replySpeak = @"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='de-DE'>
                    <voice name='Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)'>" +
                    $"{replyText}" + "</voice></speak>";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replySpeak), cancellationToken);
    ```
4. Sestavte řešení v aplikaci Visual Studio a opravte všechny chyby sestavení.

Druhý argument v metodě ' MessageFactory. text ' nastavuje [pole Speak Activity](https://github.com/Microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) v odpovědi bot. Výše uvedená změna byla nahrazena z jednoduchého textu na SSML, aby bylo možné zadat jiný než výchozí německý hlas.

### <a name="redeploy-your-bot"></a>Opětovné nasazení robota

Teď, když jste udělali potřebnou změnu robota, je dalším krokem jeho opětovné publikování do Azure App Service a pokus o to:

1. V okně Průzkumník řešení klikněte pravým tlačítkem na projekt **EchoBot** a vyberte **publikovat**.
2. Předchozí konfigurace nasazení již byla načtena jako výchozí. Jednoduše klikněte na **publikovat** vedle **EchoBot20190805125647-nasazení webu**.
3. Zpráva o **úspěšném publikování** se zobrazí v okně výstupu sady Visual Studio a otevře se webová stránka se zprávou "robot je připravený!".
4. Otevřete klientskou aplikaci pro Windows Voice Assistant, klikněte na tlačítko nastavení (ikona pravého ozubeného kolečka) a ujistěte se, že máte stále `de-de` v poli jazyk.
5. Postupujte podle pokynů v části [spuštění klienta hlasového pomocníka Windows](#run-the-windows-voice-assistant-client) pro opětovné připojení k nově nasazenému robotovi, mluvte v novém jazyce a uslyšíte v tomto jazyce reakci robota s novým hlasem.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete nadále používat službu echo-bot nasazenou v tomto kurzu, můžete ji odebrat a všechny její přidružené prostředky Azure odstranit pouhým odstraněním skupiny prostředků Azure **SpeechEchoBotTutorial-Resource**.

1. V [Azure Portal](https://portal.azure.com)klikněte na **skupiny prostředků** z navigace **služeb Azure** .
2. Vyhledejte skupinu prostředků s názvem: **SpeechEchoBotTutorial-Resource**. Klikněte na tři tečky (...).
3. Vyberte **Odstranit skupinu prostředků**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření vlastní klientské aplikace pomocí sady Speech SDK](quickstart-voice-assistant-csharp-uwp.md)

## <a name="see-also"></a>Viz také

* Nasazení do [oblasti Azure v blízkosti,](https://azure.microsoft.com/global-infrastructure/locations/) abyste viděli vylepšení doby odezvy robota
* Nasazení do [oblasti Azure, která podporuje hlasy vysoké kvality neuronové TTS](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)
* Ceny spojené s kanálem Direct line Speech:
  * [Ceny služby bot Service](https://azure.microsoft.com/pricing/details/bot-service/)
  * [Služba řeči](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
* Sestavování a nasazování vlastního robota s povoleným hlasem:
  * Sestavte [robota v rozhraní robota](https://dev.botframework.com/). Zaregistrujte si ho pomocí [kanálu Direct line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) a [Přizpůsobte si robota pro hlas](https://docs.microsoft.com/azure/bot-service/directline-speech-bot?view=azure-bot-service-4.0) .
  * Seznamte se s existujícími [řešeními pro roboty](https://microsoft.github.io/botframework-solutions/index): vytvoření [virtuálního asistenta](https://microsoft.github.io/botframework-solutions/overview/virtual-assistant-solution/) a [jeho roztažení do přímého lineového rozpoznávání](https://microsoft.github.io/botframework-solutions/clients-and-channels/tutorials/enable-speech/1-intro/) .
