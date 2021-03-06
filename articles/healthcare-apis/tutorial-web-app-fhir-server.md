---
title: Kurz k webové aplikaci – nastavení rozhraní Azure API pro FHIR
description: Tento kurz vás provede příkladem nasazení jednoduché webové aplikace. Tento první kurz popisuje požadavky a nasazení rozhraní API Azure pro FHIR.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: mihansen
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: fcad9356f87e76b14e0ef636389c11fd4290e12a
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/07/2020
ms.locfileid: "84872663"
---
# <a name="deploy-javascript-app-to-read-data-from-fhir-service"></a>Nasazení aplikace JavaScriptu pro čtení dat ze služby FHIR
V tomto kurzu nasadíte malou JavaScriptovou aplikaci, která čte data ze služby FHIR. Postup v tomto kurzu:
1. Nasazení serveru FHIR
1. Registrace veřejné klientské aplikace
1. Otestování přístupu k aplikaci
1. Vytvoření webové aplikace, která přečte tato FHIR data

## <a name="prerequisites"></a>Požadavky
Před zahájením této sady kurzů budete potřebovat následující položky:
1. Předplatné Azure
1. Tenanta Azure Active Directory.
1. Instalace [po](https://www.getpostman.com/) instalaci

> [!NOTE]
> Pro tento kurz jsou všichni uživatelé služby FHIR, aplikace Azure AD a Azure AD ve stejném tenantovi Azure AD. Pokud tomu tak není, můžete i nadále postupovat spolu s tímto kurzem, ale možná budete muset podrobně do některých odkazovaných dokumentů a provést další kroky.

## <a name="deploy-azure-api-for-fhir"></a>Nasazení Azure API for FHIR
Prvním krokem v tomto kurzu je správně nainstalovat rozhraní Azure API pro FHIR.

1. Nasazení [rozhraní API Azure pro FHIR](fhir-paas-portal-quickstart.md)
1. Jakmile budete mít rozhraní Azure API pro FHIR nasazené, nakonfigurujte nastavení [CORS](configure-cross-origin-resource-sharing.md) tak, že na Azure API pro FHIR kliknete a vyberete CORS. 
    1. Nastavit **počátek** na *
    1. Nastavit **záhlaví** na *
    1. V části **metody**zvolte **Vybrat vše** .
    1. Nastavte **maximální stáří** na **600** .

## <a name="next-steps"></a>Další kroky
Teď, když máte nasazené rozhraní API Azure pro FHIR, jste připraveni zaregistrovat veřejnou klientskou aplikaci.

>[!div class="nextstepaction"]
>[Registrace veřejné klientské aplikace](tutorial-web-app-public-app-reg.md)
