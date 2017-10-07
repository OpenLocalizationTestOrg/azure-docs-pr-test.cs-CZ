---
title: "aaaConfigure geografická replikace pro Azure SQL Database pomocí jazyka Transact-SQL | Microsoft Docs"
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
ms.openlocfilehash: 295b6b12f257dfb15131d5ee28fbe65c6476352d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurace aktivní geografickou replikací pro Azure SQL Database pomocí jazyka Transact-SQL

Tento článek ukazuje, jak tooconfigure active geografická replikace pro Azure SQL Database pomocí jazyka Transact-SQL.

tooinitiate převzetí služeb při selhání pomocí jazyka Transact-SQL, najdete v části [zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí jazyka Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

> [!NOTE]
> Pokud používáte active geografická replikace (čitelný sekundární repliky) pro zotavení po havárii byste měli nakonfigurovat skupinu převzetí služeb při selhání pro všechny databáze v rámci aplikace tooenable automatické a transparentní převzetí služeb při selhání. Tato funkce je ve verzi preview. Další informace najdete v části [skupiny automatické převzetí služeb při selhání a geografická replikace](sql-database-geo-replication-overview.md).
> 
> 

tooconfigure aktivní geografickou replikaci pomocí jazyka Transact-SQL, je nutné hello následující:

* Předplatné Azure
* Logický server Azure SQL Database <MyLocalServer> a SQL database <MyDB> -hello primární databázi, které chcete tooreplicate
* Jeden nebo více logických Azure SQL Database serverů < MySecondaryServer(n) > - hello logické servery, které budou hello partnerské servery, ve kterých se vytvoří sekundární databáze
* Přihlašovací jméno, které je DBManager na primární hello
* Mít db_ownership hello místní databáze, které budete geografická replikace
* Být DBManager na hello partnerské servery toowhich budete konfigurovat geografická replikace
* Hello nejnovější verzi systému SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="add-secondary-database"></a>Přidání sekundární databáze
Můžete použít hello **ALTER DATABASE** příkaz toocreate geograficky replikované sekundární databáze na serveru partnera. Spusťte tento příkaz v hlavní databázi hello hello server obsahující hello databáze toobe replikovat. Hello geograficky replikované databáze (hello "primární databáze") bude mít stejný název jako databáze hello právě replikován a bude ve výchozím nastavení, hello hello stejné úrovni služby jako primární databáze hello. Hello sekundární databáze může být čitelný nebo bez možnosti čtení a může být jedné databáze nebo v elastickém fondu. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).
Jakmile hello sekundární databáze se vytvoří a nasadí, zahájíte data replikace z primární databáze hello asynchronně. Hello níže uvedené kroky popisují jak geografická replikace tooconfigure pomocí nástroje Management Studio. Kroky toocreate bez možnosti čtení a čitelný sekundární databáze, jsou uvedené jako jedné databáze nebo ve fondu elastické databáze.

> [!NOTE]
> Pokud databáze existuje v serveru pro zadaný partner hello s hello stejný název jako hello primární databáze hello příkaz se nezdaří.
> 

### <a name="add-readable-secondary-single-database"></a>Přidat čitelný sekundární (jedné databáze)
Pomocí následujících kroků toocreate čitelný sekundární jako jednu databázi hello.

1. V aplikaci Management Studio připojte tooyour logického serveru Azure SQL Database.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte hello **ALTER DATABASE** příkaz toomake místní databázi do primárního geografická replikace s čitelnou sekundární databází na sekundárním serveru.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);
4. Klikněte na tlačítko **Execute** toorun hello dotazu.

### <a name="add-readable-secondary-elastic-pool"></a>Přidat čitelný sekundární (elastický fond)
Pomocí následujících kroků toocreate čitelný sekundární v elastickém fondu hello.

1. V aplikaci Management Studio připojte tooyour logického serveru Azure SQL Database.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte hello **ALTER DATABASE** příkaz toomake místní databázi do primárního geografická replikace s čitelnou sekundární databází na sekundárním serveru v elastickém fondu.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));
4. Klikněte na tlačítko **Execute** toorun hello dotazu.

## <a name="remove-secondary-database"></a>Odebrání sekundární databáze
Můžete použít hello **ALTER DATABASE** toopermanently příkaz Ukončit hello partnerství replikace mezi jeho primární a sekundární databáze. Tento příkaz je proveden v hlavní databázi hello, na které hello primární databázi nachází. Po ukončení relace hello hello sekundární databáze se stane standardní databázi pro čtení a zápis. Pokud je přerušený hello připojení toosecondary databáze hello příkazu úspěšné, ale hello sekundární bude po obnovení připojení pro čtení a zápis. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).

Pomocí následujících kroků tooremove geograficky replikované sekundární z geografická replikace partnerství hello.

1. V aplikaci Management Studio připojte tooyour logického serveru Azure SQL Database.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte hello **ALTER DATABASE** příkaz tooremove geograficky replikované sekundární.
   
        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;
4. Klikněte na tlačítko **Execute** toorun hello dotazu.

## <a name="monitor-active-geo-replication-configuration-and-health"></a>Monitorování stavu a konfigurace aktivní geografickou replikaci

Monitorování úlohy zahrnovat monitorování stavu replikace dat a monitorování konfigurace hello geografická replikace.  Můžete použít hello **sys.dm_geo_replication_links** zobrazení dynamické správy v hello hlavní databázi tooreturn informace o všech existujícímu propojení replikace pro každou databázi hello logického serveru Azure SQL Database. Toto zobrazení obsahuje řádek pro každý odkaz replikace hello mezi primární a sekundární databáze. Můžete použít hello **sys.dm_replication_link_status** dynamické správy zobrazit tooreturn řádek pro každou databázi SQL Azure, která je aktuálně zapojený do replikace odkazu replikace. To zahrnuje primární i sekundární databáze. Pokud existuje více než jeden odkaz průběžná replikace pro danou primární databázi, tato tabulka obsahuje řádek pro každou hello relací. zobrazení Hello je vytvořená ve všechny databáze, včetně logické hlavní hello. Toto zobrazení v logické hlavní hello dotazování však vrátí prázdnou sadou. Můžete použít hello **sys.dm_operation_status** dynamické správy zobrazit tooshow hello stav pro všechny operace včetně hello stav odkazů replikace hello databázi. Další informace najdete v tématu [sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx), a [sys.dm_operation_status (Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx).

Pomocí následujících kroků toomonitor aktivní geografickou replikací partnerství hello.

1. V aplikaci Management Studio připojte tooyour logického serveru Azure SQL Database.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující příkaz tooshow hello všechny databáze s odkazy geografická replikace.
   
        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM sys.geo_replication_links;
4. Klikněte na tlačítko **Execute** toorun hello dotazu.
5. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **MyDB**a potom klikněte na **nový dotaz**.
6. Hello použijte následující příkaz tooshow hello replikace zpožděním a naposledy replikace Moje sekundární databází MyDB.
   
        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status
7. Klikněte na tlačítko **Execute** toorun hello dotazu.
8. Použijte následující příkaz tooshow hello nejnovější geografická replikace operace, které souvisí s databází MyDB hello.
   
        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC
9. Klikněte na tlačítko **Execute** toorun hello dotazu.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o převzetí služeb při selhání skupiny a aktivní geografickou replikaci, najdete v části - [skupin převzetí služeb při selhání](sql-database-geo-replication-overview.md)
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)

