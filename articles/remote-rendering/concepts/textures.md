---
title: Textury
description: Pracovní postup prostředků textury
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 4b87008f4172e58440c32e4ff92a3ee1f4d5efae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565874"
---
# <a name="textures"></a>Textury

Textury jsou neměnné [sdílené prostředky](../concepts/lifetime.md). Textury lze načíst ze služby [BLOB Storage](../how-tos/conversion/blob-storage.md) a použít je přímo na modely, jak je znázorněno v [kurzu: Změna prostředí a materiálů](../tutorials/unity/materials-lighting-effects/materials-lighting-effects.md). Nejčastěji se ale textury budou součástí [převedeného modelu](../how-tos/conversion/model-conversion.md), na který odkazují jeho [materiály](materials.md).

## <a name="texture-types"></a>Typy textury

Různé typy textur mají různé případy použití:

* **2D textury** se používají hlavně v [materiálech](materials.md).
* **Cubemaps** se dá použít pro [Nebeský](../overview/features/sky.md).

## <a name="supported-texture-formats"></a>Podporované formáty textury

Všechny textury, které má tato šipka mít, musí být ve [formátu dds](https://en.wikipedia.org/wiki/DirectDraw_Surface). Nejlépe s mipmapy a kompresí textur. Pokud chcete automatizovat proces převodu, přečtěte si [Nástroj příkazového řádku TexConv](../resources/tools/tex-conv.md) .

## <a name="loading-textures"></a>Načítání textur

Při načítání textury je nutné zadat její očekávaný typ. Pokud se neshoduje typ, načítání textury se nezdařilo.
Načtení textury se stejným identifikátorem URI dvakrát vrátí stejný objekt textury, protože se jedná o [sdílený prostředek](../concepts/lifetime.md).

Podobně jako při načítání modelů existují dvě varianty prostředku textury ve zdrojovém úložišti objektů BLOB:

* Prostředek textury lze adresovat pomocí identifikátoru URI SAS. Relevantní funkce načítání je `LoadTextureFromSASAsync` s parametrem `LoadTextureFromSASParams` . Tuto variantu použijte také při načítání [vestavěných textur](../overview/features/sky.md#built-in-environment-maps).
* Texturu je možné adresovat přímo pomocí parametrů služby Blob Storage v případě, že [je úložiště objektů BLOB propojené s účtem](../how-tos/create-an-account.md#link-storage-accounts). Relevantní funkce načítání v tomto případě je `LoadTextureAsync` s parametrem `LoadTextureParams` .

Následující vzorový kód ukazuje, jak načíst texturu prostřednictvím identifikátoru URI SAS (nebo integrované textury) – Všimněte si, že pro druhý případ se liší pouze funkce načítání nebo parametr:

```cs
LoadTextureAsync _textureLoad = null;
void LoadMyTexture(AzureSession session, string textureUri)
{
    _textureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams(textureUri, TextureType.Texture2D));
    _textureLoad.Completed +=
        (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
            _textureLoad = null;
        };
}
```

```cpp
void LoadMyTexture(ApiHandle<AzureSession> session, std::string textureUri)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::Texture2D;
    params.TextureUrl = std::move(textureUri);
    ApiHandle<LoadTextureAsync> textureLoad = *session->Actions()->LoadTextureFromSASAsync(params);
    textureLoad->Completed([](ApiHandle<LoadTextureAsync> res)
    {
        if (res->IsRanToCompletion())
        {
            //use res->Result()
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}
```


V závislosti na tom, jaká textura má být použita pro, může dojít k omezením pro typ a obsah textury. Například mapa hrubosti materiálu v rámci [PBR](../overview/features/pbr-materials.md) musí být ve stupních šedi.

> [!CAUTION]
> Všechny *asynchronní* funkce v ARR vrací objekty asynchronní operace. Odkaz na tyto objekty je nutné uložit, dokud není operace dokončena. V opačném případě může systém uvolňování paměti C# operaci nejdříve odstranit a nemůže být nikdy dokončen. V ukázkovém kódu nad členskou proměnnou ' _textureLoad ' slouží k uložení odkazu, dokud událost dokončení události *dokončeno* nebude.

## <a name="next-steps"></a>Další kroky

* [Materiály](materials.md)
* [Cestovní](../overview/features/sky.md)
