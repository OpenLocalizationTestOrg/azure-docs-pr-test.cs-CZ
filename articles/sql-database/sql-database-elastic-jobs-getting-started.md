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
# <a name="getting-started-with-elastic-database-jobs"></a><span data-ttu-id="cd01d-103">Začínáme s úlohami elastické databáze</span><span class="sxs-lookup"><span data-stu-id="cd01d-103">Getting started with Elastic Database jobs</span></span>
<span data-ttu-id="cd01d-104">Úlohy elastické databáze (preview) pro Azure SQL Database vám umožní tooreliability spouštění skriptů T-SQL, které jsou rozmístěny v několika databází při automatickým opakovaným pokusem o a poskytování případné dokončení zaručuje.</span><span class="sxs-lookup"><span data-stu-id="cd01d-104">Elastic Database jobs (preview) for Azure SQL Database allows you tooreliability execute T-SQL scripts that span multiple databases while automatically retrying and providing eventual completion guarantees.</span></span> <span data-ttu-id="cd01d-105">Další informace o funkci úlohy elastické databáze hello, najdete v tématu hello [stránku Přehled funkce](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd01d-105">For more information about hello Elastic Database job feature, please see hello [feature overview page](sql-database-elastic-jobs-overview.md).</span></span>

<span data-ttu-id="cd01d-106">Toto téma rozšiřuje hello ukázka v nalezen [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cd01d-106">This topic extends hello sample found in [Getting started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span> <span data-ttu-id="cd01d-107">Po dokončení bude: Zjistěte, jak toocreate a spravovat úlohy, které správu skupiny související databází.</span><span class="sxs-lookup"><span data-stu-id="cd01d-107">When completed, you will: learn how toocreate and manage jobs that manage a group of related databases.</span></span> <span data-ttu-id="cd01d-108">Není požadovaná toouse hello elastické škálování nástroje v pořadí tootake výhod hello výhod elastické úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-108">It is not required toouse hello Elastic Scale tools in order tootake advantage of hello benefits of Elastic jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd01d-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd01d-109">Prerequisites</span></span>
<span data-ttu-id="cd01d-110">Stažení a spuštění hello [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cd01d-110">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="cd01d-111">Vytvoření mapy horizontálního oddílu manager pomocí hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="cd01d-111">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="cd01d-112">Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="cd01d-112">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="cd01d-113">Pokud již máte horizontálních oddílů nastavit s horizontálně dělená data v nich, můžete přeskočit následující kroky hello a přesunout toohello další části.</span><span class="sxs-lookup"><span data-stu-id="cd01d-113">If you already have shards set up with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="cd01d-114">Sestavení a spuštění hello **Začínáme s nástroje elastické databáze** ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd01d-114">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="cd01d-115">Postupujte podle kroků hello až do kroku 7 v části hello [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="cd01d-115">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="cd01d-116">Na konci hello tohoto kroku 7 zobrazí se hello následující příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="cd01d-116">At hello end of Step 7, you will see hello following command prompt:</span></span>

   ![příkazový řádek](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. <span data-ttu-id="cd01d-118">V příkazovém okně hello, zadejte "1" a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-118">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="cd01d-119">To vytvoří hello horizontálního oddílu mapa správce a přidá serverové toohello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="cd01d-119">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="cd01d-120">Potom zadejte "3" a stiskněte klávesu **Enter**; čtyřikrát tuto akci zopakujte.</span><span class="sxs-lookup"><span data-stu-id="cd01d-120">Then type "3" and press **Enter**; repeat this action four times.</span></span> <span data-ttu-id="cd01d-121">Vloží řádky ukázková data ve vašem horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="cd01d-121">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="cd01d-122">Hello [portálu Azure](https://portal.azure.com) by měl zobrazit tři nové databáze:</span><span class="sxs-lookup"><span data-stu-id="cd01d-122">hello [Azure Portal](https://portal.azure.com) should show three new databases:</span></span>

   ![Visual Studio potvrzení](./media/sql-database-elastic-query-getting-started/portal.png)

   <span data-ttu-id="cd01d-124">Nyní vytvoříme vlastní databázi kolekce, která odráží všechny databáze hello v mapě hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-124">At this point, we will create a custom database collection that reflects all hello databases in hello shard map.</span></span> <span data-ttu-id="cd01d-125">To nám umožňují toocreate a spustit úlohu, která přidá novou tabulku napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="cd01d-125">This will allow us toocreate and execute a job that add a new table across shards.</span></span>

<span data-ttu-id="cd01d-126">Zde jsme by obvykle vytvořit cíl mapy horizontálního oddílu, pomocí hello **New-AzureSqlJobTarget** rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd01d-126">Here we would usually create a shard map target, using hello **New-AzureSqlJobTarget** cmdlet.</span></span> <span data-ttu-id="cd01d-127">Hello horizontálního oddílu mapa správce databáze musí být nastavena jako cíl databáze a pak je hello konkrétní horizontálních mapy zadaný jako cíl.</span><span class="sxs-lookup"><span data-stu-id="cd01d-127">hello shard map manager database must be set as a database target and then hello specific shard map is specified as a target.</span></span> <span data-ttu-id="cd01d-128">Místo toho jsou všechny hello databází na serveru hello probíhající tooenumerate a přidání hello databáze toohello nové vlastní kolekce s výjimkou hello hlavní databáze.</span><span class="sxs-lookup"><span data-stu-id="cd01d-128">Instead, we are going tooenumerate all hello databases in hello server and add hello databases toohello new custom collection with hello exception of master database.</span></span>

## <a name="creates-a-custom-collection-and-add-all-databases-in-hello-server-toohello-custom-collection-target-with-hello-exception-of-master"></a><span data-ttu-id="cd01d-129">Vytvoří vlastní kolekce a přidejte všechny databáze v hello serveru toohello vlastní kolekce cílové s výjimkou hello hlavního serveru.</span><span class="sxs-lookup"><span data-stu-id="cd01d-129">Creates a custom collection and add all databases in hello server toohello custom collection target with hello exception of master.</span></span>
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
## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="cd01d-130">Vytvoření skriptu T-SQL pro provedení mezi databází</span><span class="sxs-lookup"><span data-stu-id="cd01d-130">Create a T-SQL Script for execution across databases</span></span>
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

## <a name="create-hello-job-tooexecute-a-script-across-hello-custom-group-of-databases"></a><span data-ttu-id="cd01d-131">Vytvořit úlohu tooexecute hello skript pro hello vlastní skupinu databází</span><span class="sxs-lookup"><span data-stu-id="cd01d-131">Create hello job tooexecute a script across hello custom group of databases</span></span>

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-hello-job"></a><span data-ttu-id="cd01d-132">Spuštění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="cd01d-132">Execute hello job</span></span>
<span data-ttu-id="cd01d-133">Hello následující skript prostředí PowerShell může být použité tooexecute stávající úloze:</span><span class="sxs-lookup"><span data-stu-id="cd01d-133">hello following PowerShell script can be used tooexecute an existing job:</span></span>

<span data-ttu-id="cd01d-134">Aktualizace hello proměnné tooreflect hello potřeby úlohy název toohave provést následující:</span><span class="sxs-lookup"><span data-stu-id="cd01d-134">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="cd01d-135">Načíst stav hello provádění jedné úlohy</span><span class="sxs-lookup"><span data-stu-id="cd01d-135">Retrieve hello state of a single job execution</span></span>
<span data-ttu-id="cd01d-136">Použití hello stejné **Get-AzureSqlJobExecution** rutiny s hello **metoda IncludeChildren** parametr tooview hello stav spuštěních podřízené úlohy, a to hello určitý stav pro každé spuštění úlohy proti jednotlivým databáze cílové úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-136">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-hello-state-across-multiple-job-executions"></a><span data-ttu-id="cd01d-137">Zobrazení stavu hello mezi jednotlivými spuštěními více úloh</span><span class="sxs-lookup"><span data-stu-id="cd01d-137">View hello state across multiple job executions</span></span>
<span data-ttu-id="cd01d-138">Hello **Get-AzureSqlJobExecution** rutina má více volitelné parametry, které se dají použít toodisplay více spuštěních úloh filtrované prostřednictvím hello zadané parametry.</span><span class="sxs-lookup"><span data-stu-id="cd01d-138">hello **Get-AzureSqlJobExecution** cmdlet has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="cd01d-139">Následující Hello ukazuje některé možné způsoby, jak toouse hello Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="cd01d-139">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="cd01d-140">Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:</span><span class="sxs-lookup"><span data-stu-id="cd01d-140">Retrieve all active top level job executions:</span></span>

   ```
    Get-AzureSqlJobExecution
   ```

<span data-ttu-id="cd01d-141">Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-141">Retrieve all top level job executions, including inactive job executions:</span></span>

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

<span data-ttu-id="cd01d-142">Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-142">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

<span data-ttu-id="cd01d-143">Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-143">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

<span data-ttu-id="cd01d-144">Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-144">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="cd01d-145">Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-145">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="cd01d-146">Načtení seznamu hello spuštěních úloh úlohy v rámci provedení určité úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-146">Retrieve hello list of job task executions within a specific job execution:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

<span data-ttu-id="cd01d-147">Načtěte podrobnosti o provádění úkolů úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-147">Retrieve job task execution details:</span></span>

<span data-ttu-id="cd01d-148">Následující skript prostředí PowerShell Hello lze použít tooview hello podrobnosti o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="cd01d-148">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a><span data-ttu-id="cd01d-149">Načtení selhání v rámci úlohy spuštěních úloh</span><span class="sxs-lookup"><span data-stu-id="cd01d-149">Retrieve failures within job task executions</span></span>
<span data-ttu-id="cd01d-150">objekt JobTaskExecution Hello zahrnuje vlastnost hello životní cyklus úlohy hello společně s vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="cd01d-150">hello JobTaskExecution object includes a property for hello Lifecycle of hello task along with a Message property.</span></span> <span data-ttu-id="cd01d-151">Pokud se nezdařilo provádění úloh úkolu, příliš nastaví hello životního cyklu vlastnost*se nezdařilo* a hello vlastnosti zprávy se nastaví toohello výsledné zpráva o výjimce a jeho zásobníku.</span><span class="sxs-lookup"><span data-stu-id="cd01d-151">If a job task execution failed, hello Lifecycle property will be set too*Failed* and hello Message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="cd01d-152">Pokud úloha nebyla úspěšná, je důležité tooview hello podrobnosti úlohy, které se nezdařilo pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-152">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

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

## <a name="waiting-for-a-job-execution-toocomplete"></a><span data-ttu-id="cd01d-153">Čekání toocomplete spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="cd01d-153">Waiting for a job execution toocomplete</span></span>
<span data-ttu-id="cd01d-154">Hello následující skript prostředí PowerShell může být použité toowait pro toocomplete úlohy úlohy:</span><span class="sxs-lookup"><span data-stu-id="cd01d-154">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="cd01d-155">Vytvořit zásadu vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="cd01d-155">Create a custom execution policy</span></span>
<span data-ttu-id="cd01d-156">Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="cd01d-156">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="cd01d-157">Zásady spouštění aktuálně povolit pro definování:</span><span class="sxs-lookup"><span data-stu-id="cd01d-157">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="cd01d-158">Název: Identifikátor pro zásady spouštění hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-158">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="cd01d-159">Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="cd01d-159">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="cd01d-160">Počáteční Interval opakování: Interval toowait před první opakování.</span><span class="sxs-lookup"><span data-stu-id="cd01d-160">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="cd01d-161">Maximální Interval opakování: Cap z toouse intervalech zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-161">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="cd01d-162">Koeficient omezení rychlosti Interval opakování: Koeficient použít toocalculate hello další interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-162">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="cd01d-163">Hello použije následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2).</span><span class="sxs-lookup"><span data-stu-id="cd01d-163">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span>
* <span data-ttu-id="cd01d-164">Maximální počet pokusů: hello maximální počet opakování pokusů o tooperform v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-164">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="cd01d-165">Zásady spouštění výchozí Hello používá hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cd01d-165">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="cd01d-166">Název: Zásady spouštění výchozí</span><span class="sxs-lookup"><span data-stu-id="cd01d-166">Name: Default execution policy</span></span>
* <span data-ttu-id="cd01d-167">Časový limit úlohy: 1 týden</span><span class="sxs-lookup"><span data-stu-id="cd01d-167">Job Timeout: 1 week</span></span>
* <span data-ttu-id="cd01d-168">Počáteční Interval opakování: 100 milisekund</span><span class="sxs-lookup"><span data-stu-id="cd01d-168">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="cd01d-169">Maximální Interval opakování: 30 minut</span><span class="sxs-lookup"><span data-stu-id="cd01d-169">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="cd01d-170">Opakujte koeficient Interval: 2</span><span class="sxs-lookup"><span data-stu-id="cd01d-170">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="cd01d-171">Maximální počet pokusů: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="cd01d-171">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="cd01d-172">Vytvoření zásady spouštění hello potřeby:</span><span class="sxs-lookup"><span data-stu-id="cd01d-172">Create hello desired execution policy:</span></span>

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

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="cd01d-173">Aktualizovat zásady vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="cd01d-173">Update a custom execution policy</span></span>
<span data-ttu-id="cd01d-174">Aktualizace hello potřeby tooupdate zásad spouštění:</span><span class="sxs-lookup"><span data-stu-id="cd01d-174">Update hello desired execution policy tooupdate:</span></span>

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

## <a name="cancel-a-job"></a><span data-ttu-id="cd01d-175">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="cd01d-175">Cancel a job</span></span>
<span data-ttu-id="cd01d-176">Elastické databáze úlohy podporuje požadavků na zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-176">Elastic Database Jobs supports jobs cancellation requests.</span></span>  <span data-ttu-id="cd01d-177">Pokud úlohy elastické databáze zjistí žádost o zrušení úlohy se spouští, se pokusí toostop hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-177">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="cd01d-178">Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="cd01d-178">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="cd01d-179">Zrušení aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci hello aktuálně spuštěných aspekt úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-179">Canceling Currently Executing Tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="cd01d-180">Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude dotaz pokus o toocancel hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-180">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="cd01d-181">Ruší opakování úkolů: Pokud se předtím, než se spustí úloha pro spuštění zrušení detekuje hello řízení vlákno, hello řízení přístup z více vláken bude vyhnout spouštění úloh hello a deklarovat hello žádost jako zrušená.</span><span class="sxs-lookup"><span data-stu-id="cd01d-181">Canceling Task Retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="cd01d-182">Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude požadavek na zrušení hello dodržet pro hello nadřazené úloze a všechny jeho podřízené úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-182">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="cd01d-183">toosubmit žádost o zrušení použít hello **Stop-AzureSqlJobExecution** rutiny a sadu hello **JobExecutionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="cd01d-183">toosubmit a cancellation request, use hello **Stop-AzureSqlJobExecution** cmdlet and set hello **JobExecutionId** parameter.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-hello-jobs-history"></a><span data-ttu-id="cd01d-184">Odstranit úlohu podle názvu a historie úlohy hello</span><span class="sxs-lookup"><span data-stu-id="cd01d-184">Delete a job by name and hello job's history</span></span>
<span data-ttu-id="cd01d-185">Elastické databáze úlohy podporuje asynchronní odstranění úloh.</span><span class="sxs-lookup"><span data-stu-id="cd01d-185">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="cd01d-186">Úloha může být označený k odstranění a hello systému odstraní hello úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-186">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="cd01d-187">Hello systému nebude automaticky zrušit spuštěních aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-187">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="cd01d-188">Stop-AzureSqlJobExecution místo toho musí být spuštěních vyvolaná toocancel aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-188">Instead, Stop-AzureSqlJobExecution must be invoked toocancel active job executions.</span></span>

<span data-ttu-id="cd01d-189">Odstranění úlohy tootrigger, použijte hello **odebrat AzureSqlJob** rutiny a sadu hello **JobName** parametr.</span><span class="sxs-lookup"><span data-stu-id="cd01d-189">tootrigger job deletion, use hello **Remove-AzureSqlJob** cmdlet and set hello **JobName** parameter.</span></span>

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a><span data-ttu-id="cd01d-190">Vytvořit cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="cd01d-190">Create a custom database target</span></span>
<span data-ttu-id="cd01d-191">Vlastní databázi cíle může být definován v úlohy elastické databáze, které se dají použít pro spuštění přímo nebo pro zahrnutí do skupiny vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="cd01d-191">Custom database targets can be defined in Elastic Database jobs which can be used either for execution directly or for inclusion within a custom database group.</span></span> <span data-ttu-id="cd01d-192">Vzhledem k tomu **elastické fondy** nejsou přímo, ale podporovány prostřednictvím hello rozhraní API prostředí PowerShell, můžete jednoduše vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, který zahrnuje všechny hello databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-192">Since **elastic pools** are not yet directly supported via hello PowerShell APIs, you simply create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="cd01d-193">Nastavte následující informace o databázi proměnné tooreflect hello potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="cd01d-193">Set hello following variables tooreflect hello desired database information:</span></span>

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a><span data-ttu-id="cd01d-194">Vytvořit cíl kolekce vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="cd01d-194">Create a custom database collection target</span></span>
<span data-ttu-id="cd01d-195">Cíl vlastní databázi kolekce může být definovaný tooenable provádění napříč více definovaných databázových cílů.</span><span class="sxs-lookup"><span data-stu-id="cd01d-195">A custom database collection target can be defined tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="cd01d-196">Po vytvoření skupiny databáze, databáze může být přidružené toohello vlastní kolekce cíl.</span><span class="sxs-lookup"><span data-stu-id="cd01d-196">After a database group is created, databases can be associated toohello custom collection target.</span></span>

<span data-ttu-id="cd01d-197">Nastavte hello následující konfigurace cílového proměnné tooreflect hello požadovanou vlastní kolekce:</span><span class="sxs-lookup"><span data-stu-id="cd01d-197">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="cd01d-198">Přidání databáze tooa vlastní databázi kolekce cíle</span><span class="sxs-lookup"><span data-stu-id="cd01d-198">Add databases tooa custom database collection target</span></span>
<span data-ttu-id="cd01d-199">Cíle databáze může být přidružen vlastní databázi kolekce cíle toocreate skupinu databází.</span><span class="sxs-lookup"><span data-stu-id="cd01d-199">Database targets can be associated with custom database collection targets toocreate a group of databases.</span></span> <span data-ttu-id="cd01d-200">Vždy, když se vytvoří úloha, která zaměřena na cíl, vlastní databázi kolekce, bude skupina přidružené toohello databází hello rozšířené tootarget v době spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-200">Whenever a job is created that targets a custom database collection target, it will be expanded tootarget hello databases associated toohello group at hello time of execution.</span></span>

<span data-ttu-id="cd01d-201">Přidání požadovaného hello databáze tooa konkrétní vlastní kolekce:</span><span class="sxs-lookup"><span data-stu-id="cd01d-201">Add hello desired database tooa specific custom collection:</span></span>

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="cd01d-202">Zkontrolujte hello databází v rámci kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="cd01d-202">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="cd01d-203">Použití hello **Get-AzureSqlJobTarget** rutiny tooretrieve hello podřízené databází v rámci kolekce cíl vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="cd01d-203">Use hello **Get-AzureSqlJobTarget** cmdlet tooretrieve hello child databases within a custom database collection target.</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="cd01d-204">Vytvořit úlohu tooexecute skript pro cílovou kolekci vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="cd01d-204">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="cd01d-205">Použití hello **New-AzureSqlJob** rutiny toocreate úloh pro skupinu databází definované cílovou kolekci vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="cd01d-205">Use hello **New-AzureSqlJob** cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="cd01d-206">Úlohy elastické databáze bude rozšiřovat hello úlohy do více podřízených úloh každé příslušné databáze s tooa přidružený cílové kolekce hello vlastní databázi a zkontrolujte, zda je pro každou databázi hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-206">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="cd01d-207">Znovu je důležité, aby skripty jsou odolné tooretries idempotent toobe.</span><span class="sxs-lookup"><span data-stu-id="cd01d-207">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a><span data-ttu-id="cd01d-208">Shromažďování dat mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="cd01d-208">Data collection across databases</span></span>
<span data-ttu-id="cd01d-209">**Elastické databáze úlohy** podporuje provádění dotazu napříč skupinou databází a odešle hello výsledky tooa zadaná databáze na tabulku.</span><span class="sxs-lookup"><span data-stu-id="cd01d-209">**Elastic Database jobs** supports executing a query across a group of databases and sends hello results tooa specified database’s table.</span></span> <span data-ttu-id="cd01d-210">Tabulka Hello můžete položit dotaz na po hello fakt toosee hello výsledků dotazu z každé databáze.</span><span class="sxs-lookup"><span data-stu-id="cd01d-210">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="cd01d-211">To poskytuje tooexecute asynchronní mechanismus dotazu mezi mnoha databázemi.</span><span class="sxs-lookup"><span data-stu-id="cd01d-211">This provides an asynchronous mechanism tooexecute a query across many databases.</span></span> <span data-ttu-id="cd01d-212">Selhání případech jako jedna z databází hello není dočasně k dispozici jsou automaticky zpracováván opakování.</span><span class="sxs-lookup"><span data-stu-id="cd01d-212">Failure cases such as one of hello databases being temporarily unavailable are handled automatically via retries.</span></span>

<span data-ttu-id="cd01d-213">zadané cílové tabulky Hello bude automaticky vytvořen, pokud ještě neexistuje, odpovídající schéma hello hello vrátila sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="cd01d-213">hello specified destination table will be automatically created if it does not yet exist, matching hello schema of hello returned result set.</span></span> <span data-ttu-id="cd01d-214">Pokud spuštění skriptu vrátí více sad výsledků dotazu, úlohy elastické databáze odešle hello první jeden toohello zadané cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="cd01d-214">If a script execution returns multiple result sets, Elastic Database jobs will only send hello first one toohello provided destination table.</span></span>

<span data-ttu-id="cd01d-215">Následující skript prostředí PowerShell Hello lze použít tooexecute skript shromažďování své výsledky do zadané tabulky.</span><span class="sxs-lookup"><span data-stu-id="cd01d-215">hello following PowerShell script can be used tooexecute a script collecting its results into a specified table.</span></span> <span data-ttu-id="cd01d-216">Tento skript předpokládá, že byla vytvořena skriptu T-SQL, který vrací jednu výslednou sadu a kolekce cíl vlastní databáze byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="cd01d-216">This script assumes that a T-SQL script has been created which outputs a single result set and a custom database collection target has been created.</span></span>

<span data-ttu-id="cd01d-217">Nastavte hello následující skript hello potřeby tooreflect, přihlašovací údaje a provádění cíle:</span><span class="sxs-lookup"><span data-stu-id="cd01d-217">Set hello following tooreflect hello desired script, credentials, and execution target:</span></span>

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="cd01d-218">Vytvořte a spusťte úlohu pro scénáře kolekce dat</span><span class="sxs-lookup"><span data-stu-id="cd01d-218">Create and start a job for data collection scenarios</span></span>
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a><span data-ttu-id="cd01d-219">Vytvoření plánu pro provádění úlohy pomocí aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="cd01d-219">Create a schedule for job execution using a job trigger</span></span>
<span data-ttu-id="cd01d-220">Hello následující skript prostředí PowerShell se dá použít toocreate opakovaném plánu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-220">hello following PowerShell script can be used toocreate a reoccurring schedule.</span></span> <span data-ttu-id="cd01d-221">Tento skript používá intervalu jednu minutu, ale nové AzureSqlJobSchedule také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry.</span><span class="sxs-lookup"><span data-stu-id="cd01d-221">This script uses a one minute interval, but New-AzureSqlJobSchedule also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="cd01d-222">Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.</span><span class="sxs-lookup"><span data-stu-id="cd01d-222">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="cd01d-223">Vytvoření nového plánu:</span><span class="sxs-lookup"><span data-stu-id="cd01d-223">Create a new schedule:</span></span>
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-toohave-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="cd01d-224">Vytvořit úlohu provést podle časového plánu toohave aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="cd01d-224">Create a job trigger toohave a job executed on a time schedule</span></span>
<span data-ttu-id="cd01d-225">Aktivační události úlohy může být definovaná toohave časového plánu podle tooa úlohu provést.</span><span class="sxs-lookup"><span data-stu-id="cd01d-225">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="cd01d-226">Následující skript prostředí PowerShell Hello lze použít toocreate aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd01d-226">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="cd01d-227">Nastavení hello následující proměnné toocorrespond toohello požadované úlohy a plánování:</span><span class="sxs-lookup"><span data-stu-id="cd01d-227">Set hello following variables toocorrespond toohello desired job and schedule:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="cd01d-228">Odeberte přidružení naplánované úlohy toostop z spouštění podle plánu</span><span class="sxs-lookup"><span data-stu-id="cd01d-228">Remove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="cd01d-229">toodiscontinue nadále provádění úlohy prostřednictvím aktivační události úlohy, aktivační události úlohy hello lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="cd01d-229">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span>
<span data-ttu-id="cd01d-230">Odebrání toostop aktivační události úlohy úloha spouštěna podle plánu tooa pomocí hello **odebrat AzureSqlJobTrigger** rutiny.</span><span class="sxs-lookup"><span data-stu-id="cd01d-230">Remove a job trigger toostop a job from being executed according tooa schedule using hello **Remove-AzureSqlJobTrigger** cmdlet.</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="cd01d-231">Import tooExcel výsledky dotazu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="cd01d-231">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="cd01d-232">Můžete importovat hello výsledky ze souboru aplikace Excel tooan dotazu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-232">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="cd01d-233">Spusťte aplikaci Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="cd01d-233">Launch Excel 2013.</span></span>
2. <span data-ttu-id="cd01d-234">Přejděte toohello **Data** pásu karet.</span><span class="sxs-lookup"><span data-stu-id="cd01d-234">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="cd01d-235">Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-235">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Importu pro aplikaci Excel z jiných zdrojů](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. <span data-ttu-id="cd01d-237">V hello **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru hello.</span><span class="sxs-lookup"><span data-stu-id="cd01d-237">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="cd01d-238">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-238">Then click **Next**.</span></span>
5. <span data-ttu-id="cd01d-239">V dialogovém okně hello **hello vyberte databázi, která obsahuje hello data, která chcete**, vyberte hello **ElasticDBQuery** databáze.</span><span class="sxs-lookup"><span data-stu-id="cd01d-239">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="cd01d-240">Vyberte hello **zákazníci** tabulky v zobrazení seznamu hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-240">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="cd01d-241">Pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-241">Then click **Finish**.</span></span>
7. <span data-ttu-id="cd01d-242">V hello **importovat Data** formuláři v části **vyberte požadovaný způsob tooview tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd01d-242">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="cd01d-243">Všechny řádky z hello **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit hello Excelovém listu.</span><span class="sxs-lookup"><span data-stu-id="cd01d-243">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd01d-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd01d-244">Next steps</span></span>
<span data-ttu-id="cd01d-245">Teď můžete použít funkce dat v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="cd01d-245">You can now use Excel’s data functions.</span></span> <span data-ttu-id="cd01d-246">Použít hello připojovací řetězec s názvem serveru, název databáze a pověření tooconnect vaše data a BI integrace nástrojů toohello elastické dotaz do databáze.</span><span class="sxs-lookup"><span data-stu-id="cd01d-246">Use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="cd01d-247">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="cd01d-247">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="cd01d-248">Najdete toohello elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a zda byste připojili toowith vaše nástroje tabulek systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cd01d-248">Refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="cd01d-249">Náklady</span><span class="sxs-lookup"><span data-stu-id="cd01d-249">Cost</span></span>
<span data-ttu-id="cd01d-250">Není k dispozici pro použití funkce dotazu hello elastické databáze bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="cd01d-250">There is no additional charge for using hello Elastic Database query feature.</span></span> <span data-ttu-id="cd01d-251">V tuto chvíli tato funkce je dostupná pouze v databázích premium jako koncový bod, ale hello horizontálních oddílů lze vrstvy jakékoli služby.</span><span class="sxs-lookup"><span data-stu-id="cd01d-251">However, at this time this feature is available only on premium databases as an end point, but hello shards can be of any service tier.</span></span>

<span data-ttu-id="cd01d-252">Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="cd01d-252">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
