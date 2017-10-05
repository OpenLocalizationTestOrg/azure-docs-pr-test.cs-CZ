---
title: "Konfigurace geografická replikace pro Azure SQL Database pomocí jazyka Transact-SQL | Microsoft Docs"
description: "Konfigurace geografická replikace pro Azure SQL Database pomocí jazyka Transact-SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d94d89a6-3234-46c5-8279-5eb8daad10ac
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: carlrab
ms.openlocfilehash: e5011c1c67e051616d53621b72e46ba894ca3c02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurace aktivní geografickou replikací pro Azure SQL Database pomocí jazyka Transact-SQL

Tento článek ukazuje, jak nakonfigurovat aktivní geografickou replikací pro Azure SQL Database pomocí jazyka Transact-SQL.

Chcete-li zahájit převzetí služeb při selhání pomocí jazyka Transact-SQL, přečtěte si téma [zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí jazyka Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

> [!NOTE]
> Pokud používáte active geografická replikace (čitelný sekundární repliky) pro zotavení po havárii byste měli nakonfigurovat skupinu převzetí služeb při selhání pro všechny databáze v aplikaci povolit automatické a transparentní převzetí služeb při selhání. Tato funkce je ve verzi preview. Další informace najdete v části [skupiny automatické převzetí služeb při selhání a geografická replikace](sql-database-geo-replication-overview.md).
> 
> 

Konfigurace aktivní geografickou replikaci pomocí jazyka Transact-SQL, budete potřebovat následující:

* Předplatné Azure
* Logický server Azure SQL Database <MyLocalServer> a SQL database <MyDB> -primární databázi, kterou chcete replikovat
* Jeden nebo více logických Azure SQL Database serverů < MySecondaryServer(n) > - logické servery, které budou partnerské servery, ve kterých se vytvoří sekundární databáze
* Přihlašovací jméno, které je DBManager na primárním
* Mít db_ownership místní databáze, které budete geografická replikace
* Být DBManager na partnerské servery, na kterou budete konfigurovat geografická replikace
* Nejnovější verze systému SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="add-secondary-database"></a>Přidání sekundární databáze
Můžete použít **ALTER DATABASE** příkaz Vytvořit geograficky replikované sekundární databáze na serveru partnera. Spusťte tento příkaz v hlavní databázi systému server obsahující databáze, které se mají replikovat. Geograficky replikované databáze ("primární databáze") bude mít stejný název jako databázi právě replikován a bude ve výchozím nastavení, mají stejnou úroveň služby jako primární databáze. Sekundární databáze může být čitelný nebo bez možnosti čtení a může být jedné databáze nebo v elastickém fondu. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).
Jakmile sekundární databáze se vytvoří a nasadí, data zahájíte asynchronně replikace z primární databáze. Níže uvedené kroky popisují postup konfigurace geografická replikace pomocí nástroje Management Studio. Postup vytvoření sekundární databáze bez možnosti čtení, které lze načíst, buď jako jednu databázi, nebo v elastickém fondu, jsou k dispozici.

> [!NOTE]
> Příkaz se nezdaří, pokud se databáze nachází na zadaný partnerský server se stejným názvem jako primární databáze.
> 

### <a name="add-readable-secondary-single-database"></a>Přidat čitelný sekundární (jedné databáze)
Pomocí následujících kroků můžete vytvořit čitelný sekundární jako jednu databázi.

1. V Management Studio se připojte ke svému logickému serveru Azure SQL Database.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující **ALTER DATABASE** příkaz, aby místní databázi do geografická replikace primární s čitelnou sekundární databází na sekundárním serveru.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);
4. Kliknutím na **Spustit** dotaz spustíte.

### <a name="add-readable-secondary-elastic-pool"></a>Přidat čitelný sekundární (elastický fond)
Pomocí následujících kroků můžete vytvořit čitelný sekundární v elastickém fondu.

1. V Management Studio se připojte ke svému logickému serveru Azure SQL Database.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující **ALTER DATABASE** příkaz, aby místní databázi do geografická replikace primární s čitelnou sekundární databází na sekundárním serveru v elastickém fondu.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));
4. Kliknutím na **Spustit** dotaz spustíte.

## <a name="remove-secondary-database"></a>Odebrání sekundární databáze
Můžete použít **ALTER DATABASE** příkaz trvale ukončit partnerství replikace mezi jeho primární a sekundární databáze. Tento příkaz je proveden v hlavní databázi, v němž jsou uložena primární databáze. Po ukončení relace sekundární databázi se stane standardní databázi pro čtení a zápis. Pokud dojde k přerušení připojení k sekundární databázi příkazu úspěšné, ale sekundární bude po obnovení připojení pro čtení a zápis. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).

Pomocí následujících kroků k odebrání geograficky replikované sekundární partnerství geografická replikace.

1. V Management Studio se připojte ke svému logickému serveru Azure SQL Database.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující **ALTER DATABASE** příkazu k odebrání geograficky replikované sekundární.
   
        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;
4. Kliknutím na **Spustit** dotaz spustíte.

## <a name="monitor-active-geo-replication-configuration-and-health"></a>Monitorování stavu a konfigurace aktivní geografickou replikaci

Monitorování úlohy zahrnovat monitorování konfigurace geografická replikace a stav replikace dat monitorování.  Můžete použít **sys.dm_geo_replication_links** zobrazení dynamické správy v hlavní databázi k vrácení informací o všech existujícímu propojení replikace pro každou databázi v logického serveru Azure SQL Database. Toto zobrazení obsahuje řádek pro každý odkaz replikace mezi primárním a sekundárním databáze. Můžete použít **sys.dm_replication_link_status** zobrazení dynamické správy vrátí řádek pro každou databázi SQL Azure, která je aktuálně zapojený do replikace odkazu replikace. To zahrnuje primární i sekundární databáze. Pokud existuje více než jeden odkaz průběžná replikace pro danou primární databázi, tato tabulka obsahuje řádek pro každou z relací. Zobrazení se vytvoří v všechny databáze, včetně logické hlavní. Toto zobrazení v logické hlavní dotazování však vrátí prázdnou sadou. Můžete použít **sys.dm_operation_status** zobrazení dynamické správy zobrazíte stav pro všechny operace včetně stav odkazů replikace databáze. Další informace najdete v tématu [sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx), a [sys.dm_operation_status (Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx).

Použijte následující kroky k monitorování aktivní geografickou replikací partnerství.

1. V Management Studio se připojte ke svému logickému serveru Azure SQL Database.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použitím následujícího příkazu zobrazíte všechny databáze s odkazy geografická replikace.
   
        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM sys.geo_replication_links;
4. Kliknutím na **Spustit** dotaz spustíte.
5. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **MyDB**a potom klikněte na **nový dotaz**.
6. Použijte následující příkaz pro zobrazení pomalou replikace a poslední replikace čas Moje sekundární databází MyDB.
   
        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status
7. Kliknutím na **Spustit** dotaz spustíte.
8. Použitím následujícího příkazu zobrazíte nejnovější operace geografická replikace přidružené k databázi MyDB.
   
        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC
9. Kliknutím na **Spustit** dotaz spustíte.

## <a name="next-steps"></a>Další kroky
* Další informace o převzetí služeb při selhání skupiny a aktivní geografickou replikaci, najdete v části - [skupin převzetí služeb při selhání](sql-database-geo-replication-overview.md)
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)

