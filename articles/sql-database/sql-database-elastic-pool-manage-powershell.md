---
title: "Prostředí PowerShell: Vytvoření a správa fondu elastické databáze Azure SQL | Microsoft Docs"
description: "Další informace o použití Powershellu ke správě fondu elastické databáze."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 61289770-69b9-4ae3-9252-d0e94d709331
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/06/2017
ms.author: srinia
ms.openlocfilehash: 5e76397c62e5a6ff7fb356bd81218c307f3fda31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a><span data-ttu-id="0298c-103">Vytvoření a správa fondu elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0298c-103">Create and manage an elastic pool with PowerShell</span></span>
<span data-ttu-id="0298c-104">Toto téma ukazuje, jak vytvořit a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0298c-104">This topic shows you how to create and manage scalable [elastic pools](sql-database-elastic-pool.md) with PowerShell.</span></span>  <span data-ttu-id="0298c-105">Můžete také vytvořit a spravovat pomocí služby Azure elastického fondu [portál Azure](https://portal.azure.com/), REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="0298c-105">You can also create and manage an Azure elastic pool using the [Azure portal](https://portal.azure.com/), REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="0298c-106">Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="0298c-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a><span data-ttu-id="0298c-107">Vytvoření elastického fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-107">Create an elastic pool</span></span>
<span data-ttu-id="0298c-108">[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) rutina vytvoří fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="0298c-108">The [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet creates an elastic pool.</span></span> <span data-ttu-id="0298c-109">Hodnoty eDTU na fond a minimální a maximální Dtu jsou omezeny hodnota vrstvy služby (Basic, Standard, Premium nebo Premium RS).</span><span class="sxs-lookup"><span data-stu-id="0298c-109">The values for eDTU per pool, min, and max DTUs are constrained by the service tier value (Basic, Standard, Premium, or Premium RS).</span></span> <span data-ttu-id="0298c-110">V tématu [omezení eDTU a úložiště pro elastické fondy a databází ve fondu](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span><span class="sxs-lookup"><span data-stu-id="0298c-110">See [eDTU and storage limits for elastic pools and pooled databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span></span>

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="0298c-111">Vytvoření databáze ve fondu v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="0298c-112">Použijte rutinu [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) a nastavte parametr **ElasticPoolName** na cílový fond.</span><span class="sxs-lookup"><span data-stu-id="0298c-112">Use the [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet and set the **ElasticPoolName** parameter to the target pool.</span></span> <span data-ttu-id="0298c-113">Chcete-li do pružného fondu přesunout existující databázi, přečtěte si téma [přesun databáze do pružného fondu](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span><span class="sxs-lookup"><span data-stu-id="0298c-113">To move an existing database into an elastic pool, see [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span></span>

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a><span data-ttu-id="0298c-114">Celý skript</span><span class="sxs-lookup"><span data-stu-id="0298c-114">Complete script</span></span>
<span data-ttu-id="0298c-115">Tento skript vytvoří skupinu prostředků Azure a serverem.</span><span class="sxs-lookup"><span data-stu-id="0298c-115">This script creates an Azure resource group and a server.</span></span> <span data-ttu-id="0298c-116">Po výzvě zadejte jméno a heslo správce pro nový server (nikoli vaše přihlašovací údaje Azure).</span><span class="sxs-lookup"><span data-stu-id="0298c-116">When prompted, supply an administrator username and password for the new server (not your Azure credentials).</span></span>

```PowerShell
$subscriptionId = '<your Azure subscription id>'
$resourceGroupName = '<resource group name>'
$location = '<datacenter location>'
$serverName = '<server name>'
$poolName = '<pool name>'
$databaseName = '<database name>'

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB
```

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a><span data-ttu-id="0298c-117">Vytvoření fondu elastické databáze a přidání více databází ve fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-117">Create an elastic pool and add multiple pooled databases</span></span>
<span data-ttu-id="0298c-118">Vytváření mnoho databází v elastickém fondu může trvat dobu, pokud se provádí pomocí portálu nebo rutiny prostředí PowerShell, který současně vytvořit pouze jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="0298c-118">Creation of many databases in an elastic pool can take time when done using the portal or PowerShell cmdlets that create only a single database at a time.</span></span> <span data-ttu-id="0298c-119">K automatizaci vytváření fondu elastické databáze, najdete v části [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span><span class="sxs-lookup"><span data-stu-id="0298c-119">To automate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="0298c-120">Přesun databáze do pružného fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-120">Move a database into an elastic pool</span></span>
<span data-ttu-id="0298c-121">Přesouváte databázi do nebo z fondu elastické databáze s [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span><span class="sxs-lookup"><span data-stu-id="0298c-121">You can move a database into or out of an elastic pool with the [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span></span>

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="0298c-122">Změňte nastavení výkonu elastického fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-122">Change performance settings of an elastic pool</span></span>
<span data-ttu-id="0298c-123">Když ke snížení výkonu, můžete změnit nastavení fondu pro přizpůsobení růstu.</span><span class="sxs-lookup"><span data-stu-id="0298c-123">When performance suffers, you can change the settings of the pool to accommodate growth.</span></span> <span data-ttu-id="0298c-124">Použití [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0298c-124">Use the [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span></span> <span data-ttu-id="0298c-125">Nastavte parametr - Dtu na Edtu na fond.</span><span class="sxs-lookup"><span data-stu-id="0298c-125">Set the -Dtu parameter to the eDTUs per pool.</span></span> <span data-ttu-id="0298c-126">V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0298c-126">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-the-storage-limit-for-an-elastic-pool"></a><span data-ttu-id="0298c-127">Změnit limit úložiště fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="0298c-127">Change the storage limit for an elastic pool</span></span>

<span data-ttu-id="0298c-128">Použití [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny nastavit _- StorageMB_ parametr.</span><span class="sxs-lookup"><span data-stu-id="0298c-128">Use the [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet to set the _-StorageMB_ parameter.</span></span> <span data-ttu-id="0298c-129">Zadejte limit úložiště v MB (například 2097152 sady úložiště omezit na 2 TB).</span><span class="sxs-lookup"><span data-stu-id="0298c-129">Provide the storage limit in MB (for example, 2097152 sets the storage limit to 2 TB).</span></span> <span data-ttu-id="0298c-130">V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0298c-130">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0298c-131">Výchozí maximální úložiště na každý fond pro fondy Premium s 1 500 Edtu nebo více je 750 GB.</span><span class="sxs-lookup"><span data-stu-id="0298c-131">The default max data storage per pool for Premium pools with 1500 eDTUs or more is 750 GB.</span></span> <span data-ttu-id="0298c-132">Získat vyšší _maximální velikost úložiště dat na každý fond_, musí být explicitně nastaveny limit úložiště.</span><span class="sxs-lookup"><span data-stu-id="0298c-132">To obtain the higher _max data storage size per pool_, the storage limit must be explicitly set.</span></span> <span data-ttu-id="0298c-133">Premium fondy s více než 750 GB úložiště je aktuálně ve verzi public preview v následujících oblastech: východní USA 2, západ USA, Virginia nám verze pro státní správu, západní Evropa, Německo centrální, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.</span><span class="sxs-lookup"><span data-stu-id="0298c-133">Premium pools with more than 750 GB of storage is currently in public preview in the following regions: East US 2, West US, US Gov Virginia, West Europe, Germany Central, Southeast Asia, Japan East, Australia East, Canada Central, and Canada East.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-the-status-of-pool-operations"></a><span data-ttu-id="0298c-134">Načíst stav operace fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-134">Get the status of pool operations</span></span>
<span data-ttu-id="0298c-135">Vytvoření fondu elastické databáze může trvat dobu.</span><span class="sxs-lookup"><span data-stu-id="0298c-135">Creating an elastic pool can take time.</span></span> <span data-ttu-id="0298c-136">Chcete-li sledovat stav fondu operacím, včetně vytváření a aktualizace, použijte [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0298c-136">To track the status of pool operations including creation and updates, use the [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-the-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a><span data-ttu-id="0298c-137">Získat stav přesun databáze do a z fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="0298c-137">Get the status of moving a database into and out of an elastic pool</span></span>
<span data-ttu-id="0298c-138">Přesunutí databáze může trvat dobu.</span><span class="sxs-lookup"><span data-stu-id="0298c-138">Moving a database can take time.</span></span> <span data-ttu-id="0298c-139">Sleduje stav přesunutí pomocí [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0298c-139">Track a move status using the [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="0298c-140">Získat data použití prostředků fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="0298c-140">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="0298c-141">Metriky, které mohou být načteny jako procento limit fondu prostředků:</span><span class="sxs-lookup"><span data-stu-id="0298c-141">Metrics that can be retrieved as a percentage of the resource pool limit:</span></span>

| <span data-ttu-id="0298c-142">Název metriky</span><span class="sxs-lookup"><span data-stu-id="0298c-142">Metric name</span></span> | <span data-ttu-id="0298c-143">Popis</span><span class="sxs-lookup"><span data-stu-id="0298c-143">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0298c-144">Procesor\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-144">cpu\_percent</span></span> |<span data-ttu-id="0298c-145">Průměr výpočetní využití v procentech limit fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-145">Average compute utilization in percentage of the limit of the pool.</span></span> |
| <span data-ttu-id="0298c-146">fyzické\_data\_číst\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-146">physical\_data\_read\_percent</span></span> |<span data-ttu-id="0298c-147">Průměrné využití vstupně-výstupních operací v procentech na základě limitu fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-147">Average I/O utilization in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="0298c-148">protokol\_zápisu\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-148">log\_write\_percent</span></span> |<span data-ttu-id="0298c-149">Průměr zápisu využití prostředků v procentech limit fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-149">Average write resource utilization in percentage of the limit of the pool.</span></span> |
| <span data-ttu-id="0298c-150">DTU\_spotřeba\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-150">DTU\_consumption\_percent</span></span> |<span data-ttu-id="0298c-151">Průměrná eDTU využití v procentech omezení eDTU pro fond</span><span class="sxs-lookup"><span data-stu-id="0298c-151">Average eDTU utilization in percentage of eDTU limit for the pool</span></span> |
| <span data-ttu-id="0298c-152">úložiště\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-152">storage\_percent</span></span> |<span data-ttu-id="0298c-153">Průměrné využití úložiště v procentech limit úložiště fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-153">Average storage utilization in percentage of the storage limit of the pool.</span></span> |
| <span data-ttu-id="0298c-154">pracovníci\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-154">workers\_percent</span></span> |<span data-ttu-id="0298c-155">Maximální souběžných pracovních procesů (počet požadavků) v procentech na základě limitu fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-155">Maximum concurrent workers (requests) in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="0298c-156">relace\_procent</span><span class="sxs-lookup"><span data-stu-id="0298c-156">sessions\_percent</span></span> |<span data-ttu-id="0298c-157">Maximální počet souběžných relací v procentech na základě limitu fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-157">Maximum concurrent sessions in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="0298c-158">eDTU_limit</span><span class="sxs-lookup"><span data-stu-id="0298c-158">eDTU_limit</span></span> |<span data-ttu-id="0298c-159">Aktuální maximální elastický fond DTU nastavení pro tento elastický fond během tohoto intervalu.</span><span class="sxs-lookup"><span data-stu-id="0298c-159">Current max elastic pool DTU setting for this elastic pool during this interval.</span></span> |
| <span data-ttu-id="0298c-160">úložiště\_limit</span><span class="sxs-lookup"><span data-stu-id="0298c-160">storage\_limit</span></span> |<span data-ttu-id="0298c-161">Aktuální maximální elastický fond úložiště limit nastavení pro toto elastického fondu v megabajtech během tohoto intervalu.</span><span class="sxs-lookup"><span data-stu-id="0298c-161">Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.</span></span> |
| <span data-ttu-id="0298c-162">eDTU\_použít</span><span class="sxs-lookup"><span data-stu-id="0298c-162">eDTU\_used</span></span> |<span data-ttu-id="0298c-163">Průměrná Edtu používané fondu v tomto intervalu.</span><span class="sxs-lookup"><span data-stu-id="0298c-163">Average eDTUs used by the pool in this interval.</span></span> |
| <span data-ttu-id="0298c-164">úložiště\_použít</span><span class="sxs-lookup"><span data-stu-id="0298c-164">storage\_used</span></span> |<span data-ttu-id="0298c-165">Průměrná úložiště používané fondu v tomto intervalu v bajtech</span><span class="sxs-lookup"><span data-stu-id="0298c-165">Average storage used by the pool in this interval in bytes</span></span> |

<span data-ttu-id="0298c-166">**Metriky členitosti nebo období:**</span><span class="sxs-lookup"><span data-stu-id="0298c-166">**Metrics granularity/retention periods:**</span></span>

* <span data-ttu-id="0298c-167">Data jsou vrácena v členitosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="0298c-167">Data is returned at 5-minute granularity.</span></span>  
* <span data-ttu-id="0298c-168">Doba uchování dat je 35 dnů.</span><span class="sxs-lookup"><span data-stu-id="0298c-168">Data retention is 35 days.</span></span>  

<span data-ttu-id="0298c-169">Tato rutina a rozhraní API omezí počet řádků, které mohou být načteny jedno volání na 1000 řádky (o 3 dny dat ve členitosti 5 minut).</span><span class="sxs-lookup"><span data-stu-id="0298c-169">This cmdlet and API limits the number of rows that can be retrieved in one call to 1000 rows (about 3 days of data at 5-minute granularity).</span></span> <span data-ttu-id="0298c-170">Tento příkaz lze volat vícekrát s jinou počáteční nebo koncové časové intervaly pro načtení další data, ale</span><span class="sxs-lookup"><span data-stu-id="0298c-170">But this command can be called multiple times with different start/end time intervals to retrieve more data</span></span>

<span data-ttu-id="0298c-171">Chcete-li získat metriky:</span><span class="sxs-lookup"><span data-stu-id="0298c-171">To retrieve the metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a><span data-ttu-id="0298c-172">Získat data použití prostředků pro databáze v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-172">Get resource usage data for a database in an elastic pool</span></span>
<span data-ttu-id="0298c-173">Tato rozhraní API jsou stejné jako rozhraní API používaná pro sledování využití prostředků služby jedné databáze, s výjimkou následujících sémantického rozdíl: metriky načíst jsou vyjádřené jako procentní podíl každou databázi max Edtu (nebo ekvivalentní zakončení pro základní metriku jako procesoru nebo vstupně-výstupní operace) nastavte pro tento fond.</span><span class="sxs-lookup"><span data-stu-id="0298c-173">These APIs are the same as the APIs used for monitoring the resource utilization of a single database, except for the following semantic difference: metrics retrieved are expressed as a percentage of the per database max eDTUs (or equivalent cap for the underlying metric like CPU or IO) set for that pool.</span></span> <span data-ttu-id="0298c-174">Například 50 % využití každého tyto metriky označuje, že spotřeba konkrétní prostředků je při 50 % za limitu cap databáze pro tento prostředek ve fondu nadřazené.</span><span class="sxs-lookup"><span data-stu-id="0298c-174">For example, 50% utilization of any of these metrics indicates that the specific resource consumption is at 50% of the per database cap limit for that resource in the parent pool.</span></span>

<span data-ttu-id="0298c-175">Chcete-li získat metriky:</span><span class="sxs-lookup"><span data-stu-id="0298c-175">To retrieve the metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-to-an-elastic-pool-resource"></a><span data-ttu-id="0298c-176">Přidání oznámení na prostředek elastického fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-176">Add an alert to an elastic pool resource</span></span>
<span data-ttu-id="0298c-177">Pravidla výstrah můžete přidat do fondu elastické databáze k odeslání e-mailových oznámení nebo výstrah řetězce [adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) při elastický fond dotkne prahovou hodnotu využití, které jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="0298c-177">You can add alert rules to an elastic pool to send email notifications or alert strings to [URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when the elastic pool hits a utilization threshold that you set up.</span></span> <span data-ttu-id="0298c-178">Použijte rutinu Add-AzureRmMetricAlertRule.</span><span class="sxs-lookup"><span data-stu-id="0298c-178">Use the Add-AzureRmMetricAlertRule cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0298c-179">Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut.</span><span class="sxs-lookup"><span data-stu-id="0298c-179">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="0298c-180">Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="0298c-180">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="0298c-181">Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="0298c-181">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="0298c-182">Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.</span><span class="sxs-lookup"><span data-stu-id="0298c-182">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>
>

<span data-ttu-id="0298c-183">Tento příklad přidá výstrahy pro získávání upozornění při spotřebě eDTU fondu elastické databáze překročí určitou mez.</span><span class="sxs-lookup"><span data-stu-id="0298c-183">This example adds an alert for getting notified when an elastic pool’s eDTU consumption goes above certain threshold.</span></span>

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location =  '<location'                         # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

#$Target Resource ID
$ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# create a unique rule name
$alertName = $poolName + "- DTU consumption rule"

# Create an alert rule for DTU_consumption_percent
Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail
```

<span data-ttu-id="0298c-184">Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0298c-184">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="add-alerts-to-all-databases-in-an-elastic-pool"></a><span data-ttu-id="0298c-185">Přidat výstrahy pro všechny databáze v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-185">Add alerts to all databases in an elastic pool</span></span>
<span data-ttu-id="0298c-186">Můžete přidat pravidla výstrah pro všechny databáze v elastickém fondu k odeslání e-mailových oznámení nebo výstrah řetězce [adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) když prostředek dotkne prahovou hodnotu využití nastavit tak, že výstrahy.</span><span class="sxs-lookup"><span data-stu-id="0298c-186">You can add alert rules to all database in an elastic pool to send email notifications or alert strings to [URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when a resource hits a utilization threshold set up by the alert.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0298c-187">Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut.</span><span class="sxs-lookup"><span data-stu-id="0298c-187">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="0298c-188">Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="0298c-188">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="0298c-189">Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="0298c-189">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="0298c-190">Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.</span><span class="sxs-lookup"><span data-stu-id="0298c-190">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>

<span data-ttu-id="0298c-191">Tento příklad přidá výstrahu do všech databází v elastickém fondu pro získávání oznámeno, že databáze spotřeba DTU překročí určitou mez.</span><span class="sxs-lookup"><span data-stu-id="0298c-191">This example adds an alert to each of the databases in an elastic pool for getting notified when that database’s DTU consumption goes above certain threshold.</span></span>

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a><span data-ttu-id="0298c-192">Shromažďování a sledovat data použití prostředků v rámci více fondů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="0298c-192">Collect and monitor resource usage data across multiple pools in a subscription</span></span>
<span data-ttu-id="0298c-193">Pokud máte mnoho databází v odběru, je náročná samostatně monitorování jednotlivých elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-193">When you have many databases in a subscription, it is cumbersome to monitor each elastic pool separately.</span></span> <span data-ttu-id="0298c-194">Místo toho rutiny prostředí PowerShell databáze SQL a dotazů T-SQL můžete kombinovat shromažďovat data použití prostředků z více fondů a jejich databází pro sledování a analýza využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="0298c-194">Instead, SQL database PowerShell cmdlets and T-SQL queries can be combined to collect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage.</span></span> <span data-ttu-id="0298c-195">A [Ukázka implementace](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) sady prostředí PowerShell skripty lze nalézt v úložišti GitHub SQL Server – ukázky společně s dokumentací na co znamená a způsobu jeho použití.</span><span class="sxs-lookup"><span data-stu-id="0298c-195">A [sample implementation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) of such a set of powershell scripts can be found in the GitHub SQL Server samples repository along with documentation on what it does and how to use it.</span></span>

<span data-ttu-id="0298c-196">Tato ukázka implementace, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="0298c-196">To use this sample implementation, follow these steps.</span></span>

1. <span data-ttu-id="0298c-197">Stažení [skripty a dokumentaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span><span class="sxs-lookup"><span data-stu-id="0298c-197">Download the [scripts and documentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span></span>
2. <span data-ttu-id="0298c-198">Změňte skripty pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="0298c-198">Modify the scripts for your environment.</span></span> <span data-ttu-id="0298c-199">Zadejte jeden nebo více serverů, na kterých jsou hostované elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="0298c-199">Specify one or more servers on which elastic pools are hosted.</span></span>
3. <span data-ttu-id="0298c-200">Zadejte databázi telemetrie, kam mají být uloženy shromažďovat metriky.</span><span class="sxs-lookup"><span data-stu-id="0298c-200">Specify a telemetry database where the collected metrics are to be stored.</span></span>
4. <span data-ttu-id="0298c-201">Upravte skript tak, aby určete dobu trvání provádění tyto skripty.</span><span class="sxs-lookup"><span data-stu-id="0298c-201">Customize the script to specify the duration of the scripts' execution.</span></span>

<span data-ttu-id="0298c-202">Na vysoké úrovni skripty, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0298c-202">At a high level, the scripts do the following:</span></span>

* <span data-ttu-id="0298c-203">Zobrazí všechny servery v konkrétního předplatného Azure (nebo zadaný seznam serverů).</span><span class="sxs-lookup"><span data-stu-id="0298c-203">Enumerates all servers in a given Azure subscription (or a specified list of servers).</span></span>
* <span data-ttu-id="0298c-204">Spustí úlohu na pozadí pro každý server.</span><span class="sxs-lookup"><span data-stu-id="0298c-204">Runs a background job for each server.</span></span> <span data-ttu-id="0298c-205">Úloha běží ve smyčce v pravidelných intervalech a shromažďuje telemetrická data pro všechny fondy na serveru.</span><span class="sxs-lookup"><span data-stu-id="0298c-205">The job runs in a loop at regular intervals and collects telemetry data for all the pools in the server.</span></span> <span data-ttu-id="0298c-206">Potom načte shromážděná data do databáze zadaný telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0298c-206">It then loads the collected data into the specified telemetry database.</span></span>
* <span data-ttu-id="0298c-207">Zobrazí seznam databází v každém fondu ke shromažďování dat o využití prostředků databáze.</span><span class="sxs-lookup"><span data-stu-id="0298c-207">Enumerates a list of databases in each pool to collect the database resource usage data.</span></span> <span data-ttu-id="0298c-208">Potom načte shromážděná data do databáze telemetrie.</span><span class="sxs-lookup"><span data-stu-id="0298c-208">It then loads the collected data into the telemetry database.</span></span>

<span data-ttu-id="0298c-209">Shromažďovat metriky v databázi telemetrická data mohou být analyzovány tak, aby monitorovala elastické fondy a databází v ní.</span><span class="sxs-lookup"><span data-stu-id="0298c-209">The collected metrics in the telemetry database can be analyzed to monitor the health of elastic pools and the databases in it.</span></span> <span data-ttu-id="0298c-210">Skript se nainstaluje taky předem definovaná funkce hodnota tabulku (TVF) v databázi telemetrie pomohou agregace metriky pro zadané časové okno.</span><span class="sxs-lookup"><span data-stu-id="0298c-210">The script also installs a pre-defined Table-Value function (TVF) in the telemetry database to help aggregate the metrics for a specified time window.</span></span> <span data-ttu-id="0298c-211">Například funkci TVF výsledky lze zobrazit "top N elastické fondy s maximální počet jednotek eDTU využití v daném časovém období."</span><span class="sxs-lookup"><span data-stu-id="0298c-211">For example, results of the TVF can be used to show “top N elastic pools with the maximum eDTU utilization in a given time window.”</span></span> <span data-ttu-id="0298c-212">Při dotazování a analýze shromážděných dat, použijte analytické nástroje, například aplikace Excel nebo produktu Power BI.</span><span class="sxs-lookup"><span data-stu-id="0298c-212">Optionally, use analytic tools like Excel or Power BI to query and analyze the collected data.</span></span>

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a><span data-ttu-id="0298c-213">Příklad: načtení metriky spotřeby prostředků pro elastický fond a její databáze</span><span class="sxs-lookup"><span data-stu-id="0298c-213">Example: retrieve resource consumption metrics for an elastic pool and its databases</span></span>
<span data-ttu-id="0298c-214">Tento příklad načte spotřeba metriky pro daný elastický fond a všechny její databáze.</span><span class="sxs-lookup"><span data-stu-id="0298c-214">This example retrieves the consumption metrics for a given elastic pool and all its databases.</span></span> <span data-ttu-id="0298c-215">Shromážděná data je naformátován a zapisují do formátovaného souboru .csv.</span><span class="sxs-lookup"><span data-stu-id="0298c-215">Collected data is formatted and written to a .csv formatted file.</span></span> <span data-ttu-id="0298c-216">Soubor můžete procházet pomocí aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="0298c-216">The file can be browsed with Excel.</span></span>

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login to Azure account and select the subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for the specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct the pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format the metrics and output as .csv file using the following script block.
$command = {
param($metricList, $outputFile)

# Format metrics into a table.
$table = @()
foreach($metric in $metricList) {
   foreach($metricValue in $metric.MetricValues) {
      $sx = New-Object PSObject -Property @{
      Timestamp = $metricValue.Timestamp.ToString()
      MetricName = $metric.Name;
      Average = $metricValue.Average;
      ResourceID = $metric.ResourceId
   }$table = $table += $sx
   }
}

# Output the metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="0298c-217">Latence operací elastického fondu</span><span class="sxs-lookup"><span data-stu-id="0298c-217">Latency of elastic pool operations</span></span>
* <span data-ttu-id="0298c-218">Změna minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.</span><span class="sxs-lookup"><span data-stu-id="0298c-218">Changing the min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="0298c-219">Změna Edtu na fond, závisí na celkovém množství místo využité všechny databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-219">Changing the eDTUs per pool depends on the total amount of space used by all databases in the pool.</span></span> <span data-ttu-id="0298c-220">Změny průměrně trvají méně než 90 minut na každých 100 GB.</span><span class="sxs-lookup"><span data-stu-id="0298c-220">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="0298c-221">Například pokud celkové místo používá všechny databáze ve fondu je 200 GB, pak očekávané latence pro změnu fondu eDTU na fond je 3 hodiny nebo méně.</span><span class="sxs-lookup"><span data-stu-id="0298c-221">For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for changing the pool eDTU per pool is 3 hours or less.</span></span>



## <a name="next-steps"></a><span data-ttu-id="0298c-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0298c-222">Next steps</span></span>
* <span data-ttu-id="0298c-223">[Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md): Elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu.</span><span class="sxs-lookup"><span data-stu-id="0298c-223">[Create elastic jobs](sql-database-elastic-jobs-overview.md) Elastic jobs let you run T-SQL scripts against any number of databases in the pool.</span></span>
* <span data-ttu-id="0298c-224">V tématu [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): pomocí nástrojů elastické škálování, přejděte dat, dotazů, nebo vytvořit transakce.</span><span class="sxs-lookup"><span data-stu-id="0298c-224">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic tools to scale out, move data, query, or create transactions.</span></span>
