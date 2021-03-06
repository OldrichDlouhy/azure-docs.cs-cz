---
title: Azure Resource Manager žádosti o zvýšení kvóty vCPU
description: Azure Resource Manager žádosti o zvýšení kvóty vCPU
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7fd04428edc28315e714b9e81af0bea0e9344e52
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763801"
---
# <a name="quota-increase-requests"></a>Žádosti o navýšení kvóty

Správce prostředků kvóty vCPU pro virtuální počítače a sady škálování virtuálních počítačů se vynutily na dvou úrovních každého předplatného v každé oblasti.

První úroveň je celkový regionální limit vCPU napříč všemi řadou virtuálních počítačů. Druhá úroveň je vCPU limit pro řady virtuálních počítačů, jako je například vCPU řady D-Series. Kdykoli je nasazen nový virtuální počítač, součet nových a stávajících Vcpuch využití pro tuto řadu virtuálních počítačů nesmí překročit kvótu vCPU schválenou pro danou řadu virtuálních počítačů. Celkový počet nových a existujících vCPUů nasazených ve všech řadách virtuálních počítačů by neměl přesáhnout celkovou kvótu vCPU schválenou pro předplatné. Pokud dojde k překročení některé z těchto kvót, nasazení virtuálního počítače se nepovolí.
Můžete požádat o zvýšení limitu kvóty vCPU pro řady virtuálních počítačů z Azure Portal. Zvýšení kvóty řady virtuálních počítačů automaticky zvýší celkový rozsah regionálního Vcpuu o stejné množství.

Při vytvoření nového předplatného se nemusí výchozí celková oblast vCPU rovnat součtu výchozích kvót vCPU pro všechny jednotlivé řady virtuálních počítačů. Tato skutečnost může mít za následek předplatné s dostatečnou kvótou pro každou jednotlivou řadu virtuálních počítačů, kterou chcete nasadit. Pro všechny místní Vcpuy pro všechna nasazení by mohlo dojít k dostatečnému počtu kvót. V takovém případě budete muset odeslat žádost o explicitní zvýšení celkového počtu místních vCPU. Celkový počet regionálních vCPU nesmí překročit součet schválené kvóty v rámci všech řad virtuálních počítačů v dané oblasti.

Další informace o kvótách najdete v tématech o kvótách [VCPU virtuálních počítačů](../../virtual-machines/windows/quotas.md) a [omezeních, kvótách a omezeních předplatného a služeb Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

