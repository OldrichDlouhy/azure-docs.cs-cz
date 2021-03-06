---
title: Šifrování neaktivních dat ve službě Speech Service
titleSuffix: Azure Cognitive Services
description: Šifrování neaktivních dat ve službě Speech Service.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: c2e52fbab8d984f7442d8a336e90e9f22c0bf061
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198673"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Šifrování neaktivních dat ve službě Speech Service

Služba Speech Service automaticky šifruje vaše data při jejich trvalém uložení do cloudu. Šifrování služby Speech chrání vaše data a usnadňuje splnění závazků týkajících se zabezpečení a dodržování předpisů v organizaci.

## <a name="about-cognitive-services-encryption"></a>O šifrování Cognitive Services

Data se šifrují a dešifrují s využitím [256 šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kompatibilního se [standardem FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) . Šifrování a dešifrování je transparentní, což znamená, že se pro vás spravuje šifrování a přístup. Vaše data jsou ve výchozím nastavení zabezpečená a nemusíte upravovat kód ani aplikace, abyste mohli využívat šifrování.

## <a name="about-encryption-key-management"></a>O správě šifrovacích klíčů

Když použijete Custom Speech a vlastní hlas, služba Speech Service může ukládat následující data v cloudu:  

* Data trasování řeči – pouze pokud vaše trasování zapnete pro vlastní koncový bod
* Nahraná školení a testovací data

Ve výchozím nastavení jsou vaše data uložená v úložišti Microsoftu a vaše předplatné používá šifrovací klíče spravované Microsoftem. Máte také možnost připravit si vlastní účet úložiště. Přístup ke Storu spravuje spravovaná identita a služba pro rozpoznávání řeči nemá přímý přístup k vašim datům, jako jsou data trasování řeči, přizpůsobení školicích dat a vlastní modely.

Další informace o spravované identitě najdete v tématu [co jsou spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Přineste si vlastní úložiště (BYOS) pro přizpůsobení a protokolování

Pokud chcete požádat o přístup k získání vlastního úložiště, vyplňte a odešlete [formulář žádosti služby Speech – Přineste si vlastní úložiště (BYOS)](https://aka.ms/cogsvc-cmk). Po schválení budete muset vytvořit vlastní účet úložiště, abyste mohli ukládat data požadovaná pro přizpůsobení a protokolování. Při přidávání účtu úložiště umožní prostředek služby řeči spravovanou identitu přiřazenou systémem. Po povolení spravované identity přiřazené systémem se tento prostředek zaregistruje ve službě Azure Active Directory (AAD). Po registraci bude spravované identitě udělen přístup k účtu úložiště. Další informace o spravovaných identitách najdete tady. Další informace o spravované identitě najdete v tématu [co jsou spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> Pokud zakážete spravované identity přiřazené systémem, odebere se přístup k účtu úložiště. Tím dojde k tomu, že části služby pro rozpoznávání řeči, které budou vyžadovat přístup k účtu úložiště, přestanou fungovat.  

Služba Speech v současné době nepodporuje Customer Lockbox. Zákaznická data ale můžete ukládat pomocí BYOS, což vám umožní dosáhnout podobných ovládacích prvků pro data [Customer Lockbox](../../security/fundamentals/customer-lockbox-overview.md). Pamatujte, že data služby Speech Service zůstanou a jsou zpracována v oblasti, ve které byl prostředek řeči vytvořen. To platí pro veškerá data v klidovém režimu a data při přenosu. Pokud používáte funkce vlastního nastavení, jako je Custom Speech a vlastní hlas, všechna zákaznická data se přenesou, ukládají a zpracovávají ve stejné oblasti, ve které se nachází BYOS (Pokud se používá) a prostředek služby Speech.

> [!IMPORTANT]
> Společnost **Microsoft nepoužívá** zákaznická data ke zlepšení svých modelů řeči. Pokud je navíc protokolování koncových bodů zakázané a nepoužívá se žádné vlastní nastavení, neukládají se žádná zákaznická data. 

## <a name="next-steps"></a>Další kroky

* [Služba Speech Service – Přineste si vlastní úložiště (BYOS) – formulář žádosti](https://aka.ms/cogsvc-cmk)
* [Co jsou spravované identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
