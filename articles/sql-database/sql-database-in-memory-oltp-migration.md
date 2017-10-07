---
title: "aaaIn technologie OLTP zlepšuje SQL txn výkonu | Microsoft Docs"
description: "Výkon transakcí OLTP v paměti přijímat tooimprove v existující databázi SQL."
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 1bed796069565eb6741953837cf0477c2ddd8519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a>Použití OLTP v paměti tooimprove výkon aplikace v databázi SQL
[OLTP v paměti](sql-database-in-memory.md) použité tooimprove hello výkon zpracování transakcí, přijímání dat a scénáře přechodný dat, může být v [Premium](sql-database-service-tiers.md) databází SQL Azure bez zvýšení hello cenovou úroveň. 

> [!NOTE] 
> Zjistěte, jak [kvora zdvojnásobí klíče databáze zatížení při snížení DTU 70 % s databází SQL](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Postupujte podle těchto kroků tooadopt OLTP v paměti v existující databázi.

## <a name="step-1-ensure-you-are-using-a-premium-database"></a>Krok 1: Ujistěte se, že používáte databáze Premium
OLTP v paměti je podporována pouze v databázích Premium. V paměti je podporováno, pokud hello vrácen, výsledkem je 1 (ne 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* znamená *extrémně zpracování transakcí*



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a>Krok 2: Určení objekty toomigrate tooIn paměti OLTP
Zahrnuje SSMS **přehled analýzy výkonu transakce** sestavy, který můžete spustit na databázi s aktivní úlohu. Sestava Hello identifikuje tabulky a uložené procedury, které jsou kandidáty pro migraci tooIn technologie OLTP.

V aplikaci SSMS toogenerate hello sestavy:

* V hello **Průzkumník objektů**, klikněte pravým tlačítkem na uzel vaší databáze.
* Klikněte na tlačítko **sestavy** > **standardních sestav** > **přehled analýzy výkonu transakce**.

Další informace najdete v tématu [stanovení, pokud tabulka nebo uložené měli přenést postup tooIn technologie OLTP](http://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>Krok 3: Vytvoření databáze porovnatelný z hlediska testu
Předpokládejme, že hello sestava indikující, že vaše databáze má tabulku, která by využívat výhody převést tooa paměťově optimalizované tabulky. Doporučujeme nejprve testování tooconfirm hello indikace testování.

Je třeba testovací kopii vaši produkční databázi. Hello testovací databáze by měla být v hello stejné úrovni vrstvy služby jako vaši produkční databázi.

tooease testování, testovací databáze upravit takto:

1. Připojte databáze toohello test pomocí aplikace SSMS.
2. tooavoid nutnosti hello možnost WITH (SNAPSHOT) v dotazech, nastavte možnost databáze hello, jak je znázorněno v hello následující příkaz jazyka T-SQL:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Krok 4: Migrace tabulky
Musíte vytvořit a naplnit kopii paměťově optimalizované tabulky hello chcete tootest. Můžete ho vytvořit pomocí:

* Hello užitečný průvodce optimalizace paměti v aplikaci SSMS.
* Ruční T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Průvodce optimalizace paměti v aplikaci SSMS
toouse této možnosti migrace:

1. Pomocí SSMS připojte toohello testovací databáze.
2. V hello **Průzkumník objektů**, klikněte pravým tlačítkem na hello tabulce a pak klikněte na tlačítko **Advisor optimalizace paměti**.
   
   * Hello **tabulky paměti Optimalizátor Advisor** průvodce se zobrazí.
3. V Průvodci hello, klikněte na tlačítko **ověření migrace** (nebo hello **Další** tlačítko) nepodporovaný toosee Pokud hello tabulka obsahuje všechny funkce, které nejsou podporovány v paměťově optimalizovaných tabulkách. Další informace naleznete v tématu:
   
   * Hello *kontrolní seznam optimalizace paměti* v [Advisor optimalizace paměti](http://msdn.microsoft.com/library/dn284308.aspx).
   * [Konstrukce Transact-SQL nepodporuje OLTP v paměti](http://msdn.microsoft.com/library/dn246937.aspx).
   * [Migrace tooIn technologie OLTP](http://msdn.microsoft.com/library/dn247639.aspx).
4. Pokud hello tabulka nemá žádné nepodporované funkce, hello advisor můžete provádět hello skutečné schéma a migraci dat za vás.

#### <a name="manual-t-sql"></a>Ruční T-SQL
toouse této možnosti migrace:

1. Připojte databáze tooyour test pomocí aplikace SSMS (nebo podobného nástroje).
2. Získáte hello dokončení skriptu T-SQL pro tabulku a jeho indexů.
   
   * V aplikaci SSMS klikněte pravým tlačítkem na uzel vaší tabulky.
   * Klikněte na tlačítko **skript tabulky jako** > **vytvořit** > **nové okno dotazu**.
3. V okně hello skript, přidejte WITH (hodnotou MEMORY_OPTIMIZED = ON) toohello příkazu CREATE TABLE.
4. Pokud je index na clusteru, změňte tooNONCLUSTERED.
5. Přejmenujte existující tabulky hello pomocí procedura SP_RENAME.
6. Vytvořte hello nové paměťově optimalizované kopie hello tabulky pomocí upravená CREATE TABLE skriptu.
7. Kopírovat hello data tooyour paměťově optimalizované tabulky pomocí INSERT... VYBERTE * DO:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Krok 5 (volitelné): migrace uložené procedury
funkce v paměti Hello můžete také upravit uložené procedury pro zlepšení výkonu.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Informace o nativně kompilovaných uložených procedur
Nativně kompilované uložené procedury, musíte mít následující možnosti na jeho klauzule T-SQL s hello:

* MOŽNOST NATIVE_COMPILATION
* Svázání se SCHÉMATEM: význam tabulek, které hello uložené procedury nelze mít jejich definice sloupců změnit žádným způsobem, který by došlo k ovlivnění hello uložené procedury, není-li vyřadit hello uložené procedury.

Nativní modul musí používat jednu velký [ATOMICKÝCH bloků](http://msdn.microsoft.com/library/dn452281.aspx) pro správu transakce. Neexistuje žádný atribut role pro explicitní BEGIN TRANSACTION, nebo pro vrácení transakce zpět. Pokud váš kód zjistí narušení obchodní pravidlo, jej můžete ukončit blok atomic hello s [THROW](http://msdn.microsoft.com/library/ee677615.aspx) příkaz.

### <a name="typical-create-procedure-for-natively-compiled"></a>Typické CREATE PROCEDURE pro nativně kompilované
Obvykle hello T-SQL toocreate nativně kompilované uložené procedury je podobné toohello následující šablony:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Pro hello TRANSACTION_ISOLATION_LEVEL je SNÍMEK hello nejběžnější hodnotu hello nativně kompilované uložené procedury. Ale podmnožinu hello jiné hodnoty jsou podporovány také:
  
  * REPEATABLE READ
  * SERIALIZOVATELNÉ
* Hello hodnotu jazyka musí být přítomen v zobrazení sys.languages hello.

### <a name="how-toomigrate-a-stored-procedure"></a>Jak toomigrate uložené procedury
kroky migrace Hello jsou:

1. Získáte hello CREATE PROCEDURE skriptu toohello regulární interpretovaný uložené procedury.
2. Přepisování jeho předchozí šablonu záhlaví toomatch hello.
3. Ověření, zda hello uložené procedury kód T-SQL používá všechny funkce, které nejsou podporovány pro nativně kompilované uložené procedury. Implementace řešení, v případě potřeby.
   
   * Podrobnosti najdete v tématu [problémy s migrací pro nativně kompilované uložené procedury](http://msdn.microsoft.com/library/dn296678.aspx).
4. Přejmenujte hello staré uložené procedury pomocí procedura SP_RENAME. Nebo jednoduše ho VYŘADIT.
5. Spusťte skript upravená vytvořit postup T-SQL.

## <a name="step-6-run-your-workload-in-test"></a>Krok 6: Spuštění úlohy v testu
Spusťte úlohu ve vaší databázi test, který je podobný toohello zatížení, které běží v produkční databázi. To by měl odhalit hello výkonnější dosáhnout používání funkce hello v paměti pro tabulky a uložené procedury.

Hlavní atributy hello úlohy jsou:

* Počet souběžných připojení.
* Poměr pro čtení a zápis.

tootailor a spuštění hello testovací úlohy, zvažte použití hello užitečný ostress.exe nástroj, který ukazuje [zde](sql-database-in-memory.md).

latence sítě toominimize, spusťte test v hello stejné zeměpisné oblasti Azure kde hello databáze existuje.

## <a name="step-7-post-implementation-monitoring"></a>Krok 7: Po implementaci monitorování
Vezměte v úvahu monitorování výkonu důsledky hello vaší implementace v paměti v produkčním prostředí:

* [Monitorování úložiště v paměti](sql-database-in-memory-oltp-monitoring.md).
* [Monitorování databáze Azure SQL Database pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>Související odkazy
* [Paměť OLTP (v paměti optimalizace)](http://msdn.microsoft.com/library/dn133186.aspx)
* [Úvod tooNatively kompilované uložené procedury](http://msdn.microsoft.com/library/dn133184.aspx)
* [Advisor optimalizace paměti](http://msdn.microsoft.com/library/dn284308.aspx)

