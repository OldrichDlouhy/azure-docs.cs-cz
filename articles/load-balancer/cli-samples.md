---
title: Ukázky v Azure CLI pro Load Balancer
titleSuffix: Azure Load Balancer
description: Ukázky Azure CLI
services: load-balancer
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 06/14/2018
ms.author: allensu
ms.openlocfilehash: 3198160fe443f4650d8c5a880c3538efeb9346a1
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86205247"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Ukázky v Azure CLI pro Load Balancer

Následující tabulka obsahuje odkazy na skripty Bash vytvořené pomocí Azure CLI.

* [Vyrovnávání zatížení provozu do virtuálních počítačů pro zajištění vysoké dostupnosti](./scripts/load-balancer-linux-cli-sample-nlb.md)

  Vytvoří několik virtuálních počítačů s vysokou dostupností a konfigurací s vyrovnáváním zatížení.

* [Vyrovnávání zatížení virtuálních počítačů napříč zónami dostupnosti](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md)

  Vytvoří tři virtuální počítače v různých zónách dostupnosti v rámci oblasti a Standard Load Balancer s IP adresou redundantní zóny. Tato konfigurace nástroje pro vyrovnávání zatížení pomáhá chránit vaše aplikace a data před nepravděpodobným selháním nebo ztrátou celého datového centra.

* [Vyrovnávání zatížení virtuálních počítačů v konkrétní zóně dostupnosti](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)

  Vytvoří tři virtuální počítače, Standard Load Balancer s oblastí front-endu IP, která pomáhá zarovnat cestu k datům a prostředky v jedné zóně pro danou oblast.

* [Vyrovnávání zatížení více webů na virtuálních počítačích](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md)

  Vytvoří dva virtuální počítače s několika konfiguracemi IP adres, které jsou připojené k sadě dostupnosti Azure přístupné prostřednictvím Azure Load Balancer.
