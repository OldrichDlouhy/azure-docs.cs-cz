---
title: Content Moderator šifrování dat v klidovém umístění
titleSuffix: Azure Cognitive Services
description: Content Moderator šifrování dat v klidovém umístění.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 38fc21ee45db25f015a6b8b534b0d922efa636f2
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310542"
---
# <a name="content-moderator-encryption-of-data-at-rest"></a>Content Moderator šifrování dat v klidovém umístění

Content Moderator automaticky šifruje vaše data, když se trvale uloží do cloudu, což pomáhá splnit cíle vaší organizace na zabezpečení a dodržování předpisů.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Klíče spravované zákazníkem jsou dostupné jenom v cenové úrovni E0. Chcete-li požádat o možnost použití klíčů spravovaných zákazníkem, vyplňte a odešlete [formulář žádosti o klíč spravovaný zákazníkem Content moderator](https://aka.ms/cogsvc-cmk). Bude to trvat přibližně 3-5 pracovních dnů, než se vrátí na stav vaší žádosti. V závislosti na poptávce můžete být do fronty zařazené a schválené, protože místo bude k dispozici. Po schválení pro použití CMK s využitím služby Content Moderator budete muset vytvořit nový prostředek Content Moderator a jako cenovou úroveň vybrat E0. Jakmile se vytvoří prostředek Content Moderator s cenovou úrovní E0, můžete k nastavení spravované identity použít Azure Key Vault.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="enable-data-encryption-for-your-content-moderator-team"></a>Povolení šifrování dat pro váš Content Moderator tým

Pokud chcete povolit šifrování dat pro váš tým Content Moderator recenze, přečtěte si [rychlý Start: vyzkoušejte Content moderator na webu](quick-start.md#create-a-review-team).  

> [!NOTE]
> Je potřeba zadat _ID prostředku_ pomocí cenové úrovně Content moderator E0.

## <a name="next-steps"></a>Další kroky

* Úplný seznam služeb, které podporují CMK, najdete v tématu [klíče spravované zákazníkem pro Cognitive Services](../encryption/cognitive-services-encryption-keys-portal.md)
* [Co je Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
* [Formulář žádosti o klíč spravovaný zákazníkem Cognitive Services](https://aka.ms/cogsvc-cmk)

