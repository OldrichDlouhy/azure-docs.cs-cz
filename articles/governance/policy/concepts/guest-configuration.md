---
title: Informace o tom, jak auditovat obsah virtuálních počítačů
description: Přečtěte si, jak Azure Policy používá agenta konfigurace hosta k auditování nastavení v rámci virtuálních počítačů.
ms.date: 05/20/2020
ms.topic: conceptual
ms.openlocfilehash: f2f07a3e88984a84ca1529052d5899ad8570a268
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072823"
---
# <a name="understand-azure-policys-guest-configuration"></a>Vysvětlení konfigurace hosta ve službě Azure Policy

Azure Policy můžou auditovat nastavení v počítači, a to pro počítače běžící v Azure i v [počítačích připojených k Arc](../../../azure-arc/servers/overview.md).
Ověřování se provádí pomocí rozšíření Konfigurace hosta a prostřednictvím klienta. Toto rozšíření prostřednictvím klienta ověřuje nastavení, jako například:

- Konfigurace operačního systému
- Konfigurace nebo přítomnost aplikací
- Nastavení prostředí

V tuto chvíli většina zásad konfigurace hosta Azure Policy jenom auditovat nastavení v rámci počítače.
Nepoužívají konfigurace. Výjimkou je jedna integrovaná zásada, na [kterou se odkazuje níže](#applying-configurations-using-guest-configuration).

## <a name="enable-guest-configuration"></a>Povolit konfiguraci hosta

Pokud chcete auditovat stav počítačů ve vašem prostředí, včetně počítačů v Azure a připojených počítačů ARC, Projděte si následující podrobnosti.

## <a name="resource-provider"></a>Poskytovatel prostředků

Než budete moct použít konfiguraci hosta, musíte zaregistrovat poskytovatele prostředků. Poskytovatel prostředků je zaregistrován automaticky, pokud je přiřazení zásady konfigurace hostů provedeno prostřednictvím portálu. Můžete provést ruční registraci prostřednictvím [portálu](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure PowerShell](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell)nebo rozhraní příkazového [řádku Azure](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli).

## <a name="deploy-requirements-for-azure-virtual-machines"></a>Nasazení požadavků pro virtuální počítače Azure

Pokud chcete auditovat nastavení v rámci počítače, je povolená [rozšíření virtuálního počítače](../../../virtual-machines/extensions/overview.md) a počítač musí mít systémově spravovanou identitu. Rozšíření stáhne příslušné přiřazení zásad a odpovídající definici konfigurace. Identita se používá k ověření počítače při jeho čtení a zápisu do služby konfigurace hosta. Pro připojené počítače ARC není rozšíření vyžadováno, protože je zahrnuto v agentovi počítače připojeného k ARC.

> [!IMPORTANT]
> K auditování virtuálních počítačů Azure se vyžaduje rozšíření konfigurace hosta a spravovaná identita. Pro provádění auditů na virtuálních počítačích Azure je potřeba > rozšíření konfigurace hosta. Pokud chcete nasadit rozšíření ve velkém měřítku, přiřaďte následující iniciativu zásad: > nasazení rozšíření nasaďte ve velkém rozsahu, přiřaďte následující definice zásad: 
>  - [Nasazení požadavků pro povolení zásad konfigurace hostů na virtuálních počítačích](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8)

### <a name="limits-set-on-the-extension"></a>Omezení nastavená pro rozšíření

Pokud chcete omezit rozšíření proti ovlivnění aplikací spuštěných v počítači, konfigurace hosta nemůže překročit více než 5% procesoru. Toto omezení existuje jak pro předdefinované, tak pro vlastní definice. Totéž platí pro službu konfigurace hosta v agentovi počítače připojeného k ARC.

### <a name="validation-tools"></a>Nástroje ověřování

V počítači používá klient konfigurace Host místní nástroje pro spuštění auditu.

V následující tabulce je uveden seznam místních nástrojů používaných na každém podporovaném operačním systému. U předdefinovaného obsahu obsluhuje konfigurace hosta automatické načítání těchto nástrojů.

|Operační systém|Nástroj pro ověření|Poznámky|
|-|-|-|
|Windows|[Konfigurace požadovaného stavu prostředí PowerShell](/powershell/scripting/dsc/overview/overview) v2| Po straně bylo načteno do složky, kterou používá Azure Policy. Nekoliduje s Windows PowerShell DSC. PowerShell Core není přidaný do systémové cesty.|
|Linux|[Nespec](https://www.chef.io/inspec/)| Nainstaluje 2.2.61 INSPEC verze ve výchozím umístění a přidá se do systémové cesty. Jsou nainstalované i závislosti pro INSPEC Package, včetně Ruby a Pythonu. |

### <a name="validation-frequency"></a>Frekvence ověřování

Klient konfigurace hosta kontroluje nový obsah každých 5 minut. Po přijetí přiřazení hostů se nastavení této konfigurace znovu zkontrolují v intervalu 15 minut. Výsledky se odešlou do poskytovatele prostředků konfigurace hosta, až se audit dokončí. Když dojde k [aktivaci vyhodnocení](../how-to/get-compliance-data.md#evaluation-triggers) zásad, stav počítače se zapíše do poskytovatele prostředků konfigurace hosta. Tato aktualizace způsobí Azure Policy vyhodnocení vlastností Azure Resource Manager. Vyhodnocení Azure Policy na vyžádání načte nejnovější hodnotu z poskytovatele prostředků konfigurace hosta. Neaktivuje ale nové auditování konfigurace v rámci počítače.

## <a name="supported-client-types"></a>Podporované typy klientů

Zásady konfigurace hosta jsou zahrnuté do nových verzí. Starší verze operačních systémů, které jsou k dispozici ve Azure Marketplace, jsou vyloučené, pokud není agent konfigurace hosta kompatibilní.
Následující tabulka obsahuje seznam podporovaných operačních systémů pro Image Azure:

|Publisher|Název|Verze|
|-|-|-|
|Canonical|Ubuntu Server|14,04 a novější|
|Credativ|Debian|8 a novější|
|Partnerský vztah Microsoftu|Windows Server|2012 a novější|
|Partnerský vztah Microsoftu|Klient Windows|Windows 10|
|OpenLogic|CentOS|7,3 a novější|
|Red Hat|Red Hat Enterprise Linux|7,4-7,8, 9,0 a novější|
|SUSE|SLES|12 SP3 a novější|

Vlastní image virtuálních počítačů jsou podporovány zásadami konfigurace hosta, pokud se jedná o jeden z operačních systémů uvedených v tabulce výše.

## <a name="guest-configuration-extension-network-requirements"></a>Síťové požadavky rozšíření konfigurace hosta

Aby počítače komunikovaly s poskytovatelem prostředků konfigurace hosta v Azure, vyžadují odchozí přístup k datacentrům Azure na portu **443**. Pokud síť v Azure nepovoluje odchozí přenosy, nakonfigurujte výjimky s pravidly [skupiny zabezpečení sítě](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) . [Označení služby](../../../virtual-network/service-tags-overview.md) "GuestAndHybridManagement" lze použít k odkazování na službu konfigurace hosta.

## <a name="managed-identity-requirements"></a>Požadavky na spravovanou identitu

Zásady v iniciativě [nasazují požadavky pro povolení zásad konfigurace hostů na virtuálních počítačích](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8) povolují spravovanou identitu přiřazenou systémem, pokud taková neexistuje. V iniciativě existují dvě definice zásad, které spravují vytváření identit. Podmínky IF v definicích zásad zajišťují správné chování na základě aktuálního stavu prostředku počítače v Azure.

Pokud počítač momentálně nemá žádné spravované identity, platí tyto zásady: [ \[ Preview \] : Přidejte spravovanou identitu přiřazenou systémem a povolte přiřazení konfigurace hostů na virtuálních počítačích bez identit](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3cf2ab00-13f1-4d0c-8971-2ac904541a7e) .

Pokud má počítač nyní uživatelsky přiřazenou identitu systému, platí následující: ve [ \[ verzi Preview \] : Přidání spravované identity přiřazené systémem pro povolení přiřazení konfigurace hostů na virtuálních počítačích s identitou přiřazenou uživatelem](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F497dff13-db2a-4c0f-8603-28fa3b331ab6) .

## <a name="guest-configuration-definition-requirements"></a>Požadavky na definici konfigurace hosta

Každý audit spouštěný pomocí konfigurace hosta vyžaduje dvě definice zásad, definici **DeployIfNotExists** a definici **AuditIfNotExists** . Definice zásad **DeployIfNotExists** spravují závislosti pro provádění auditů na každém počítači.

Definice zásad **DeployIfNotExists** ověří a opraví následující položky:

- Ověřte, že počítač má přiřazenou konfiguraci k vyhodnocení. Pokud aktuálně není k dispozici žádné přiřazení, načtěte přiřazení a připravte počítač podle:
  - Ověřování na počítači pomocí [spravované identity](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Instalace nejnovější verze rozšíření **Microsoft. GuestConfiguration**
  - Instalace [ověřovacích nástrojů](#validation-tools) a závislostí, pokud je to potřeba

Pokud přiřazení **DeployIfNotExists** nedodržuje předpisy, lze použít [úlohu nápravy](../how-to/remediate-resources.md#create-a-remediation-task) .

Jakmile je přiřazení **DeployIfNotExists** kompatibilní, přiřazení zásady **AuditIfNotExists** určí, jestli je přiřazení hostů kompatibilní nebo nekompatibilní. Nástroj pro ověření poskytuje výsledky pro klienta konfigurace hosta. Klient předává výsledky do rozšíření hosta, které je zpřístupní prostřednictvím poskytovatele prostředků konfigurace hosta.

Azure Policy používá k hlášení dodržování předpisů v uzlu **dodržování** předpisů vlastnost poskytovatelů prostředků konfigurace hosta ( **complianceStatus** ). Další informace najdete v tématu [získání dat o dodržování předpisů](../how-to/get-compliance-data.md).

> [!NOTE]
> Zásady **DeployIfNotExists** se vyžadují, aby zásady **AuditIfNotExists** vracely výsledky. Bez **DeployIfNotExists**se v zásadách **AuditIfNotExists** zobrazuje "0 z 0" prostředků jako stav.

V iniciativě jsou zahrnuty všechny předdefinované zásady pro konfiguraci hosta, aby bylo možné seskupit definice pro použití v přiřazeních. Integrovaná iniciativa s názvem _ \[ Preview \] : audit zabezpečení hesel uvnitř počítačů se systémy Linux a Windows_ obsahuje 18 zásad. Pro systém Linux existuje šest párů **DeployIfNotExists** a **AuditIfNotExists** pro Windows a tři páry. Logika [definice zásad](definition-structure.md#policy-rule) ověřuje, zda je vyhodnocen pouze cílový operační systém.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Auditování nastavení operačního systému po oborových plánech

Jedna z iniciativ v Azure Policy poskytuje možnost auditu nastavení operačního systému podle směrného plánu. Definice, _ \[ verze Preview \] : Auditovat virtuální počítače s Windows, které neodpovídají nastavení základní hodnoty zabezpečení Azure,_ zahrnuje sadu pravidel založených na zásady skupiny Active Directory.

Většina nastavení je k dispozici jako parametry. Parametry umožňují přizpůsobit, co je auditováno.
Zarovnejte zásady s vašimi požadavky nebo namapujte zásady na informace třetích stran, jako jsou například oborový zákon o standardech.

Některé parametry podporují rozsah celočíselných hodnot. Například nastavení maximálního stáří hesla může auditovat platné nastavení Zásady skupiny. Rozsah "1; 70" by potvrdil, že uživatelé musí měnit hesla alespoň každých 70 dní, ale ne méně než jeden den.

Pokud zásadu přiřadíte pomocí šablony Azure Resource Manager (šablona ARM), použijte k řízení výjimek soubor parametrů. Soubory vraťte se změnami do systému správy verzí, jako je třeba Git. Komentáře k změnám souborů poskytují důkaz o tom, proč je přiřazení výjimkou očekávané hodnoty.

#### <a name="applying-configurations-using-guest-configuration"></a>Použití konfigurace pomocí konfigurace hosta

Nejnovější funkce Azure Policy konfiguruje nastavení v počítačích. Definice _nastaví časové pásmo na počítačích s Windows_ a provede změny v počítači konfigurací časového pásma.

Při přiřazování definic, které začínají na _Konfigurovat_, musíte také přiřadit _předpoklady nasazení definice a povolit zásadu konfigurace hosta na virtuálních počítačích s Windows_. V případě, že se rozhodnete, můžete tyto definice kombinovat v iniciativě.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Přiřazování zásad do počítačů mimo Azure

Zásady auditu, které jsou k dispozici pro konfiguraci hosta, zahrnují typ prostředku **Microsoft. HybridCompute/počítače** . Všechny počítače připojené ke [službě Azure ARC pro servery](../../../azure-arc/servers/overview.md) , které jsou v oboru přiřazení zásad, jsou automaticky zahrnuté.

### <a name="multiple-assignments"></a>Více přiřazení

Zásady konfigurace hosta momentálně podporují přiřazování stejného přiřazení hostů jenom jednou pro každý počítač, a to i v případě, že přiřazení zásady používá jiné parametry.

## <a name="client-log-files"></a>Soubory protokolů klienta

Rozšíření konfigurace hosta zapisuje soubory protokolu do následujících umístění:

Systému`C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux`/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

Kde `<version>` odkazuje na aktuální číslo verze.

### <a name="collecting-logs-remotely"></a>Vzdálené shromažďování protokolů

Prvním krokem při řešení potíží s konfiguracemi konfigurace hosta nebo moduly by se měly používat `Test-GuestConfigurationPackage` rutiny podle kroků, jak [vytvořit vlastní zásady auditu konfigurace hostů pro Windows](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows).
Pokud to neproběhne úspěšně, může shromažďování protokolů klienta pomáhat s diagnostikou problémů.

#### <a name="windows"></a>Windows

Pomocí [příkazu spustit pro virtuální počítače Azure](../../../virtual-machines/windows/run-command.md)Zachyťte informace ze souborů protokolů, což může být užitečné v následujícím ukázkovém skriptu PowerShellu.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Pomocí [příkazu spustit pro virtuální počítače Azure](../../../virtual-machines/linux/run-command.md)Zachyťte informace ze souborů protokolů, což může být užitečné v následujícím ukázkovém skriptu bash.

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Ukázky konfigurace hosta

Ukázky integrovaných zásad konfigurace hosta jsou k dispozici v následujících umístěních:

- [Předdefinované definice zásad – konfigurace hostů](../samples/built-in-policies.md#guest-configuration)
- [Integrované iniciativy – konfigurace hostů](../samples/built-in-initiatives.md#guest-configuration)
- [Azure Policy Samples – úložiště GitHub](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Další kroky

- Podívejte se, jak zobrazit podrobnosti jednotlivých nastavení ze [zobrazení dodržování předpisů konfigurace hostů.](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration)
- Přečtěte si příklady na [Azure Policy Samples](../samples/index.md).
- Projděte si [strukturu definic Azure Policy](definition-structure.md).
- Projděte si [Vysvětlení efektů zásad](effects.md).
- Zjistěte, jak [programově vytvářet zásady](../how-to/programmatically-create.md).
- Přečtěte si, jak [získat data o dodržování předpisů](../how-to/get-compliance-data.md).
- Přečtěte si, jak [opravit prostředky, které nedodržují předpisy](../how-to/remediate-resources.md).
- Seznamte se s tím, co skupina pro správu [organizuje vaše prostředky pomocí skupin pro správu Azure](../../management-groups/overview.md).
