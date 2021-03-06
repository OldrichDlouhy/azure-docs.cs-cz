---
title: 'Kurz: použití vlastního detektoru loga k rozpoznávání služeb Azure – Custom Vision'
titleSuffix: Azure Cognitive Services
description: V tomto kurzu provedete ukázkovou aplikaci, která používá Custom Vision jako součást scénáře detekce loga. Přečtěte si, jak se používá Custom Vision s dalšími komponentami k zajištění ucelené aplikace.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 2bd17e10e6123c48087116e947dc21f4cf788ef3
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/18/2020
ms.locfileid: "84987862"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Kurz: rozpoznání loga služeb Azure v obrázcích fotoaparátu

V tomto kurzu se seznámíte s ukázkovou aplikací, která používá Custom Vision jako součást většího scénáře. Aplikace pro vizuální zřizování AI, aplikace Xamarin. Forms pro mobilní platformy, analyzuje obrázky kamery s logy služby Azure a pak nasadí skutečné služby na účet Azure uživatele. V tomto článku se dozvíte, jak používá Custom Vision ve spolupráci s dalšími komponentami k zajištění užitečné ucelené aplikace. Můžete spustit celý scénář aplikace pro sebe nebo můžete dokončit pouze Custom Vision část nastavení a prozkoumat, jak ji aplikace používá.

V tomto kurzu se dozvíte, jak:

> [!div class="checklist"]
> - Vytvořte vlastní objektový detektor pro rozpoznávání log Services Azure.
> - Připojte svoji aplikaci k Azure Počítačové zpracování obrazu a Custom Vision.
> - Vytvoření účtu instančního objektu Azure pro nasazení služeb Azure z aplikace

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/cognitive-services/), ještě než začnete. 

## <a name="prerequisites"></a>Požadavky

- [Visual Studio 2017 nebo novější](https://www.visualstudio.com/downloads/)
- Úlohy Xamarin pro Visual Studio (viz [instalace Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- Emulátor pro iOS nebo Android pro Visual Studio
- Rozhraní příkazového [řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (volitelné)

## <a name="get-the-source-code"></a>Získání zdrojového kódu

Pokud chcete použít poskytnutou webovou aplikaci, naklonujte nebo Stáhněte zdrojový kód aplikace z úložiště AI pro [Visual Provisioning](https://github.com/Microsoft/AIVisualProvision) na GitHubu. Otevřete soubor *source/VisualProvision. sln* v aplikaci Visual Studio. Později upravíte některé soubory projektu, abyste mohli aplikaci spustit.

## <a name="create-an-object-detector"></a>Vytvoření detektoru objektu

Přihlaste se k [webu Custom Vision](https://customvision.ai/) a vytvořte nový projekt. Zadejte projekt rozpoznávání objektů a použijte doménu loga; Tato akce umožní službě používat algoritmus optimalizovaný pro detekci loga. 

![Okno New-Project na webu Custom Vision v prohlížeči Chrome](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Nahrávání a označování obrázků

V dalším kroku nahlaste algoritmus detekce loga nahráváním imagí loga služeb Azure a jejich ručním označením. Úložiště AIVisualProvision zahrnuje sadu školicích imagí, které můžete použít. Na webu vyberte tlačítko **Přidat obrázky** na kartě **školicích imagí** . Pak přejdete do složky **Documents/images/Training_DataSet** v úložišti. Je nutné ručně označit loga v každém obrázku, takže pokud testujete pouze testování tohoto projektu, můžete chtít nahrát pouze podmnožinu imagí. Nahrajte aspoň 15 instancí každé značky, kterou plánujete použít.

Po nahrání školicích snímků vyberte na displeji první. Zobrazí se okno označování. Nakreslete pole a přiřaďte značky pro každé logo v každém obrázku. 

![Označení loga na webu Custom Vision](media/azure-logo-tutorial/tag-logos.png)

Aplikace je nakonfigurovaná tak, aby fungovala s konkrétními řetězci značek. Definice najdete v souboru *Source\VisualProvision\Services\Recognition\RecognitionService.cs* :

[!code-csharp[Tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?name=snippet_constants)]

Až označíte obrázek, načtěte vpravo a označte ho jako další. Po dokončení zavřete okno označování.

## <a name="train-the-object-detector"></a>Naučit se detektor objektů

V levém podokně nastavte **značky** na **označené** , aby se zobrazily obrázky. Pak vyberte zelené tlačítko v horní části stránky a prohlaste model. Algoritmus bude vlakem rozpoznávat stejné značky v nových obrázcích. Také otestuje model u některých z vašich stávajících imagí, aby vygenerovala hodnocení přesnosti.

![Web Custom Vision na kartě školicích imagí. Na tomto snímku obrazovky je tlačítko Výuka obrysové.](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Získat adresu URL předpovědi

Po vyškolení modelu jste připraveni ho integrovat do své aplikace. Budete muset získat adresu URL koncového bodu (adresu vašeho modelu, který bude aplikace dotazovat) a klíč předpovědi (pro udělení přístupu aplikace k požadavkům předpovědi). Na kartě **výkon** v horní části stránky vyberte tlačítko **Adresa URL předpovědi** .

![Web Custom Vision zobrazující okno prediktivního rozhraní API, které zobrazuje adresu URL a klíč rozhraní API](media/azure-logo-tutorial/cusvis-endpoint.png)

Zkopírujte adresu URL koncového bodu a hodnotu **prediktivního klíče** do příslušných polí v souboru *Source\VisualProvision\AppSettings.cs* :

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_cusvis_keys)]

## <a name="examine-custom-vision-usage"></a>Kontrola využití Custom Vision

Otevřete soubor *source/VisualProvision/Services/Recognition/CustomVisionService. cs* , abyste viděli, jak aplikace používá vaši Custom Vision klíč a adresu URL koncového bodu. Metoda **PredictImageContentsAsync** přebírá bajtový datový proud obrázkového souboru spolu s tokenem zrušení (pro správu asynchronních úloh), volá rozhraní API prediktivního Custom Vision a vrátí výsledek předpovědi. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?name=snippet_prediction)]

Tento výsledek má podobu **PredictionResult** instance, která obsahuje seznam **předpovědních** instancí. **Předpověď** obsahuje zjištěnou značku a její umístění ohraničovacího rámečku v obrázku.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?name=snippet_prediction_class)]

Pokud chcete získat další informace o tom, jak aplikace zpracovává Tato data, začněte metodou **GetResourcesAsync** . Tato metoda je definována v souboru *source/VisualProvision/Services/Recognition/RecognitionService. cs* .  

## <a name="add-computer-vision"></a>Přidat Počítačové zpracování obrazu

Custom Vision část tohoto kurzu jste dokončili. Pokud chcete aplikaci spustit, bude potřeba integrovat i službu Počítačové zpracování obrazu. Aplikace používá funkci rozpoznávání textu Počítačové zpracování obrazu k doplnění procesu zjišťování loga. Logo Azure může rozpoznat jeho vzhled *nebo* text, který se v blízkosti něj tiskne. Na rozdíl od Custom Vision modelů je Počítačové zpracování obrazu předvedený, aby na obrázcích a videích prováděl určité operace.

Přihlaste se k odběru služby Počítačové zpracování obrazu, abyste získali adresu URL klíče a koncového bodu. Nápovědu k tomuto kroku najdete v tématu [Jak získat klíče předplatného](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe).

![Služba Počítačové zpracování obrazu v Azure Portal s vybranou nabídkou pro rychlý Start. Odkaz na klíče je popsaný, jak je adresa URL koncového bodu rozhraní API.](media/azure-logo-tutorial/comvis-keys.png)

Potom otevřete soubor *Source\VisualProvision\AppSettings.cs* a naplňte `ComputerVisionEndpoint` `ComputerVisionKey` proměnné a pomocí správných hodnot.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_comvis_keys)]

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu

K nasazení služeb do předplatného Azure vyžaduje aplikace účet instančního objektu Azure. Instanční objekt umožňuje delegovat specifická oprávnění k aplikaci pomocí řízení přístupu na základě rolí. Další informace najdete v tématu [Průvodce instančními objekty služby](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals).

Instanční objekt můžete vytvořit pomocí Azure Cloud Shell nebo rozhraní příkazového řádku Azure CLI, jak je znázorněno zde. Začněte tím, že se přihlásíte a vyberete předplatné, které chcete použít.

```azurecli
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Pak vytvořte instanční objekt. (Dokončení tohoto procesu může nějakou dobu trvat.)

```azurecli
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

Po úspěšném dokončení by se měl zobrazit následující výstup JSON, včetně nezbytných přihlašovacích údajů.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```

Poznamenejte si `clientId` hodnoty a `tenantId` . Přidejte je do příslušných polí v souboru *Source\VisualProvision\AppSettings.cs* .

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?name=snippet_serviceprincipal)]

## <a name="run-the-app"></a>Spuštění aplikace

V tuto chvíli jste přidali přístup k aplikaci:

- Trained Custom Vision model
- Služba Počítačové zpracování obrazu
- Hlavní účet služby

Pomocí těchto kroků spusťte aplikaci:

1. V aplikaci Visual Studio Průzkumník řešení vyberte buď projekt **VisualProvision. Android** , nebo projekt **VisualProvision. iOS** . V rozevírací nabídce na hlavním panelu nástrojů vyberte odpovídající emulátor nebo připojené mobilní zařízení. Pak aplikaci spusťte.

    > [!NOTE]
    > Pro spuštění emulátoru iOS budete potřebovat zařízení s MacOS.

1. Na první obrazovce zadejte ID klienta instančního objektu, ID tenanta a heslo. Vyberte tlačítko pro **přihlášení** .

    > [!NOTE]
    > V některých emulátorech se v tomto kroku nemusí aktivovat tlačítko pro **přihlášení** . Pokud k tomu dojde, zastavte aplikaci, otevřete soubor *source/VisualProvision/Pages/LoginPage. XAML* , najděte `Button` element s označením **Login**, odeberte následující řádek a pak znovu spusťte aplikaci.
    >  ```xaml
    >  IsEnabled="{Binding IsValid}"
    >  ```
    
    ![Obrazovka aplikace zobrazující pole pro přihlašovací údaje instančního objektu](media/azure-logo-tutorial/app-credentials.png)

1. Na další obrazovce vyberte předplatné Azure z rozevírací nabídky. (Tato nabídka by měla obsahovat všechna předplatná, ke kterým má objekt služby přístup.) Klikněte na tlačítko **pokračovat** . V tomto okamžiku se může zobrazit výzva k udělení přístupu k fotoaparátu a úložišti fotografií zařízení. Udělte přístupová oprávnění.

    ![Obrazovka aplikace, ve které se zobrazuje rozevírací pole pro cílové předplatné Azure](media/azure-logo-tutorial/app-az-subscription.png)


1. Fotoaparát v zařízení se aktivuje. Povezměte si fotografii některého z poučených log služby Azure. Okno nasazení by vás mělo vyzvat k výběru oblasti a skupiny prostředků pro nové služby (jak byste to provedli, pokud je nasazujete z Azure Portal). 

    ![Obrazovka kamery smartphone zaměřená na dva Vystřižení loga Azure](media/azure-logo-tutorial/app-camera-capture.png)

    ![Obrazovka aplikace zobrazující pole pro oblast nasazení a skupinu prostředků](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud jste postupovali podle všech kroků v tomto scénáři a použili jste aplikaci k nasazení služeb Azure do svého účtu, pokračujte na [Azure Portal](https://ms.portal.azure.com/). Tam můžete zrušit služby, které nechcete používat.

Pokud plánujete vytvořit vlastní projekt rozpoznávání objektů pomocí Custom Vision, možná budete chtít odstranit projekt detekce loga, který jste vytvořili v tomto kurzu. Bezplatné předplatné pro Custom Vision umožňuje pouze dva projekty. Pokud chcete odstranit projekt zjišťování loga, na [webu Custom Vision](https://customvision.ai)otevřete **projekty** a potom vyberte ikonu odpadkového koše v části **můj nový projekt**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu nastavíte a prozkoumáte plnohodnotnou aplikaci Xamarin. Forms, která používá službu Custom Vision k detekci loga v obrazech mobilní kamery. V dalším kroku se dozvíte, jaké jsou osvědčené postupy pro vytváření Custom Vision modelu, takže když ho vytvoříte pro vlastní aplikaci, můžete ho nastavit jako účinný a přesný.

> [!div class="nextstepaction"]
> [Jak vylepšit třídění](getting-started-improving-your-classifier.md)
