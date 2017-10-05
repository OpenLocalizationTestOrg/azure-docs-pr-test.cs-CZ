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
ms.openlocfilehash: 459941d2c82e5d4ef62beab4ccf775ab8f5efce4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí jazyka Transact-SQL

Tento článek ukazuje, jak zahájit převzetí služeb při selhání pro sekundární databáze SQL pomocí jazyka Transact-SQL. Konfigurace geografická replikace najdete v tématu [konfigurace geografická replikace pro Azure SQL Database](sql-database-geo-replication-transact-sql.md).

Chcete-li zahájit převzetí služeb při selhání, budete potřebovat následující:

* Přihlašovací jméno, které je DBManager na primárním
* Mít db_ownership místní databáze, které budete geografická replikace
* Být DBManager na partnerské servery, na kterou budete konfigurovat geografická replikace
* Nejnovější verzi systému SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>  

## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Zahájení plánované převzetí služeb při selhání povýšení sekundární databáze k nové primární
Můžete použít **ALTER DATABASE** příkaz podporovat sekundární databáze k nové primární databáze plánované způsobem, degradování existující primární k sekundární databázi. Tento příkaz je proveden v hlavní databázi na logickém serveru Azure SQL Database, ve které je umístěn geograficky replikované sekundární databáze, která je povyšovaným. Tato funkce je určená pro plánované převzetí služeb při selhání, například během cvičení zotavení po Havárii a vyžaduje, aby primární databáze k dispozici.

Příkaz provede následující pracovní postup:

1. Replikace se dočasně přepne do synchronním režimu, způsobuje všechny zbývající transakce zapsány na sekundární a blokování všechny nové transakce;
2. Přepínače role ve dvou databázích ve spolupráci se geografická replikace.  

Toto pořadí zaručuje, že ve dvou databázích jsou synchronizovány před přepněte role, a proto nedošlo ke ztrátě dat dojde. Není k dispozici na krátkou dobu, během které obě databáze jsou k dispozici (v řádu 0 až 25 sekund), při přepínání role. Pokud primární databáze má více sekundárních databází, příkaz se automaticky znovu nakonfigurovat jiné sekundární databáze k připojení k nové primární.  Celou operaci by měl trvat méně než minutu za normálních okolností. Další informace najdete v tématu [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) a [úrovně služeb](sql-database-service-tiers.md).

Pomocí následujících kroků zahájíte plánované převzetí služeb při selhání.

1. V Management Studio se připojte k logického serveru Azure SQL Database, ve které je umístěn geograficky replikované sekundární databáze.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující **ALTER DATABASE** příkaz Přepnout sekundární databázi do primární role.
   
        ALTER DATABASE <MyDB> FAILOVER;
4. Kliknutím na **Spustit** dotaz spustíte.

> [!NOTE]
> Ve výjimečných případech je možné, že operaci nelze dokončit a může se zobrazit zablokované. Uživatel v takovém případě můžete spustit příkaz vynuceného převzetí služeb při selhání a přijmout ztráty dat.
> 
> 

## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Zahájit neplánované převzetí služeb při selhání z primární databáze do sekundární databáze
Můžete použít **ALTER DATABASE** příkaz podporovat sekundární databáze k nové primární databáze neplánované způsobem, vynucení snížení úrovně existující primární k sekundární databázi v době, kdy primární databáze již není k dispozici. Tento příkaz je proveden v hlavní databázi na logickém serveru Azure SQL Database, ve které je umístěn geograficky replikované sekundární databáze, která je povyšovaným.

Tato funkce je určená pro zotavení po havárii, při obnovení databáze dostupnosti je velmi důležité a ztrátu dat je přijatelná. Po vyvolání Vynucené převzetí služeb při selhání zadaná sekundární databáze okamžitě stane primární databáze a začne přijímat transakce zápisu. Jakmile původní primární databáze je možné se připojit s novou primární databázi, přírůstkové zálohování je provést na původní primární databáze a původní primární databáze se stane do sekundární databáze pro nové primární databáze; následně je jenom synchronizace repliky nový primární.

Ale protože bod obnovení v čas není podporována na sekundární databáze, pokud uživatel, které chcete obnovit data zapsána na původní primární databázi, která byla nereplikoval nové primární databázi před Vynucené převzetí služeb při selhání došlo k chybě, uživatel bude potřebovat používat podporu, aby to ztráty dat obnovit.

Pokud primární databáze má více sekundárních databází, příkaz se automaticky znovu nakonfigurovat jiné sekundární databáze k připojení k nové primární.

Pomocí následujících kroků zahájíte neplánované převzetí služeb při selhání.

1. V Management Studio se připojte k logického serveru Azure SQL Database, ve které je umístěn geograficky replikované sekundární databáze.
2. Otevřete složku databází, rozbalte **systémové databáze** složku, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **nový dotaz**.
3. Použijte následující **ALTER DATABASE** příkaz Přepnout sekundární databázi do primární role.
   
        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;
4. Kliknutím na **Spustit** dotaz spustíte.

> [!NOTE]
> Pokud příkaz se objeví, když primární a sekundární jsou online z původního primárního se stane nový sekundární okamžitě, bez synchronizace dat. Spouštění transakce při vydání příkazu ke ztrátě dat. může dojít, pokud je primární.
> 
> 

## <a name="next-steps"></a>Další kroky
* Po převzetí služeb při selhání Ujistěte se, že požadavky na ověřování pro server a databáze jsou nakonfigurovány na nový primární. Podrobnosti najdete v tématu [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md).
* Další obnovení po havárii pomocí aktivní geografickou replikaci, včetně postupu obnovení před a po obnovení a provedením postupu zotavení po havárii, najdete v části [zotavení po havárii](sql-database-disaster-recovery.md)
* Blogový příspěvek Sasha Nosov o aktivní geografickou replikaci, najdete v části [Spotlight na nové geografická replikace funkce](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
* Informace o návrhu cloudové aplikace na používání aktivní geografické replikace najdete v tématu [návrhu cloudové aplikace pro kontinuitu podnikových procesů pomocí geografická replikace](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* Informace o používání aktivní geografické replikace s elastické fondy najdete v tématu [strategie zotavení po havárii elastický fond](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
* Přehled kontinuity podnikových procesů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)

