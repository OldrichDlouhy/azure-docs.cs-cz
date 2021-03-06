---
author: IEvangelist
ms.author: dapine
ms.date: 02/19/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 2ac93f5aba722eea78267a512999a5581a887b99
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/29/2020
ms.locfileid: "77474186"
---
Dotazy na kontejner se účtují v cenové úrovni prostředku Azure, který se používá pro `ApiKey` .

U kontejnerů Azure Cognitive Services není licencováno běžet bez připojení ke koncovému bodu měření/fakturace. Kontejnerům musíte povolit, aby informace o fakturaci komunikovaly s koncovým bodem fakturace. Kontejnery Cognitive Services neodesílají zákaznická data, jako je například analyzovaný obrázek nebo text, do společnosti Microsoft.

### <a name="connect-to-azure"></a>Připojení k Azure

Kontejner potřebuje ke spuštění hodnoty argumentu fakturace. Tyto hodnoty umožní kontejneru připojení ke koncovému bodu fakturace. Kontejner hlásí využití každých 10 až 15 minut. Pokud se kontejner nepřipojí k Azure v rámci povoleného časového okna, kontejner pokračuje v běhu, ale neobsluhuje dotazy, dokud se neobnoví koncový bod fakturace. Pokus o připojení se desetkrát vyzkouší v intervalu 10 až 15 minut. Pokud se nemůže připojit ke koncovému bodu fakturace v rámci 10 pokusů, kontejner přestane obsluhovat požadavky.

### <a name="billing-arguments"></a>Argumenty fakturace

Příkaz spustí kontejner, pokud jsou k dispozici všechny tři z následujících možností s platnými hodnotami: <a href="https://docs.docker.com/engine/reference/commandline/run/" target="_blank"> `docker run` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>

| Možnost | Description |
|--------|-------------|
| `ApiKey` | Klíč rozhraní API prostředku Cognitive Services, který se používá ke sledování fakturačních informací.<br/>Hodnota této možnosti musí být nastavená na klíč rozhraní API pro zřízený prostředek, který je zadaný v `Billing` . |
| `Billing` | Koncový bod prostředku Cognitive Services, který se používá ke sledování fakturačních informací.<br/>Hodnota této možnosti musí být nastavená na identifikátor URI koncového bodu zřízeného prostředku Azure.|
| `Eula` | Označuje, že jste přijali licenci pro kontejner.<br/>Hodnota této možnosti musí být nastavena na hodnotu **přijmout**. |
