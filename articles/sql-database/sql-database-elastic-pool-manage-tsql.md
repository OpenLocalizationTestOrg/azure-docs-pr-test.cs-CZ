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
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a>Sledování a správě fondu elastické databáze pomocí jazyka Transact-SQL
Toto téma ukazuje, jak toomanage škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí jazyka Transact-SQL.  Můžete také vytvořit a spravovat služby Azure elastického fondu hello [portál Azure](https://portal.azure.com/), [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md). Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).


Použití hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) a [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) příkazy toocreate a přesun databáze do a z elastické fondy. můžete použít tyto příkazy musí existovat Hello elastického fondu. Tyto příkazy vliv pouze databáze. Hello nastavení vlastností fondu (například min a max Edtu) a vytvoření nových fondů nelze změnit pomocí příkazů T-SQL.

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Vytvoření databáze ve fondu v elastickém fondu
Pomocí příkazu CREATE DATABASE hello s hello SERVICE_OBJECTIVE možnost.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

Všechny databáze v elastickém fondu dědit hello úroveň služby hello elastický fond (Basic, Standard, Premium). 

## <a name="move-a-database-between-elastic-pools"></a>Přesun databáze mezi elastické fondy
Použijte příkaz ALTER DATABASE hello pomocí hello upravit a nastavení služby\_cíle možnost jako ELASTICKÁ\_fondu. Nastavit hello toohello název hello cílový fond.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Přesun databáze do pružného fondu
Použijte příkaz ALTER DATABASE hello pomocí hello upravit a nastavení služby\_cíle možnost jako ELASTIC_POOL. Nastavit hello toohello název hello cílový fond.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu elastické databáze
Použijte příkaz ALTER DATABASE hello a nastavte hello SERVICE_OBJECTIVE tooone úrovně výkonu hello (například S0 nebo S1).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Seznam databází v elastickém fondu
Použití hello [sys.database\_služby \_zobrazení cíle](https://msdn.microsoft.com/library/mt712619) toolist všechny hello databází v elastickém fondu. Přihlaste se toohello hlavní databáze tooquery hello zobrazení.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Získat data použití prostředků fondu elastické databáze
Použití hello [sys.elastic\_fondu \_prostředků \_zobrazení statistiky](https://msdn.microsoft.com/library/mt280062.aspx) Statistika využití prostředků tooexamine hello elastického fondu na logickém serveru. Přihlaste se toohello hlavní databáze tooquery hello zobrazení.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a>Získat využití prostředků pro databázi ve fondu
Použití hello [sys.dm\_ db\_ prostředků\_zobrazení statistiky](https://msdn.microsoft.com/library/dn800981.aspx) nebo [sys.resource \_zobrazení statistiky](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello prostředků využití statistiky databáze v elastickém fondu. Tento proces je podobný tooquerying využití prostředků pro jednu databázi.

## <a name="next-steps"></a>Další kroky
Po vytvoření fondu elastické databáze, můžete spravovat elastické databáze ve fondu hello vytvoření elastických úloh. Elastické úlohy usnadnění spouštění skriptů T-SQL pro libovolný počet databází ve fondu hello. Další informace najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md). 

V tématu [horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): použít tooscale nástroje elastické databáze out, přesun dat, dotazů, nebo vytvořit transakce.

