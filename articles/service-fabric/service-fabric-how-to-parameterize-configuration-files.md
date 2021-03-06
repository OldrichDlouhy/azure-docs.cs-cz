---
title: Parametrizovat konfigurační soubory v Azure Service Fabric
description: Naučte se, jak parametrizovat konfigurační soubory v Service Fabric, což je užitečná technika při správě více prostředí.
author: mikkelhegn
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: 4e96a732cffd70b0a5c24e7ebafe214297a72720
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75644626"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Jak parametrizovat konfigurační soubory v Service Fabric

V tomto článku se dozvíte, jak parametrizovat konfigurační soubor v Service Fabric.  Pokud ještě nejste obeznámeni se základními koncepty správy aplikací pro více prostředí, přečtěte si téma [Správa aplikací pro více prostředí](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Postup pro konfigurační soubory Parametrizace

V tomto příkladu přepíšete konfigurační hodnotu pomocí parametrů ve vašem nasazení aplikace.

1. Otevřete * \<MyService>\PackageRoot\Config\Settings.xml* soubor v projektu služby.
1. Přidáním následujícího kódu XML nastavte název a hodnotu konfiguračního parametru, například velikost mezipaměti rovnou 25.

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Uložte soubor a zavřete ho.
1. Otevřete soubor * \<MyApplication>\ApplicationPackageRoot\ApplicationManifest.xml* .
1. V souboru ApplicationManifest.xml deklarujte parametr a výchozí hodnotu v `Parameters` elementu.  Doporučuje se, aby název parametru obsahoval název služby (například "Mojesluzba").

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. V `ServiceManifestImport` části ApplicationManifest.xml souboru přidejte `ConfigOverrides` `ConfigOverride` element and, odkazování na konfigurační balíček, část a parametr.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> V případě, že přidáte ConfigOverride, Service Fabric vždy zvolí parametry aplikace nebo výchozí hodnotu zadanou v manifestu aplikace.
>
>

## <a name="next-steps"></a>Další kroky
Informace o dalších možnostech správy aplikací, které jsou k dispozici v sadě Visual Studio, najdete v tématu [Správa aplikací Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).
