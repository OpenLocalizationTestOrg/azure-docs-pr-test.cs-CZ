---
title: "aaaHow toouse dávkování tooimprove výkonu aplikací Azure SQL Database"
description: "Hello tématu poskytuje důkaz, že dávkování databáze operations výrazně imroves hello rychlost a škálovatelnost aplikací Azure SQL Database. I když tyto dávkování techniky fungovat pro libovolnou databázi systému SQL Server, hello cílem tohoto článku hello je v Azure."
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 124b203ee69c595f0813852ff09ef9ec6841233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a><span data-ttu-id="ece2e-104">Jak toouse dávkování výkon aplikace tooimprove databáze SQL</span><span class="sxs-lookup"><span data-stu-id="ece2e-104">How toouse batching tooimprove SQL Database application performance</span></span>
<span data-ttu-id="ece2e-105">Dávkování operací tooAzure SQL Database výrazně zlepšuje hello výkon a škálovatelnost aplikací.</span><span class="sxs-lookup"><span data-stu-id="ece2e-105">Batching operations tooAzure SQL Database significantly improves hello performance and scalability of your applications.</span></span> <span data-ttu-id="ece2e-106">V pořadí toounderstand hello výhody hello první část tohoto článku popisuje některé vzorové výsledky testů, které porovnávají požadavky sekvenční a dávkové tooa databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="ece2e-106">In order toounderstand hello benefits, hello first part of this article covers some sample test results that compare sequential and batched requests tooa SQL Database.</span></span> <span data-ttu-id="ece2e-107">Hello zbývající části článku hello se zobrazuje hello techniky, scénáře a aspekty toohelp toouse dávkování úspěšně v aplikacích Azure.</span><span class="sxs-lookup"><span data-stu-id="ece2e-107">hello remainder of hello article shows hello techniques, scenarios, and considerations toohelp you toouse batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="ece2e-108">Proč je dávkování důležité pro databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="ece2e-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="ece2e-109">Dávkování volání tooa vzdálené služby je dobře známé strategie pro zvýšení výkonu a škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="ece2e-109">Batching calls tooa remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="ece2e-110">Existuje se vyřešilo zpracování náklady tooany interakce s vzdálené služby, jako je například serializace, přenos v síti a deserializace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-110">There are fixed processing costs tooany interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="ece2e-111">Balení mnoho samostatné transakcí do jedné dávkové minimalizuje tyto náklady.</span><span class="sxs-lookup"><span data-stu-id="ece2e-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="ece2e-112">V tomto dokumentu chceme tooexamine různé dávkování strategie SQL Database a scénáře.</span><span class="sxs-lookup"><span data-stu-id="ece2e-112">In this paper, we want tooexamine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="ece2e-113">I když tyto strategie jsou také důležité pro místní aplikace, které používají systém SQL Server, tady je několik důvodů pro zvýraznění hello použití dávkování pro databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="ece2e-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting hello use of batching for SQL Database:</span></span>

* <span data-ttu-id="ece2e-114">Je potenciálně vyšší latence sítě přístup k databázi SQL, zejména pokud v přístupu k databázi SQL z mimo hello stejného datového centra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ece2e-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside hello same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="ece2e-115">Hello víceklientské vlastnosti SQL Database znamená, že hello efektivitu hello dat přístup k vrstvě korelaci toohello celkovou škálovatelnost hello databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-115">hello multitenant characteristics of SQL Database means that hello efficiency of hello data access layer correlates toohello overall scalability of hello database.</span></span> <span data-ttu-id="ece2e-116">Databáze SQL zabránit přivlastňuje databáze prostředků toohello úkor ostatních klientů žádné jednoho klienta nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="ece2e-116">SQL Database must prevent any single tenant/user from monopolizing database resources toohello detriment of other tenants.</span></span> <span data-ttu-id="ece2e-117">Databáze SQL v odpovědi toousage překračující předdefinované kvóty, můžete snížit propustnost nebo odpovědět s omezení výjimky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-117">In response toousage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="ece2e-118">Efektivitu své činnosti, jako je například dávkování, povolte je toodo další práci v databázi SQL dříve, než dorazila těchto mezních hodnot.</span><span class="sxs-lookup"><span data-stu-id="ece2e-118">Efficiencies, such as batching, enable you toodo more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="ece2e-119">Dávkování je také efektivní pro architektury, které používají více databází (horizontálního dělení).</span><span class="sxs-lookup"><span data-stu-id="ece2e-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="ece2e-120">Hello efektivitu interakce se jednotlivých jednotek databáze je stále klíčovým faktorem vaší celkovou škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="ece2e-120">hello efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="ece2e-121">Jednou z výhod hello používání databáze SQL je, že nemáte servery hello toomanage databázi hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="ece2e-121">One of hello benefits of using SQL Database is that you don’t have toomanage hello servers that host hello database.</span></span> <span data-ttu-id="ece2e-122">Však této spravované infrastruktury také znamená, že můžete mít toothink jinak o optimalizace databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-122">However, this managed infrastructure also means that you have toothink differently about database optimizations.</span></span> <span data-ttu-id="ece2e-123">Už můžete zobrazit tooimprove hello databáze hardware nebo síť infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ece2e-123">You can no longer look tooimprove hello database hardware or network infrastructure.</span></span> <span data-ttu-id="ece2e-124">Microsoft Azure řídí těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="ece2e-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="ece2e-125">Hello hlavní oblasti, která můžete řídit je, jak vaše aplikace komunikuje s databází SQL.</span><span class="sxs-lookup"><span data-stu-id="ece2e-125">hello main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="ece2e-126">Dávkování je jedním z těchto optimalizace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="ece2e-127">první část Hello hello dokumentu prověří různé dávkování techniky pro aplikace .NET, které používají SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ece2e-127">hello first part of hello paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="ece2e-128">Hello poslední dvě části se věnují dávkování pokyny a scénáře.</span><span class="sxs-lookup"><span data-stu-id="ece2e-128">hello last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="ece2e-129">Dávkování strategie</span><span class="sxs-lookup"><span data-stu-id="ece2e-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="ece2e-130">Všimněte si o výsledcích časování v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="ece2e-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="ece2e-131">Výsledky nejsou srovnávacích testů, ale jsou určené tooshow **relativní výkon**.</span><span class="sxs-lookup"><span data-stu-id="ece2e-131">Results are not benchmarks but are meant tooshow **relative performance**.</span></span> <span data-ttu-id="ece2e-132">Časování jsou založené na v průměru minimálně 10 test spustí.</span><span class="sxs-lookup"><span data-stu-id="ece2e-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="ece2e-133">Operace se vloží do prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="ece2e-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="ece2e-134">Tyto testy byly měřená pre-V12 a neodpovídají nutně toothroughput, ke kterému může dojít v databázi V12 pomocí hello nové [úrovních služeb](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="ece2e-134">These tests were measured pre-V12, and they do not necessarily correspond toothroughput that you might experience in a V12 database using hello new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="ece2e-135">Hello relativní výhodou hello dávkování technika by mělo být podobné.</span><span class="sxs-lookup"><span data-stu-id="ece2e-135">hello relative benefit of hello batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="ece2e-136">Transakce</span><span class="sxs-lookup"><span data-stu-id="ece2e-136">Transactions</span></span>
<span data-ttu-id="ece2e-137">Podle všeho neobvyklé toobegin o dávkování podle hovoříte o transakce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-137">It seems strange toobegin a review of batching by discussing transactions.</span></span> <span data-ttu-id="ece2e-138">Ale hello použití transakce na straně klienta má vliv dávkování jemně straně serveru, který zlepšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="ece2e-138">But hello use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="ece2e-139">A transakcí lze přidat pouze zadání několika řádků kódu, takže poskytují rychlý způsob tooimprove výkonu sekvenčních operací.</span><span class="sxs-lookup"><span data-stu-id="ece2e-139">And transactions can be added with only a few lines of code, so they provide a fast way tooimprove performance of sequential operations.</span></span>

<span data-ttu-id="ece2e-140">Zvažte hello následující kód C#, který obsahuje posloupnost vložení a aktualizace operací na jednoduché tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-140">Consider hello following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="ece2e-141">Hello následující kód ADO.NET postupně provádí tyto operace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-141">hello following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="ece2e-142">Hello toooptimize nejlepší způsob, jak tento kód je tooimplement určitou formu klienta dávkování těchto volání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-142">hello best way toooptimize this code is tooimplement some form of client-side batching of these calls.</span></span> <span data-ttu-id="ece2e-143">Ale výkonu hello tooincrease jednoduchý způsob tohoto kódu při jednoduše zabalení hello pořadí volání v transakci.</span><span class="sxs-lookup"><span data-stu-id="ece2e-143">But there is a simple way tooincrease hello performance of this code by simply wrapping hello sequence of calls in a transaction.</span></span> <span data-ttu-id="ece2e-144">Tady je hello stejný kód, který používá transakce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-144">Here is hello same code that uses a transaction.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

<span data-ttu-id="ece2e-145">Transakce se používá ve skutečnosti v obou těchto příkladech.</span><span class="sxs-lookup"><span data-stu-id="ece2e-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="ece2e-146">V prvním příkladu Dobrý den každý jednotlivých volání je implicitní transakci.</span><span class="sxs-lookup"><span data-stu-id="ece2e-146">In hello first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="ece2e-147">V druhém příkladu hello explicitní transakce zabalí všechny hello volání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-147">In hello second example, an explicit transaction wraps all of hello calls.</span></span> <span data-ttu-id="ece2e-148">Za dokumentaci hello hello [předběžné transakčního protokolu](https://msdn.microsoft.com/library/ms186259.aspx), jsou záznamy protokolu vyprázdněn toohello disku při hello transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="ece2e-148">Per hello documentation for hello [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed toohello disk when hello transaction commits.</span></span> <span data-ttu-id="ece2e-149">Proto zahrnutím další volání v transakci hello zápisu toohello transakčního protokolu můžete počkat, až je hello transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="ece2e-149">So by including more calls in a transaction, hello write toohello transaction log can delay until hello transaction is committed.</span></span> <span data-ttu-id="ece2e-150">V důsledku toho jsou povolení dávkování pro hello zápisy toohello serveru transakčního protokolu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-150">In effect, you are enabling batching for hello writes toohello server’s transaction log.</span></span>

<span data-ttu-id="ece2e-151">Hello následující tabulka uvádí některé výsledky testování ad hoc.</span><span class="sxs-lookup"><span data-stu-id="ece2e-151">hello following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="ece2e-152">Hello testy prováděné hello stejné sekvenční vloží s i bez transakce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-152">hello tests performed hello same sequential inserts with and without transactions.</span></span> <span data-ttu-id="ece2e-153">Pro další perspektivy spustili hello první sada testů vzdáleně z databáze toohello přenosných počítačů v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ece2e-153">For more perspective, hello first set of tests ran remotely from a laptop toohello database in Microsoft Azure.</span></span> <span data-ttu-id="ece2e-154">Hello druhá sada testů spustili z cloudové služby a databáze, že oba nacházejí v rámci hello stejné datacenter Microsoft Azure (západní USA).</span><span class="sxs-lookup"><span data-stu-id="ece2e-154">hello second set of tests ran from a cloud service and database that both resided within hello same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="ece2e-155">Hello následující tabulka uvádí hello doba v milisekundách sekvenční operace INSERT a bez transakce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-155">hello following table shows hello duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="ece2e-156">**Místní tooAzure**:</span><span class="sxs-lookup"><span data-stu-id="ece2e-156">**On-Premises tooAzure**:</span></span>

| <span data-ttu-id="ece2e-157">Operace</span><span class="sxs-lookup"><span data-stu-id="ece2e-157">Operations</span></span> | <span data-ttu-id="ece2e-158">Žádná transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-158">No Transaction (ms)</span></span> | <span data-ttu-id="ece2e-159">Transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-160">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-160">1</span></span> |<span data-ttu-id="ece2e-161">130</span><span class="sxs-lookup"><span data-stu-id="ece2e-161">130</span></span> |<span data-ttu-id="ece2e-162">402</span><span class="sxs-lookup"><span data-stu-id="ece2e-162">402</span></span> |
| <span data-ttu-id="ece2e-163">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-163">10</span></span> |<span data-ttu-id="ece2e-164">1208</span><span class="sxs-lookup"><span data-stu-id="ece2e-164">1208</span></span> |<span data-ttu-id="ece2e-165">1226</span><span class="sxs-lookup"><span data-stu-id="ece2e-165">1226</span></span> |
| <span data-ttu-id="ece2e-166">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-166">100</span></span> |<span data-ttu-id="ece2e-167">12662</span><span class="sxs-lookup"><span data-stu-id="ece2e-167">12662</span></span> |<span data-ttu-id="ece2e-168">10395</span><span class="sxs-lookup"><span data-stu-id="ece2e-168">10395</span></span> |
| <span data-ttu-id="ece2e-169">1000</span><span class="sxs-lookup"><span data-stu-id="ece2e-169">1000</span></span> |<span data-ttu-id="ece2e-170">128852</span><span class="sxs-lookup"><span data-stu-id="ece2e-170">128852</span></span> |<span data-ttu-id="ece2e-171">102917</span><span class="sxs-lookup"><span data-stu-id="ece2e-171">102917</span></span> |

<span data-ttu-id="ece2e-172">**Azure tooAzure (stejného datového centra)**:</span><span class="sxs-lookup"><span data-stu-id="ece2e-172">**Azure tooAzure (same datacenter)**:</span></span>

| <span data-ttu-id="ece2e-173">Operace</span><span class="sxs-lookup"><span data-stu-id="ece2e-173">Operations</span></span> | <span data-ttu-id="ece2e-174">Žádná transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-174">No Transaction (ms)</span></span> | <span data-ttu-id="ece2e-175">Transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-176">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-176">1</span></span> |<span data-ttu-id="ece2e-177">21</span><span class="sxs-lookup"><span data-stu-id="ece2e-177">21</span></span> |<span data-ttu-id="ece2e-178">26</span><span class="sxs-lookup"><span data-stu-id="ece2e-178">26</span></span> |
| <span data-ttu-id="ece2e-179">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-179">10</span></span> |<span data-ttu-id="ece2e-180">220</span><span class="sxs-lookup"><span data-stu-id="ece2e-180">220</span></span> |<span data-ttu-id="ece2e-181">56</span><span class="sxs-lookup"><span data-stu-id="ece2e-181">56</span></span> |
| <span data-ttu-id="ece2e-182">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-182">100</span></span> |<span data-ttu-id="ece2e-183">2145</span><span class="sxs-lookup"><span data-stu-id="ece2e-183">2145</span></span> |<span data-ttu-id="ece2e-184">341</span><span class="sxs-lookup"><span data-stu-id="ece2e-184">341</span></span> |
| <span data-ttu-id="ece2e-185">1000</span><span class="sxs-lookup"><span data-stu-id="ece2e-185">1000</span></span> |<span data-ttu-id="ece2e-186">21479</span><span class="sxs-lookup"><span data-stu-id="ece2e-186">21479</span></span> |<span data-ttu-id="ece2e-187">2756</span><span class="sxs-lookup"><span data-stu-id="ece2e-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-188">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-188">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-189">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-189">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-190">Na základě výsledků testu předchozí hello zabalení jedné operace v transakci ve skutečnosti sníží výkon.</span><span class="sxs-lookup"><span data-stu-id="ece2e-190">Based on hello previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="ece2e-191">Ale můžete zvýšit hello počet operací v rámci jedné transakce, zlepšování výkonu hello stane více označen.</span><span class="sxs-lookup"><span data-stu-id="ece2e-191">But as you increase hello number of operations within a single transaction, hello performance improvement becomes more marked.</span></span> <span data-ttu-id="ece2e-192">rozdíly ve výkonnosti Hello je také významnější, když dojde k všechny operace v rámci datového centra hello Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ece2e-192">hello performance difference is also more noticeable when all operations occur within hello Microsoft Azure datacenter.</span></span> <span data-ttu-id="ece2e-193">Hello zvýší latence používání databáze SQL z datového centra Microsoft Azure mimo hello ruší hello výkonnější použití transakcí.</span><span class="sxs-lookup"><span data-stu-id="ece2e-193">hello increased latency of using SQL Database from outside hello Microsoft Azure datacenter overshadows hello performance gain of using transactions.</span></span>

<span data-ttu-id="ece2e-194">I když hello použití transakcí může zvýšit výkon, pokračovat příliš[sledovat osvědčené postupy pro připojení a transakce](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="ece2e-194">Although hello use of transactions can increase performance, continue too[observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="ece2e-195">Zachovat hello transakce co nejkratší připojení k databázi možná a zavřít hello po dokončení práce hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-195">Keep hello transaction as short as possible, and close hello database connection after hello work completes.</span></span> <span data-ttu-id="ece2e-196">pomocí příkazu v předchozím příkladu hello Hello zaručuje, že hello připojení je ukončeno po dokončení hello následné kód bloku.</span><span class="sxs-lookup"><span data-stu-id="ece2e-196">hello using statement in hello previous example assures that hello connection is closed when hello subsequent code block completes.</span></span>

<span data-ttu-id="ece2e-197">Hello předchozí příklad ukazuje, že přidáte kód ADO.NET tooany místní transakce se dvěma řádky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-197">hello previous example demonstrates that you can add a local transaction tooany ADO.NET code with two lines.</span></span> <span data-ttu-id="ece2e-198">Transakce nabízejí rychlý způsob tooimprove hello výkon kód, který umožňuje sekvenční vložit, aktualizovat a odstranit operace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-198">Transactions offer a quick way tooimprove hello performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="ece2e-199">Ale hello nejrychlejší výkonu, zvažte změnu hello kód další tootake výhod dávkování na straně klienta, jako jsou parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-199">However, for hello fastest performance, consider changing hello code further tootake advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="ece2e-200">Další informace o transakcích v ADO.NET naleznete v tématu [místní transakce v ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="ece2e-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="ece2e-201">Parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="ece2e-201">Table-valued parameters</span></span>
<span data-ttu-id="ece2e-202">Parametry s hodnotou tabulky podporují uživatelem definovaná tabulka typy jako parametry příkazy jazyka Transact-SQL, uložené procedury a funkce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="ece2e-203">Tato technika dávkování na straně klienta vám umožní toosend více řádků dat v rámci parametr s hodnotou tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-203">This client-side batching technique allows you toosend multiple rows of data within hello table-valued parameter.</span></span> <span data-ttu-id="ece2e-204">parametry s hodnotou tabulky toouse nejdříve definovat typ tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-204">toouse table-valued parameters, first define a table type.</span></span> <span data-ttu-id="ece2e-205">Následující příkaz jazyka Transact-SQL Hello vytvoří typ tabulky s názvem **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="ece2e-205">hello following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="ece2e-206">V kódu, můžete vytvořit **DataTable** s hello přesnou stejné názvy a typy hello typ tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-206">In code, you create a **DataTable** with hello exact same names and types of hello table type.</span></span> <span data-ttu-id="ece2e-207">To předat **DataTable** parametr v textu dotazu nebo uložené proceduře volání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="ece2e-208">Hello následující příklad ukazuje, tento postup:</span><span class="sxs-lookup"><span data-stu-id="ece2e-208">hello following example shows this technique:</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. hello following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

<span data-ttu-id="ece2e-209">V předchozím příkladu hello hello **SqlCommand** objekt vloží řádky z parametr s hodnotou tabulky  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="ece2e-209">In hello previous example, hello **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="ece2e-210">Hello vytvořili **DataTable** objektu je přiřazen toothis parametr s hello **SqlCommand.Parameters.Add** metoda.</span><span class="sxs-lookup"><span data-stu-id="ece2e-210">hello previously created **DataTable** object is assigned toothis parameter with hello **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="ece2e-211">Dávkování hello vložení v jednom volání výrazně zvyšuje výkon hello přes sekvenční vložení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-211">Batching hello inserts in one call significantly increases hello performance over sequential inserts.</span></span>

<span data-ttu-id="ece2e-212">tooimprove hello předchozí příklad další, použijte uloženou proceduru místo příkaz založený na textu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-212">tooimprove hello previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="ece2e-213">Následující příkaz Transact-SQL Hello vytvoří uložené procedury, která přebírá hello **SimpleTestTableType** parametr s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-213">hello following Transact-SQL command creates a stored procedure that takes hello **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="ece2e-214">Změňte hello **SqlCommand** objektu deklarace v hello předchozí kód příklad toohello následující.</span><span class="sxs-lookup"><span data-stu-id="ece2e-214">Then change hello **SqlCommand** object declaration in hello previous code example toohello following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="ece2e-215">Ve většině případů parametry s hodnotou tabulky mít ekvivalentní, nebo lepší výkon než jinými technikami, dávkování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="ece2e-216">Parametry s hodnotou tabulky jsou často vhodnější, protože jsou flexibilnější než jiné možnosti.</span><span class="sxs-lookup"><span data-stu-id="ece2e-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="ece2e-217">Jiné postupy, jako je například hromadného kopírování SQL, například povolit pouze hello vkládání nových řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-217">For example, other techniques, such as SQL bulk copy, only permit hello insertion of new rows.</span></span> <span data-ttu-id="ece2e-218">Ale s parametry s hodnotou tabulky, můžete použít logiku v hello uložené procedury toodetermine řádky jsou aktualizace a který se vloží.</span><span class="sxs-lookup"><span data-stu-id="ece2e-218">But with table-valued parameters, you can use logic in hello stored procedure toodetermine which rows are updates and which are inserts.</span></span> <span data-ttu-id="ece2e-219">typ tabulky Hello může být také upravené toocontain sloupec "Operace", která udává, zda text hello zadat řádek by měla být vložit, aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="ece2e-219">hello table type can also be modified toocontain an “Operation” column that indicates whether hello specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="ece2e-220">Hello následující tabulka ukazuje výsledky testů ad-hoc pro použití hello parametry s hodnotou tabulky v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="ece2e-220">hello following table shows ad-hoc test results for hello use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="ece2e-221">Operace</span><span class="sxs-lookup"><span data-stu-id="ece2e-221">Operations</span></span> | <span data-ttu-id="ece2e-222">Místní tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-222">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="ece2e-223">Azure stejného datového centra (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-224">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-224">1</span></span> |<span data-ttu-id="ece2e-225">124</span><span class="sxs-lookup"><span data-stu-id="ece2e-225">124</span></span> |<span data-ttu-id="ece2e-226">32</span><span class="sxs-lookup"><span data-stu-id="ece2e-226">32</span></span> |
| <span data-ttu-id="ece2e-227">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-227">10</span></span> |<span data-ttu-id="ece2e-228">131</span><span class="sxs-lookup"><span data-stu-id="ece2e-228">131</span></span> |<span data-ttu-id="ece2e-229">25</span><span class="sxs-lookup"><span data-stu-id="ece2e-229">25</span></span> |
| <span data-ttu-id="ece2e-230">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-230">100</span></span> |<span data-ttu-id="ece2e-231">338</span><span class="sxs-lookup"><span data-stu-id="ece2e-231">338</span></span> |<span data-ttu-id="ece2e-232">51</span><span class="sxs-lookup"><span data-stu-id="ece2e-232">51</span></span> |
| <span data-ttu-id="ece2e-233">1000</span><span class="sxs-lookup"><span data-stu-id="ece2e-233">1000</span></span> |<span data-ttu-id="ece2e-234">2615</span><span class="sxs-lookup"><span data-stu-id="ece2e-234">2615</span></span> |<span data-ttu-id="ece2e-235">382</span><span class="sxs-lookup"><span data-stu-id="ece2e-235">382</span></span> |
| <span data-ttu-id="ece2e-236">10000</span><span class="sxs-lookup"><span data-stu-id="ece2e-236">10000</span></span> |<span data-ttu-id="ece2e-237">23830</span><span class="sxs-lookup"><span data-stu-id="ece2e-237">23830</span></span> |<span data-ttu-id="ece2e-238">3586</span><span class="sxs-lookup"><span data-stu-id="ece2e-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-239">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-239">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-240">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-240">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-241">Hello výkonnější z dávkování je okamžitě zřejmá.</span><span class="sxs-lookup"><span data-stu-id="ece2e-241">hello performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="ece2e-242">V předchozím testu sekvenčních hello 1000 operace trvalo 129 sekund mimo hello datacenter a 21 sekund z v rámci datového centra hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-242">In hello previous sequential test, 1000 operations took 129 seconds outside hello datacenter and 21 seconds from within hello datacenter.</span></span> <span data-ttu-id="ece2e-243">Ale s parametry s hodnotou tabulky 1000 operace trvat jenom 2.6 sekund mimo datové centrum hello a 0,4 sekundy v rámci datového centra hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside hello datacenter and 0.4 seconds within hello datacenter.</span></span>

<span data-ttu-id="ece2e-244">Další informace o parametry s hodnotou tabulky najdete v tématu [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="ece2e-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="ece2e-245">Hromadné kopírování SQL</span><span class="sxs-lookup"><span data-stu-id="ece2e-245">SQL bulk copy</span></span>
<span data-ttu-id="ece2e-246">Hromadné kopírování SQL je jiný způsob tooinsert velké objemy dat do cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-246">SQL bulk copy is another way tooinsert large amounts of data into a target database.</span></span> <span data-ttu-id="ece2e-247">Aplikace .NET mohou použít hello **SqlBulkCopy** třída tooperform hromadné operace vložení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-247">.NET applications can use hello **SqlBulkCopy** class tooperform bulk insert operations.</span></span> <span data-ttu-id="ece2e-248">**SqlBulkCopy** je podobný v funkce toohello nástroj příkazového řádku, **Bcp.exe**, nebo hello příkazu Transact-SQL, **BULK INSERT**.</span><span class="sxs-lookup"><span data-stu-id="ece2e-248">**SqlBulkCopy** is similar in function toohello command-line tool, **Bcp.exe**, or hello Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="ece2e-249">Hello následující příklad kódu ukazuje, jak toobulk kopie hello řádků ve zdroji hello **DataTable**, tabulce, toohello cílové tabulky v systému SQL Server, MyTable.</span><span class="sxs-lookup"><span data-stu-id="ece2e-249">hello following code example shows how toobulk copy hello rows in hello source **DataTable**, table, toohello destination table in SQL Server, MyTable.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

<span data-ttu-id="ece2e-250">Existují případy, kdy je hromadné kopírování upřednostňované nad parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="ece2e-251">V tématu hello srovnávací tabulka s hodnotou tabulky parametrů versus BULK INSERT operace v tématu hello [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="ece2e-251">See hello comparison table of Table-Valued parameters versus BULK INSERT operations in hello topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="ece2e-252">Hello následující výsledky testů ad-hoc zobrazit výkon hello dávkování s **SqlBulkCopy** v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="ece2e-252">hello following ad-hoc test results show hello performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="ece2e-253">Operace</span><span class="sxs-lookup"><span data-stu-id="ece2e-253">Operations</span></span> | <span data-ttu-id="ece2e-254">Místní tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-254">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="ece2e-255">Azure stejného datového centra (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-256">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-256">1</span></span> |<span data-ttu-id="ece2e-257">433</span><span class="sxs-lookup"><span data-stu-id="ece2e-257">433</span></span> |<span data-ttu-id="ece2e-258">57</span><span class="sxs-lookup"><span data-stu-id="ece2e-258">57</span></span> |
| <span data-ttu-id="ece2e-259">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-259">10</span></span> |<span data-ttu-id="ece2e-260">441</span><span class="sxs-lookup"><span data-stu-id="ece2e-260">441</span></span> |<span data-ttu-id="ece2e-261">32</span><span class="sxs-lookup"><span data-stu-id="ece2e-261">32</span></span> |
| <span data-ttu-id="ece2e-262">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-262">100</span></span> |<span data-ttu-id="ece2e-263">636</span><span class="sxs-lookup"><span data-stu-id="ece2e-263">636</span></span> |<span data-ttu-id="ece2e-264">53</span><span class="sxs-lookup"><span data-stu-id="ece2e-264">53</span></span> |
| <span data-ttu-id="ece2e-265">1000</span><span class="sxs-lookup"><span data-stu-id="ece2e-265">1000</span></span> |<span data-ttu-id="ece2e-266">2535</span><span class="sxs-lookup"><span data-stu-id="ece2e-266">2535</span></span> |<span data-ttu-id="ece2e-267">341</span><span class="sxs-lookup"><span data-stu-id="ece2e-267">341</span></span> |
| <span data-ttu-id="ece2e-268">10000</span><span class="sxs-lookup"><span data-stu-id="ece2e-268">10000</span></span> |<span data-ttu-id="ece2e-269">21605</span><span class="sxs-lookup"><span data-stu-id="ece2e-269">21605</span></span> |<span data-ttu-id="ece2e-270">2737</span><span class="sxs-lookup"><span data-stu-id="ece2e-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-271">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-271">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-272">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-272">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-273">V menší velikosti dávky parametry s hodnotou tabulky hello použití outperformed hello **SqlBulkCopy** třídy.</span><span class="sxs-lookup"><span data-stu-id="ece2e-273">In smaller batch sizes, hello use table-valued parameters outperformed hello **SqlBulkCopy** class.</span></span> <span data-ttu-id="ece2e-274">Ale **SqlBulkCopy** provést 12-31 % rychlejší než parametry s hodnotou tabulky pro testy hello 1 000 a 10 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for hello tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="ece2e-275">Jako parametry s hodnotou tabulky **SqlBulkCopy** je vhodný pro dávkové vložení, zejména v případě, že porovnání výkonu toohello operací jiný-zpracovat v dávce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared toohello performance of non-batched operations.</span></span>

<span data-ttu-id="ece2e-276">Další informace o hromadné kopírování v ADO.NET naleznete v tématu [operace hromadného kopírování v systému SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="ece2e-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="ece2e-277">Příkazy s parametry vložit více řádků</span><span class="sxs-lookup"><span data-stu-id="ece2e-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="ece2e-278">Jeden alternativní pro malé dávky je tooconstruct velké parametry příkazu INSERT, která vloží více řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-278">One alternative for small batches is tooconstruct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="ece2e-279">Hello následující příklad kódu ukazuje tento postup.</span><span class="sxs-lookup"><span data-stu-id="ece2e-279">hello following code example demonstrates this technique.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


<span data-ttu-id="ece2e-280">V tomto příkladu je určená tooshow hello základní koncept.</span><span class="sxs-lookup"><span data-stu-id="ece2e-280">This example is meant tooshow hello basic concept.</span></span> <span data-ttu-id="ece2e-281">Realističtější scénář by projít řetězec dotazu hello požadované entity tooconstruct hello a parametry příkazu hello současně.</span><span class="sxs-lookup"><span data-stu-id="ece2e-281">A more realistic scenario would loop through hello required entities tooconstruct hello query string and hello command parameters simultaneously.</span></span> <span data-ttu-id="ece2e-282">Jste omezeni tooa celkem parametry dotazu 2100, toto nastavení omezuje hello celkový počet řádků, které lze zpracovat tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="ece2e-282">You are limited tooa total of 2100 query parameters, so this limits hello total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="ece2e-283">Hello následující výsledky testování ad-hoc zobrazit hello výkonu tento typ příkazu insert v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="ece2e-283">hello following ad-hoc test results show hello performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="ece2e-284">Operace</span><span class="sxs-lookup"><span data-stu-id="ece2e-284">Operations</span></span> | <span data-ttu-id="ece2e-285">Parametry s hodnotou tabulky (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="ece2e-286">Vložení jedním příkazem (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-287">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-287">1</span></span> |<span data-ttu-id="ece2e-288">32</span><span class="sxs-lookup"><span data-stu-id="ece2e-288">32</span></span> |<span data-ttu-id="ece2e-289">20</span><span class="sxs-lookup"><span data-stu-id="ece2e-289">20</span></span> |
| <span data-ttu-id="ece2e-290">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-290">10</span></span> |<span data-ttu-id="ece2e-291">30</span><span class="sxs-lookup"><span data-stu-id="ece2e-291">30</span></span> |<span data-ttu-id="ece2e-292">25</span><span class="sxs-lookup"><span data-stu-id="ece2e-292">25</span></span> |
| <span data-ttu-id="ece2e-293">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-293">100</span></span> |<span data-ttu-id="ece2e-294">33</span><span class="sxs-lookup"><span data-stu-id="ece2e-294">33</span></span> |<span data-ttu-id="ece2e-295">51</span><span class="sxs-lookup"><span data-stu-id="ece2e-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-296">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-296">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-297">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-297">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-298">Tento přístup může být mírně rychlejší pro balíků, které jsou menší než 100 řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="ece2e-299">I když zlepšování hello je malá, tento postup je jinou možnost, která může fungovat i ve vašem scénáři konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece2e-299">Although hello improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="ece2e-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="ece2e-300">DataAdapter</span></span>
<span data-ttu-id="ece2e-301">Hello **DataAdapter** třída vám umožní toomodify **datovou sadu** objekt a potom odeslat změny hello jako operace INSERT, UPDATE a DELETE.</span><span class="sxs-lookup"><span data-stu-id="ece2e-301">hello **DataAdapter** class allows you toomodify a **DataSet** object and then submit hello changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="ece2e-302">Pokud používáte hello **DataAdapter** tímto způsobem je důležité toonote, který samostatné volání jsou vytvářeny pro každou operaci distinct.</span><span class="sxs-lookup"><span data-stu-id="ece2e-302">If you are using hello **DataAdapter** in this manner, it is important toonote that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="ece2e-303">tooimprove výkonu, použijte hello **UpdateBatchSize** vlastnost toohello počet operací, které by měl zpracovat v dávce najednou.</span><span class="sxs-lookup"><span data-stu-id="ece2e-303">tooimprove performance, use hello **UpdateBatchSize** property toohello number of operations that should be batched at a time.</span></span> <span data-ttu-id="ece2e-304">Další informace najdete v tématu [provádění dávkové operace pomocí DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="ece2e-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="ece2e-305">Rozhraní Entity framework</span><span class="sxs-lookup"><span data-stu-id="ece2e-305">Entity framework</span></span>
<span data-ttu-id="ece2e-306">Rozhraní Entity Framework aktuálně nepodporuje dávkování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="ece2e-307">Různé vývojáři v komunitě hello pokusili toodemonstrate řešení, například přepsání hello **SaveChanges** metoda.</span><span class="sxs-lookup"><span data-stu-id="ece2e-307">Different developers in hello community have attempted toodemonstrate workarounds, such as override hello **SaveChanges** method.</span></span> <span data-ttu-id="ece2e-308">Ale hello řešení jsou obvykle toohello komplexní a vlastní aplikace a datového modelu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-308">But hello solutions are typically complex and customized toohello application and data model.</span></span> <span data-ttu-id="ece2e-309">Hello Entity Framework webu codeplex projekt má aktuálně stránky diskuzi na žádost o této funkce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-309">hello Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="ece2e-310">tooview toto pojednání, najdete v části [poznámky ze schůzky návrhu - 2 srpen 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="ece2e-310">tooview this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="ece2e-311">XML</span><span class="sxs-lookup"><span data-stu-id="ece2e-311">XML</span></span>
<span data-ttu-id="ece2e-312">Pro úplnost se domníváme, že je důležité tootalk o XML jako strategie dávkování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-312">For completeness, we feel that it is important tootalk about XML as a batching strategy.</span></span> <span data-ttu-id="ece2e-313">Použití hello XML má však žádné výhody přes jiné metody a několik nevýhody.</span><span class="sxs-lookup"><span data-stu-id="ece2e-313">However, hello use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="ece2e-314">Hello přístup je podobné tootable s hodnotou parametry, ale souboru XML nebo řetězec, je předaná tooa uložené procedury místo uživatelem definovaná tabulka.</span><span class="sxs-lookup"><span data-stu-id="ece2e-314">hello approach is similar tootable-valued parameters, but an XML file or string is passed tooa stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="ece2e-315">Hello uložené procedury analyzuje hello příkazy v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ece2e-315">hello stored procedure parses hello commands in hello stored procedure.</span></span>

<span data-ttu-id="ece2e-316">Existuje několik nevýhody toothis přístup:</span><span class="sxs-lookup"><span data-stu-id="ece2e-316">There are several disadvantages toothis approach:</span></span>

* <span data-ttu-id="ece2e-317">Práce s XML může být náročná a chyba náchylné k chybám.</span><span class="sxs-lookup"><span data-stu-id="ece2e-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="ece2e-318">Analýza hello XML na hello databáze může být náročná na prostředky procesoru.</span><span class="sxs-lookup"><span data-stu-id="ece2e-318">Parsing hello XML on hello database can be CPU-intensive.</span></span>
* <span data-ttu-id="ece2e-319">Ve většině případů je tato metoda pomalejší než parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="ece2e-320">Z těchto důvodů se nedoporučuje hello použití XML pro dotazy batch.</span><span class="sxs-lookup"><span data-stu-id="ece2e-320">For these reasons, hello use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="ece2e-321">Dávkování aspekty</span><span class="sxs-lookup"><span data-stu-id="ece2e-321">Batching considerations</span></span>
<span data-ttu-id="ece2e-322">Hello následující části obsahují další pokyny pro použití hello dávkování v aplikacích databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="ece2e-322">hello following sections provide more guidance for hello use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="ece2e-323">Kompromisy</span><span class="sxs-lookup"><span data-stu-id="ece2e-323">Tradeoffs</span></span>
<span data-ttu-id="ece2e-324">V závislosti na vaší architektury dávkování může zahrnovat kompromis mezi výkon a odolnost.</span><span class="sxs-lookup"><span data-stu-id="ece2e-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="ece2e-325">Zvažte například scénář hello, kde vaše role neočekávaně ocitne mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="ece2e-325">For example, consider hello scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="ece2e-326">Pokud ztratíte jeden řádek dat, dopad hello je menší než hello dopad ztráty velké dávku neodeslané řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-326">If you lose one row of data, hello impact is smaller than hello impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="ece2e-327">Existuje větší riziko, pokud vyrovnávací paměť řádky před jejich odesláním toohello databáze v zadaném časovém období.</span><span class="sxs-lookup"><span data-stu-id="ece2e-327">There is a greater risk when you buffer rows before sending them toohello database in a specified time window.</span></span>

<span data-ttu-id="ece2e-328">Z důvodu této kompromis vyhodnoťte hello typ operací, že batch můžete.</span><span class="sxs-lookup"><span data-stu-id="ece2e-328">Because of this tradeoff, evaluate hello type of operations that you batch.</span></span> <span data-ttu-id="ece2e-329">Batch důkladnějšímu (větší dávky a delší dobu windows) s daty, která je méně kritický.</span><span class="sxs-lookup"><span data-stu-id="ece2e-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="ece2e-330">Velikost dávky</span><span class="sxs-lookup"><span data-stu-id="ece2e-330">Batch size</span></span>
<span data-ttu-id="ece2e-331">V našich testech se obvykle žádné výhody toobreaking velké dávky na menší bloky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-331">In our tests, there was typically no advantage toobreaking large batches into smaller chunks.</span></span> <span data-ttu-id="ece2e-332">Ve skutečnosti tento pododdíl často za následek nižší výkon než jeden velký dávky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="ece2e-333">Zvažte například scénář, kde chcete tooinsert 1 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-333">For example, consider a scenario where you want tooinsert 1000 rows.</span></span> <span data-ttu-id="ece2e-334">Hello následující tabulka ukazuje, jak dlouho trvalo parametry s hodnotou tabulky toouse, tooinsert 1 000 řádků při rozdělit do menších dávek.</span><span class="sxs-lookup"><span data-stu-id="ece2e-334">hello following table shows how long it takes toouse table-valued parameters tooinsert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="ece2e-335">Velikost dávky</span><span class="sxs-lookup"><span data-stu-id="ece2e-335">Batch size</span></span> | <span data-ttu-id="ece2e-336">Iterace</span><span class="sxs-lookup"><span data-stu-id="ece2e-336">Iterations</span></span> | <span data-ttu-id="ece2e-337">Parametry s hodnotou tabulky (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ece2e-338">1000</span><span class="sxs-lookup"><span data-stu-id="ece2e-338">1000</span></span> |<span data-ttu-id="ece2e-339">1</span><span class="sxs-lookup"><span data-stu-id="ece2e-339">1</span></span> |<span data-ttu-id="ece2e-340">347</span><span class="sxs-lookup"><span data-stu-id="ece2e-340">347</span></span> |
| <span data-ttu-id="ece2e-341">500</span><span class="sxs-lookup"><span data-stu-id="ece2e-341">500</span></span> |<span data-ttu-id="ece2e-342">2</span><span class="sxs-lookup"><span data-stu-id="ece2e-342">2</span></span> |<span data-ttu-id="ece2e-343">355</span><span class="sxs-lookup"><span data-stu-id="ece2e-343">355</span></span> |
| <span data-ttu-id="ece2e-344">100</span><span class="sxs-lookup"><span data-stu-id="ece2e-344">100</span></span> |<span data-ttu-id="ece2e-345">10</span><span class="sxs-lookup"><span data-stu-id="ece2e-345">10</span></span> |<span data-ttu-id="ece2e-346">465</span><span class="sxs-lookup"><span data-stu-id="ece2e-346">465</span></span> |
| <span data-ttu-id="ece2e-347">50</span><span class="sxs-lookup"><span data-stu-id="ece2e-347">50</span></span> |<span data-ttu-id="ece2e-348">20</span><span class="sxs-lookup"><span data-stu-id="ece2e-348">20</span></span> |<span data-ttu-id="ece2e-349">630</span><span class="sxs-lookup"><span data-stu-id="ece2e-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-350">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-350">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-351">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-351">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-352">Uvidíte, že nejlepší výkon hello 1 000 řádků je toosubmit všechny najednou.</span><span class="sxs-lookup"><span data-stu-id="ece2e-352">You can see that hello best performance for 1000 rows is toosubmit them all at once.</span></span> <span data-ttu-id="ece2e-353">V jiné testy (není tady zobrazené) došlo malé výkonu nárůst toobreak dávce 10000 řádek do dvou dávek 5000.</span><span class="sxs-lookup"><span data-stu-id="ece2e-353">In other tests (not shown here) there was a small performance gain toobreak a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="ece2e-354">Ale hello schématu tabulky pro tyto testy se poměrně snadno, měli byste provést testy na konkrétních dat a tooverify velikosti dávky tato zjištění.</span><span class="sxs-lookup"><span data-stu-id="ece2e-354">But hello table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes tooverify these findings.</span></span>

<span data-ttu-id="ece2e-355">Jiné tooconsider faktor je, že pokud celkový počet batch hello příliš velká, SQL Database může omezení a odmítnout toocommit hello batch.</span><span class="sxs-lookup"><span data-stu-id="ece2e-355">Another factor tooconsider is that if hello total batch becomes too large, SQL Database might throttle and refuse toocommit hello batch.</span></span> <span data-ttu-id="ece2e-356">Nejlepších výsledků dosáhnete hello testovací toodetermine vaše konkrétní scénář, pokud dojde velikost dávky ideální.</span><span class="sxs-lookup"><span data-stu-id="ece2e-356">For hello best results, test your specific scenario toodetermine if there is an ideal batch size.</span></span> <span data-ttu-id="ece2e-357">Zkontrolujte velikost dávky hello konfigurovat v modulu runtime tooenable rychlé úpravy na základě výkonu nebo chyby.</span><span class="sxs-lookup"><span data-stu-id="ece2e-357">Make hello batch size configurable at runtime tooenable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="ece2e-358">Nakonec vyrovnávat hello velikost dávky hello s hello rizika spojená s dávkování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-358">Finally, balance hello size of hello batch with hello risks associated with batching.</span></span> <span data-ttu-id="ece2e-359">Pokud nejsou přechodné chyby nebo hello role selže, vezměte v úvahu důsledky hello opakování operace hello nebo ztráty dat hello v dávce hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-359">If there are transient errors or hello role fails, consider hello consequences of retrying hello operation or of losing hello data in hello batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="ece2e-360">Paralelní zpracování</span><span class="sxs-lookup"><span data-stu-id="ece2e-360">Parallel processing</span></span>
<span data-ttu-id="ece2e-361">Co když trvalo hello přístup snižuje velikost dávky hello ale používá více vláken tooexecute hello pracovní?</span><span class="sxs-lookup"><span data-stu-id="ece2e-361">What if you took hello approach of reducing hello batch size but used multiple threads tooexecute hello work?</span></span> <span data-ttu-id="ece2e-362">Naše testy znovu, ukázalo, že několik menších vícevláknové dávek zpravidla dělá horší, než jeden větší batch.</span><span class="sxs-lookup"><span data-stu-id="ece2e-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="ece2e-363">Hello následující testovací pokusí tooinsert 1 000 řádků v jedné nebo více paralelních dávek.</span><span class="sxs-lookup"><span data-stu-id="ece2e-363">hello following test attempts tooinsert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="ece2e-364">Tento test ukazuje, jak více souběžných dávky ve skutečnosti snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="ece2e-365">Velikost dávky [iterací]</span><span class="sxs-lookup"><span data-stu-id="ece2e-365">Batch size [Iterations]</span></span> | <span data-ttu-id="ece2e-366">Dva vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-366">Two threads (ms)</span></span> | <span data-ttu-id="ece2e-367">Čtyři vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-367">Four threads (ms)</span></span> | <span data-ttu-id="ece2e-368">Šest vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="ece2e-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ece2e-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="ece2e-369">1000 [1]</span></span> |<span data-ttu-id="ece2e-370">277</span><span class="sxs-lookup"><span data-stu-id="ece2e-370">277</span></span> |<span data-ttu-id="ece2e-371">315</span><span class="sxs-lookup"><span data-stu-id="ece2e-371">315</span></span> |<span data-ttu-id="ece2e-372">266</span><span class="sxs-lookup"><span data-stu-id="ece2e-372">266</span></span> |
| <span data-ttu-id="ece2e-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="ece2e-373">500 [2]</span></span> |<span data-ttu-id="ece2e-374">548</span><span class="sxs-lookup"><span data-stu-id="ece2e-374">548</span></span> |<span data-ttu-id="ece2e-375">278</span><span class="sxs-lookup"><span data-stu-id="ece2e-375">278</span></span> |<span data-ttu-id="ece2e-376">256</span><span class="sxs-lookup"><span data-stu-id="ece2e-376">256</span></span> |
| <span data-ttu-id="ece2e-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="ece2e-377">250 [4]</span></span> |<span data-ttu-id="ece2e-378">405</span><span class="sxs-lookup"><span data-stu-id="ece2e-378">405</span></span> |<span data-ttu-id="ece2e-379">329</span><span class="sxs-lookup"><span data-stu-id="ece2e-379">329</span></span> |<span data-ttu-id="ece2e-380">265</span><span class="sxs-lookup"><span data-stu-id="ece2e-380">265</span></span> |
| <span data-ttu-id="ece2e-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="ece2e-381">100 [10]</span></span> |<span data-ttu-id="ece2e-382">488</span><span class="sxs-lookup"><span data-stu-id="ece2e-382">488</span></span> |<span data-ttu-id="ece2e-383">439</span><span class="sxs-lookup"><span data-stu-id="ece2e-383">439</span></span> |<span data-ttu-id="ece2e-384">391</span><span class="sxs-lookup"><span data-stu-id="ece2e-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="ece2e-385">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-385">Results are not benchmarks.</span></span> <span data-ttu-id="ece2e-386">V tématu hello [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="ece2e-386">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="ece2e-387">Existuje několik možných důvodů pro hello snížení výkonu kvůli tooparallelism:</span><span class="sxs-lookup"><span data-stu-id="ece2e-387">There are several potential reasons for hello degradation in performance due tooparallelism:</span></span>

* <span data-ttu-id="ece2e-388">Existuje více souběžných sítě volání místo jeden.</span><span class="sxs-lookup"><span data-stu-id="ece2e-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="ece2e-389">Více operací pro jedinou tabulku může způsobit konflikty a blokování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="ece2e-390">Existují režijní náklady spojené s více vláken.</span><span class="sxs-lookup"><span data-stu-id="ece2e-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="ece2e-391">Hello výdajů otevření více připojení převáží hello výhodou paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-391">hello expense of opening multiple connections outweighs hello benefit of parallel processing.</span></span>

<span data-ttu-id="ece2e-392">Pokud cílíte různých tabulek nebo databází, je možné toosee poklesu výkonu získáte pomocí této strategie.</span><span class="sxs-lookup"><span data-stu-id="ece2e-392">If you target different tables or databases, it is possible toosee some performance gain with this strategy.</span></span> <span data-ttu-id="ece2e-393">Scénář pro tento postup by horizontálního dělení databáze nebo federace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="ece2e-394">Horizontálního dělení používá více databází a databázi tooeach různých datových trasy.</span><span class="sxs-lookup"><span data-stu-id="ece2e-394">Sharding uses multiple databases and routes different data tooeach database.</span></span> <span data-ttu-id="ece2e-395">Pokud každé malé dávky tooa jiné databázi, může být pak paralelní provádění operací hello efektivnější.</span><span class="sxs-lookup"><span data-stu-id="ece2e-395">If each small batch is going tooa different database, then performing hello operations in parallel can be more efficient.</span></span> <span data-ttu-id="ece2e-396">Ale hello výkonnější není dostatečně významné toouse jako hello základ pro horizontálního dělení rozhodnutí toouse databáze ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-396">However, hello performance gain is not significant enough toouse as hello basis for a decision toouse database sharding in your solution.</span></span>

<span data-ttu-id="ece2e-397">V některé návrhy můžete paralelní provádění menší dávek způsobit lepší propustnost požadavky v rámci systému zatížení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="ece2e-398">V takovém případě i když je rychlejší tooprocess jeden větší batch, více listů paralelní zpracování může být efektivnější.</span><span class="sxs-lookup"><span data-stu-id="ece2e-398">In this case, even though it is quicker tooprocess a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="ece2e-399">Pokud používáte paralelní provádění, vezměte v úvahu řízení hello maximální počet pracovních vláken.</span><span class="sxs-lookup"><span data-stu-id="ece2e-399">If you do use parallel execution, consider controlling hello maximum number of worker threads.</span></span> <span data-ttu-id="ece2e-400">Zmenšete počet může být výsledkem nižší výskyt kolizí a rychlejší dobu provádění.</span><span class="sxs-lookup"><span data-stu-id="ece2e-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="ece2e-401">Zvažte také hello další zátěže, které to umístí na hello cílová databáze v připojení a transakce.</span><span class="sxs-lookup"><span data-stu-id="ece2e-401">Also, consider hello additional load that this places on hello target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="ece2e-402">Faktory související výkonu</span><span class="sxs-lookup"><span data-stu-id="ece2e-402">Related performance factors</span></span>
<span data-ttu-id="ece2e-403">Dávkování ovlivní také typické pokyny na výkon databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="ece2e-404">Můžete například vložit pro tabulky, které mají velký primární klíč, nebo mnoho neclusterované indexy je snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="ece2e-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="ece2e-405">Pokud parametry s hodnotou tabulky pomocí uložené procedury, můžete použít příkaz hello **SET NOCOUNT ON** od začátku hello hello procedury.</span><span class="sxs-lookup"><span data-stu-id="ece2e-405">If table-valued parameters use a stored procedure, you can use hello command **SET NOCOUNT ON** at hello beginning of hello procedure.</span></span> <span data-ttu-id="ece2e-406">Tento příkaz potlačí hello návrat hello počet hello ovlivněných řádků v postupu hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-406">This statement suppresses hello return of hello count of hello affected rows in hello procedure.</span></span> <span data-ttu-id="ece2e-407">Ale v testech hello použití **SET NOCOUNT ON** nemělo žádný vliv nebo snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-407">However, in our tests, hello use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="ece2e-408">Hello testovací uložené procedury bylo jednoduché s jedním **vložit** příkazu z parametru s hodnotou tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-408">hello test stored procedure was simple with a single **INSERT** command from hello table-valued parameter.</span></span> <span data-ttu-id="ece2e-409">Je možné, že by složitější uložené procedury těžit z tohoto prohlášení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="ece2e-410">Ale Nepředpokládejte, že přidání **SET NOCOUNT ON** tooyour uložené procedury automaticky zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="ece2e-410">But don’t assume that adding **SET NOCOUNT ON** tooyour stored procedure automatically improves performance.</span></span> <span data-ttu-id="ece2e-411">toounderstand hello vliv, testovací vaše uložené procedury s i bez hello **SET NOCOUNT ON** příkaz.</span><span class="sxs-lookup"><span data-stu-id="ece2e-411">toounderstand hello effect, test your stored procedure with and without hello **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="ece2e-412">Dávkování scénáře</span><span class="sxs-lookup"><span data-stu-id="ece2e-412">Batching scenarios</span></span>
<span data-ttu-id="ece2e-413">Hello následující části popisují, jak toouse parametry s hodnotou tabulky třemi způsoby aplikace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-413">hello following sections describe how toouse table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="ece2e-414">Hello první scénář popisuje, jak ukládání do vyrovnávací paměti a dávkování vzájemně spolupracují.</span><span class="sxs-lookup"><span data-stu-id="ece2e-414">hello first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="ece2e-415">Druhý scénář Hello zlepšuje výkon provádění operací s podrobnostmi v jedné uložené procedury volání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-415">hello second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="ece2e-416">Hello konečné scénář ukazuje jak toouse parametry s hodnotou tabulky v operaci "UPSERT".</span><span class="sxs-lookup"><span data-stu-id="ece2e-416">hello final scenario shows how toouse table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="ece2e-417">Ukládání do vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="ece2e-417">Buffering</span></span>
<span data-ttu-id="ece2e-418">I když je několik scénářů, které jsou zřejmé kandidáta pro dávkování, existuje mnoho scénářů, které může využívat výhod dávkování zpožděné zpracování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="ece2e-419">Zpožděné zpracování také však představuje větší riziko, že je v případě hello neočekávané selhání ztrátám dat hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-419">However, delayed processing also carries a greater risk that hello data is lost in hello event of an unexpected failure.</span></span> <span data-ttu-id="ece2e-420">Je důležité toounderstand toto riziko a zvážit důsledky hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-420">It is important toounderstand this risk and consider hello consequences.</span></span>

<span data-ttu-id="ece2e-421">Představte si třeba webové aplikace, která sleduje hello navigační historii jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-421">For example, consider a web application that tracks hello navigation history of each user.</span></span> <span data-ttu-id="ece2e-422">S každým požadavkem stránky může aplikace hello nastavit uživatele databáze volání toorecord hello stránky zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-422">On each page request, hello application could make a database call toorecord hello user’s page view.</span></span> <span data-ttu-id="ece2e-423">Ale vyšší výkon a škálovatelnost lze dosáhnout ukládání do vyrovnávací paměti hello uživatelé navigační aktivity a pak odešle tato data toohello databáze v dávkách.</span><span class="sxs-lookup"><span data-stu-id="ece2e-423">But higher performance and scalability can be achieved by buffering hello users’ navigation activities and then sending this data toohello database in batches.</span></span> <span data-ttu-id="ece2e-424">Můžete aktivovat aktualizaci databáze hello uplynulý čas nebo velikost vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="ece2e-424">You can trigger hello database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="ece2e-425">Pravidlo může například určit, že tento hello batch, měla by být zpracována po 20 sekund nebo když vyrovnávací paměti hello dosáhne 1000 položek.</span><span class="sxs-lookup"><span data-stu-id="ece2e-425">For example, a rule could specify that hello batch should be processed after 20 seconds or when hello buffer reaches 1000 items.</span></span>

<span data-ttu-id="ece2e-426">Hello následující příklad kódu používá [reaktivní rozšíření - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess do vyrovnávací paměti události vyvolané službou třída monitorování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-426">hello following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess buffered events raised by a monitoring class.</span></span> <span data-ttu-id="ece2e-427">Když hello výplněmi vyrovnávací paměti nebo je dosaženo časového limitu, hello dávku uživatelská data se odesílají toohello databáze s parametr s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-427">When hello buffer fills or a timeout is reached, hello batch of user data is sent toohello database with a table-valued parameter.</span></span>

<span data-ttu-id="ece2e-428">Hello následující NavHistoryData třída modely hello navigační podrobné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="ece2e-428">hello following NavHistoryData class models hello user navigation details.</span></span> <span data-ttu-id="ece2e-429">Obsahuje základní informace, jako je například hello uživatelský identifikátor, adresa URL hello přístup a hello doba přístupu k.</span><span class="sxs-lookup"><span data-stu-id="ece2e-429">It contains basic information such as hello user identifier, hello URL accessed, and hello access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="ece2e-430">Hello NavHistoryDataMonitor třída je zodpovědná za ukládání do vyrovnávací paměti hello uživatele navigační data toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-430">hello NavHistoryDataMonitor class is responsible for buffering hello user navigation data toohello database.</span></span> <span data-ttu-id="ece2e-431">Obsahuje metody, RecordUserNavigationEntry, který odpovídá zobrazením **OnAdded** událostí.</span><span class="sxs-lookup"><span data-stu-id="ece2e-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="ece2e-432">Hello následující kód ukazuje hello logiku konstruktoru, který používá Rx toocreate kolekci pozorovatelné založené na události hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-432">hello following code shows hello constructor logic that uses Rx toocreate an observable collection based on hello event.</span></span> <span data-ttu-id="ece2e-433">Pak přihlásí toothis pozorovatelné kolekce s metodou hello vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="ece2e-433">It then subscribes toothis observable collection with hello Buffer method.</span></span> <span data-ttu-id="ece2e-434">přetížení Hello Určuje, že vyrovnávací paměť hello by měly být odeslány každých 20 sekund nebo 1 000 položek.</span><span class="sxs-lookup"><span data-stu-id="ece2e-434">hello overload specifies that hello buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="ece2e-435">Obslužná rutina Hello převede všechny položky hello uložená do vyrovnávací paměti typu s hodnotou tabulky a pak předá tento typ tooa uložený postup této batch hello procesy.</span><span class="sxs-lookup"><span data-stu-id="ece2e-435">hello handler converts all of hello buffered items into a table-valued type and then passes this type tooa stored procedure that processes hello batch.</span></span> <span data-ttu-id="ece2e-436">Hello následující kód ukazuje dokončení definice hello hello NavHistoryDataEventArgs i hello NavHistoryDataMonitor třídy.</span><span class="sxs-lookup"><span data-stu-id="ece2e-436">hello following code shows hello complete definition for both hello NavHistoryDataEventArgs and hello NavHistoryDataMonitor classes.</span></span>

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

<span data-ttu-id="ece2e-437">toouse této vyrovnávací paměti třídy hello aplikace vytvoří objekt NavHistoryDataMonitor statické.</span><span class="sxs-lookup"><span data-stu-id="ece2e-437">toouse this buffering class, hello application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="ece2e-438">Pokaždé, když uživatel přistupuje k na stránce aplikace hello volá metodu NavHistoryDataMonitor.RecordUserNavigationEntry hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-438">Each time a user accesses a page, hello application calls hello NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="ece2e-439">ukládání do vyrovnávací paměti logiku Hello pokračuje tootake péče o odesílání tyto položky toohello databáze v dávkách.</span><span class="sxs-lookup"><span data-stu-id="ece2e-439">hello buffering logic proceeds tootake care of sending these entries toohello database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="ece2e-440">Hlavní podrobností</span><span class="sxs-lookup"><span data-stu-id="ece2e-440">Master detail</span></span>
<span data-ttu-id="ece2e-441">Parametry s hodnotou tabulky jsou užitečné pro jednoduché scénáře INSERT.</span><span class="sxs-lookup"><span data-stu-id="ece2e-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="ece2e-442">Však může být náročnější toobatch vložení, které zahrnují více než jedna tabulka.</span><span class="sxs-lookup"><span data-stu-id="ece2e-442">However, it can be more challenging toobatch inserts that involve more than one table.</span></span> <span data-ttu-id="ece2e-443">scénář "hlavního a podrobného" Hello je dobrým příkladem.</span><span class="sxs-lookup"><span data-stu-id="ece2e-443">hello “master/detail” scenario is a good example.</span></span> <span data-ttu-id="ece2e-444">hlavní tabulka Hello identifikuje hello primární entity.</span><span class="sxs-lookup"><span data-stu-id="ece2e-444">hello master table identifies hello primary entity.</span></span> <span data-ttu-id="ece2e-445">Minimálně jedna tabulka podrobností uložit víc dat o hello entity.</span><span class="sxs-lookup"><span data-stu-id="ece2e-445">One or more detail tables store more data about hello entity.</span></span> <span data-ttu-id="ece2e-446">V tomto scénáři vynutit relace cizích klíčů relace hello podrobnosti tooa jedinečný hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="ece2e-446">In this scenario, foreign key relationships enforce hello relationship of details tooa unique master entity.</span></span> <span data-ttu-id="ece2e-447">Vezměte v úvahu zjednodušenou verzi PurchaseOrder tabulka a její přidružené OrderDetail tabulkou.</span><span class="sxs-lookup"><span data-stu-id="ece2e-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="ece2e-448">Hello následující Transact-SQL vytvoří tabulku PurchaseOrder hello s čtyři sloupce: OrderID, OrderDate, CustomerID a stav.</span><span class="sxs-lookup"><span data-stu-id="ece2e-448">hello following Transact-SQL creates hello PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="ece2e-449">Každý pořadí obsahuje jeden nebo více nákupy produktu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="ece2e-450">Tyto informace se zaznamená v tabulce PurchaseOrderDetail hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-450">This information is captured in hello PurchaseOrderDetail table.</span></span> <span data-ttu-id="ece2e-451">Hello následující Transact-SQL vytvoří hello PurchaseOrderDetail tabulku se sloupci pět: OrderID, OrderDetailID, ProductID, UnitPrice a OrderQty.</span><span class="sxs-lookup"><span data-stu-id="ece2e-451">hello following Transact-SQL creates hello PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="ece2e-452">Hello OrderID sloupec v tabulce PurchaseOrderDetail hello musíte odkázat pořadí z tabulky PurchaseOrder hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-452">hello OrderID column in hello PurchaseOrderDetail table must reference an order from hello PurchaseOrder table.</span></span> <span data-ttu-id="ece2e-453">Následující definice cizího klíče Hello vynucuje toto omezení.</span><span class="sxs-lookup"><span data-stu-id="ece2e-453">hello following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="ece2e-454">V pořadí toouse vracející tabulku parametrů musí mít jeden typ uživatelem definovaná tabulka pro každou cílovou tabulku.</span><span class="sxs-lookup"><span data-stu-id="ece2e-454">In order toouse table-valued parameters, you must have one user-defined table type for each target table.</span></span>

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

<span data-ttu-id="ece2e-455">Poté definujte uložené procedury, která přijímá tabulky z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="ece2e-456">Tento postup umožňuje batch toolocally aplikace sadu objednávek a podrobnosti o pořadí v jediném volání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-456">This procedure allows an application toolocally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="ece2e-457">Hello následující Transact-SQL poskytuje hello deklarace dokončení uložené procedury v tomto příkladu pořadí nákupu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-457">hello following Transact-SQL provides hello complete stored procedure declaration for this purchase order example.</span></span>

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects hello order identifiers in hello @orders
    -- table with hello actual order identifiers in hello PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders toohello PurchaseOrder table, storing hello actual
    -- order identifiers in hello @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match hello passed-in order identifiers with hello actual identifiers
    -- and complete hello @IdentityLink table for use with inserting hello details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert hello order details into hello PurchaseOrderDetail table, 
          -- using hello actual order identifiers of hello master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

<span data-ttu-id="ece2e-458">V tomto příkladu hello místně definované @IdentityLink ukládá hello skutečnými hodnotami OrderID z hello nově vložené řádky tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-458">In this example, hello locally defined @IdentityLink table stores hello actual OrderID values from hello newly inserted rows.</span></span> <span data-ttu-id="ece2e-459">Tyto identifikátory pořadí se liší od hello dočasné OrderID hodnoty v hello @orders a @details parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-459">These order identifiers are different from hello temporary OrderID values in hello @orders and @details table-valued parameters.</span></span> <span data-ttu-id="ece2e-460">Z tohoto důvodu hello @IdentityLink tabulky pak připojí hello OrderID hodnoty z hello @orders toohello skutečné OrderID hodnoty parametrů pro hello nové řádky v tabulce PurchaseOrder hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-460">For this reason, hello @IdentityLink table then connects hello OrderID values from hello @orders parameter toohello real OrderID values for hello new rows in hello PurchaseOrder table.</span></span> <span data-ttu-id="ece2e-461">Po provedení tohoto kroku hello @IdentityLink tabulky můžete usnadnit vkládání podrobnosti pořadí hello s hello skutečné OrderID, který splňuje hello omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="ece2e-461">After this step, hello @IdentityLink table can facilitate inserting hello order details with hello actual OrderID that satisfies hello foreign key constraint.</span></span>

<span data-ttu-id="ece2e-462">Tuto uloženou proceduru lze z kódu nebo jiná volání jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="ece2e-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="ece2e-463">Najdete v části parametry s hodnotou tabulky hello tento dokument příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-463">See hello table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="ece2e-464">Hello následující Transact-SQL ukazuje, jak toocall hello sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="ece2e-464">hello following Transact-SQL shows how toocall hello sp_InsertOrdersBatch.</span></span>

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

<span data-ttu-id="ece2e-465">Toto řešení umožňuje každé dávky toouse sadu OrderID hodnot, které začínají znakem 1.</span><span class="sxs-lookup"><span data-stu-id="ece2e-465">This solution allows each batch toouse a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="ece2e-466">Tyto hodnoty dočasné OrderID popisují hello vztahy v dávce hello, ale skutečné hodnoty OrderID hello jsou určeny v době hello operace insert hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-466">These temporary OrderID values describe hello relationships in hello batch, but hello actual OrderID values are determined at hello time of hello insert operation.</span></span> <span data-ttu-id="ece2e-467">Můžete spustit hello stejné příkazy v předchozím příkladu hello opakovaně a generovat jedinečný objednávky v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-467">You can run hello same statements in hello previous example repeatedly and generate unique orders in hello database.</span></span> <span data-ttu-id="ece2e-468">Z tohoto důvodu je vhodné přidat další kód nebo databáze logiku, která brání duplicitní objednávky při použití tohoto dávkování techniku.</span><span class="sxs-lookup"><span data-stu-id="ece2e-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="ece2e-469">Tento příklad ukazuje, že i složitější databázových operací, jako je například seznam podrobnosti operace, může zpracovat v dávce pomocí parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="ece2e-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="ece2e-470">UPSERT</span></span>
<span data-ttu-id="ece2e-471">Jiné dávkování scénář zahrnuje současně aktualizaci existujících řádků a vkládání nových řádků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="ece2e-472">Tato operace je někdy označují tooas operace "UPSERT" (aktualizace + insert).</span><span class="sxs-lookup"><span data-stu-id="ece2e-472">This operation is sometimes referred tooas an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="ece2e-473">Místo provedení samostatné volání tooINSERT a aktualizace, je příkazu MERGE hello nejlépe hodí toothis úloha.</span><span class="sxs-lookup"><span data-stu-id="ece2e-473">Rather than making separate calls tooINSERT and UPDATE, hello MERGE statement is best suited toothis task.</span></span> <span data-ttu-id="ece2e-474">Hello příkazu MERGE můžete provést i insert a operace v jednom volání aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ece2e-474">hello MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="ece2e-475">Parametry s hodnotou tabulky můžete použít s hello SLOUČENÍ příkaz tooperform aktualizace a vkládání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-475">Table-valued parameters can be used with hello MERGE statement tooperform updates and inserts.</span></span> <span data-ttu-id="ece2e-476">Představte si třeba zjednodušené zaměstnanec tabulku, která obsahuje následující sloupce hello: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="ece2e-476">For example, consider a simplified Employee table that contains hello following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="ece2e-477">V tomto příkladu můžete hello fakt, že tento hello SocialSecurityNumber je jedinečný tooperform SLOUČENÍM několika zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="ece2e-477">In this example, you can use hello fact that hello SocialSecurityNumber is unique tooperform a MERGE of multiple employees.</span></span> <span data-ttu-id="ece2e-478">Nejprve vytvořte hello uživatele definovaný typ tabulky:</span><span class="sxs-lookup"><span data-stu-id="ece2e-478">First, create hello user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="ece2e-479">Dále vytvořte uložené procedury nebo napsat kód, že používá hello SLOUČENÍ příkaz tooperform hello aktualizace a vkládání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-479">Next, create a stored procedure or write code that uses hello MERGE statement tooperform hello update and insert.</span></span> <span data-ttu-id="ece2e-480">Hello následující příklad používá příkazu MERGE hello na parametr s hodnotou tabulky @employees, typu EmployeeTableType.</span><span class="sxs-lookup"><span data-stu-id="ece2e-480">hello following example uses hello MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="ece2e-481">Hello obsah hello @employees tabulky nejsou zobrazeny zde.</span><span class="sxs-lookup"><span data-stu-id="ece2e-481">hello contents of hello @employees table are not shown here.</span></span>

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

<span data-ttu-id="ece2e-482">Další informace najdete v dokumentaci hello a příklady příkazu MERGE hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-482">For more information, see hello documentation and examples for hello MERGE statement.</span></span> <span data-ttu-id="ece2e-483">I když hello pracovní je možné provádět v několika krocích volání uložené procedury s samostatné operace INSERT a UPDATE, je efektivnější hello příkazu MERGE.</span><span class="sxs-lookup"><span data-stu-id="ece2e-483">Although hello same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, hello MERGE statement is more efficient.</span></span> <span data-ttu-id="ece2e-484">Kód databáze můžete také vytvořit volání jazyka Transact-SQL, která pomocí příkazu MERGE hello přímo bez nutnosti dvě volání databáze pro příkaz INSERT a UPDATE.</span><span class="sxs-lookup"><span data-stu-id="ece2e-484">Database code can also construct Transact-SQL calls that use hello MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="ece2e-485">Souhrnná doporučení</span><span class="sxs-lookup"><span data-stu-id="ece2e-485">Recommendation summary</span></span>
<span data-ttu-id="ece2e-486">Hello následující seznam obsahuje souhrn hello dávkování doporučení, které jsou popsané v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="ece2e-486">hello following list provides a summary of hello batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="ece2e-487">Pomocí ukládání do vyrovnávací paměti a dávkování tooincrease hello výkon a škálovatelnost aplikace SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ece2e-487">Use buffering and batching tooincrease hello performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="ece2e-488">Pochopení hello kompromisy mezi dávkování nebo ukládání do vyrovnávací paměti a odolnost.</span><span class="sxs-lookup"><span data-stu-id="ece2e-488">Understand hello tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="ece2e-489">Při selhání role vyváží hello riziko ztráty nezpracované dávku důležitých podnikových dat výhodou výkonu hello dávkování.</span><span class="sxs-lookup"><span data-stu-id="ece2e-489">During a role failure, hello risk of losing an unprocessed batch of business-critical data might outweigh hello performance benefit of batching.</span></span>
* <span data-ttu-id="ece2e-490">Byl proveden pokus tookeep všechny databáze toohello volání v rámci jednoho datového centra tooreduce latence.</span><span class="sxs-lookup"><span data-stu-id="ece2e-490">Attempt tookeep all calls toohello database within a single datacenter tooreduce latency.</span></span>
* <span data-ttu-id="ece2e-491">Pokud si zvolíte jednu dávkování techniku, nabízejí parametry s hodnotou tabulky hello optimálního výkonu a flexibility.</span><span class="sxs-lookup"><span data-stu-id="ece2e-491">If you choose a single batching technique, table-valued parameters offer hello best performance and flexibility.</span></span>
* <span data-ttu-id="ece2e-492">Pro hello nejrychlejší vložit výkonu, postupujte podle následujících obecných pokynů ale otestovat váš scénář:</span><span class="sxs-lookup"><span data-stu-id="ece2e-492">For hello fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="ece2e-493">< 100 řádků pomocí jedné parametrizovaného příkaz INSERT.</span><span class="sxs-lookup"><span data-stu-id="ece2e-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="ece2e-494">< 1 000 řádků použijte parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="ece2e-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="ece2e-495">Pro > = 1 000 řádků, použijte SqlBulkCopy.</span><span class="sxs-lookup"><span data-stu-id="ece2e-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="ece2e-496">Pro aktualizaci a operace odstranění, použijte parametry s hodnotou tabulky s logiky uložené procedury, která určuje hello správné operace na každý řádek v tabulce parametru hello.</span><span class="sxs-lookup"><span data-stu-id="ece2e-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines hello correct operation on each row in hello table parameter.</span></span>
* <span data-ttu-id="ece2e-497">Pokyny pro velikost dávky:</span><span class="sxs-lookup"><span data-stu-id="ece2e-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="ece2e-498">Použití hello největší batch velikosti, které dávají smysl pro vaše aplikace a podnikových požadavků.</span><span class="sxs-lookup"><span data-stu-id="ece2e-498">Use hello largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="ece2e-499">Vyrovnávat hello výkonu získáte velké dávek s hello rizika dočasné nebo závažné selhání.</span><span class="sxs-lookup"><span data-stu-id="ece2e-499">Balance hello performance gain of large batches with hello risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="ece2e-500">Co je důsledkem hello opakování nebo ke ztrátě dat. hello v dávce hello?</span><span class="sxs-lookup"><span data-stu-id="ece2e-500">What is hello consequence of retries or loss of hello data in hello batch?</span></span> 
  * <span data-ttu-id="ece2e-501">Otestujte hello největší batch velikost tooverify, databáze SQL není odmítnout ho.</span><span class="sxs-lookup"><span data-stu-id="ece2e-501">Test hello largest batch size tooverify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="ece2e-502">Vytvořte nastavení konfigurace tohoto ovládacího prvku dávkování, jako je například velikost dávky hello nebo hello vyrovnávací paměti časový interval.</span><span class="sxs-lookup"><span data-stu-id="ece2e-502">Create configuration settings that control batching, such as hello batch size or hello buffering time window.</span></span> <span data-ttu-id="ece2e-503">Tato nastavení poskytují flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="ece2e-503">These settings provide flexibility.</span></span> <span data-ttu-id="ece2e-504">Hello dávkování chování v produkčním prostředí bez opětovného nasazení hello cloudovou službu, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="ece2e-504">You can change hello batching behavior in production without redeploying hello cloud service.</span></span>
* <span data-ttu-id="ece2e-505">Vyhněte se paralelní zpracování dávek, které působí na jednotlivé tabulky v jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="ece2e-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="ece2e-506">Pokud si zvolíte toodivide jedné dávkové napříč několika pracovních vláken, spusťte testy toodetermine hello ideální počet vláken.</span><span class="sxs-lookup"><span data-stu-id="ece2e-506">If you do choose toodivide a single batch across multiple worker threads, run tests toodetermine hello ideal number of threads.</span></span> <span data-ttu-id="ece2e-507">Po neurčené prahová hodnota další podprocesy bude snížit výkon, a nikoli zvýšit ji.</span><span class="sxs-lookup"><span data-stu-id="ece2e-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="ece2e-508">Vezměte v úvahu ukládání do vyrovnávací paměti na velikost a čas jako způsob implementace dávkování pro více scénářů.</span><span class="sxs-lookup"><span data-stu-id="ece2e-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ece2e-509">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ece2e-509">Next steps</span></span>
<span data-ttu-id="ece2e-510">Tento článek zaměřuje na tom, jak návrhu databáze a kódování techniky související toobatching může zlepšit výkon aplikace a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="ece2e-510">This article focused on how database design and coding techniques related toobatching can improve your application performance and scalability.</span></span> <span data-ttu-id="ece2e-511">Ale toto je pouze jediný faktor v vaše celková strategie.</span><span class="sxs-lookup"><span data-stu-id="ece2e-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="ece2e-512">Další způsoby tooimprove výkon a škálovatelnost, najdete v části [Azure SQL Database – Průvodce výkonem pro izolované databáze](sql-database-performance-guidance.md) a [cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="ece2e-512">For more ways tooimprove performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

