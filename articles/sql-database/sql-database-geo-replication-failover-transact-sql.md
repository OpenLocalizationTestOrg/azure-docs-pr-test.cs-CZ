---
title: "TSQL:initiate převzetí služeb při selhání pro Azure SQL Database | Microsoft Docs"
description: "Zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí jazyka Transact-SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5eb2d256-025d-4f5a-99d4-17f702b37f14
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 418953e044ba84ce758063d56a371af45d5cdfe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí jazyka Transact-SQL

Tento článek ukazuje, jak tooinitiate převzetí služeb při selhání tooa sekundární databáze SQL pomocí jazyka Transact-SQL. tooconfigure geografická replikace, najdete v části [konfigurace geografická replikace pro Azure SQL Database](sql-database-geo-replication-transact-sql.md).

tooinitiate převzetí služeb při selhání, je třeba hello následující:

* Přihlašovací jméno, které je DBManager na primární hello
* Mít db_ownership hello místní databáze, které budete geografická replikace
* Být DBManager na hello partnerské servery toowhich budete konfigurovat geografická replikace
* Nejnovější verzi systému SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Se doporučuje hello vždy použít nejnovější verzi tooremain Management Studio synchronizovat se službou aktualizace tooMicrosoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>  

## <a name="initiate-a-planned-failover-promoting-a-secondary-database-toobecome-hello-new-primary"></a>Zahájení plánované převzetí služeb při selhání povýšení nový primární sekundární databáze toobecome hello
Můžete použít hello **ALTER DATABASE** příkaz toopromote nový primární sekundární databáze toobecome hello databáze plánovaný cyklicky, degradování hello existující primární toobecome sekundární. Tento příkaz je proveden v hlavní databázi hello na hello Azure SQL Database logickém serveru, ve které hello geograficky replikované sekundární databáze, která je povyšovaným nachází. Tato funkce je určená pro plánované převzetí služeb při selhání, například během cvičení hello zotavení po Havárii a vyžaduje, že tuto primární databázi hello být k dispozici.

příkaz Hello provede hello následující pracovní postup:

1. Dočasně toosynchronous režim replikace přepínače, způsobuje všechny zbývající transakce toobe vyprázdněných toohello sekundární a blokování všechny nové transakce;
2. Přepínače hello role hello dvou databází ve spolupráci se geografická replikace hello.  

Toto pořadí zaručuje, že hello dvě databáze nejsou synchronizovány před přepněte hello role, a proto nedošlo ke ztrátě dat dojde. Není k dispozici na krátkou dobu, během které obě databáze jsou k dispozici (v pořadí hello 0 too25 sekund), při přepínání hello role. Pokud hello primární databáze má více sekundární databáze, hello příkaz bude automaticky reconfigure hello nový primární jiných sekundárních tooconnect toohello.  Hello celou operaci lze provést méně než minutu toocomplete za normálních okolností. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).

Pomocí následujících kroků tooinitiate plánované převzetí služeb při selhání hello.

1. V aplikaci Management Studio připojte toohello Azure SQL Database logického serveru, ve které je umístěn geograficky replikované sekundární databáze.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte hello **ALTER DATABASE** příkaz tooswitch hello sekundární databáze toohello primární roli.
   
        ALTER DATABASE <MyDB> FAILOVER;
4. Klikněte na tlačítko **Execute** toorun hello dotazu.

> [!NOTE]
> Ve výjimečných případech je možné, že hello operaci nelze dokončit a může se zobrazit zablokované. V takovém případě hello a uživatel může provést hello Vynucené převzetí služeb při selhání příkaz přijmout ztráty dat.
> 
> 

## <a name="initiate-an-unplanned-failover-from-hello-primary-database-toohello-secondary-database"></a>Zahájit neplánované převzetí služeb při selhání z hello primární databáze toohello sekundární databáze
Můžete použít hello **ALTER DATABASE** příkaz toopromote nový primární sekundární databáze toobecome hello databáze neplánované způsobem, vynucení hello degradace hello existující primární toobecome sekundární současně při hello primární databáze již není k dispozici. Tento příkaz je proveden v hlavní databázi hello na hello Azure SQL Database logickém serveru, ve které hello geograficky replikované sekundární databáze, která je povyšovaným nachází.

Tato funkce je určená pro zotavení po havárii, při obnovení dostupnost databáze hello je důležité a ztrátu dat je přijatelná. Po vyvolání Vynucené převzetí služeb při selhání hello zadané sekundární databáze okamžitě stane hello primární databáze a začne přijímat transakce zápisu. Jakmile hello původní primární databáze je možné tooreconnect s novou primární databázi, je přírůstkové zálohy pořízené hello původní primární databáze a hello původní primární databáze se stane do sekundární databáze pro nové primární databáze hello; následně je jenom synchronizace repliky nový primární hello.

Ale protože bod obnovení v době není podporován na hello sekundární databáze, hello uživatele, které toorecover data potvrzené toohello původní primární databázi, která by nebyl replikovat toohello nové primární databáze před hello Vynucené převzetí služeb při selhání došlo k chybě, Hello bude stačit, když uživatel tooengage podporu toorecover to ztráty dat.

Pokud hello primární databáze má více sekundární databáze, hello příkaz bude automaticky reconfigure hello nový primární jiných sekundárních tooconnect toohello.

Pomocí následujících kroků tooinitiate neplánované převzetí služeb při selhání hello.

1. V aplikaci Management Studio připojte toohello Azure SQL Database logického serveru, ve které je umístěn geograficky replikované sekundární databáze.
2. Otevřete složku databází hello, rozbalte hello **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte hello **ALTER DATABASE** příkaz tooswitch hello sekundární databáze toohello primární roli.
   
        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;
4. Klikněte na tlačítko **Execute** toorun hello dotazu.

> [!NOTE]
> Pokud příkaz hello se objeví, pokud je online primární i sekundární hello původního primárního se stane nový sekundární okamžitě, bez synchronizace dat hello. Spouštění transakce při vydání příkazu hello ztrátu dat může dojít, pokud je primární hello.
> 
> 

## <a name="next-steps"></a>Další kroky
* Po převzetí služeb při selhání Ujistěte se, že hello požadavky na ověřování pro server a databáze jsou nakonfigurovány na nový primární hello. Podrobnosti najdete v tématu [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md).
* toolearn obnovení po havárii pomocí aktivní geografickou replikaci, včetně postupu obnovení před a po obnovení a provedením postupu zotavení po havárii, najdete v části [zotavení po havárii](sql-database-disaster-recovery.md)
* Blogový příspěvek Sasha Nosov o aktivní geografickou replikaci, najdete v části [Spotlight na nové geografická replikace funkce](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Informace o návrhu cloudové aplikace toouse active geografická replikace najdete v tématu [návrhu cloudové aplikace pro kontinuitu podnikových procesů pomocí geografická replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Informace o používání aktivní geografické replikace s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* Přehled kontinuity podnikových procesů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)

