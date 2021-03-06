---
title: Obnovení certifikátu Azure Application Gateway
description: Naučte se obnovit certifikát přidružený k naslouchací službě Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: de57a58f7c891009d2e0cc43b351c2cad42a2766
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84807873"
---
# <a name="renew-application-gateway-certificates"></a>Prodloužit platnost Application Gatewaych certifikátů

V určitém okamžiku budete muset prodloužit platnost certifikátů, pokud jste nakonfigurovali aplikační bránu pro šifrování TLS/SSL.

Certifikát přidružený k naslouchacímu procesu můžete obnovit pomocí Azure Portal, Azure PowerShell nebo rozhraní příkazového řádku Azure:

## <a name="azure-portal"></a>portál Azure

Pokud chcete obnovit certifikát naslouchacího procesu z portálu, přejděte na naslouchací procesy služby Application Gateway. Klikněte na naslouchací proces s certifikátem, který je třeba obnovit, a potom klikněte na tlačítko **obnovit nebo upravit vybraný certifikát**.

![Prodloužit platnost certifikátu](media/renew-certificate/ssl-cert.png)

Nahrajte nový certifikát PFX, zadejte jeho název, zadejte heslo a klikněte na **Uložit**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

K obnovení certifikátu pomocí Azure PowerShell použijte následující skript:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Další kroky

Informace o tom, jak nakonfigurovat snižování zátěže TLS pomocí Azure Application Gateway, najdete v tématu [Konfigurace snižování zátěže TLS](application-gateway-ssl-portal.md) .
