---
title: "Prostředí PowerShell: Vytvoření a správa fondu elastické databáze Azure SQL | Microsoft Docs"
description: "Zjistěte, jak toouse prostředí PowerShell toomanage fondu elastické databáze."
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
ms.openlocfilehash: 92de2a4b243dcc74502064e9d2c31682691753d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a><span data-ttu-id="93c08-103">Vytvoření a správa fondu elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="93c08-103">Create and manage an elastic pool with PowerShell</span></span>
<span data-ttu-id="93c08-104">Toto téma ukazuje, jak toocreate a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="93c08-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with PowerShell.</span></span>  <span data-ttu-id="93c08-105">Můžete také vytvořit a spravovat Azure elastickém fondu pomocí hello [portál Azure](https://portal.azure.com/), REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="93c08-105">You can also create and manage an Azure elastic pool using hello [Azure portal](https://portal.azure.com/), REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="93c08-106">Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="93c08-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a><span data-ttu-id="93c08-107">Vytvoření elastického fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-107">Create an elastic pool</span></span>
<span data-ttu-id="93c08-108">Hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) rutina vytvoří fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="93c08-108">hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet creates an elastic pool.</span></span> <span data-ttu-id="93c08-109">Hello hodnoty eDTU na fond a minimální a maximální Dtu jsou omezeny hodnota vrstvy služby hello (Basic, Standard, Premium nebo Premium RS).</span><span class="sxs-lookup"><span data-stu-id="93c08-109">hello values for eDTU per pool, min, and max DTUs are constrained by hello service tier value (Basic, Standard, Premium, or Premium RS).</span></span> <span data-ttu-id="93c08-110">V tématu [omezení eDTU a úložiště pro elastické fondy a databází ve fondu](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span><span class="sxs-lookup"><span data-stu-id="93c08-110">See [eDTU and storage limits for elastic pools and pooled databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span></span>

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="93c08-111">Vytvoření databáze ve fondu v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="93c08-112">Použití hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) rutiny a sadu hello **ElasticPoolName** parametr toohello cílový fond.</span><span class="sxs-lookup"><span data-stu-id="93c08-112">Use hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet and set hello **ElasticPoolName** parameter toohello target pool.</span></span> <span data-ttu-id="93c08-113">toomove existující databáze do pružného fondu, najdete v části [přesun databáze do pružného fondu](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span><span class="sxs-lookup"><span data-stu-id="93c08-113">toomove an existing database into an elastic pool, see [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span></span>

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a><span data-ttu-id="93c08-114">Celý skript</span><span class="sxs-lookup"><span data-stu-id="93c08-114">Complete script</span></span>
<span data-ttu-id="93c08-115">Tento skript vytvoří skupinu prostředků Azure a serverem.</span><span class="sxs-lookup"><span data-stu-id="93c08-115">This script creates an Azure resource group and a server.</span></span> <span data-ttu-id="93c08-116">Po zobrazení výzvy zadejte jméno a heslo správce pro nový server hello (nikoli vaše přihlašovací údaje Azure).</span><span class="sxs-lookup"><span data-stu-id="93c08-116">When prompted, supply an administrator username and password for hello new server (not your Azure credentials).</span></span>

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

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a><span data-ttu-id="93c08-117">Vytvoření fondu elastické databáze a přidání více databází ve fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-117">Create an elastic pool and add multiple pooled databases</span></span>
<span data-ttu-id="93c08-118">Vytváření mnoho databází v elastickém fondu může trvat dobu, pokud se provádí pomocí hello portálu nebo rutiny prostředí PowerShell, který současně vytvořit pouze jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="93c08-118">Creation of many databases in an elastic pool can take time when done using hello portal or PowerShell cmdlets that create only a single database at a time.</span></span> <span data-ttu-id="93c08-119">Vytvoření tooautomate do pružného fondu, najdete v části [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span><span class="sxs-lookup"><span data-stu-id="93c08-119">tooautomate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="93c08-120">Přesun databáze do pružného fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-120">Move a database into an elastic pool</span></span>
<span data-ttu-id="93c08-121">Přesouváte databázi do nebo z fondu elastické databáze s hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span><span class="sxs-lookup"><span data-stu-id="93c08-121">You can move a database into or out of an elastic pool with hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span></span>

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="93c08-122">Změňte nastavení výkonu elastického fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-122">Change performance settings of an elastic pool</span></span>
<span data-ttu-id="93c08-123">Když ke snížení výkonu, můžete změnit nastavení hello hello fondu tooaccommodate růstu.</span><span class="sxs-lookup"><span data-stu-id="93c08-123">When performance suffers, you can change hello settings of hello pool tooaccommodate growth.</span></span> <span data-ttu-id="93c08-124">Použití hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny.</span><span class="sxs-lookup"><span data-stu-id="93c08-124">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span></span> <span data-ttu-id="93c08-125">Nastavit hello - Dtu parametr toohello Edtu na fond.</span><span class="sxs-lookup"><span data-stu-id="93c08-125">Set hello -Dtu parameter toohello eDTUs per pool.</span></span> <span data-ttu-id="93c08-126">V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="93c08-126">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a><span data-ttu-id="93c08-127">Změnit hello limit úložiště fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="93c08-127">Change hello storage limit for an elastic pool</span></span>

<span data-ttu-id="93c08-128">Použití hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) rutiny tooset hello _- StorageMB_ parametr.</span><span class="sxs-lookup"><span data-stu-id="93c08-128">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet tooset hello _-StorageMB_ parameter.</span></span> <span data-ttu-id="93c08-129">Zadejte hello limit úložiště v MB (například 2097152 nastaví hello úložiště limit too2 TB).</span><span class="sxs-lookup"><span data-stu-id="93c08-129">Provide hello storage limit in MB (for example, 2097152 sets hello storage limit too2 TB).</span></span> <span data-ttu-id="93c08-130">V tématu [omezení eDTU a úložiště](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) pro možné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="93c08-130">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c08-131">Hello výchozí maximální dat úložiště na každý fond pro fondy Premium s 1 500 Edtu nebo více je 750 GB.</span><span class="sxs-lookup"><span data-stu-id="93c08-131">hello default max data storage per pool for Premium pools with 1500 eDTUs or more is 750 GB.</span></span> <span data-ttu-id="93c08-132">tooobtain hello vyšší _maximální velikost úložiště dat na každý fond_, musí být explicitně nastaveny hello limit úložiště.</span><span class="sxs-lookup"><span data-stu-id="93c08-132">tooobtain hello higher _max data storage size per pool_, hello storage limit must be explicitly set.</span></span> <span data-ttu-id="93c08-133">Premium fondy s více než 750 GB úložiště je aktuálně ve verzi public preview v hello následující oblasti: východní USA 2, západ USA, Virginia nám verze pro státní správu, západní Evropa, Německo centrální, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada.</span><span class="sxs-lookup"><span data-stu-id="93c08-133">Premium pools with more than 750 GB of storage is currently in public preview in hello following regions: East US 2, West US, US Gov Virginia, West Europe, Germany Central, Southeast Asia, Japan East, Australia East, Canada Central, and Canada East.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a><span data-ttu-id="93c08-134">Získat stav hello operací fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-134">Get hello status of pool operations</span></span>
<span data-ttu-id="93c08-135">Vytvoření fondu elastické databáze může trvat dobu.</span><span class="sxs-lookup"><span data-stu-id="93c08-135">Creating an elastic pool can take time.</span></span> <span data-ttu-id="93c08-136">Stav hello tootrack operací fondu, včetně vytváření a aktualizace, použijte hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) rutiny.</span><span class="sxs-lookup"><span data-stu-id="93c08-136">tootrack hello status of pool operations including creation and updates, use hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a><span data-ttu-id="93c08-137">Získat stav hello přesun databáze do a z fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="93c08-137">Get hello status of moving a database into and out of an elastic pool</span></span>
<span data-ttu-id="93c08-138">Přesunutí databáze může trvat dobu.</span><span class="sxs-lookup"><span data-stu-id="93c08-138">Moving a database can take time.</span></span> <span data-ttu-id="93c08-139">Sledování stavu přesunutí pomocí hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) rutiny.</span><span class="sxs-lookup"><span data-stu-id="93c08-139">Track a move status using hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="93c08-140">Získat data použití prostředků fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="93c08-140">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="93c08-141">Metriky, které mohou být načteny jako procento hello limit fondu prostředků:</span><span class="sxs-lookup"><span data-stu-id="93c08-141">Metrics that can be retrieved as a percentage of hello resource pool limit:</span></span>

| <span data-ttu-id="93c08-142">Název metriky</span><span class="sxs-lookup"><span data-stu-id="93c08-142">Metric name</span></span> | <span data-ttu-id="93c08-143">Popis</span><span class="sxs-lookup"><span data-stu-id="93c08-143">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="93c08-144">Procesor\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-144">cpu\_percent</span></span> |<span data-ttu-id="93c08-145">Průměr výpočetní využití v procentech hello limit hello fondu.</span><span class="sxs-lookup"><span data-stu-id="93c08-145">Average compute utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-146">fyzické\_data\_číst\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-146">physical\_data\_read\_percent</span></span> |<span data-ttu-id="93c08-147">Průměrné využití vstupně-výstupních operací v procentech na hello limitu hello fondu.</span><span class="sxs-lookup"><span data-stu-id="93c08-147">Average I/O utilization in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-148">protokol\_zápisu\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-148">log\_write\_percent</span></span> |<span data-ttu-id="93c08-149">Průměr zápisu využití prostředků v procentech hello limit hello fondu.</span><span class="sxs-lookup"><span data-stu-id="93c08-149">Average write resource utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-150">DTU\_spotřeba\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-150">DTU\_consumption\_percent</span></span> |<span data-ttu-id="93c08-151">Průměrná eDTU využití v procentech omezení eDTU pro fond hello</span><span class="sxs-lookup"><span data-stu-id="93c08-151">Average eDTU utilization in percentage of eDTU limit for hello pool</span></span> |
| <span data-ttu-id="93c08-152">úložiště\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-152">storage\_percent</span></span> |<span data-ttu-id="93c08-153">Průměrné využití úložiště v procentech hello limit úložiště fondu hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-153">Average storage utilization in percentage of hello storage limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-154">pracovníci\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-154">workers\_percent</span></span> |<span data-ttu-id="93c08-155">Maximální souběžných pracovních procesů (počet požadavků) v procentech na hello limitu hello fondu.</span><span class="sxs-lookup"><span data-stu-id="93c08-155">Maximum concurrent workers (requests) in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-156">relace\_procent</span><span class="sxs-lookup"><span data-stu-id="93c08-156">sessions\_percent</span></span> |<span data-ttu-id="93c08-157">Maximální počet souběžných relací v procentech na hello limitu hello fondu.</span><span class="sxs-lookup"><span data-stu-id="93c08-157">Maximum concurrent sessions in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="93c08-158">eDTU_limit</span><span class="sxs-lookup"><span data-stu-id="93c08-158">eDTU_limit</span></span> |<span data-ttu-id="93c08-159">Aktuální maximální elastický fond DTU nastavení pro tento elastický fond během tohoto intervalu.</span><span class="sxs-lookup"><span data-stu-id="93c08-159">Current max elastic pool DTU setting for this elastic pool during this interval.</span></span> |
| <span data-ttu-id="93c08-160">úložiště\_limit</span><span class="sxs-lookup"><span data-stu-id="93c08-160">storage\_limit</span></span> |<span data-ttu-id="93c08-161">Aktuální maximální elastický fond úložiště limit nastavení pro toto elastického fondu v megabajtech během tohoto intervalu.</span><span class="sxs-lookup"><span data-stu-id="93c08-161">Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.</span></span> |
| <span data-ttu-id="93c08-162">eDTU\_použít</span><span class="sxs-lookup"><span data-stu-id="93c08-162">eDTU\_used</span></span> |<span data-ttu-id="93c08-163">Průměrná Edtu používané hello fondu v tomto intervalu.</span><span class="sxs-lookup"><span data-stu-id="93c08-163">Average eDTUs used by hello pool in this interval.</span></span> |
| <span data-ttu-id="93c08-164">úložiště\_použít</span><span class="sxs-lookup"><span data-stu-id="93c08-164">storage\_used</span></span> |<span data-ttu-id="93c08-165">Průměrná úložiště používané hello fondu v tomto intervalu v bajtech</span><span class="sxs-lookup"><span data-stu-id="93c08-165">Average storage used by hello pool in this interval in bytes</span></span> |

<span data-ttu-id="93c08-166">**Metriky členitosti nebo období:**</span><span class="sxs-lookup"><span data-stu-id="93c08-166">**Metrics granularity/retention periods:**</span></span>

* <span data-ttu-id="93c08-167">Data jsou vrácena v členitosti 5 minut.</span><span class="sxs-lookup"><span data-stu-id="93c08-167">Data is returned at 5-minute granularity.</span></span>  
* <span data-ttu-id="93c08-168">Doba uchování dat je 35 dnů.</span><span class="sxs-lookup"><span data-stu-id="93c08-168">Data retention is 35 days.</span></span>  

<span data-ttu-id="93c08-169">Tato rutina a rozhraní API omezí hello počet řádků, které mohou být načteny v jedno volání too1000 řádků (o 3 dny dat ve členitosti 5 minut).</span><span class="sxs-lookup"><span data-stu-id="93c08-169">This cmdlet and API limits hello number of rows that can be retrieved in one call too1000 rows (about 3 days of data at 5-minute granularity).</span></span> <span data-ttu-id="93c08-170">Tento příkaz lze volat vícekrát s jinou počáteční nebo koncové časové intervaly tooretrieve více dat, ale</span><span class="sxs-lookup"><span data-stu-id="93c08-170">But this command can be called multiple times with different start/end time intervals tooretrieve more data</span></span>

<span data-ttu-id="93c08-171">Metrika tooretrieve hello:</span><span class="sxs-lookup"><span data-stu-id="93c08-171">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a><span data-ttu-id="93c08-172">Získat data použití prostředků pro databáze v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-172">Get resource usage data for a database in an elastic pool</span></span>
<span data-ttu-id="93c08-173">Tato rozhraní API jsou hello stejná hodnota jako hello rozhraní API používaná pro sledování využití prostředků hello služby jedné databáze, s výjimkou hello následující sémantického rozdíl: metriky načíst jsou vyjádřené jako procentní podíl hello za Edtu max databáze (nebo ekvivalentní zakončení pro hello základní metrika jako procesoru nebo vstupně-výstupní operace) nastavte pro tento fond.</span><span class="sxs-lookup"><span data-stu-id="93c08-173">These APIs are hello same as hello APIs used for monitoring hello resource utilization of a single database, except for hello following semantic difference: metrics retrieved are expressed as a percentage of hello per database max eDTUs (or equivalent cap for hello underlying metric like CPU or IO) set for that pool.</span></span> <span data-ttu-id="93c08-174">Například 50 % využití každého tyto metriky označuje, že spotřeba konkrétní prostředků hello je na 50 % hello za limitu cap databáze pro tento prostředek ve fondu nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-174">For example, 50% utilization of any of these metrics indicates that hello specific resource consumption is at 50% of hello per database cap limit for that resource in hello parent pool.</span></span>

<span data-ttu-id="93c08-175">Metrika tooretrieve hello:</span><span class="sxs-lookup"><span data-stu-id="93c08-175">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="93c08-176">Přidat prostředek výstrahy tooan elastického fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-176">Add an alert tooan elastic pool resource</span></span>
<span data-ttu-id="93c08-177">Můžete přidat pravidla výstrah tooan elastický fond toosend e-mailová oznámení nebo upozornit řetězce příliš[adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) při elastického fondu hello dotkne prahovou hodnotu využití, které jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="93c08-177">You can add alert rules tooan elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when hello elastic pool hits a utilization threshold that you set up.</span></span> <span data-ttu-id="93c08-178">Pomocí rutiny Add-AzureRmMetricAlertRule hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-178">Use hello Add-AzureRmMetricAlertRule cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c08-179">Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut.</span><span class="sxs-lookup"><span data-stu-id="93c08-179">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="93c08-180">Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="93c08-180">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="93c08-181">Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="93c08-181">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="93c08-182">Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.</span><span class="sxs-lookup"><span data-stu-id="93c08-182">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>
>

<span data-ttu-id="93c08-183">Tento příklad přidá výstrahy pro získávání upozornění při spotřebě eDTU fondu elastické databáze překročí určitou mez.</span><span class="sxs-lookup"><span data-stu-id="93c08-183">This example adds an alert for getting notified when an elastic pool’s eDTU consumption goes above certain threshold.</span></span>

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

<span data-ttu-id="93c08-184">Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="93c08-184">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a><span data-ttu-id="93c08-185">Přidat výstrahy tooall databází v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-185">Add alerts tooall databases in an elastic pool</span></span>
<span data-ttu-id="93c08-186">Můžete přidat pravidla výstrah e-mailová oznámení tooall databáze v toosend elastického fondu nebo výstrahy řetězce příliš[adresy URL koncových bodů](https://msdn.microsoft.com/library/mt718036.aspx) když prostředek dotkne prahovou hodnotu využití nastavit tak, že výstraha hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-186">You can add alert rules tooall database in an elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when a resource hits a utilization threshold set up by hello alert.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93c08-187">Monitorování pro elastické fondy využití prostředků má prodlení alespoň 5 minut.</span><span class="sxs-lookup"><span data-stu-id="93c08-187">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="93c08-188">Nastavení výstrah menší než 10 minut pro elastické fondy není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="93c08-188">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="93c08-189">Nastavit všechny výstrahy pro elastické fondy s dobou (parametr s názvem "-velikost_okna" v rozhraní API prostředí PowerShell) menší než 10 minut nemusí aktivovat.</span><span class="sxs-lookup"><span data-stu-id="93c08-189">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="93c08-190">Ujistěte se, že všechny výstrahy definujete pro elastické fondy použít dobu 10 minut (WindowSize) nebo víc.</span><span class="sxs-lookup"><span data-stu-id="93c08-190">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>

<span data-ttu-id="93c08-191">Tento příklad přidá výstrahy tooeach hello databází v elastickém fondu pro získávání oznámeno, že databáze spotřeba DTU překročí určitou mez.</span><span class="sxs-lookup"><span data-stu-id="93c08-191">This example adds an alert tooeach of hello databases in an elastic pool for getting notified when that database’s DTU consumption goes above certain threshold.</span></span>

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop hello alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a><span data-ttu-id="93c08-192">Shromažďování a sledovat data použití prostředků v rámci více fondů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="93c08-192">Collect and monitor resource usage data across multiple pools in a subscription</span></span>
<span data-ttu-id="93c08-193">Pokud máte mnoho databází v odběru, je náročná toomonitor každý elastického fondu samostatně.</span><span class="sxs-lookup"><span data-stu-id="93c08-193">When you have many databases in a subscription, it is cumbersome toomonitor each elastic pool separately.</span></span> <span data-ttu-id="93c08-194">Místo toho rutiny prostředí PowerShell databáze SQL a dotazů T-SQL může být kombinované toocollect data použití prostředků z více fondů a jejich databází pro sledování a analýza využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="93c08-194">Instead, SQL database PowerShell cmdlets and T-SQL queries can be combined toocollect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage.</span></span> <span data-ttu-id="93c08-195">A [Ukázka implementace](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) sady prostředí PowerShell skripty lze nalézt v hello Githubu SQL Server – ukázky úložiště společně s dokumentací na co dělá a jak toouse ho.</span><span class="sxs-lookup"><span data-stu-id="93c08-195">A [sample implementation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) of such a set of powershell scripts can be found in hello GitHub SQL Server samples repository along with documentation on what it does and how toouse it.</span></span>

<span data-ttu-id="93c08-196">toouse Tato ukázka implementace, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="93c08-196">toouse this sample implementation, follow these steps.</span></span>

1. <span data-ttu-id="93c08-197">Stáhnout hello [skripty a dokumentaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span><span class="sxs-lookup"><span data-stu-id="93c08-197">Download hello [scripts and documentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span></span>
2. <span data-ttu-id="93c08-198">Upravte hello skripty pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="93c08-198">Modify hello scripts for your environment.</span></span> <span data-ttu-id="93c08-199">Zadejte jeden nebo více serverů, na kterých jsou hostované elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="93c08-199">Specify one or more servers on which elastic pools are hosted.</span></span>
3. <span data-ttu-id="93c08-200">Zadejte databázi telemetrie, kde hello shromažďovat metriky jsou toobe uložené.</span><span class="sxs-lookup"><span data-stu-id="93c08-200">Specify a telemetry database where hello collected metrics are toobe stored.</span></span>
4. <span data-ttu-id="93c08-201">Přizpůsobte hello skriptu toospecify hello trvání provádění hello skriptů.</span><span class="sxs-lookup"><span data-stu-id="93c08-201">Customize hello script toospecify hello duration of hello scripts' execution.</span></span>

<span data-ttu-id="93c08-202">Na vysoké úrovni skripty hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="93c08-202">At a high level, hello scripts do hello following:</span></span>

* <span data-ttu-id="93c08-203">Zobrazí všechny servery v konkrétního předplatného Azure (nebo zadaný seznam serverů).</span><span class="sxs-lookup"><span data-stu-id="93c08-203">Enumerates all servers in a given Azure subscription (or a specified list of servers).</span></span>
* <span data-ttu-id="93c08-204">Spustí úlohu na pozadí pro každý server.</span><span class="sxs-lookup"><span data-stu-id="93c08-204">Runs a background job for each server.</span></span> <span data-ttu-id="93c08-205">Hello úloha běží ve smyčce v pravidelných intervalech a shromažďuje telemetrická data pro všechny fondy hello hello serveru.</span><span class="sxs-lookup"><span data-stu-id="93c08-205">hello job runs in a loop at regular intervals and collects telemetry data for all hello pools in hello server.</span></span> <span data-ttu-id="93c08-206">Potom načte hello shromažďovaných dat do databáze zadaný telemetrie hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-206">It then loads hello collected data into hello specified telemetry database.</span></span>
* <span data-ttu-id="93c08-207">Zobrazí seznam databází v každém fondu toocollect hello databáze data použití prostředků.</span><span class="sxs-lookup"><span data-stu-id="93c08-207">Enumerates a list of databases in each pool toocollect hello database resource usage data.</span></span> <span data-ttu-id="93c08-208">Potom načte hello shromažďovaných dat do databáze telemetrie hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-208">It then loads hello collected data into hello telemetry database.</span></span>

<span data-ttu-id="93c08-209">Hello shromažďovat metriky v hello telemetrie databáze může být analyzovaných toomonitor hello stavu elastické fondy a hello databází v ní.</span><span class="sxs-lookup"><span data-stu-id="93c08-209">hello collected metrics in hello telemetry database can be analyzed toomonitor hello health of elastic pools and hello databases in it.</span></span> <span data-ttu-id="93c08-210">skript Hello nainstaluje taky předem definovaná funkce hodnota tabulku (TVF) v hello telemetrie databáze toohelp agregační hello metriky pro zadané časové okno.</span><span class="sxs-lookup"><span data-stu-id="93c08-210">hello script also installs a pre-defined Table-Value function (TVF) in hello telemetry database toohelp aggregate hello metrics for a specified time window.</span></span> <span data-ttu-id="93c08-211">Například může být výsledky hello TVF používá tooshow "hlavních elastické fondy s hello maximální počet jednotek eDTU využití v daném časovém období."</span><span class="sxs-lookup"><span data-stu-id="93c08-211">For example, results of hello TVF can be used tooshow “top N elastic pools with hello maximum eDTU utilization in a given time window.”</span></span> <span data-ttu-id="93c08-212">Volitelně můžete použít analytické nástroje, například aplikace Excel nebo produktu Power BI tooquery a analyzovat hello shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="93c08-212">Optionally, use analytic tools like Excel or Power BI tooquery and analyze hello collected data.</span></span>

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a><span data-ttu-id="93c08-213">Příklad: načtení metriky spotřeby prostředků pro elastický fond a její databáze</span><span class="sxs-lookup"><span data-stu-id="93c08-213">Example: retrieve resource consumption metrics for an elastic pool and its databases</span></span>
<span data-ttu-id="93c08-214">Tento příklad načte hello spotřeba metriky pro daný elastický fond a všechny její databáze.</span><span class="sxs-lookup"><span data-stu-id="93c08-214">This example retrieves hello consumption metrics for a given elastic pool and all its databases.</span></span> <span data-ttu-id="93c08-215">Shromážděná data je naformátován a zapisovat formátovaného souboru .csv tooa.</span><span class="sxs-lookup"><span data-stu-id="93c08-215">Collected data is formatted and written tooa .csv formatted file.</span></span> <span data-ttu-id="93c08-216">soubor Hello lze procházet pomocí aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="93c08-216">hello file can be browsed with Excel.</span></span>

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login tooAzure account and select hello subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for hello specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct hello pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format hello metrics and output as .csv file using hello following script block.
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

# Output hello metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="93c08-217">Latence operací elastického fondu</span><span class="sxs-lookup"><span data-stu-id="93c08-217">Latency of elastic pool operations</span></span>
* <span data-ttu-id="93c08-218">Změna hello minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.</span><span class="sxs-lookup"><span data-stu-id="93c08-218">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="93c08-219">Změna hello Edtu na fond závisí na celkovém množství místo využité všechny databáze ve fondu hello hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-219">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="93c08-220">Změny průměrně trvají méně než 90 minut na každých 100 GB.</span><span class="sxs-lookup"><span data-stu-id="93c08-220">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="93c08-221">Například pokud celkové místo hello používá všechny databáze ve fondu hello je 200 GB, pak hello očekává, že latence pro změnu eDTU fondu hello na fondu je 3 hodiny nebo méně.</span><span class="sxs-lookup"><span data-stu-id="93c08-221">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>



## <a name="next-steps"></a><span data-ttu-id="93c08-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93c08-222">Next steps</span></span>
* <span data-ttu-id="93c08-223">[Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md) elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="93c08-223">[Create elastic jobs](sql-database-elastic-jobs-overview.md) Elastic jobs let you run T-SQL scripts against any number of databases in hello pool.</span></span>
* <span data-ttu-id="93c08-224">Najdete v části [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): pomocí nástroje elastické tooscale out, přesun dat, dotazů, nebo vytvořit transakce.</span><span class="sxs-lookup"><span data-stu-id="93c08-224">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic tools tooscale out, move data, query, or create transactions.</span></span>
