---
title: zahrnout soubor
description: zahrnout soubor
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: f4cd9cad3813378fcdc3f06e8ab1c28eced4f93c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "67175532"
---
Name | Komerční adresa URL | Adresa URL pro státní správu | Description
--- | --- | --- | ---
Azure Active Directory | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Slouží k řízení přístupu a správě identit pomocí Azure Active Directory. 
Backup | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Používá se ke koordinaci a přenosu dat replikace.
Replikace | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Používá se pro koordinaci a operace správy replikací.
Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Používá se pro přístup k účtu úložiště, který ukládá replikovaná data.
Telemetrie (volitelné) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Používá se pro telemetrii.
Čas synchronizace | ``time.windows.com`` | ``time.nist.gov`` | Používá se ke kontrole časové synchronizace mezi systémovým a globálním časem ve všech nasazeních.


