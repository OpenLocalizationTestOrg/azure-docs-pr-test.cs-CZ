---
title: "T-SQL: Správa fondu elastické databáze Azure SQL Database | Microsoft Docs"
description: "Použijte T-SQL toomanage fondu elastické databáze Azure SQL Database."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
ms.openlocfilehash: 666f131b2c88849a1a9ea83381bbc27548e93599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a><span data-ttu-id="26984-103">Sledování a správě fondu elastické databáze pomocí jazyka Transact-SQL</span><span class="sxs-lookup"><span data-stu-id="26984-103">Monitor and manage an elastic pool with Transact-SQL</span></span>
<span data-ttu-id="26984-104">Toto téma ukazuje, jak toomanage škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="26984-104">This topic shows you how toomanage scalable [elastic pools](sql-database-elastic-pool.md) with Transact-SQL.</span></span>  <span data-ttu-id="26984-105">Můžete také vytvořit a spravovat služby Azure elastického fondu hello [portál Azure](https://portal.azure.com/), [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="26984-105">You can also create and manage an Azure elastic pool hello [Azure portal](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="26984-106">Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="26984-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>


<span data-ttu-id="26984-107">Použití hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) a [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) příkazy toocreate a přesun databáze do a z elastické fondy.</span><span class="sxs-lookup"><span data-stu-id="26984-107">Use hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) and [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) commands toocreate and move databases into and out of elastic pools.</span></span> <span data-ttu-id="26984-108">můžete použít tyto příkazy musí existovat Hello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="26984-108">hello elastic pool must exist before you can use these commands.</span></span> <span data-ttu-id="26984-109">Tyto příkazy vliv pouze databáze.</span><span class="sxs-lookup"><span data-stu-id="26984-109">These commands affect only databases.</span></span> <span data-ttu-id="26984-110">Hello nastavení vlastností fondu (například min a max Edtu) a vytvoření nových fondů nelze změnit pomocí příkazů T-SQL.</span><span class="sxs-lookup"><span data-stu-id="26984-110">Creation of new pools and hello setting of pool properties (such as min and max eDTUs) cannot be changed with T-SQL commands.</span></span>

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="26984-111">Vytvoření databáze ve fondu v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="26984-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="26984-112">Pomocí příkazu CREATE DATABASE hello s hello SERVICE_OBJECTIVE možnost.</span><span class="sxs-lookup"><span data-stu-id="26984-112">Use hello CREATE DATABASE command with hello SERVICE_OBJECTIVE option.</span></span>   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

<span data-ttu-id="26984-113">Všechny databáze v elastickém fondu dědit hello úroveň služby hello elastický fond (Basic, Standard, Premium).</span><span class="sxs-lookup"><span data-stu-id="26984-113">All databases in an elastic pool inherit hello service tier of hello elastic pool (Basic, Standard, Premium).</span></span> 

## <a name="move-a-database-between-elastic-pools"></a><span data-ttu-id="26984-114">Přesun databáze mezi elastické fondy</span><span class="sxs-lookup"><span data-stu-id="26984-114">Move a database between elastic pools</span></span>
<span data-ttu-id="26984-115">Použijte příkaz ALTER DATABASE hello pomocí hello upravit a nastavení služby\_cíle možnost jako ELASTICKÁ\_fondu.</span><span class="sxs-lookup"><span data-stu-id="26984-115">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC\_POOL.</span></span> <span data-ttu-id="26984-116">Nastavit hello toohello název hello cílový fond.</span><span class="sxs-lookup"><span data-stu-id="26984-116">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="26984-117">Přesun databáze do pružného fondu</span><span class="sxs-lookup"><span data-stu-id="26984-117">Move a database into an elastic pool</span></span>
<span data-ttu-id="26984-118">Použijte příkaz ALTER DATABASE hello pomocí hello upravit a nastavení služby\_cíle možnost jako ELASTIC_POOL.</span><span class="sxs-lookup"><span data-stu-id="26984-118">Use hello ALTER DATABASE command with hello MODIFY and set SERVICE\_OBJECTIVE option as ELASTIC_POOL.</span></span> <span data-ttu-id="26984-119">Nastavit hello toohello název hello cílový fond.</span><span class="sxs-lookup"><span data-stu-id="26984-119">Set hello name toohello name of hello target pool.</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="26984-120">Přesunutí databáze z fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="26984-120">Move a database out of an elastic pool</span></span>
<span data-ttu-id="26984-121">Použijte příkaz ALTER DATABASE hello a nastavte hello SERVICE_OBJECTIVE tooone úrovně výkonu hello (například S0 nebo S1).</span><span class="sxs-lookup"><span data-stu-id="26984-121">Use hello ALTER DATABASE command and set hello SERVICE_OBJECTIVE tooone of hello performance levels (such as S0 or S1).</span></span>

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a><span data-ttu-id="26984-122">Seznam databází v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="26984-122">List databases in an elastic pool</span></span>
<span data-ttu-id="26984-123">Použití hello [sys.database\_služby \_zobrazení cíle](https://msdn.microsoft.com/library/mt712619) toolist všechny hello databází v elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="26984-123">Use hello [sys.database\_service \_objectives view](https://msdn.microsoft.com/library/mt712619) toolist all hello databases in an elastic pool.</span></span> <span data-ttu-id="26984-124">Přihlaste se toohello hlavní databáze tooquery hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="26984-124">Log in toohello master database tooquery hello view.</span></span>

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="26984-125">Získat data použití prostředků fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="26984-125">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="26984-126">Použití hello [sys.elastic\_fondu \_prostředků \_zobrazení statistiky](https://msdn.microsoft.com/library/mt280062.aspx) Statistika využití prostředků tooexamine hello elastického fondu na logickém serveru.</span><span class="sxs-lookup"><span data-stu-id="26984-126">Use hello [sys.elastic\_pool \_resource \_stats view](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello resource usage statistics of an elastic pool on a logical server.</span></span> <span data-ttu-id="26984-127">Přihlaste se toohello hlavní databáze tooquery hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="26984-127">Log in toohello master database tooquery hello view.</span></span>

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a><span data-ttu-id="26984-128">Získat využití prostředků pro databázi ve fondu</span><span class="sxs-lookup"><span data-stu-id="26984-128">Get resource usage for a pooled database</span></span>
<span data-ttu-id="26984-129">Použití hello [sys.dm\_ db\_ prostředků\_zobrazení statistiky](https://msdn.microsoft.com/library/dn800981.aspx) nebo [sys.resource \_zobrazení statistiky](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello prostředků využití statistiky databáze v elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="26984-129">Use hello [sys.dm\_ db\_ resource\_stats view](https://msdn.microsoft.com/library/dn800981.aspx) or [sys.resource \_stats view](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello resource usage statistics of a database in an elastic pool.</span></span> <span data-ttu-id="26984-130">Tento proces je podobný tooquerying využití prostředků pro jednu databázi.</span><span class="sxs-lookup"><span data-stu-id="26984-130">This process is similar tooquerying resource usage for a single database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26984-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26984-131">Next steps</span></span>
<span data-ttu-id="26984-132">Po vytvoření fondu elastické databáze, můžete spravovat elastické databáze ve fondu hello vytvoření elastických úloh.</span><span class="sxs-lookup"><span data-stu-id="26984-132">After creating an elastic pool, you can manage elastic databases in hello pool by creating elastic jobs.</span></span> <span data-ttu-id="26984-133">Elastické úlohy usnadnění spouštění skriptů T-SQL pro libovolný počet databází ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="26984-133">Elastic jobs facilitate running T-SQL scripts against any number of databases in hello pool.</span></span> <span data-ttu-id="26984-134">Další informace najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="26984-134">For more information, see [Elastic database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

<span data-ttu-id="26984-135">V tématu [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): použít tooscale nástroje elastické databáze out, přesun dat, dotazů, nebo vytvořit transakce.</span><span class="sxs-lookup"><span data-stu-id="26984-135">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic database tools tooscale out, move data, query, or create transactions.</span></span>

