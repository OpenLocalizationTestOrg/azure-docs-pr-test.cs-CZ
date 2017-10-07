---
title: "model aaaData pro zálohování Azure"
description: "V tomto článku bude zmíněn podrobnosti modelu dat Power BI pro Azure Backup sestavy."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 0767c330-690d-474d-85a6-aa8ddc410bb2
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e3e7ca13c7a3f007c206bd56b8753166a2c264b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-model-for-azure-backup-reports"></a>Datový model pro sestavy Azure Backup
Tento článek popisuje hello Power BI datový model pro vytváření sestav Azure Backup. Tento datový model, můžete filtrovat pomocí stávajících sestav na základě příslušných polí a více pomocí tabulky a pole v modelu hello je důležité, vytvořte vlastní sestavy. 

## <a name="creating-new-reports-in-power-bi"></a>Vytvoření nové sestavy v Power BI
Přizpůsobení funkcí, pomocí kterého můžete poskytuje Power BI [vytváření sestav pomocí hello datový model](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Pomocí Azure Backup datový model
Můžete použít následující pole poskytovaná v rámci hello dat modelu sestavy toocreate a přizpůsobení stávajících sestav hello.

### <a name="alert"></a>Výstrahy
Tato tabulka obsahuje základní pole a agregace přes různé výstrahy související pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Celé číslo |Počet výstrah, které jsou vytvořené v vybrané časové období |
| % ActiveAlertsCreatedInPeriod |Procento |Procento aktivní výstrahy za vybrané časové období |
| % CriticalAlertsCreatedInPeriod |Procento |Procento kritické výstrahy za vybrané časové období |
| AlertOccurenceDate |Datum |Datum vytvoření výstrahy |
| AlertSeverity |Text |Závažnost výstrahy hello například kritický |
| AlertStatus |Text |Stav výstrahy hello například aktivní |
| AlertType |Text |Typ hello generuje upozornění, například zálohování |
| AlertUniqueId |Text |Jedinečné Id hello generované výstrahy |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Desetinné číslo |Průměrná doba (v minutách) tooresolve výstrahy pro vybrané časové období |
| EntityState |Text |Aktuální stav výstrahy objektu hello například aktivní, odstraněno |

### <a name="backup-item"></a>Zálohování položek
Tato tabulka poskytuje základní pole a agregace přes různé zálohování pole související s položkou.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #BackupItems |Celé číslo |Počet zálohování položek |
| #UnprotectedBackupItems |Celé číslo |Počet zálohování položek, které bylo zastaveno pro ochranu nebo nakonfigurovat pro zálohování, ale zálohování není spuštěna|
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| BackupItemFriendlyName |Text |Popisný název položky zálohování |
| BackupItemId |Text |ID položky zálohování |
| BackupItemName |Text |Název položky zálohování |
| BackupItemType |Text |Typ zálohování položky například virtuální počítač, FileFolder |
| EntityState |Text |Aktuální stav objektu hello zálohování položek například aktivní, odstraněno |
| LastBackupDateTime |Datum a čas |Čas poslední zálohy pro vybranou položku Zálohování |
| LastBackupState |Text |Stav poslední zálohy pro vybranou položku zálohování například bylo úspěšné, neúspěšné |
| LastSuccessfulBackupDateTime |Datum a čas |Čas poslední úspěšné zálohy pro vybranou položku Zálohování |
| ProtectionState |Text |Aktuální stav ochrany hello zálohování položky například chráněné, ProtectionStopped |

### <a name="calendar"></a>Kalendáře
Tato tabulka obsahuje podrobnosti o pole související s kalendáři.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| Datum |Datum |Data vybraná pro filtrování dat |
| DateKey |Text |Jedinečný klíč pro každou položku Datum |
| DayDiff |Desetinné číslo |Rozdíl v den pro filtrování dat, například, 0 znamená data aktuálního dne, -1 označuje předchozí jeden den na data, 0 a -1 označuje dat pro aktuální a předchozí den  |
| Měsíc |Text |Měsíc roku hello vybrané pro filtrování dat, začínajícího první den měsíce a končí 31 dní |
| MonthDate | Datum |Datum v měsíci hello při ukončení měsíce, vybraný pro filtrování dat |
| MonthDiff |Desetinné číslo |Rozdíl v měsíci pro filtrování dat, například, 0 znamená data aktuálního měsíce, -1 označuje data předchozího měsíce, 0 a -1 označuje dat pro aktuální a předchozí měsíc |
| Týden |Text |Týden vybraný pro filtrování dat, týden začíná v neděli a končí na sobotu |
| WeekDate |Datum |Datum v týdnu hello při ukončení týden, vybraný pro filtrování dat |
| WeekDiff |Desetinné číslo |Rozdíl v týdnu pro filtrování dat, například, 0 znamená data aktuálního týdne, -1 označuje předchozího týdne data, 0 a -1 označuje dat pro aktuální a předchozí týden |
| Rok |Text |Kalendářní rok vybrané pro filtrování dat |
| YearDate |Datum |Datum v roce hello při ukončení roku, vybraný pro filtrování dat |

### <a name="job"></a>Úloha
Tato tabulka poskytuje základní pole a agregace přes různá pole související úlohy.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #JobsCreatedInPeriod |Celé číslo |Počet úloh vytvořených v hello vybrané časové období |
| % FailuresForJobsCreatedInPeriod |Procento |Procento celkového úlohy selhání v hello vybrané časové období |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Desetinné číslo |80. hodnota percentilu dat přenesených v MB pro **zálohování** úlohy vytvořené v hello vybrané časové období |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Desetinné číslo |Průměrná doba v minutách pro **dokončené zálohování** úlohy vytvořené v vybrané časové období |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Desetinné číslo |Průměrná doba v minutách pro **bylo dokončeno obnovení** úlohy vytvořené v vybrané časové období |
| BackupStorageDestination |Text |Cíl zálohování úložiště cloudu, například Disk  |
| EntityState |Text |Aktuální stav objektu úlohy hello například aktivní, odstraněno |
| JobFailureCode |Text |Selhání kódu řetězec kvůli které došlo k selhání úlohy |
| JobOperation |Text |Operaci, pro kterou úlohy je třeba spustit zálohování, obnovení, nakonfigurujte zálohování |
| JobStartDate |Datum |Datum při spuštění úlohy |
| JobStartTime |Čas |Čas při spuštění úlohy |
| JobStatus |Text |Stav hello dokončil úkol například dokončen, se nezdařilo |
| JobUniqueId |Text |Jedinečné Id tooidentify hello úlohy |

### <a name="policy"></a>Zásada
Tato tabulka obsahuje základní pole a agregace přes různá pole související se zásadami.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #Policies |Celé číslo |Počet zásady zálohování, které existují v systému hello |
| #PoliciesInUse |Celé číslo |Počet zásad, které jsou právě používány pro konfiguraci zálohování |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| BackupDaysOfTheWeek |Text |Počet dnů v týdnu hello mít byla naplánovaného zálohování |
| BackupFrequency |Text |Frekvence, se kterým se spouštějí zálohování, například denně, týdně |
| BackupTimes |Text |Datum a čas, kdy jsou naplánované zálohy |
| DailyRetentionDuration |Celé číslo |Doba celkový uchování ve dnech pro nastavená zálohování |
| DailyRetentionTimes |Text |Datum a čas, kdy byl nakonfigurován denní uchování |
| EntityState |Text |Aktuální stav objektu zásad hello například aktivní, odstraněno |
| MonthlyRetentionDaysOfTheMonth |Text |Data vybraná pro měsíční uchování měsíci hello |
| MonthlyRetentionDaysOfTheWeek |Text |Počet dnů v týdnu hello vybrané pro měsíční uchování |
| MonthlyRetentionDuration |Desetinné číslo |Doba uchování celkový počet měsíců pro nastavená zálohování |
| MonthlyRetentionFormat |Text |Zadejte konfigurace pro měsíční uchování například denně za den, každý týden základě za týden na základě |
| MonthlyRetentionTimes |Text |Datum a čas, když je nakonfigurovaný měsíční uchování |
| MonthlyRetentionWeeksOfTheMonth |Text |Týdnech měsíce hello po měsíční uchování nakonfigurovat například First, Last atd. |
| PolicyName |Text |Název definované zásady hello |
| PolicyUniqueId |Text |Jedinečné Id tooidentify hello zásady |
| RetentionType |Text |Zadejte zásady uchovávání informací například denně, týdně, měsíčně, ročně |
| WeeklyRetentionDaysOfTheWeek |Text |Počet dnů v týdnu hello vybrané pro týdenní uchování |
| WeeklyRetentionDuration |Desetinné číslo |Celková doba uchování týdenní týdnů pro nastavená zálohování |
| WeeklyRetentionTimes |Text |Datum a čas, když je nakonfigurovaný týdenního uchovávání |
| YearlyRetentionDaysOfTheMonth |Text |Data vybraná pro roční uchování měsíci hello |
| YearlyRetentionDaysOfTheWeek |Text |Počet dnů v týdnu hello vybrané pro roční uchování |
| YearlyRetentionDuration |Desetinné číslo |Doba uchování celkový počet roků pro nastavená zálohování |
| YearlyRetentionFormat |Text |Zadejte konfigurace pro roční uchování například denně za den, každý týden základě za týden na základě |
| YearlyRetentionMonthsOfTheYear |Text |Měsíce roku hello vybrané pro roční uchování |
| YearlyRetentionTimes |Text |Datum a čas, když je nakonfigurovaný roční uchování |
| YearlyRetentionWeeksOfTheMonth |Text |Týdnech měsíce hello po roční uchování nakonfigurovat například First, Last atd. |

### <a name="protected-server"></a>Chráněného serveru
Tato tabulka poskytuje základní pole a agregace přes různé chráněná pole související serveru.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #ProtectedServers |Celé číslo |Počet chráněných serverů |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| AzureBackupAgentOSType |Text |Typ operačního systému, aplikace Azure Backup Agent |
| AzureBackupAgentOSVersion |Text |Verze operačního systému, aplikace Azure Backup Agent |
| AzureBackupAgentUpdateDate |Text |Datum aktualizace agenta agenta zálohování |
| AzureBackupAgentVersion |Text |Číslo verze agenta zálohování |
| BackupManagementType |Text |Typ zprostředkovatele pro provádění zálohování například IaaSVM FileFolder |
| EntityState |Text |Aktuální stav objektu chráněný server hello například aktivní, odstraněno |
| ProtectedServerFriendlyName |Text |Popisný název chráněného serveru |
| ProtectedServerName |Text |Název chráněného serveru |
| ProtectedServerType |Text |Typ chráněného serveru zálohovat například IaaSVMContainer |
| ProtectedServerName |Text |Název položky zálohování toowhich chráněného serveru patří |
| RegisteredContainerId |Text |ID kontejneru zaregistrované pro zálohování |

### <a name="storage"></a>Úložiště
Tato tabulka obsahuje základní pole a agregace přes různá pole související s úložištěm.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #ProtectedInstances |Desetinné číslo |Počet sloužící k výpočtu front-endu úložiště v fakturace, počítaný na základě nejnovější hodnotu ve vybraném časovém chráněné instance |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| CloudStorageInMB |Desetinné číslo |Cloudové zálohování úložiště používané zálohování, počítá na základě nejnovější hodnoty ve vybraném časovém |
| EntityState |Text |Aktuální stav objektu hello například aktivní, odstraněno |
| LastUpdatedDate |Datum |Datum poslední aktualizace vybraného řádku |

### <a name="time"></a>Čas
Tato tabulka obsahuje podrobné informace o souvisejících s časem pole.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| Hodina |Čas |Hodiny dne hello například 1:00:00 PM |
| HourNumber |Desetinné číslo |Číslo hodin za den hello například 13,00 |
| Minuta |Desetinné číslo |Minuta, hodina hello |
| PeriodOfTheDay |Text |Časový úsek období za den hello například AM 12 3 |
| Čas |Čas |Okamžik dne hello například 12:00:01: 00 |
| TimeKey |Text |Hodnota klíče toorepresent čas |

### <a name="vault"></a>Trezor
Tato tabulka nabízí v porovnání s různá pole související trezoru základní pole a agregace.

| Pole | Datový typ | Popis |
| --- | --- | --- |
| #Vaults |Celé číslo |Počet trezorů |
| AsOnDateTime |Datum a čas |Nejnovější aktualizace čas pro vybraný řádek hello |
| AzureDataCenter |Text |Datové centrum, kde se nachází úložiště |
| EntityState |Text |Aktuální stav objektu úložiště hello například aktivní, odstraněno |
| StorageReplicationType |Text |Typ replikace úložiště pro trezor hello například GeoRedundant |
| SubscriptionId |Text |Id předplatného zákazníka hello vybraného pro generování sestav |
| VaultName |Text |Název úložiště hello |
| VaultTags |Text |Značky přidružené toohello trezoru |

## <a name="next-steps"></a>Další kroky
Po prostudování hello datový model pro vytváření sestav Azure Backup, naleznete v nápovědě hello následující články pro další informace o vytváření a zobrazování sestav ve službě Power BI.

* [Vytváření sestav v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtrování sestavy v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
