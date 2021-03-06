---
title: Azure Service Bus zasílání zpráv – fronty, témata a odběry
description: Tento článek poskytuje přehled entit zasílání zpráv Azure Service Bus (fronty, témata a odběry).
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: deeebf56d6e2f4ccfac37c70170a0d1cb4d272a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119169"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Fronty, témata a odběry služby Service Bus

Microsoft Azure Service Bus podporuje sadu cloudových technologií orientovaných na zprávy, včetně spolehlivé služby Řízení front zpráv a trvalého publikování/odběru zpráv. Tyto "zprostředkované" možnosti zasílání zpráv se dají představit jako funkce odděleného zasílání zpráv, které podporují publikování a dočasná odhlašování a vyrovnávání zatížení pomocí úlohy Service Bus Messaging. Oddělená komunikace má mnoho výhod – klienti a servery se například můžou spojit podle potřeby a provádět své operace asynchronním způsobem.

Mezi entity zasílání zpráv, které tvoří základní možnosti zasílání zpráv v Service Bus, patří fronty, témata a odběry a pravidla a akce.

## <a name="queues"></a>Fronty

Fronty nabízejí doručování zpráv *First in, First out* (FIFO) do jednoho nebo více konkurenčních zákazníků. To znamená, že přijímače obvykle obdrží a zpracuje zprávy v pořadí, ve kterém byly přidány do fronty, a pouze jeden příjemce zprávy obdrží a zpracuje každou zprávu. Klíčovou výhodou použití front je dosažení "" dočasného "příspojku" komponent aplikace. Jinými slovy, producenti (odesílatelé) a příjemci (příjemci) nemusí odesílat a přijímat zprávy současně, protože zprávy jsou uložené ve frontě trvale. Kromě toho producent nemusí čekat na odpověď od příjemce, aby mohl pokračovat ve zpracování a odesílání zpráv.

Související výhodou je "vyrovnávání zatížení", která umožňuje výrobcům a příjemcům posílat a přijímat zprávy s různými sazbami. V mnoha aplikacích se zatížení systému mění v průběhu času. Doba zpracování požadovaná pro každou pracovní jednotku je však obvykle konstantní. Producenti a spotřebitelé propojovací zprávy mají za to, že je nutné zřídit pouze náročné aplikace, aby bylo možné zpracovávat průměrné zatížení namísto zatížení ve špičce. S měnící se příchozí zátěží se mění hloubka fronty. Tato schopnost přímo šetří peníze v souvislosti s množstvím infrastruktury, která je nutná k provozu zatížení aplikace. Po zvýšení zátěže je možné přidat další pracovní procesy pro čtení z fronty. Každou zprávu zpracovává jen jeden pracovní proces. Toto vyrovnávání zatížení na základě vyžádání obsahu navíc umožňuje optimální využití pracovních počítačů i v případě, že se pracovní počítače liší v závislosti na výpočetním výkonu, protože vyžádají zprávy s maximální sazbou. Tento model je často označován jako "konkurenční příjemce".

Použití front ke mezilehlým výrobcům zpráv a spotřebitelům poskytuje podstatné propojení mezi komponentami. Vzhledem k tomu, že výrobci a příjemci si vzájemně nevědí, může být příjemce upgradován, aniž by to mělo žádný vliv na producenta.

### <a name="create-queues"></a>Vytvořit fronty

Fronty se vytvářejí pomocí šablon [Azure Portal](service-bus-quickstart-portal.md), [PowerShellu](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md)nebo [Správce prostředků](service-bus-resource-manager-namespace-queue.md). Pak můžete odesílat a přijímat zprávy pomocí objektu [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) .

Chcete-li se rychle naučit, jak vytvořit frontu, Odeslat a přijmout zprávy do fronty a z ní, Projděte si rychlé [starty](service-bus-quickstart-portal.md) pro jednotlivé metody. Podrobnější kurz týkající se použití front najdete v tématu [Začínáme s Service Bus fronty](service-bus-dotnet-get-started-with-queues.md).

Pracovní ukázku najdete v [ukázce BasicSendReceiveUsingQueueClient](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) na GitHubu.

### <a name="receive-modes"></a>Režimy příjmu

Můžete zadat dva různé režimy, v nichž Service Bus přijímá zprávy: *ReceiveAndDelete* nebo *PeekLock*. V režimu [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) je operace Receive v jednom snímku; To znamená, že když Service Bus obdrží požadavek od příjemce, označí zprávu jako spotřebou a vrátí ji do aplikace příjemce. **ReceiveAndDelete** režim je nejjednodušší model a funguje nejlépe ve scénářích, ve kterých aplikace může tolerovat nezpracovávání zprávy, pokud dojde k selhání. Pro pochopení tohoto scénáře Vezměte v úvahu scénář, ve kterém příjemce vystaví žádost o přijetí, a poté dojde k chybě před jejím zpracováním. Vzhledem k tomu, že Service Bus označí zprávu jako spotřebou, když se aplikace znovu spustí a začne znovu přijímat zprávy, vynechá zprávu, která byla spotřebována před selháním.

V režimu [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) se operace Receive stane dvěma fázemi, což umožňuje podporovat aplikace, které nemůžou tolerovat chybějící zprávy. Když Service Bus obdrží požadavek, najde další zprávu, která se má spotřebovat, zamkne ji, aby zabránila ostatním příjemcům v jejich přijetí, a pak ji vrátí do aplikace. Poté, co aplikace dokončí zpracování zprávy (nebo ji uloží spolehlivě pro budoucí zpracování), dokončí druhou fázi procesu Receive voláním [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) na přijaté zprávě. Když Service Bus uvidí volání **CompleteAsync** , označí zprávu jako spotřebovaná.

Pokud aplikace z nějakého důvodu nemůže zprávu zpracovat, může zavolat metodu [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) na přijatou zprávu (místo [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Tato metoda umožňuje Service Bus odemknout zprávu a zpřístupnit ji, aby ji bylo možné znovu přijmout, a to buď stejným příjemcem, nebo jiným konkurenčním zákazníkem. Za druhé je časový limit spojený s zámkem a pokud aplikace nedokáže zpracovat zprávu před vypršením časového limitu zámku (například pokud dojde k chybě aplikace), Service Bus odemkne zprávu a zpřístupní ji, aby ji bylo možné znovu přijmout (v podstatě provádět operaci [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) ).

V případě, že aplikace po zpracování zprávy dojde k chybě, ale před vydáním žádosti **CompleteAsync** je zpráva po restartování znovu doručena do aplikace. Tento proces se často nazývá *alespoň po* zpracování; To znamená, že každá zpráva se zpracuje alespoň jednou. V některých případech ale může být stejná zpráva doručena znovu. Pokud scénář nemůže tolerovat duplicitní zpracování, pak v aplikaci je vyžadována další logika, která zjišťuje duplicity, které lze dosáhnout na základě vlastnosti [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) zprávy, která zůstává při pokusůch o doručení konstantní. Tato funkce se označuje stejně jako zpracování *právě jednou* .

## <a name="topics-and-subscriptions"></a>Témata a předplatná

Na rozdíl od front, ve kterých každá zpráva zpracovává jeden uživatel, *témata* a *odběry* poskytují ve vzoru pro *publikování a odběry* formu komunikace 1: n. V případě škálování na velký počet příjemců je k dispozici každá publikovaná zpráva pro každé předplatné registrované v tématu. Zprávy jsou odesílány do tématu a doručeny do jednoho nebo více přidružených předplatných v závislosti na pravidlech filtru, která lze nastavit na základě jednotlivých předplatných. Odběry mohou použít další filtry k omezení zpráv, které chtějí přijímat. Zprávy jsou odesílány do tématu stejným způsobem, jako jsou odesílány do fronty, ale zprávy nejsou přijímány přímo z tématu. Místo toho jsou přijímána z předplatných. Předplatné tématu se podobá virtuální frontě, která přijímá kopie zpráv odesílaných do tématu. Zprávy jsou přijímány z předplatného stejně, jako jsou přijímány z fronty.

V porovnání se funkce odesílání zpráv, které jsou mapovány ve frontě, přímo do tématu a její funkce pro přijímání zpráv mapuje na předplatné. Tato funkce mimo jiné znamená, že odběry podporují stejné vzory popsané dříve v této části, s ohledem na fronty: konkurenční příjemce, dočasné oddělení, Vyrovnávání zatížení a vyrovnávání zatížení.

### <a name="create-topics-and-subscriptions"></a>Vytvoření témat a odběrů

Vytvoření tématu je podobné jako vytvoření fronty, jak je popsáno v předchozí části. Pak odešlete zprávy pomocí třídy [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) . Chcete-li dostávat zprávy, vytvořte v tématu jedno nebo více předplatných. Podobně jako fronty se zprávy z předplatného přijímají pomocí objektu [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) namísto objektu [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) . Vytvořte klienta předplatného, předejte název tématu, název předplatného a (volitelně) režim příjmu jako parametry.

Úplný pracovní příklad najdete na webu GitHub [BasicSendReceiveUsingTopicSubscriptionClient Sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) .

### <a name="rules-and-actions"></a>Pravidla a akce

V mnoha scénářích musí být zprávy, které mají specifické vlastnosti, zpracovávány různými způsoby. Chcete-li povolit toto zpracování, můžete nakonfigurovat odběry a vyhledat zprávy s požadovanými vlastnostmi a následně provést určité úpravy těchto vlastností. I když Service Bus odběry uvidí všechny zprávy odeslané do tématu, můžete zkopírovat pouze podmnožinu těchto zpráv do fronty virtuálních předplatných. Toto filtrování je provedeno pomocí filtrů předplatného. Takové úpravy se nazývají *akce filtru*. Po vytvoření předplatného můžete dodat výraz filtru, který pracuje s vlastnostmi zprávy, jak vlastnosti systému (například **popisek**), tak i vlastní vlastnosti aplikace (například **StoreName**). Výraz filtru SQL je v tomto případě volitelný. bez výrazu filtru SQL se u všech zpráv pro toto předplatné provede jakákoli akce filtru definovaná v rámci předplatného.

Úplný pracovní příklad najdete na webu GitHub [TopicSubscriptionWithRuleOperationsSample Sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) .

Další informace o možných hodnotách filtru naleznete v dokumentaci ke třídám [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) a [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) .

## <a name="java-message-service-jms-20-entities-preview"></a>Entity služby zprávy Java (JMS) 2,0 (Preview)

Klientské aplikace, které se připojují k Azure Service Bus Premium a využívají [knihovnu Azure Service Bus JMS](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) , mohou využít níže uvedené entity.

### <a name="queues"></a>Fronty

Fronty v JMS jsou sémanticky srovnatelné s tradičními Service Bus frontami popsanými výše.

Chcete-li vytvořit frontu, využijte níže uvedené metody ve `JMSContext` třídě –

```java
Queue createQueue(String queueName)
```

### <a name="topics"></a>Témata

Témata v JMS jsou sémanticky srovnatelná s tradičními tématy Service Bus popsanými výše.

Chcete-li vytvořit téma, využijte níže uvedené metody ve `JMSContext` třídě –

```java
Topic createTopic(String topicName)
```

### <a name="temporary-queues"></a>Dočasné fronty

Když klientská aplikace vyžaduje dočasnou entitu, která existuje po dobu života aplikace, může použít dočasné fronty. Ty jsou využívány ve vzoru [požadavek-odpověď](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) .

Chcete-li vytvořit dočasnou frontu, využijte níže uvedené metody ve `JMSContext` třídě –

```java
TemporaryQueue createTemporaryQueue()
```

### <a name="temporary-topics"></a>Dočasná témata

Stejně jako dočasné fronty existují dočasná témata, která umožňují publikování/přihlášení k odběru prostřednictvím dočasné entity, která existuje po dobu života aplikace.

Chcete-li vytvořit dočasné téma, využijte níže uvedené metody ve `JMSContext` třídě –

```java
TemporaryTopic createTemporaryTopic()
```

### <a name="java-message-service-jms-subscriptions"></a>Odběry zpráv JMS (Java Message Service)

I když jsou tyto odběry sémanticky podobné předplatným uvedeným výše (tj. existují v tématu a umožňují sémantiku publikování a odběru), představuje specifikace služby zprávy Java koncepty **sdílených**, **nesdílených**, **trvalých** a **netrvalých** atributů pro dané předplatné.

> [!NOTE]
> Níže uvedené odběry jsou dostupné v Azure Service Bus Premium úrovně Preview pro klientské aplikace, které se připojují k Azure Service Bus pomocí [knihovny Azure Service Bus JMS](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms).
>
> V rámci verze Public Preview nelze tyto odběry vytvořit pomocí Azure Portal.
>

#### <a name="shared-durable-subscriptions"></a>Sdílená trvalá předplatná

Sdílené odolné předplatné se používá v případě, že se všechny zprávy publikované v tématu mají přijmout a zpracovat aplikace bez ohledu na to, jestli aplikace aktivně spotřebovává předplatné z předplatného.

Vzhledem k tomu, že se jedná o sdílené předplatné, můžou z předplatného přijímat všechny aplikace, které jsou ověřené pro příjem z Service Bus.

Chcete-li vytvořit sdílené trvalé předplatné, použijte níže uvedené metody `JMSContext` třídy –

```java
JMSConsumer createSharedDurableConsumer(Topic topic, String name)

JMSConsumer createSharedDurableConsumer(Topic topic, String name, String messageSelector)
```

Sdílené trvalé předplatné bude i nadále existovat, pokud se neodstraní pomocí `unsubscribe` metody `JMSContext` třídy.

```java
void unsubscribe(String name)
```

#### <a name="unshared-durable-subscriptions"></a>Nesdílené trvalé odběry

Stejně jako u sdíleného odolného předplatného se používá nesdílené trvalé předplatné, pokud jsou všechny zprávy publikované v tématu přijaté a zpracovávané aplikací, bez ohledu na to, jestli aplikace aktivně spotřebovává z předplatného.

Vzhledem k tomu, že se jedná o nesdílené předplatné, může z něho přijmout jenom aplikace, která předplatné vytvořila.

K vytvoření nesdíleného odolného předplatného použijte níže uvedené metody z `JMSContext` třídy – 

```java
JMSConsumer createDurableConsumer(Topic topic, String name)

JMSConsumer createDurableConsumer(Topic topic, String name, String messageSelector, boolean noLocal)
```

> [!NOTE]
> Tato `noLocal` funkce je aktuálně Nepodporovaná a ignoruje se.
>

Nesdílené trvalé předplatné nadále existuje, pokud se neodstraní pomocí `unsubscribe` metody `JMSContext` třídy.

```java
void unsubscribe(String name)
```

#### <a name="shared-non-durable-subscriptions"></a>Sdílená netrvalá předplatná

Sdílené neodolné předplatné se používá v případě, že více klientských aplikací potřebuje přijímat a zpracovávat zprávy z jediného předplatného, a to až do doby, než budou aktivně spotřebovávat nebo přijímat.

Vzhledem k tomu, že odběr není trvalý, není trvalý. Toto předplatné nepřijímá zprávy, pokud k ní neexistují žádní aktivní spotřebitelé.

Chcete-li vytvořit sdílené netrvalé předplatné, vytvořte, `JmsConsumer` jak je znázorněno v níže uvedených metodách `JMSContext` třídy –

```java
JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName)

JMSConsumer createSharedConsumer(Topic topic, String sharedSubscriptionName, String messageSelector)
```

Sdílené netrvalé předplatné bude i nadále existovat, dokud neobdrží aktivní příjemci.

#### <a name="unshared-non-durable-subscriptions"></a>Nesdílené odběry, které nejsou trvalé

Nesdílené netrvalé předplatné se používá, když klientská aplikace potřebuje přijmout a zpracovat zprávu z předplatného, jenom až do chvíle, kdy se z něho bude aktivně spotřebovávat. V tomto předplatném může existovat jenom jeden příjemce, tj. na klienta, který předplatné vytvořil.

Vzhledem k tomu, že odběr není trvalý, není trvalý. Pokud není k dispozici žádný aktivní spotřebitel, zprávy toto předplatné neobdrží.

Pokud chcete vytvořit nesdílené netrvalé předplatné, vytvořte, jak je `JMSConsumer` znázorněno v níže uvedených metodách, z ' JMSContext Class- 

```java
JMSConsumer createConsumer(Destination destination)

JMSConsumer createConsumer(Destination destination, String messageSelector)

JMSConsumer createConsumer(Destination destination, String messageSelector, boolean noLocal)
```

> [!NOTE]
> Tato `noLocal` funkce je aktuálně Nepodporovaná a ignoruje se.
>

Nesdílené předplatné bez trvalého sdílení dál existuje, dokud nezíská aktivní příjemce.

#### <a name="message-selectors"></a>Selektory zpráv

Stejně jako **filtry a akce** existují pro pravidelná Service Bus předplatná, existují **Selektory zpráv** pro odběry JMS.

Selektory zpráv je možné nastavit u každého předplatného JMS a existují jako podmínka filtru ve vlastnostech záhlaví zprávy. Doručovat se jenom zprávy s vlastnostmi záhlaví, které odpovídají výrazu selektoru zpráv. Hodnota null nebo prázdný řetězec značí, že neexistuje selektor zpráv pro předplatné JMS nebo spotřebitele.

## <a name="next-steps"></a>Další kroky

Další informace a příklady použití zasílání zpráv Service Bus najdete v následujících tématech pokročilých:

* [Přehled Service Busho zasílání zpráv](service-bus-messaging-overview.md)
* [Rychlý start: Odesílání a přijímání zpráv pomocí webu Azure Portal a architektury .NET](service-bus-quickstart-portal.md)
* [Kurz: Aktualizace zásob pomocí portálu Azure Portal a témat/odběrů](service-bus-tutorial-topics-subscriptions-portal.md)


