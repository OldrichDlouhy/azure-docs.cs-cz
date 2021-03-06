---
title: Doporučení zabezpečení pro úložiště objektů BLOB
titleSuffix: Azure Storage
description: Přečtěte si o doporučeních zabezpečení pro úložiště objektů BLOB. Implementace těchto pokynů vám pomůže splnit vaše povinnosti zabezpečení, jak je popsáno v našem sdíleném modelu zodpovědnosti.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 06/17/2020
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 808cf713cc34f90224cb3d5ce8a714fbafdcedbc
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86202945"
---
# <a name="security-recommendations-for-blob-storage"></a>Doporučení zabezpečení pro úložiště objektů BLOB

Tento článek obsahuje doporučení zabezpečení pro úložiště objektů BLOB. Implementace těchto doporučení vám pomůže splnit vaše povinnosti zabezpečení, jak je popsáno v našem sdíleném modelu zodpovědnosti. Další informace o tom, co společnost Microsoft splní zodpovědností poskytovatele služeb, najdete v článku [sdílené odpovědnosti pro cloud computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf).

Některá doporučení obsažená v tomto článku můžete automaticky monitorovat pomocí Azure Security Center. Azure Security Center je první linií obrany při ochraně vašich prostředků v Azure. Informace o Azure Security Center najdete v tématu [co je Azure Security Center?](../../security-center/security-center-intro.md).

Azure Security Center pravidelně analyzuje stav zabezpečení vašich prostředků Azure, aby se identifikovaly potenciální ohrožení zabezpečení. Pak vám poskytne doporučení, jak je řešit. Další informace o Azure Security Center doporučeních najdete v tématu [doporučení zabezpečení v Azure Security Center](../../security-center/security-center-recommendations.md).

## <a name="data-protection"></a>Ochrana dat

| Doporučení | Komentáře | Security Center |
|-|----|--|
| Použití modelu nasazení Azure Resource Manager | Vytvořte nové účty úložiště pomocí modelu nasazení Azure Resource Manager pro důležitá vylepšení zabezpečení, včetně řízení přístupu (RBAC) a auditování, Správce prostředků nasazení a zásad správného řízení, přístupu ke spravovaným identitám, přístupu k Azure Key Vault tajných kódů a ověřování a autorizace založeného na službě Azure AD pro přístup k Azure Storage dat a prostředků. Pokud je to možné, migrujte existující účty úložiště, které používají model nasazení Classic pro použití Azure Resource Manager. Další informace o Azure Resource Manager najdete v tématu [Azure Resource Manager Overview](/azure/azure-resource-manager/resource-group-overview). | - |
| Povolit rozšířenou ochranu před internetovými útoky pro všechny vaše účty úložiště | Rozšířená ochrana před internetovými útoky pro Azure Storage poskytuje další vrstvu zabezpečení, která detekuje neobvyklé a potenciálně nebezpečné pokusy o přístup k účtům úložiště nebo jejich zneužití. Výstrahy zabezpečení se aktivují v Azure Security Center, když se vyskytnou anomálie v aktivitě a jsou také odesílány prostřednictvím e-mailu správcům předplatného, s podrobnostmi o podezřelých aktivitách a doporučeních k tomu, jak tyto hrozby Další informace najdete v tématu [Rozšířená ochrana před internetovými útoky pro Azure Storage](../common/storage-advanced-threat-protection.md). | [Ano](../../security-center/security-center-sql-service-recommendations.md) |
| Zapnout obnovitelné odstranění pro data objektů BLOB | Obnovitelné odstranění umožňuje obnovit data objektů BLOB po jejím odstranění. Další informace o obnovitelném odstranění najdete v tématu [obnovitelné odstranění pro objekty blob Azure Storage](storage-blob-soft-delete.md). | - |
| Ukládání důležitých podnikových dat v neměnných objektech blob | Nakonfigurujte právní zásady uchovávání informací na základě času a uložte data objektů BLOB ve stavu ČERVa (zápis jednou, číst mnoho). Objekty blob uložené v immutably se dají přečíst, ale po dobu trvání intervalu uchování se nedají upravit ani odstranit. Další informace najdete v tématu [ukládání důležitých podnikových dat objektů BLOB s neměnném úložištěm](storage-blob-immutable-storage.md). | - |
| Omezení tokenů sdíleného přístupového podpisu (SAS) pouze na připojení HTTPS | Vyžadování protokolu HTTPS v případě, že klient používá token SAS pro přístup k datům objektu blob, pomáhá minimalizovat riziko odposlouchávání. Další informace najdete v tématu [udělení omezeného přístupu k prostředkům Azure Storage pomocí sdílených přístupových podpisů (SAS)](../common/storage-sas-overview.md). | - |

## <a name="identity-and-access-management"></a>Správa identit a přístupu

| Doporučení | Komentáře | Security Center |
|-|----|--|
| Použití Azure Active Directory (Azure AD) k autorizaci přístupu k datům objektu BLOB | Azure AD poskytuje nejvyšší zabezpečení a usnadňuje použití sdíleného klíče pro autorizaci požadavků do úložiště objektů BLOB. Další informace najdete v tématu [autorizace přístupu k objektům blob a frontám Azure pomocí Azure Active Directory](../common/storage-auth-aad.md). | - |
| Mějte na paměti, že při přiřazování oprávnění k objektu zabezpečení služby Azure AD pomocí RBAC je potřeba vzít v úvahu i hlavní povinný. | Když přiřadíte roli uživateli, skupině nebo aplikaci, přidělte tomuto objektu zabezpečení pouze ta oprávnění, která jsou potřebná k provedení svých úkolů. Omezení přístupu k prostředkům pomáhá zabránit neúmyslnému a škodlivému zneužití vašich dat. | - |
| K udělení omezeného přístupu k datům objektů BLOB klientům použijte delegování uživatelů. | SAS delegování uživatele je zabezpečené pomocí přihlašovacích údajů pro Azure Active Directory (Azure AD) a také podle oprávnění zadaných pro SAS. SAS delegování uživatele je obdobou pro SAS služby v souvislosti s jeho rozsahem a funkcí, ale nabízí výhody zabezpečení prostřednictvím SAS služby. Další informace najdete v tématu [udělení omezeného přístupu k prostředkům Azure Storage pomocí sdílených přístupových podpisů (SAS)](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). | - |
| Zabezpečení přístupových klíčů k účtu pomocí Azure Key Vault | Microsoft doporučuje, abyste pomocí Azure AD schvalovali požadavky na Azure Storage. Pokud ale musíte použít autorizaci pomocí sdíleného klíče, zabezpečte klíče účtu pomocí Azure Key Vault. Klíče můžete načíst z trezoru klíčů za běhu a nemusíte je ukládat do aplikace. Další informace o Azure Key Vault najdete v tématu [Azure Key Vault Overview](../../key-vault/general/overview.md). | - |
| Pravidelně znovu vygenerujte klíče účtu | Střídání klíčů účtu pravidelně snižuje riziko odhalení vašich dat u škodlivých aktérů. | - |
| Mějte na paměti, že hlavní povinný nejnižší oprávnění při přiřazování oprávnění k SAS | Při vytváření SAS zadejte jenom ta oprávnění, která klient vyžaduje k provedení své funkce. Omezení přístupu k prostředkům pomáhá zabránit neúmyslnému a škodlivému zneužití vašich dat. | - |
| Pro všechna SAS, která vydáváte klientům, je nutné mít plán odvolání. | Pokud dojde k ohrožení zabezpečení SAS, budete chtít toto SAS odvolat co nejrychleji. Pokud chcete odvolat SAS delegování uživatele, Odvolejte klíč delegování uživatele, abyste mohli rychle zrušit platnost všech signatur přidružených k tomuto klíči. Pokud chcete odvolat SAS služby, která je přidružená k uložené zásadě přístupu, můžete zásady uloženého přístupu odstranit, přejmenovat zásadu nebo změnit její dobu platnosti na čas, který je v minulosti. Další informace najdete v tématu [udělení omezeného přístupu k prostředkům Azure Storage pomocí sdílených přístupových podpisů (SAS)](../common/storage-sas-overview.md).  | - |
| Pokud přidružení zabezpečení služby není přidružené k uloženým zásadám přístupu, nastavte čas vypršení platnosti na jednu hodinu nebo méně. | SAS služby, která není přidružená k uložené zásadě přístupu, nejde odvolat. Z tohoto důvodu se doporučuje omezit čas vypršení platnosti tak, aby SAS platilo jednu hodinu nebo méně. | - |
| Zakázat anonymní veřejný přístup pro čtení kontejnerů a objektů BLOB | Anonymní veřejný přístup pro čtení kontejneru a jeho objekty blob udělí každému klientovi přístup jen pro čtení k těmto prostředkům. Vyhněte se povolení veřejného přístupu pro čtení, pokud to váš scénář nevyžaduje. Informace o tom, jak zakázat anonymní veřejný přístup pro účet úložiště, najdete v tématu [Konfigurace anonymního veřejného přístupu pro čtení pro kontejnery a objekty blob](anonymous-read-access-configure.md).  | - |

## <a name="networking"></a>Sítě

| Doporučení | Komentáře | Security Center |
|-|----|--|
| Nakonfigurujte minimální požadovanou verzi protokolu TLS (Transport Layer Security) pro účet úložiště.  | Vyžaduje, aby klienti používali bezpečnější verzi protokolu TLS, aby bylo možné podávat žádosti na účet Azure Storage konfigurací minimální verze TLS pro tento účet. Další informace najdete v tématu [Konfigurace minimální požadované verze protokolu TLS (Transport Layer Security) pro účet úložiště](../common/transport-layer-security-configure-minimum-version.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) .| - |
| Povolení možnosti **Vyžadovat zabezpečený přenos** u všech účtů úložiště | Pokud povolíte možnost **Vyžadovat zabezpečený přenos** , musí probíhat všechny požadavky na účet úložiště přes zabezpečená připojení. Všechny požadavky vytvořené přes protokol HTTP selžou. Další informace najdete v tématu [vyžadování zabezpečeného přenosu v Azure Storage](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). | [Ano](../../security-center/security-center-sql-service-recommendations.md) |
| Povolení pravidel brány firewall | Nakonfigurujte pravidla brány firewall tak, aby se omezil přístup k vašemu účtu úložiště na požadavky, které pocházejí ze zadaných IP adres nebo rozsahů, nebo ze seznamu podsítí v Azure Virtual Network (VNet). Další informace o konfiguraci pravidel brány firewall najdete v tématu [konfigurace Azure Storage bran firewall a virtuálních sítí](../common/storage-network-security.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). | - |
| Povolení přístupu k účtu úložiště důvěryhodným službám Microsoftu | Zapnutím pravidel brány firewall pro váš účet úložiště se ve výchozím nastavení zablokuje příchozí požadavky na data, pokud žádosti pocházejí ze služby v rámci Azure Virtual Network (VNet) nebo z povolených veřejných IP adres. Blokované požadavky zahrnují ty z jiných služeb Azure, od Azure Portal, ze služeb protokolování a metriky atd. Žádosti z jiných služeb Azure můžete povolit přidáním výjimky, která umožňuje důvěryhodným službám Microsoftu přístup k účtu úložiště. Další informace o přidání výjimky pro důvěryhodné služby společnosti Microsoft najdete v tématu [konfigurace Azure Storage bran firewall a virtuálních sítí](../common/storage-network-security.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).| - |
| Použití privátních koncových bodů | Privátní koncový bod přiřadí privátní IP adresu z vaší služby Azure Virtual Network (VNet) k účtu úložiště. Zabezpečuje veškerý provoz mezi vaší virtuální sítí a účtem úložiště prostřednictvím privátního propojení. Další informace o privátních koncových bodech najdete v tématu [připojení soukromě k účtu úložiště pomocí privátního koncového bodu Azure](../../private-link/create-private-endpoint-storage-portal.md). | - |
| Použití značek služby virtuální sítě | Značka služby představuje skupinu předpon IP adres z dané služby Azure. Společnost Microsoft spravuje předpony adres, které jsou součástí značky služby, a automaticky aktualizuje označení služby jako adresy změny. Další informace o značkách služby podporovaných nástrojem Azure Storage najdete v tématu [Přehled služeb Azure](../../virtual-network/service-tags-overview.md). Kurz, který ukazuje použití značek služby k vytváření odchozích pravidel sítě, najdete v tématu [omezení přístupu k prostředkům PaaS](../../virtual-network/tutorial-restrict-network-access-to-resources.md). | - |
| Omezení síťového přístupu na konkrétní sítě | Omezení síťového přístupu k sítím hostujícím klienty vyžaduje přístup snižuje riziko napadení prostředků pro síťové útoky. | [Ano](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Protokolování a monitorování

| Doporučení | Komentáře | Security Center |
|-|----|--|
| Sledování způsobu autorizace požadavků | Povolte protokolování Azure Storage, abyste mohli sledovat, jak byl každý požadavek vytvořený proti Azure Storage autorizovaný. Protokoly označují, zda byl požadavek anonymně vytvořen pomocí tokenu OAuth 2,0, pomocí sdíleného klíče nebo pomocí sdíleného přístupového podpisu (SAS). Další informace najdete v tématu [protokolování analýzy Azure Storage](../common/storage-analytics-logging.md). | - |

## <a name="next-steps"></a>Další kroky

- [Dokumentace k zabezpečení Azure](https://docs.microsoft.com//azure/security/)
- [Dokumentace k zabezpečení vývoje](https://docs.microsoft.com/azure/security/develop/).
