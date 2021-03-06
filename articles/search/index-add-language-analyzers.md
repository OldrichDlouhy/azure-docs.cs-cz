---
title: Přidat analyzátory jazyka do polí řetězců
titleSuffix: Azure Cognitive Search
description: Vícejazyčná lexikální analýza pro jiné než anglické dotazy a indexy v Azure Kognitivní hledání.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/05/2020
ms.openlocfilehash: 8f0909ee1cdce1e6180b91a30b2e9b281098c826
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85130547"
---
# <a name="add-language-analyzers-to-string-fields-in-an-azure-cognitive-search-index"></a>Přidání analyzátorů jazyka do polí řetězců v indexu Azure Kognitivní hledání

*Analyzátor jazyka* je konkrétní typ [analyzátoru textu](search-analyzers.md) , který provádí lexikální analýzu pomocí jazykových pravidel cílového jazyka. Každé prohledávatelné pole má vlastnost **Analyzer** . Pokud se obsah skládá z přeložených řetězců, jako jsou například samostatná pole pro angličtinu a čínský text, můžete zadat jazykové analyzátory pro každé pole, abyste měli přístup k bohatě lingvistických funkcím těchto analyzátorů.

## <a name="when-to-use-a-language-analyzer"></a>Kdy použít analyzátor jazyka

Analyzátor jazyka byste měli vzít v úvahu, pokud povědomí o struktuře slov nebo vět přidá hodnotu k analýze textu. Běžným příkladem je přidružení nepravidelných operací formulářů ("přenést" a "převedená") nebo podstatná jména v množném číslech ("myši" a "Mouse"). Bez jazykového povědomí se tyto řetězce analyzují jenom na fyzických vlastnostech, které neumožňují zachycení připojení. Vzhledem k tomu, že velké bloky textu mají větší pravděpodobně obsah, pole obsahující popisy, recenze nebo souhrny jsou vhodnými kandidáty pro analyzátor jazyka.

Analyzátory jazyka byste měli zvážit také v případě, že obsah se skládá z jiných než západních řetězců jazyka. I když je [výchozím analyzátorem](search-analyzers.md#default-analyzer) jazyk – nezávislá, koncept použití mezer a speciálních znaků (spojovníky a lomítka) na oddělení řetězců je více použitelný pro západní jazyky, než je západní. 

Například v čínštině, japonštině, korejštině (CJK) a dalších asijských jazycích není mezera nutně oddělovačem slov. Vezměte v úvahu následující japonský řetězec. Vzhledem k tomu, že nemá žádné mezery, by nástroj Language-nezávislá Analyzer pravděpodobně analyzoval celý řetězec jako jeden token, když ve skutečnosti je řetězec ve skutečnosti fráze.

```
これは私たちの銀河系の中ではもっとも重く明るいクラスの球状星団です。
(This is the heaviest and brightest group of spherical stars in our galaxy.)
```

V příkladu výše by úspěšný dotaz musel obsahovat úplný token nebo částečný token pomocí zástupného znaku přípony, což vede k nepřirozenému a omezení možností vyhledávání.

Lepším řešením je hledat jednotlivá slova: 明るい (světlé), 私たちの (náš), 銀河系 (Galaxy). Použití jednoho z japonských analyzátorů, které jsou k dispozici v Kognitivní hledání, má pravděpodobně odemčení tohoto chování, protože tyto analyzátory jsou lépe vybavené při rozdělování bloku textu na smysluplná slova v cílovém jazyce.

## <a name="comparing-lucene-and-microsoft-analyzers"></a>Porovnání aplikací Lucene a Microsoft Analyzer

Azure Kognitivní hledání podporuje analyzátory jazyka 35, které jsou zajištěné pomocí aplikace Lucene a analyzátory jazyka 50 pomocí technologie Microsoft pro zpracování přirozeného jazyka, používané v Office a Bingu.

Někteří vývojáři můžou preferovat známé, jednoduché a open source řešení Lucene. Analyzátory jazyka Lucene jsou rychlejší, ale analyzátory Microsoftu mají pokročilé možnosti, jako je lemmatizátor nebo předzpracování, wordová odkódování (v jazycích, jako je němčina, dánština, holandština, švédština, norština, finština, práce, maďarština, slovenština) a rozpoznávání entit (adresy URL, e-maily, data, čísla). Pokud je to možné, měli byste při rozhodování o tom, který z nich nejlépe vyhovuje, spustit porovnání analyzátorů Microsoftu a Lucene. 

Indexování pomocí analyzátorů Microsoft Analyzer se v závislosti na jazyku dokončí na průměrně dvakrát až třikrát pomaleji než jejich ekvivalenty Lucene. Pro dotazy na průměrnou velikost by nemělo být významně ovlivněný výkon hledání. 

### <a name="english-analyzers"></a>Analyzátory angličtiny

Výchozím analyzátorem je standardní Lucene, který funguje dobře pro angličtinu, ale ne i pro angličtinu nebo analyzátor od Microsoftu pro angličtinu. 
 
+ Analyzátor angličtiny pro Lucene v angličtině rozšiřuje standardní analyzátor. Odebere possessives (koncovou) z slov, aplikuje odvozování na Porter lemmatizátor Algorithm a odebere anglická slova o stopách.  

+ Microsoft English Analyzer vykonává lemmatizátor nebo předzpracování místo odvozování. To znamená, že může zpracovávat inflected a nepravidelně větší množství slov, což má za následek relevantnější výsledky hledání. 

## <a name="configuring-analyzers"></a>Konfigurace analyzátorů

Analyzátory jazyka se používají tak, jak jsou. Pro každé pole v definici indexu můžete nastavit vlastnost **Analyzer** na název analyzátoru, který určuje jazyk a jazykové sady (Microsoft nebo Lucene). Stejný analyzátor bude použit při indexování a hledání daného pole. Můžete mít například samostatná pole pro anglická, francouzská a španělská označení hotelu, která existují vedle sebe ve stejném indexu.

> [!NOTE]
> Není možné použít jiný analyzátor jazyka při indexování, než je doba dotazu v poli. Tato funkce je vyhrazena pro [vlastní analyzátory](index-add-custom-analyzers.md). Z tohoto důvodu, pokud se pokusíte nastavit vlastnosti **searchAnalyzer** nebo **indexAnalyzer** na název analyzátoru jazyka, REST API vrátí chybovou odpověď. Místo toho je nutné použít vlastnost **Analyzer** .

Pomocí parametru dotazu **searchFields** určete, které pole pro konkrétní jazyk chcete v dotazech vyhledat. Můžete zkontrolovat příklady dotazů, které obsahují vlastnost Analyzer v [dokumentu hledání](https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Další informace o vlastnostech indexu najdete v tématu [Create index &#40;Azure Kognitivní hledání REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index). Další informace o analýze v Azure Kognitivní hledání najdete v tématu [analyzátory v azure kognitivní hledání](https://docs.microsoft.com/azure/search/search-analyzers).

<a name="language-analyzer-list"></a>

## <a name="language-analyzer-list"></a>Seznam analyzátoru jazyka 
 Níže je uvedený seznam podporovaných jazyků společně s názvy Lucene a Microsoft Analyzer.  

|Jazyk|Název programu Microsoft Analyzer|Název analyzátoru Lucene|  
|--------------|-----------------------------|--------------------------|  
|Arabština|ar. Microsoft|ar. Lucene|  
|Arménština||HY. Lucene|  
|Bengálština|BN. Microsoft||  
|Baskičtina||EU. Lucene|  
|Bulharština|BG. Microsoft|BG. Lucene|  
|Katalánština|CA. Microsoft|CA. Lucene|  
|Čínština (zjednodušená)|zh-Hans. Microsoft|zh-Hans. Lucene|  
|Čínština (tradiční)|zh-Hant. Microsoft|zh-Hant. Lucene|  
|Chorvatština|HR. Microsoft||  
|Čeština|cs. Microsoft|cs. Lucene|  
|Dánština|da. Microsoft|da. Lucene|  
|Nizozemština|NL – Microsoft|NL – Lucene|  
|Angličtina|EN. Microsoft|EN. Lucene|  
|Estonština|et. Microsoft||  
|Finština|Fi. Microsoft|Fi. Lucene|  
|Francouzština|fr. Microsoft|fr. Lucene|  
|Galicijština||HK. Lucene|  
|Němčina|de. Microsoft|de. Lucene|  
|Řečtina|El. Microsoft|El. Lucene|  
|Gudžarátština|Gu. Microsoft||  
|Hebrejština|IT. Microsoft||  
|Hindština|Dobrý den. Microsoft|Dobrý den. Lucene|  
|Maďarština|hu. Microsoft|hu. Lucene|  
|Islandština|je. Microsoft||  
|Indonéština (Bahasa)|ID. Microsoft|ID. Lucene|  
|Irština||GA. Lucene|  
|Italština|IT. Microsoft|IT. Lucene|  
|Japonština|Ja. Microsoft|Ja. Lucene|  
|Kannadština|KN. Microsoft||  
|Korejština|Ko. Microsoft|Ko. Lucene|  
|Lotyština|Lotyšsko. Microsoft|Lotyšsko. Lucene|  
|Litevština|lt. Microsoft||  
|Malajalámština|ml. Microsoft||  
|Malajština (latinka)|MS. Microsoft||  
|Maráthština|Mr. Microsoft||  
|Norština|NB. Microsoft|Ne. Lucene|  
|Perština||FA. Lucene|  
|Polština|pl. Microsoft|pl. Lucene|  
|Portugalština (Brazílie)|pt-br. Microsoft|pt-br. Lucene|  
|portugalština (Portugalsko)|pt-PT. Microsoft|pt-PT. Lucene|  
|Paňdžábština|PA. Microsoft||  
|Rumunština|ro. Microsoft|ro. Lucene|  
|Ruština|ru. Microsoft|ru. Lucene|  
|Srbština (cyrilice)|SR-cyrilice. Microsoft||  
|Srbština (latinka)|SR-Latin. Microsoft||  
|Slovenština|sk. Microsoft||  
|Slovinština|SL. Microsoft||  
|Španělština|ES. Microsoft|ES. Lucene|  
|Švédština|sv. Microsoft|sv. Lucene|  
|Tamilština|Ta. Microsoft||  
|Telugština|te. Microsoft||  
|Thajština|th. Microsoft|th. Lucene|  
|Turečtina|TR. Microsoft|TR. Lucene|  
|Ukrajinština|Spojené království. Microsoft||  
|Urdština|vaše společnost. Microsoft||  
|Vietnamština|VI. Microsoft||  

 Všechny analyzátory s názvy popsanými pomocí **Lucene** jsou napájené z [analyzátorů jazyka Apache Lucene](https://lucene.apache.org/core/6_6_1/core/overview-summary.html ).

## <a name="see-also"></a>Viz také  

+ [Vytvoření indexu &#40;Azure Kognitivní hledání REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  

+ [Deanalýza třídy](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  

