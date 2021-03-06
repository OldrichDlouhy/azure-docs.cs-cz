---
title: Povolit dvojité šifrování na discích se správou REST Azure Portal
description: Pro data spravovaného disku pomocí Azure Portal zapněte dvojité šifrování v klidovém formátu.
author: roygara
ms.date: 07/23/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: b244ef36e4e1a15327c053402b28ee2272fc71f0
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2020
ms.locfileid: "87136123"
---
# <a name="azure-portal---enable-double-encryption-at-rest-on-your-managed-disks"></a>Azure Portal – na spravovaných discích povolit dvojité šifrování v klidovém prostředí

Azure Disk Storage podporuje dvojité šifrování v klidovém umístění pro spravované disky. Koncepční informace o šifrovaném šifrování v klidovém umístění a dalších typech šifrování spravovaného disku naleznete v části [dvojité šifrování v klidovém oddílu v](disk-encryption.md#double-encryption-at-rest) článku o šifrování disku.

[!INCLUDE [virtual-machines-double-encryption-at-rest-portal](../../../includes/virtual-machines-disks-double-encryption-at-rest-portal.md)]

## <a name="next-steps"></a>Další kroky

- [Azure PowerShell – povolení klíčů spravovaných zákazníkem pomocí disků spravovaných šifrováním na straně serveru](disks-enable-customer-managed-keys-powershell.md)
- [Ukázky šablon Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)