---
title: Porozumění přístupu k virtuálnímu počítači za běhu v Azure Security Center
description: Tento dokument vysvětluje, jak přístup k virtuálnímu počítači za běhu v Azure Security Center pomáhá řídit přístup k virtuálním počítačům Azure.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 07/12/2020
ms.author: memildin
ms.openlocfilehash: 50398632f47d889ecb79b32faef94c9c5923789c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531398"
---
# <a name="understanding-just-in-time-jit-vm-access"></a>Porozumění přístupu k virtuálnímu počítači JIT (just-in-time)

Tato stránka vysvětluje principy za Azure Security Center funkcí přístupu k VIRTUÁLNÍm počítačům JIT (just-in-time) a logiky na základě doporučení.

Informace o tom, jak u virtuálních počítačů použít JIT pomocí Azure Portal (buď Security Center nebo Azure Virtual Machines) nebo programově, najdete v tématu [zabezpečení portů pro správu pomocí JIT](security-center-just-in-time.md).


## <a name="the-risk-of-open-management-ports-on-a-virtual-machine"></a>Riziko otevřených portů pro správu na virtuálním počítači

Aktéri hrozeb aktivně využívají přístupné počítače s otevřenými porty pro správu, jako je RDP nebo SSH. Všechny vaše virtuální počítače jsou potenciálními cíli pro útok. Když je virtuální počítač úspěšně napadený, používá se jako vstupní bod k útoku dalších prostředků ve vašem prostředí.



## <a name="why-jit-vm-access-is-the-solution"></a>Proč je řešením přístup k virtuálnímu počítači JIT 

Stejně jako u všech technik prevence kyberbezpečnosti by měl váš cíl snížit plochu pro útok. V takovém případě to znamená, že má méně otevřených portů, hlavně porty pro správu.

Vaši legitimní uživatelé používají i tyto porty, takže je nepraktické, aby byly uzavřeny.

Chcete-li tento dilematem vyřešit, Azure Security Center nabízí JIT. Pomocí JIT můžete na virtuální počítače uzamknout příchozí provoz a omezit tak vystavení útokům, a to v případě potřeby snadného přístupu k virtuálním počítačům.



## <a name="how-jit-operates-with-network-security-groups-and-azure-firewall"></a>Jak pracuje JIT se skupinami zabezpečení sítě a Azure Firewall

Když povolíte přístup k virtuálnímu počítači za běhu, můžete vybrat porty na virtuálním počítači, do kterého se bude blokovat příchozí provoz. Security Center zajistí, že pro vybrané porty ve [skupině zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) (NSG) a [Azure firewall pravidla](https://docs.microsoft.com/azure/firewall/rule-processing)existují pravidla odepřít veškerý příchozí provoz. Tato pravidla omezují přístup k portům pro správu virtuálních počítačů Azure a brání jejich útokům. 

Pokud pro vybrané porty už existují další pravidla, budou mít tato stávající pravidla přednost před novými pravidly odepřít všechna příchozí provoz. Pokud na vybraných portech neexistují žádná pravidla, nová pravidla budou mít v NSG a Azure Firewall nejvyšší prioritu.

Když si uživatel požádá o přístup k virtuálnímu počítači, Security Center zkontroluje, jestli má uživatel pro tento virtuální počítač oprávnění [Access Control na základě role (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) . Pokud je žádost schválená, Security Center nakonfiguruje skupin zabezpečení sítě a Azure Firewall, aby povolovaly příchozí provoz na vybrané porty z příslušné IP adresy (nebo rozsahu) po určenou dobu. Po vypršení časového limitu Security Center obnoví skupin zabezpečení sítě do jejich předchozích stavů. Připojení, která jsou již navázána, nejsou přerušena.

> [!NOTE]
> JIT nepodporuje virtuální počítače chráněné pomocí bran Azure firewall řízené nástrojem [Azure firewall Manager](https://docs.microsoft.com/azure/firewall-manager/overview).




## <a name="how-security-center-identifies-which-vms-should-have-jit-applied"></a>Jak Security Center identifikuje, které virtuální počítače by měly být použité kompilátorem JIT

Následující diagram ukazuje logiku, kterou Security Center použít při rozhodování, jak kategorizovat podporované virtuální počítače: 

[![Logický tok virtuálních počítačů (VM) za běhu (just-in-time)](media/just-in-time-explained/jit-logic-flow.png)](media/just-in-time-explained/jit-logic-flow.png#lightbox)

Když Security Center najde počítač, který může využívat JIT, přidá tento počítač na kartu **nesprávné prostředky** pro doporučení. 

![Doporučení pro přístup k virtuálnímu počítači JIT (just-in-time)](./media/just-in-time-explained/unhealthy-resources.png)


## <a name="faq---questions-about-just-in-time-virtual-machine-access"></a>Nejčastější dotazy týkající se přístupu k virtuálnímu počítači za běhu

### <a name="what-permissions-are-needed-to-configure-and-use-jit"></a>Jaká oprávnění jsou nutná ke konfiguraci a používání JIT?

Pokud chcete vytvořit vlastní role, které mohou pracovat s JIT, budete potřebovat následující podrobnosti:

| Umožnění uživateli: | Oprávnění k nastavení|
| --- | --- |
| Konfigurace nebo úprava zásad JIT pro virtuální počítač | *Přiřaďte k roli tyto akce:*  <ul><li>V oboru předplatného nebo skupiny prostředků, která je přidružená k virtuálnímu počítači:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> V oboru předplatného nebo skupiny prostředků virtuálního počítače: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Vyžádat přístup JIT k virtuálnímu počítači | *Přiřaďte uživatele k těmto akcím:*  <ul><li>V oboru předplatného nebo skupiny prostředků, která je přidružená k virtuálnímu počítači:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>V oboru předplatného nebo skupiny prostředků, která je přidružená k virtuálnímu počítači:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  V oboru předplatného nebo skupiny prostředků nebo virtuálního počítače:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  V oboru předplatného nebo skupiny prostředků nebo virtuálního počítače:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|
|Čtení zásad JIT| *Přiřaďte uživatele k těmto akcím:*  <ul><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/read`</li><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action`</li><li>`Microsoft.Security/policies/read`</li><li>`Microsoft.Compute/virtualMachines/read`</li><li>`Microsoft.Network/*/read`</li>|





## <a name="next-steps"></a>Další kroky

Tato stránka vysvětluje, _Proč_ by se měla použít přístup k virtuálnímu počítači JIT (just-in-time). 

Přejděte k článku s postupem, kde se dozvíte, jak povolit JIT a žádat o přístup k virtuálním počítačům s podporou JIT:

> [!div class="nextstepaction"]
> [Jak zabezpečit porty pro správu pomocí JIT](security-center-just-in-time.md)
