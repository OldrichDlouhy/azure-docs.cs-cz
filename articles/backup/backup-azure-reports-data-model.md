---
title: Datový model pro události diagnostiky Azure Backup
description: Tento datový model se odkazuje na režim konkrétní prostředek odeslání diagnostických událostí na Log Analytics (LA).
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 46d40694da4eb025afc11da0f14b28691bf13bb8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/20/2020
ms.locfileid: "86538866"
---
# <a name="data-model-for-azure-backup-diagnostics-events"></a>Datový model pro události diagnostiky Azure Backup

## <a name="coreazurebackup"></a>CoreAzureBackup

Tato tabulka poskytuje informace o základních entitách zálohování, jako jsou trezory a zálohované položky.

| **Pole**                         | **Typ dat** | **Popis**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                        | Text          | Identifikátor prostředku pro shromažďovaná data Například ID prostředku úložiště Recovery Services. |
| OperationName                     | Text          | Toto pole představuje název aktuální operace – BackupItem, BackupItemAssociation nebo ProtectedContainer. |
| Kategorie                          | Text          | Toto pole představuje kategorii dat diagnostiky nabízených do protokolů Azure Monitor. Například CoreAzureBackup. |
| AgentVersion                      | Text          | Číslo verze zálohy agenta nebo agenta ochrany (v případě SC DPM a MABS) |
| AzureBackupAgentVersion           | Text          | Verze agenta Azure Backup na serveru pro správu zálohování |
| AzureDataCenter                   | Text          | Datové centrum, ve kterém se nachází trezor                       |
| BackupItemAppVersion              | Text          | Verze aplikace pro zálohovanou položku                       |
| BackupItemFriendlyName            | Text          | Popisný název zálohované položky                             |
| BackupItemName                    | Text          | Název zálohované položky                                      |
| BackupItemProtectionState         | Text          | Stav ochrany zálohované položky                          |
| BackupItemFrontEndSize            | Text          | Velikost front-endu pro zálohovanou položku                            |
| BackupItemType                    | Text          | Typ zálohované položky Příklad: virtuální počítač, složka adresáře             |
| BackupItemUniqueId                | Text          | Jedinečný identifikátor zálohované položky                         |
| BackupManagementServerType        | Text          | Typ záložního serveru pro správu, jako v MABS, SC DPM     |
| BackupManagementServerUniqueId    | Text          | Pole pro jedinečnou identifikaci serveru pro správu zálohování      |
| BackupManagementType              | Text          | Typ poskytovatele pro server, který provádí úlohu zálohování. Například IaaSVM, složka. |
| BackupManagementServerName        | Text          | Název serveru pro správu zálohování                         |
| BackupManagementServerOSVersion   | Text          | Verze operačního systému serveru pro správu zálohování                   |
| BackupManagementServerVersion     | Text          | Verze serveru pro správu zálohování                      |
| LatestRecoveryPointLocation       | Text          | Umístění posledního bodu obnovení pro zálohovanou položku    |
| LatestRecoveryPointTime           | DateTime      | Datum a čas posledního bodu obnovení pro zálohovanou položku   |
| OldestRecoveryPointLocation       | Text          | Umístění nejstaršího bodu obnovení pro zálohovanou položku    |
| OldestRecoveryPointTime           | DateTime      | Datum a čas posledního bodu obnovení pro zálohovanou položku   |
| PolicyUniqueId                    | Text          | Jedinečné ID pro identifikaci zásad                             |
| ProtectedContainerFriendlyName    | Text          | Popisný název chráněného serveru                        |
| ProtectedContainerLocation        | Text          | Bez ohledu na to, jestli je chráněný kontejner umístěný místně nebo v Azure |
| ProtectedContainerName            | Text          | Název chráněného kontejneru                            |
| ProtectedContainerOSType          | Text          | Typ operačního systému chráněného kontejneru                          |
| ProtectedContainerOSVersion       | Text          | Verze operačního systému chráněného kontejneru                        |
| ProtectedContainerProtectionState | Text          | Stav ochrany chráněného kontejneru                  |
| ProtectedContainerType            | Text          | Zda chráněný kontejner je server nebo kontejner  |
| ProtectedContainerUniqueId        | Text          | Jedinečné ID, které slouží k identifikaci chráněného kontejneru pro vše kromě virtuálních počítačů zálohovaných pomocí DPM, MABS |
| ProtectedContainerWorkloadType    | Text          | Typ zálohovaného chráněného kontejneru. Například IaaSVMContainer |
| ProtectionGroupName               | Text          | Název skupiny ochrany, ve které je chráněná položka zálohy, pro SC DPM a MABS, pokud je k dispozici |
| ResourceGroupName                 | Text          | Skupina prostředků prostředku (například trezor Recovery Services) pro shromažďovaná data |
| SchemaVersion                     | Text          | Toto pole označuje aktuální verzi schématu. je **v2** |
| SecondaryBackupProtectionState    | Text          | Zda je pro zálohovanou položku povolena sekundární ochrana  |
| Stav                             | Text          | Stav objektu zálohované položky Například aktivní, odstraněno |
| StorageReplicationType            | Text          | Typ replikace úložiště pro trezor Například Neredundantní |
| SubscriptionId                    | Text          | Identifikátor předplatného prostředku (například Recovery Services trezor), pro který se shromažďují data |
| VaultName                         | Text          | Název trezoru                                            |
| VaultTags                         | Text          | Značky přidružené k prostředku trezoru                    |
| VaultUniqueId                     | Text          | Jedinečný identifikátor trezoru                             |
| SourceSystem                      | Text          | Zdrojový systém aktuálních dat – Azure                  |

## <a name="addonazurebackupalerts"></a>AddonAzureBackupAlerts

Tato tabulka poskytuje podrobnosti o polích souvisejících s výstrahami.

| **Pole**                      | **Typ dat** | **Popis**                                              |
| :----------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | Jedinečný identifikátor prostředku, pro který se shromažďují data Například ID prostředku úložiště Recovery Services |
| OperationName                  | Text          | Název aktuální operace Například výstraha            |
| Kategorie                       | Text          | Kategorie dat diagnostiky nabízených do protokolů Azure Monitor – AddonAzureBackupAlerts |
| AlertCode                      | Text          | Kód pro jedinečnou identifikaci typu výstrahy                     |
| AlertConsolidationStatus       | Text          | Zjistit, jestli je výstraha konsolidovaná výstraha nebo ne         |
| AlertOccurrenceDateTime        | DateTime      | Datum a čas, kdy byla výstraha vytvořena                     |
| AlertRaisedOn                  | Text          | Typ entity, na které se aktivuje výstraha                        |
| AlertSeverity                  | Text          | Závažnost výstrahy Například kritický                 |
| AlertStatus                    | Text          | Stav výstrahy Například aktivní                     |
| AlertTimeToResolveInMinutes    | Číslo        | Čas potřebný k vyřešení výstrahy. Prázdné pro aktivní výstrahy.     |
| AlertType                      | Text          | Typ výstrahy Například zálohování                           |
| AlertUniqueId                  | Text          | Jedinečný identifikátor vygenerované výstrahy                    |
| BackupItemUniqueId             | Text          | Jedinečný identifikátor zálohované položky přidružené k výstraze |
| BackupManagementServerUniqueId | Text          | Pole k jednoznačné identifikaci záložního serveru pro správu, ve kterém je chráněná položka, pokud je k dispozici |
| BackupManagementType           | Text          | Typ poskytovatele pro server, který provádí úlohu zálohování, například IaaSVM, složka adresáře |
| CountOfAlertsConsolidated      | Číslo        | Počet sloučených výstrah, pokud se jedná o konsolidovanou výstrahu  |
| ProtectedContainerUniqueId     | Text          | Jedinečný identifikátor chráněného serveru přidruženého k výstraze |
| RecommendedAction              | Text          | Akce doporučená pro vyřešení výstrahy                      |
| SchemaVersion                  | Text          | Aktuální verze schématu, například **v2**            |
| Stav                          | Text          | Aktuální stav objektu výstrahy, například aktivní, odstraněn |
| StorageUniqueId                | Text          | Jedinečné ID použité k identifikaci entity úložiště                |
| VaultUniqueId                  | Text          | Jedinečné ID, které slouží k identifikaci trezoru souvisejícího s výstrahou    |
| SourceSystem                   | Text          | Zdrojový systém aktuálních dat – Azure                    |

## <a name="addonazurebackupprotectedinstance"></a>AddonAzureBackupProtectedInstance

Tato tabulka poskytuje základní pole související s chráněnými instancemi.

| **Pole**                      | **Typ dat** | **Popis**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | Jedinečný identifikátor prostředku, pro který se shromažďují data Například ID prostředku úložiště Recovery Services |
| OperationName                  | Text          | Název operace, například ProtectedInstance         |
| Kategorie                       | Text          | Kategorie dat diagnostiky nabízených do protokolů Azure Monitor – AddonAzureBackupProtectedInstance |
| BackupItemUniqueId             | Text          | Jedinečné ID zálohované položky                                 |
| BackupManagementServerUniqueId | Text          | Pole k jednoznačné identifikaci záložního serveru pro správu, ve kterém je chráněná položka, pokud je k dispozici |
| BackupManagementType           | Text          | Typ poskytovatele pro server, který provádí úlohu zálohování, například IaaSVM, složka adresáře |
| ProtectedContainerUniqueId     | Text          | Jedinečné ID pro identifikaci chráněného kontejneru, na kterém je úloha spuštěná |
| ProtectedInstanceCount         | Text          | Počet chráněných instancí pro přidruženou zálohovanou položku nebo chráněný kontejner v dané datum a čas |
| SchemaVersion                  | Text          | Aktuální verze schématu, například **v2**            |
| Stav                          | Text          | Stav objektu zálohované položky, například Active, Deleted |
| VaultUniqueId                  | Text          | Jedinečný identifikátor chráněného trezoru přidruženého k chráněné instanci |
| SourceSystem                   | Text          | Zdrojový systém aktuálních dat – Azure                    |

## <a name="addonazurebackupjobs"></a>AddonAzureBackupJobs

Tato tabulka poskytuje podrobnosti o polích souvisejících s úlohou.

| **Pole**                      | **Typ dat** | **Popis**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | Identifikátor prostředku pro shromažďovaná data Například ID prostředku úložiště Recovery Services |
| OperationName                  | Text          | Toto pole představuje název aktuální operace – úloha    |
| Kategorie                       | Text          | Toto pole představuje kategorii dat diagnostiky nabízených do Azure Monitor protokolů – AddonAzureBackupJobs |
| AdhocOrScheduledJob            | Text          | Pole, které určuje, jestli má být úloha ad hoc nebo plánovaná           |
| BackupItemUniqueId             | Text          | Jedinečné ID, které slouží k identifikaci zálohované položky související s entitou úložiště |
| BackupManagementServerUniqueId | Text          | Jedinečné ID, které slouží k identifikaci management server zálohy související s entitou úložiště |
| BackupManagementType           | Text          | Typ poskytovatele pro provádění zálohování, například IaaSVM, složku, do které tato úloha patří |
| DataTransferredInMB            | Číslo        | Data přenesená v MB pro tuto úlohu                          |
| JobDurationInSecs              | Číslo        | Celková doba trvání úlohy v sekundách                                |
| JobFailureCode                 | Text          | Řetězec kódu chyby, protože došlo k selhání úlohy    |
| JobOperation                   | Text          | Operace, pro kterou je úloha spuštěná, například zálohování, obnovení, konfigurace zálohování |
| JobOperationSubType            | Text          | Dílčí typ operace úlohy Například ' log ' v případě úlohy zálohování protokolu |
| JobStartDateTime               | DateTime      | Datum a čas spuštění úlohy                       |
| JobStatus                      | Text          | Stav dokončené úlohy, například dokončeno, neúspěšné   |
| JobUniqueId                    | Text          | Jedinečné ID pro identifikaci úlohy                                |
| ProtectedContainerUniqueId     | Text          | Jedinečný identifikátor chráněného serveru přidruženého k úloze |
| RecoveryJobDestination         | Text          | Cíl úlohy obnovení, kde se data obnovují   |
| RecoveryJobRPDateTime          | DateTime      | Datum a čas vytvoření bodu obnovení, který se má obnovit |
| RecoveryJobLocation            | Text          | Umístění, kam se uložil bod obnovení, který se má obnovit |
| RecoveryLocationType           | Text          | Typ umístění pro obnovení                                |
| SchemaVersion                  | Text          | Aktuální verze schématu, například **v2**            |
| Stav                          | Text          | Aktuální stav objektu úlohy, například aktivní, odstraněn |
| VaultUniqueId                  | Text          | Jedinečný identifikátor chráněného trezoru přidruženého k úloze |
| SourceSystem                   | Text          | Zdrojový systém aktuálních dat – Azure                    |

## <a name="addonazurebackuppolicy"></a>AddonAzureBackupPolicy

Tato tabulka poskytuje podrobnosti o polích souvisejících s zásadami.

| **Pole**                       | **Typ dat**  | **Popis**                                              |
| ------------------------------- | -------------- | ------------------------------------------------------------ |
| ResourceId                      | Text           | Jedinečný identifikátor prostředku, pro který se shromažďují data Například ID prostředku úložiště Recovery Services |
| OperationName                   | Text           | Název operace, například zásada nebo PolicyAssociation |
| Kategorie                        | Text           | Kategorie dat diagnostiky nabízených do protokolů Azure Monitor – AddonAzureBackupPolicy |
| BackupDaysOfTheWeek             | Text           | Dny v týdnu, kdy bylo naplánováno zálohování            |
| BackupFrequency                 | Text           | Frekvence, se kterou se spouští zálohy. Například denně, týdně |
| BackupManagementType            | Text           | Typ poskytovatele pro server, který provádí úlohu zálohování. Například IaaSVM, složka. |
| BackupManagementServerUniqueId  | Text           | Pole k jednoznačné identifikaci záložního serveru pro správu, ve kterém je chráněná položka, pokud je k dispozici |
| BackupTimes                     | Text           | Datum a čas plánování zálohování                     |
| DailyRetentionDuration          | Celé číslo   | Celková doba uchování ve dnech pro nakonfigurované zálohy      |
| DailyRetentionTimes             | Text           | Datum a čas, kdy se nakonfigurovali denní uchovávání            |
| DiffBackupDaysOfTheWeek         | Text           | Dny v týdnu pro rozdílové zálohy pro SQL v zálohování virtuálních počítačů Azure |
| DiffBackupFormat                | Text           | Formát pro rozdílové zálohování SQL v zálohování virtuálních počítačů Azure   |
| DiffBackupRetentionDuration     | Desetinné číslo | Doba uchování rozdílového zálohování pro SQL v zálohování virtuálních počítačů Azure |
| DiffBackupTime                  | Čas           | Čas rozdílového zálohování SQL v zálohování virtuálních počítačů Azure     |
| LogBackupFrequency              | Desetinné číslo | Frekvence zálohování protokolů pro SQL                            |
| LogBackupRetentionDuration      | Desetinné číslo | Doba uchování pro zálohování protokolů pro SQL v zálohování virtuálních počítačů Azure |
| MonthlyRetentionDaysOfTheMonth  | Text           | Týdny v měsíci, kdy je nastaveno měsíční uchovávání.  Například First, Last atd. |
| MonthlyRetentionDaysOfTheWeek   | Text           | Dny v týdnu vybrané pro měsíční uchování              |
| MonthlyRetentionDuration        | Text           | Celková doba uchování v měsících pro nakonfigurované zálohy    |
| MonthlyRetentionFormat          | Text           | Typ konfigurace pro měsíční uchování. Například denně pro každý den v týdnu na základě týdne |
| MonthlyRetentionTimes           | Text           | Datum a čas, kdy se nakonfiguruje měsíční uchování           |
| MonthlyRetentionWeeksOfTheMonth | Text           | Týdny v měsíci, kdy je nastaveno měsíční uchovávání.   Například First, Last atd. |
| PolicyName                      | Text           | Název definované zásady                                   |
| PolicyUniqueId                  | Text           | Jedinečné ID pro identifikaci zásad                             |
| PolicyTimeZone                  | Text           | Časové pásmo, ve kterém se v protokolech mají pole s časem zásad zadávat |
| RetentionDuration               | Text           | Doba uchování pro nakonfigurované zálohy                    |
| RetentionType                   | Text           | Typ uchování                                            |
| SchemaVersion                   | Text           | Toto pole označuje aktuální verzi schématu. je **v2** |
| Stav                           | Text           | Aktuální stav objektu zásad Například aktivní, odstraněno |
| SynchronisationFrequencyPerDay  | Celé číslo   | Počet pokusů za den, kdy je záloha souborů synchronizovaná pro SC DPM a MABS |
| VaultUniqueId                   | Text           | Jedinečné ID trezoru, ke kterému patří tato zásada          |
| WeeklyRetentionDaysOfTheWeek    | Text           | Dny v týdnu vybrané pro týdenní uchování               |
| WeeklyRetentionDuration         | Desetinné číslo | Celková doba uchování v týdnu pro nakonfigurované zálohy v týdnech |
| WeeklyRetentionTimes            | Text           | Datum a čas, kdy je nastaveno týdenní uchovávání            |
| YearlyRetentionDaysOfTheMonth   | Text           | Data v měsíci vybraná pro roční uchování             |
| YearlyRetentionDaysOfTheWeek    | Text           | Dny v týdnu vybrané pro roční uchování               |
| YearlyRetentionDuration         | Desetinné číslo | Celková doba uchování v letech pro nakonfigurovaná zálohování     |
| YearlyRetentionFormat           | Text           | Typ konfigurace pro roční uchování, například denně pro každý den, týdně na základě týdne |
| YearlyRetentionMonthsOfTheYear  | Text           | Měsíců v roce vybraných pro roční uchování             |
| YearlyRetentionTimes            | Text           | Datum a čas, kdy se nakonfiguruje roční uchování            |
| YearlyRetentionWeeksOfTheMonth  | Text           | Týdnů v měsíci vybraném pro roční uchování             |
| SourceSystem                    | Text           | Zdrojový systém aktuálních dat – Azure                    |

## <a name="addonazurebackupstorage"></a>AddonAzureBackupStorage

Tato tabulka poskytuje podrobnosti o polích souvisejících s úložištěm.

| **Pole**                      | **Typ dat** | **Popis**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | Identifikátor prostředku pro shromažďovaná data Například ID prostředku úložiště Recovery Services |
| OperationName                  | Text          | Toto pole představuje název aktuální operace – Storage nebo StorageAssociation. |
| Kategorie                       | Text          | Toto pole představuje kategorii dat diagnostiky nabízených do Azure Monitor protokolů – AddonAzureBackupStorage |
| BackupItemUniqueId             | Text          | Jedinečné ID, které slouží k identifikaci zálohované položky pro virtuální počítače, které se zálohují pomocí DPM, MABS |
| BackupManagementServerUniqueId | Text          | Pole k jednoznačné identifikaci záložního serveru pro správu, ve kterém je chráněná položka, pokud je k dispozici |
| BackupManagementType           | Text          | Typ poskytovatele pro server, který provádí úlohu zálohování. Například IaaSVM, složka. |
| PreferredWorkloadOnVolume      | Text          | Úloha, pro kterou je tento svazek upřednostňovaným úložištěm      |
| ProtectedContainerUniqueId     | Text          | Jedinečný identifikátor chráněného kontejneru přidruženého k zálohovaným položkám |
| SchemaVersion                  | Text          | Verze schématu Například **v2**                   |
| Stav                          | Text          | Stav objektu zálohované položky Například aktivní, odstraněno |
| StorageAllocatedInMBs          | Číslo        | Velikost úložiště přidělená odpovídající zálohovanou položkou v odpovídajícím úložišti typu disk |
| StorageConsumedInMBs           | Číslo        | Velikost úložiště spotřebovaného odpovídající zálohovanou položkou v příslušném úložišti |
| StorageName                    | Text          | Název entity úložiště Například jednotku e:\                      |
| StorageTotalSizeInGBs          | Text          | Celková velikost úložiště v GB, spotřebovaná entitou úložiště     |
| StorageType                    | Text          | Typ úložiště, například Cloud, svazek, disk             |
| StorageUniqueId                | Text          | Jedinečné ID použité k identifikaci entity úložiště                |
| VaultUniqueId                  | Text          | Jedinečné ID, které slouží k identifikaci trezoru souvisejícího s entitou úložiště |
| VolumeFriendlyName             | Text          | Popisný název svazku úložiště                          |
| SourceSystem                   | Text          | Zdrojový systém aktuálních dat – Azure                    |

## <a name="next-steps"></a>Další kroky

- [Naučte se odesílat diagnostická data do Log Analytics](./backup-azure-diagnostic-events.md)
- [Naučte se zapisovat dotazy do tabulek specifických pro prostředky.](./backup-azure-monitoring-use-azuremonitor.md#sample-kusto-queries)
