---
title: Nejčastější dotazy k Azure důvěrného zpracování
description: Odpovědi na nejčastější dotazy týkající se důvěrného výpočetního prostředí Azure
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: bb821d00a168e3b8f0636b93696376dc8b5d492e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "83772894"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Nejčastější dotazy týkající se důvěrného výpočetního prostředí Azure

Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se spouštění [důvěrných výpočetních úloh v Azure](overview.md).

Pokud váš problém s Azure není v tomto článku řešen, navštivte fóra Azure na [webu MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Svůj problém můžete vystavit na těchto fórech nebo odeslat na [ @AzureSupport Twitter](https://twitter.com/AzureSupport). Můžete také odeslat žádost o podporu Azure. Pokud chcete odeslat žádost o podporu, vyberte na [stránce podpory Azure](https://azure.microsoft.com/support/options/)možnost získat podporu.

## <a name="confidential-computing-virtual-machines"></a>Důvěrné výpočetní Virtual Machines<a id="vm-faq"></a>

**Jak můžu nasazovat virtuální počítače řady DCsv2 v Azure?**

Tady je několik způsobů, jak můžete nasadit virtuální počítač s DCsv2:
   - Použití [šablony Azure Resource Manager](../virtual-machines/windows/template-description.md)
   - Z [Azure Portal](https://portal.azure.com/#create/hub)
   - V šabloně řešení [Azure důvěrné výpočetní prostředí (Virtual Machine)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview) Marketplace. Šablona řešení Marketplace vám pomůže omezit zákazníky na podporované scénáře (oblasti, obrázky, dostupnost, šifrování disků). 

**Budou všechny image operačních systémů fungovat s důvěrným výpočetním prostředím Azure?**

Ne. Virtuální počítače se dají nasadit jenom na operační počítače generace 2 s Ubuntu serverem 18,04, Ubuntu serverem 16,04, Windows Server 2019 Datacenter a Windows Server 2016 Datacenter. Další informace o virtuálních počítačích 2. generace v systémech [Linux](../virtual-machines/linux/generation-2.md) a [Windows](../virtual-machines/windows/generation-2.md)

**Virtuální počítače s DCsv2 jsou na portálu zobrazeny šedě a nelze je vybrat.**

Na základě informačního bublinového rámečku u virtuálního počítače je potřeba vzít v úvahu různé akce:
   -    **UnsupportedGeneration**: Změňte generaci image virtuálního počítače na "Gen2".
   -    **NotAvailableForSubscription**: oblast zatím není pro vaše předplatné k dispozici. Vyberte dostupnou oblast.
   -    **InsufficientQuota**: [vytvořte žádost o podporu, která zvýší vaši kvótu](../azure-portal/supportability/per-vm-quota-requests.md). Bezplatné zkušební předplatné nemají kvótu pro virtuální počítače s důvěrnými výpočetními prostředími. 

**Virtuální počítače s DCsv2 se při pokusu o jejich hledání v selektoru velikosti portálu nezobrazí.**

Ujistěte se, že jste vybrali oblast, která je [k dispozici](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines). Také se ujistěte, že v selektoru velikosti vyberete možnost zrušit všechny filtry. 

**Zobrazila se chyba při selhání nasazení šablony Azure Resource Manager: operace nemohla být dokončena, protože má za následek překročení schválené kvóty jader řady Standard DcsV2.**

[Vytvořte žádost o podporu, která zvýší vaši kvótu](../azure-portal/supportability/per-vm-quota-requests.md). Bezplatné zkušební předplatné nemají kvótu pro virtuální počítače s důvěrnými výpočetními prostředími. 

**Jaký je rozdíl mezi virtuálními počítači řady DCsv2-Series a DC-Series?**

Virtuální počítače DC-Series běží na starších 6 procesorech Intel s Intel SGX a mají méně celkové paměti, méně enklávy paměti mezipaměti (EPC) a jsou dostupné jenom v dvou oblastech (USA – východ a Evropa – západ v Standard_DC2s a Standard_DC4s velikosti). Nejsou k dispozici žádné plány pro zajištění všeobecně dostupných virtuálních počítačů a nedoporučují se pro produkční použití. K nasazení těchto virtuálních počítačů použijte instanci webu Marketplace [COMPUTE COMPUTE DC-Series VM [Preview]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview) .

**Jsou virtuální počítače DCsv2 k dispozici globálně?**

Ne. Tyto virtuální počítače jsou v tuto chvíli dostupné jenom ve vybraných oblastech. Poslední dostupné oblasti najdete na [stránce produkty podle oblastí](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) . 

**Návody na virtuální počítače s DCsv2 nainstalovat sadu Open enklávy SDK?**
   
Pokyny k instalaci sady OE SDK na místní počítač Azure nebo v místním počítači najdete v pokynech k [Open ENKLÁVY SDK GitHubu](https://github.com/openenclave/openenclave).
     
Můžete se také podívat na otevřený enklávy SDK GitHub pro pokyny k instalaci specifické pro konkrétní operační systém:
   - [Instalace rozhraní OE SDK ve Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
   - [Instalace sady OE SDK na Ubuntu 18,04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)
   - [Instalace sady OE SDK na Ubuntu 16,04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_16.04.md)
