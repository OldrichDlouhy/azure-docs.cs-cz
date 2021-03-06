---
title: Správa verzí objektů BLOB (Preview)
titleSuffix: Azure Storage
description: Správa verzí úložiště objektů BLOB (Preview) automaticky udržuje předchozí verze objektu a identifikuje je pomocí časových razítek. Předchozí verze objektu blob můžete obnovit, aby se data obnovila, pokud se omylem změnila nebo odstranila.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: fd620e253e661f986f67a440272937026cb4ff7f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528396"
---
# <a name="blob-versioning-preview"></a>Správa verzí objektů BLOB (Preview)

Chcete-li automaticky zachovat předchozí verze objektu, můžete povolit správu verzí služby Blob Storage (Preview).  Pokud je povolená Správa verzí objektů blob, můžete obnovit předchozí verzi objektu blob, aby se data obnovila v případě, že se omylem změnila nebo odstranila.

Správa verzí objektů BLOB je povolená v účtu úložiště a platí pro všechny objekty BLOB v účtu úložiště. Po povolení správy verzí objektů BLOB pro účet úložiště Azure Storage automaticky zachovává verze všech objektů BLOB v účtu úložiště.

Microsoft doporučuje používat správu verzí objektů BLOB k údržbě předchozích verzí objektu BLOB pro zajištění ochrany dat ve vynikajícím formátu. Pokud je to možné, použijte místo snímků objektů BLOB vytváření verzí objektů blob, abyste zachovali předchozí verze. Snímky objektů BLOB poskytují podobné funkce v tom, že udržují dřívější verze objektu blob, ale snímky musí být v aplikaci zachované ručně.

> [!IMPORTANT]
> Správa verzí objektů BLOB vám neumožňuje obnovit z náhodného odstranění účtu úložiště nebo kontejneru. Pokud chcete zabránit nechtěnému odstranění účtu úložiště, nakonfigurujte na prostředku účtu úložiště zámek **CannotDelete** . Další informace o uzamykání prostředků Azure najdete v tématu [uzamčení prostředků, aby se zabránilo neočekávaným změnám](../../azure-resource-manager/management/lock-resources.md).

## <a name="how-blob-versioning-works"></a>Jak funguje Správa verzí objektů BLOB

Verze zachytí stav objektu BLOB v daném časovém okamžiku. Když je pro účet úložiště povolená Správa verzí objektů blob, Azure Storage automaticky vytvoří novou verzi objektu BLOB pokaždé, když se objekt BLOB upraví nebo odstraní.

Při vytváření objektu BLOB s povoleným správou verzí je nový objekt blob aktuální verzí objektu BLOB (nebo základního objektu BLOB). Pokud následně tento objekt BLOB upravíte, Azure Storage vytvoří verzi, která před úpravou zachytí stav objektu BLOB. Upravený objekt BLOB se změní na novou aktuální verzi. Při každé změně objektu BLOB se vytvoří nová verze.

Když odstraníte objekt BLOB s povoleným správou verzí, Azure Storage vytvoří verzi, která před odstraněním zachytí stav objektu BLOB. Aktuální verze objektu BLOB se pak odstraní, ale verze objektu BLOB jsou trvalé, takže je v případě potřeby možné znovu vytvořit. 

Verze objektů BLOB jsou neměnné. Nemůžete změnit obsah nebo metadata existující verze objektu BLOB.

### <a name="version-id"></a>ID verze

Každá verze objektu BLOB je identifikována ID verze. Hodnota ID verze je časové razítko, při kterém byl objekt BLOB zapsán nebo aktualizován. ID verze je přiřazeno v době, kdy je vytvořena verze.

Můžete provádět operace čtení nebo odstranění u konkrétní verze objektu BLOB zadáním jeho ID verze. Pokud ID verze vynecháte, operace bude fungovat s aktuální verzí (základní objekt BLOB).

Když zavoláte operaci zápisu pro vytvoření nebo úpravu objektu blob, Azure Storage v odpovědi vrátí hlavičku *x-MS-Version-ID* . Tato hlavička obsahuje ID verze pro aktuální verzi objektu blob, která byla vytvořena operací zápisu.

ID verze zůstane pro dobu života verze stejné.

### <a name="versioning-on-write-operations"></a>Správa verzí při operacích zápisu

Když je zapnutá Správa verzí objektů blob, každá operace zápisu do objektu BLOB vytvoří novou verzi. Operace zápisu zahrnují [objekt BLOB pro vložení](/rest/api/storageservices/put-blob), [seznam blokovaných](/rest/api/storageservices/put-block-list) [objektů, kopírování objektů BLOB](/rest/api/storageservices/copy-blob)a [nastavení metadat objektů BLOB](/rest/api/storageservices/set-blob-metadata).

Pokud operace zápisu vytvoří nový objekt blob, bude výsledný objekt blob aktuální verzí objektu BLOB. Pokud operace zápisu upraví existující objekt blob, budou nová data zachycena v aktualizovaném objektu blob, což je aktuální verze, a Azure Storage vytvoří verzi, která uloží předchozí stav objektu BLOB.

V diagramech, které jsou uvedené v tomto článku, se pro jednoduchost zobrazuje ID verze jako jednoduchá celočíselná hodnota. Ve skutečnosti je ID verze časové razítko. Aktuální verze je zobrazena modře a předchozí verze jsou zobrazeny šedě.

Následující diagram ukazuje, jak operace zápisu ovlivňují verze objektů BLOB. Tento objekt BLOB je aktuální verze, když se vytvoří objekt BLOB. Když dojde ke změně stejného objektu blob, vytvoří se nová verze, která uloží předchozí stav objektu BLOB a aktualizovaný objekt BLOB se změní na aktuální verzi.

:::image type="content" source="media/versioning-overview/write-operations-blob-versions.png" alt-text="Diagram znázorňující, jak operace zápisu ovlivňují objekty blob ve verzi":::

> [!NOTE]
> Objekt blob, který byl vytvořen před správou verzí povolený pro účet úložiště, nemá ID verze. Když se tento objekt BLOB upraví, změní se stávající objekt blob na aktuální verzi a vytvoří se verze, aby se před aktualizací uložil stav objektu BLOB. Verzi je přiřazeno ID verze, které je jeho časem vytvoření.

### <a name="versioning-on-delete-operations"></a>Správa verzí při operacích odstranění

Když odstraníte objekt blob, bude aktuální verze objektu BLOB předchozí verzí a základní objekt BLOB se odstraní. Při odstranění objektu BLOB jsou zachovány všechny existující předchozí verze objektu BLOB.

Volání operace [odstranění objektu BLOB](/rest/api/storageservices/delete-blob) bez ID verze odstraní základní objekt BLOB. Pokud chcete odstranit konkrétní verzi, zadejte pro tuto verzi na operaci odstranit ID.

Následující diagram znázorňuje efekt operace odstranění u objektu BLOB s verzí:

:::image type="content" source="media/versioning-overview/delete-versioned-base-blob.png" alt-text="Diagram znázorňující odstranění objektu BLOB se správou verzí":::

Zápis nových dat do objektu BLOB vytvoří novou verzi objektu BLOB. Jakékoli existující verze nejsou ovlivněny, jak je znázorněno v následujícím diagramu.

:::image type="content" source="media/versioning-overview/recreate-deleted-base-blob.png" alt-text="Diagram znázorňující opětovné vytvoření objektu BLOB s verzí po odstranění":::

### <a name="blob-types"></a>Typy objektů blob

Pokud je pro účet úložiště povolená Správa verzí objektů blob, všechny operace zápisu a odstranění v objektech blob bloku aktivují vytvoření nové verze s výjimkou operace [Block Put](/rest/api/storageservices/put-block) .

Pro objekty blob stránky a doplňovací objekty BLOB se při vytváření verze aktivuje jenom podmnožina operací zápisu a odstranění. Mezi tyto operace patří:

- [Vložení objektu blob](/rest/api/storageservices/put-blob)
- [Seznam blokovaných umístění](/rest/api/storageservices/put-block-list)
- [Odstranění objektu blob](/rest/api/storageservices/delete-blob)
- [Nastavení metadat objektu BLOB](/rest/api/storageservices/set-blob-metadata)
- [Zkopírování objektu blob](/rest/api/storageservices/copy-blob)

Následující operace neaktivují vytvoření nové verze. Chcete-li zachytit změny z těchto operací, proveďte ruční snímek:

- [Vložit stránku](/rest/api/storageservices/put-page) (objekt blob stránky)
- [Připojit blok](/rest/api/storageservices/append-block) (doplňovací objekt BLOB)

Všechny verze objektu BLOB musí být stejného typu BLOB. Pokud má objekt BLOB předchozí verze, nemůžete přepsat objekt BLOB jednoho typu jiným typem, pokud nejprve neodstraníte objekt BLOB a všechny jeho verze.

### <a name="access-tiers"></a>Úrovně přístupu

Můžete přesunout libovolnou verzi objektu blob bloku, včetně aktuální verze, na jinou úroveň přístupu objektu BLOB voláním operace [nastavit úroveň objektu BLOB](/rest/api/storageservices/set-blob-tier) . Přesunutím starší verze objektu blob do studené nebo archivní úrovně můžete využít výhod nižších cen. Další informace najdete v tématu [Azure Blob Storage: horká, studená a archivní úroveň přístupu](storage-blob-storage-tiers.md).

K automatizaci procesu přesunu objektů blob bloku do příslušné úrovně použijte správu životního cyklu objektů BLOB. Další informace o správě životního cyklu najdete v tématu [Správa životního cyklu úložiště objektů BLOB v Azure](storage-lifecycle-management-concepts.md).

## <a name="enable-or-disable-blob-versioning"></a>Povolit nebo zakázat správu verzí objektů BLOB

Informace o tom, jak povolit nebo zakázat správu verzí objektů blob, najdete v tématu [Povolení nebo zakázání správy verzí objektů BLOB](versioning-enable.md).

Zakázání správy verzí objektů BLOB neodstraní existující objekty blob, verze ani snímky. Když vypnete správu verzí objektů blob, všechny existující verze budou v účtu úložiště dostupné. Následně se nevytvoří žádné nové verze.

Pokud byl vytvořený nebo změněný objekt BLOB v účtu úložiště, pak přepsání objektu BLOB vytvoří novou verzi. Aktualizovaný objekt BLOB už nemá aktuální verzi a nemá ID verze. Všechny následné aktualizace objektu BLOB přepíšou jeho data bez uložení předchozího stavu.

Po zakázání verze můžete číst nebo odstraňovat verze s použitím ID verze. Po zakázání správy verzí můžete také uvést verze objektu BLOB.

Následující diagram ukazuje, jak se mění objekt BLOB po zakázání správy verzí, vytváří objekt blob, který není ve verzi. Všechny existující verze přidružené k tomuto objektu BLOB jsou trvalé.

:::image type="content" source="media/versioning-overview/modify-base-blob-versioning-disabled.png" alt-text="Diagram znázorňující, že se základní objekt BLOB změnil po zakázání správy verzí":::

## <a name="blob-versioning-and-soft-delete"></a>Správa verzí a obnovitelné odstranění objektů BLOB

Správa verzí objektů BLOB a měkké odstranění objektů BLOB společně poskytují optimální ochranu dat. Pokud povolíte obnovitelné odstranění, určíte, jak dlouho Azure Storage by měl zachovat objekt BLOB s odstraněnou příznakem. Jakákoli dočasná Odstraněná verze objektu BLOB zůstane v systému a může být v rámci doby uchování obnovitelného odstranění neodstraní. Další informace o obnovitelném odstranění objektů BLOB najdete v tématu [obnovitelné odstranění pro objekty blob Azure Storage](storage-blob-soft-delete.md).

### <a name="deleting-a-blob-or-version"></a>Odstranění objektu BLOB nebo verze

Obnovitelné odstranění nabízí další ochranu pro odstraňování verzí objektů BLOB. Pokud je v účtu úložiště zapnutá jak Správa verzí, tak i obnovitelné odstranění, Azure Storage vytvoří novou verzi, která uloží stav objektu BLOB hned před odstraněním a odstraní aktuální verzi. Nová verze není Odstraněná a při vypršení doby uchování dočasného odstranění se neodebere.

Při odstranění předchozí verze objektu BLOB se odstraní i verze. Odstraněná verze se uchovává po celou dobu uchování, která je určená v nastaveních obnovitelného odstranění pro účet úložiště a je trvale Odstraněná po vypršení doby uchování obnovitelného odstranění.

Pokud chcete odebrat předchozí verzi objektu blob, explicitně ji odstraňte zadáním ID verze.

Následující diagram ukazuje, co se stane, když odstraníte objekt BLOB nebo verzi objektu BLOB.

:::image type="content" source="media/versioning-overview/soft-delete-historical-version.png" alt-text="Diagram znázorňující odstranění verze s povoleným obnovitelnému odstranění":::

Pokud je v účtu úložiště zapnutá jak Správa verzí, tak i obnovitelné odstranění, při úpravě nebo odstranění verze objektu BLOB nebo objektu BLOB se nevytvoří žádný snímek s odstraněným odstraněnou.

### <a name="restoring-a-soft-deleted-version"></a>Obnovení softwarové odstraněné verze

Odstraněnou verzi objektu blob můžete obnovit tak, že zavoláte operaci zrušit [odstranění objektu BLOB](/rest/api/storageservices/undelete-blob) ve verzi, zatímco doba uchování obnovitelného odstranění je platná. Operace obnovení **objektu BLOB** obnoví všechny dočasně odstraněné verze objektu BLOB.

Obnovení softwarově odstraněných verzí pomocí operace **obnovení objektu BLOB** nezvýší žádnou verzi na aktuální verzi. Chcete-li obnovit aktuální verzi, nejprve obnovte všechny dočasně odstraněné verze a pak pomocí operace [kopírování objektu BLOB](/rest/api/storageservices/copy-blob) zkopírujte předchozí verzi pro obnovení objektu BLOB.

Následující diagram ukazuje, jak obnovit obnovitelné odstraněné verze objektů BLOB pomocí operace obnovení **objektu BLOB** a jak obnovit aktuální verzi objektu BLOB pomocí operace **kopírování objektu** BLOB.

:::image type="content" source="media/versioning-overview/undelete-version.png" alt-text="Diagram znázorňující, jak obnovit obnovitelné odstraněné verze":::

Po uplynutí doby uchování obnovitelného odstranění budou všechny odstraněné verze objektů BLOB trvale odstraněny.

## <a name="blob-versioning-and-blob-snapshots"></a>Vytváření verzí objektů BLOB a snímky objektů BLOB

Snímek objektu BLOB je kopie objektu BLOB jen pro čtení, která je pořízena v určitém časovém okamžiku. Snímky objektů BLOB a verze objektů BLOB jsou podobné, ale snímek je vytvořený ručně nebo vaší aplikací, zatímco verze objektu BLOB je automaticky vytvořená na operaci zápisu nebo odstranění, pokud je pro váš účet úložiště povolená Správa verzí objektů BLOB.

> [!IMPORTANT]
> Microsoft doporučuje, abyste po povolení správy verzí objektů BLOB aktualizovali také aplikaci tak, aby zastavila vytváření snímků objektů blob bloku. Pokud je pro váš účet úložiště povolená Správa verzí, všechny aktualizace a odstranění objektů blob bloku se zachytí a uchovávají ve verzích. Pořízení snímků nenabízí žádné další ochrany dat objektů blob bloku, pokud je povolená Správa verzí objektů BLOB a může zvyšovat náklady a složitost aplikace.

### <a name="snapshot-a-blob-when-versioning-is-enabled"></a>Vytvoření snímku objektu blob, když je povolená Správa verzí

I když se to nedoporučuje, můžete pořídit snímek objektu blob, který je také ve verzi. Pokud aplikaci nemůžete aktualizovat, aby zastavila vytváření snímků objektů blob, když povolíte správu verzí, aplikace může podporovat snímky i verze.

Při pořizování snímku objektu BLOB s verzí se vytvoří nová verze ve stejnou chvíli, kdy se snímek vytvoří. Při pořízení snímku se vytvoří také nová aktuální verze.

Následující diagram ukazuje, co se stane při pořizování snímku objektu BLOB s verzí. V diagramu verze a snímky objektů BLOB s ID verze 2 a 3 obsahují stejná data.

:::image type="content" source="media/versioning-overview/snapshot-versioned-blob.png" alt-text="Diagram znázorňující snímky objektu BLOB s verzí":::

## <a name="authorize-operations-on-blob-versions"></a>Autorizovat operace s verzemi objektů BLOB

Přístup k verzím objektů blob můžete autorizovat pomocí jednoho z následujících přístupů:

- Pomocí řízení přístupu na základě role (RBAC) udělíte oprávnění objektu zabezpečení služby Azure Active Directory (Azure AD). Microsoft doporučuje používat Azure AD pro zajištění vysokého zabezpečení a snadného použití. Další informace o používání služby Azure AD s operacemi objektů BLOB najdete v tématu [autorizace přístupu k objektům blob a frontám pomocí Azure Active Directory](../common/storage-auth-aad.md).
- Pomocí sdíleného přístupového podpisu (SAS) můžete delegovat přístup k verzím objektů BLOB. Zadejte ID verze pro typ podepsaného prostředku `bv` , který představuje verzi objektu BLOB pro vytvoření tokenu SAS pro operace v konkrétní verzi. Další informace o sdílených přístupových podpisech najdete v článku [udělení omezeného přístupu k Azure Storage prostředkům pomocí sdílených přístupových podpisů (SAS)](../common/storage-sas-overview.md).
- Použitím přístupových klíčů účtu k autorizaci operací s verzemi objektů BLOB se sdíleným klíčem. Další informace najdete v tématu [autorizace pomocí sdíleného klíče](/rest/api/storageservices/authorize-with-shared-key).

Správa verzí objektů BLOB je navržená tak, aby chránila vaše data před náhodným nebo škodlivým odstraněním. Kvůli vylepšení ochrany vyžaduje odstranění verze objektu BLOB zvláštní oprávnění. Následující části popisují oprávnění potřebná k odstranění verze objektu BLOB.

### <a name="rbac-action-to-delete-a-blob-version"></a>Akce RBAC pro odstranění verze objektu BLOB

Následující tabulka uvádí, které akce RBAC podporují odstranění objektu BLOB nebo verze objektu BLOB.

| Popis | Operace Blob service | Vyžaduje se akce s daty RBAC. | Integrovaná podpora rolí RBAC |
|----------------------------------------------|------------------------|---------------------------------------------------------------------------------------|-------------------------------|
| Odstraňuje se aktuální verze objektu BLOB. | Odstranění objektu blob | **Microsoft. Storage/storageAccounts/blobServices/Containers/BLOBs/DELETE** | Přispěvatel dat objektu BLOB služby Storage |
| Odstraňuje se verze | Odstranění objektu blob | **Microsoft. Storage/storageAccounts/blobServices/Containers/BLOBs/deleteBlobVersion/Action** | Vlastník dat objektu BLOB služby Storage |

### <a name="shared-access-signature-sas-parameters"></a>Parametry sdíleného přístupového podpisu (SAS)

Podepsaný prostředek pro verzi objektu BLOB je `bv` . Další informace najdete v tématech [Vytvoření SAS služby](/rest/api/storageservices/create-service-sas) nebo [Vytvoření SAS pro delegování uživatelů](/rest/api/storageservices/create-user-delegation-sas).

Následující tabulka uvádí oprávnění požadovaná na SAS k odstranění verze objektu BLOB.

| **Oprávnění** | **Symbol identifikátoru URI** | **Povolené operace** |
|----------------|----------------|------------------------|
| Odstranit         | x              | Odstraní verzi objektu BLOB. |

## <a name="about-the-preview"></a>O verzi Preview

Správa verzí objektů BLOB je dostupná ve verzi Preview v následujících oblastech:

- USA – východ 2
- Střední USA
- Severní Evropa
- Západní Evropa
- Francie – střed
- Kanada – východ
- Střední Kanada

Verze Preview je určena pouze pro neprodukční použití.

Verze 2019-10-10 a vyšší z Azure Storage REST API podporuje správu verzí objektů BLOB.

### <a name="storage-account-support"></a>Podpora účtu úložiště

Správa verzí objektů BLOB je dostupná pro následující typy účtů úložiště:

- Účty úložiště pro obecné účely v2
- Zablokovat účty úložiště objektů BLOB
- Účty úložiště Blob

Pokud je váš účet úložiště účet pro obecné účely V1, použijte Azure Portal k upgradu na účet pro obecné účely v2. Další informace o účtech úložiště najdete v tématu [Přehled účtu Azure Storage](../common/storage-account-overview.md).

Účty úložiště s hierarchickým oborem názvů povoleným pro použití s Azure Data Lake Storage Gen2 nejsou aktuálně podporovány.

### <a name="register-for-the-preview"></a>Zaregistrovat se pro verzi Preview

Pokud se chcete zaregistrovat ve verzi Preview, použijte PowerShell nebo Azure CLI a odešlete žádost o registraci funkce v rámci vašeho předplatného. Po schválení žádosti můžete povolit správu verzí objektů BLOB pomocí všech nových nebo existujících účtů úložiště blob bloku s obecným účelem v2, BLOB Storage nebo Premium.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Pokud se chcete zaregistrovat v prostředí PowerShell, zavolejte příkaz [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) .

```powershell
# Register for blob versioning (preview)
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning

# Refresh the Azure Storage provider namespace
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Pokud se chcete zaregistrovat v Azure CLI, zavolejte příkaz [AZ Feature Register](/cli/azure/feature#az-feature-register) .

```azurecli
az feature register --namespace Microsoft.Storage \
    --name Versioning
```

---

### <a name="check-the-status-of-your-registration"></a>Ověření stavu registrace

Pokud chcete zjistit stav registrace, použijte PowerShell nebo Azure CLI.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Pokud chcete zjistit stav vaší registrace pomocí PowerShellu, zavolejte příkaz [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature) .

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName Versioning
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Pokud chcete zjistit stav registrace pomocí Azure CLI, zavolejte příkaz [AZ Feature](/cli/azure/feature#az-feature-show) .

```azurecli
az feature show --namespace Microsoft.Storage \
    --name Versioning
```

---

## <a name="pricing-and-billing"></a>Ceny a fakturace

Povolení správy verzí objektů BLOB může mít za následek další poplatky za úložiště dat vašeho účtu. Při návrhu aplikace je důležité vědět, jak se tyto poplatky můžou snížit, abyste mohli minimalizovat náklady.

Verze objektů blob, jako jsou snímky objektů blob, se účtují stejnou sazbou jako aktivní data. Pokud verze sdílí bloky nebo stránky s jeho základním objektem blob, platíte jenom za všechny další bloky nebo stránky, které nejsou sdílené mezi verzí a základním objektem BLOB.

> [!NOTE]
> Povolení správy verzí pro často přepsaná data může vést ke zvýšení poplatků za kapacitu úložiště a zvýšené latenci během výpisu operací. Aby se tyto otázky zmírnily, ukládejte často přepsaná data v samostatném účtu úložiště s vypnutou správou verzí.

### <a name="important-billing-considerations"></a>Důležité informace o fakturaci

Při povolování správy verzí objektů BLOB nezapomeňte vzít v úvahu následující body:

- Váš účet úložiště se za jedinečné bloky nebo stránky účtuje bez ohledu na to, jestli jsou v objektu BLOB nebo v předchozí verzi objektu BLOB. K vašemu účtu se neúčtují další poplatky za verze přidružené k objektu blob, dokud neaktualizujete objekt blob, na kterém jsou založené. Po aktualizaci se objekt BLOB odliší od předchozích verzí. Pokud k tomu dojde, budou se vám účtovat jedinečné bloky nebo stránky v jednotlivých objektech blob nebo ve verzi.
- Když nahradíte blok v rámci objektu blob bloku, tento blok se následně účtuje jako jedinečný blok. To platí i v případě, že má blok stejné ID bloku a stejná data jako ve verzi. Po opětovném potvrzení bloku se odliší od jeho protějšku v jakékoli verzi a bude vám účtováno za jeho data. Totéž platí pro stránku v objektu blob stránky, který je aktualizovaný se stejnými daty.
- Úložiště objektů BLOB nemá způsob, jak určit, zda dva bloky obsahují identická data. Každý blok, který je nahraný a potvrzený, se považuje za jedinečný, a to i v případě, že má stejná data a stejné ID bloku. Vzhledem k tomu, že se poplatky za jedinečné bloky dostanou, je důležité vzít v úvahu, že pokud je povolená Správa verzí, budete mít za následek další jedinečné bloky a další poplatky za aktualizaci objektu BLOB.
- Pokud je povolená Správa verzí objektů blob, navrhněte operace aktualizace pro objekty blob bloku tak, aby aktualizovaly nejnižší možný počet bloků. Operace zápisu, které umožňují jemně odstupňovanou kontrolu nad bloky, jsou [vloženy do bloku](/rest/api/storageservices/put-block) a [seznamu blokovaných umístění](/rest/api/storageservices/put-block-list). Operace [Put BLOB](/rest/api/storageservices/put-blob) na druhé straně nahrazuje celý obsah objektu blob, takže může vést k dalším poplatkům.

### <a name="versioning-billing-scenarios"></a>Scénáře fakturace správy verzí

Následující scénáře ukazují, jak se účtují poplatky za objekt blob bloku a jeho verze.

#### <a name="scenario-1"></a>Scénář 1

Ve scénáři 1 má objekt BLOB předchozí verzi. Objekt BLOB se od vytvoření verze neaktualizoval. poplatky se účtují jenom pro jedinečné bloky 1, 2 a 3.

![Prostředky Azure Storage](./media/versioning-overview/versions-billing-scenario-1.png)

#### <a name="scenario-2"></a>Scénář 2

Ve scénáři 2 se aktualizoval jeden blok (blok 3 v diagramu) v objektu BLOB. I když aktualizovaný blok obsahuje stejná data a stejné ID, není stejný jako blok 3 v předchozí verzi. Výsledkem je, že se účtu účtuje čtyři bloky.

![Prostředky Azure Storage](./media/versioning-overview/versions-billing-scenario-2.png)

#### <a name="scenario-3"></a>Scénář 3

Ve scénáři 3 se objekt BLOB aktualizoval, ale jeho verze ne. Blok 3 byl nahrazen blokem 4 v základním objektu blob, ale předchozí verze stále odráží blok 3. Výsledkem je, že se účtu účtuje čtyři bloky.

![Prostředky Azure Storage](./media/versioning-overview/versions-billing-scenario-3.png)

#### <a name="scenario-4"></a>Scénář 4

Ve scénáři 4 byl základní objekt BLOB zcela aktualizován a neobsahuje žádné původní bloky. V důsledku toho se účet účtuje za všechny osm jedinečných bloků &mdash; ve čtyřech základních objektech blob a čtyři v předchozí verzi. K tomuto scénáři může dojít, pokud píšete do objektu BLOB s operací Put blob, protože nahrazuje celý obsah základního objektu BLOB.

![Prostředky Azure Storage](./media/versioning-overview/versions-billing-scenario-4.png)

## <a name="see-also"></a>Viz také

- [Povolení správy verzí objektů blob](versioning-enable.md)
- [Vytvoření snímku objektu BLOB](/rest/api/storageservices/creating-a-snapshot-of-a-blob)
- [Obnovitelné odstranění pro objekty blob Azure Storage](storage-blob-soft-delete.md)
