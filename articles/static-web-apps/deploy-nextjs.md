---
title: 'Kurz: nasazení serveru – vykreslování dalších webů. js ve službě Azure static Web Apps'
description: Vygenerujte a nasaďte dynamické weby Next. js pomocí statického Web Apps Azure.
services: static-web-apps
author: christiannwamba
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/08/2020
ms.author: chnwamba
ms.openlocfilehash: fe139921cb73ee0e224c995e2dd5eb5fc50f3979
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599851"
---
# <a name="deploy-server-rendered-nextjs-websites-on-azure-static-web-apps-preview"></a>Nasazení serveru – vykreslených dalších webů. js ve službě Azure static Web Apps Preview

V tomto kurzu se naučíte nasadit statický web vygenerovaný pomocí aplikace [Next. js](https://nextjs.org) do služby [Azure static Web Apps](overview.md). Pokud chcete začít, naučíte se, jak nastavit, nakonfigurovat a nasadit další aplikaci. js. Během tohoto procesu se naučíte také řešit běžné výzvy, které se často vyskytují při generování statických stránek pomocí nástroje Next. js.

## <a name="prerequisites"></a>Požadavky

- Účet Azure s aktivním předplatným. [Vytvořte si účet zdarma](https://azure.microsoft.com/free/).
- Účet GitHub. [Vytvořte si účet zdarma](https://github.com/join).
- Nainstalovaný jazyk [Node.js](https://nodejs.org).

## <a name="set-up-a-nextjs-app"></a>Nastavení aplikace Next. js

Místo použití dalšího rozhraní příkazového řádku. js k vytvoření aplikace můžete použít počáteční úložiště, které obsahuje existující aplikaci Next. js. Toto úložiště obsahuje aplikaci Next. js s dynamickými trasami, které zvýrazňují běžný problém s nasazením. Dynamické trasy vyžadují další konfiguraci nasazení, o které se v průběhu chvilky naučíte.

Začněte vytvořením nového úložiště v rámci účtu GitHub z úložiště šablon. 

1. Přejít na<http://github.com/staticwebdev/nextjs-starter/generate>
1. Pojmenování úložiště **nextjs-Starter**
1. Pak na svém počítači naklonujte nové úložiště. Ujistěte se, že `<YOUR_GITHUB_ACCOUNT_NAME>` jste nahradili názvem vašeho účtu.

    ```bash
    git clone http://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/nextjs-starter
    ```

1. Přejděte k nově naklonované aplikaci Next. js:

   ```bash
   cd nextjs-starter
   ```

1. Závislosti instalace:

    ```bash
    npm install
    ```

1. Spustit aplikaci Next. js ve vývoji:

    ```bash
    npm run dev
    ```

Přejděte na adresu <http://localhost:3000> a otevřete aplikaci, kde by se měl zobrazit následující web v upřednostňovaném prohlížeči:

:::image type="content" source="media/deploy-nextjs/start-nextjs-app.png" alt-text="Spustit aplikaci Next. js":::

Když kliknete na architekturu nebo knihovnu, měla by se zobrazit Stránka s podrobnostmi o vybrané položce:

:::image type="content" source="media/deploy-nextjs/start-nextjs-details.png" alt-text="Stránka podrobností":::

## <a name="generate-a-static-website-from-nextjs-build"></a>Generování statického webu z buildu Next. js

Při sestavování serveru Next. js pomocí nástroje `npm run build` je aplikace sestavena jako tradiční webová aplikace, ne jako statická lokalita. Chcete-li vygenerovat statickou lokalitu, použijte následující konfiguraci aplikace.

1. Chcete-li nakonfigurovat statické trasy, vytvořte soubor s názvem _Next. config. js_ v kořenovém adresáři projektu a přidejte následující kód.

    ```javascript
    module.exports = {
      exportTrailingSlash: true,
      exportPathMap: function() {
        return {
          '/': { page: '/' }
        };
      }
    };
    ```
    
      Tato konfigurace mapuje `/` na další stránku. js, která je obsluhována pro `/` trasu a která je stránkovací soubor _pages/index. js_ .

1. Aktualizujte skript sestavení _Package. JSON_tak, aby po sestavení vygeneroval i statický web pomocí `next export` příkazu. `export`Příkaz vygeneruje statickou lokalitu.

    ```json
    "scripts": {
      "dev": "next dev",
      "build": "next build && next export",
    },
    ```

    Když je tento příkaz na místě, statický Web Apps spustí `build` skript při každém vložení potvrzení změn.

1. Generovat statický web:

    ```bash
    npm run build
    ```

    Statický web se vygeneruje a zkopíruje do složky _out_ v kořenu pracovního adresáře.

    > [!NOTE]
    > Tato složka je uvedena v souboru _. gitignore_ , protože při nasazení by měla být generována pomocí CI/CD.

## <a name="push-your-static-website-to-github"></a>Vložení statického webu do GitHubu

Azure static Web Apps nasadí vaši aplikaci z úložiště GitHubu a zachová se tak pro všechna vložená potvrzení do určené větve. Pomocí následujících příkazů Synchronizujte změny na GitHubu.

1. Všechny změněné soubory fáze

    ```bash
    git add .
    ```

1. Potvrdit všechny změny

    ```bash
    git commit -m "Update build config"
    ```

1. Doručovat změny do GitHubu.

    ```bash
    git push origin master
    ```

## <a name="deploy-your-static-website"></a>Nasazení statického webu

Následující kroky ukazují, jak propojit aplikaci, kterou jste právě odeslali do GitHubu, do služby Azure static Web Apps. V Azure můžete aplikaci nasadit do produkčního prostředí.

### <a name="create-a-static-app"></a>Vytvoření statické aplikace

1. Přejít na [Azure Portal](https://portal.azure.com)
1. Klikněte na **vytvořit prostředek** .
1. Hledání **statického Web Apps**
1. Klikněte na **statické Web Apps (Preview)** .
1. Klikněte na **vytvořit** .

1. V rozevíracím seznamu *předplatné* vyberte předplatné nebo použijte výchozí hodnotu.
1. V rozevíracím seznamu *Skupina prostředků* klikněte na **Nový** odkaz. Do *nového názvu skupiny prostředků*zadejte **mystaticsite** a klikněte na **OK** .
1. Do textového pole **název** zadejte globálně jedinečný název vaší aplikace. Mezi platné znaky patří `a-z` , `A-Z` , `0-9` a `-` . Tato hodnota se používá jako předpona adresy URL vaší statické aplikace ve formátu `https://<APP_NAME>.azurestaticapps.net` .
1. V rozevíracím seznamu *oblast* vyberte oblast, která je pro vás nejblíže.
1. V rozevíracím seznamu SKU vyberte **volné** .

   :::image type="content" source="media/deploy-nextjs/create-static-web-app.png" alt-text="Vytvoření statické webové aplikace":::

### <a name="add-a-github-repository"></a>Přidat úložiště GitHub

Nový účet statického Web Apps potřebuje k úložišti přístup pomocí aplikace Next. js, aby mohl automaticky nasadit potvrzení změn.

1. Klikněte na **tlačítko Přihlásit se pomocí GitHubu** .
1. Vyberte **organizaci** , ve které jste úložiště vytvořili pro svůj projekt Next. js, což může být vaše uživatelské jméno GitHubu.
1. Vyhledejte a vyberte název úložiště, které jste vytvořili dříve.
1. Z rozevíracího seznamu *větev* vyberte možnost **Hlavní** jako větev.

   :::image type="content" source="media/deploy-nextjs/connect-github.png" alt-text="Připojení GitHubu":::

### <a name="configure-the-build-process"></a>Konfigurace procesu sestavení

Statická Web Apps Azure je sestavená tak, aby automaticky provedla běžné úkoly, jako je instalace modulů npm a spuštění `npm run build` během každého nasazení. Existuje však několik nastavení, jako je zdrojová složka aplikace a cílová složka pro sestavení, které je třeba nakonfigurovat ručně.

1. Chcete-li nakonfigurovat statickou výstupní složku, klikněte na kartu **sestavení** .

   :::image type="content" source="media/deploy-nextjs/build-tab.png" alt-text="Karta sestavení":::

2. Zadejte **text** do textového pole *umístění artefaktu aplikace* .

### <a name="review-and-create"></a>Zkontrolovat a vytvořit

1. Kliknutím na tlačítko **Revize + vytvořit** ověřte správnost podrobností.
1. Kliknutím na **vytvořit** zahájíte vytváření prostředku a zároveň zajistěte akci GitHubu pro nasazení.
1. Po dokončení nasazení klikněte na **Přejít k prostředku** .
1. V okně _Přehled_ klikněte na odkaz *URL* a otevřete nasazenou aplikaci. 

Pokud si web nahraje hned hned, je pracovní postup akcí GitHubu na pozadí stále spuštěný. Po dokončení pracovního postupu můžete kliknutím na aktualizovat prohlížeč zobrazit webovou aplikaci.
Pokud si web nahraje hned hned, je pracovní postup akcí GitHubu na pozadí stále spuštěný. Po dokončení pracovního postupu můžete kliknutím na aktualizovat prohlížeč zobrazit webovou aplikaci.

Stav pracovních postupů akcí můžete zjistit tak, že přejdete k akcím pro vaše úložiště:

```url
https://github.com/<YOUR_GITHUB_USERNAME>/nextjs-starter/actions
```

### <a name="sync-changes"></a>Synchronizovat změny

Při vytváření aplikace se v úložišti Azure static Web Apps vytvořil soubor pracovního postupu akcí GitHubu. Tento soubor budete muset přenést do místního úložiště, aby se synchronizoval historie Gitu.

Vraťte se do terminálu a spusťte následující příkaz `git pull origin master` .

## <a name="configure-dynamic-routes"></a>Konfigurace dynamických tras

Přejděte k nově nasazenému webu a klikněte na jedno z log rozhraní nebo loga knihovny. Místo toho, abyste získali stránku s podrobnostmi, dostanete chybovou stránku 404.

:::image type="content" source="media/deploy-nextjs/404-in-production.png" alt-text="404 na dynamických trasách":::

Důvodem této chyby je, že soubor Next. js vygeneroval pouze domovskou stránku na základě konfigurace aplikace.

## <a name="generate-static-pages-from-dynamic-routes"></a>Generování statických stránek z dynamických tras

1. Aktualizujte soubor _Next. config. js_ tak, aby soubor Next. js používal seznam všech dostupných dat pro generování statických stránek pro každé rozhraní a knihovnu:

   ```javascript
   const data = require('./utils/projectsData');

   module.exports = {
     exportTrailingSlash: true,
     exportPathMap: async function () {
       const { projects } = data;
       const paths = {
         '/': { page: '/' },
       };
  
       projects.forEach((project) => {
         paths[`/project/${project.slug}`] = {
           page: '/project/[path]',
           query: { path: project.slug },
         };
       });
  
       return paths;
     },
   };
   ```

   > [!NOTE]
   > `exportPathMap`Funkce je asynchronní funkce, takže můžete vytvořit požadavek na rozhraní API v této funkci a použít vrácený seznam k vygenerování cest.

2. Nahrajte nové změny do úložiště GitHubu a počkejte pár minut, než akce GitHubu znovu vytvoří váš web. Po dokončení sestavení se zobrazí chyba 404.

   :::image type="content" source="media/deploy-nextjs/404-in-production-fixed.png" alt-text="404 na pevných dynamických trasách":::

> [!div class="nextstepaction"]
> [Nastavení vlastní domény](custom-domain.md)
