---
title: "Začínáme s úlohami elastické databáze | Microsoft Docs"
description: "použití úlohy elastické databáze"
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
ms.openlocfilehash: 05c20e880d4eb1eacdecc0c4c7e7491dfe1e6a89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-elastic-database-jobs"></a><span data-ttu-id="711d9-103">Začínáme s úlohami elastické databáze</span><span class="sxs-lookup"><span data-stu-id="711d9-103">Getting started with Elastic Database jobs</span></span>
<span data-ttu-id="711d9-104">Elastické databáze úlohy (preview) pro databázi SQL Azure umožňuje spolehlivost spuštění skriptů T-SQL, které jsou rozmístěny v několika databází při automaticky opakování a poskytování případné dokončení záruky.</span><span class="sxs-lookup"><span data-stu-id="711d9-104">Elastic Database jobs (preview) for Azure SQL Database allows you to reliability execute T-SQL scripts that span multiple databases while automatically retrying and providing eventual completion guarantees.</span></span> <span data-ttu-id="711d9-105">Další informace o funkci úlohy elastické databáze, najdete v tématu [stránku Přehled funkce](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="711d9-105">For more information about the Elastic Database job feature, please see the [feature overview page](sql-database-elastic-jobs-overview.md).</span></span>

<span data-ttu-id="711d9-106">Toto téma rozšiřuje najít v ukázce [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="711d9-106">This topic extends the sample found in [Getting started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span> <span data-ttu-id="711d9-107">Po dokončení bude: Naučte se vytvářet a spravovat úlohy, které správu skupiny související databází.</span><span class="sxs-lookup"><span data-stu-id="711d9-107">When completed, you will: learn how to create and manage jobs that manage a group of related databases.</span></span> <span data-ttu-id="711d9-108">Není nutné používat nástroje elastické škálování, pokud chcete využít výhod elastické úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-108">It is not required to use the Elastic Scale tools in order to take advantage of the benefits of Elastic jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="711d9-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="711d9-109">Prerequisites</span></span>
<span data-ttu-id="711d9-110">Stažení a spuštění [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="711d9-110">Download and run the [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-the-sample-app"></a><span data-ttu-id="711d9-111">Vytvoření horizontálního oddílu mapy manager pomocí ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="711d9-111">Create a shard map manager using the sample app</span></span>
<span data-ttu-id="711d9-112">Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="711d9-112">Here you will create a shard map manager along with several shards, followed by insertion of data into the shards.</span></span> <span data-ttu-id="711d9-113">Pokud již máte horizontálních oddílů nastavit s horizontálně dělená data v nich, můžete přeskočit následující kroky a přesunout k další části.</span><span class="sxs-lookup"><span data-stu-id="711d9-113">If you already have shards set up with sharded data in them, you can skip the following steps and move to the next section.</span></span>

1. <span data-ttu-id="711d9-114">Sestavení a spuštění **Začínáme s nástroje elastické databáze** ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="711d9-114">Build and run the **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="711d9-115">Postupujte podle pokynů až do kroku 7 v části [stažení a spuštění ukázkové aplikace](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="711d9-115">Follow the steps until step 7 in the section [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="711d9-116">Na konci tohoto kroku 7 zobrazí se následující příkazový řádek:</span><span class="sxs-lookup"><span data-stu-id="711d9-116">At the end of Step 7, you will see the following command prompt:</span></span>

   ![příkazový řádek](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. <span data-ttu-id="711d9-118">V okně příkazového řádku zadejte "1" a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="711d9-118">In the command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="711d9-119">To vytvoří horizontálního oddílu správce mapy a přidá dva horizontálních oddílů server.</span><span class="sxs-lookup"><span data-stu-id="711d9-119">This creates the shard map manager, and adds two shards to the server.</span></span> <span data-ttu-id="711d9-120">Potom zadejte "3" a stiskněte klávesu **Enter**; čtyřikrát tuto akci zopakujte.</span><span class="sxs-lookup"><span data-stu-id="711d9-120">Then type "3" and press **Enter**; repeat this action four times.</span></span> <span data-ttu-id="711d9-121">Vloží řádky ukázková data ve vašem horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="711d9-121">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="711d9-122">[Portálu Azure](https://portal.azure.com) by měl zobrazit tři nové databáze:</span><span class="sxs-lookup"><span data-stu-id="711d9-122">The [Azure Portal](https://portal.azure.com) should show three new databases:</span></span>

   ![Visual Studio potvrzení](./media/sql-database-elastic-query-getting-started/portal.png)

   <span data-ttu-id="711d9-124">Nyní vytvoříme vlastní databázi kolekce, která odráží všechny databáze v mapě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="711d9-124">At this point, we will create a custom database collection that reflects all the databases in the shard map.</span></span> <span data-ttu-id="711d9-125">To vám umožní nám vytvářet a spouštět úlohy, která přidá novou tabulku napříč horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="711d9-125">This will allow us to create and execute a job that add a new table across shards.</span></span>

<span data-ttu-id="711d9-126">Zde jsme by obvykle vytvoření mapy horizontálního oddílu cíle, pomocí **New-AzureSqlJobTarget** rutiny.</span><span class="sxs-lookup"><span data-stu-id="711d9-126">Here we would usually create a shard map target, using the **New-AzureSqlJobTarget** cmdlet.</span></span> <span data-ttu-id="711d9-127">Databáze manager mapy horizontálního oddílu musí být nastavena jako cíl databáze a pak je konkrétní horizontálních mapy zadaný jako cíl.</span><span class="sxs-lookup"><span data-stu-id="711d9-127">The shard map manager database must be set as a database target and then the specific shard map is specified as a target.</span></span> <span data-ttu-id="711d9-128">Místo toho bude výčet všechny databáze na serveru a přidejte databáze do nové vlastní kolekce, s výjimkou hlavní databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-128">Instead, we are going to enumerate all the databases in the server and add the databases to the new custom collection with the exception of master database.</span></span>

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a><span data-ttu-id="711d9-129">Vytvoří vlastní kolekce a přidejte všechny databáze na serveru k cíli vlastní kolekce s výjimkou hlavní server.</span><span class="sxs-lookup"><span data-stu-id="711d9-129">Creates a custom collection and add all databases in the server to the custom collection target with the exception of master.</span></span>
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
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```
## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="711d9-130">Vytvoření skriptu T-SQL pro provedení mezi databází</span><span class="sxs-lookup"><span data-stu-id="711d9-130">Create a T-SQL Script for execution across databases</span></span>
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

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a><span data-ttu-id="711d9-131">Vytvořit úlohu pro spuštění skriptu přes vlastní skupinu databází</span><span class="sxs-lookup"><span data-stu-id="711d9-131">Create the job to execute a script across the custom group of databases</span></span>

   ```
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a><span data-ttu-id="711d9-132">Provedení úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-132">Execute the job</span></span>
<span data-ttu-id="711d9-133">Následující skript prostředí PowerShell můžete použít ke spuštění stávající úloze:</span><span class="sxs-lookup"><span data-stu-id="711d9-133">The following PowerShell script can be used to execute an existing job:</span></span>

<span data-ttu-id="711d9-134">Aktualizujte tak, aby odrážela název požadované úlohy, který provedli následující proměnnou:</span><span class="sxs-lookup"><span data-stu-id="711d9-134">Update the following variable to reflect the desired job name to have executed:</span></span>

   ```
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a><span data-ttu-id="711d9-135">Načíst stav provádění jedné úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-135">Retrieve the state of a single job execution</span></span>
<span data-ttu-id="711d9-136">Použijte stejný **Get-AzureSqlJobExecution** rutiny s **metoda IncludeChildren** parametr, pokud chcete zobrazit stav podřízených spuštění úlohy, konkrétně určitém stavu pro každé spuštění úlohy každou databázi cílem úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-136">Use the same **Get-AzureSqlJobExecution** cmdlet with the **IncludeChildren** parameter to view the state of child job executions, namely the specific state for each job execution against each database targeted by the job.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a><span data-ttu-id="711d9-137">Zobrazení stavu mezi jednotlivými spuštěními více úloh</span><span class="sxs-lookup"><span data-stu-id="711d9-137">View the state across multiple job executions</span></span>
<span data-ttu-id="711d9-138">**Get-AzureSqlJobExecution** rutina má více volitelné parametry, které lze použít k zobrazení více spuštění úlohy, filtrovaný pomocí zadané parametry.</span><span class="sxs-lookup"><span data-stu-id="711d9-138">The **Get-AzureSqlJobExecution** cmdlet has multiple optional parameters that can be used to display multiple job executions, filtered through the provided parameters.</span></span> <span data-ttu-id="711d9-139">Následující ukazuje některé možné způsoby, jak používat Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="711d9-139">The following demonstrates some of the possible ways to use Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="711d9-140">Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:</span><span class="sxs-lookup"><span data-stu-id="711d9-140">Retrieve all active top level job executions:</span></span>

   ```
    Get-AzureSqlJobExecution
   ```

<span data-ttu-id="711d9-141">Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-141">Retrieve all top level job executions, including inactive job executions:</span></span>

   ```
    Get-AzureSqlJobExecution -IncludeInactive
   ```

<span data-ttu-id="711d9-142">Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-142">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

   ```
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

<span data-ttu-id="711d9-143">Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-143">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

<span data-ttu-id="711d9-144">Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-144">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

   ```
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="711d9-145">Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-145">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

   ```
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

<span data-ttu-id="711d9-146">Načtení seznamu spuštěních úloh úlohy v rámci provedení určité úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-146">Retrieve the list of job task executions within a specific job execution:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

<span data-ttu-id="711d9-147">Načtěte podrobnosti o provádění úkolů úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-147">Retrieve job task execution details:</span></span>

<span data-ttu-id="711d9-148">Následující skript prostředí PowerShell slouží k zobrazení podrobností o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="711d9-148">The following PowerShell script can be used to view the details of a job task execution, which is particularly useful when debugging execution failures.</span></span>
   ```
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a><span data-ttu-id="711d9-149">Načtení selhání v rámci úlohy spuštěních úloh</span><span class="sxs-lookup"><span data-stu-id="711d9-149">Retrieve failures within job task executions</span></span>
<span data-ttu-id="711d9-150">Objekt JobTaskExecution obsahuje vlastnost pro životní cyklus úlohy společně s vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="711d9-150">The JobTaskExecution object includes a property for the Lifecycle of the task along with a Message property.</span></span> <span data-ttu-id="711d9-151">Pokud se nezdařilo provádění úloh úkolu, vlastnost životního cyklu bude nutné nastavit *se nezdařilo* a vlastnosti zprávy se nastaví výsledné zpráva o výjimce a jeho zásobníku.</span><span class="sxs-lookup"><span data-stu-id="711d9-151">If a job task execution failed, the Lifecycle property will be set to *Failed* and the Message property will be set to the resulting exception message and its stack.</span></span> <span data-ttu-id="711d9-152">Pokud úloha nebyla úspěšná, je důležité k zobrazení podrobností úlohy, které se nezdařilo pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="711d9-152">If a job did not succeed, it is important to view the details of job tasks that did not succeed for a given job.</span></span>

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

## <a name="waiting-for-a-job-execution-to-complete"></a><span data-ttu-id="711d9-153">Čekání na dokončení provedení úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-153">Waiting for a job execution to complete</span></span>
<span data-ttu-id="711d9-154">Následující skript prostředí PowerShell umožňuje počkejte na dokončení úlohy úlohy:</span><span class="sxs-lookup"><span data-stu-id="711d9-154">The following PowerShell script can be used to wait for a job task to complete:</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="711d9-155">Vytvořit zásadu vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="711d9-155">Create a custom execution policy</span></span>
<span data-ttu-id="711d9-156">Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="711d9-156">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="711d9-157">Zásady spouštění aktuálně povolit pro definování:</span><span class="sxs-lookup"><span data-stu-id="711d9-157">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="711d9-158">Název: Identifikátor pro zásady spouštění.</span><span class="sxs-lookup"><span data-stu-id="711d9-158">Name: Identifier for the execution policy.</span></span>
* <span data-ttu-id="711d9-159">Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="711d9-159">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="711d9-160">Počáteční Interval opakování: Interval čekání před první opakování.</span><span class="sxs-lookup"><span data-stu-id="711d9-160">Initial Retry Interval: Interval to wait before first retry.</span></span>
* <span data-ttu-id="711d9-161">Maximální Interval opakování: Limitu opakování intervalů používat.</span><span class="sxs-lookup"><span data-stu-id="711d9-161">Maximum Retry Interval: Cap of retry intervals to use.</span></span>
* <span data-ttu-id="711d9-162">Koeficient omezení rychlosti Interval opakování: Koeficient používá k výpočtu další interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="711d9-162">Retry Interval Backoff Coefficient: Coefficient used to calculate the next interval between retries.</span></span>  <span data-ttu-id="711d9-163">Se používá následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2).</span><span class="sxs-lookup"><span data-stu-id="711d9-163">The following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span>
* <span data-ttu-id="711d9-164">Maximální počet pokusů: Maximální počet opakování pokusů provést v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-164">Maximum Attempts: The maximum number of retry attempts to perform within a job.</span></span>

<span data-ttu-id="711d9-165">Výchozí zásadu spouštění používá následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="711d9-165">The default execution policy uses the following values:</span></span>

* <span data-ttu-id="711d9-166">Název: Zásady spouštění výchozí</span><span class="sxs-lookup"><span data-stu-id="711d9-166">Name: Default execution policy</span></span>
* <span data-ttu-id="711d9-167">Časový limit úlohy: 1 týden</span><span class="sxs-lookup"><span data-stu-id="711d9-167">Job Timeout: 1 week</span></span>
* <span data-ttu-id="711d9-168">Počáteční Interval opakování: 100 milisekund</span><span class="sxs-lookup"><span data-stu-id="711d9-168">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="711d9-169">Maximální Interval opakování: 30 minut</span><span class="sxs-lookup"><span data-stu-id="711d9-169">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="711d9-170">Opakujte koeficient Interval: 2</span><span class="sxs-lookup"><span data-stu-id="711d9-170">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="711d9-171">Maximální počet pokusů: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="711d9-171">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="711d9-172">Vytvořte zásadu požadované spouštění:</span><span class="sxs-lookup"><span data-stu-id="711d9-172">Create the desired execution policy:</span></span>

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

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="711d9-173">Aktualizovat zásady vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="711d9-173">Update a custom execution policy</span></span>
<span data-ttu-id="711d9-174">Aktualizujte zásady spouštění požadované aktualizace:</span><span class="sxs-lookup"><span data-stu-id="711d9-174">Update the desired execution policy to update:</span></span>

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

## <a name="cancel-a-job"></a><span data-ttu-id="711d9-175">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-175">Cancel a job</span></span>
<span data-ttu-id="711d9-176">Elastické databáze úlohy podporuje požadavků na zrušení úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-176">Elastic Database Jobs supports jobs cancellation requests.</span></span>  <span data-ttu-id="711d9-177">Pokud elastické databáze úlohy zjistí žádost o zrušení úlohy se spouští, se ho pokusí zastavit úlohu.</span><span class="sxs-lookup"><span data-stu-id="711d9-177">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt to stop the job.</span></span>

<span data-ttu-id="711d9-178">Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="711d9-178">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="711d9-179">Zrušení aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci aktuálně prováděné aspekt úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-179">Canceling Currently Executing Tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within the currently executing aspect of the task.</span></span>  <span data-ttu-id="711d9-180">Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude pokus o dotaz zrušíte.</span><span class="sxs-lookup"><span data-stu-id="711d9-180">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt to cancel the query.</span></span>
2. <span data-ttu-id="711d9-181">Ruší opakování úkolů: V případě zrušení zjištění vlákno řízení předtím, než se spustí úloha pro spuštění, vlákno řízení se vyhnout, spouští se úloha a deklarovat požadavek, protože došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="711d9-181">Canceling Task Retries: If a cancellation is detected by the control thread before a task is launched for execution, the control thread will avoid launching the task and declare the request as canceled.</span></span>

<span data-ttu-id="711d9-182">Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude pro nadřazené úloze a všechny jeho podřízené úlohy dodržet žádost o zrušení.</span><span class="sxs-lookup"><span data-stu-id="711d9-182">If a job cancellation is requested for a parent job, the cancellation request will be honored for the parent job and for all of its child jobs.</span></span>

<span data-ttu-id="711d9-183">Odeslat žádost o zrušení, použijte **Stop-AzureSqlJobExecution** rutiny a nastavte **JobExecutionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="711d9-183">To submit a cancellation request, use the **Stop-AzureSqlJobExecution** cmdlet and set the **JobExecutionId** parameter.</span></span>

   ```
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a><span data-ttu-id="711d9-184">Odstranit úlohu podle názvu a historie úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-184">Delete a job by name and the job's history</span></span>
<span data-ttu-id="711d9-185">Elastické databáze úlohy podporuje asynchronní odstranění úloh.</span><span class="sxs-lookup"><span data-stu-id="711d9-185">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="711d9-186">Úloha může být označený k odstranění a systém bude odstranění úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="711d9-186">A job can be marked for deletion and the system will delete the job and all its job history after all job executions have completed for the job.</span></span> <span data-ttu-id="711d9-187">Systém nebude automaticky zrušit spuštěních aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-187">The system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="711d9-188">Místo toho musí být volána Stop-AzureSqlJobExecution zrušit aktivní úloha spuštění.</span><span class="sxs-lookup"><span data-stu-id="711d9-188">Instead, Stop-AzureSqlJobExecution must be invoked to cancel active job executions.</span></span>

<span data-ttu-id="711d9-189">Chcete-li aktivovat odstranění úlohy, použijte **odebrat AzureSqlJob** rutiny a nastavte **JobName** parametr.</span><span class="sxs-lookup"><span data-stu-id="711d9-189">To trigger job deletion, use the **Remove-AzureSqlJob** cmdlet and set the **JobName** parameter.</span></span>

   ```
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a><span data-ttu-id="711d9-190">Vytvořit cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="711d9-190">Create a custom database target</span></span>
<span data-ttu-id="711d9-191">Vlastní databázi cíle může být definován v úlohy elastické databáze, které se dají použít pro spuštění přímo nebo pro zahrnutí do skupiny vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-191">Custom database targets can be defined in Elastic Database jobs which can be used either for execution directly or for inclusion within a custom database group.</span></span> <span data-ttu-id="711d9-192">Vzhledem k tomu **elastické fondy** nejsou přímo, ale podporovány prostřednictvím rozhraní API prostředí PowerShell, můžete jednoduše vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, která zahrnuje všechny databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="711d9-192">Since **elastic pools** are not yet directly supported via the PowerShell APIs, you simply create a custom database target and custom database collection target which encompasses all the databases in the pool.</span></span>

<span data-ttu-id="711d9-193">Nastavte následující proměnné tak, aby odrážela informace o požadované databázi:</span><span class="sxs-lookup"><span data-stu-id="711d9-193">Set the following variables to reflect the desired database information:</span></span>

   ```
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a><span data-ttu-id="711d9-194">Vytvořit cíl kolekce vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="711d9-194">Create a custom database collection target</span></span>
<span data-ttu-id="711d9-195">Povolit spuštění v rámci více cílů definovaných databázových lze definovat cílovou kolekci vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-195">A custom database collection target can be defined to enable execution across multiple defined database targets.</span></span> <span data-ttu-id="711d9-196">Po vytvoření skupiny databáze, databáze může být přidružený k cíli vlastní kolekce.</span><span class="sxs-lookup"><span data-stu-id="711d9-196">After a database group is created, databases can be associated to the custom collection target.</span></span>

<span data-ttu-id="711d9-197">Nastavte následující proměnné tak, aby odrážela konfigurace cílového požadovanou vlastní kolekce:</span><span class="sxs-lookup"><span data-stu-id="711d9-197">Set the following variables to reflect the desired custom collection target configuration:</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a><span data-ttu-id="711d9-198">Přidání databází do kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="711d9-198">Add databases to a custom database collection target</span></span>
<span data-ttu-id="711d9-199">Cíle databáze může být přidružen cíle kolekce vlastní databázi a vytvořte skupinu databází.</span><span class="sxs-lookup"><span data-stu-id="711d9-199">Database targets can be associated with custom database collection targets to create a group of databases.</span></span> <span data-ttu-id="711d9-200">Vždy, když se vytvoří úloha, která zaměřena na cíl, vlastní databázi kolekce, bude rozšířena do cílové databáze přidružený ke skupině v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="711d9-200">Whenever a job is created that targets a custom database collection target, it will be expanded to target the databases associated to the group at the time of execution.</span></span>

<span data-ttu-id="711d9-201">Přidejte databázi požadované určité vlastní kolekci:</span><span class="sxs-lookup"><span data-stu-id="711d9-201">Add the desired database to a specific custom collection:</span></span>

   ```
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="711d9-202">Zkontrolujte databází v rámci kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="711d9-202">Review the databases within a custom database collection target</span></span>
<span data-ttu-id="711d9-203">Použití **Get-AzureSqlJobTarget** rutiny načíst podřízené databází v rámci kolekce cíl vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-203">Use the **Get-AzureSqlJobTarget** cmdlet to retrieve the child databases within a custom database collection target.</span></span>

   ```
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="711d9-204">Vytvořit úlohu pro spuštění skriptu mezi cílovou kolekci vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="711d9-204">Create a job to execute a script across a custom database collection target</span></span>
<span data-ttu-id="711d9-205">Použití **New-AzureSqlJob** rutiny vytvořit úlohu pro skupinu databází definované cílovou kolekci vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-205">Use the **New-AzureSqlJob** cmdlet to create a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="711d9-206">Elastické databáze úlohy se úloha rozšířit více podřízených úloh, každou odpovídající databázi přidruženého cílové kolekce vlastní databázi a ujistěte se, že skript se spustí na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="711d9-206">Elastic Database jobs will expand the job into multiple child jobs each corresponding to a database associated with the custom database collection target and ensure that the script is executed against each database.</span></span> <span data-ttu-id="711d9-207">Znovu je důležité, aby skripty se idempotent chcete být odolní vůči opakování.</span><span class="sxs-lookup"><span data-stu-id="711d9-207">Again, it is important that scripts are idempotent to be resilient to retries.</span></span>

   ```
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a><span data-ttu-id="711d9-208">Shromažďování dat mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="711d9-208">Data collection across databases</span></span>
<span data-ttu-id="711d9-209">**Elastické databáze úlohy** podporuje provádění dotazu napříč skupinou databází a odesílá výsledky do tabulky zadaná databáze.</span><span class="sxs-lookup"><span data-stu-id="711d9-209">**Elastic Database jobs** supports executing a query across a group of databases and sends the results to a specified database’s table.</span></span> <span data-ttu-id="711d9-210">V tabulce můžete položit dotaz na ve skutečnosti zobrazíte výsledky dotazu z každé databáze.</span><span class="sxs-lookup"><span data-stu-id="711d9-210">The table can be queried after the fact to see the query’s results from each database.</span></span> <span data-ttu-id="711d9-211">To poskytuje asynchronní mechanismus, při spuštění dotazu mezi mnoha databázemi.</span><span class="sxs-lookup"><span data-stu-id="711d9-211">This provides an asynchronous mechanism to execute a query across many databases.</span></span> <span data-ttu-id="711d9-212">Selhání případech jako jedna z databází není dočasně k dispozici jsou automaticky zpracováván opakování.</span><span class="sxs-lookup"><span data-stu-id="711d9-212">Failure cases such as one of the databases being temporarily unavailable are handled automatically via retries.</span></span>

<span data-ttu-id="711d9-213">Zadané cílové tabulky se automaticky vytvoří, pokud ještě neexistuje, odpovídající schéma vrácené výsledné sady.</span><span class="sxs-lookup"><span data-stu-id="711d9-213">The specified destination table will be automatically created if it does not yet exist, matching the schema of the returned result set.</span></span> <span data-ttu-id="711d9-214">Pokud spuštění skriptu vrátí více sad výsledků dotazu, odeslat úlohy elastické databáze pouze první z nich pro zadané cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="711d9-214">If a script execution returns multiple result sets, Elastic Database jobs will only send the first one to the provided destination table.</span></span>

<span data-ttu-id="711d9-215">Následující skript prostředí PowerShell můžete použít ke spuštění skriptu shromažďování své výsledky do zadané tabulky.</span><span class="sxs-lookup"><span data-stu-id="711d9-215">The following PowerShell script can be used to execute a script collecting its results into a specified table.</span></span> <span data-ttu-id="711d9-216">Tento skript předpokládá, že byla vytvořena skriptu T-SQL, který vrací jednu výslednou sadu a kolekce cíl vlastní databáze byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="711d9-216">This script assumes that a T-SQL script has been created which outputs a single result set and a custom database collection target has been created.</span></span>

<span data-ttu-id="711d9-217">Nastavte následující tak, aby odrážela požadované skriptu, přihlašovací údaje a provádění cíl:</span><span class="sxs-lookup"><span data-stu-id="711d9-217">Set the following to reflect the desired script, credentials, and execution target:</span></span>

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="711d9-218">Vytvořte a spusťte úlohu pro scénáře kolekce dat</span><span class="sxs-lookup"><span data-stu-id="711d9-218">Create and start a job for data collection scenarios</span></span>
   ```
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a><span data-ttu-id="711d9-219">Vytvoření plánu pro provádění úlohy pomocí aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="711d9-219">Create a schedule for job execution using a job trigger</span></span>
<span data-ttu-id="711d9-220">Následující skript prostředí PowerShell slouží k vytvoření opakovaném plánu.</span><span class="sxs-lookup"><span data-stu-id="711d9-220">The following PowerShell script can be used to create a reoccurring schedule.</span></span> <span data-ttu-id="711d9-221">Tento skript používá intervalu jednu minutu, ale nové AzureSqlJobSchedule také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry.</span><span class="sxs-lookup"><span data-stu-id="711d9-221">This script uses a one minute interval, but New-AzureSqlJobSchedule also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="711d9-222">Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.</span><span class="sxs-lookup"><span data-stu-id="711d9-222">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="711d9-223">Vytvoření nového plánu:</span><span class="sxs-lookup"><span data-stu-id="711d9-223">Create a new schedule:</span></span>
   ```
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="711d9-224">Vytvořit aktivační událost úlohy tak, aby měl úlohu provést podle časového plánu</span><span class="sxs-lookup"><span data-stu-id="711d9-224">Create a job trigger to have a job executed on a time schedule</span></span>
<span data-ttu-id="711d9-225">Aktivační události úlohy lze definovat za účelem mít úlohu provést podle časového plánu.</span><span class="sxs-lookup"><span data-stu-id="711d9-225">A job trigger can be defined to have a job executed according to a time schedule.</span></span> <span data-ttu-id="711d9-226">Následující skript prostředí PowerShell slouží k vytvoření aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-226">The following PowerShell script can be used to create a job trigger.</span></span>

<span data-ttu-id="711d9-227">Nastavte následující proměnné tak, aby odpovídaly požadované úlohy a plán:</span><span class="sxs-lookup"><span data-stu-id="711d9-227">Set the following variables to correspond to the desired job and schedule:</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a><span data-ttu-id="711d9-228">Odebrání naplánované přidružení o zastavení úlohy ve spouštění podle plánu.</span><span class="sxs-lookup"><span data-stu-id="711d9-228">Remove a scheduled association to stop job from executing on schedule</span></span>
<span data-ttu-id="711d9-229">Odebrání ze opakovaném provádění úlohy prostřednictvím aktivační události úlohy, můžete odebrat aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="711d9-229">To discontinue reoccurring job execution through a job trigger, the job trigger can be removed.</span></span>
<span data-ttu-id="711d9-230">Odebrat aktivační události úlohy zastavení úlohy z se spouští podle plánu pomocí **odebrat AzureSqlJobTrigger** rutiny.</span><span class="sxs-lookup"><span data-stu-id="711d9-230">Remove a job trigger to stop a job from being executed according to a schedule using the **Remove-AzureSqlJobTrigger** cmdlet.</span></span>

   ```
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a><span data-ttu-id="711d9-231">Import výsledků dotazu elastické databáze do aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="711d9-231">Import elastic database query results to Excel</span></span>
 <span data-ttu-id="711d9-232">Můžete importovat výsledky z dotazu do souboru aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="711d9-232">You can import the results from of a query to an Excel file.</span></span>

1. <span data-ttu-id="711d9-233">Spusťte aplikaci Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="711d9-233">Launch Excel 2013.</span></span>
2. <span data-ttu-id="711d9-234">Přejděte na **Data** pásu karet.</span><span class="sxs-lookup"><span data-stu-id="711d9-234">Navigate to the **Data** ribbon.</span></span>
3. <span data-ttu-id="711d9-235">Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="711d9-235">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Importu pro aplikaci Excel z jiných zdrojů](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. <span data-ttu-id="711d9-237">V **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru.</span><span class="sxs-lookup"><span data-stu-id="711d9-237">In the **Data Connection Wizard** type the server name and login credentials.</span></span> <span data-ttu-id="711d9-238">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="711d9-238">Then click **Next**.</span></span>
5. <span data-ttu-id="711d9-239">V dialogovém okně **vyberte databáze, která obsahuje data, která chcete**, vyberte **ElasticDBQuery** databáze.</span><span class="sxs-lookup"><span data-stu-id="711d9-239">In the dialog box **Select the database that contains the data you want**, select the **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="711d9-240">Vyberte **zákazníci** tabulky v zobrazení seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="711d9-240">Select the **Customers** table in the list view and click **Next**.</span></span> <span data-ttu-id="711d9-241">Pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="711d9-241">Then click **Finish**.</span></span>
7. <span data-ttu-id="711d9-242">V **importovat Data** formuláři v části **vyberte, jak chcete zobrazit tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="711d9-242">In the **Import Data** form, under **Select how you want to view this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="711d9-243">Všechny řádky z **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit listu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="711d9-243">All the rows from **Customers** table, stored in different shards populate the Excel sheet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="711d9-244">Další kroky</span><span class="sxs-lookup"><span data-stu-id="711d9-244">Next steps</span></span>
<span data-ttu-id="711d9-245">Teď můžete použít funkce dat v aplikaci Excel.</span><span class="sxs-lookup"><span data-stu-id="711d9-245">You can now use Excel’s data functions.</span></span> <span data-ttu-id="711d9-246">Použijte připojovací řetězec s názvem serveru, názvu databáze a pověření pro připojení k databázi elastické dotazu vaše integrace nástrojů BI a data.</span><span class="sxs-lookup"><span data-stu-id="711d9-246">Use the connection string with your server name, database name and credentials to connect your BI and data integration tools to the elastic query database.</span></span> <span data-ttu-id="711d9-247">Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje.</span><span class="sxs-lookup"><span data-stu-id="711d9-247">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="711d9-248">Viz elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a SQL Server tabulky, které by se připojit k vaší nástrojem.</span><span class="sxs-lookup"><span data-stu-id="711d9-248">Refer to the elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect to with your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="711d9-249">Náklady</span><span class="sxs-lookup"><span data-stu-id="711d9-249">Cost</span></span>
<span data-ttu-id="711d9-250">Není k dispozici pro použití funkce dotazu elastické databáze bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="711d9-250">There is no additional charge for using the Elastic Database query feature.</span></span> <span data-ttu-id="711d9-251">V tuto chvíli tato funkce je dostupná pouze v databázích premium jako koncový bod, ale horizontálních oddílů lze vrstvy jakékoli služby.</span><span class="sxs-lookup"><span data-stu-id="711d9-251">However, at this time this feature is available only on premium databases as an end point, but the shards can be of any service tier.</span></span>

<span data-ttu-id="711d9-252">Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="711d9-252">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
