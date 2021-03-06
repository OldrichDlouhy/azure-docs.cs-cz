---
title: Nastavení instance a ověřování (se skriptem)
titleSuffix: Azure Digital Twins
description: Podívejte se, jak nastavit instanci služby Azure Digital revlákens, včetně správného ověřování. Skriptovaná verze
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 522096b921faf34130f0c37f727d89c7bf95c530
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337904"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-scripted"></a>Nastavení instance a ověřování Azure pro digitální vlákna (skriptované)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Tento článek popisuje postup **Nastavení nové instance digitálního vlákna Azure**, včetně vytvoření instance a nastavení ověřování. Po dokončení tohoto článku budete mít instanci digitálních vláken Azure, která je připravená na zahájení programování.

Tato verze tohoto článku dokončí tento postup spuštěním [ukázky **skriptu automatizovaného nasazení** ](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/) , který zjednodušuje proces. Chcete-li zobrazit ruční kroky, pomocí kterých se skript spouští na pozadí, přečtěte si příručku k ruční verzi tohoto článku: [*Postupy: nastavení instance a ověřování (ruční)*](how-to-set-up-instance-manual.md).

[!INCLUDE [digital-twins-setup-starter.md](../../includes/digital-twins-setup-starter.md)]

## <a name="run-the-deployment-script"></a>Spuštění skriptu nasazení

V tomto článku se používá ukázka kódu pro digitální vlákna Azure, která umožňuje nasadit instanci digitálních vláken Azure a požadované ověřování částečně automaticky. Můžete ji také použít jako výchozí bod pro psaní vlastních skriptových interakcí.

Vzorový skript je napsán v prostředí PowerShell. Je součástí [ukázek digitálních vláken Azure](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/), které si můžete stáhnout do svého počítače tak, že přejdete na tento ukázkový odkaz a vyberete tlačítko *Stáhnout ZIP* pod nadpisem.

Ve složce stažené ukázkové složky se skript nasazení nachází na _Azure_Digital_Twins_samples.zip > skripty > **deploy.ps1** _.

Tady je postup, jak spustit skript nasazení v Cloud Shell.
1. V prohlížeči přejdete do okna [Azure Cloud Shell](https://shell.azure.com/) . Přihlaste se pomocí tohoto příkazu:
    ```azurecli-interactive
    az login
    ```
    Pokud rozhraní příkazového řádku může otevřít výchozí prohlížeč, bude to mít za následek a načíst přihlašovací stránku Azure. V opačném případě otevřete stránku prohlížeče na adrese *https://aka.ms/devicelogin* a zadejte autorizační kód zobrazený v terminálu.
 
2. Po přihlášení se podívejte na panel ikon Cloud Shellového okna. Vyberte ikonu Odeslat/stáhnout soubory a zvolte nahrát.

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell-upload.png" alt-text="Cloud Shell okno zobrazující výběr možnosti nahrání":::

    Na svém počítači přejděte na soubor _**deploy.ps1**_ a stiskněte otevřít. Tím se soubor nahraje do Cloud Shell, abyste ho mohli spustit v okně Cloud Shell.

3. Spusťte skript odesláním `./deploy.ps1` příkazu v okně Cloud Shell. Když se skript spustí pomocí automatizovaného postupu nastavení, budete vyzváni k předání těchto hodnot:
    * Pro instanci: *ID předplatného* vašeho předplatného Azure, které se má použít.
    * Pro instanci: *umístění* , kam chcete instanci nasadit. Pokud chcete zjistit, které oblasti podporují digitální vlákna Azure, přejděte na [*produkty Azure dostupné v jednotlivých oblastech*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
    * Pro instanci: název *skupiny prostředků* . Můžete použít existující skupinu prostředků nebo zadat nový název, který chcete vytvořit.
    * Pro instanci: *název* instance digitálního vlákna Azure. Název nové instance musí být v rámci vašeho předplatného jedinečný (to znamená, že pokud má vaše předplatné jinou instanci digitálních vláken Azure v oblasti, která už používá zvolený název, zobrazí se výzva k výběru jiného názvu).
    * Pro registraci aplikace: *Zobrazovaný název aplikace Azure AD* , který se má přidružit k registraci. V této registraci aplikace nakonfigurujete přístupová oprávnění k [rozhraním API pro digitální vlákna Azure](how-to-use-apis-sdks.md). Později se klientská aplikace ověří proti registraci aplikace a v důsledku toho se jim přidělí nakonfigurovaná přístupová oprávnění k rozhraním API.
    * Pro registraci aplikace: *Adresa URL pro odpověď aplikace Azure AD* pro aplikaci Azure AD. Můžete použít `http://localhost` .

Skript vytvoří instanci digitálních vláken Azure, přiřadí uživatele Azure, aby k instanci používala roli *Azure Digital Realiass Owner (Preview)* , a nastavení registrace aplikace Azure AD pro vaši klientskou aplikaci.

Tady je ukázka výstupního protokolu ze skriptu:

:::image type="content" source="media/how-to-set-up-instance/deployment-script-output.png" alt-text="Okno Cloud Shell zobrazující protokol vstupů a výstupů prostřednictvím spuštění skriptu nasazení" lightbox="media/how-to-set-up-instance/deployment-script-output.png":::

Pokud se skript úspěšně dokončí, vysloví se konečný výtisk `Deployment completed successfully` . Jinak vyřešte chybovou zprávu a znovu spusťte skript. Přeskočí kroky, které jste už dokončili, a začněte znovu požádat o vstup v místě, kde jste skončili.

Po dokončení skriptu teď máte k dispozici instanci digitálního vlákna Azure, která je připravená k použití, a oprávnění nastavená pro její správu.

## <a name="collect-important-values"></a>Shromažďovat důležité hodnoty

Existují dvě důležité hodnoty pro registraci aplikace, které budete potřebovat později k [ověření klientské aplikace s využitím rozhraní API pro digitální vlákna Azure](how-to-authenticate-client.md). 

Pokud je chcete najít, přejděte pomocí [tohoto odkazu](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) na stránku s přehledem registrace aplikace Azure AD v Azure Portal. Tato stránka zobrazuje všechny registrace aplikací, které byly vytvořeny v rámci vašeho předplatného.

Měla by se zobrazit registrace aplikace, kterou jste právě vytvořili v tomto seznamu. Vyberte ho a otevřete jeho podrobnosti:

:::image type="content" source="media/how-to-set-up-instance/app-important-values.png" alt-text="Zobrazení důležitých hodnot pro registraci aplikace na portálu":::

Poznamenejte si *ID aplikace (klienta)* a *ID adresáře (tenanta)* **zobrazené na stránce** . Pokud nejste osoba, která bude psát kód pro klientské aplikace, budete muset tyto hodnoty sdílet s osobou, která bude.

## <a name="verify-success"></a>Ověřit úspěch

Pokud chcete ověřit vytvoření prostředků a oprávnění nastavených skriptem, můžete si je prohlédnout v [Azure Portal](https://portal.azure.com).

### <a name="verify-instance"></a>Ověřit instanci

Pokud chcete ověřit, jestli se vaše instance vytvořila, na [stránce Azure Portal digitální vlákna](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) na webu Azure. Tato stránka obsahuje seznam všech instancí digitálních vláken Azure. V seznamu vyhledejte název nově vytvořené instance.

### <a name="verify-user-role-assignment"></a>Ověřit přiřazení role uživatele

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

### <a name="verify-app-registration"></a>Ověřit registraci aplikace

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

Nejdřív ověřte, že nastavení oprávnění pro digitální vlákna Azure byla v registraci správně nastavená. Provedete to tak, že v řádku nabídek vyberete *manifest* a zobrazí se kód manifestu registrace aplikace. Posuňte se do dolní části okna Code (kód) a vyhledejte tato pole v části `requiredResourceAccess` . Hodnoty by měly odpovídat hodnotám na snímku obrazovky níže:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

## <a name="other-possible-steps-for-your-organization"></a>Další možné kroky pro vaši organizaci

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Další kroky

Napsání ověřovacího kódu klientské aplikace najdete v tématu Postup připojení klientské aplikace k instanci.
* [*Postupy: psaní kódu ověřování aplikace*](how-to-authenticate-client.md)
