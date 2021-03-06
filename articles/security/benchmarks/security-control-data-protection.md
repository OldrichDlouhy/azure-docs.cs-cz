---
title: Řízení zabezpečení Azure – Ochrana dat
description: Ochrana dat kontroly zabezpečení Azure
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: d89320807c6322120490db85100453edf593aded
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045531"
---
# <a name="security-control-data-protection"></a>Řízení zabezpečení: Ochrana dat

Doporučení ochrany dat se zaměřují na řešení problémů souvisejících s šifrováním, seznamů řízení přístupu, řízení přístupu na základě identity a protokolování auditu pro přístup k datům.

## <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: Udržujte inventář citlivých informací

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.1 | 13,1 | Zákazník |

Pomocí značek můžete posloužit ke sledování prostředků Azure, které ukládají nebo zpracovávají citlivé informace.

- [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: izolujte systémy, které ukládají nebo zpracovávají citlivé informace.

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.2 | 13,2, 2,10 | Zákazník |

Implementujte izolaci pomocí samostatných předplatných a skupin pro správu pro jednotlivé domény zabezpečení, jako je například typ prostředí a úroveň citlivosti dat. Můžete omezit úroveň přístupu k prostředkům Azure, které vaše aplikace a podniková prostředí vyžadují. Přístup k prostředkům Azure můžete řídit prostřednictvím řízení přístupu na základě role Azure (RBAC). 

- [Vytvoření dalších předplatných Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

- [Postup vytvoření Skupiny pro správu](https://docs.microsoft.com/azure/governance/management-groups/create)

- [Vytváření a používání značek](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

## <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: Sledujte a zablokujte neoprávněný přenos citlivých informací

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.3 | 13,3 | Shared |

Využijte řešení třetích stran z Azure Marketplace na hraničních sítích, které monitorují neoprávněný přenos citlivých informací a zablokují tyto přenosy, a upozorní odborníky na zabezpečení informací.

Pro základní platformu, která je spravovaná Microsoftem, Microsoft zpracovává veškerý obsah zákazníků jako citlivý a chráněný proti ztrátám a expozici zákaznických dat. Aby se zajistilo zabezpečení zákaznických dat v Azure, společnost Microsoft implementovala a udržuje sadu robustních ovládacích prvků a možností ochrany dat.

- [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: šifrování všech citlivých informací během přenosu

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.4 | 14,4 | Shared |

Šifrování všech citlivých informací během přenosu. Ujistěte se, že klienti, kteří se připojují k prostředkům Azure, můžou vyjednávat TLS 1,2 nebo vyšší.

Pokud je to možné, postupujte podle Azure Security Center doporučení pro šifrování v klidovém režimu a šifrování.

- [Pochopení šifrování při přenosu pomocí Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

## <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: k identifikaci citlivých dat použijte aktivní nástroj zjišťování.

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.5 | 14,5 | Shared |

Pokud není k dispozici žádná funkce pro konkrétní službu v Azure, použijte k identifikaci všech citlivých informací uložených, zpracovávaných nebo odeslaných technologickými systémy organizace, které jsou umístěné na pracovišti, nebo na vzdáleném poskytovateli služeb, a aktualizujte inventář citlivých informací organizace pomocí nástroje pro aktivní zjišťování třetí strany.

K identifikaci citlivých informací v dokumentech Office 365 použijte Azure Information Protection.

Využijte Azure SQL Information Protection k usnadnění klasifikace a označování informací uložených v Azure SQL Database.

- [Implementace zjišťování dat SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

- [Postup implementace Azure Information Protection](https://docs.microsoft.com/azure/information-protection/deployment-roadmap)

- [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: k řízení přístupu k prostředkům použijte řízení přístupu na základě role

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4.6 | 14,6 | Zákazník |

Využijte Azure AD RBAC k řízení přístupu k datům a prostředkům, jinak použijte metody řízení přístupu specifické pro službu.

- [Jak nakonfigurovat RBAC v Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

## <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: použití prevence ztráty dat na základě hostitele k vymáhání řízení přístupu

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4,7 | 14,7 | Shared |

Pokud je to nutné pro dodržování předpisů u výpočetních prostředků, implementujte nástroj třetí strany, například řešení ochrany před únikem informací na základě automatizovaného hostitele, abyste vynutili řízení přístupu k datům i v případě, že se data zkopírují mimo systém.

Pro základní platformu, která je spravovaná Microsoftem, Microsoft považuje veškerý obsah zákazníka za citlivý a vede na skvělé délky, aby se zabránilo ochraně před ztrátou a únikem informací a riziky zákazníků. Aby se zajistilo zabezpečení zákaznických dat v Azure, společnost Microsoft implementovala a udržuje sadu robustních ovládacích prvků a možností ochrany dat.

- [Pochopení ochrany zákaznických dat v Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

## <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: šifrování citlivých informací v klidovém umístění

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4,8 | 14,8 | Zákazník |

Pro všechny prostředky Azure použijte šifrování v klidovém provozu. Microsoft doporučuje povolit správu šifrovacích klíčů v Azure, ale v některých případech je k dispozici možnost spravovat vlastní klíče. 

- [Vysvětlení šifrování v klidovém umístění v Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

- [Postup konfigurace šifrovacích klíčů spravovaných zákazníkem](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal)

## <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: protokolovat a upozornit na změny kritických prostředků Azure

| ID Azure | ID služby CI | Zodpovědní |
|--|--|--|
| 4,9 | 14,9 | Zákazník |

Pomocí Azure Monitor s protokolem aktivit Azure můžete vytvářet výstrahy, které se použijí v případě, že se změny projeví u kritických prostředků Azure.

- [Vytvoření upozornění pro události protokolu aktivit Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)


## <a name="next-steps"></a>Další kroky

- Zobrazit další kontrolu zabezpečení: [Správa ohrožení](security-control-vulnerability-management.md) zabezpečení