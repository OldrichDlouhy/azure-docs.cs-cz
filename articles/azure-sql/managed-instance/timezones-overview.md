---
title: Časová pásma spravované instance Azure SQL
description: Další informace o konkrétních časových pásmech spravované instance Azure SQL
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
ms.date: 05/25/2020
ms.openlocfilehash: 84df755d4a89b83a0842a74a619fad5275396dec
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2020
ms.locfileid: "84711353"
---
# <a name="time-zones-in-azure-sql-managed-instance"></a>Časová pásma ve spravované instanci Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Koordinovaný světový čas (UTC) je doporučeným časovým pásmem pro datovou vrstvu cloudových řešení. Spravovaná instance Azure SQL nabízí také možnost volby časových pásem pro splnění potřeb stávajících aplikací, které ukládají hodnoty data a času, a volání funkcí data a času s implicitním kontextem konkrétního časového pásma.

Funkce T-SQL, jako je [GETDATE ()](/sql/t-sql/functions/getdate-transact-sql) nebo CLR Code, sledují časové pásmo nastavené na úrovni instance. Úlohy agenta SQL Server sledují také plány podle časového pásma instance.

  > [!NOTE]
  > Azure SQL Database nepodporuje nastavení časového pásma. vždy se řídí časem UTC. Použijte v [časovém pásmu](/sql/t-sql/queries/at-time-zone-transact-sql) v SQL Database, pokud potřebujete interpretovat informace o datu a čase v časovém pásmu UTC.

## <a name="supported-time-zones"></a>Podporovaná časová pásma

Sada podporovaných časových pásem je děděna ze základního operačního systému spravované instance. Pravidelně se aktualizuje a získá nové definice časových pásem a projeví se změny stávajících.

[Zásady letního času a časového pásma pro letní](https://aka.ms/time) čas garantuje historickou přesnost od 2010 do výše.

Seznam s názvy podporovaných časových pásem je zveřejněn prostřednictvím zobrazení [Sys. time_zone_info](/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) System.

## <a name="set-a-time-zone"></a>Nastavení časového pásma

Časové pásmo spravované instance lze nastavit pouze během vytváření instance. Výchozí časové pásmo je UTC.

  >[!NOTE]
  > Časové pásmo existující spravované instance nelze změnit.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Nastavení časového pásma pomocí Azure Portal

Když zadáte parametry nové instance, vyberte časové pásmo ze seznamu podporovaných časových pásem.
  
![Nastavení časového pásma během vytváření instance](./media/timezones-overview/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Šablona Azure Resource Manageru

V [šabloně správce prostředků](https://aka.ms/sql-mi-create-arm-posh) zadejte vlastnost timezoneId, která nastaví časové pásmo během vytváření instance.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Seznam podporovaných hodnot pro vlastnost timezoneId najdete na konci tohoto článku.

Pokud není zadán, je časové pásmo nastaveno na hodnotu UTC.

## <a name="check-the-time-zone-of-an-instance"></a>Zkontroluje časové pásmo instance.

Funkce [CURRENT_TIMEZONE](/sql/t-sql/functions/current-timezone-transact-sql) vrací zobrazovaný název časového pásma instance.

## <a name="cross-feature-considerations"></a>Požadavky na různé funkce

### <a name="restore-and-import"></a>Obnovení a import

Můžete obnovit záložní soubor nebo importovat data do spravované instance z instance nebo serveru s jiným nastavením časového pásma. Ujistěte se, že to uděláte opatrně. Analýza chování aplikace a výsledků dotazů a sestav, stejně jako při přenosu dat mezi dvěma SQL Server instancemi s odlišným nastavením časových pásem.

### <a name="point-in-time-restore"></a>Obnovení k určitému bodu v čase

Když provedete obnovení k určitému bodu v čase, doba obnovení do je interpretována jako čas UTC. Tímto způsobem se vyhnete nejasnostem způsobeným letním časem a jeho potenciálním změnám.

### <a name="auto-failover-groups"></a>Skupiny automatického převzetí služeb při selhání

Použití stejného časového pásma v rámci primární a sekundární instance ve skupině převzetí služeb při selhání není vynutilo, ale důrazně doporučujeme.

  >[!WARNING]
  > Důrazně doporučujeme použít stejné časové pásmo pro primární a sekundární instanci ve skupině převzetí služeb při selhání. Z důvodu určitých vzácných případů použití je zachovávání stejného časového pásma napříč primárními a sekundárními instancemi vynutilo. Je důležité si uvědomit, že v případě ručního nebo automatického převzetí služeb při selhání sekundární instance uchová své původní časové pásmo.

## <a name="limitations"></a>Omezení

- Časové pásmo existující spravované instance nelze změnit.
- Externí procesy spouštěné z úloh agenta SQL Server nezohledňují časové pásmo instance.

## <a name="list-of-supported-time-zones"></a>Seznam podporovaných časových pásem

| **ID časového pásma** | **Zobrazovaný název časového pásma** |
| --- | --- |
| Datová hranice ((běžný čas) | (UTC-12:00) Mezinárodní časová osa – západ |
| UTC-11 | (UTC-11:00) Koordinovaný světový čas-11 |
| Aleutské (běžný čas) | (UTC-10:00) Aleutské ostrovy |
| Havajština (běžný čas) | (UTC-10:00) Ostrovy |
| Souostroví Markézy (běžný čas) | (UTC-09:30) Souostroví Markézy ostrovy |
| Aljaška ((běžný čas) | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Koordinovaný světový čas-09 |
| Tichomoří (běžný čas) (Mexiko) | (UTC-08:00) Baja Kalifornie |
| UTC-08 | (UTC-08:00) Koordinovaný světový čas – 08 |
| Tichomoří (běžný čas) | (UTC-08:00) Tichomoří (USA & Kanada) |
| Horská oblast USA (běžný čas) | (UTC-07:00) Arizona |
| Horská oblast – Mexiko (běžný čas) | (UTC-07:00) Chihuahua, La Paz, Mazatlan |
| Horská oblast (běžný čas) | (UTC-07:00) Horská oblast USA & Kanada |
| Střední Amerika (běžný čas) | (UTC-06:00) Střední Amerika |
| Střední oblast (běžný čas) | (UTC-06:00) Středový čas (US & Kanada) |
| Velikonoční ostrov (běžný čas) | (UTC-06:00) Velikonoční ostrov |
| Střední oblast – Mexiko (běžný čas) | (UTC-06:00) Guadalajara, Mexiko, Monterrey |
| Střední Kanada (běžný čas) | (UTC-06:00) Saskatchewan |
| SA – Tichomoří (běžný čas) | (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Východní oblast – Mexiko (běžný čas) | (UTC-05:00) Chetumal |
| Východní oblast (běžný čas) | (UTC-05:00) Východ (USA & Kanada) |
| Haiti (běžný čas) | (UTC-05:00) Haiti |
| Kuba (běžný čas) | (UTC-05:00) Havana |
| USA – východ (běžný čas) | (UTC-05:00) Indiana (východ) |
| Turks a Caicos (běžný čas) | (UTC-05:00) Turks a Caicos |
| Paraguay (běžný čas) | (UTC-04:00) Asunción |
| Atlantik (běžný čas) | (UTC-04:00) Atlantik (Kanada) |
| Venezuela (běžný čas) | (UTC-04:00) Caracas |
| Střední Brazílie (běžný čas) | (UTC-04:00) Cuiaba |
| SA (západní běžný čas) | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Tichomoří SA (běžný čas) | (UTC-04:00) Santiago de Chile |
| Newfoundland (běžný čas) | (UTC-03:30) Newfoundland |
| Tocantins ((běžný čas) | (UTC-03:00) Araguaína |
| E. Jižní Amerika (běžný čas) | (UTC-03:00) Brazílie |
| SA – východ (běžný čas) | (UTC-03:00) Cayenne, Fortaleza |
| Argentina (běžný čas) | (UTC-03:00) Město Buenos Aires |
| Grónsko (běžný čas) | (UTC-03:00) Grónsko |
| Montevideo ((běžný čas) | (UTC-03:00) Montevideo ( |
| Magallanský (běžný čas) | (UTC-03:00) Punta Arenas |
| Saint-Pierre (běžný čas) | (UTC-03:00) Saint-Pierre a Miquelon |
| Bahia ((běžný čas) | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Koordinovaný univerzální čas-02 |
| Střední Atlantik (běžný čas) | (UTC-02:00) Střední Atlantik – starý |
| Azory (běžný čas) | (UTC-01:00) Azory |
| Cabo Verde (běžný čas) | (UTC-01:00) Cabo Verde. |
| UTC | Koordinovaný univerzální čas (UTC) |
| GMT (běžný čas) | (UTC + 00:00) Dublin, Edinburgh, Lisabon, Londýn |
| Greenwichský běžný čas | (UTC + 00:00) Monrovia, Reykjavík |
| W. Evropa (běžný čas) | (UTC + 01:00) Amsterdam, Berlín, Bern, Řím, Stockholm, Vídeň |
| Střední Evropa (běžný čas) | (UTC + 01:00) Praha, Bratislava, Budapešť, Lublaň, Praha |
| Románské země (běžný čas) | (UTC + 01:00) Brusel, Kodaň, Madrid, Paříž |
| Maroko (běžný čas) | (UTC + 01:00) Casablanca |
| Svatý Tomáš (běžný čas) | (UTC + 01:00) Svatý Tomáš |
| Střední Evropa (běžný čas) | (UTC + 01:00) Sarajevo, Skopje, Varšava, Záhřeb |
| W. Střední Afrika (běžný čas) | (UTC + 01:00) Západ střední Afriky |
| Jordánsko (běžný čas) | (UTC + 02:00) Ammán |
| Balkán ((běžný čas) | (UTC + 02:00) Atény, Bukurešť |
| Střední východ (běžný čas) | (UTC + 02:00) Bejrút |
| Egypt (běžný čas) | (UTC + 02:00) Cairo |
| E. Evropa (běžný čas) | (UTC + 02:00) Kišiněv |
| Sýrie (běžný čas) | (UTC + 02:00) Damašek |
| Západní banka (běžný čas) | (UTC + 02:00) Gazy, Hebron |
| Jižní Afrika (běžný čas) | (UTC + 02:00) Harare, Pretoria |
| Finsko ((běžný čas) | (UTC + 02:00) Helsinky, Kyjev, Riga, Sofie, Tallin, Vilnius |
| Izrael (běžný čas) | (UTC + 02:00) Časové |
| Kaliningrad (běžný čas) | (UTC + 02:00) Kaliningrad |
| Súdán (běžný čas) | (UTC + 02:00) Chartúm |
| Libye (běžný čas) | (UTC + 02:00) Tripolis |
| Namibie (běžný čas) | (UTC + 02:00) Windhoek |
| Arabský běžný čas | (UTC + 03:00) Bagdád |
| Turecko (běžný čas) | (UTC + 03:00) Istanbul |
| Arabského času (běžný čas) | (UTC + 03:00) Kuvajt, Rijád |
| Bělorusko (běžný čas) | (UTC + 03:00) Minsk |
| Ruština (běžný čas) | (UTC + 03:00) Moskva, Svatá Petěrburg |
| E. Afrika (běžný čas) | (UTC + 03:00) Nairobi |
| Írán (běžný čas) | (UTC + 03:30) Teherán |
| Arábie ((běžný čas) | (UTC + 04:00) Abú Dhabi, Muskat |
| Astrachaň (běžný čas) | (UTC + 04:00) Astrachaň, Uljanovsk |
| Ázerbájdžán (běžný čas) | (UTC + 04:00) Baku |
| Čas Ruska Zóna 3 | (UTC + 04:00) Iževsk, Samara |
| Mauricius (běžný čas) | (UTC + 04:00) Louis portu |
| Saratovský (běžný čas) | (UTC + 04:00) Saratovský |
| Gruzie (běžný čas) | (UTC + 04:00) Tbilisi |
| Volgograd (běžný čas) | (UTC + 04:00) Volgograd |
| Kavkaz ((běžný čas) | (UTC + 04:00) Jerevan |
| Afghánistán (běžný čas) | (UTC + 04:30) Kábul |
| Západní Asie (běžný čas) | (UTC + 05:00) Ašchabád, Taškent |
| Jekatěrinburg (běžný čas) | (UTC + 05:00) Jekatěrinburg |
| Pákistán (běžný čas) | (UTC + 05:00) Islámábád, Karáčí |
| Indie (běžný čas) | (UTC + 05:30) Čennaj, Kolkata (Kalkata, Bombaj, Nové Dillí |
| Srí Lanka (běžný čas) | (UTC + 05:30) Srí Džajawardenepúra |
| Nepál (běžný čas) | (UTC + 05:45) Káthmándú |
| Střední Asie (běžný čas) | (UTC + 06:00) Astana |
| Bangladéš (běžný čas) | (UTC + 06:00) Dháka |
| Omsk (běžný čas) | (UTC + 06:00) Omsk |
| Myanmar (běžný čas) | (UTC + 06:30) Yangon (Rangún) |
| JIHOVÝCHODNÍ Asie (běžný čas) | (UTC + 07:00) Bangkok, Hanoj, Jakarta |
| Altajský (běžný čas) | (UTC + 07:00) Barnaul, Gorno-Altajsk |
| W. Mongolsko (běžný čas) | (UTC + 07:00) Chovd |
| Severní Asie (běžný čas) | (UTC + 07:00) Krasnojarsk |
| N. Střední Asie (běžný čas) | (UTC + 07:00) Novosibirsk |
| Tomská oblast (běžný čas) | (UTC + 07:00) Tomská oblast |
| Čína (běžný čas) | (UTC + 08:00) Peking, Čchung-čching, Hongkong, Urumqi |
| Východ severní Asie (běžný čas) | (UTC + 08:00) Irkutsk |
| Singapur (běžný čas) | (UTC + 08:00) Kuala Lumpur, Singapur |
| W. Austrálie (běžný čas) | (UTC + 08:00) Perth |
| Taipei (běžný čas) | (UTC + 08:00) WAN |
| Ulánbátar (běžný čas) | (UTC + 08:00) Ulánbátar |
| Austrálie – střed (běžný čas) | (UTC + 08:45) Eucla |
| Zabajkalský (běžný čas) | (UTC + 09:00) Čita |
| Tokio (běžný čas) | (UTC + 09:00) Ósaka, Sapporo, Tokio |
| Severní Korea (běžný čas) | (UTC + 09:00) Pchjongjang |
| Korea (běžný čas) | (UTC + 09:00) Soul |
| Jakutsk (běžný čas) | (UTC + 09:00) Jakutsk |
| Normalizovan. Austrálie (běžný čas) | (UTC + 09:30) Adelaide |
| Střední Austrálie (běžný čas) | (UTC + 09:30) Darwin |
| E. Austrálie (běžný čas) | (UTC + 10:00) Brisbane |
| Východní Austrálie (běžný čas) | (UTC + 10:00) Canberra, Melbourne, Sydney |
| Západní Tichomoří (běžný čas) | (UTC + 10:00) Guam, Port Moresby |
| Tasmánie ((běžný čas) | (UTC + 10:00) Hobart |
| Vladivostok ((běžný čas) | (UTC + 10:00) Vladivostok ( |
| Ostrov lorda Howa (běžný čas) | (UTC + 10:30) Ostrov lorda Howa ostrov |
| Bougainville (běžný čas) | (UTC + 11:00) Bougainville ostrov |
| Rusko – časové pásmo 10 | (UTC + 11:00) Čokurdach |
| Magadan (běžný čas) | (UTC + 11:00) Magadan |
| Norfolk (běžný čas) | (UTC + 11:00) Ostrov Norfolk |
| Sachalin (běžný čas) | (UTC + 11:00) Sachalin |
| Střední Tichomoří (běžný čas) | (UTC + 11:00) Šalamounovy ostrovy, Nová Kaledonie |
| Rusko – časové pásmo 11 | (UTC + 12:00) Anadyr, Petropavlovsk-Kamčatskij |
| Nový Zéland (běžný čas) | (UTC + 12:00) Auckland, Wellington |
| UTC + 12 | (UTC + 12:00) Koordinovaný světový čas + 12 |
| Fidži (běžný čas) | (UTC + 12:00) Fidži |
| Kamčatka (běžný čas) | (UTC + 12:00) Petropavlovsk-Kamčatskij – starý |
| Chathamovy ostrovy (běžný čas) | (UTC + 12:45) Chathamovy ostrovy |
| UTC + 13 | (UTC + 13:00) Koordinovaný světový čas + 13 |
| Tonga (běžný čas) | (UTC + 13:00) Nuku ' Alofa |
| Samoa (běžný čas) | (UTC + 13:00) Samoa |
| Ostrovy line (běžný čas) | (UTC + 14:00) Kiritimati ostrov |

## <a name="see-also"></a>Viz také 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [CURRENT_TIMEZONE_ID (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-id-transact-sql)
- [V ČASOVÉm PÁSMu (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys. time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
