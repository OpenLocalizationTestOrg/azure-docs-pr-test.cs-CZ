---
title: "aaaGetting začít s úlohy elastické databáze | Microsoft Docs"
description: "jak toouse úlohy elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 2540de0e-2235-4cdd-9b6a-b841adba00e5
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: bc5894d2df4235738ab961db4f69c11cdf786cc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a>Začínáme s úlohami elastické databáze
Úlohy elastické databáze (preview) pro Azure SQL Database vám umožní tooreliability spouštění skriptů T-SQL, které jsou rozmístěny v několika databází při automatickým opakovaným pokusem o a poskytování případné dokončení zaručuje. Další informace o funkci úlohy elastické databáze hello, najdete v tématu hello [stránku Přehled funkce](sql-database-elastic-jobs-overview.md).

Toto téma rozšiřuje hello ukázka v nalezen [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md). Po dokončení bude: Zjistěte, jak toocreate a spravovat úlohy, které správu skupiny související databází. Není požadovaná toouse hello elastické škálování nástroje v pořadí tootake výhod hello výhod elastické úlohy.

## <a name="prerequisites"></a>Požadavky
Stažení a spuštění hello [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>Vytvoření mapy horizontálního oddílu manager pomocí hello ukázkové aplikace
Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do hello horizontálních oddílů. Pokud již máte horizontálních oddílů nastavit s horizontálně dělená data v nich, můžete přeskočit následující kroky hello a přesunout toohello další části.

1. Sestavení a spuštění hello **Začínáme s nástroje elastické databáze** ukázkové aplikace. Postupujte podle kroků hello až do kroku 7 v části hello [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Na konci hello tohoto kroku 7 zobrazí se hello následující příkazový řádek:

   ![příkazový řádek](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. V příkazovém okně hello, zadejte "1" a stiskněte klávesu **Enter**. To vytvoří hello horizontálního oddílu mapa správce a přidá serverové toohello horizontálních oddílů. Potom zadejte "3" a stiskněte klávesu **Enter**; čtyřikrát tuto akci zopakujte. Vloží řádky ukázková data ve vašem horizontálních oddílů.
3. Hello [portálu Azure](https://portal.azure.com) by měl zobrazit tři nové databáze:

   ![Visual Studio potvrzení](./media/sql-database-elastic-query-getting-started/portal.png)

   Nyní vytvoříme vlastní databázi kolekce, která odráží všechny databáze hello v mapě hello horizontálního oddílu. To nám umožňují toocreate a spustit úlohu, která přidá novou tabulku napříč horizontálních oddílů.

Zde jsme by obvykle vytvořit cíl mapy horizontálního oddílu, pomocí hello **New-AzureSqlJobTarget** rutiny. Hello horizontálního oddílu mapa správce databáze musí být nastavena jako cíl databáze a pak je hello konkrétní horizontálních mapy zadaný jako cíl. Místo toho jsou všechny hello databází na serveru hello probíhající tooenumerate a přidání hello databáze toohello nové vlastní kolekce s výjimkou hello hlavní databáze.

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a>Vytvoří vlastní kolekce a přidejte všechny databáze v hello serveru toohello vlastní kolekce cílové s výjimkou hello hlavního serveru.
   ```
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in hello custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Vytvoření skriptu T-SQL pro provedení mezi databází
   ```
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a>Vytvořit úlohu tooexecute hello skript pro hello vlastní skupinu databází

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a>Spuštění úlohy hello
Hello následující skript prostředí PowerShell může být použité tooexecute stávající úloze:

Aktualizace hello proměnné tooreflect hello potřeby úlohy název toohave provést následující:

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a>Načíst stav hello provádění jedné úlohy
Použití hello stejné **Get-AzureSqlJobExecution** rutiny s hello **metoda IncludeChildren** parametr tooview hello stav spuštěních podřízené úlohy, a to hello určitý stav pro každé spuštění úlohy proti jednotlivým databáze cílové úlohou hello.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a>Zobrazení stavu hello mezi jednotlivými spuštěními více úloh
Hello **Get-AzureSqlJobExecution** rutina má více volitelné parametry, které se dají použít toodisplay více spuštěních úloh filtrované prostřednictvím hello zadané parametry. Následující Hello ukazuje některé možné způsoby, jak toouse hello Get-AzureSqlJobExecution:

Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:

   ```
    Get-AzureSqlJobExecution
   ```

Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Načtení seznamu hello spuštěních úloh úlohy v rámci provedení určité úlohy:

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Načtěte podrobnosti o provádění úkolů úlohy:

Následující skript prostředí PowerShell Hello lze použít tooview hello podrobnosti o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Načtení selhání v rámci úlohy spuštěních úloh
objekt JobTaskExecution Hello zahrnuje vlastnost hello životní cyklus úlohy hello společně s vlastností zpráv. Pokud se nezdařilo provádění úloh úkolu, příliš nastaví hello životního cyklu vlastnost*se nezdařilo* a hello vlastnosti zprávy se nastaví toohello výsledné zpráva o výjimce a jeho zásobníku. Pokud úloha nebyla úspěšná, je důležité tooview hello podrobnosti úlohy, které se nezdařilo pro danou úlohu.

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-toocomplete"></a>Čekání toocomplete spuštění úlohy
Hello následující skript prostředí PowerShell může být použité toowait pro toocomplete úlohy úlohy:

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Vytvořit zásadu vlastní spuštění
Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.

Zásady spouštění aktuálně povolit pro definování:

* Název: Identifikátor pro zásady spouštění hello.
* Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.
* Počáteční Interval opakování: Interval toowait před první opakování.
* Maximální Interval opakování: Cap z toouse intervalech zkuste to znovu.
* Koeficient omezení rychlosti Interval opakování: Koeficient použít toocalculate hello další interval mezi opakovanými pokusy.  Hello použije následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2).
* Maximální počet pokusů: hello maximální počet opakování pokusů o tooperform v rámci úlohy.

Zásady spouštění výchozí Hello používá hello následující hodnoty:

* Název: Zásady spouštění výchozí
* Časový limit úlohy: 1 týden
* Počáteční Interval opakování: 100 milisekund
* Maximální Interval opakování: 30 minut
* Opakujte koeficient Interval: 2
* Maximální počet pokusů: 2 147 483 647

Vytvoření zásady spouštění hello potřeby:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Aktualizovat zásady vlastní spuštění
Aktualizace hello potřeby tooupdate zásad spouštění:

   ```
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Zrušení úlohy
Elastické databáze úlohy podporuje požadavků na zrušení úlohy.  Pokud úlohy elastické databáze zjistí žádost o zrušení úlohy se spouští, se pokusí toostop hello úlohy.

Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:

1. Zrušení aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci hello aktuálně spuštěných aspekt úlohy hello.  Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude dotaz pokus o toocancel hello.
2. Ruší opakování úkolů: Pokud se předtím, než se spustí úloha pro spuštění zrušení detekuje hello řízení vlákno, hello řízení přístup z více vláken bude vyhnout spouštění úloh hello a deklarovat hello žádost jako zrušená.

Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude požadavek na zrušení hello dodržet pro hello nadřazené úloze a všechny jeho podřízené úlohy.

toosubmit žádost o zrušení použít hello **Stop-AzureSqlJobExecution** rutiny a sadu hello **JobExecutionId** parametr.

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a>Odstranit úlohu podle názvu a historie úlohy hello
Elastické databáze úlohy podporuje asynchronní odstranění úloh. Úloha může být označený k odstranění a hello systému odstraní hello úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu hello. Hello systému nebude automaticky zrušit spuštěních aktivní úlohy.  

Stop-AzureSqlJobExecution místo toho musí být spuštěních vyvolaná toocancel aktivní úlohy.

Odstranění úlohy tootrigger, použijte hello **odebrat AzureSqlJob** rutiny a sadu hello **JobName** parametr.

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Vytvořit cíl vlastní databázi
Vlastní databázi cíle může být definován v úlohy elastické databáze, které se dají použít pro spuštění přímo nebo pro zahrnutí do skupiny vlastní databázi. Vzhledem k tomu **elastické fondy** nejsou přímo, ale podporovány prostřednictvím hello rozhraní API prostředí PowerShell, můžete jednoduše vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, který zahrnuje všechny hello databáze ve fondu hello.

Nastavte následující informace o databázi proměnné tooreflect hello potřeby hello:

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Vytvořit cíl kolekce vlastní databázi
Cíl vlastní databázi kolekce může být definovaný tooenable provádění napříč více definovaných databázových cílů. Po vytvoření skupiny databáze, databáze může být přidružené toohello vlastní kolekce cíl.

Nastavte hello následující konfigurace cílového proměnné tooreflect hello požadovanou vlastní kolekce:

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a>Přidání databáze tooa vlastní databázi kolekce cíle
Cíle databáze může být přidružen vlastní databázi kolekce cíle toocreate skupinu databází. Vždy, když se vytvoří úloha, která zaměřena na cíl, vlastní databázi kolekce, bude skupina přidružené toohello databází hello rozšířené tootarget v době spuštění hello.

Přidání požadovaného hello databáze tooa konkrétní vlastní kolekce:

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a>Zkontrolujte hello databází v rámci kolekce cíl vlastní databázi
Použití hello **Get-AzureSqlJobTarget** rutiny tooretrieve hello podřízené databází v rámci kolekce cíl vlastní databázi.

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a>Vytvořit úlohu tooexecute skript pro cílovou kolekci vlastní databázi
Použití hello **New-AzureSqlJob** rutiny toocreate úloh pro skupinu databází definované cílovou kolekci vlastní databázi. Úlohy elastické databáze bude rozšiřovat hello úlohy do více podřízených úloh každé příslušné databáze s tooa přidružený cílové kolekce hello vlastní databázi a zkontrolujte, zda je pro každou databázi hello skriptu. Znovu je důležité, aby skripty jsou odolné tooretries idempotent toobe.

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Shromažďování dat mezi databázemi
**Elastické databáze úlohy** podporuje provádění dotazu napříč skupinou databází a odešle hello výsledky tooa zadaná databáze na tabulku. Tabulka Hello můžete položit dotaz na po hello fakt toosee hello výsledků dotazu z každé databáze. To poskytuje tooexecute asynchronní mechanismus dotazu mezi mnoha databázemi. Selhání případech jako jedna z databází hello není dočasně k dispozici jsou automaticky zpracováván opakování.

zadané cílové tabulky Hello bude automaticky vytvořen, pokud ještě neexistuje, odpovídající schéma hello hello vrátila sadu výsledků. Pokud spuštění skriptu vrátí více sad výsledků dotazu, úlohy elastické databáze odešle hello první jeden toohello zadané cílové tabulky.

Následující skript prostředí PowerShell Hello lze použít tooexecute skript shromažďování své výsledky do zadané tabulky. Tento skript předpokládá, že byla vytvořena skriptu T-SQL, který vrací jednu výslednou sadu a kolekce cíl vlastní databáze byla vytvořena.

Nastavte hello následující skript hello potřeby tooreflect, přihlašovací údaje a provádění cíle:

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Vytvořte a spusťte úlohu pro scénáře kolekce dat
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Vytvoření plánu pro provádění úlohy pomocí aktivační události úlohy
Hello následující skript prostředí PowerShell se dá použít toocreate opakovaném plánu. Tento skript používá intervalu jednu minutu, ale nové AzureSqlJobSchedule také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry. Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.

Vytvoření nového plánu:
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a>Vytvořit úlohu provést podle časového plánu toohave aktivační události úlohy
Aktivační události úlohy může být definovaná toohave časového plánu podle tooa úlohu provést. Následující skript prostředí PowerShell Hello lze použít toocreate aktivační události úlohy.

Nastavení hello následující proměnné toocorrespond toohello požadované úlohy a plánování:

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a>Odeberte přidružení naplánované úlohy toostop z spouštění podle plánu
toodiscontinue nadále provádění úlohy prostřednictvím aktivační události úlohy, aktivační události úlohy hello lze odebrat.
Odebrání toostop aktivační události úlohy úloha spouštěna podle plánu tooa pomocí hello **odebrat AzureSqlJobTrigger** rutiny.

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a>Import tooExcel výsledky dotazu elastické databáze
 Můžete importovat hello výsledky ze souboru aplikace Excel tooan dotazu.

1. Spusťte aplikaci Excel 2013.
2. Přejděte toohello **Data** pásu karet.
3. Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.

   ![Importu pro aplikaci Excel z jiných zdrojů](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. V hello **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru hello. Pak klikněte na tlačítko **Další**.
5. V dialogovém okně hello **hello vyberte databázi, která obsahuje hello data, která chcete**, vyberte hello **ElasticDBQuery** databáze.
6. Vyberte hello **zákazníci** tabulky v zobrazení seznamu hello a klikněte na tlačítko **Další**. Pak klikněte na tlačítko **Dokončit**.
7. V hello **importovat Data** formuláři v části **vyberte požadovaný způsob tooview tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.

Všechny řádky z hello **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit hello Excelovém listu.

## <a name="next-steps"></a>Další kroky
Teď můžete použít funkce dat v aplikaci Excel. Použít hello připojovací řetězec s názvem serveru, název databáze a pověření tooconnect vaše data a BI integrace nástrojů toohello elastické dotaz do databáze. Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje. Najdete toohello elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a zda byste připojili toowith vaše nástroje tabulek systému SQL Server.

### <a name="cost"></a>Náklady
Není k dispozici pro použití funkce dotazu hello elastické databáze bez dalších poplatků. V tuto chvíli tato funkce je dostupná pouze v databázích premium jako koncový bod, ale hello horizontálních oddílů lze vrstvy jakékoli služby.

Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
