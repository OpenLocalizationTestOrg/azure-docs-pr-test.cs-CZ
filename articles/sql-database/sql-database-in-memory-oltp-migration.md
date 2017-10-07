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
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a><span data-ttu-id="fbf1e-103">Použití OLTP v paměti tooimprove výkon aplikace v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="fbf1e-103">Use In-Memory OLTP tooimprove your application performance in SQL Database</span></span>
<span data-ttu-id="fbf1e-104">[OLTP v paměti](sql-database-in-memory.md) použité tooimprove hello výkon zpracování transakcí, přijímání dat a scénáře přechodný dat, může být v [Premium](sql-database-service-tiers.md) databází SQL Azure bez zvýšení hello cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-104">[In-Memory OLTP](sql-database-in-memory.md) can be used tooimprove hello performance of transaction processing, data ingestion, and transient data scenarios, in  [Premium](sql-database-service-tiers.md) Azure SQL Databases without increasing hello pricing tier.</span></span> 

> [!NOTE] 
> <span data-ttu-id="fbf1e-105">Zjistěte, jak [kvora zdvojnásobí klíče databáze zatížení při snížení DTU 70 % s databází SQL](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span><span class="sxs-lookup"><span data-stu-id="fbf1e-105">Learn how [Quorum doubles key database’s workload while lowering DTU by 70% with SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span></span>


<span data-ttu-id="fbf1e-106">Postupujte podle těchto kroků tooadopt OLTP v paměti v existující databázi.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-106">Follow these steps tooadopt In-Memory OLTP in your existing database.</span></span>

## <a name="step-1-ensure-you-are-using-a-premium-database"></a><span data-ttu-id="fbf1e-107">Krok 1: Ujistěte se, že používáte databáze Premium</span><span class="sxs-lookup"><span data-stu-id="fbf1e-107">Step 1: Ensure you are using a Premium database</span></span>
<span data-ttu-id="fbf1e-108">OLTP v paměti je podporována pouze v databázích Premium.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-108">In-Memory OLTP is supported only in Premium databases.</span></span> <span data-ttu-id="fbf1e-109">V paměti je podporováno, pokud hello vrácen, výsledkem je 1 (ne 0):</span><span class="sxs-lookup"><span data-stu-id="fbf1e-109">In-Memory is supported if hello returned result is 1 (not 0):</span></span>

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

<span data-ttu-id="fbf1e-110">*XTP* znamená *extrémně zpracování transakcí*</span><span class="sxs-lookup"><span data-stu-id="fbf1e-110">*XTP* stands for *Extreme Transaction Processing*</span></span>



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a><span data-ttu-id="fbf1e-111">Krok 2: Určení objekty toomigrate tooIn paměti OLTP</span><span class="sxs-lookup"><span data-stu-id="fbf1e-111">Step 2: Identify objects toomigrate tooIn-Memory OLTP</span></span>
<span data-ttu-id="fbf1e-112">Zahrnuje SSMS **přehled analýzy výkonu transakce** sestavy, který můžete spustit na databázi s aktivní úlohu.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-112">SSMS includes a **Transaction Performance Analysis Overview** report that you can run against a database with an active workload.</span></span> <span data-ttu-id="fbf1e-113">Sestava Hello identifikuje tabulky a uložené procedury, které jsou kandidáty pro migraci tooIn technologie OLTP.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-113">hello report identifies tables and stored procedures that are candidates for migration tooIn-Memory OLTP.</span></span>

<span data-ttu-id="fbf1e-114">V aplikaci SSMS toogenerate hello sestavy:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-114">In SSMS, toogenerate hello report:</span></span>

* <span data-ttu-id="fbf1e-115">V hello **Průzkumník objektů**, klikněte pravým tlačítkem na uzel vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-115">In hello **Object Explorer**, right-click your database node.</span></span>
* <span data-ttu-id="fbf1e-116">Klikněte na tlačítko **sestavy** > **standardních sestav** > **přehled analýzy výkonu transakce**.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-116">Click **Reports** > **Standard Reports** > **Transaction Performance Analysis Overview**.</span></span>

<span data-ttu-id="fbf1e-117">Další informace najdete v tématu [stanovení, pokud tabulka nebo uložené měli přenést postup tooIn technologie OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-117">For more information, see [Determining if a Table or Stored Procedure Should Be Ported tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span></span>

## <a name="step-3-create-a-comparable-test-database"></a><span data-ttu-id="fbf1e-118">Krok 3: Vytvoření databáze porovnatelný z hlediska testu</span><span class="sxs-lookup"><span data-stu-id="fbf1e-118">Step 3: Create a comparable test database</span></span>
<span data-ttu-id="fbf1e-119">Předpokládejme, že hello sestava indikující, že vaše databáze má tabulku, která by využívat výhody převést tooa paměťově optimalizované tabulky.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-119">Suppose hello report indicates your database has a table that would benefit from being converted tooa memory-optimized table.</span></span> <span data-ttu-id="fbf1e-120">Doporučujeme nejprve testování tooconfirm hello indikace testování.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-120">We recommend that you first test tooconfirm hello indication by testing.</span></span>

<span data-ttu-id="fbf1e-121">Je třeba testovací kopii vaši produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-121">You need a test copy of your production database.</span></span> <span data-ttu-id="fbf1e-122">Hello testovací databáze by měla být v hello stejné úrovni vrstvy služby jako vaši produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-122">hello test database should be at hello same service tier level as your production database.</span></span>

<span data-ttu-id="fbf1e-123">tooease testování, testovací databáze upravit takto:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-123">tooease testing, tweak your test database as follows:</span></span>

1. <span data-ttu-id="fbf1e-124">Připojte databáze toohello test pomocí aplikace SSMS.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-124">Connect toohello test database by using SSMS.</span></span>
2. <span data-ttu-id="fbf1e-125">tooavoid nutnosti hello možnost WITH (SNAPSHOT) v dotazech, nastavte možnost databáze hello, jak je znázorněno v hello následující příkaz jazyka T-SQL:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-125">tooavoid needing hello WITH (SNAPSHOT) option in queries, set hello database option as shown in hello following T-SQL statement:</span></span>
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a><span data-ttu-id="fbf1e-126">Krok 4: Migrace tabulky</span><span class="sxs-lookup"><span data-stu-id="fbf1e-126">Step 4: Migrate tables</span></span>
<span data-ttu-id="fbf1e-127">Musíte vytvořit a naplnit kopii paměťově optimalizované tabulky hello chcete tootest.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-127">You must create and populate a memory-optimized copy of hello table you want tootest.</span></span> <span data-ttu-id="fbf1e-128">Můžete ho vytvořit pomocí:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-128">You can create it by using either:</span></span>

* <span data-ttu-id="fbf1e-129">Hello užitečný průvodce optimalizace paměti v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-129">hello handy Memory Optimization Wizard in SSMS.</span></span>
* <span data-ttu-id="fbf1e-130">Ruční T-SQL.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-130">Manual T-SQL.</span></span>

#### <a name="memory-optimization-wizard-in-ssms"></a><span data-ttu-id="fbf1e-131">Průvodce optimalizace paměti v aplikaci SSMS</span><span class="sxs-lookup"><span data-stu-id="fbf1e-131">Memory Optimization Wizard in SSMS</span></span>
<span data-ttu-id="fbf1e-132">toouse této možnosti migrace:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-132">toouse this migration option:</span></span>

1. <span data-ttu-id="fbf1e-133">Pomocí SSMS připojte toohello testovací databáze.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-133">Connect toohello test database with SSMS.</span></span>
2. <span data-ttu-id="fbf1e-134">V hello **Průzkumník objektů**, klikněte pravým tlačítkem na hello tabulce a pak klikněte na tlačítko **Advisor optimalizace paměti**.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-134">In hello **Object Explorer**, right-click on hello table, and then click **Memory Optimization Advisor**.</span></span>
   
   * <span data-ttu-id="fbf1e-135">Hello **tabulky paměti Optimalizátor Advisor** průvodce se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-135">hello **Table Memory Optimizer Advisor** wizard is displayed.</span></span>
3. <span data-ttu-id="fbf1e-136">V Průvodci hello, klikněte na tlačítko **ověření migrace** (nebo hello **Další** tlačítko) nepodporovaný toosee Pokud hello tabulka obsahuje všechny funkce, které nejsou podporovány v paměťově optimalizovaných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-136">In hello wizard, click **Migration validation** (or hello **Next** button) toosee if hello table has any unsupported features that are unsupported in memory-optimized tables.</span></span> <span data-ttu-id="fbf1e-137">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-137">For more information, see:</span></span>
   
   * <span data-ttu-id="fbf1e-138">Hello *kontrolní seznam optimalizace paměti* v [Advisor optimalizace paměti](http://msdn.microsoft.com/library/dn284308.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-138">hello *memory optimization checklist* in [Memory Optimization Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span></span>
   * <span data-ttu-id="fbf1e-139">[Konstrukce Transact-SQL nepodporuje OLTP v paměti](http://msdn.microsoft.com/library/dn246937.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-139">[Transact-SQL Constructs Not Supported by In-Memory OLTP](http://msdn.microsoft.com/library/dn246937.aspx).</span></span>
   * <span data-ttu-id="fbf1e-140">[Migrace tooIn technologie OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-140">[Migrating tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span></span>
4. <span data-ttu-id="fbf1e-141">Pokud hello tabulka nemá žádné nepodporované funkce, hello advisor můžete provádět hello skutečné schéma a migraci dat za vás.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-141">If hello table has no unsupported features, hello advisor can perform hello actual schema and data migration for you.</span></span>

#### <a name="manual-t-sql"></a><span data-ttu-id="fbf1e-142">Ruční T-SQL</span><span class="sxs-lookup"><span data-stu-id="fbf1e-142">Manual T-SQL</span></span>
<span data-ttu-id="fbf1e-143">toouse této možnosti migrace:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-143">toouse this migration option:</span></span>

1. <span data-ttu-id="fbf1e-144">Připojte databáze tooyour test pomocí aplikace SSMS (nebo podobného nástroje).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-144">Connect tooyour test database by using SSMS (or a similar utility).</span></span>
2. <span data-ttu-id="fbf1e-145">Získáte hello dokončení skriptu T-SQL pro tabulku a jeho indexů.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-145">Obtain hello complete T-SQL script for your table and its indexes.</span></span>
   
   * <span data-ttu-id="fbf1e-146">V aplikaci SSMS klikněte pravým tlačítkem na uzel vaší tabulky.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-146">In SSMS, right-click your table node.</span></span>
   * <span data-ttu-id="fbf1e-147">Klikněte na tlačítko **skript tabulky jako** > **vytvořit** > **nové okno dotazu**.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-147">Click **Script Table As** > **CREATE To** > **New Query Window**.</span></span>
3. <span data-ttu-id="fbf1e-148">V okně hello skript, přidejte WITH (hodnotou MEMORY_OPTIMIZED = ON) toohello příkazu CREATE TABLE.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-148">In hello script window, add WITH (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE statement.</span></span>
4. <span data-ttu-id="fbf1e-149">Pokud je index na clusteru, změňte tooNONCLUSTERED.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-149">If there is a CLUSTERED index, change it tooNONCLUSTERED.</span></span>
5. <span data-ttu-id="fbf1e-150">Přejmenujte existující tabulky hello pomocí procedura SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-150">Rename hello existing table by using SP_RENAME.</span></span>
6. <span data-ttu-id="fbf1e-151">Vytvořte hello nové paměťově optimalizované kopie hello tabulky pomocí upravená CREATE TABLE skriptu.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-151">Create hello new memory-optimized copy of hello table by running your edited CREATE TABLE script.</span></span>
7. <span data-ttu-id="fbf1e-152">Kopírovat hello data tooyour paměťově optimalizované tabulky pomocí INSERT... VYBERTE * DO:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-152">Copy hello data tooyour memory-optimized table by using INSERT...SELECT * INTO:</span></span>

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a><span data-ttu-id="fbf1e-153">Krok 5 (volitelné): migrace uložené procedury</span><span class="sxs-lookup"><span data-stu-id="fbf1e-153">Step 5 (optional): Migrate stored procedures</span></span>
<span data-ttu-id="fbf1e-154">funkce v paměti Hello můžete také upravit uložené procedury pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-154">hello In-Memory feature can also modify a stored procedure for improved performance.</span></span>

### <a name="considerations-with-natively-compiled-stored-procedures"></a><span data-ttu-id="fbf1e-155">Informace o nativně kompilovaných uložených procedur</span><span class="sxs-lookup"><span data-stu-id="fbf1e-155">Considerations with natively compiled stored procedures</span></span>
<span data-ttu-id="fbf1e-156">Nativně kompilované uložené procedury, musíte mít následující možnosti na jeho klauzule T-SQL s hello:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-156">A natively compiled stored procedure must have hello following options on its T-SQL WITH clause:</span></span>

* <span data-ttu-id="fbf1e-157">MOŽNOST NATIVE_COMPILATION</span><span class="sxs-lookup"><span data-stu-id="fbf1e-157">NATIVE_COMPILATION</span></span>
* <span data-ttu-id="fbf1e-158">Svázání se SCHÉMATEM: význam tabulek, které hello uložené procedury nelze mít jejich definice sloupců změnit žádným způsobem, který by došlo k ovlivnění hello uložené procedury, není-li vyřadit hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-158">SCHEMABINDING: meaning tables that hello stored procedure cannot have their column definitions changed in any way that would affect hello stored procedure, unless you drop hello stored procedure.</span></span>

<span data-ttu-id="fbf1e-159">Nativní modul musí používat jednu velký [ATOMICKÝCH bloků](http://msdn.microsoft.com/library/dn452281.aspx) pro správu transakce.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-159">A native module must use one big [ATOMIC blocks](http://msdn.microsoft.com/library/dn452281.aspx) for transaction management.</span></span> <span data-ttu-id="fbf1e-160">Neexistuje žádný atribut role pro explicitní BEGIN TRANSACTION, nebo pro vrácení transakce zpět.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-160">There is no role for an explicit BEGIN TRANSACTION, or for ROLLBACK TRANSACTION.</span></span> <span data-ttu-id="fbf1e-161">Pokud váš kód zjistí narušení obchodní pravidlo, jej můžete ukončit blok atomic hello s [THROW](http://msdn.microsoft.com/library/ee677615.aspx) příkaz.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-161">If your code detects a violation of a business rule, it can terminate hello atomic block with a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) statement.</span></span>

### <a name="typical-create-procedure-for-natively-compiled"></a><span data-ttu-id="fbf1e-162">Typické CREATE PROCEDURE pro nativně kompilované</span><span class="sxs-lookup"><span data-stu-id="fbf1e-162">Typical CREATE PROCEDURE for natively compiled</span></span>
<span data-ttu-id="fbf1e-163">Obvykle hello T-SQL toocreate nativně kompilované uložené procedury je podobné toohello následující šablony:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-163">Typically hello T-SQL toocreate a natively compiled stored procedure is similar toohello following template:</span></span>

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

* <span data-ttu-id="fbf1e-164">Pro hello TRANSACTION_ISOLATION_LEVEL je SNÍMEK hello nejběžnější hodnotu hello nativně kompilované uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-164">For hello TRANSACTION_ISOLATION_LEVEL, SNAPSHOT is hello most common value for hello natively compiled stored procedure.</span></span> <span data-ttu-id="fbf1e-165">Ale podmnožinu hello jiné hodnoty jsou podporovány také:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-165">However,  a subset of hello other values are also supported:</span></span>
  
  * <span data-ttu-id="fbf1e-166">REPEATABLE READ</span><span class="sxs-lookup"><span data-stu-id="fbf1e-166">REPEATABLE READ</span></span>
  * <span data-ttu-id="fbf1e-167">SERIALIZOVATELNÉ</span><span class="sxs-lookup"><span data-stu-id="fbf1e-167">SERIALIZABLE</span></span>
* <span data-ttu-id="fbf1e-168">Hello hodnotu jazyka musí být přítomen v zobrazení sys.languages hello.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-168">hello LANGUAGE value must be present in hello sys.languages view.</span></span>

### <a name="how-toomigrate-a-stored-procedure"></a><span data-ttu-id="fbf1e-169">Jak toomigrate uložené procedury</span><span class="sxs-lookup"><span data-stu-id="fbf1e-169">How toomigrate a stored procedure</span></span>
<span data-ttu-id="fbf1e-170">kroky migrace Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-170">hello migration steps are:</span></span>

1. <span data-ttu-id="fbf1e-171">Získáte hello CREATE PROCEDURE skriptu toohello regulární interpretovaný uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-171">Obtain hello CREATE PROCEDURE script toohello regular interpreted stored procedure.</span></span>
2. <span data-ttu-id="fbf1e-172">Přepisování jeho předchozí šablonu záhlaví toomatch hello.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-172">Rewrite its header toomatch hello previous template.</span></span>
3. <span data-ttu-id="fbf1e-173">Ověření, zda hello uložené procedury kód T-SQL používá všechny funkce, které nejsou podporovány pro nativně kompilované uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-173">Ascertain whether hello stored procedure T-SQL code uses any features that are not supported for natively compiled stored procedures.</span></span> <span data-ttu-id="fbf1e-174">Implementace řešení, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-174">Implement workarounds if necessary.</span></span>
   
   * <span data-ttu-id="fbf1e-175">Podrobnosti najdete v tématu [problémy s migrací pro nativně kompilované uložené procedury](http://msdn.microsoft.com/library/dn296678.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-175">For details see [Migration Issues for Natively Compiled Stored Procedures](http://msdn.microsoft.com/library/dn296678.aspx).</span></span>
4. <span data-ttu-id="fbf1e-176">Přejmenujte hello staré uložené procedury pomocí procedura SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-176">Rename hello old stored procedure by using SP_RENAME.</span></span> <span data-ttu-id="fbf1e-177">Nebo jednoduše ho VYŘADIT.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-177">Or simply DROP it.</span></span>
5. <span data-ttu-id="fbf1e-178">Spusťte skript upravená vytvořit postup T-SQL.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-178">Run your edited CREATE PROCEDURE T-SQL script.</span></span>

## <a name="step-6-run-your-workload-in-test"></a><span data-ttu-id="fbf1e-179">Krok 6: Spuštění úlohy v testu</span><span class="sxs-lookup"><span data-stu-id="fbf1e-179">Step 6: Run your workload in test</span></span>
<span data-ttu-id="fbf1e-180">Spusťte úlohu ve vaší databázi test, který je podobný toohello zatížení, které běží v produkční databázi.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-180">Run a workload in your test database that is similar toohello workload that runs in your production database.</span></span> <span data-ttu-id="fbf1e-181">To by měl odhalit hello výkonnější dosáhnout používání funkce hello v paměti pro tabulky a uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-181">This should reveal hello performance gain achieved by your use of hello In-Memory feature for tables and stored procedures.</span></span>

<span data-ttu-id="fbf1e-182">Hlavní atributy hello úlohy jsou:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-182">Major attributes of hello workload are:</span></span>

* <span data-ttu-id="fbf1e-183">Počet souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-183">Number of concurrent connections.</span></span>
* <span data-ttu-id="fbf1e-184">Poměr pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-184">Read/write ratio.</span></span>

<span data-ttu-id="fbf1e-185">tootailor a spuštění hello testovací úlohy, zvažte použití hello užitečný ostress.exe nástroj, který ukazuje [zde](sql-database-in-memory.md).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-185">tootailor and run hello test workload, consider using hello handy ostress.exe tool, which illustrated in [here](sql-database-in-memory.md).</span></span>

<span data-ttu-id="fbf1e-186">latence sítě toominimize, spusťte test v hello stejné zeměpisné oblasti Azure kde hello databáze existuje.</span><span class="sxs-lookup"><span data-stu-id="fbf1e-186">toominimize network latency, run your test in hello same Azure geographic region where hello database exists.</span></span>

## <a name="step-7-post-implementation-monitoring"></a><span data-ttu-id="fbf1e-187">Krok 7: Po implementaci monitorování</span><span class="sxs-lookup"><span data-stu-id="fbf1e-187">Step 7: Post-implementation monitoring</span></span>
<span data-ttu-id="fbf1e-188">Vezměte v úvahu monitorování výkonu důsledky hello vaší implementace v paměti v produkčním prostředí:</span><span class="sxs-lookup"><span data-stu-id="fbf1e-188">Consider monitoring hello performance effects of your In-Memory implementations in production:</span></span>

* <span data-ttu-id="fbf1e-189">[Monitorování úložiště v paměti](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="fbf1e-189">[Monitor In-Memory Storage](sql-database-in-memory-oltp-monitoring.md).</span></span>
* [<span data-ttu-id="fbf1e-190">Monitorování databáze Azure SQL Database pomocí zobrazení dynamické správy</span><span class="sxs-lookup"><span data-stu-id="fbf1e-190">Monitoring Azure SQL Database using dynamic management views</span></span>](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a><span data-ttu-id="fbf1e-191">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="fbf1e-191">Related links</span></span>
* [<span data-ttu-id="fbf1e-192">Paměť OLTP (v paměti optimalizace)</span><span class="sxs-lookup"><span data-stu-id="fbf1e-192">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
* [<span data-ttu-id="fbf1e-193">Úvod tooNatively kompilované uložené procedury</span><span class="sxs-lookup"><span data-stu-id="fbf1e-193">Introduction tooNatively Compiled Stored Procedures</span></span>](http://msdn.microsoft.com/library/dn133184.aspx)
* [<span data-ttu-id="fbf1e-194">Advisor optimalizace paměti</span><span class="sxs-lookup"><span data-stu-id="fbf1e-194">Memory Optimization Advisor</span></span>](http://msdn.microsoft.com/library/dn284308.aspx)

