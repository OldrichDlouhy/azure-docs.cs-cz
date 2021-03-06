---
title: Připojení k Azure Media Services V3 API – Java
description: Tento článek popisuje, jak se připojit k rozhraní Azure Media Services V3 API pomocí Java.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2019
ms.custom: devx-track-java
ms.author: juliako
ms.openlocfilehash: 098e1db7470124dc7c15b3ee65d6ab9cb3fadabd
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319826"
---
# <a name="connect-to-media-services-v3-api---java"></a>Připojení k Media Services V3 API – Java

V tomto článku se dozvíte, jak se připojit k sadě Azure Media Services V3 Java SDK pomocí metody přihlašování instančního objektu.

V tomto článku se k vývoji ukázkové aplikace používá Visual Studio Code.

## <a name="prerequisites"></a>Požadavky

- Po [napsání Java pomocí Visual Studio Code](https://code.visualstudio.com/docs/java/java-tutorial) nainstalujte:

   - JDK
   - Apache Maven
   - Balíček rozšíření Java
- Ujistěte se, že jste nastavili `JAVA_HOME` a `PATH` proměnné prostředí.
- [Vytvořte účet Media Services](./create-account-howto.md). Nezapomeňte si pamatovat název skupiny prostředků a název účtu Media Services.
- Postupujte podle kroků v tématu [rozhraní API pro přístup](./access-api-howto.md) . Poznamenejte si ID předplatného, ID aplikace (ID klienta), ověřovací klíč (tajný kód) a ID tenanta, které budete potřebovat v pozdějším kroku.

Také si přečtěte:

- [Java v Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Správa projektů v jazyce Java v VS Code](https://code.visualstudio.com/docs/java/java-project)

> [!IMPORTANT]
> Přečtěte si [zásady vytváření názvů](media-services-apis-overview.md#naming-conventions).

## <a name="create-a-maven-project"></a>Vytvoření projektu Maven

Otevřete nástroj příkazového řádku a `cd` v adresáři, ve kterém chcete vytvořit projekt.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Při spuštění příkazu se `pom.xml` `App.java` vytvoří soubory, a. 

## <a name="add-dependencies"></a>Přidat závislosti

1. V Visual Studio Code otevřete složku, ve které je váš projekt
1. Vyhledejte a otevřete`pom.xml`
1. Přidat potřebné závislosti

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Připojení k klientovi Java

1. Otevřete `App.java` soubor pod a ujistěte se, `src\main\java\com\azure\ams` že je balíček zahrnutý v horní části:

    ```java
    package com.azure.ams;
    ```
1. V rámci příkazu Package přidejte tyto příkazy importu:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. Pokud chcete vytvořit přihlašovací údaje služby Active Directory, které potřebujete k vytvoření žádostí, přidejte do metody Main třídy App následující kód a nastavte hodnoty, které jste získali z [rozhraní API pro přístup](./access-api-howto.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Spusťte aplikaci.

## <a name="see-also"></a>Viz také:

- [Media Services koncepty](concepts-overview.md)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Referenční dokumentace jazyka Java](https://aka.ms/ams-v3-java-ref)
- [com. Microsoft. Azure. MediaServices. v2018_07_01: Azure-Správa-média](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Další kroky

Nyní můžete zahrnout `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` a začít pracovat s entitami.

Další příklady kódu naleznete v části úložiště [ukázek sady Java SDK](/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/) .
