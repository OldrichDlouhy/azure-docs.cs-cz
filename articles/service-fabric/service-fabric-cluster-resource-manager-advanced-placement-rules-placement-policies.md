---
title: Service Fabric zásady umístění Správce prostředků clusteru
description: Přehled dalších zásad umístění a pravidel pro Service Fabric služby
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9aea157d03f344e07a81f0588d3e0127f17ca75d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "75834419"
---
# <a name="placement-policies-for-service-fabric-services"></a>Zásady umístění pro služby Service Fabric
Zásady umístění jsou další pravidla, která se dají použít k řízení umístění služby v některých specifických, méně častých scénářích. Příklady těchto scénářů:

- Váš Service Fabric cluster zahrnuje geografické vzdálenosti, jako je například několik místních datových center nebo napříč oblastmi Azure.
- Vaše prostředí zahrnuje více oblastí geopolitické nebo právní kontroly nebo jiné případy, kdy máte hranice zásad, které je potřeba vyhovět.
- V důsledku velkých vzdáleností nebo používání pomalejších nebo méně spolehlivých síťových propojení dochází k důležitým informacím o výkonu nebo latenci komunikace.
- Je potřeba uchovávat určité úlohy společně umístěného jako nejlepší úsilí, a to buď s ostatními úlohami, nebo v blízkosti zákazníků.

Většina těchto požadavků je zarovnaná s fyzickým rozložením clusteru, který je reprezentován jako doména selhání clusteru. 

Pokročilé zásady umístění, které vám pomůžou vyřešit tyto scénáře:

1. Neplatné domény
2. Požadované domény
3. Preferované domény
4. Nepovoluje se balení repliky.

Většinu následujících ovládacích prvků lze nakonfigurovat prostřednictvím vlastností uzlů a omezení umístění, ale některé jsou složitější. Aby se zjednodušily věci, Service Fabric cluster Správce prostředků poskytuje tyto další zásady umístění. Zásady umístění se konfigurují na základě instance služby podle názvu. Můžou se taky aktualizovat dynamicky.

## <a name="specifying-invalid-domains"></a>Zadání neplatných domén
Zásada umístění **InvalidDomain** umožňuje určit, že konkrétní doména selhání není pro konkrétní službu platná. Tato zásada zajišťuje, že se konkrétní služba nikdy nespustí v konkrétní oblasti, například pro geopolitické nebo firemní účely. Více neplatných domén může být zadáno pomocí samostatných zásad.

<center>

![Neplatný příklad domény][Image1]
</center>

Kód:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Určení požadovaných domén
Požadovaná zásada umístění domény vyžaduje, aby byla služba přítomná jenom v zadané doméně. Více požadovaných domén lze zadat pomocí samostatných zásad.

<center>

![Příklad požadované domény][Image2]
</center>

Kód:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Určení upřednostňované domény pro primární repliky stavové služby
Upřednostňovaná primární doména určuje doménu selhání, do které se umístí primární doména. Primární končí v této doméně, pokud je všechno v pořádku. Pokud se doména nebo primární replika nepovede nebo dojde k výpadku, primární se přesune do jiného umístění, v ideálním případě ve stejné doméně. Pokud se toto nové umístění nenachází v upřednostňované doméně, Správce prostředků ho znovu přesunout do upřednostňované domény, co nejrychleji. Toto nastavení je přirozeně vhodné jenom pro stavové služby. Tato zásada je nejužitečnější v clusterech, které jsou rozloženy napříč oblastmi Azure nebo více datacentry, ale mají služby, které upřednostňují umístění v určitém umístění. Udržování primárních primárních uživatelů nebo jiných služeb pomáhá zajistit nižší latenci, zejména pro čtení, které jsou ve výchozím nastavení zpracovávány v primárních počítačích.

<center>

![Preferované primární domény a převzetí služeb při selhání][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(primaryDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Vyžadování distribuce repliky a nepovolení balení
Repliky jsou _obvykle_ distribuovány mezi selhání a upgradovací domény, když je cluster v pořádku. Existují však případy, kdy může dojít k dočasnému zabalení více replik pro daný oddíl do jedné domény. Řekněme například, že cluster má devět uzlů ve třech doménách selhání, FD:/0, FD:/1 a FD:/2. Řekněme také, že vaše služba má tři repliky. Řekněme, že se uzly, které byly použity pro tyto repliky v FD:/1 a FD:/2, přestaly používat. Obvykle by cluster Správce prostředků preferovat jiné uzly v těchto doménách selhání. V takovém případě řekněme, že v důsledku potíží s kapacitou nejsou žádné další uzly v těchto doménách platné. Pokud cluster Správce prostředků sestaví náhrady pro tyto repliky, museli byste vybrat uzly v FD:/0. To však vytvoří situaci _,_ kdy je porušení omezení domény selhání. Dobalením se zvyšuje pravděpodobnost, že by celá sada replik mohla přijít nebo může dojít ke ztrátě. 

> [!NOTE]
> Další informace o omezeních a prioritách omezení obvykle najdete v [tomto tématu](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Pokud jste někdy viděli zprávu o stavu, například " `The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain` ", pak jste narazili na tuto podmínku nebo něco podobného. Obvykle se dočasně balí jenom jedna nebo dvě repliky. Pokud je v dané doméně méně než kvorum replik, budete v bezpečí. Balení je zřídka, ale může se stát, ale tyto situace jsou obvykle přechodné, protože se uzly vrátí zpět. Pokud uzly zůstanou mimo provoz a cluster Správce prostředků musí nastavovat náhrady, obvykle jsou v ideálních doménách selhání k dispozici další uzly.

Některé úlohy by preferovat vždycky mít cílový počet replik, i když se zabalí do méně domén. Tyto úlohy se sází proti celkovému počtu současných trvalých selhání domény a obvykle můžou obnovit místní stav. Jiné úlohy by místo toho mohly trvat výpadky před rizikem nebo ztrátou dat. Většina produkčních úloh běží s více než třemi replikami, více než třemi doménami selhání a řadou platných uzlů na doménu selhání. Z tohoto důvodu výchozí chování umožňuje dobalení domény ve výchozím nastavení. Výchozí chování umožňuje normální vyrovnávání a převzetí služeb při selhání při zpracování těchto extrémních případů, i když to znamená dočasná dobalení domény.

Pokud chcete toto balení pro danou úlohu zakázat, můžete zadat `RequireDomainDistribution` zásady služby. Pokud je tato zásada nastavená, Správce prostředků clusteru zajistí, aby se žádné dvě repliky ze stejného oddílu nespouštěly ve stejné chybě nebo v upgradovací doméně.

Kód:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nyní by bylo možné použít tyto konfigurace pro služby v clusteru, který nebyl geograficky rozložený? Je možné, že to ale není velký důvod. Požadovaná, neplatná a Upřednostňovaná konfigurace domény by se měla vyhnout, pokud je nevyžadují jejich scénáře. Nedává žádný smysl, aby se pokusil vynutit spuštění dané úlohy v jednom stojanu nebo preferovat některé segmenty svého místního clusteru přes jiné. Různé konfigurace hardwaru by měly být rozloženy mezi doménami selhání a zpracovávány prostřednictvím normálního omezení umístění a vlastností uzlu.

## <a name="next-steps"></a>Další kroky
- Další informace o konfiguraci služeb najdete v informacích [o konfiguraci služeb](service-fabric-cluster-resource-manager-configure-services.md) .

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
