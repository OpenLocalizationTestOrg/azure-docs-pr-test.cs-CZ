---
title: "aaaLog analýzy datového modelu pro zálohování Azure"
description: "V tomto článku bude zmíněn podrobnosti modelu dat analýzy protokolů pro data Azure Backup."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04ac16e38b896851f60b1c4ffbea4343347ae32c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Model dat analýzy protokolů pro zálohování Azure data
Tento článek popisuje hello datový model používá pro generování sestav dat tooLog Analytics vkládání. Tento datový model můžete vytvářet vlastní dotazy, řídicí panely a využívat v OMS. 

## <a name="using-azure-backup-data-model"></a>Pomocí Azure Backup datový model
Můžete použít následující pole, které jsou k dispozici jako součást hello data modelu toocreate vizuály, vlastních dotazů a řídicí panel podle vašich požadavků hello.

### <a name="alert"></a>Výstrahy
Tato tabulka obsahuje podrobnosti o výstrahy související pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| AlertUniqueId_s |Text |Jedinečné Id hello generované výstrahy |
| AlertType_s |Text |Typ hello vygeneruje výstrahu, například zálohování |
| AlertStatus_s |Text |Stav hello výstrahy, například aktivní |
| AlertOccurenceDateTime_s |Datum a čas |Datum a čas vytvoření výstrahy |
| AlertSeverity_s |Text |Závažnost hello výstrahy, například kritický |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Jedinečné Id toowhich hello zálohování položek, ke které patří tato výstraha příliš|
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello výstrah, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, ke které patří tato výstraha příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello – výstraha |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněné toowhich, ke které patří tato výstraha příliš|
| VaultUniqueId_s |Text |Jedinečné Id hello chráněné toowhich, ke které patří tato výstraha příliš|
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="backupitem"></a>BackupItem
Tato tabulka obsahuje podrobné informace o zálohování související s položkou pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Jedinečné Id položky zálohování hello |
| BackupItemId_s |Text |ID položky zálohování |
| BackupItemName_s |Text |Název položky zálohování |
| BackupItemFriendlyName_s |Text |Popisný název položky zálohování |
| BackupItemType_s |Text |Typ zálohování položky, například virtuální počítač, FileFolder |
| ProtectedServerName_s |Text |Název položky zálohování toowhich chráněného serveru patří příliš|
| ProtectionState_s |Text |Aktuální stav ochrany hello zálohování položky, například chráněné, ProtectionStopped |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello zálohování položek, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, ke které patří tato zálohování položka příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - BackupItem |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="backupitemassociation"></a>BackupItemAssociation
Tato tabulka obsahuje podrobné informace o přidružení položky zálohování s různými entitami.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Jedinečné Id položky zálohování hello |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello zálohování položek přidružení, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, ke které patří tato zálohování položka příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - BackupItemAssociation |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| PolicyUniqueId_g |Text |Jedinečné Id tooidentify hello zásady, která zálohování položka souvisí příliš|
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněné toowhich serveru, ke které patří tato zálohování položka příliš|
| VaultUniqueId_s |Text |Jedinečné Id toowhich hello trezoru, ke které patří tato zálohování položka příliš|
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="job"></a>Úloha
Tato tabulka obsahuje podrobnosti o pole související úlohy.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Jedinečné Id toowhich zálohování položek hello, ke které patří tato úloha příliš|
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello úlohy, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, ke které patří tato úloha příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello – úloha |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněné toowhich, ke které patří tato úloha příliš|
| VaultUniqueId_s |Text |Jedinečné Id hello chráněné toowhich, ke které patří tato úloha příliš|
| JobOperation_s |Text |Operaci, pro kterou úlohy je třeba spustit zálohování, obnovení, nakonfigurujte zálohování |
| JobStatus_s |Text |Stav hello dokončení úlohy, například dokončeno, se nezdařilo |
| JobFailureCode_s |Text |Selhání kódu řetězec kvůli které došlo k selhání úlohy |
| JobStartDateTime_s |Datum a čas |Datum a čas, kdy úloha spustila spuštěná |
| BackupStorageDestination_s |Text |Cílové úložiště záloh, například cloudu, disku  |
| JobDurationInSecs_s | Číslo |Celkový počet úloh doby v sekundách |
| DataTransferredInMB_s | Číslo |Data přenesená v MB pro tuto úlohu|
| JobUniqueId_g |Text |Jedinečné Id tooidentify hello úlohy |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="policy"></a>Zásada
Tato tabulka obsahuje podrobnosti o pole související se zásadami.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello zásad, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, které tyto zásady patří příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - zásad |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| PolicyUniqueId_g |Text |Jedinečné Id tooidentify hello zásady |
| PolicyName_s |Text |Název definované zásady hello |
| BackupFrequency_s |Text |Frekvence, se kterým se spustit zálohování, například denně, týdně |
| BackupTimes_s |Text |Datum a čas, kdy jsou naplánované zálohy |
| BackupDaysOfTheWeek_s |Text |Počet dnů v týdnu hello mít byla naplánovaného zálohování |
| RetentionDuration_s |Celé číslo |Doba uchování pro nastavená zálohování |
| DailyRetentionDuration_s |Celé číslo |Doba celkový uchování ve dnech pro nastavená zálohování |
| DailyRetentionTimes_s |Text |Datum a čas, kdy byl nakonfigurován denní uchování |
| WeeklyRetentionDuration_s |Desetinné číslo |Celková doba uchování týdenní týdnů pro nastavená zálohování |
| WeeklyRetentionTimes_s |Text |Datum a čas, když je nakonfigurovaný týdenního uchovávání |
| WeeklyRetentionDaysOfTheWeek_s |Text |Počet dnů v týdnu hello vybrané pro týdenní uchování |
| MonthlyRetentionDuration_s |Desetinné číslo |Doba uchování celkový počet měsíců pro nastavená zálohování |
| MonthlyRetentionTimes_s |Text |Datum a čas, když je nakonfigurovaný měsíční uchování |
| MonthlyRetentionFormat_s |Text |Typ konfigurace pro měsíční uchování, například denní, den, každý týden základě za týden na základě |
| MonthlyRetentionDaysOfTheWeek_s |Text |Počet dnů v týdnu hello vybrané pro měsíční uchování |
| MonthlyRetentionWeeksOfTheMonth_s |Text |Týdny hello měsíce, když je nakonfigurovaná měsíční uchovávání informací, například první, poslední atd. |
| YearlyRetentionDuration_s |Desetinné číslo |Doba uchování celkový počet roků pro nastavená zálohování |
| YearlyRetentionTimes_s |Text |Datum a čas, když je nakonfigurovaný roční uchování |
| YearlyRetentionMonthsOfTheYear_s |Text |Měsíce roku hello vybrané pro roční uchování |
| YearlyRetentionFormat_s |Text |Typ konfigurace pro roční uchování, například denní, den, každý týden základě za týden na základě |
| YearlyRetentionDaysOfTheMonth_s |Text |Data vybraná pro roční uchování měsíci hello |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="policyassociation"></a>PolicyAssociation
Tato tabulka obsahuje podrobné informace o přidružení zásady s různými entitami.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello zásad, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování například IaaSVM, toowhich FileFolder, které tyto zásady patří příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - PolicyAssociation |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| PolicyUniqueId_g |Text |Jedinečné Id tooidentify hello zásady |
| VaultUniqueId_s |Text |Jedinečné Id toowhich hello trezoru, které tyto zásady patří příliš|
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="protectedserver"></a>ProtectedServer
Tato tabulka obsahuje podrobnosti o chráněný server související pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| ProtectedServerName_s |Text |Název chráněného serveru |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav hello chráněný objekt serveru, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování například IaaSVM, toowhich FileFolder, které tento chráněný server náleží příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - ProtectedServer |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněném serveru |
| RegisteredContainerId_s |Text |ID kontejneru zaregistrované pro zálohování |
| ProtectedServerType_s |Text |Typ chráněného serveru zálohovat například systému Windows |
| ProtectedServerFriendlyName_s |Text |Popisný název chráněného serveru |
| AzureBackupAgentVersion_s |Text |Číslo verze agenta zálohování |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Tato tabulka obsahuje podrobné informace o přidružení chráněném serveru s jinými entitami.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav hello chráněný objekt přidružení serveru, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, které tento chráněný server náleží příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello - ProtectedServerAssociation |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněném serveru |
| VaultUniqueId_s |Text |Jedinečné Id toowhich hello trezoru, které tento chráněný server náleží příliš|
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="storage"></a>Úložiště
Tato tabulka obsahuje podrobnosti o pole související s úložištěm.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| CloudStorageInBytes_s |Desetinné číslo |Cloudové zálohování úložiště používané zálohování, počítá na základě nejnovější hodnoty |
| ProtectedInstances_s |Desetinné číslo |Počet instancí chráněné sloužící k výpočtu front-endu úložiště v fakturace, počítaný na základě nejnovější hodnotu. |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu úložiště hello, například aktivní, odstraněno |
| BackupManagementType_s |Text |Typ zprostředkovatele pro provádění zálohování, například IaaSVM toowhich FileFolder, které toto úložiště patří příliš|
| OperationName |Text |Toto pole představuje název aktuální operace hello – úložiště |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| ProtectedServerUniqueId_s |Text |Jedinečné Id hello chráněný server, pro které je vypočtena úložiště |
| VaultUniqueId_s |Text |Jedinečné Id hello úložiště pro úložiště se počítá. |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole representse typ prostředku hello, pro které se shromažďují data - trezory |

### <a name="vault"></a>Trezor
Tato tabulka obsahuje podrobné informace o trezoru související pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| EventName_s |Text |Toto pole představuje název této události, je vždy AzureBackupCentralReport |
| SchemaVersion_s |Text |Toto pole označuje aktuální verze schématu hello, je **V1** |
| State_s |Text |Aktuální stav objektu hello trezoru, například aktivní, odstraněno |
| OperationName |Text |Toto pole představuje název aktuální operace hello - trezoru |
| Kategorie |Text |Toto pole představuje kategorii dat diagnostiky nabídnutých tooLog analýzy, je AzureBackupReport |
| Prostředek |Text |Toto je hello prostředků, pro které se shromažďují data, se zobrazí název trezoru služeb zotavení |
| VaultUniqueId_s |Text |Jedinečné Id trezoru hello |
| VaultName_s |Text |Název úložiště hello |
| AzureDataCenter_s |Text |Datové centrum, kde se nachází úložiště |
| StorageReplicationType_s |Text |Typ replikace úložiště pro hello trezor, například GeoRedundant |
| SourceSystem |Text |Systém zdrojového hello aktuální dat – Azure |
| ID prostředku |Text |Toto pole představuje id prostředku pro kterou se data shromažďují, zobrazuje id prostředku trezoru služeb zotavení |
| SubscriptionId |Text |Toto pole představuje id předplatného prostředku hello (RS trezoru), pro které se shromažďují data |
| ResourceGroup |Text |Toto pole představuje skupinu prostředků hello prostředku (RS trezoru), pro které se shromažďují data |
| ResourceProvider |Text |Toto pole představuje hello poskytovatel prostředků pro které se shromažďují data - Microsoft.RecoveryServices |
| ResourceType |Text |Toto pole představuje typ prostředku hello, pro které se shromažďují data - trezory |

## <a name="next-steps"></a>Další kroky
Po prostudování hello datový model pro vytváření sestav Azure Backup, můžete spustit [vytváření řídicího panelu](../log-analytics/log-analytics-dashboards.md) analýzy protokolů a OMS.
