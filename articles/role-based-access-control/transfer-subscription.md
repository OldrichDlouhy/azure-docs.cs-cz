---
title: Přenos předplatného Azure do jiného adresáře Azure AD (Preview)
description: Přečtěte si, jak přenést předplatné Azure a známé související prostředky do jiného adresáře Azure Active Directory (Azure AD).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.workload: identity
ms.date: 07/01/2020
ms.author: rolyon
ms.openlocfilehash: 664687d096a3a9c6ce9a6c7de0025604e046b0a1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029973"
---
# <a name="transfer-an-azure-subscription-to-a-different-azure-ad-directory-preview"></a>Přenos předplatného Azure do jiného adresáře Azure AD (Preview)

> [!IMPORTANT]
> Pomocí těchto kroků můžete přenést předplatné do jiného adresáře služby Azure AD v současnosti ve verzi Public Preview.
> Tato verze Preview se poskytuje bez smlouvy o úrovni služeb a nedoporučuje se pro úlohy v produkčním prostředí. Některé funkce se nemusí podporovat nebo mohou mít omezené možnosti.
> Další informace najdete v [dodatečných podmínkách použití pro verze Preview v Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Organizace můžou mít několik předplatných Azure. Každé předplatné je přidruženo ke konkrétnímu adresáři Azure Active Directory (Azure AD). Chcete-li zjednodušit správu, můžete chtít převést předplatné na jiný adresář služby Azure AD. Když přenášíte předplatné do jiného adresáře služby Azure AD, některé prostředky se nepřenesou do cílového adresáře. Například všechna přiřazení rolí a vlastní role v řízení přístupu na základě role Azure (Azure RBAC) se **trvale** odstraní ze zdrojového adresáře a nepřesunou se do cílového adresáře.

Tento článek popisuje základní kroky, pomocí kterých můžete přenést předplatné do jiného adresáře služby Azure AD a znovu vytvořit některé prostředky po přenosu.

## <a name="overview"></a>Přehled

Převod předplatného Azure na jiný adresář služby Azure AD je složitý proces, který musí být pečlivě naplánován a proveden. Mnoho služeb Azure vyžaduje, aby objekty zabezpečení (identity) fungovaly normálně nebo dokonce i spravovaly jiné prostředky Azure. Tento článek se snaží pokrýt většinu služeb Azure, které jsou silně závislé na objektech zabezpečení, ale nejsou vyčerpávající.

> [!IMPORTANT]
> Převod předplatného vyžaduje ukončení procesu.

Následující diagram znázorňuje základní kroky, které je třeba provést při přenosu odběru do jiného adresáře.

1. Příprava na přenos

1. Převod vlastnictví fakturace předplatného Azure na jiný účet

1. Opětovné vytvoření prostředků v cílovém adresáři, například přiřazení rolí, vlastní role a spravované identity

    ![Přenést diagram odběru](./media/transfer-subscription/transfer-subscription.png)

### <a name="deciding-whether-to-transfer-a-subscription-to-a-different-directory"></a>Rozhodnutí, jestli se má přenést předplatné do jiného adresáře

Níže jsou uvedeny některé důvody, proč byste mohli chtít přenést předplatné:

- V důsledku fúze nebo akvizice společnosti budete chtít spravovat nakoupené předplatné v primárním adresáři služby Azure AD.
- Někdo ve vaší organizaci vytvořil předplatné a chcete konsolidovat správu do konkrétního adresáře Azure AD.
- Máte aplikace, které závisí na konkrétním ID předplatného nebo adrese URL a nemůžete ji snadno změnit.
- Část vaší firmy byla rozdělena do samostatné společnosti a potřebujete přesunout některé z vašich prostředků do jiného adresáře služby Azure AD.
- Chcete spravovat některé z vašich prostředků v jiném adresáři služby Azure AD pro účely izolace zabezpečení.

Převod předplatného vyžaduje ukončení procesu. V závislosti na vašem scénáři může být vhodnější pouze znovu vytvořit prostředky a kopírovat data do cílového adresáře a předplatného.

### <a name="understand-the-impact-of-transferring-a-subscription"></a>Pochopení dopadu na převod předplatného

Několik prostředků Azure má závislost na předplatném nebo adresáři. V závislosti na vaší situaci uvádí následující tabulka známý dopad na převod předplatného. Provedením kroků v tomto článku můžete znovu vytvořit některé z prostředků, které existovaly před přenosem předplatného.

> [!IMPORTANT]
> V této části jsou uvedené známé služby Azure nebo prostředky, které jsou závislé na vašem předplatném. Vzhledem k tomu, že typy prostředků v Azure se neustále vyvíjí, můžou existovat další závislosti, které tady nejsou uvedené, což může způsobit zásadní změnu prostředí. 

| Služba nebo prostředek | Ovlivněné | Obnovitelné | Máte vliv na to? | Co můžete dělat |
| --------- | --------- | --------- | --------- | --------- |
| Přiřazení rolí | Ano | Yes | [Zobrazení seznamu přiřazení rolí](#save-all-role-assignments) | Všechna přiřazení rolí se trvale odstraní. Je nutné mapovat uživatele, skupiny a instanční objekty k odpovídajícím objektům v cílovém adresáři. Je nutné znovu vytvořit přiřazení rolí. |
| Vlastní role | Ano | Yes | [Výpis vlastních rolí](#save-custom-roles) | Všechny vlastní role se trvale odstraní. Je nutné znovu vytvořit vlastní role a jakékoli přiřazení rolí. |
| Spravované identity přiřazené systémem | Ano | Yes | [Výpis spravovaných identit](#list-role-assignments-for-managed-identities) | Je nutné zakázat a znovu povolit spravované identity. Je nutné znovu vytvořit přiřazení rolí. |
| Spravované identity přiřazené uživatelem | Ano | Yes | [Výpis spravovaných identit](#list-role-assignments-for-managed-identities) | Spravované identity musíte odstranit, znovu vytvořit a připojit k příslušnému prostředku. Je nutné znovu vytvořit přiřazení rolí. |
| Azure Key Vault | Ano | Yes | [Seznam Key Vault zásad přístupu](#list-other-known-resources) | Je nutné aktualizovat ID tenanta přidruženého k trezorům klíčů. Je nutné odebrat a přidat nové zásady přístupu. |
| Databáze SQL Azure s ověřováním Azure AD | Yes | No | [Ověření databází Azure SQL pomocí ověřování Azure AD](#list-other-known-resources) |  |  |
| Azure Storage a Azure Data Lake Storage Gen2 | Ano | Yes |  | Je nutné znovu vytvořit všechny seznamy ACL. |
| Azure Data Lake Storage Gen1 | Ano |  |  | Je nutné znovu vytvořit všechny seznamy ACL. |
| Azure Files | Ano | Yes |  | Je nutné znovu vytvořit všechny seznamy ACL. |
| Synchronizace souborů Azure | Ano | Yes |  |  |
| Spravované disky Azure | Yes | – |  |  |
| Azure Container Services pro Kubernetes | Ano | Yes |  |  |
| Azure Active Directory Domain Services | Yes | No |  |  |
| Registrace aplikací | Ano | Ano |  |  |

Pokud používáte šifrování v klidovém umístění pro určitý prostředek, jako je například účet úložiště nebo databáze SQL, která má závislost na trezoru klíčů, který není ve stejném předplatném, které se přenáší, může vést k neodstranitelné situaci. Pokud máte tuto situaci, měli byste podniknout kroky k použití jiného trezoru klíčů nebo k dočasnému zakázání klíčů spravovaných zákazníkem, abyste se vyhnuli tomuto neopravitelnému scénáři.

## <a name="prerequisites"></a>Předpoklady

K provedení těchto kroků budete potřebovat:

- [Bash v Azure Cloud Shell](/azure/cloud-shell/overview) nebo [Azure CLI](https://docs.microsoft.com/cli/azure)
- Správce účtu předplatného, které chcete přenést do zdrojového adresáře
- Role [vlastníka](built-in-roles.md#owner) v cílovém adresáři

## <a name="step-1-prepare-for-the-transfer"></a>Krok 1: Příprava na přenos

### <a name="sign-in-to-source-directory"></a>Přihlaste se ke zdrojovému adresáři

1. Přihlaste se k Azure jako správce.

1. Seznam vašich předplatných získáte pomocí příkazu [AZ Account list](/cli/azure/account#az-account-list) .

    ```azurecli
    az account list --output table
    ```

1. Pomocí [AZ Account set](https://docs.microsoft.com/cli/azure/account#az-account-set) nastavte aktivní předplatné, které chcete přenést.

    ```azurecli
    az account set --subscription "Marketing"
    ```

### <a name="install-the-resource-graph-extension"></a>Instalace rozšíření Resource-Graph Extension

 Rozšíření Resource-Graph umožňuje použít příkaz [AZ Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) k dotazování prostředků spravovaných pomocí Azure Resource Manager. Tento příkaz použijete v pozdějších krocích.

1. Chcete-li zjistit, zda je nainstalováno rozšíření *grafu prostředků* , použijte příkaz [AZ Extension List](https://docs.microsoft.com/cli/azure/extension#az-extension-list) .

    ```azurecli
    az extension list
    ```

1. Pokud ne, nainstalujte rozšíření *Resource-Graph* .

    ```azurecli
    az extension add --name resource-graph
    ```

### <a name="save-all-role-assignments"></a>Uložit všechna přiřazení rolí

1. K vypsání všech přiřazení rolí (včetně zděděných přiřazení rolí) použijte [seznam AZ role Assignment](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-list) .

    Chcete-li usnadnit kontrolu seznamu, můžete výstup exportovat jako JSON, TSV nebo tabulku. Další informace najdete v tématu [přiřazení rolí k seznamům pomocí Azure RBAC a Azure CLI](role-assignments-list-cli.md).

    ```azurecli
    az role assignment list --all --include-inherited --output json > roleassignments.json
    az role assignment list --all --include-inherited --output tsv > roleassignments.tsv
    az role assignment list --all --include-inherited --output table > roleassignments.txt
    ```

1. Uložte seznam přiřazení rolí.

    Když přenášíte předplatné, všechna přiřazení rolí se **trvale** odstraní, takže je důležité uložit kopii.

1. Zkontrolujte seznam přiřazení rolí. Může se jednat o přiřazení rolí, která nebudete potřebovat v cílovém adresáři.

### <a name="save-custom-roles"></a>Uložení vlastních rolí

1. K vypsání vlastních rolí použijte [seznam AZ role definition](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-list) . Další informace najdete v tématu [Vytvoření nebo aktualizace vlastních rolí Azure pomocí Azure CLI](custom-roles-cli.md).

    ```azurecli
    az role definition list --custom-role-only true --output json --query '[].{roleName:roleName, roleType:roleType}'
    ```

1. Uložte jednotlivé vlastní role, které budete potřebovat v cílovém adresáři, jako samostatný soubor JSON.

    ```azurecli
    az role definition list --name <custom_role_name> > customrolename.json
    ```

1. Vytvořte kopie souborů vlastních rolí.

1. Upravte každou kopii tak, aby používala následující formát.

    Později tyto soubory použijete k opětovnému vytvoření vlastních rolí v cílovém adresáři.

    ```json
    {
      "Name": "",
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": []
    }
    ```

### <a name="determine-user-group-and-service-principal-mappings"></a>Určení mapování objektů uživatele, skupiny a služby

1. Na základě vašeho seznamu přiřazení rolí Určete uživatele, skupiny a instanční objekty, které budete mapovat do cílového adresáře.

    Typ objektu zabezpečení můžete identifikovat tak, že `principalType` v každém přiřazení role prohlížíte vlastnost.

1. V případě potřeby vytvořte v cílovém adresáři všechny uživatele, skupiny nebo instanční objekty, které budete potřebovat.

### <a name="list-role-assignments-for-managed-identities"></a>Seznam přiřazení rolí pro spravované identity

Spravované identity se při přenosu předplatného do jiného adresáře neaktualizují. V důsledku toho budou přerušeny všechny existující spravované identity přiřazené systémem nebo uživatelem. Po přenosu můžete znovu povolit všechny spravované identity přiřazené systémem. Pro uživatelem přiřazené spravované identity bude nutné je znovu vytvořit a připojit v cílovém adresáři.

1. Projděte si [seznam služeb Azure, které podporují spravované identity,](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) abyste si poznamenali, kde možná používáte spravované identity.

1. Pomocí příkaz [AZ AD SP list](/cli/azure/identity?view=azure-cli-latest#az-identity-list) můžete zobrazit seznam spravovaných identit přiřazených systémem a uživatelem.

    ```azurecli
    az ad sp list --all --filter "servicePrincipalType eq 'ManagedIdentity'"
    ```

1. V seznamu spravovaných identit určete, která z nich je přiřazena k systému a která jsou přiřazená uživateli. K určení typu můžete použít následující kritéria.

    | Kritéria | Typ spravované identity |
    | --- | --- |
    | `alternativeNames`vlastnost obsahuje`isExplicit=False` | Přiřazeno systémem |
    | `alternativeNames`vlastnost nezahrnuje`isExplicit` | Přiřazeno systémem |
    | `alternativeNames`vlastnost obsahuje`isExplicit=True` | Přiřazeno uživatelem |

    Můžete taky použít příkaz [AZ identity list](https://docs.microsoft.com/cli/azure/identity#az-identity-list) , který vypíše uživatelem přiřazené identity. Další informace najdete v tématu [Vytvoření, vypsání nebo odstranění spravované identity přiřazené uživatelem pomocí Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md).

    ```azurecli
    az identity list
    ```

1. Získejte seznam `objectId` hodnot spravovaných identit.

1. Hledáním v seznamu přiřazení rolí zjistíte, jestli pro spravované identity neexistují žádné přiřazení rolí.

### <a name="list-key-vaults"></a>Vypsat trezory klíčů

Když vytvoříte Trezor klíčů, je automaticky svázán s výchozím ID klienta Azure Active Directory pro předplatné, ve kterém je vytvořený. Zároveň jsou k tomuto ID tenanta vázány i všechny položky zásad přístupu. Další informace najdete v tématu [přesun Azure Key Vault do jiného předplatného](../key-vault/general/move-subscription.md).

> [!WARNING]
> Pokud používáte šifrování v klidovém umístění pro určitý prostředek, jako je například účet úložiště nebo databáze SQL, která má závislost na trezoru klíčů, který není ve stejném předplatném, které se přenáší, může vést k neodstranitelné situaci. Pokud máte tuto situaci, měli byste podniknout kroky k použití jiného trezoru klíčů nebo k dočasnému zakázání klíčů spravovaných zákazníkem, abyste se vyhnuli tomuto neopravitelnému scénáři.

- Pokud máte Trezor klíčů, použijte příkaz [AZ Key trezor show k zobrazení](https://docs.microsoft.com/cli/azure/keyvault#az-keyvault-show) seznamu zásad přístupu. Další informace najdete v tématu [poskytnutí Key Vault ověřování pomocí zásad řízení přístupu](../key-vault/key-vault-group-permissions-for-apps.md).

    ```azurecli
    az keyvault show --name MyKeyVault
    ```

### <a name="list-azure-sql-databases-with-azure-ad-authentication"></a>Vypsání databází Azure SQL pomocí ověřování Azure AD

- Pomocí [AZ SQL Server AD – admin list](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) a [AZ Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) Extension zjistíte, jestli používáte databáze SQL Azure s ověřováním Azure AD. Další informace najdete v tématu [Konfigurace a Správa ověřování Azure Active Directory pomocí SQL](../sql-database/sql-database-aad-authentication-configure.md).

    ```azurecli
    az sql server ad-admin list --ids $(az graph query -q 'resources | where type == "microsoft.sql/servers" | project id' -o tsv | cut -f1)
    ```

### <a name="list-acls"></a>Seznam seznamů ACL

1. Pokud používáte Azure Data Lake Storage Gen1, Seznamte se s seznamy ACL, které jsou použity pro libovolný soubor pomocí Azure Portal nebo PowerShellu.

1. Pokud používáte Azure Data Lake Storage Gen2, Seznamte se s seznamy ACL, které jsou použity pro libovolný soubor pomocí Azure Portal nebo PowerShellu.

1. Pokud používáte soubory Azure, Seznamte se s seznamy ACL, které jsou použity pro libovolný soubor.

### <a name="list-other-known-resources"></a>Výpis dalších známých prostředků

1. K získání ID předplatného použijte [AZ Account show](https://docs.microsoft.com/cli/azure/account#az-account-show) .

    ```azurecli
    subscriptionId=$(az account show --query id | sed -e 's/^"//' -e 's/"$//')
    ```

1. K vypsání dalších prostředků Azure se známými závislostmi adresáře Azure AD použijte rozšíření [AZ Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) Extension.

    ```azurecli
    az graph query -q \
    'resources | where type != "microsoft.azureactivedirectory/b2cdirectories" | where  identity <> "" or properties.tenantId <> "" or properties.encryptionSettingsCollection.enabled == true | project name, type, kind, identity, tenantId, properties.tenantId' \
    --subscriptions $subscriptionId --output table
    ```

## <a name="step-2-transfer-billing-ownership"></a>Krok 2: přenos vlastnictví fakturace

V tomto kroku převedete vlastnictví fakturace předplatného ze zdrojového adresáře do cílového adresáře.

> [!WARNING]
> Při přenosu vlastnictví fakturace předplatného se všechna přiřazení rolí ve zdrojovém adresáři **trvale** odstraní a nelze je obnovit. Až převedete vlastnictví fakturace předplatného, nemůžete se vrátit zpátky. Před provedením tohoto kroku se ujistěte, že jste dokončili předchozí kroky.

1. Postupujte podle kroků v části [přenos vlastnictví fakturace předplatného Azure na jiný účet](../cost-management-billing/manage/billing-subscription-transfer.md). Pokud chcete přenést předplatné do jiného adresáře služby Azure AD, musíte zaškrtnout políčko **předplatné služby Azure AD** .

1. Po dokončení převodu vlastnictví se vraťte k tomuto článku a znovu vytvořte prostředky v cílovém adresáři.

## <a name="step-3-re-create-resources"></a>Krok 3: opětovné vytvoření prostředků

### <a name="sign-in-to-target-directory"></a>Přihlásit se k cílovému adresáři

1. V cílovém adresáři se přihlaste jako uživatel, který požadavek na přenos přijal.

    Přístup ke správě prostředků bude mít jenom uživatel v novém účtu, který žádost o přenos přijal.

1. Seznam vašich předplatných získáte pomocí příkazu [AZ Account list](https://docs.microsoft.com/cli/azure/account#az-account-list) .

    ```azurecli
    az account list --output table
    ```

1. Pomocí [AZ Account set](https://docs.microsoft.com/cli/azure/account#az-account-set) nastavte aktivní předplatné, které chcete použít.

    ```azurecli
    az account set --subscription "Contoso"
    ```

### <a name="create-custom-roles"></a>Vytváření vlastních rolí
        
- Pomocí [AZ role definition Create vytvořte](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-create) vytvořte jednotlivé vlastní role ze souborů, které jste vytvořili dříve. Další informace najdete v tématu [Vytvoření nebo aktualizace vlastních rolí Azure pomocí Azure CLI](custom-roles-cli.md).

    ```azurecli
    az role definition create --role-definition <role_definition>
    ```

### <a name="create-role-assignments"></a>Vytvoření přiřazení rolí

- Pomocí [AZ role Assignment Create vytvořte](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) přiřazení rolí pro uživatele, skupiny a instanční objekty. Další informace najdete v tématu [Přidání nebo odebrání přiřazení rolí pomocí Azure RBAC a Azure CLI](role-assignments-cli.md).

    ```azurecli
    az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
    ```

### <a name="update-system-assigned-managed-identities"></a>Aktualizace spravovaných identit přiřazených systémem

1. Zakažte a znovu povolte spravované identity přiřazené systémem.

    | Služba Azure | Další informace | 
    | --- | --- |
    | Virtuální počítače | [Konfigurace spravovaných identit pro prostředky Azure na virtuálním počítači Azure pomocí Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#system-assigned-managed-identity) |
    | Škálovací sady virtuálních počítačů | [Konfigurace spravovaných identit pro prostředky Azure v sadě škálování virtuálního počítače pomocí Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#system-assigned-managed-identity) |
    | Další služby | [Služby, které podporují spravované identity prostředků Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) |

1. Pomocí [AZ role Assignment Create vytvořte](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) přiřazení rolí pro spravované identity přiřazené systémem. Další informace najdete v tématu [přiřazení přístupu spravované identity k prostředku pomocí Azure CLI](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-user-assigned-managed-identities"></a>Aktualizace spravovaných identit přiřazených uživatelem

1. Odstraňte, znovu vytvořte a připojte spravované identity přiřazené uživatelem.

    | Služba Azure | Další informace | 
    | --- | --- |
    | Virtuální počítače | [Konfigurace spravovaných identit pro prostředky Azure na virtuálním počítači Azure pomocí Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity) |
    | Škálovací sady virtuálních počítačů | [Konfigurace spravovaných identit pro prostředky Azure v sadě škálování virtuálního počítače pomocí Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#user-assigned-managed-identity) |
    | Další služby | [Služby, které podporují spravované identity prostředků Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)<br/>[Vytvoření, vypsání nebo odstranění spravované identity přiřazené uživatelem pomocí Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) |

1. Pomocí [AZ role Assignment Create vytvořte](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) přiřazení rolí pro spravované identity přiřazené uživatelem. Další informace najdete v tématu [přiřazení přístupu spravované identity k prostředku pomocí Azure CLI](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-key-vaults"></a>Aktualizovat trezory klíčů

Tato část popisuje základní kroky pro aktualizaci trezorů klíčů. Další informace najdete v tématu [přesun Azure Key Vault do jiného předplatného](../key-vault/general/move-subscription.md).

1. Aktualizujte ID tenanta přidružené ke všem stávajícím trezorům klíčů v rámci předplatného do cílového adresáře.

1. Odeberte všechny stávající položky zásad přístupu.

1. Přidejte nové položky zásad přístupu přidružené k cílovému adresáři.

### <a name="update-acls"></a>Aktualizace seznamů ACL

1. Pokud používáte Azure Data Lake Storage Gen1, přiřaďte příslušné seznamy ACL. Další informace najdete v tématu [zabezpečení dat uložených v Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

1. Pokud používáte Azure Data Lake Storage Gen2, přiřaďte příslušné seznamy ACL. Další informace najdete v tématu [řízení přístupu v Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

1. Pokud používáte soubory Azure, přiřaďte příslušné seznamy ACL.

### <a name="review-other-security-methods"></a>Kontrola dalších metod zabezpečení

I když jsou přiřazení rolí během přenosu odebrána, můžou uživatelé v původním účtu vlastníka dál mít přístup k předplatnému prostřednictvím jiných metod zabezpečení, včetně těchto:

- Přístupové klíče pro služby, jako je Storage.
- [Certifikáty pro správu](../cloud-services/cloud-services-certs-create.md) , které udělují správcům přístup k prostředkům předplatného.
- Oprávnění pro vzdálený přístup ke službám, jako je Azure Virtual Machines.

Pokud je vaším záměrem odebrat přístup uživatelů ve zdrojovém adresáři, aby k nim nemají přístup v cílovém adresáři, měli byste zvážit, jestli máte všechna přihlašovací údaje otočit. Dokud se přihlašovací údaje neaktualizují, budou mít uživatelé po přenosu přístup i nadále.

1. Otočte přístupové klíče účtu úložiště. Další informace najdete v tématu [Správa přístupových klíčů účtu úložiště](../storage/common/storage-account-keys-manage.md).

1. Pokud používáte přístupové klíče pro jiné služby, například Azure SQL Database nebo Azure Service Bus zasílání zpráv, přetočit přístupové klávesy.

1. Pro prostředky, které používají tajné kódy, otevřete nastavení prostředku a aktualizujte tajný klíč.

1. Pro prostředky, které používají certifikáty, aktualizujte certifikát.

## <a name="next-steps"></a>Další kroky

- [Převod vlastnictví fakturace předplatného Azure na jiný účet](../cost-management-billing/manage/billing-subscription-transfer.md)
- [Přenos předplatných Azure mezi předplatiteli a CSP](../cost-management-billing/manage/transfer-subscriptions-subscribers-csp.md)
- [Přiřazení nebo přidání předplatného Azure do tenanta Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
