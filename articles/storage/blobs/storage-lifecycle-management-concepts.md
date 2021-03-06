---
title: Správa životního cyklu Azure Storage
description: Naučte se vytvářet pravidla zásad životního cyklu pro přechod dat o splatnosti z horké na studenou a archivní úroveň.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/24/2020
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.reviewer: yzheng
ms.openlocfilehash: 6285c25c44b7b8c5b2c1d9c148424fc36912b57c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528691"
---
# <a name="manage-the-azure-blob-storage-lifecycle"></a>Správa životního cyklu úložiště objektů blob v Azure

Datové sady mají jedinečné životní cykly. Brzy v životním cyklu lidé k často přistupuje k některým datům. Ale nutnost přístupu se v důsledku stáří dat nevýznamně hodí. Některá data v cloudu zůstala nečinná a jsou po uložení zřídka dostupná. Po vytvoření vyprší platnost některých dat nebo měsíců, zatímco jiné sady dat se aktivně čtou a upravují po celou dobu jejich životnosti. Správa životního cyklu Azure Blob Storage nabízí bohatou zásadu založenou na pravidlech pro účty GPv2 a BLOB Storage. Zásady můžete použít k převodu dat do příslušných úrovní přístupu nebo vypršení jejich platnosti na konci životního cyklu dat.

Zásady správy životního cyklu vám umožňují:

- Pokud chcete optimalizovat výkon a náklady, převeďte objekty blob na studenou vrstvu úložiště (horkou, horkou pro archivaci nebo studenou).
- Odstranění objektů blob na konci jejich životního cyklu
- Definujte pravidla, která se spustí jednou denně na úrovni účtu úložiště.
- Použití pravidel u kontejnerů nebo podmnožiny objektů BLOB (použití předpon názvů nebo [značek indexu objektů BLOB](storage-manage-find-blobs.md) jako filtrů)

Vezměte v úvahu scénář, kdy data budou často přístupná v počátečních fázích životního cyklu, ale pouze občas po dvou týdnech. Po prvním měsíci se k datové sadě používá zřídka. V tomto scénáři je horké úložiště nejlépe v počátečních fázích. Studená úložiště jsou nejvhodnější pro příležitostné přístupy. Archivní úložiště je nejlepší možností po stáří dat za měsíc. Nastavením vrstev úložiště v závislosti na stáří dat můžete navrhnout nejlevnější možnosti úložiště podle vašich potřeb. Pro dosažení tohoto přechodu jsou k dispozici pravidla zásad správy životního cyklu pro přesun dat o splatnosti do úrovní chladiče.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-account-support"></a>Podpora účtu úložiště

Zásady správy životního cyklu jsou dostupné s účty Pro obecné účely v2 (GPv2), účty BLOB Storage a účty úložiště blob bloku úrovně Premium. V Azure Portal můžete upgradovat existující účet Pro obecné účely (GPv1) na účet GPv2. Další informace o účtech úložiště najdete v tématu [Přehled účtu Azure Storage](../common/storage-account-overview.md).  

## <a name="pricing"></a>Ceny

Funkce správy životního cyklu je bezplatná. Zákazníkům se účtují běžné provozní náklady za volání rozhraní API [vrstvy objektů BLOB](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) . Operace odstranění je zadarmo. Další informace o cenách najdete v tématu [ceny za objekty blob bloku](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="regional-availability"></a>Regionální dostupnost

Funkce správy životního cyklu je dostupná ve všech oblastech Azure.

## <a name="add-or-remove-a-policy"></a>Přidat nebo odebrat zásadu

Zásadu můžete přidat, upravit nebo odebrat pomocí kterékoli z následujících metod:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](https://github.com/Azure/azure-powershell/releases)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [REST API](https://docs.microsoft.com/rest/api/storagerp/managementpolicies)

Zásady je možné číst nebo zapisovat v plném rozsahu. Částečné aktualizace nejsou podporovány. 

> [!NOTE]
> Pokud pro svůj účet úložiště povolíte pravidla brány firewall, můžou být požadavky správy životního cyklu blokované. Tyto požadavky můžete odblokovat poskytováním výjimek pro důvěryhodné služby společnosti Microsoft. Další informace najdete v části výjimky v tématu [Konfigurace bran firewall a virtuálních sítí](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

Tento článek popisuje, jak spravovat zásady pomocí portálu a metod PowerShellu.  

# <a name="portal"></a>[Azure Portal](#tab/azure-portal)

Existují dva způsoby, jak přidat zásadu prostřednictvím Azure Portal. 

* [Zobrazení seznamu Azure Portal](#azure-portal-list-view)
* [Azure Portal zobrazení kódu](#azure-portal-code-view)

#### <a name="azure-portal-list-view"></a>Zobrazení seznamu Azure Portal

1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com).

2. V Azure Portal vyhledejte a vyberte svůj účet úložiště. 

3. V části **BLOB Service**vyberte možnost **Správa životního cyklu** , abyste mohli zobrazit nebo změnit vaše pravidla.

4. Vyberte kartu **zobrazení seznamu** .

5. Vyberte **Přidat pravidlo** a potom vyplňte pole formuláře **sady akcí** . V následujícím příkladu jsou objekty blob přesunuté do studeného úložiště, pokud se nezměnily po dobu 30 dnů.

   ![Stránka sady akcí správy životního cyklu v Azure Portal](media/storage-lifecycle-management-concepts/lifecycle-management-action-set.png)

6. Výběrem **filtru sada** přidejte volitelný filtr. Pak vyberte **Procházet** a určete kontejner a složku, podle které chcete filtrovat.

   ![Stránka sady filtru správy životního cyklu v Azure Portal](media/storage-lifecycle-management-concepts/lifecycle-management-filter-set-browse.png)

8. Vyberte možnost **zkontrolovat a přidat** a zkontrolujte nastavení zásad.

9. Pokud chcete přidat novou zásadu, vyberte **Přidat** .

#### <a name="azure-portal-code-view"></a>Azure Portal zobrazení kódu
1. Přihlaste se k [portálu Azure Portal](https://portal.azure.com).

2. V Azure Portal vyhledejte a vyberte svůj účet úložiště.

3. V části **BLOB Service**vyberte pro zobrazení nebo změnu zásad možnost **Správa životního cyklu** .

4. Následující JSON je příkladem zásady, kterou lze vložit do karty **zobrazení kódu** .

   ```json
   {
     "rules": [
       {
         "name": "ruleFoo",
         "enabled": true,
         "type": "Lifecycle",
         "definition": {
           "filters": {
             "blobTypes": [ "blockBlob" ],
             "prefixMatch": [ "container1/foo" ]
           },
           "actions": {
             "baseBlob": {
               "tierToCool": { "daysAfterModificationGreaterThan": 30 },
               "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
               "delete": { "daysAfterModificationGreaterThan": 2555 }
             },
             "snapshot": {
               "delete": { "daysAfterCreationGreaterThan": 90 }
             }
           }
         }
       }
     ]
   }
   ```

5. Vyberte **Uložit**.

6. Další informace o tomto příkladu JSON najdete v částech [zásady](#policy) a [pravidla](#rules) .

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

K přidání zásad do účtu úložiště se dá použít následující skript PowerShellu. `$rgname`Proměnná musí být inicializována s názvem vaší skupiny prostředků. `$accountName`Proměnná musí být inicializována s názvem vašeho účtu úložiště.

```powershell
#Install the latest module
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$rgname = ""
$accountName = ""

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd

#Create a new rule object
#PowerShell automatically sets Type as “Lifecycle” because it is the only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1
```

# <a name="template"></a>[Šablona](#tab/template)

Správu životního cyklu můžete definovat pomocí Azure Resource Manager šablon. Tady je Ukázková šablona pro nasazení účtu úložiště RA-GRS GPv2 se zásadami správy životního cyklu.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "storageAccountName": "[uniqueString(resourceGroup().id)]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-04-01",
      "sku": {
        "name": "Standard_RAGRS"
      },
      "kind": "StorageV2",
      "properties": {
        "networkAcls": {}
      }
    },
    {
      "name": "[concat(variables('storageAccountName'), '/default')]",
      "type": "Microsoft.Storage/storageAccounts/managementPolicies",
      "apiVersion": "2019-04-01",
      "dependsOn": [
        "[variables('storageAccountName')]"
      ],
      "properties": {
        "policy": {...}
      }
    }
  ],
  "outputs": {}
}
```

---

## <a name="policy"></a>Zásady

Zásady správy životního cyklu jsou kolekce pravidel v dokumentu JSON:

```json
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```

Zásada je kolekcí pravidel:

| Název parametru | Typ parametru | Poznámky |
|----------------|----------------|-------|
| `rules`        | Pole objektů pravidel | V zásadě je vyžadováno alespoň jedno pravidlo. V zásadách můžete definovat až 100 pravidel.|

Každé pravidlo v zásadě má několik parametrů:

| Název parametru | Typ parametru | Poznámky | Vyžadováno |
|----------------|----------------|-------|----------|
| `name`         | Řetězec |Název pravidla může obsahovat až 256 alfanumerických znaků. Název pravidla rozlišuje velká a malá písmena.  Musí být jedinečný v rámci zásad. | Ano |
| `enabled`      | Logická hodnota | Volitelná logická hodnota, která povolí dočasné vypnutí pravidla. Výchozí hodnota je true, pokud není nastavena. | Nepravda | 
| `type`         | Hodnota výčtu | Aktuální platný typ je `Lifecycle` . | Ano |
| `definition`   | Objekt definující pravidlo životního cyklu | Každá definice se skládá ze sady filtrů a sady akcí. | Ano |

## <a name="rules"></a>Pravidla

Každá definice pravidla obsahuje sadu filtrů a sadu akcí. [Sada filtr](#rule-filters) omezuje akce pravidla na určitou sadu objektů v rámci názvů kontejnerů nebo objektů. [Sada akcí](#rule-actions) aplikuje akce vrstvy nebo odstranění na filtrovanou sadu objektů.

### <a name="sample-rule"></a>Ukázkové pravidlo

Následující ukázkové pravidlo filtruje účet, aby spouštěl akce na objektech, které existují uvnitř `container1` a začínají na `foo` .  

>[!NOTE]
>Správa životního cyklu podporuje jenom typ objektu blob bloku.  

- Úroveň objektu BLOB na studenou vrstvu 30 dní od poslední změny
- Úroveň objektu BLOB na archivní vrstvu 90 dní od poslední změny
- Odstranit objekt BLOB 2 555 dní (sedm let) po poslední úpravě
- Odstranit snímky objektů BLOB 90 dní po vytvoření snímku

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="rule-filters"></a>Filtry pravidla

Filtruje akce pravidla omezení na podmnožinu objektů BLOB v rámci účtu úložiště. Pokud je definován více než jeden filtr, logické `AND` spuštění na všech filtrech.

Filtry zahrnují:

| Název filtru | Typ filtru | Poznámky | Je povinné |
|-------------|-------------|-------|-------------|
| blobTypes   | Pole předdefinovaných hodnot výčtu. | Aktuální verze podporuje `blockBlob` . | Ano |
| prefixMatch | Pole řetězců, pro které mají být předpony spárovány. Každé pravidlo může definovat až 10 předpon. Řetězec předpony musí začínat názvem kontejneru. Například pokud chcete, aby se všechny objekty blob shodovaly v rámci `https://myaccount.blob.core.windows.net/container1/foo/...` pravidla, prefixMatch je `container1/foo` . | Pokud prefixMatch nedefinujete, pravidlo se použije na všechny objekty BLOB v účtu úložiště.  | No |
| blobIndexMatch | Pole hodnot slovníku sestávající z klíče značek indexu objektu BLOB a podmínky hodnoty, které mají být porovnány. Každé pravidlo může definovat až 10 stavových značek indexu objektu BLOB. Například pokud chcete, aby se všechny objekty blob shodovaly s `Project = Contoso` v rámci `https://myaccount.blob.core.windows.net/` pro pravidlo, je blobIndexMatch `{"name": "Project","op": "==","value": "Contoso"}` . | Pokud blobIndexMatch nedefinujete, pravidlo se použije na všechny objekty BLOB v účtu úložiště. | No |

> [!NOTE]
> Index objektu BLOB je ve verzi Public Preview a je dostupný v oblastech **Francie – střed** a Francie – **jih** . Další informace o této funkci spolu se známými problémy a omezeních najdete v tématu [Správa a hledání dat v Azure Blob Storage s využitím indexu objektů BLOB (Preview)](storage-manage-find-blobs.md).

### <a name="rule-actions"></a>Akce pravidla

Akce se aplikují na filtrované objekty BLOB při splnění podmínky spuštění.

Správa životního cyklu podporuje vrstvení a odstraňování objektů BLOB a odstraňování snímků objektů BLOB. Pro každé pravidlo pro objekty blob nebo snímky objektů BLOB definujte alespoň jednu akci.

| Akce        | Základní objekt BLOB                                   | Snímek      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Podpora objektů BLOB v současnosti v úrovni Hot         | Nepodporováno |
| tierToArchive | Podpora blobů v současnosti na horké nebo studené úrovni | Nepodporováno |
| odstranění        | Podporováno                                   | Podporováno     |

>[!NOTE]
>Pokud definujete více než jednu akci u stejného objektu blob, bude správa životního cyklu v objektu BLOB platit nejméně náročná akce. Například akce `delete` je levnější než akce `tierToArchive` . Akce `tierToArchive` je levnější než akce `tierToCool` .

Podmínky spuštění jsou založené na stáří. Základní objekty blob používají čas poslední změny ke sledování stáří a snímky objektů BLOB používají čas vytvoření snímku ke sledování stáří.

| Podmínka spuštění akce             | Hodnota podmínky                          | Popis                             |
|----------------------------------|------------------------------------------|-----------------------------------------|
| daysAfterModificationGreaterThan | Celočíselná hodnota označující stáří ve dnech | Podmínka pro základní akce objektů BLOB     |
| daysAfterCreationGreaterThan     | Celočíselná hodnota označující stáří ve dnech | Podmínka pro akce snímku objektu BLOB |

## <a name="examples"></a>Příklady

Následující příklady ukazují, jak řešit běžné scénáře s pravidly zásad životního cyklu.

### <a name="move-aging-data-to-a-cooler-tier"></a>Přesunout data o stárnutí do úrovně chladicího procesu

Tento příklad ukazuje, jak převést objekty blob bloku s předponou `container1/foo` nebo `container2/bar` . Zásady přemění objekty blob, které se nezměnily během více než 30 dní, do studeného úložiště, a objekty BLOB se v 90 dnech nezměnily na úroveň archivu:

```json
{
  "rules": [
    {
      "name": "agingRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo", "container2/bar" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="archive-data-after-ingest"></a>Archivovat data po ingestování

Některá data v cloudu zůstanou nečinná a v případě potřeby jsou po uložení k dispozici zřídka. Následující zásady životního cyklu jsou nakonfigurovány k archivaci dat krátce po ingestování. Tento příklad přechází objekty blob bloku v účtu úložiště v kontejneru `archivecontainer` do archivní úrovně. Přechod se provádí na objektech blob 0 dní od poslední změny:

> [!NOTE] 
> Doporučujeme nahrát objekty blob přímo do archivní vrstvy, aby byly efektivnější. Můžete použít záhlaví x-MS-Access-úrovně pro [PutBlob](https://docs.microsoft.com/rest/api/storageservices/put-blob) nebo [PutBlockList](https://docs.microsoft.com/rest/api/storageservices/put-block-list) s REST verze 2018-11-09 a novějšími nebo nejnovějšími klientskými knihovnami pro úložiště objektů BLOB. 

```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "archivecontainer" ]
        },
        "actions": {
          "baseBlob": {
              "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
          }
        }
      }
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Vypršení platnosti dat na základě stáří

U některých dat se očekává, že vyprší platnost dnů nebo měsíců po vytvoření. Zásady správy životního cyklu můžete nakonfigurovat tak, aby data prošla odstraněním na základě stáří dat. Následující příklad ukazuje zásadu, která odstraní všechny objekty blob bloku starší než 365 dní.

```json
{
  "rules": [
    {
      "name": "expirationRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

### <a name="delete-data-with-blob-index-tags"></a>Odstranění dat pomocí značek indexu objektu BLOB
Některá data by měla být vyhodnocena pouze v případě, že jsou výslovně označeny k odstranění. Můžete nakonfigurovat zásady správy životního cyklu tak, aby prošly daty, která jsou označená atributy klíč/hodnota indexu objektu BLOB. Následující příklad ukazuje zásadu, která odstraní všechny objekty blob bloku s příznakem `Project = Contoso` . Další informace o indexu objektů BLOB najdete v tématu [Správa a hledání dat v Azure Blob Storage s využitím indexu objektů BLOB (Preview)](storage-manage-find-blobs.md).

```json
{
    "rules": [
        {
            "enabled": true,
            "name": "DeleteContosoData",
            "type": "Lifecycle",
            "definition": {
                "actions": {
                    "baseBlob": {
                        "delete": {
                            "daysAfterModificationGreaterThan": 0
                        }
                    }
                },
                "filters": {
                    "blobIndexMatch": [
                        {
                            "name": "Project",
                            "op": "==",
                            "value": "Contoso"
                        }
                    ],
                    "blobTypes": [
                        "blockBlob"
                    ]
                }
            }
        }
    ]
}
```

### <a name="delete-old-snapshots"></a>Odstranit staré snímky

Pro data, která se pravidelně upravují a přibývají k nim přistupovaly během své životnosti, se snímky často používají ke sledování starších verzí dat. Můžete vytvořit zásadu, která odstraní staré snímky na základě stáří snímku. Stáří snímku je určeno vyhodnocením času vytvoření snímku. Toto pravidlo zásad odstraní snímky objektů blob bloku v rámci kontejneru `activedata` , které jsou 90 dní nebo starší po vytvoření snímku.

```json
{
  "rules": [
    {
      "name": "snapshotRule",
      "enabled": true,
      "type": "Lifecycle",
    "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "activedata" ]
        },
        "actions": {
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

## <a name="faq"></a>Nejčastější dotazy

**Vytvořili jsem novou zásadu, proč se akce nespouštějí hned?**  
Platforma spouští zásady životního cyklu jednou denně. Po nakonfigurování zásady může trvat až 24 hodin, než se některé akce poprvé spustí.  

**Pokud aktualizujem existující zásady, jak dlouho trvá, než se akce spustí?**  
Aktualizovaná zásada trvá až 24 hodin, než se dostanou platit. Jakmile se zásada uplatní, může trvat až 24 hodin, než se akce spustí. Proto může dokončení akcí zásad trvat až 48 hodin.   

**Jak zabráním ručnímu recyklení archivovaného objektu blob, jak brání jeho přesunutí do archivní úrovně dočasně?**  
Když se objekt BLOB přesune z jedné úrovně přístupu na jiný, čas poslední změny se nezmění. Pokud jste archivovaný objekt BLOB ručně znovu vypnuli do vrstvy Hot, bude se ho modul pro správu životního cyklu přesunout zpátky do archivní úrovně. Zakažte pravidlo, které bude mít dočasně vliv na tento objekt blob, aby se zabránilo jeho archivaci znovu. Znovu povolí pravidlo, když se dá objekt BLOB bezpečně přesunout zpátky do archivní úrovně. Objekt blob můžete také zkopírovat do jiného umístění, pokud je potřeba udržet se trvale na horké nebo studené úrovni.

## <a name="next-steps"></a>Další kroky

Přečtěte si, jak obnovit data po náhodném odstranění:

- [Obnovitelné odstranění objektů blob služby Azure Storage](../blobs/storage-blob-soft-delete.md)

Naučte se spravovat a vyhledávat data pomocí indexu objektů BLOB:

- [Správa a hledání dat v Azure Blob Storage s využitím indexu objektů BLOB](storage-manage-find-blobs.md)
