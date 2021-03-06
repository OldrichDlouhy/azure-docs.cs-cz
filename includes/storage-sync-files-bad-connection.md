---
title: zahrnout soubor
description: zahrnout soubor
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b2ff542d2782293e89b66e5d25cb67a9bcde6da8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75772886"
---
K této chybě může dojít v případě, že server nemá přístup ke službě Synchronizace souborů Azure. Při řešení této chyby můžete použít následující postup:

1. Ověřte, že `FileSyncSvc.exe` Brána firewall neblokuje službu systému Windows.
2. Ověřte, že je port 443 otevřený pro odchozí připojení ke službě Azure File Sync. Můžete to provést pomocí `Test-NetConnection` rutiny. Adresu URL místo zástupného textu `<azure-file-sync-endpoint>` najdete v dokumentu [Nastavení proxy a brány firewall Synchronizace souborů Azure](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall). 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Ujistěte se, že je konfigurace proxy nastavená očekávaným způsobem. Můžete to provést pomocí rutiny `Get-StorageSyncProxyConfiguration`. Další informace o konfiguraci proxy pro Synchronizaci souborů Azure najdete v tématu [Nastavení proxy a brány firewall Synchronizace souborů Azure](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
4. Pomocí rutiny Test-StorageSyncNetworkConnectivity Zkontrolujte síťové připojení k koncovým bodům služby. Další informace najdete v tématu [Test připojení k síti koncových bodů služby](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#test-network-connectivity-to-service-endpoints).    

5. Pokud potřebujete další pomoc při řešení potíží s připojením k síti, obraťte se na správce sítě.
