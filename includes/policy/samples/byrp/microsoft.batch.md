---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a5af8d1a48c19f0b73805c5bb431f17c289e1104
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2020
ms.locfileid: "86276072"
---
|Name<br /><sub>(Azure Portal)</sub> |Description |Vliv (s) |Verze<br /><sub>GitHubu</sub> |
|---|---|---|---|
|[Nasazení nastavení diagnostiky pro účet Batch do centra událostí](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdb51110f-0865-4a6e-b274-e2e07a5b2cd7) |Nasadí nastavení diagnostiky pro účet Batch ke streamování do místního centra událostí, pokud se vytvoří nebo aktualizuje libovolný účet Batch, který postrádá tato nastavení diagnostiky. |DeployIfNotExists, zakázáno |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Nasazení nastavení diagnostiky pro účet Batch do pracovního prostoru Log Analytics](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc84e5349-db6d-4769-805e-e14037dab9b5) |Nasadí nastavení diagnostiky pro účet Batch ke streamování do oblastního Log Analytics pracovního prostoru, když se vytvoří nebo aktualizuje libovolný účet Batch, pro který chybí tato nastavení diagnostiky. |DeployIfNotExists, zakázáno |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[V účtech Batch by měly být povolené diagnostické protokoly.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F428256e6-1fac-4f48-a757-df34c2b3336d) |Audituje povolení diagnostických protokolů. To umožňuje znovu vytvořit stopy aktivity, které se mají použít pro účely šetření. Když dojde k bezpečnostnímu incidentu nebo při ohrožení zabezpečení sítě |AuditIfNotExists, zakázáno |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json) |
|[Pravidla upozornění na metriky by měly být nakonfigurovaná na účtech Batch.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F26ee67a2-f81a-4ba8-b9ce-8550bd5ee1a7) |Audituje konfiguraci pravidel upozornění metrik na účtu Batch, aby se aktivovala požadovaná metrika. |AuditIfNotExists, zakázáno |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json) |
