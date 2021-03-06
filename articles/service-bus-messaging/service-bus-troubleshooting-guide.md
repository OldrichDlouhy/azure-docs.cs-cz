---
title: Průvodce odstraňováním potíží pro Azure Service Bus | Microsoft Docs
description: Tento článek poskytuje seznam výjimek zasílání zpráv Azure Service Bus a navrhovaných akcí, které se mají učinit, když dojde k výjimce.
ms.topic: article
ms.date: 07/15/2020
ms.openlocfilehash: 6071aae85daa1852c9384656d7caf5e2deffd84e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2020
ms.locfileid: "87071302"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Průvodce odstraňováním potíží pro Azure Service Bus
V tomto článku najdete tipy a doporučení pro odstraňování problémů s několika problémy, které se můžou zobrazit při použití Azure Service Bus. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Problémy s připojením, certifikátem nebo časovým limitem
Následující postup vám může pomáhat s odstraňováním potíží s připojením/vypršením časového limitu pro všechny služby v rámci *. servicebus.windows.net. 

- Přejděte na nebo [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/` . Pomáhá s kontrolou, jestli máte problémy s filtrováním IP adres nebo virtuální sítí nebo řetězem certifikátů, které jsou obvyklé při použití sady Java SDK.

    Příklad úspěšné zprávy:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Příklad chybové zprávy o selhání:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Spusťte následující příkaz, který zkontroluje, jestli je v bráně firewall blokovaný port. Použité porty jsou 443 (HTTPS), 5671 (AMQP) a 9354 (NET Messaging/SBMP). V závislosti na knihovně, kterou používáte, se používají také jiné porty. Tady je ukázkový příkaz, který zkontroluje, jestli je port 5671 blokovaný. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    V systému Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Pokud dochází k problémům s přerušovaným připojením, spusťte následující příkaz, který zkontroluje, jestli nedošlo k vyřazeným paketům. Tento příkaz se pokusí vytvořit 25 různých připojení TCP každé 1 sekundy ke službě. Pak můžete zjistit, kolik z nich bylo úspěšné/neúspěšné, a také zobrazit latenci připojení TCP. Nástroj si můžete stáhnout `psping` [tady](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    Ekvivalentní příkazy můžete použít, pokud používáte jiné nástroje `tnc` , například, `ping` a tak dále. 
- Získejte trasování sítě, pokud předchozí kroky neumožňují a neanalyzují ho pomocí nástrojů, jako je třeba [Wireshark](https://www.wireshark.org/). V případě potřeby kontaktujte [Podpora Microsoftu](https://support.microsoft.com/) . 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Problémy, které se mohou vyskytnout při upgradech nebo restartu služby

### <a name="symptoms"></a>Příznaky
- Žádosti můžou být v neomezeném omezení.
- Je možné, že v příchozích zprávách nebo požadavcích dojde k přerušení.
- Soubor protokolu může obsahovat chybové zprávy.
- Aplikace se můžou po několik sekund odpojit od služby.

### <a name="cause"></a>Příčina
Aktualizace a restarty back-end služby můžou způsobovat tyto problémy ve vašich aplikacích.

### <a name="resolution"></a>Řešení
Pokud kód aplikace používá sadu SDK, zásady opakování jsou již vytvořeny a aktivní. Aplikace se znovu připojí bez významného dopadu na aplikaci nebo pracovní postup.

## <a name="unauthorized-access-send-claims-are-required"></a>Neautorizovaný přístup: vyžaduje se odeslání deklarací identity.

### <a name="symptoms"></a>Příznaky 
Tato chyba se může zobrazit při pokusu o přístup k Service Bus tématu ze sady Visual Studio na místním počítači pomocí spravované identity přiřazené uživatelem s oprávněním odeslat.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

### <a name="cause"></a>Příčina
Identita nemá oprávnění pro přístup k Service Busmu tématu. 

### <a name="resolution"></a>Řešení
Pokud chcete tuto chybu vyřešit, nainstalujte knihovnu [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .  Další informace najdete v tématu [ověřování místního vývoje](..\key-vault\service-to-service-authentication.md#local-development-authentication). 

Informace o tom, jak přiřadit oprávnění k rolím, najdete v tématu [ověřování spravované identity pomocí Azure Active Directory pro přístup k prostředkům Azure Service Bus](service-bus-managed-service-identity.md).

## <a name="service-bus-exception-put-token-failed"></a>Výjimka Service Bus: token Put se nezdařil.

### <a name="symptoms"></a>Příznaky
Při pokusu o odeslání více než 1000 zpráv pomocí stejného Service Bus Připojení se zobrazí následující chybová zpráva: 

`Microsoft.Azure.ServiceBus.ServiceBusException: Put token failed. status-code: 403, status-description: The maximum number of '1000' tokens per connection has been reached.` 

### <a name="cause"></a>Příčina
Existuje omezení počtu tokenů, které se používají k posílání a přijímání zpráv pomocí jediného připojení k Service Busmu oboru názvů. Je to 1000. 

### <a name="resolution"></a>Řešení
Pro odeslání dalších zpráv otevřete nové připojení k Service Busmu oboru názvů.

## <a name="next-steps"></a>Další kroky
Viz následující články: 

- [Azure Resource Manager výjimky](service-bus-resource-manager-exceptions.md). Vygeneruje výjimky vygenerované při interakci s Azure Service Bus pomocí Azure Resource Manager (prostřednictvím šablon nebo přímých volání).
- [Výjimky zasílání zpráv](service-bus-messaging-exceptions.md). Poskytuje seznam výjimek generovaných .NET Framework pro Azure Service Bus. 

