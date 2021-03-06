---
title: Zřizování starších zařízení pomocí symetrických klíčů – Azure IoT Hub Device Provisioning Service
description: Jak pomocí symetrických klíčů zřídit starší verze zařízení s instancí služby Device Provisioning Service (DPS)
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 4d1a92f3ebf32d2270eb77ec9c79fe860ba090e1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75434713"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Jak zřídit starší zařízení pomocí symetrických klíčů

Běžný problém s mnoha staršími zařízeními je, že často mají identitu, která se skládá z jedné části informací. Tato informace o identitě je obvykle adresa MAC nebo sériové číslo. Starší zařízení nemusí obsahovat certifikát, čip TPM ani žádnou jinou funkci zabezpečení, která se dá použít k bezpečné identifikaci zařízení. Služba Device Provisioning pro službu IoT Hub zahrnuje ověření symetrického klíče. Ověření identity symetrického klíče se dá použít k identifikaci zařízení na základě informací, jako je adresa MAC nebo sériové číslo.

Pokud můžete snadno nainstalovat [modul hardwarového zabezpečení (HSM)](concepts-security.md#hardware-security-module) a certifikát, může to být lepší přístup k identifikaci a zřizování vašich zařízení. Vzhledem k tomu, že tento přístup vám může dovolit obejít aktualizaci kódu nasazeného na všechna vaše zařízení a nebudete mít v imagi zařízení vložený tajný klíč.

V tomto článku se předpokládá, že ani modul HARDWAROVÉho zabezpečení nebo certifikát není možnost životaschopnosti. Předpokládá se ale, že máte nějakou metodu aktualizace kódu zařízení, abyste mohli tato zařízení zřídit pomocí služby Device Provisioning. 

Tento článek také předpokládá, že se aktualizace zařízení provádí v zabezpečeném prostředí, aby se zabránilo neoprávněnému přístupu k klíči hlavní skupiny nebo odvozenému klíči zařízení.

Tento článek je orientovaný na pracovní stanici s Windows. Stejným postupem se však můžete řídit i na Linuxu. Příklad pro Linux najdete v článku o [zřizování architektury s více tenanty](how-to-provision-multitenant.md).

> [!NOTE]
> Vzorek použitý v tomto článku je napsán v jazyce C. K dispozici je také [ukázkový vzorový klíč zřízení zařízení v C#](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/SymmetricKeySample) . Pokud chcete použít tuto ukázku, Stáhněte nebo naklonujte úložiště [Azure-IoT-Samples-CSharp](https://github.com/Azure-Samples/azure-iot-samples-csharp) a postupujte podle pokynů v tomto ukázkovém kódu. Podle pokynů v tomto článku můžete vytvořit skupinu pro zápis symetrického klíče pomocí portálu a najít rozsah ID a primární a sekundární klíče pro spuštění ukázky. Pomocí ukázky můžete také vytvořit jednotlivé registrace.

## <a name="overview"></a>Přehled

Na základě informací, které toto zařízení identifikuje, bude pro každé zařízení definováno jedinečné ID registrace. Například adresa MAC nebo sériové číslo.

Ve službě Device Provisioning se vytvoří skupina pro registraci, která používá [symetrický klíč ověření identity](concepts-symmetric-key-attestation.md) . Skupina pro registraci bude obsahovat hlavní klíč skupiny. Tento hlavní klíč se použije k výpočtu hodnoty hash každého jedinečného ID registrace a vytvoří jedinečný klíč zařízení pro každé zařízení. Zařízení použije tento odvozený klíč zařízení s jedinečným ID registrace k ověření pomocí služby Device Provisioning a přiřadí se ke službě IoT Hub.

Kód zařízení, který je znázorněn v tomto článku, bude postupovat stejným způsobem jako [rychlý Start: zřízení simulovaného zařízení s symetrickými klíči](quick-create-simulated-device-symm-key.md). Kód bude simulovat zařízení pomocí ukázky ze [sady Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). Simulované zařízení se potvrdí pomocí skupiny registrací místo jednotlivé registrace, jak je znázorněno v rychlém startu.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Požadavky

* Dokončení [nastavení IoT Hub Device Provisioning Service pomocí](./quick-setup-auto-provision.md) nástroje pro rychlý Start Azure Portal

Následující požadavky jsou pro vývojové prostředí systému Windows. Informace o systému Linux nebo macOS najdete v příslušné části [Příprava vývojového prostředí](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) v dokumentaci k sadě SDK.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 se zapnutou úlohou [vývoj desktopových aplikací v jazyce C++](https://docs.microsoft.com/cpp/?view=vs-2019#pivot=workloads) . Podporují se také sady Visual Studio 2015 a Visual Studio 2017.

* Nainstalovaná nejnovější verze [Gitu](https://git-scm.com/download/)

## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Příprava vývojového prostředí Azure IoT C SDK

V této části připravíte vývojové prostředí použité k sestavení [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). 

Sada SDK obsahuje vzorový kód pro simulované zařízení. Toto simulované zařízení se pokusí zřídit během spouštěcí sekvence zařízení.

1. Stáhněte si [sestavovací systém cmake](https://cmake.org/download/).

    Je důležité, aby požadavky na sadu Visual Studio (Visual Studio a sada funkcí Vývoj desktopových aplikací pomocí C++) byly na vašem počítači nainstalované ještě **před** zahájením instalace `CMake`. Jakmile jsou požadované součásti k dispozici a stažený soubor je ověřený, nainstalujte sestavovací systém CMake.

2. Vyhledejte název značky pro [nejnovější verzi](https://github.com/Azure/azure-iot-sdk-c/releases/latest) sady SDK.

3. Otevřete prostředí příkazového řádku nebo Git Bash. Spuštěním následujících příkazů naklonujte nejnovější verzi úložiště GitHub pro [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) . Použijte značku, kterou jste našli v předchozím kroku, jako hodnotu `-b` parametru:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Buďte připravení na to, že může trvat i několik minut, než se tato operace dokončí.

4. V kořenovém adresáři úložiště Git vytvořte podadresář `cmake` a přejděte do této složky. Z adresáře spusťte následující příkazy `azure-iot-sdk-c` :

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. Spuštěním následujícího příkazu sestavte verzi sady SDK určenou pro platformu vašeho vývojového klienta. V adresáři `cmake` se vygeneruje řešení Visual Studia pro simulované zařízení. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```
    
    Pokud `cmake` nenajde váš kompilátor C++, můžou se při spuštění výše uvedeného příkazu zobrazit chyby sestavení. Pokud k tomu dojde, zkuste tento příkaz spustit v [příkazovém řádku sady Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Po úspěšném sestavení by posledních pár řádků výstupu mělo vypadat přibližně takto:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-a-symmetric-key-enrollment-group"></a>Vytvořit skupinu pro zápis symetrického klíče

1. Přihlaste se k [Azure Portal](https://portal.azure.com)a otevřete instanci služby Device Provisioning.

2. Vyberte kartu **spravovat registrace** a pak klikněte na tlačítko **Přidat skupinu** registrací v horní části stránky. 

3. Do pole **Přidat skupinu**registrací zadejte následující informace a klikněte na tlačítko **Uložit** .

   - **Název skupiny**: zadejte **mylegacydevices**.

   - **Typ ověření identity**: vyberte **symetrický klíč**.

   - **Automaticky vygenerovat klíče**: Toto políčko zaškrtněte.

   - **Vyberte, jak chcete přiřadit zařízení k**centrům: vyberte **statická konfigurace** , abyste se mohli přiřadit k určitému centru.

   - **Vyberte centra IoT, do kterých se dá tato skupina přiřadit**: vyberte jednu z vašich Center.

     ![Přidat skupinu registrace pro ověření symetrického klíče](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Po uložení registrace se vygeneruje **Primární klíč** a **Sekundární klíč** a tyto klíče se přidají do položky registrace. Skupina pro registraci symetrického klíče se zobrazí jako **mylegacydevices** ve sloupci *název skupiny* na kartě *skupiny* registrací. 

    Otevřete registraci a zkopírujte hodnotu vygenerovaného **primárního klíče**. Tento klíč je klíčem hlavní skupiny.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Zvolit jedinečný registrační identifikátor zařízení

Aby bylo možné identifikovat jednotlivá zařízení, musí být definováno jedinečné ID registrace. V zařízení můžete použít adresu MAC, sériové číslo nebo libovolné jedinečné informace. 

V tomto příkladu používáme kombinaci adresy MAC a sériového čísla, které tvoří následující řetězec pro ID registrace.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Vytvořte jedinečné ID registrace pro vaše zařízení. Platné znaky jsou malé alfanumerické znaky a spojovníky (-).


## <a name="derive-a-device-key"></a>Odvodit klíč zařízení 

Pokud chcete vygenerovat klíč zařízení, použijte hlavní klíč skupiny k výpočtu [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) jedinečného ID registrace zařízení a výsledek převeďte na Formát Base64.

Nezahrnujte hlavní klíč skupiny do kódu zařízení.


#### <a name="linux-workstations"></a>Pracovní stanice Linux

Pokud používáte pracovní stanici se systémem Linux, můžete použít OpenSSL k vygenerování odvozeného klíče zařízení, jak je znázorněno v následujícím příkladu.

Nahraďte hodnotu **klíče** **primárním klíčem** , který jste si poznamenali dříve.

Hodnotu **REG_ID** nahraďte ID registrace.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Pracovní stanice založené na systému Windows

Pokud používáte pracovní stanici se systémem Windows, můžete použít PowerShell k vygenerování odvozeného klíče zařízení, jak je znázorněno v následujícím příkladu.

Nahraďte hodnotu **klíče** **primárním klíčem** , který jste si poznamenali dříve.

Hodnotu **REG_ID** nahraďte ID registrace.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Zařízení použije odvozený klíč zařízení s jedinečným ID registrace a provede ověření symetrického klíče pomocí skupiny registrací během zřizování.



## <a name="create-a-device-image-to-provision"></a>Vytvoření image zařízení pro zřízení

V této části budete aktualizovat ukázku zřizování s názvem **prov \_ dev \_ Client \_ Sample** v sadě Azure IoT C SDK, kterou jste si nastavili dříve. 

Tento ukázkový kód simuluje spouštěcí sekvenci zařízení, která odesílá požadavek na zřízení do instance služby Device Provisioning. Spouštěcí sekvence způsobí, že se zařízení rozpozná a přiřadí ke službě IoT Hub, kterou jste nakonfigurovali ve skupině pro registraci.

1. Na webu Azure Portal vyberte okno **Přehled** vaší služby Device Provisioning Service a poznamenejte si hodnotu **_Rozsah ID_**.

    ![Extrahování informací o koncovém bodu služby Device Provisioning z okna portálu](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. V sadě Visual Studio otevřete soubor řešení **azure_iot_sdks. sln** , který byl vygenerován starším spuštěním cmake. Soubor řešení by se měl nacházet v následujícím umístění:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. V podokně *Průzkumník řešení* sady Visual Studio přejděte do složky **Provision\_Samples**. Rozbalte ukázkový projekt s názvem **prov\_dev\_client\_sample**. Rozbalte **zdrojové soubory** a otevřete **prov\_dev\_client\_sample.c**.

4. Najděte konstantu `id_scope` a nahraďte její hodnotu hodnotou **Rozsah ID**, kterou jste si zkopírovali. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Ve stejném souboru vyhledejte definici funkce `main()`. Zkontrolujte, jestli je proměnná `hsm_type` nastavená na hodnotu `SECURE_DEVICE_TYPE_SYMMETRIC_KEY`, jak je vidět dole:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Vyhledejte volání v části `prov_dev_set_symmetric_key_info()` **prov \_ dev \_ Client \_ Sample. c** , který je zakomentován.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    Odkomentujte volání funkce a nahraďte zástupné hodnoty (včetně lomených závorek) jedinečným ID registrace vašeho zařízení a vygenerovaným klíčem odvozeného zařízení.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6", "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=");
    ```
   
    Uložte soubor.

7. Klikněte pravým tlačítkem na projekt **prov\_dev\_client\_sample** a vyberte **Nastavit jako spouštěný projekt**. 

8. V nabídce sady Visual Studio vyberte **ladit**  >  **Spustit bez ladění** a spusťte řešení. Po zobrazení výzvy ke znovusestavení projektu klikněte na **Ano** a před spuštěním projekt znovu sestavte.

    Následující výstup je příkladem úspěšného spuštění simulovaného zařízení a připojení k instanci služby zřizování pro přiřazení k IoT Hubu:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

9. Na portálu přejděte do služby IoT Hub, ke které se simulované zařízení přiřadilo, a klikněte na kartu **zařízení IoT** . Po úspěšném zřízení simulovaného centra pro centrum se jeho ID zařízení zobrazí v okně **zařízení IoT** se *stavem* **povoleno**. Možná budete muset nahoře kliknout na tlačítko **Aktualizovat**. 

    ![Zařízení je zaregistrované u centra IoT](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Problematika zabezpečení

Mějte na paměti, že to nechá odvozený klíč zařízení zahrnutý jako součást obrázku, což není doporučený postup z hlediska zabezpečení. Toto je jeden z důvodů, proč zabezpečení a snadné použití jsou kompromisy. 





## <a name="next-steps"></a>Další kroky

* Další informace o opětovném zřízení najdete v tématu Koncepty opětovného [zřizování zařízení IoT Hub](concepts-device-reprovision.md) 
* [Rychlý start: Zřízení simulovaného zařízení se symetrickými klíči](quick-create-simulated-device-symm-key.md)
* Další informace o zrušení zřízení najdete v tématu [Postup zrušení zřízení zařízení, která byla dříve automaticky zřízena](how-to-unprovision-devices.md) . 











