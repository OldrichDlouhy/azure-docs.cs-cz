---
title: Konfigurace koncových bodů sítě služby Azure Files | Microsoft Docs
description: Přehled možností sítě pro soubory Azure.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 3/19/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 0859b034cf0caa60039fbf9eb4dd9be54448a940
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510327"
---
# <a name="configuring-azure-files-network-endpoints"></a>Konfigurace koncových bodů sítě služby soubory Azure
Soubory Azure poskytují dva hlavní typy koncových bodů pro přístup ke sdíleným složkám Azure: 
- Veřejné koncové body, které mají veřejnou IP adresu a jsou přístupné odkudkoli na světě.
- Privátní koncové body, které existují v rámci virtuální sítě a které mají privátní IP adresu v adresním prostoru virtuální sítě.

V účtu služby Azure Storage existují veřejné a privátní koncové body. Účet úložiště je konstrukce správy, která představuje sdílený fond úložiště, ve kterém můžete nasadit více sdílených složek a další prostředky úložiště, jako jsou kontejnery nebo fronty objektů BLOB.

Tento článek se zaměřuje na konfiguraci koncových bodů účtu úložiště pro přímý přístup ke sdílené složce Azure. Většina podrobností uvedených v tomto dokumentu se vztahuje také na to, jak Azure File Sync spolupracují s veřejnými a soukromými koncovými body pro účet úložiště. Další informace o požadavcích na síť pro Azure File Sync nasazení najdete v tématu [konfigurace Azure File Sync proxy serveru a nastavení brány firewall](storage-sync-files-firewall-and-proxy.md).

Před načtením tohoto průvodce doporučujeme přečíst si [informace o sítích Azure Files](storage-files-networking-overview.md) .

## <a name="prerequisites"></a>Požadavky
- V tomto článku se předpokládá, že jste už vytvořili předplatné Azure. Pokud ještě nemáte předplatné, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.
- V tomto článku se předpokládá, že už máte vytvořenou sdílenou složku Azure v účtu úložiště, ke kterému se chcete připojit z místního prostředí. Informace o tom, jak vytvořit sdílenou složku Azure, najdete v tématu [Vytvoření sdílené složky Azure](storage-how-to-create-file-share.md).
- Pokud máte v úmyslu použít Azure PowerShell, [nainstalujte nejnovější verzi](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Pokud máte v úmyslu používat rozhraní příkazového řádku Azure, [nainstalujte nejnovější verzi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-private-endpoint"></a>Vytvoření privátního koncového bodu
Vytvořením privátního koncového bodu pro účet úložiště dojde k nasazení těchto prostředků Azure:

- **Privátní koncový bod**: prostředek Azure, který představuje privátní koncový bod účtu úložiště. Můžete si to představit jako prostředek, který připojuje účet úložiště a síťové rozhraní.
- **Síťové rozhraní (nic)**: síťové rozhraní, které udržuje privátní IP adresu v zadané virtuální síti nebo podsíti. Jedná se o stejný prostředek, který se nasadí při nasazení virtuálního počítače, ale místo aby se přiřadil k VIRTUÁLNÍmu počítači, je vlastníkem privátního koncového bodu.
- **Privátní zóna DNS**: Pokud jste ještě nikdy nasadili privátní koncový bod pro tuto virtuální síť, bude pro vaši virtuální síť nasazená nová privátní zóna DNS. Pro účet úložiště v této zóně DNS se vytvoří i záznam DNS. Pokud jste již v této virtuální síti nasadili privátní koncový bod, bude do existující zóny DNS přidán záznam nového záznamu pro účet úložiště. Nasazení zóny DNS je volitelné, ale důrazně se doporučuje a vyžaduje se, pokud připojujete sdílené složky Azure k instančnímu objektu služby AD nebo pomocí rozhraní REST API.

> [!Note]  
> Tento článek používá příponu DNS účtu úložiště pro veřejné oblasti Azure, `core.windows.net` . Tento komentář platí také pro cloudy Azure, jako je Cloud pro státní správu USA a Azure Čína, stačí nahradit příslušné přípony vašeho prostředí. 

# <a name="portal"></a>[Azure Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-private-portal](../../../includes/storage-files-networking-endpoints-private-portal.md)]

Pokud máte virtuální počítač ve vaší virtuální síti nebo jste nakonfigurovali předávání DNS, jak je popsáno v tématu [konfigurace předávání DNS pro soubory Azure](storage-files-networking-dns.md), můžete otestovat správné nastavení privátního koncového bodu spuštěním následujících příkazů z PowerShellu, příkazového řádku nebo terminálu (funguje pro Windows, Linux nebo MacOS). Musíte nahradit `<storage-account-name>` odpovídajícím názvem účtu úložiště:

```
nslookup <storage-account-name>.file.core.windows.net
```

Pokud vše úspěšně fungovalo, měl by se zobrazit následující výstup, kde `192.168.0.5` je privátní IP adresa privátního koncového bodu ve vaší virtuální síti (výstup zobrazený pro Windows):

```Output
Server:  UnKnown
Address:  10.2.4.4

Non-authoritative answer:
Name:    storageaccount.privatelink.file.core.windows.net
Address:  192.168.0.5
Aliases:  storageaccount.file.core.windows.net
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-private-powershell](../../../includes/storage-files-networking-endpoints-private-powershell.md)]

Pokud máte virtuální počítač ve vaší virtuální síti nebo jste nakonfigurovali předávání DNS, jak je popsáno v tématu [konfigurace předávání DNS pro soubory Azure](storage-files-networking-dns.md), můžete otestovat správné nastavení privátního koncového bodu s následujícími příkazy:

```PowerShell
$storageAccountHostName = [System.Uri]::new($storageAccount.PrimaryEndpoints.file) | `
    Select-Object -ExpandProperty Host

Resolve-DnsName -Name $storageAccountHostName
```

Pokud vše úspěšně fungovalo, měl by se zobrazit následující výstup, kde `192.168.0.5` je privátní IP adresa privátního koncového bodu ve vaší virtuální síti:

```Output
Name                             Type   TTL   Section    NameHost
----                             ----   ---   -------    --------
storageaccount.file.core.windows CNAME  60    Answer     storageaccount.privatelink.file.core.windows.net
.net

Name       : storageaccount.privatelink.file.core.windows.net
QueryType  : A
TTL        : 600
Section    : Answer
IP4Address : 192.168.0.5
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-private-cli](../../../includes/storage-files-networking-endpoints-private-cli.md)]

Pokud máte virtuální počítač ve vaší virtuální síti nebo jste nakonfigurovali předávání DNS, jak je popsáno v tématu [konfigurace předávání DNS pro soubory Azure](storage-files-networking-dns.md), můžete otestovat správné nastavení privátního koncového bodu s následujícími příkazy:

```bash
httpEndpoint=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | \
    tr -d '"')

hostName=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint) | tr -d "/")
nslookup $hostName
```

Pokud vše úspěšně fungovalo, měl by se zobrazit následující výstup, kde `192.168.0.5` je privátní IP adresa privátního koncového bodu ve vaší virtuální síti. Všimněte si, že k připojení sdílené složky místo cesty byste měli dál používat storageaccount.file.core.windows.net `privatelink` .

```Output
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
storageaccount.file.core.windows.net      canonical name = storageaccount.privatelink.file.core.windows.net.
Name:   storageaccount.privatelink.file.core.windows.net
Address: 192.168.0.5
```

---

## <a name="restrict-access-to-the-public-endpoint"></a>Omezení přístupu k veřejnému koncovému bodu
Přístup k veřejnému koncovému bodu můžete omezit pomocí nastavení brány firewall účtu úložiště. Obecně platí, že většina zásad brány firewall pro účet úložiště omezí přístup k síti na jednu nebo více virtuálních sítí. Existují dva způsoby, jak omezit přístup k účtu úložiště na virtuální síť:

- [Vytvořte jeden nebo několik privátních koncových bodů pro účet úložiště](#create-a-private-endpoint) a omezte veškerý přístup k veřejnému koncovému bodu. Tím se zajistí, že budou mít přístup ke sdíleným složkám Azure v rámci účtu úložiště jenom přenosy pocházející z požadovaných virtuálních sítí.
- Omezte veřejný koncový bod na jednu nebo více virtuálních sítí. To funguje pomocí funkce virtuální sítě s názvem *koncové body služby*. Když omezíte provoz na účet úložiště prostřednictvím koncového bodu služby, stále přistupujete k účtu úložiště prostřednictvím veřejné IP adresy.

### <a name="disable-access-to-the-public-endpoint"></a>Zakázat přístup k veřejnému koncovému bodu
Když je zakázaný přístup k veřejnému koncovému bodu, je možné, že k účtu úložiště budete mít přístup prostřednictvím svých privátních koncových bodů. Jinak budou požadavky na veřejný koncový bod účtu úložiště odmítnuty. 

# <a name="portal"></a>[Azure Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-public-disable-portal](../../../includes/storage-files-networking-endpoints-public-disable-portal.md)]

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-public-disable-powershell](../../../includes/storage-files-networking-endpoints-public-disable-powershell.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-public-disable-cli](../../../includes/storage-files-networking-endpoints-public-disable-cli.md)]

---

### <a name="restrict-access-to-the-public-endpoint-to-specific-virtual-networks"></a>Omezení přístupu k veřejnému koncovému bodu na konkrétní virtuální sítě
Když omezíte účet úložiště na konkrétní virtuální sítě, povolujete požadavky na veřejný koncový bod v rámci zadaných virtuálních sítí. To funguje pomocí funkce virtuální sítě s názvem *koncové body služby*. Tato možnost se dá použít s privátními koncovými body nebo bez nich.

# <a name="portal"></a>[Azure Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-portal](../../../includes/storage-files-networking-endpoints-public-restrict-portal.md)]

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-powershell](../../../includes/storage-files-networking-endpoints-public-restrict-powershell.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-cli](../../../includes/storage-files-networking-endpoints-public-restrict-cli.md)]

---

## <a name="see-also"></a>Viz také:
- [Požadavky na síť pro Azure Files](storage-files-networking-overview.md)
- [Konfigurace předávání DNS pro soubory Azure](storage-files-networking-dns.md)
- [Konfigurace S2S VPN pro soubory Azure](storage-files-configure-s2s-vpn.md)