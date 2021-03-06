---
title: Použití Azure Mapsho vykreslování – Vizualizér chyb
description: V tomto článku se dozvíte, jak vizualizovat upozornění a chyby vrácené rozhraním API pro převod Creator.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: b3f9451a5ffd13c67232107d8db1e2da4a3891ec
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524739"
---
# <a name="using-the-azure-maps-drawing-error-visualizer"></a>Použití Azure Mapsho vykreslování – Vizualizér chyb

Vizualizér chyb při vykreslování je samostatná webová aplikace, která zobrazuje [Upozornění a chyby balíčku pro vykreslování](drawing-conversion-error-codes.md) během procesu převodu. Webová aplikace Vizualizér chyb se skládá ze statické stránky, kterou můžete použít bez připojení k Internetu.  K opravě chyb a upozornění podle [požadavků balíčku pro vykreslování](drawing-requirements.md)můžete použít Vizualizér chyb. [Rozhraní API pro převod Azure Maps](https://docs.microsoft.com/rest/api/maps/conversion) vrátí odpověď s odkazem na Vizualizér chyb pouze v případě, že je zjištěna chyba.

## <a name="prerequisites"></a>Předpoklady

Předtím, než si můžete stáhnout Vizualizér chyba vykreslování, budete potřebovat:

1. [Vytvoření účtu Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account)
2. [Získejte primární klíč předplatného](quick-demo-map-app.md#get-the-primary-key-for-your-account), označovaný také jako primární klíč nebo klíč předplatného.
3. [Vytvoření prostředku autora](how-to-manage-creator.md)

V tomto kurzu se používá aplikace [po](https://www.postman.com/) aplikaci, ale můžete zvolit jiné vývojové prostředí API.

## <a name="download"></a>Stáhnout

1. Nahrajte balíček pro kreslení do služby Azure Maps Creator, abyste získali `udid` pro nahraný balíček. Postup nahrání balíčku najdete v tématu [nahrání balíčku pro kreslení](tutorial-creator-indoor-maps.md#upload-a-drawing-package).

2. Teď, když se balíček pro kreslení nahraje, použijeme `udid` pro nahráný balíček k převedení balíčku na data mapy. Postup pro převod balíčku najdete v tématu [Převod balíčku pro kreslení](tutorial-creator-indoor-maps.md#convert-a-drawing-package).

    >[!NOTE]
    >Pokud je váš proces převodu úspěšný, nebudete dostávat odkaz na nástroj Vizualizér chyb.

3. Na kartě **hlavičky** odpovědi v aplikaci pro odesílání vyhledejte `diagnosticPackageLocation` vlastnost, kterou vrátí rozhraní API pro převod. Odpověď by měla vypadat podobně jako následující kód JSON:

    ```json
    {
        "operationId": "77dc9262-d3b8-4e32-b65d-74d785b53504",
        "created": "2020-04-22T19:39:54.9518496+00:00",
        "status": "Failed",
        "resourceLocation": "https://atlas.microsoft.com/conversion/{conversionId}?api-version=1.0",
        "properties": {
            "diagnosticPackageLocation": "https://atlas.microsoft.com/mapData/ce61c3c1-faa8-75b7-349f-d863f6523748?api-version=1.0"
        }
    }
    ```

4. Stáhněte si `HTTP-GET` požadavek na adresu URL a Stáhněte si Vizualizér chyb balíčku pro kreslení `diagnosticPackageLocation` .

## <a name="setup"></a>Nastavení

V rámci staženého balíčku zip z tohoto `diagnosticPackageLocation` odkazu najdete dva soubory.

* _VisualizationTool.zip_: obsahuje zdrojový kód, médium a webovou stránku pro Vizualizér chyb při vykreslování.
* _ConversionWarningsAndErrors.js_: obsahuje formátovaný seznam upozornění, chyby a další podrobnosti, které jsou používány Vizualizérm chyb kreslení.

Rozbalte složku _VisualizationTool.zip_ . Obsahuje následující položky:

* Složka _assets_ : obsahuje obrázky a mediální soubory
* _statická_ složka: zdrojový kód
* Soubor _index.html_ : webová aplikace

Otevřete soubor _index.html_ pomocí kteréhokoli z následujících prohlížečů s příslušným číslem verze. Je možné použít jinou verzi, pokud verze nabízí stejně kompatibilní chování jako uvedená verze.

* Microsoft Edge 80
* Safari 13
* Chrome 80
* Firefox 74

## <a name="using-the-drawing-error-visualizer-tool"></a>Používání nástroje pro Vizualizér chyb při vykreslování

Po spuštění nástroje pro Vizualizér chyb při vykreslování se zobrazí stránka pro nahrání. Stránka pro nahrání obsahuje pole pro přetažení. Rozevírací pole přetažení & funguje také jako tlačítko, které spustí dialogové okno Průzkumníka souborů.

:::image type="content" source="./media/drawing-errors-visualizer/start-page.png" alt-text="Vykreslování aplikace Vizualizér chyb – Úvodní stránka":::

_ConversionWarningsAndErrors.jsv_ souboru byl umístěn v kořenovém adresáři staženého adresáře. Pokud chcete načíst _ConversionWarningsAndErrors.js_ , můžete buď pře& táhnout soubor do pole, nebo kliknout na pole, najít soubor v dialogovém okně Průzkumníka souborů a pak tento soubor nahrát.

:::image type="content" source="./media/drawing-errors-visualizer/loading-data.gif" alt-text="Kreslení aplikace Vizualizér chyb – přetáhnutím a přetažením načtěte data":::

Po načtení _ConversionWarningsAndErrors.js_ souboru se zobrazí seznam chyb a upozornění balíčku pro kreslení. Jednotlivé chyby nebo upozornění jsou určeny vrstvou, úrovní a podrobnou zprávou. Chcete-li zobrazit podrobné informace o chybě nebo upozornění, klikněte na odkaz **Podrobnosti** . Nerušivý oddíl se pak zobrazí pod seznamem. Nyní můžete přejít k jednotlivým chybám a získat další informace o tom, jak chybu vyřešit.

:::image type="content" source="./media/drawing-errors-visualizer/errors.png" alt-text="Vykreslování aplikace Vizualizér chyb – chyby a upozornění":::

## <a name="next-steps"></a>Další kroky

Jakmile [balíček pro vykreslování splňuje požadavky](drawing-requirements.md), můžete použít [službu Azure Maps DataSet](https://docs.microsoft.com/rest/api/maps/conversion) k převodu balíčku pro kreslení na datovou sadu. Pak můžete k vývoji aplikace použít webový modul vnitřních map. Další informace najdete v následujících článcích:

> [!div class="nextstepaction"]
> [Vykreslení kódů chyb převodu](drawing-conversion-error-codes.md)

> [!div class="nextstepaction"]
> [Autor pro mapy vnitřníchy](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Použití modulu mapy Vnitřníchy](how-to-use-indoor-module.md)

> [!div class="nextstepaction"]
> [Implementovat dynamické styly vnitřní mapy](indoor-map-dynamic-styling.md)