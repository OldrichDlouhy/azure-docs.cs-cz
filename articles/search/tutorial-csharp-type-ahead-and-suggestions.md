---
title: Kurz C# pro automatické dokončování a návrhy
titleSuffix: Azure Cognitive Search
description: Přidání automatického dokončování a návrhů ke shromáždění vstupu hledaného termínu od uživatelů pomocí rozevíracího seznamu Tento kurz sestaví na stávajícím projektu hotelů.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 07/15/2020
ms.openlocfilehash: 760624b06d00a873ff48c659ef65f9af62cd6454
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084020"
---
# <a name="tutorial-add-autocomplete-and-suggestions-using-the-net-sdk"></a>Kurz: přidání automatického dokončování a návrhů pomocí sady .NET SDK

Naučte se implementovat automatické dokončování (dotazy typeahead a navrhované dokumenty), když uživatel začne psát do vyhledávacího pole. V tomto kurzu zobrazíme samostatně dokončené dotazy a výsledky návrhů a pak dohromady. Uživatel může zadat jenom dva nebo tři znaky, aby bylo možné najít všechny dostupné výsledky.

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Přidat návrhy
> * Přidat zvýraznění k návrhům
> * Přidat automatické dokončování
> * Kombinování automatického dokončování a návrhů

## <a name="prerequisites"></a>Předpoklady

Tento kurz je součástí série a sestavuje se na stránkování projektu vytvořeném v [kurzu C#: stránkování výsledků hledání – Azure kognitivní hledání](tutorial-csharp-paging.md).

Případně si můžete stáhnout a spustit řešení pro tento konkrétní kurz: [3-Add-typeahead](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/3-add-typeahead).

## <a name="add-suggestions"></a>Přidat návrhy

Pojďme začít s nejjednodušším případem, který uživateli nabízí alternativy: rozevírací seznam návrhů.

1. V souboru index. cshtml změňte `@id` příkaz **TextBoxFor** na **azureautosuggest**.

    ```cs
     @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautosuggest" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

2. Po zavření tohoto příkazu po ukončení ** &lt; /div &gt; **zadejte tento skript. Tento skript využívá [widget automatického dokončování](https://api.jqueryui.com/autocomplete/) z open source knihovny uživatelského rozhraní jQuery a prezentuje rozevírací seznam navrhovaných výsledků. 

    ```javascript
    <script>
        $("#azureautosuggest").autocomplete({
            source: "/Home/Suggest?highlights=false&fuzzy=false",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

    ID "azureautosuggest" připojí výše uvedený skript k vyhledávacímu poli. Možnost source widgetu je nastavena na metodu navrhnout, která volá rozhraní API pro návrh se dvěma parametry dotazu: **Hlavní** a **Přibližná**, v této instanci je nastavena na hodnotu false. K aktivaci hledání je navíc potřeba aspoň dva znaky.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Přidat do zobrazení odkazy na skripty jQuery

1. Chcete-li získat přístup k knihovně jQuery, změňte &lt; &gt; část Head souboru zobrazení na následující kód:

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Typeahead</title>
        <link href="https://code.jquery.com/ui/1.12.1/themes/start/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. Vzhledem k tomu, že zavádíme nový odkaz jQuery, musíme také odebrat nebo odkomentovat výchozí odkaz jQuery v souboru _Layout. cshtml (v **zobrazeních/sdílených** složkách). Vyhledejte následující řádky a přidejte komentář na první řádek skriptu, jak je znázorněno na obrázku. Tato změna zabrání v konfliktu odkazů na jQuery.

    ```html
    <environment include="Development">
        <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

    Teď můžeme použít předdefinované funkce jQuery pro automatické dokončování.

### <a name="add-the-suggest-action-to-the-controller"></a>Přidejte do kontroleru akci navrhnout.

1. V domovském řadiči přidejte akci **navrhnout** (říká se za akci **stránky** ).

    ```cs
        public async Task<ActionResult> Suggest(bool highlights, bool fuzzy, string term)
        {
            InitSearch();

            // Setup the suggest parameters.
            var parameters = new SuggestParameters()
            {
                UseFuzzyMatching = fuzzy,
                Top = 8,
            };

            if (highlights)
            {
                parameters.HighlightPreTag = "<b>";
                parameters.HighlightPostTag = "</b>";
            }

            // Only one suggester can be specified per index. It is defined in the index schema.
            // The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and simply searches the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", parameters);

            // Convert the suggest query results to a list that can be displayed in the client.
            List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();

            // Return the list of suggestions.
            return new JsonResult(suggestions);
        }
    ```

    Parametr **Top** určuje, kolik výsledků se má vrátit (Pokud není zadaný, výchozí hodnota je 5). V indexu Azure se zadá modul pro _návrhy_ , který se provede, když se nastavují data, a ne klientská aplikace, jako je třeba tento kurz. V takovém případě má modul pro návrhy název "SG" a hledá pole **hotelu** – nic jiného. 

    Přibližná shoda umožňuje zahrnutí "blízko neúspěšných" do výstupu, až po jednu vzdálenost úprav. Pokud je parametr **zvýrazňujes** nastaven na hodnotu true, jsou do výstupu přidány tučné značky HTML. V další části nastavíme tyto dva parametry na true (pravda).

2. Může se zobrazit několik syntaktických chyb. Pokud ano, přidejte následující dva příkazy **using** do horní části souboru.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

3. Spusťte aplikaci. Získáte rozsah možností, když zadáte "No", například? Nyní zkuste "PA".

    ![Když zadáte "No", objeví se dva návrhy.](./media/tutorial-csharp-create-first-app/azure-search-suggest-po.png)

    Všimněte si, že písmena, která zadáte, _musí_ začínat slovem a nemusí být ve slově obsažena.

4. Ve skriptu zobrazení nastavte **&fuzzy** na true a spusťte aplikaci znovu. Nyní zadejte "No". Všimněte si, že hledání se předpokládá, že máte jedno písmeno špatné.
 
    ![Zadání "PA" s fuzzy nastavenou na hodnotu true](./media/tutorial-csharp-create-first-app/azure-search-suggest-fuzzy.png)

    Pokud vás zajímá, [syntaxe dotazů Lucene v Azure kognitivní hledání](https://docs.microsoft.com/azure/search/query-lucene-syntax) popisuje logiku použitou v přibližných prohledáních podrobněji.

## <a name="add-highlighting-to-the-suggestions"></a>Přidat zvýraznění k návrhům

Vzhled návrhů pro uživatele můžeme vylepšit nastavením parametru **zvýrazní** na hodnotu true. Nejdřív ale potřebujeme přidat nějaký kód pro zobrazení tučného textu.

1. Do zobrazení (index. cshtml) přidejte následující skript za skript **azureautosuggest** , který jste zadali výše.

    ```javascript
    <script>
        var updateTextbox = function (event, ui) {
            var result = ui.item.value.replace(/<\/?[^>]+(>|$)/g, "");
            $("#azuresuggesthighlights").val(result);
            return false;
        };

        $("#azuresuggesthighlights").autocomplete({
            html: true,
            source: "/home/suggest?highlights=true&fuzzy=false&",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },
            select: updateTextbox,
            focus: updateTextbox
        }).data("ui-autocomplete")._renderItem = function (ul, item) {
            return $("<li></li>")
                .data("item.autocomplete", item)
                .append("<a>" + item.label + "</a>")
                .appendTo(ul);
        };
    </script>
    ```

2. Nyní Změňte ID textového pole tak, aby vypadalo následovně.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresuggesthighlights" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. Znovu spusťte aplikaci a v návrzích byste měli vidět zadaný text zvýrazněný tučně. Řekněme, zkuste zadat "PA".
 
    ![Zadání "PA" se zvýrazněním](./media/tutorial-csharp-create-first-app/azure-search-suggest-highlight.png)

4. Logika použitá ve výše uvedeném skriptu pro zvýraznění není foolproof. Pokud zadáte termín, který se zobrazí dvakrát ve stejném názvu, tučné výsledky nemusejí být naprosto, co byste chtěli. Zkuste zadat "mo".

    Jednou z otázek, které vývojář potřebuje k zodpovězení, je, kdy skript pracuje dostatečně dobře a kdy by měl adaptivní být řešen. V tomto kurzu už nebudeme brát v úvahu žádné další, ale při hledání přesných algoritmů je něco zvážit, pokud pro vaše data neplatí zvýraznění. Další informace najdete v tématu [zvýrazňování přístupů](search-pagination-page-layout.md#hit-highlighting).

## <a name="add-autocomplete"></a>Přidat automatické dokončování

Další variace, která se mírně liší od návrhů, je automatického dokončování (někdy nazývané "Type-dopředu"), které dokončuje termín dotazu. Znovu začneme s nejjednodušší implementací, než zlepšíte uživatelské prostředí.

1. Do zobrazení zadejte následující skript, a to podle předchozích skriptů.

    ```javascript
    <script>
        $("#azureautocompletebasic").autocomplete({
            source: "/Home/Autocomplete",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

2. Nyní Změňte ID textového pole, aby vypadalo takto.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocompletebasic" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. V případě domovského kontroleru musíme do akce **navrhnout** zadat akci **automatického dokončování** .

    ```cs
        public async Task<ActionResult> AutoComplete(string term)
        {
            InitSearch();

            // Setup the autocomplete parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 6
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Convert the results to a list that can be displayed in the client.
            List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();

            // Return the list.
            return new JsonResult(autocomplete);
        }
    ```

    Všimněte si, že používáme stejnou funkci *návrhy* , která se nazývá "SG", a to při automatickém dokončování hledání jako u návrhů (takže se snažíme jenom provést automatické dokončování názvů hotelů).

    K dispozici je řada nastavení **AutocompleteMode** a používáme **OneTermWithContext**. Popis dalších možností najdete v tématu [rozhraní API pro automatické dokončování](https://docs.microsoft.com/rest/api/searchservice/autocomplete) .

4. Spusťte aplikaci. Všimněte si, jak je rozsah možností zobrazených v rozevíracím seznamu jedinou slovem. Zkuste zadat slova začínající řetězcem "znovu". Všimněte si, jak se počet možností zkracuje při psaní dalších písmen.

    ![Psaní pomocí základního automatického dokončování](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocompletebasic.png)

    Vzhledem k tomu, že skript návrhů, který jste spustili dříve, je pravděpodobně užitečnější než tento skript automatického dokončování. Aby bylo možné provést další uživatelsky přívětivější, je nejlepší přidat do vyhledávání návrhů.

## <a name="combine-autocompletion-and-suggestions"></a>Kombinování automatického dokončování a návrhů

Kombinace automatického dokončování a návrhů je nejsložitější z našich možností a pravděpodobně poskytuje nejlepší uživatelské prostředí. To, co chceme zobrazit, je první volbou Azure Kognitivní hledání pro autotexting text, který je právě psaný. Také chceme, aby byl jako rozevírací seznam k disřadě návrhů.

K dispozici jsou knihovny, které nabízejí tuto funkci – často se označuje jako "vložené autodokončování" nebo podobný název. Tuto funkci však nativně implementujeme, abyste viděli, co se chystá. V tomto příkladu budeme začít pracovat na řadiči jako první.

1. Musíme do kontroleru přidat akci, která vrátí jenom jeden výsledek automatického dokončování, spolu se zadaným počtem návrhů. Budeme volat tuto akci **AutocompleteAndSuggest**. V domovském řadiči přidejte následující akci, a to za dalšími novými akcemi.

    ```cs
        public async Task<ActionResult> AutocompleteAndSuggest(string term)
        {
            InitSearch();

            // Setup the type-ahead search parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 1,
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Setup the suggest search parameters.
            var sp = new SuggestParameters()
            {
                Top = 8,
            };

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and it searches only the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", sp);

            // Create an empty list.
            var results = new List<string>();

            if (autocompleteResult.Results.Count > 0)
            {
                // Add the top result for type-ahead.
                results.Add(autocompleteResult.Results[0].Text);
            }
            else
            {
                // There were no type-ahead suggestions, so add an empty string.
                results.Add("");
            }
            for (int n = 0; n < suggestResult.Results.Count; n++)
            {
                // Now add the suggestions.
                results.Add(suggestResult.Results[n].Text);
            }

            // Return the list.
            return new JsonResult(results);
        }
    ```

    Jedna možnost automatického dokončování se vrátí v horní části seznamu **výsledků** , po kterém následují všechny návrhy.

2. V zobrazení nejprve implementujeme štych, aby se v oblasti tučného textu, který uživatel zadal, vykresluje slovo světle šedého automatického dokončování. HTML obsahuje relativní umístění pro tento účel. Změňte příkaz **TextBoxFor** (a jeho okolní &lt; příkazy div &gt; ) na následující, což znamená, že druhé vyhledávací pole identifikované jako v **underneath** rámci našeho normálního vyhledávacího pole je napravo pomocí tohoto vyhledávacího pole 39 pixelů z výchozího umístění!

    ```cs
    <div id="underneath" class="searchBox" style="position: relative; left: 0; top: 0">
    </div>

    <div id="searchinput" class="searchBoxForm" style="position: relative; left: 0; top: -39px">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocomplete" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

    Všimněte si, že znovu měníme ID, aby se v tomto případě **azureautocomplete** .

3. V zobrazení také zadejte následující skript, a to po všech skriptech, které jste doposud zadali. Existuje hodně dalších.

    ```javascript
    <script>
        $('#azureautocomplete').autocomplete({
            delay: 500,
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },

            // Use Ajax to set up a "success" function.
            source: function (request, response) {
                var controllerUrl = "/Home/AutoCompleteAndSuggest?term=" + $("#azureautocomplete").val();
                $.ajax({
                    url: controllerUrl,
                    dataType: "json",
                    success: function (data) {
                        if (data && data.length > 0) {

                            // Show the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = data[0];

                            // Remove the top suggestion as it is used for inline autocomplete.
                            var array = new Array();
                            for (var n = 1; n < data.length; n++) {
                                array[n - 1] = data[n];
                            }

                            // Show the drop-down list of suggestions.
                            response(array);
                        } else {

                            // Nothing is returned, so clear the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }
                });
            }
        });

        // Complete on TAB.
        // Clear on ESC.
        // Clear if backspace to less than 2 characters.
        // Clear if any arrow key hit as user is navigating the suggestions.
        $("#azureautocomplete").keydown(function (evt) {

            var suggestedText = document.getElementById("underneath").innerHTML;
            if (evt.keyCode === 9 /* TAB */ && suggestedText.length > 0) {
                $("#azureautocomplete").val(suggestedText);
                return false;
            } else if (evt.keyCode === 27 /* ESC */) {
                document.getElementById("underneath").innerHTML = "";
                $("#azureautocomplete").val("");
            } else if (evt.keyCode === 8 /* Backspace */) {
                if ($("#azureautocomplete").val().length < 2) {
                    document.getElementById("underneath").innerHTML = "";
                }
            } else if (evt.keyCode >= 37 && evt.keyCode <= 40 /* Any arrow key */) {
                document.getElementById("underneath").innerHTML = "";
            }
        });

        // Character replace function.
        function setCharAt(str, index, chr) {
            if (index > str.length - 1) return str;
            return str.substr(0, index) + chr + str.substr(index + 1);
        }

        // This function is needed to clear the "underneath" text when the user clicks on a suggestion, and to
        // correct the case of the autocomplete option when it does not match the case of the user input.
        // The interval function is activated with the input, blur, change, or focus events.
        $("#azureautocomplete").on("input blur change focus", function (e) {

            // Set a 2 second interval duration.
            var intervalDuration = 2000, 
                interval = setInterval(function () {

                    // Compare the autocorrect suggestion with the actual typed string.
                    var inputText = document.getElementById("azureautocomplete").value;
                    var autoText = document.getElementById("underneath").innerHTML;

                    // If the typed string is longer than the suggestion, then clear the suggestion.
                    if (inputText.length > autoText.length) {
                        document.getElementById("underneath").innerHTML = "";
                    } else {

                        // If the strings match, change the case of the suggestion to match the case of the typed input.
                        if (autoText.toLowerCase().startsWith(inputText.toLowerCase())) {
                            for (var n = 0; n < inputText.length; n++) {
                                autoText = setCharAt(autoText, n, inputText[n]);
                            }
                            document.getElementById("underneath").innerHTML = autoText;

                        } else {
                            // The strings do not match, so clear the suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }

                    // If the element loses focus, stop the interval checking.
                    if (!$input.is(':focus')) clearInterval(interval);

                }, intervalDuration);
        });
    </script>
    ```

    Všimněte si, že chytřejší použití funkce **interval** pro vymazání základního textu, když se už neshoduje s tím, co uživatel zapisuje, a také nastavování stejného případu (horní nebo dolní), jak zapisuje uživatel (jako "PA" odpovídá "PA", "PA", "PA" při hledání "), aby byl text v překrytí úhledný.

    Přečtěte si komentáře ve skriptu, abyste získali úplný princip.

4. Nakonec musíme provést drobné úpravy dvou tříd HTML, aby byly transparentní. Přidejte následující řádek do tříd **searchBoxForm** a **searchBox** v souboru hotely. CSS.

    ```html
        background: rgba(0,0,0,0);
    ```

5. Teď aplikaci spusťte. Do vyhledávacího pole zadejte "PA". Získáte "Palace" jako návrh automatického dokončování spolu se dvěma hotely, které obsahují "PA"?

    ![Zadání s vloženým dokončováním a návrhy](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocomplete.png)

6. Vyzkoušejte si tabulátory, které přijímají návrh automatického dokončování, a zkuste vybrat návrhy pomocí kláves se šipkami a klávesy TAB a zkuste to znovu pomocí myši a jediného kliknutí. Ověřte, že skript pracuje s těmito situacemi úhledně.

    Můžete se rozhodnout, že se načtou do knihovny, která pro vás tuto funkci nabízí, ale teď víte aspoň jeden způsob, jak získat automatické dokončení vloženého textu!

## <a name="takeaways"></a>Shrnutí

Vezměte v úvahu následující poznatky z tohoto projektu:

* Automatického dokončování (označuje se také jako "typ dopředu") a návrhy mohou uživateli povolit zadání pouze několika klíčů k nalezení přesně toho, co chtějí.
* Automatického dokončování a návrhy, které společně spolupracují, můžou poskytovat bohatou činnost koncového uživatele.
* Funkce automatického dokončování testujte vždy se všemi formami vstupu.
* Použití funkce **setInterval** může být užitečné při ověřování a opravách prvků uživatelského rozhraní.

## <a name="next-steps"></a>Další kroky

V dalším kurzu jsme si vyhledali jiný způsob, jak zlepšit činnost koncového uživatele, a to pomocí omezujících vlastností, abyste mohli zúžit hledání jediným kliknutím.

> [!div class="nextstepaction"]
> [Kurz v jazyce C#: použití omezujících vlastností k podpoře navigace – Azure Kognitivní hledání](tutorial-csharp-facets.md)


