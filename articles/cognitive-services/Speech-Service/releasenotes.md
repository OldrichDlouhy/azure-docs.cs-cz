---
title: Poznámky k verzi – služba pro rozpoznávání řeči
titleSuffix: Azure Cognitive Services
description: Běžící protokol vydaných verzí funkcí hlasové služby, vylepšení, oprav chyb a známé problémy.
services: cognitive-services
author: oliversc
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: oliversc
ms.custom: seodec18
ms.openlocfilehash: 152907908f12a41679b3161e0c4b39348926399e
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373789"
---
# <a name="speech-service-release-notes"></a>Poznámky k verzi služby Speech Service

## <a name="speech-sdk-1130-2020-july-release"></a>Sada Speech SDK 1.13.0:2020 – červenec

**Poznámka**: sada Speech SDK v systému Windows závisí na sdílených Microsoft Visual C++ distribuovatelné pro Visual Studio 2015, 2017 a 2019. Stáhněte si ho a nainstalujte [odsud.](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

**Nové funkce**
- **C#**: Přidání podpory pro přepis asynchronní konverzace. Další informace [najdete v dokumentaci.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-async-conversation-transcription)  
- **JavaScript**: přidání podpory rozpoznávání mluvčího pro [prohlížeč](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser/speaker-recognition) i [node.js](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/speaker-recognition).
- **JavaScript**: Přidání podpory pro automatické rozpoznání jazyka/ID jazyka. Další informace [najdete v dokumentaci.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-javascript)
- **Cíl-C**: Přidali jsme podporu pro konverzaci s více zařízeními a Přepisy konverzace. 
- **Python**: Přidání komprimované zvukové podpory pro Python v systému Windows a Linux. Další informace [najdete v dokumentaci.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams) 

**Opravy chyb**
- **Vše**: Opravili jsme problém, který způsobil, že se KeywordRecognizer po rozpoznávání nepřesunuly proudy vpřed.
- **Vše**: Opravili jsme problém, který způsobil, že datový proud získaný z KeywordRecognitionResult neobsahuje klíčové slovo.
- **Vše**: Opravili jsme problém, který SendMessageAsync skutečně neposílá zprávu po skončení čekání na tento kabel.
- **Vše**: Opravili jsme chybu v rozhraních API rozpoznávání mluvčího, když uživatelé spouštějí několik VoiceProfileClient:: SpeakerRecEnrollProfileAsync a nečekali na jejich dokončení.
- **All**: pevný povolit protokolování souborů v třídách VoiceProfileClient a SpeakerRecognizer.
- **JavaScript**: byl vyřešen [problém](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/74) s omezením při minimalizaci prohlížeče.
- **JavaScript**: byl opraven [problém](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/78) s nevrácenou pamětí v datových proudech.
- **JavaScript**: přidání do mezipaměti odpovědí protokolu OCSP z NodeJS.
- **Java**: Opravili jsme problém, který způsobil, že pole BigInteger vždycky vrátí hodnotu 0.
- **iOS**: opraveno a [problém](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/702) s publikováním aplikací založených na sadě Speech SDK v iOS App Storu.

**ukázky**
- **C++**: [sem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/cpp/windows/console/samples/speaker_recognition_samples.cpp)přidejte vzorový kód pro rozpoznávání mluvčího.

**COVID-19 – zkrácené testování:** Vzhledem k tomu, že během posledních několika týdnů pracujete vzdáleně, nemůžeme provést tolik testování ručních ověření jako obvykle. Neudělali jsme žádné změny, které by bylo možné jakkoli rozdělit, a naše automatizované testy byly úspěšné. V nepravděpodobném případě, že jsme něco zmeškali, dejte nám prosím na [GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen)informace.<br>
Buďte v pořádku!

## <a name="text-to-speech-2020-july-release"></a>Převod textu na řeč 2020 – červenec verze

### <a name="new-features"></a>Nové funkce

* **Neuronové TTS, 15 novinek neuronové**– nové hlasy přidané do portfolia neuronové TTS jsou Salma v `ar-EG` arabštině (Egypt), Zariyah v `ar-SA` arabštině (Saúdská Arábie), alba v `ca-ES` Katalánština (Španělsko), Christel v dánštině ( `da-DK` Dánsko), Neerja v `es-IN` angličtině (Indie), Noora ve `fi-FI` finštině (Finsko), Swara v `hi-IN` hindštině (Indie), Colette v `nl-NL` holandštině (Nizozemsko), Zofia v `pl-PL` polštině (Polsko), Fernanda v `pt-PT` portugalštině (Portugalsko), Dariya v `ru-RU` ruštině (Rusko), Hillevi ve švédštině (Švédsku), Achara v thajštině (Thajsko), HiuGaai v `sv-SE` `th-TH` `zh-HK` čínštině (kantonština, tradiční) a HsiaoYu v `zh-TW` čínštině (tchajwanští. Zkontroluje všechny [podporované jazyky](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices).  

* **Vlastní hlas, zjednodušené hlasové testování s výukovým tokem pro zjednodušení uživatelského prostředí**: Díky nové funkci testování se každý hlas automaticky testuje pomocí předdefinované testovací sady optimalizované pro jednotlivé jazyky, které budou pokrývat scénáře obecného a hlasového asistenta. Tyto sady testů pečlivě vyberou a testují, aby zahrnovaly typické případy použití a fonémy v jazyce. Kromě toho mohou uživatelé i nadále vybrat nahrávání vlastních testovacích skriptů při výuce modelu.

* **Vytvoření zvukového obsahu: byla vydána sada nových funkcí pro zajištění výkonnějšího ladění hlasu a možností správy zvuku.**

    * `Pitch`, `rate` a `volume` jsou rozšířeny, aby podporovaly optimalizaci s předdefinovanou hodnotou, jako je například pomalé, střední a rychlé. Pro uživatele je teď snadné vybrat hodnotu "Constant" pro jejich úpravu zvuku.

    ![Ladění zvuku](media/release-notes/audio-tuning.png)

    * Uživatelé teď můžou zkontrolovat `Audio history` soubor pro práci. Díky této funkci mohou uživatelé snadno sledovat veškeré vygenerované zvuky související s pracovním souborem. Můžou si prohlédnout historii verze a porovnat kvalitu současně s optimalizací. 

    ![Historie zvuků](media/release-notes/audio-history.png)

    * Tato `Clear` funkce je teď pružnější. Uživatelé můžou vymazat konkrétní parametr ladění a přitom ponechat další parametry dostupné pro vybraný obsah.  

    * Na [úvodní stránce](https://speech.microsoft.com/audiocontentcreation) se přidalo video s kurzem, které uživatelům pomůže rychle začít pracovat s laděním hlasu a řečí pro řeč. 

### <a name="general-tts-voice-quality-improvements"></a>Obecná vylepšení kvality hlasu TTS

* Vylepšený TTS vocoder v pro vyšší věrnost a nižší latenci.

    * Aktualizovali jsme Elsa na `it-IT` nové vocoder, které dosáhlo + 0,464 paměti CMOS (srovnávací průměrné skóre názoru) v kvalitě hlasu, 40% rychlejší v syntéze a 30% snížení při první latenci bajtů. 
    * Aktualizovali jsme Xiaoxiao na `zh-CN` nové vocoder s + 0148 paměti CMOS pro obecné domény, + 0,348 pro styl newscast a + 0,195 pro styl Lyrical. 

* Aktualizované `de-DE` a `ja-JP` hlasové modely, aby byl výstup TTS více přirozený.
    
    * Aktualizace Katja v `de-DE` nástroji pomocí nejnovější metody modelování Prosody, MOS (střední skóre stanoviska) je + 0,13. 
    * Aktualizovali jsme Nanami v `ja-JP` rámci nového sklonu pro zdůraznění Prosody, MOS (střední skóre stanoviska) je + 0,19;  

* Vylepšená přesnost výslovnosti na úrovni aplikace v 5 jazycích.

    | Jazyk | Omezení chyby výslovnosti |
    |---|---|
    | `en-GB` | 51% |
    | `ko-KR` | sedmnáct |
    | `pt-BR` | 39% |
    | `pt-PT` | 77% |
    | `id-ID` | 46% |

### <a name="bug-fixes"></a>Opravy chyb

* Čtení měny
    * Opravili jsme problém se čtením měny pro `es-ES` a`es-MX`
     
    | Jazyk | Vstup | Přečtení po vylepšení |
    |---|---|---|
    | `es-MX` | $1,58 | un Peso cincuenta y Ocho centavos |
    | `es-ES` | $1,58 | dólar cincuenta y Ocho centavos |

    * Podpora pro zápornou měnu (například-€325) v následujících národních prostředích: `en-US` , `en-GB` , `fr-FR` , `it-IT` , `en-AU` , `en-CA` .

* Vylepšené čtení adres v `pt-PT` .
* Opravili jsme `en-AU` problémy s výslovností Natasha () a Libby ( `en-UK` ) na slovech "for" a "4".  
* Opravené chyby v nástroji pro tvorbu zvukového obsahu
    * Další a neočekávané pozastavení po druhém odstavci je opraveno.  
    * Funkce No Break je přidána zpátky z regresní chyby. 
    * Vyřešil se problém s náhodným aktualizací Speech studia.  

### <a name="samplessdk"></a>Ukázky/sada SDK

* JavaScript: řeší problém s přehráváním v prohlížeči Firefox a Safari v macOS a iOS. 

## <a name="speech-sdk-1121-2020-june-release"></a>Sada Speech SDK 1.12.1:2020 – červenou verzi
**Rozpoznávání řeči (označované také jako SPX)**
-   Přidání funkcí vyhledávání v nápovědě k rozhraní příkazového řádku:
    -   `spx help find --text TEXT`
    -   `spx help find --topic NAME`
-   Aktualizováno pro práci s nově nasazenými v 3.0 a Custom Speech rozhraní API pro nasazení:
    -   `spx help batch examples`
    -   `spx help csr examples`

**Nové funkce**
-   **C \# , C++**: rozpoznávání mluvčího Preview: Tato funkce umožňuje identifikaci mluvčího (který mluví?) a ověření mluvčího (Jedná se o to, kdo to vyžádá?). Začněte s [přehledem](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/speaker-recognition-overview), přečtěte si [článek Základy rozpoznávání mluvčího](https://docs.microsoft.com/azure/cognitive-services/speech-service/speaker-recognition-basics)nebo [Referenční dokumentace k rozhraní API](https://docs.microsoft.com/rest/api/speakerrecognition/).

**Opravy chyb**
-   **C \# , C++**: pevný záznam o mikrofonu v rozpoznávání mluvčího nepracuje v 1,12.
-   **JavaScript**: oprava pro převod textu na řeč v prohlížeči Firefox a Safari v MacOS a iOS.
-   Oprava chyby narušení přístupu ověřovatele aplikace systému Windows v přepisu konverzace při použití streamu s 8 kanály.
-   Oprava chyby narušení přístupu ověřovače aplikací systému Windows v překladu konverzací na více zařízení.

**ukázky**
-   **C#**: [Ukázka kódu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) pro rozpoznávání mluvčího.
-   **C++**: [Ukázka kódu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) pro rozpoznávání mluvčího.
-   **Java**: [Ukázka kódu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/intent-recognition) pro rozpoznávání záměrů v Androidu. 

**COVID-19 – zkrácené testování:** Vzhledem k tomu, že během posledních několika týdnů pracujete vzdáleně, nemůžeme provést tolik testování ručních ověření jako obvykle. Neudělali jsme žádné změny, které by bylo možné jakkoli rozdělit, a naše automatizované testy byly úspěšné. V nepravděpodobném případě, že jsme něco zmeškali, dejte nám prosím na [GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen)informace.<br>
Buďte v pořádku!


## <a name="speech-sdk-1120-2020-may-release"></a>Sada Speech SDK 1.12.0:2020 – může vydávat
**Rozhraní příkazového řádku pro rozpoznávání řeči (také znám jako SPX)**
- **SPX** je nový nástroj příkazového řádku, který umožňuje provádět rozpoznávání, syntézu, překlad, dávkování a vlastní správu řeči z příkazového řádku. Použijte ji k otestování hlasové služby nebo ke skriptování úloh služby Speech, které potřebujete provést. Stáhněte si nástroj a přečtěte si dokumentaci [zde](https://docs.microsoft.com/azure/cognitive-services/speech-service/spx-overview).

**Nové funkce**
- **Přejít**: Podpora jazyka New přejít pro [rozpoznávání řeči](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) a [vlastního hlasového asistenta](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go) [Sem](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-go)nastavte vývojové prostředí. Vzorový kód najdete níže v části s ukázkami. 
- **JavaScript**: přidaná Podpora prohlížeče pro převod textu na řeč. Další informace [najdete v dokumentaci.](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech-audio-file?pivots=programming-language-JavaScript)
- **C++, C#, Java**: nový `KeywordRecognizer` objekt a rozhraní API podporovaná na platformách Windows, Android, Linux & iOS. Přečtěte si dokumentaci [zde](https://docs.microsoft.com/azure/cognitive-services/speech-service/custom-keyword-overview). Vzorový kód najdete níže v části s ukázkami. 
- **Java**: Přidání konverzace s více zařízeními s podporou překladu Podívejte se na odkaz na [Tento dokument.](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.transcription)

**Vylepšení & optimalizace**
- **JavaScript**: optimalizovaná implementace mikrofonu prohlížeče vylepšuje přesnost rozpoznávání řeči.
- **Java**: refaktorované vazby pomocí přímé implementace JNI bez SWIG. Tím se sníží 10x velikost vazeb pro všechny balíčky Java používané pro Windows, Android, Linux a Mac a usnadňují další vývoj implementace sady Speech SDK jazyka Java.
- **Linux**: aktualizovaná [dokumentace k](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux) podpoře s nejnovějšími poznámkami k RHEL 7.
- Vylepšená logika připojení pro pokus o připojení vícekrát v případě chyb služby a sítě.
- Aktualizovali jsme stránku rychlý Start řeči [Portal.Azure.com](https://portal.azure.com) , která vývojářům pomůžou zabrat další krok v cestě ke službě Azure Speech.

**Opravy chyb**
- **C#, Java**: Opravili jsme [problém](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/587) s načítáním knihoven SDK v systému Linux ARM (32 a 64 bitů).
- **C#**: Opravili jsme explicitní odstraňování nativních popisovačů pro objekty TranslationRecognizer, IntentRecognizer a Connection.
- **C#**: pevná Správa životnosti vstupů zvuku pro objekt ConversationTranscriber.
- Opravili jsme problém, kdy `IntentRecognizer` byl důvod výsledku správně nastavený při rozpoznávání záměrů z jednoduchých frází.
- Opravili jsme problém, kdy `SpeechRecognitionEventArgs` se posun výsledku nesprávně nastavil.
- Opravili spor, ve kterém se sada SDK pokoušela Odeslat síťovou zprávu před otevřením připojení protokolu WebSocket. Bylo reprodukovatelné `TranslationRecognizer` při přidávání účastníků.
- Pevná nevracení paměti v modulu pro rozpoznávání klíčových slov.

**ukázky**
- **Přejít**: Přidali jsme rychlé starty pro [rozpoznávání řeči](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone?pivots=programming-language-go) a [vlastního hlasového asistenta](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/voice-assistants?pivots=programming-language-go). [Zde](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples)najdete vzorový kód. 
- **JavaScript**: Přidali jsme rychlé starty pro převod [textu na řeč](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/text-to-speech?pivots=programming-language-javascript), [překlady](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/translate-speech-to-text?pivots=programming-language-javascript)a [rozpoznávání záměru](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/intent-recognition?pivots=programming-language-javascript).
- Ukázky rozpoznávání klíčových slov pro jazyky [C \# ](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer) a [Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer) (Android).  

**COVID-19 – zkrácené testování:** Vzhledem k tomu, že během posledních několika týdnů pracujete vzdáleně, nemůžeme provést tolik testování ručních ověření jako obvykle. Neudělali jsme žádné změny, které by bylo možné jakkoli rozdělit, a naše automatizované testy byly úspěšné. V nepravděpodobném případě, že jsme něco zmeškali, dejte nám prosím na [GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen)informace.<br>
Buďte v pořádku!

## <a name="speech-sdk-1110-2020-march-release"></a>Sada Speech SDK 1.11.0:2020-březen verze
**Nové funkce**
- Linux: Přidání podpory pro Red Hat Enterprise Linux (RHEL)/CentOS 7 x64 s [pokyny](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-rhel-centos-7) , jak nakonfigurovat systém pro sadu Speech SDK.
- Linux: Přidání podpory pro .NET Core C# v systémech Linux ARM32 a ARM64. Další informace si můžete přečíst [zde](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=linux). 
- C#, C++: přidáno `UtteranceId` v `ConversationTranscriptionResult` , konzistentní ID napříč všemi zprostředkujícími a konečný výsledek rozpoznávání řeči. Podrobnosti pro [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.transcription.conversationtranscriptionresult?view=azure-dotnet), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/transcription-conversationtranscriptionresult).
- Python: přidala se podpora pro `Language ID` . Podívejte se prosím na speech_sample. py v [úložišti GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console).
- Windows: Přidání komprimovaného formátu zvukového vstupu na platformu Windows pro všechny konzolové aplikace Win32. Podrobnosti [.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams) 
- JavaScript: podporuje syntézu řeči (převod textu na řeč) v NodeJS. Další informace najdete [tady](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech). 
- JavaScript: přidejte nové rozhraní API, které umožní kontrolu všech zpráv Send a Received. Další informace najdete [tady](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript). 
        
**Opravy chyb**
- C#, C++: vyřešil se problém, takže `SendMessageAsync` teď odesílá binární zprávu jako binární typ. Podrobnosti pro [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.sendmessageasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Connection_SendMessageAsync_System_String_System_Byte___System_UInt32_), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection).
- C#, C++: Opravili jsme problém, kdy použití `Connection MessageReceived` události může způsobit chybu, pokud `Recognizer` je uvolněn před `Connection` objektem. Podrobnosti pro [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection.messagereceived?view=azure-dotnet), [C++](https://docs.microsoft.com/cpp/cognitive-services/speech/connection#messagereceived).
- Android: velikost zvukové vyrovnávací paměti z mikrofonu se snížila z 800ms na 100 ms, aby se zlepšila latence.
- Android: Opravili jsme [problém](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/563) s emulátorem x86 pro Android v Android Studio.
- JavaScript: Přidání podpory pro oblasti v Číně s `fromSubscription` rozhraním API. Podrobnosti [.](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#fromsubscription-string--string-) 
- JavaScript: přidejte další informace o chybě pro chyby připojení z NodeJS.
        
**ukázky**
- Unity: veřejná ukázka rozpoznávání záměrů je pevná, kde se LUIS import JSON nezdařil. Podrobnosti [.](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/369)
- Python: byla přidána ukázka pro `Language ID` . Podrobnosti [.](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py)
    
**Covid19 zkrácené testování:** Vzhledem k tomu, že během posledních několika týdnů pracujete vzdáleně, nemůžeme provést tolik ručních testů ověřování zařízení, jako bychom normálně. Příkladem je testování mikrofonu vstupu a mluvčího výstupu v systémech Linux, iOS a macOS. Neudělali jsme žádné změny, které jsme na těchto platformách mohli jakkoli rozdělit, a naše automatizované testy jsme prošli všemi úspěchy. V nepravděpodobném případě, že jsme něco zmeškali, dejte nám prosím na [GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen)informace.<br> Děkujeme za vaši trvalou podporu. Jako vždycky prosím vydejte dotazy nebo připomínky na [GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) nebo [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/731).<br>
Buďte v pořádku!

## <a name="speech-sdk-1100-2020-february-release"></a>Sada Speech SDK 1.10.0:2020 – Únorová verze

**Nové funkce**

 - Přidání balíčků Pythonu pro podporu nové verze 3,8 Pythonu
 - Red Hat Enterprise Linux (RHEL)/CentOS 8 x64 Support (C++, C#, Java, Python).
   > [!NOTE] 
   > Zákazníci musí podle [těchto pokynů](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)nakonfigurovat OpenSSL.
 - Podpora ARM32 pro Linux pro Debian a Ubuntu.
 - DialogServiceConnector nyní podporuje volitelné parametr "robot ID" v BotFrameworkConfig. Tento parametr umožňuje použití několika přímých roboty řeči s jedním prostředkem řeči Azure. Bez zadaného parametru se použije výchozí robot (tak, jak je určený na stránce Konfigurace kanálu pro přímé čáry).
 - DialogServiceConnector má teď vlastnost SpeechActivityTemplate. Obsah tohoto řetězce JSON bude použit přímým převodem na řádku k předběžnému naplnění široké škály podporovaných polí ve všech činnostech, které se dodávají k přímému line Speech bot, včetně aktivit automaticky generovaných v reakci na události, jako je rozpoznávání řeči.
 - TTS nyní používá pro ověřování klíč předplatného, což snižuje latenci prvního výsledku Shrnutí po vytvoření syntetizátoru.
 - Aktualizované modely rozpoznávání řeči pro 19 národních prostředí pro průměrné snížení četnosti chyb slov 18,6% (ES-ES, ES-MX, fr-CA, fr-FR, IT-IT, ja-JP, ko-KR, pt-BR, zh-CN, zh-HK, NB-NO, Fi-FL, ru-RU, pl-PL, CA-ES, zh-TW, th-TH, pt-PT, tr-TR). Nové modely přináší významná vylepšení napříč více doménami včetně diktování, přepisu a scénářů indexování videa.

**Opravy chyb**

 - Opravená chyba, kde konverzace Transcriber v rozhraní JAVA API nečekala správně 
 - Oprava emulátoru Androidu x86 pro [problém GitHubu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363) pro Xamarin
 - Přidat chybějící (Get | Set) vlastnosti metod na AudioConfig
 - Oprava chyby TTS, kde audioDataStream nebylo možné zastavit, pokud selhalo připojení
 - Použití koncového bodu bez oblasti způsobí selhání USP pro překladatele konverzace.
 - Generování ID v univerzálních aplikacích pro Windows teď používá vhodně jedinečný algoritmus GUID; dřív a záměrně neúmyslně nastavily implementaci podložit, která často produkuje kolizí v rámci velkých sad interakcí.
 
 **ukázky**
 
 - Ukázka Unity pro použití sady Speech SDK s [mikrofonem Unity a streamování přes nabízený režim](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone)

**Další změny**

 - [Dokumentace ke konfiguraci OpenSSL se aktualizovala pro Linux.](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)

## <a name="speech-sdk-190-2020-january-release"></a>Sada Speech SDK 1.9.0:2020 – leden Release

**Nové funkce**

- Konverzace s více zařízeními: umožňuje propojit více zařízení se stejnou hlasovou nebo textovou konverzací a volitelně překládat zprávy odesílané mezi nimi. Další informace najdete v [tomto článku](multi-device-conversation.md). 
- Podpora rozpoznávání klíčových slov se přidala pro balíček Android. AAR a přidala se podpora pro typy x86 a x64. 
- Cíl-C: `SendMessage` a `SetMessageProperty` metody přidané do `Connection` objektu. Další informace [najdete v dokumentaci.](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxconnection)
- Rozhraní TTS C++ API teď podporuje `std::wstring` jako Shrnutí textu, takže před předáním do sady SDK je potřeba převést wstring na řetězec. [Tady](https://docs.microsoft.com/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync)najdete podrobnosti. 
- C#: [ID jazyka](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-csharp) a [konfigurace zdrojového jazyka](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-specify-source-language?pivots=programming-language-csharp) jsou nyní k dispozici.
- JavaScript: Přidání funkce do `Connection` objektu pro předání vlastní zprávy ze služby Speech jako zpětného volání `receivedServiceMessage` .
- JavaScript: Přidání podpory pro pro `FromHost API` usnadnění používání s Prem kontejnery a svrchovanými cloudy. Další informace [najdete v dokumentaci.](speech-container-howto.md)
- JavaScript: nyní jsme se spojili `NODE_TLS_REJECT_UNAUTHORIZED` s příspěvkem z [orgads](https://github.com/orgads). [Tady](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75)najdete podrobnosti.

**Změny způsobující chyby**

- `OpenSSL`byla aktualizována na verzi 1.1.1 b a staticky propojená se základní knihovnou sady Speech SDK pro Linux. To může způsobit přerušení, pokud vaše Doručená pošta nebyla `OpenSSL` nainstalována do `/usr/lib/ssl` adresáře v systému. Pokud chcete tento problém obejít, přečtěte si [naši dokumentaci](how-to-configure-openssl-linux.md) v části dokumentace k sadě Speech SDK.
- Změnili jsme datový typ vrácený pro C# `WordLevelTimingResult.Offset` z `int` na, `long` aby bylo umožněno přístupu k `WordLevelTimingResults` datům, když jsou data řeči delší než 2 minuty.
- `PushAudioInputStream`a `PullAudioInputStream` teď pošle informace o hlavičce WAV do služby pro rozpoznávání řeči na základě `AudioStreamFormat` , volitelně zadané při jejich vytvoření. Zákazníci teď musí používat [podporovaný formát zvukového vstupu](how-to-use-audio-input-streams.md). Všechny ostatní formáty získají dílčí optimální výsledky rozpoznávání nebo můžou způsobit jiné problémy. 

**Opravy chyb**

- Podívejte se na `OpenSSL` aktualizaci v části nejnovější změny výše. V systému Linux a Java jsme opravili občasné chyby a problémy s výkonem (při vysokém zatížení zamknete spory). 
- Java: vylepšení uzavření objektu ve scénářích s vysokou mírou souběžnosti.
- Znovu se strukturuje náš balíček NuGet. Odebrali jsme tři kopie `Microsoft.CognitiveServices.Speech.core.dll` a `Microsoft.CognitiveServices.Speech.extension.kws.dll` složky lib, čímž se balíček NuGet zmenší a rychleji stáhne a přidali jsme hlavičky potřebné ke kompilaci některých nativních aplikací C++.
- Pevné ukázky pro rychlý Start [najdete tady](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp). Ty se ukončily bez zobrazení výjimky "mikrofon nebyl nalezen" na Linux, macOS a Windows.
- Opravili jsme chybu sady SDK s dlouhým výsledkem rozpoznávání řeči na určitých cestách kódu, jako je [Tato ukázka](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp).
- Opravená chyba nasazení sady SDK ve službě Azure Web App Environment pro vyřešení [tohoto problému se zákazníky](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396).
- Opravili jsme chybu TTS při použití vícenásobné `<voice>` značky nebo `<audio>` značky k vyřešení [tohoto problému zákazníka](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433). 
- Opravená chyba TTS 401 při obnovení sady SDK z pozastavené.
- JavaScript: Děkujeme, že se vám jako příspěvek z [Euirim](https://github.com/euirim)opravil cyklický import zvukových dat. 
- JavaScript: Přidání podpory pro nastavení vlastností služby, jak je přidáno v 1,7.
- JavaScript: Opravili jsme problém, kdy by chyba připojení mohla způsobit průběžné pokusy o opětovné připojení k soketu WebSocket.

**ukázky**

- [Sem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo)přidejte ukázku rozpoznávání klíčových slov pro Android.
- [Sem](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs)přidejte ukázku TTS pro scénář serveru.
- [Sem](quickstarts/multi-device-conversation.md)jste přidali rychlý Start konverzace s více zařízeními pro jazyky C# a C++.

**Další změny**

- Optimalizovaná velikost knihovny SDK jádra v Androidu
- Sada SDK v 1.9.0 a vyšší podporuje typy i `int` `string` v poli verze hlasového podpisu pro Transcriber pro konverzaci.

## <a name="speech-sdk-180-2019-november-release"></a>Sada Speech SDK 1.8.0:2019 – listopadová verze

**Nové funkce**

- Přidali jsme `FromHost()` rozhraní API, které můžete používat s kontejnery Prem a svrchovanými cloudy.
- Přidání automatického zdrojového Rozpoznávání jazyka pro rozpoznávání řeči (v jazycích Java a C++)
- Přidání `SourceLanguageConfig` objektu pro rozpoznávání řeči, pomocí kterého se určí očekávané zdrojové jazyky (v jazycích Java a C++)
- Přidání `KeywordRecognizer` podpory pro Windows (UWP), Android a iOS prostřednictvím balíčků NuGet a Unity
- Bylo přidáno rozhraní API vzdálené konverzace Java, které umožní přepis konverzace v asynchronních dávkách.

**Změny způsobující chyby**

- Funkce konverzace Transcriber přesunuté do oboru názvů `Microsoft.CognitiveServices.Speech.Transcription` .
- Součást metod konverzace Transcriber je přesunuta do nové `Conversation` třídy.
- Vyřazena podpora pro 32-bit (ARMv7 a x86) iOS

**Opravy chyb**

- Opravit pro chybu, pokud `KeywordRecognizer` je místní použití bez platného klíče předplatného služby Speech

**ukázky**

- Ukázka Xamarin pro`KeywordRecognizer`
- Ukázka Unity pro`KeywordRecognizer`
- Ukázky pro automatické zdrojové Rozpoznávání jazyka v jazyce C++ a Java

## <a name="speech-sdk-170-2019-september-release"></a>Sada Speech SDK 1.7.0:2019 – září verze

**Nové funkce**

- Přidání podpory beta verze pro Xamarin on Univerzální platforma Windows (UWP), Android a iOS
- Přidala se podpora iOS pro Unity.
- Přidání `Compressed` podpory vstupu pro ALaw, mulaw, FLAC v Androidu, iOS a Linux
- Přidáno `SendMessageAsync` do `Connection` třídy pro odeslání zprávy službě
- Přidáno `SetMessageProperty` do `Connection` třídy pro vlastnost nastavení zprávy
- TTS – přidané vazby pro Java (JRE a Android), Python, SWIFT a cíl-C
- TTS přidal podporu přehrávání pro macOS, iOS a Android.
- Přidání informací o hranici slova pro TTS

**Opravy chyb**

- Problém s opravenou IL2CPP sestavení v Unity 2019 pro Android
- Opravený problém se nesprávně zpracovávanými nepoškozenými záhlavími ve vstupním souboru WAV
- Opravený problém s identifikátory UUID není jedinečný v některých vlastnostech připojení.
- Opravili jsme několik upozornění na specifikátory hodnoty null v vazbách SWIFT (může vyžadovat malé změny kódu).
- Opravili jsme chybu, která způsobila, že připojení protokolu WebSocket se v zatížení sítě nekorektně zavřou.
- Opravili jsme problém v Androidu, který někdy má za následek duplicitní ID dojmů, které používá.`DialogServiceConnector`
- Vylepšení stability připojení v rámci vícenásobných interakcí a hlášení selhání (prostřednictvím `Canceled` událostí), když k nim dojde`DialogServiceConnector`
- `DialogServiceConnector`spuštění relace nyní bude správně poskytovat události, včetně při volání `ListenOnceAsync()` během aktivní`StartKeywordRecognitionAsync()`
- Vyřešená havárie přidružená k `DialogServiceConnector` přijatým aktivitám

**ukázky**

- Rychlý Start pro Xamarin
- Aktualizovaný rychlý Start pro CPP s ARM64 informacemi pro Linux
- Aktualizovaný rychlý Start Unity s informacemi pro iOS

## <a name="speech-sdk-160-2019-june-release"></a>Sada Speech SDK 1.6.0:2019 – červenou verzi

**ukázky**

- Ukázky rychlý Start pro převod textu na řeč pro UWP a Unity
- Ukázka rychlého startu pro SWIFT v iOS
- Ukázky Unity pro rozpoznávání řeči & Rozpoznávání záměru a překlad
- Aktualizované ukázky pro rychlý Start pro`DialogServiceConnector`

**Vylepšení/změny**

- Obor názvů dialogového okna:
  - Přejmenování `SpeechBotConnector` na `DialogServiceConnector`
  - Přejmenování `BotConfig` na `DialogServiceConfig`
  - `BotConfig::FromChannelSecret()`bylo přemapováno na`DialogServiceConfig::FromBotSecret()`
  - Po přejmenování budou dál podporovány všechny existující klienty s přímým ovládáním řeči.
- Aktualizace adaptéru pro převod textu na řeč pro podporu proxy a trvalého připojení
- Vylepšit chybovou zprávu, když je předána neplatná oblast
- SWIFT/objektiv-C:
  - Vylepšené zasílání zpráv o chybách: metody, které mohou mít za následek chybu, jsou nyní k dispozici ve dvou verzích: jeden, který zpřístupňuje `NSError` objekt pro zpracování chyb, a druhý, který vyvolává výjimku. Bývalé jsou zpřístupněny SWIFT. Tato změna vyžaduje úpravu stávajícího kódu SWIFT.
  - Vylepšené zpracování událostí

**Opravy chyb**

- Oprava pro TTS: místo, kde `SpeakTextAsync` se v budoucnu vrátilo, dokud se nedokončí vykreslování zvuku
- Oprava pro zařazování řetězců v jazyce C# pro povolení plné jazykové podpory
- Oprava problému aplikace .NET Core pro načtení základní knihovny s cílovou architekturou net461 v ukázkách
- Oprava pro příležitostné problémy s nasazováním nativních knihoven do výstupní složky v ukázkách
- Oprava pro spolehlivou zavírání webového soketu
- Oprava pro možnou chybu při otevírání připojení v rámci velmi velkého zatížení systému Linux
- Oprava chybějících metadat v sadě rozhraní pro macOS
- Oprava pro problémy s `pip install --user` Windows

## <a name="speech-sdk-151"></a>Sada Speech SDK 1.5.1

Toto je vydání opravy chyby a ovlivňuje pouze nativní/spravované sady SDK. Nemá vliv na verzi JavaScriptu sady SDK.

**Opravy chyb**

- Opravte FromSubscription při použití s přepisem konverzace.
- Opravte chybu v klíčovém slovu hledání pro hlasové asistenty.

## <a name="speech-sdk-150-2019-may-release"></a>Sada Speech SDK 1.5.0:2019 – může vydávat

**Nové funkce**

- Klíčové slovo hledání (KWS) je teď k dispozici pro Windows a Linux. Funkce KWS můžou pracovat s jakýmkoli typem mikrofonu. oficiální podpora KWS se ale v současné době omezila na pole mikrofonu, která najdete v sadě Azure Kinect DK hardware nebo v sadě Speech SDK (Speech Devices SDK).
- Funkce pomocných parametrů fráze je dostupná prostřednictvím sady SDK. Další informace najdete [tady](how-to-phrase-lists.md).
- Funkce přepisu konverzace je k dispozici prostřednictvím sady SDK. Podívejte se [sem](conversation-transcription-service.md).
- Přidejte podporu pro hlasové asistenty pomocí kanálu Direct line Speech.

**ukázky**

- Byly přidány ukázky pro nové funkce nebo nové služby, které sada SDK podporuje.

**Vylepšení/změny**

- Přidání různých vlastností nástroje pro rozpoznávání pro úpravu chování služby nebo výsledků služby (například vulgárních výrazů a dalších).
- Nástroj pro rozpoznávání teď můžete nakonfigurovat pomocí standardních vlastností konfigurace, a to i v případě, že jste vytvořili Nástroj pro rozpoznávání `FromEndpoint` .
- Cíl-C: `OutputFormat` vlastnost byla přidána do `SPXSpeechConfiguration` .
- Sada SDK teď podporuje Debian 9 jako distribuci Linux.

**Opravy chyb**

- Opravili jsme problém, kdy byl prostředek mluvčíe ve fázi převodu textu na řeč příliš nestrukturované.

## <a name="speech-sdk-142"></a>Sada Speech SDK 1.4.2

Toto je vydání opravy chyby a ovlivňuje pouze nativní/spravované sady SDK. Nemá vliv na verzi JavaScriptu sady SDK.

## <a name="speech-sdk-141"></a>Sada Speech SDK 1.4.1

Toto je verze jenom pro JavaScript. Nepřidaly se žádné funkce. Byly provedeny následující opravy:

- Zabrání webovému balíčku načíst agenta HTTPS-proxy-.

## <a name="speech-sdk-140-2019-april-release"></a>Sada Speech SDK 1.4.0:2019 – duben

**Nové funkce**

- Sada SDK teď podporuje službu převodu textu na řeč jako beta verzi. Podporuje se v desktopech Windows a Linux z C++ a C#. Další informace najdete v [přehledu převod textu na řeč](text-to-speech.md#get-started).
- Sada SDK teď podporuje zvukové soubory MP3 a Opus/OGG jako vstupní soubory streamu. Tato funkce je k dispozici pouze pro Linux z C++ a C# a je momentálně ve verzi beta (další podrobnosti najdete [tady](how-to-use-codec-compressed-audio-input-streams.md)).
- Sada Speech SDK pro jazyky Java, .NET Core, C++ a objektivní-C získala podporu macOS. Podpora pro macOS je v současnosti ve verzi beta.
- iOS: sada Speech SDK pro iOS (cíl-C) je teď publikovaná taky jako CocoaPod.
- JavaScript: podpora pro jiný než výchozí mikrofon jako vstupní zařízení.
- JavaScript: podpora proxy serveru pro Node.js.

**ukázky**

- Byly přidány ukázky pro použití sady Speech SDK s C++ a s cílem-C v macOS.
- Byly přidány ukázky demonstrující použití služby převodu textu na řeč.

**Vylepšení/změny**

- Python: další vlastnosti výsledků rozpoznávání jsou nyní zpřístupněny prostřednictvím `properties` Vlastnosti.
- Pro další podporu vývoje a ladění můžete přesměrovat informace o protokolování a diagnostice sady SDK do souboru protokolu (další podrobnosti najdete [tady](how-to-use-logging.md)).
- JavaScript: zlepšení výkonu zpracování zvuku.

**Opravy chyb**

- Mac/iOS: Chyba, která vedla k dlouhému čekání v případě, že nebylo možné navázat spojení se službou Speech.
- Python: zlepšení zpracování chyb pro argumenty v zpětných voláních Pythonu.
- JavaScript: Opraveno chybné vytváření sestav stavu pro rozpoznávání řeči skončilo v RequestSession.

## <a name="speech-sdk-131-2019-february-refresh"></a>Sada Speech SDK 1.3.1:2019 – aktualizace z února

Toto je vydání opravy chyby a ovlivňuje pouze nativní/spravované sady SDK. Nemá vliv na verzi JavaScriptu sady SDK.

**Oprava chyby**

- Při použití vstupu mikrofonu byla vyřešena nevracení paměti. Na základě datového proudu nebo vstupu souboru není ovlivněno.

## <a name="speech-sdk-130-2019-february-release"></a>Sada Speech SDK 1.3.0:2019 – Únorová verze

**Nové funkce**

- Sada Speech SDK podporuje výběr vstupního mikrofonu skrze `AudioConfig` třídu. To vám umožní streamovat zvuková data do služby pro rozpoznávání řeči z jiného než výchozího mikrofonu. Další informace najdete v dokumentaci popisující [Výběr zvukového vstupního zařízení](how-to-select-audio-input-devices.md). Tato funkce není ještě dostupná z JavaScriptu.
- Sada Speech SDK teď ve verzi beta verze podporuje Unity. Poskytněte zpětnou vazbu v části problém v [ukázkovém úložišti GitHubu](https://aka.ms/csspeech/samples). Tato verze podporuje Unity v systémech Windows x86 a x64 (desktopové nebo Univerzální platforma Windows aplikace) a Androidu (ARM32/64, x86). Další informace najdete v našem [rychlém](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity)startu pro Unity.
- Soubor `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (dodán v předchozích verzích) už není potřeba. Tato funkce je teď integrovaná do základní sady SDK.

**ukázky**

V našem [ukázkovém úložišti](https://aka.ms/csspeech/samples)je k dispozici následující nový obsah:

- Další ukázky pro `AudioConfig.FromMicrophoneInput` .
- Další ukázky Pythonu pro rozpoznávání a překlad záměrů.
- Další ukázky pro používání `Connection` objektu v iOS
- Další ukázky Java pro překlad pomocí zvukového výstupu.
- Nová ukázka použití [dávkového přepisu REST API](batch-transcription.md).

**Vylepšení/změny**

- Python
  - Vylepšené ověřování parametrů a chybové zprávy v `SpeechConfig` .
  - Přidejte podporu `Connection` objektu.
  - Podpora 32-bitové verze Pythonu (x86) ve Windows.
  - Sada Speech SDK for Python je mimo verzi beta.
- iOS
  - Sada SDK je teď založená na sadě iOS SDK verze 12,1.
  - Sada SDK teď podporuje iOS verze 9,2 a novější.
  - Vylepšete referenční dokumentaci a opravte několik názvů vlastností.
- JavaScript
  - Přidejte podporu `Connection` objektu.
  - Přidat soubory definice typu pro seskupené JavaScripty
  - Počáteční podpora a implementace pro pomocný parametr fráze
  - Návratová kolekce vlastností s JSON služby pro rozpoznávání
- Knihovny DLL systému Windows teď obsahují prostředek verze.
- Pokud vytvoříte Nástroj pro rozpoznávání `FromEndpoint` , můžete přidat parametry přímo k adrese URL koncového bodu. Pomocí `FromEndpoint` standardních vlastností konfigurace nemůžete konfigurovat nástroj pro rozpoznávání.

**Opravy chyb**

- Prázdné uživatelské jméno proxy serveru a heslo proxy serveru nebyly zpracovány správně. Pokud v této verzi nastavíte uživatelské jméno a heslo proxy serveru na prázdný řetězec, nebudou odeslány při připojování k proxy serveru.
- Identifikátor SessionId, který sada SDK vytvořil, nebyl vždycky pro některé jazyky &nbsp; nebo prostředí vždy náhodný. K vyřešení tohoto problému se přidala inicializace náhodného generátoru.
- Vylepšete zpracování autorizačního tokenu. Pokud chcete použít autorizační token, zadejte v `SpeechConfig` a ponechte klíč předplatného prázdný. Pak vytvořte Nástroj pro rozpoznávání obvyklým způsobem.
- V některých případech se `Connection` objekt nevydal správně. Tento problém byl opraven.
- Ukázka JavaScriptu byla opravena tak, aby podporovala zvukový výstup pro syntézu překladu i v Safari.

## <a name="speech-sdk-121"></a>Sada Speech SDK 1.2.1

Toto je verze jenom pro JavaScript. Nepřidaly se žádné funkce. Byly provedeny následující opravy:

- Požárový konec streamu. konec, ne na řeč. end.
- Oprava chyby v zvukové pumpě, která neplánovala další odeslání, pokud se aktuální odeslání nepovedlo.
- Opravte průběžné rozpoznávání pomocí ověřovacího tokenu.
- Oprava chyb pro různé funkce pro rozpoznávání nebo koncové body.
- Vylepšení dokumentace.

## <a name="speech-sdk-120-2018-december-release"></a>Sada Speech SDK 1.2.0:2018 – prosinec verze

**Nové funkce**

- Python
  - V této verzi je k dispozici beta verze podpory Pythonu (3,5 a novější). Další informace naleznete zde] (rychlý Start – python.md).
- JavaScript
  - Sada Speech SDK pro JavaScript je open source. Zdrojový kód je k dispozici na [GitHubu](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  - Nyní podporujeme Node.js, další informace najdete [tady](quickstart-js-node.md).
  - Omezení délky pro zvukové relace bylo odebráno, k automatickému připojení dojde v rámci pokrytí.
- `Connection`předmětů
  - Z `Recognizer` , můžete získat přístup k `Connection` objektu. Tento objekt vám umožňuje explicitně iniciovat připojení služby a přihlásit se k odběru událostí a odpojovat se.
    (Tato funkce ještě není dostupná z JavaScriptu a Pythonu.)
- Podpora pro Ubuntu 18,04.
- Android
  - Povolená podpora ProGuard během generování APK

**Vylepšen**

- Vylepšení využití interního vlákna, což snižuje počet vláken, zámků a mutexů.
- Vylepšené zasílání zpráv o chybách/informace. V několika případech se chybové zprávy nerozšířily tak, jak jsou.
- Aktualizované závislosti vývoje v JavaScriptu pro použití aktuálnosti modulů.

**Opravy chyb**

- Pevná nevracení paměti kvůli neshodě typu v `RecognizeAsync` .
- V některých případech došlo k úniku výjimek.
- Oprava nevrácené paměti v argumentech události překladu.
- Opravili jsme problém se zámkem při opětovném připojení v dlouho spuštěných relacích.
- Opravili jsme problém, který by mohl vést k chybějícímu konečnému výsledku pro neúspěšné překlady.
- C#: Pokud `async` v hlavním vlákně nečekala operace, je možné, že by se modul pro rozpoznávání odstranil před dokončením asynchronní úlohy.
- Java: Opravili jsme problém, což vedlo k chybě virtuálního počítače Java.
- Cíl-C: pevné mapování výčtu; Místo bylo vráceno RecognizedIntent `RecognizingIntent` .
- JavaScript: Nastavte výchozí výstupní formát na možnost "jednoduchý" v `SpeechConfig` .
- JavaScript: odebrání nekonzistence mezi vlastnostmi objektu konfigurace v jazyce JavaScript a dalších jazycích.

**ukázky**

- Aktualizovali a opravili jsme několik ukázek (například výstupní hlasy pro překlad atd.).
- Do [ukázkového úložiště](https://aka.ms/csspeech/samples)se přidaly Node.js ukázky.

## <a name="speech-sdk-110"></a>Sada Speech SDK 1.1.0

**Nové funkce**

- Podpora pro Android x86/x64.
- Podpora proxy serveru: v `SpeechConfig` objektu nyní můžete zavolat funkci pro nastavení informací o proxy serveru (název hostitele, port, uživatelské jméno a heslo). Tato funkce není ještě k dispozici v iOS.
- Vylepšený kód chyby a zprávy. Pokud rozpoznávání vrátilo chybu, již `Reason` bylo nastaveno (ve zrušené události) nebo `CancellationDetails` (ve výsledku rozpoznávání) na `Error` . Zrušená událost teď obsahuje dva další členy `ErrorCode` a `ErrorDetails` . Pokud server vrátil Další informace o chybě s nahlášenou chybou, bude nyní k dispozici v nových členech.

**Vylepšen**

- Do konfigurace nástroje pro rozpoznávání se přidalo další ověření a přidala se další chybová zpráva.
- Vylepšené zpracování dlouhodobě tichého podoby uprostřed zvukového souboru.
- Balíček NuGet: u .NET Frameworkch projektů brání sestavování s konfigurací AnyCPU.

**Opravy chyb**

- V nástroji pro rozpoznávání se našla několik výjimek. Kromě toho jsou výjimky zachyceny a převedeny na `Canceled` událost.
- Opravte nevrácení paměti ve správě vlastností.
- Opravili jsme chybu, ve které by zvukový vstupní soubor mohl způsobit selhání nástroje pro rozpoznávání.
- Opravili jsme chybu, kdy se události dostaly po události ukončení relace.
- Opravili jsme některé konflikty časování ve vláknování.
- Opravili jsme problém s kompatibilitou iOS, který by mohl způsobit chybu.
- Vylepšení stability pro podporu mikrofonu Androidu.
- Opravili jsme chybu, kdy by nástroj pro rozpoznávání v JavaScriptu ignoroval jazyk rozpoznávání.
- Opravili jsme chybu, která brání v nastavení `EndpointId` (v některých případech) v JavaScriptu.
- Změnili jsme pořadí parametrů v AddIntent v JavaScriptu a Přidali jste chybějící `AddIntent` podpis JavaScriptu.

**ukázky**

- Přidány ukázky jazyka C++ a C# pro použití pro vyžádání obsahu a nabízené vysílání do [vzorového úložiště](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Sada Speech SDK 1.0.1

Vylepšení spolehlivosti a opravy chyb:

- Opravená potenciální závažná chyba kvůli konfliktu časování v nástroji pro rozpoznávání disposing
- Opravená potenciální závažná chyba pro případ zrušení vlastností
- Přidala se další chyba a kontrola parametrů.
- Cíl-C: Opravili jsme možnou závažnou chybu způsobenou přepsáním názvu v NSString.
- Cíl-C: upravená viditelnost rozhraní API
- JavaScript: opravené související události a jejich datové části.
- Vylepšení dokumentace.

V našem [ukázkovém úložišti](https://aka.ms/csspeech/samples)se přidala Nová ukázka pro JavaScript.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Cognitive Services Speech SDK 1.0.0:2018 – září verze

**Nové funkce**

- Podpora pro cíl-C v iOS. Podívejte se na náš rychlý úvodní úkol [v jazyce C pro iOS](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md).
- Podpora JavaScriptu v prohlížeči Podívejte se na náš [rychlý úvodní JavaScript](quickstart-js-browser.md).

**Změny způsobující chyby**

- V této verzi se zavádí určitý počet nezměněných změn.
  Podrobnosti najdete na [této stránce](https://aka.ms/csspeech/breakingchanges_1_0_0) .

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Cognitive Services Speech SDK 0.6.0:2018 – srpen Release

**Nové funkce**

- Aplikace pro UWP vytvořené pomocí sady Speech SDK teď můžou předat sadu Windows App Certification Kit (WACK).
  Podívejte se na [rychlé zprovoznění UWP](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp).
- Podpora pro .NET Standard 2,0 na platformě Linux (Ubuntu 16,04 x64).
- Experimentální: podporují Java 8 v systému Windows (64 bitů) a Linux (Ubuntu 16,04 x64).
  Podívejte se na [rychlý úvodní Java Runtime Environment](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre).

**Změna funkčnosti**

- Vystavení dalších podrobností o chybách při chybě připojení

**Změny způsobující chyby**

- V jazyce Java (Android) `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` již funkce nevyžaduje parametr cesty. Tato cesta se teď automaticky detekuje na všech podporovaných platformách.
- Přístupový objekt get vlastnosti `EndpointUrl` v jazycích Java a C# byl odebrán.

**Opravy chyb**

- V jazyce Java je nyní implementována výsledná syntéza zvuku na nástroji pro rozpoznávání překladu.
- Opravili jsme chybu, která by mohla způsobit neaktivní vlákna a zvýšený počet otevřených a nevyužitých soketů.
- Opravili jsme problém, kdy dlouhodobě běžící rozpoznávání může končit uprostřed přenosu.
- Opravili jste konflikt časování při vypnutí nástroje pro rozpoznávání.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Cognitive Services Speech SDK 0.5.0:2018 – červenec verze

**Nové funkce**

- Podpora platformy Android (rozhraní API 23: Android 6,0 Marshmallow nebo novější). Podívejte se na [rychlý Start pro Android](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android).
- Podpora .NET Standard 2,0 ve Windows. Podívejte se na [rychlé zprovoznění .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore).
- Experimentální: podporují UWP ve Windows (verze 1709 nebo novější).
  - Podívejte se na [rychlé zprovoznění UWP](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp).
  - Poznámka: aplikace pro UWP vytvořené pomocí sady Speech SDK ještě nepředá certifikační sadu aplikací pro Windows (WACK).
- Podpora dlouhotrvajícího rozpoznávání při automatickém opětovném připojení.

**Funkční změny**

- `StartContinuousRecognitionAsync()`podporuje dlouhodobě běžící rozpoznávání.
- Výsledek rozpoznávání obsahuje více polí. Jsou posunuty od zahájení zvuku a doby trvání (v tiků) rozpoznaného textu a dalších hodnot, které představují stav rozpoznávání, například `InitialSilenceTimeout` a `InitialBabbleTimeout` .
- Podporuje AuthorizationToken pro vytváření instancí továrny.

**Změny způsobující chyby**

- Události rozpoznávání: `NoMatch` Typ události byl sloučen do `Error` události.
- SpeechOutputFormat v jazyce C# byla přejmenována na `OutputFormat` , aby zůstala v souladu s C++.
- Návratový typ některých metod `AudioInputStream` rozhraní se mírně změnil:
  - V jazyce Java se `read` Metoda nyní vrací `long` místo `int` .
  - V jazyce C# se `Read` Metoda nyní vrací `uint` místo `int` .
  - V jazyce C++ se `Read` `GetFormat` nyní vrátí metody a `size_t` místo `int` .
- C++: instance zvukových vstupních streamů se teď dají předat jenom jako `shared_ptr` .

**Opravy chyb**

- Opravily se nesprávné návratové hodnoty v důsledku vypršení `RecognizeAsync()` časového limitu.
- Došlo k odebrání závislosti v knihovnách Media Foundation v systému Windows. SDK teď používá základní zvuková rozhraní API.
- Oprava dokumentace: Přidání stránky [oblastí](regions.md) pro popis podporovaných oblastí.

**Známý problém**

- Sada Speech SDK pro Android nehlásí výsledky Shrnutí řeči pro překlad. Tento problém bude vyřešen v příští verzi.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitive Services Speech SDK 0.4.0:2018 – červená verze

**Funkční změny**

- AudioInputStream

  Nástroj pro rozpoznávání teď může jako zdroj zvuku spotřebovat datový proud. Další informace najdete v části související [Průvodce](how-to-use-audio-input-streams.md).

- Podrobný výstupní formát

  Když vytvoříte `SpeechRecognizer` , můžete požadovat `Detailed` nebo `Simple` výstupní formát. `DetailedSpeechRecognitionResult`Obsahuje hodnocení spolehlivosti, rozpoznaný text, nezpracovaný lexikální tvar, normalizovanou formu a normalizovanou formu s maskovánými vulgárními výrazy.

**Zásadní změna**

- Změněno na `SpeechRecognitionResult.Text` z `SpeechRecognitionResult.RecognizedText` jazyka C#.

**Opravy chyb**

- Opravili jsme možný problém zpětného volání ve vrstvě USP během vypínání.
- Pokud nástroj pro rozpoznávání použil zvukový vstupní soubor, byl uložený na popisovači souboru déle, než je nutné.
- Bylo odebráno několik zablokování mezi čerpadlem zpráv a modulem pro rozpoznávání.
- Spustí `NoMatch` výsledek, když vypršel časový limit odpovědi na službu.
- Knihovny Media Foundation v systému Windows jsou načteny opožděně. Tato knihovna se vyžaduje jenom pro vstup přes mikrofon.
- Rychlost nahrávání zvukových dat je omezená na přibližně dvojnásobek původní rychlosti zvuku.
- V systému Windows jsou nyní sestavení C# .NET silně pojmenována.
- Oprava dokumentace: `Region` jsou požadovány informace k vytvoření nástroje pro rozpoznávání.

Přidali jsme další ukázky a neustále se aktualizují. Nejnovější sadu ukázek najdete v části [sada Speech SDK Samples v úložišti GitHub](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitive Services Speech SDK 0.2.12733:2018-může vydávat

Tato verze je první verze Public Preview sady Cognitive Services Speech SDK.
