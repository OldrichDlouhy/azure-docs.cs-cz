---
title: Konfigurovat ověřování klientů u příchozích volání – Azure Event Grid IoT Edge | Microsoft Docs
description: Nakonfigurujte protokoly rozhraní API, které jsou vystavené Event Grid v IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: a0bba9559c2a0b4ff6c8a4e5f2287692e27f8a1a
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171699"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Konfigurovat ověřování klientů u příchozích volání

Tato příručka obsahuje příklady možných konfigurací ověřování klientů pro modul Event Grid. Modul Event Grid podporuje dva typy ověřování klientů:

* Založený na klíči SAS (Shared Access Signature)
* Na základě certifikátu

V tématu Průvodce [zabezpečením a ověřováním](security-authentication.md) najdete všechny možné konfigurace.

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Povolit ověřování klientů založených na certifikátech, žádné certifikáty podepsané svým držitelem

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Povolit ověřování klientů na základě certifikátů, povolit certifikáty podepsané svým držitelem

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Nastavte vlastnost **inbound__clientAuth__clientCert__allowUnknownCA** na **hodnotu true** pouze v testovacích prostředích, protože obvykle používáte certifikáty podepsané svým držitelem. Pro produkční úlohy doporučujeme tuto vlastnost nastavit na **hodnotu false** a certifikáty od certifikační autority (CA).

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Povolit ověřování klientů založených na certifikátech a SAS – klíč

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=true",
    "inbound__clientAuth__sasKeys__key1=<some-secret1-here>",
    "inbound__clientAuth__sasKeys__key2=<some-secret2-here>",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Ověřování klienta na základě klíčů SAS umožňuje, aby modul pro správu a provoz v prostředí neiot Edge probíhají operace správy a běhu za předpokladu, že porty rozhraní API jsou přístupné mimo IoT Edge síť.
