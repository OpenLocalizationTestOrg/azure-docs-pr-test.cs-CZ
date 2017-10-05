---
title: "Jak používat dávkování pro zvýšení výkonu aplikací Azure SQL Database"
description: "V tématu poskytuje důkazy této dávkování databázových operací výrazně imroves rychlosti a škálovatelnost aplikací Azure SQL Database. I když tyto dávkování techniky fungovat pro libovolnou databázi systému SQL Server, je zaměřená článek v Azure."
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
ms.openlocfilehash: 22cff47444306e599325ba3035d83a0266d69c72
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a><span data-ttu-id="76ad4-104">Jak používat dávkování pro zvýšení výkonu aplikací databáze SQL</span><span class="sxs-lookup"><span data-stu-id="76ad4-104">How to use batching to improve SQL Database application performance</span></span>
<span data-ttu-id="76ad4-105">Dávkování operací do Azure SQL Database výrazně zvyšuje výkon a škálovatelnost aplikací.</span><span class="sxs-lookup"><span data-stu-id="76ad4-105">Batching operations to Azure SQL Database significantly improves the performance and scalability of your applications.</span></span> <span data-ttu-id="76ad4-106">Chcete-li pochopit výhody, první část tohoto článku popisuje některé ukázkové výsledky testů, porovnávající postupného a dávkové požadavky na databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="76ad4-106">In order to understand the benefits, the first part of this article covers some sample test results that compare sequential and batched requests to a SQL Database.</span></span> <span data-ttu-id="76ad4-107">Zbývající část článek ukazuje techniky, scénáře a požadavky umožňují použít dávkování úspěšně v aplikacích Azure.</span><span class="sxs-lookup"><span data-stu-id="76ad4-107">The remainder of the article shows the techniques, scenarios, and considerations to help you to use batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="76ad4-108">Proč je dávkování důležité pro databázi SQL?</span><span class="sxs-lookup"><span data-stu-id="76ad4-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="76ad4-109">Dávkování volání vzdálené služby je dobře známé strategie pro zvýšení výkonu a škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="76ad4-109">Batching calls to a remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="76ad4-110">Jsou pevně dané zpracování náklady na všechny interakce s vzdálené služby, jako je například serializace, přenos v síti a deserializace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-110">There are fixed processing costs to any interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="76ad4-111">Balení mnoho samostatné transakcí do jedné dávkové minimalizuje tyto náklady.</span><span class="sxs-lookup"><span data-stu-id="76ad4-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="76ad4-112">V tomto dokumentu jsme chcete prověřit různé SQL Database dávkování strategie a scénáře.</span><span class="sxs-lookup"><span data-stu-id="76ad4-112">In this paper, we want to examine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="76ad4-113">I když tyto strategie jsou také důležité pro místní aplikace, které používají systém SQL Server, tady je několik důvodů pro zvýraznění dávkování pro databázi SQL. použití:</span><span class="sxs-lookup"><span data-stu-id="76ad4-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting the use of batching for SQL Database:</span></span>

* <span data-ttu-id="76ad4-114">Je potenciálně vyšší latence sítě přístup k databázi SQL, zejména pokud v přístupu k databázi SQL z mimo stejného datového centra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="76ad4-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside the same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="76ad4-115">Víceklientské vlastnosti SQL Database znamená, že efektivitu na data přístup vrstvy odpovídá celkové škálovatelnost databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-115">The multitenant characteristics of SQL Database means that the efficiency of the data access layer correlates to the overall scalability of the database.</span></span> <span data-ttu-id="76ad4-116">Databáze SQL zabránit přivlastňuje databázových prostředků na úkor ostatních klientů žádné jednoho klienta nebo uživatele.</span><span class="sxs-lookup"><span data-stu-id="76ad4-116">SQL Database must prevent any single tenant/user from monopolizing database resources to the detriment of other tenants.</span></span> <span data-ttu-id="76ad4-117">Databáze SQL v reakci na využití překračující předdefinované kvóty, můžete snížit propustnost nebo odpovědět s omezení výjimky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-117">In response to usage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="76ad4-118">Efektivitu své činnosti, jako je například dávkování, umožňují provést u databáze SQL dříve, než dorazila těchto mezních hodnot.</span><span class="sxs-lookup"><span data-stu-id="76ad4-118">Efficiencies, such as batching, enable you to do more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="76ad4-119">Dávkování je také efektivní pro architektury, které používají více databází (horizontálního dělení).</span><span class="sxs-lookup"><span data-stu-id="76ad4-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="76ad4-120">Efektivitu interakce se jednotlivých jednotek databáze je stále klíčovým faktorem vaší celkovou škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="76ad4-120">The efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="76ad4-121">Jednou z výhod používání databáze SQL je, že nemáte ke správě serverů, které jsou hostiteli databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-121">One of the benefits of using SQL Database is that you don’t have to manage the servers that host the database.</span></span> <span data-ttu-id="76ad4-122">Však této spravované infrastruktury také znamená, že můžete mít jinak myslet optimalizace databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-122">However, this managed infrastructure also means that you have to think differently about database optimizations.</span></span> <span data-ttu-id="76ad4-123">Už můžete zobrazit ke zlepšení databáze hardware nebo síť infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="76ad4-123">You can no longer look to improve the database hardware or network infrastructure.</span></span> <span data-ttu-id="76ad4-124">Microsoft Azure řídí těchto prostředích.</span><span class="sxs-lookup"><span data-stu-id="76ad4-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="76ad4-125">Hlavní oblasti, která můžete řídit je, jak vaše aplikace komunikuje s databází SQL.</span><span class="sxs-lookup"><span data-stu-id="76ad4-125">The main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="76ad4-126">Dávkování je jedním z těchto optimalizace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="76ad4-127">První část papíru prověří různé dávkování techniky pro aplikace .NET, které používají SQL Database.</span><span class="sxs-lookup"><span data-stu-id="76ad4-127">The first part of the paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="76ad4-128">Poslední dvě části se věnují dávkování pokyny a scénáře.</span><span class="sxs-lookup"><span data-stu-id="76ad4-128">The last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="76ad4-129">Dávkování strategie</span><span class="sxs-lookup"><span data-stu-id="76ad4-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="76ad4-130">Všimněte si o výsledcích časování v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="76ad4-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="76ad4-131">Výsledky nejsou srovnávacích testů, ale jsou určené k zobrazení **relativní výkon**.</span><span class="sxs-lookup"><span data-stu-id="76ad4-131">Results are not benchmarks but are meant to show **relative performance**.</span></span> <span data-ttu-id="76ad4-132">Časování jsou založené na v průměru minimálně 10 test spustí.</span><span class="sxs-lookup"><span data-stu-id="76ad4-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="76ad4-133">Operace se vloží do prázdná tabulka.</span><span class="sxs-lookup"><span data-stu-id="76ad4-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="76ad4-134">Tyto testy byly měřená pre-V12 a jejich nemusí odpovídat nutně propustnosti, kterou může docházet v databázi V12 pomocí nové [úrovních služeb](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="76ad4-134">These tests were measured pre-V12, and they do not necessarily correspond to throughput that you might experience in a V12 database using the new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="76ad4-135">Relativní výhodou dávkování technika by mělo být podobné.</span><span class="sxs-lookup"><span data-stu-id="76ad4-135">The relative benefit of the batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="76ad4-136">Transakce</span><span class="sxs-lookup"><span data-stu-id="76ad4-136">Transactions</span></span>
<span data-ttu-id="76ad4-137">Podle všeho neobvyklé zahájíte o dávkování podle hovoříte o transakce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-137">It seems strange to begin a review of batching by discussing transactions.</span></span> <span data-ttu-id="76ad4-138">Ale použití transakce na straně klienta má vliv dávkování jemně straně serveru, který zlepšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="76ad4-138">But the use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="76ad4-139">A transakce lze přidat pouze zadání několika řádků kódu, takže poskytují rychlý způsob, jak zvýšit výkon při sekvenčních operací.</span><span class="sxs-lookup"><span data-stu-id="76ad4-139">And transactions can be added with only a few lines of code, so they provide a fast way to improve performance of sequential operations.</span></span>

<span data-ttu-id="76ad4-140">Vezměte v úvahu následující C# kód, který obsahuje posloupnost vložení a aktualizace operací na jednoduché tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-140">Consider the following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="76ad4-141">Následující kód ADO.NET postupně provádí tyto operace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-141">The following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="76ad4-142">Nejlepší způsob, jak optimalizovat tento kód je implementace určitou formu klienta dávkování těchto volání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-142">The best way to optimize this code is to implement some form of client-side batching of these calls.</span></span> <span data-ttu-id="76ad4-143">Ale je jednoduchý způsob, jak zvýšit výkon tento kód jednoduše zabalení pořadí volání v transakci.</span><span class="sxs-lookup"><span data-stu-id="76ad4-143">But there is a simple way to increase the performance of this code by simply wrapping the sequence of calls in a transaction.</span></span> <span data-ttu-id="76ad4-144">Zde je stejný kód, který používá transakce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-144">Here is the same code that uses a transaction.</span></span>

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

<span data-ttu-id="76ad4-145">Transakce se používá ve skutečnosti v obou těchto příkladech.</span><span class="sxs-lookup"><span data-stu-id="76ad4-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="76ad4-146">V prvním příkladu každý jednotlivých volání je implicitní transakci.</span><span class="sxs-lookup"><span data-stu-id="76ad4-146">In the first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="76ad4-147">V druhém příkladu explicitní transakce zabalí všechny volání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-147">In the second example, an explicit transaction wraps all of the calls.</span></span> <span data-ttu-id="76ad4-148">Za v dokumentaci [předběžné transakčního protokolu](https://msdn.microsoft.com/library/ms186259.aspx), záznamy protokolu jsou vyprazdňuje na disk, když transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="76ad4-148">Per the documentation for the [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed to the disk when the transaction commits.</span></span> <span data-ttu-id="76ad4-149">Další volání uvedete v transakci, můžete tak zápis transakčního protokolu počkat, až je transakce potvrzena.</span><span class="sxs-lookup"><span data-stu-id="76ad4-149">So by including more calls in a transaction, the write to the transaction log can delay until the transaction is committed.</span></span> <span data-ttu-id="76ad4-150">V důsledku toho jsou povolení dávkování při zápisu do protokolu transakcí serveru.</span><span class="sxs-lookup"><span data-stu-id="76ad4-150">In effect, you are enabling batching for the writes to the server’s transaction log.</span></span>

<span data-ttu-id="76ad4-151">V následující tabulce jsou uvedeny některé výsledky testování ad hoc.</span><span class="sxs-lookup"><span data-stu-id="76ad4-151">The following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="76ad4-152">Testy provést stejné sekvenční vložení a bez transakce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-152">The tests performed the same sequential inserts with and without transactions.</span></span> <span data-ttu-id="76ad4-153">Pro další perspektivy první sada testů spustili vzdáleně z přenosného počítače do databáze v Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="76ad4-153">For more perspective, the first set of tests ran remotely from a laptop to the database in Microsoft Azure.</span></span> <span data-ttu-id="76ad4-154">Druhá sada testů spustili z cloudové služby a databáze oba nacházejí ve stejném datovém centru Microsoft Azure (západní USA).</span><span class="sxs-lookup"><span data-stu-id="76ad4-154">The second set of tests ran from a cloud service and database that both resided within the same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="76ad4-155">Následující tabulka uvádí dobu v milisekundách sekvenční operace INSERT a bez transakce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-155">The following table shows the duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="76ad4-156">**Místním nasazením a Azure**:</span><span class="sxs-lookup"><span data-stu-id="76ad4-156">**On-Premises to Azure**:</span></span>

| <span data-ttu-id="76ad4-157">Operace</span><span class="sxs-lookup"><span data-stu-id="76ad4-157">Operations</span></span> | <span data-ttu-id="76ad4-158">Žádná transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-158">No Transaction (ms)</span></span> | <span data-ttu-id="76ad4-159">Transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-160">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-160">1</span></span> |<span data-ttu-id="76ad4-161">130</span><span class="sxs-lookup"><span data-stu-id="76ad4-161">130</span></span> |<span data-ttu-id="76ad4-162">402</span><span class="sxs-lookup"><span data-stu-id="76ad4-162">402</span></span> |
| <span data-ttu-id="76ad4-163">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-163">10</span></span> |<span data-ttu-id="76ad4-164">1208</span><span class="sxs-lookup"><span data-stu-id="76ad4-164">1208</span></span> |<span data-ttu-id="76ad4-165">1226</span><span class="sxs-lookup"><span data-stu-id="76ad4-165">1226</span></span> |
| <span data-ttu-id="76ad4-166">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-166">100</span></span> |<span data-ttu-id="76ad4-167">12662</span><span class="sxs-lookup"><span data-stu-id="76ad4-167">12662</span></span> |<span data-ttu-id="76ad4-168">10395</span><span class="sxs-lookup"><span data-stu-id="76ad4-168">10395</span></span> |
| <span data-ttu-id="76ad4-169">1000</span><span class="sxs-lookup"><span data-stu-id="76ad4-169">1000</span></span> |<span data-ttu-id="76ad4-170">128852</span><span class="sxs-lookup"><span data-stu-id="76ad4-170">128852</span></span> |<span data-ttu-id="76ad4-171">102917</span><span class="sxs-lookup"><span data-stu-id="76ad4-171">102917</span></span> |

<span data-ttu-id="76ad4-172">**Azure do Azure (stejného datového centra)**:</span><span class="sxs-lookup"><span data-stu-id="76ad4-172">**Azure to Azure (same datacenter)**:</span></span>

| <span data-ttu-id="76ad4-173">Operace</span><span class="sxs-lookup"><span data-stu-id="76ad4-173">Operations</span></span> | <span data-ttu-id="76ad4-174">Žádná transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-174">No Transaction (ms)</span></span> | <span data-ttu-id="76ad4-175">Transakce (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-176">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-176">1</span></span> |<span data-ttu-id="76ad4-177">21</span><span class="sxs-lookup"><span data-stu-id="76ad4-177">21</span></span> |<span data-ttu-id="76ad4-178">26</span><span class="sxs-lookup"><span data-stu-id="76ad4-178">26</span></span> |
| <span data-ttu-id="76ad4-179">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-179">10</span></span> |<span data-ttu-id="76ad4-180">220</span><span class="sxs-lookup"><span data-stu-id="76ad4-180">220</span></span> |<span data-ttu-id="76ad4-181">56</span><span class="sxs-lookup"><span data-stu-id="76ad4-181">56</span></span> |
| <span data-ttu-id="76ad4-182">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-182">100</span></span> |<span data-ttu-id="76ad4-183">2145</span><span class="sxs-lookup"><span data-stu-id="76ad4-183">2145</span></span> |<span data-ttu-id="76ad4-184">341</span><span class="sxs-lookup"><span data-stu-id="76ad4-184">341</span></span> |
| <span data-ttu-id="76ad4-185">1000</span><span class="sxs-lookup"><span data-stu-id="76ad4-185">1000</span></span> |<span data-ttu-id="76ad4-186">21479</span><span class="sxs-lookup"><span data-stu-id="76ad4-186">21479</span></span> |<span data-ttu-id="76ad4-187">2756</span><span class="sxs-lookup"><span data-stu-id="76ad4-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-188">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-188">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-189">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-189">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-190">Na základě předchozího výsledků testu zabalení jedné operace v transakci ve skutečnosti sníží výkon.</span><span class="sxs-lookup"><span data-stu-id="76ad4-190">Based on the previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="76ad4-191">Ale můžete zvýšit počet operací v rámci jedné transakce, zlepšování výkonu stane více označen.</span><span class="sxs-lookup"><span data-stu-id="76ad4-191">But as you increase the number of operations within a single transaction, the performance improvement becomes more marked.</span></span> <span data-ttu-id="76ad4-192">Rozdíl výkonu je také více patrné, dojde-li všechny operace v rámci datového centra Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="76ad4-192">The performance difference is also more noticeable when all operations occur within the Microsoft Azure datacenter.</span></span> <span data-ttu-id="76ad4-193">Zvýší latence používání databáze SQL z oblasti mimo datové centrum Microsoft Azure ruší výkonnější použití transakcí.</span><span class="sxs-lookup"><span data-stu-id="76ad4-193">The increased latency of using SQL Database from outside the Microsoft Azure datacenter overshadows the performance gain of using transactions.</span></span>

<span data-ttu-id="76ad4-194">Přestože použití transakcí může zvýšit výkon, i nadále [sledovat osvědčené postupy pro připojení a transakce](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="76ad4-194">Although the use of transactions can increase performance, continue to [observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="76ad4-195">Zachovat transakce co nejkratší a zavřete připojení k databázi po dokončení práce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-195">Keep the transaction as short as possible, and close the database connection after the work completes.</span></span> <span data-ttu-id="76ad4-196">Pomocí příkazu v předchozím příkladu zaručuje, že připojení je ukončeno po dokončení následných kód bloku.</span><span class="sxs-lookup"><span data-stu-id="76ad4-196">The using statement in the previous example assures that the connection is closed when the subsequent code block completes.</span></span>

<span data-ttu-id="76ad4-197">Předchozí příklad ukazuje, můžete přidat místní transakce pro jakýkoli kód ADO.NET se dvěma řádky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-197">The previous example demonstrates that you can add a local transaction to any ADO.NET code with two lines.</span></span> <span data-ttu-id="76ad4-198">Transakce nabízejí rychlý způsob, jak zlepšit výkon kód, který umožňuje sekvenční vložit, aktualizovat a odstraňovat operace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-198">Transactions offer a quick way to improve the performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="76ad4-199">Ale nejrychlejší výkonu, zvažte změnu kód dále využít výhody dávkování na straně klienta, jako jsou parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-199">However, for the fastest performance, consider changing the code further to take advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="76ad4-200">Další informace o transakcích v ADO.NET naleznete v tématu [místní transakce v ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="76ad4-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="76ad4-201">Parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="76ad4-201">Table-valued parameters</span></span>
<span data-ttu-id="76ad4-202">Parametry s hodnotou tabulky podporují uživatelem definovaná tabulka typy jako parametry příkazy jazyka Transact-SQL, uložené procedury a funkce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="76ad4-203">Tato technika dávkování na straně klienta, umožní vám odesílat více řádků dat v rámci parametr s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-203">This client-side batching technique allows you to send multiple rows of data within the table-valued parameter.</span></span> <span data-ttu-id="76ad4-204">Pokud chcete používat parametry s hodnotou tabulky, nejdříve definujte typ tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-204">To use table-valued parameters, first define a table type.</span></span> <span data-ttu-id="76ad4-205">Následující příkaz Transact-SQL vytvoří typ tabulky s názvem **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="76ad4-205">The following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="76ad4-206">V kódu, můžete vytvořit **DataTable** s přesnou stejné názvy a typy typu tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-206">In code, you create a **DataTable** with the exact same names and types of the table type.</span></span> <span data-ttu-id="76ad4-207">To předat **DataTable** parametr v textu dotazu nebo uložené proceduře volání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="76ad4-208">Následující příklad ukazuje, tento postup:</span><span class="sxs-lookup"><span data-stu-id="76ad4-208">The following example shows this technique:</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
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

<span data-ttu-id="76ad4-209">V předchozím příkladu **SqlCommand** objekt vloží řádky z parametr s hodnotou tabulky  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="76ad4-209">In the previous example, the **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="76ad4-210">Dříve vytvořenou **DataTable** objektu je přiřazen tento parametr se **SqlCommand.Parameters.Add** metoda.</span><span class="sxs-lookup"><span data-stu-id="76ad4-210">The previously created **DataTable** object is assigned to this parameter with the **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="76ad4-211">Dávkování vloží jedno volání výrazně zvyšuje výkon přes sekvenční vložení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-211">Batching the inserts in one call significantly increases the performance over sequential inserts.</span></span>

<span data-ttu-id="76ad4-212">Chcete-li zlepšit další předchozí příklad, použijte uložené procedury místo příkaz založený na textu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-212">To improve the previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="76ad4-213">Následující příkaz Transact-SQL vytvoří uložené procedury, která přebírá **SimpleTestTableType** parametr s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-213">The following Transact-SQL command creates a stored procedure that takes the **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="76ad4-214">Změňte **SqlCommand** deklarace v předchozím příkladu kódu pro následující objekt.</span><span class="sxs-lookup"><span data-stu-id="76ad4-214">Then change the **SqlCommand** object declaration in the previous code example to the following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="76ad4-215">Ve většině případů parametry s hodnotou tabulky mít ekvivalentní, nebo lepší výkon než jinými technikami, dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="76ad4-216">Parametry s hodnotou tabulky jsou často vhodnější, protože jsou flexibilnější než jiné možnosti.</span><span class="sxs-lookup"><span data-stu-id="76ad4-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="76ad4-217">Jinými technikami, jako je například hromadného kopírování SQL, například povolit jenom vkládání nových řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-217">For example, other techniques, such as SQL bulk copy, only permit the insertion of new rows.</span></span> <span data-ttu-id="76ad4-218">Ale s parametry s hodnotou tabulky, můžete použít logiku v uložené proceduře k určení, které řádky jsou aktualizace a který se vloží.</span><span class="sxs-lookup"><span data-stu-id="76ad4-218">But with table-valued parameters, you can use logic in the stored procedure to determine which rows are updates and which are inserts.</span></span> <span data-ttu-id="76ad4-219">Typ tabulky můžete také upravit tak, aby obsahovala sloupec "Operace", která určuje, zda zadaný řádek by měl být vložit, aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="76ad4-219">The table type can also be modified to contain an “Operation” column that indicates whether the specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="76ad4-220">V následující tabulce jsou uvedeny ad-hoc výsledků testů pro použití s hodnotou tabulky parametrů v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="76ad4-220">The following table shows ad-hoc test results for the use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="76ad4-221">Operace</span><span class="sxs-lookup"><span data-stu-id="76ad4-221">Operations</span></span> | <span data-ttu-id="76ad4-222">Místním nasazením a Azure (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-222">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="76ad4-223">Azure stejného datového centra (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-224">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-224">1</span></span> |<span data-ttu-id="76ad4-225">124</span><span class="sxs-lookup"><span data-stu-id="76ad4-225">124</span></span> |<span data-ttu-id="76ad4-226">32</span><span class="sxs-lookup"><span data-stu-id="76ad4-226">32</span></span> |
| <span data-ttu-id="76ad4-227">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-227">10</span></span> |<span data-ttu-id="76ad4-228">131</span><span class="sxs-lookup"><span data-stu-id="76ad4-228">131</span></span> |<span data-ttu-id="76ad4-229">25</span><span class="sxs-lookup"><span data-stu-id="76ad4-229">25</span></span> |
| <span data-ttu-id="76ad4-230">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-230">100</span></span> |<span data-ttu-id="76ad4-231">338</span><span class="sxs-lookup"><span data-stu-id="76ad4-231">338</span></span> |<span data-ttu-id="76ad4-232">51</span><span class="sxs-lookup"><span data-stu-id="76ad4-232">51</span></span> |
| <span data-ttu-id="76ad4-233">1000</span><span class="sxs-lookup"><span data-stu-id="76ad4-233">1000</span></span> |<span data-ttu-id="76ad4-234">2615</span><span class="sxs-lookup"><span data-stu-id="76ad4-234">2615</span></span> |<span data-ttu-id="76ad4-235">382</span><span class="sxs-lookup"><span data-stu-id="76ad4-235">382</span></span> |
| <span data-ttu-id="76ad4-236">10000</span><span class="sxs-lookup"><span data-stu-id="76ad4-236">10000</span></span> |<span data-ttu-id="76ad4-237">23830</span><span class="sxs-lookup"><span data-stu-id="76ad4-237">23830</span></span> |<span data-ttu-id="76ad4-238">3586</span><span class="sxs-lookup"><span data-stu-id="76ad4-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-239">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-239">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-240">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-240">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-241">Zvýšení výkonu z dávkování je okamžitě zřejmá.</span><span class="sxs-lookup"><span data-stu-id="76ad4-241">The performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="76ad4-242">V předchozím testu sekvenčních 1000 operace trvalo 129 sekund mimo datové centrum a 21 sekund z v rámci datového centra.</span><span class="sxs-lookup"><span data-stu-id="76ad4-242">In the previous sequential test, 1000 operations took 129 seconds outside the datacenter and 21 seconds from within the datacenter.</span></span> <span data-ttu-id="76ad4-243">Ale s parametry s hodnotou tabulky 1000 operace trvat jenom 2.6 sekund mimo datové centrum a 0,4 sekundy v rámci datového centra.</span><span class="sxs-lookup"><span data-stu-id="76ad4-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside the datacenter and 0.4 seconds within the datacenter.</span></span>

<span data-ttu-id="76ad4-244">Další informace o parametry s hodnotou tabulky najdete v tématu [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="76ad4-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="76ad4-245">Hromadné kopírování SQL</span><span class="sxs-lookup"><span data-stu-id="76ad4-245">SQL bulk copy</span></span>
<span data-ttu-id="76ad4-246">Hromadné kopírování SQL je další způsob vložení velkých objemů dat do cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-246">SQL bulk copy is another way to insert large amounts of data into a target database.</span></span> <span data-ttu-id="76ad4-247">Aplikace .NET mohou použít **SqlBulkCopy** třída provést hromadné operace vložení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-247">.NET applications can use the **SqlBulkCopy** class to perform bulk insert operations.</span></span> <span data-ttu-id="76ad4-248">**SqlBulkCopy** je podobně jako u nástroj příkazového řádku **Bcp.exe**, nebo pomocí příkazu Transact-SQL **BULK INSERT**.</span><span class="sxs-lookup"><span data-stu-id="76ad4-248">**SqlBulkCopy** is similar in function to the command-line tool, **Bcp.exe**, or the Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="76ad4-249">Následující příklad kódu ukazuje, jak hromadně kopírovat řádky ve zdroji **DataTable**, tabulky do cílové tabulky v systému SQL Server MyTable.</span><span class="sxs-lookup"><span data-stu-id="76ad4-249">The following code example shows how to bulk copy the rows in the source **DataTable**, table, to the destination table in SQL Server, MyTable.</span></span>

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

<span data-ttu-id="76ad4-250">Existují případy, kdy je hromadné kopírování upřednostňované nad parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="76ad4-251">Podívejte se na tabulku porovnání s hodnotou tabulky parametrů versus BULK INSERT operace v tématu [zavolat parametry](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="76ad4-251">See the comparison table of Table-Valued parameters versus BULK INSERT operations in the topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="76ad4-252">Zobrazit následující výsledky testů ad-hoc výkon dávkování s **SqlBulkCopy** v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="76ad4-252">The following ad-hoc test results show the performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="76ad4-253">Operace</span><span class="sxs-lookup"><span data-stu-id="76ad4-253">Operations</span></span> | <span data-ttu-id="76ad4-254">Místním nasazením a Azure (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-254">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="76ad4-255">Azure stejného datového centra (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-256">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-256">1</span></span> |<span data-ttu-id="76ad4-257">433</span><span class="sxs-lookup"><span data-stu-id="76ad4-257">433</span></span> |<span data-ttu-id="76ad4-258">57</span><span class="sxs-lookup"><span data-stu-id="76ad4-258">57</span></span> |
| <span data-ttu-id="76ad4-259">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-259">10</span></span> |<span data-ttu-id="76ad4-260">441</span><span class="sxs-lookup"><span data-stu-id="76ad4-260">441</span></span> |<span data-ttu-id="76ad4-261">32</span><span class="sxs-lookup"><span data-stu-id="76ad4-261">32</span></span> |
| <span data-ttu-id="76ad4-262">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-262">100</span></span> |<span data-ttu-id="76ad4-263">636</span><span class="sxs-lookup"><span data-stu-id="76ad4-263">636</span></span> |<span data-ttu-id="76ad4-264">53</span><span class="sxs-lookup"><span data-stu-id="76ad4-264">53</span></span> |
| <span data-ttu-id="76ad4-265">1000</span><span class="sxs-lookup"><span data-stu-id="76ad4-265">1000</span></span> |<span data-ttu-id="76ad4-266">2535</span><span class="sxs-lookup"><span data-stu-id="76ad4-266">2535</span></span> |<span data-ttu-id="76ad4-267">341</span><span class="sxs-lookup"><span data-stu-id="76ad4-267">341</span></span> |
| <span data-ttu-id="76ad4-268">10000</span><span class="sxs-lookup"><span data-stu-id="76ad4-268">10000</span></span> |<span data-ttu-id="76ad4-269">21605</span><span class="sxs-lookup"><span data-stu-id="76ad4-269">21605</span></span> |<span data-ttu-id="76ad4-270">2737</span><span class="sxs-lookup"><span data-stu-id="76ad4-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-271">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-271">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-272">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-272">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-273">V menší velikosti dávky, použijte parametry s hodnotou tabulky outperformed **SqlBulkCopy** třídy.</span><span class="sxs-lookup"><span data-stu-id="76ad4-273">In smaller batch sizes, the use table-valued parameters outperformed the **SqlBulkCopy** class.</span></span> <span data-ttu-id="76ad4-274">Ale **SqlBulkCopy** provést 12-31 % rychlejší než parametry s hodnotou tabulky pro testy 1 000 a 10 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for the tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="76ad4-275">Jako parametry s hodnotou tabulky **SqlBulkCopy** je vhodný pro dávkové vložení, zejména v případě, že ve srovnání s výkon operací jiný-zpracovat v dávce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared to the performance of non-batched operations.</span></span>

<span data-ttu-id="76ad4-276">Další informace o hromadné kopírování v ADO.NET naleznete v tématu [operace hromadného kopírování v systému SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="76ad4-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="76ad4-277">Příkazy s parametry vložit více řádků</span><span class="sxs-lookup"><span data-stu-id="76ad4-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="76ad4-278">Jeden alternativou k malé dávky je vytvořit velké parametrizované příkaz INSERT, který se vloží více řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-278">One alternative for small batches is to construct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="76ad4-279">Následující příklad kódu ukazuje tento postup.</span><span class="sxs-lookup"><span data-stu-id="76ad4-279">The following code example demonstrates this technique.</span></span>

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


<span data-ttu-id="76ad4-280">Tento příklad slouží k zobrazení základní koncept.</span><span class="sxs-lookup"><span data-stu-id="76ad4-280">This example is meant to show the basic concept.</span></span> <span data-ttu-id="76ad4-281">Realističtější scénář by projít požadované entity současně vytvořit řetězec dotazu a parametry příkazu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-281">A more realistic scenario would loop through the required entities to construct the query string and the command parameters simultaneously.</span></span> <span data-ttu-id="76ad4-282">Jste omezený na celkem parametry dotazu 2100, toto nastavení omezuje celkový počet řádků, které lze zpracovat tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="76ad4-282">You are limited to a total of 2100 query parameters, so this limits the total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="76ad4-283">Následující výsledky testů ad-hoc zobrazit výkon tento typ příkazu insert v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="76ad4-283">The following ad-hoc test results show the performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="76ad4-284">Operace</span><span class="sxs-lookup"><span data-stu-id="76ad4-284">Operations</span></span> | <span data-ttu-id="76ad4-285">Parametry s hodnotou tabulky (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="76ad4-286">Vložení jedním příkazem (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-287">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-287">1</span></span> |<span data-ttu-id="76ad4-288">32</span><span class="sxs-lookup"><span data-stu-id="76ad4-288">32</span></span> |<span data-ttu-id="76ad4-289">20</span><span class="sxs-lookup"><span data-stu-id="76ad4-289">20</span></span> |
| <span data-ttu-id="76ad4-290">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-290">10</span></span> |<span data-ttu-id="76ad4-291">30</span><span class="sxs-lookup"><span data-stu-id="76ad4-291">30</span></span> |<span data-ttu-id="76ad4-292">25</span><span class="sxs-lookup"><span data-stu-id="76ad4-292">25</span></span> |
| <span data-ttu-id="76ad4-293">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-293">100</span></span> |<span data-ttu-id="76ad4-294">33</span><span class="sxs-lookup"><span data-stu-id="76ad4-294">33</span></span> |<span data-ttu-id="76ad4-295">51</span><span class="sxs-lookup"><span data-stu-id="76ad4-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-296">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-296">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-297">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-297">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-298">Tento přístup může být mírně rychlejší pro balíků, které jsou menší než 100 řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="76ad4-299">I když zlepšení je malý, tento postup je další možností, které může fungovat i ve vašem scénáři konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76ad4-299">Although the improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="76ad4-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="76ad4-300">DataAdapter</span></span>
<span data-ttu-id="76ad4-301">**DataAdapter** třída umožňuje upravovat **datovou sadu** objektu a pak odeslat provedené změny jako operace INSERT, UPDATE a DELETE.</span><span class="sxs-lookup"><span data-stu-id="76ad4-301">The **DataAdapter** class allows you to modify a **DataSet** object and then submit the changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="76ad4-302">Pokud používáte **DataAdapter** tímto způsobem je důležité si uvědomit, že samostatné volání jsou vytvářeny pro každou operaci distinct.</span><span class="sxs-lookup"><span data-stu-id="76ad4-302">If you are using the **DataAdapter** in this manner, it is important to note that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="76ad4-303">Chcete-li zvýšit výkon, použijte **UpdateBatchSize** vlastnost počet operací, které by měl zpracovat v dávce najednou.</span><span class="sxs-lookup"><span data-stu-id="76ad4-303">To improve performance, use the **UpdateBatchSize** property to the number of operations that should be batched at a time.</span></span> <span data-ttu-id="76ad4-304">Další informace najdete v tématu [provádění dávkové operace pomocí DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="76ad4-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="76ad4-305">Rozhraní Entity framework</span><span class="sxs-lookup"><span data-stu-id="76ad4-305">Entity framework</span></span>
<span data-ttu-id="76ad4-306">Rozhraní Entity Framework aktuálně nepodporuje dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="76ad4-307">Různé vývojáři v komunitě Pokusili jste se ukazují alternativní řešení, například přepsání **SaveChanges** metoda.</span><span class="sxs-lookup"><span data-stu-id="76ad4-307">Different developers in the community have attempted to demonstrate workarounds, such as override the **SaveChanges** method.</span></span> <span data-ttu-id="76ad4-308">Ale řešení jsou obvykle komplexní a přizpůsobit v aplikaci a datového modelu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-308">But the solutions are typically complex and customized to the application and data model.</span></span> <span data-ttu-id="76ad4-309">Rozhraní Entity Framework projektu webu codeplex má v současnosti diskusní stránky na žádost o této funkce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-309">The Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="76ad4-310">Tato diskuse najdete v tématu [poznámky ze schůzky návrhu - 2 srpen 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="76ad4-310">To view this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="76ad4-311">XML</span><span class="sxs-lookup"><span data-stu-id="76ad4-311">XML</span></span>
<span data-ttu-id="76ad4-312">Pro úplnost se domníváme, že je potřeba mluvit o XML jako strategie dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-312">For completeness, we feel that it is important to talk about XML as a batching strategy.</span></span> <span data-ttu-id="76ad4-313">Použití XML má však žádné výhody přes jiné metody a několik nevýhody.</span><span class="sxs-lookup"><span data-stu-id="76ad4-313">However, the use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="76ad4-314">Přístup je podobná parametry s hodnotou tabulky, ale souboru XML nebo řetězec předaný uložené proceduře místo uživatelem definovaná tabulka.</span><span class="sxs-lookup"><span data-stu-id="76ad4-314">The approach is similar to table-valued parameters, but an XML file or string is passed to a stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="76ad4-315">Uložená procedura analyzuje příkazy v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="76ad4-315">The stored procedure parses the commands in the stored procedure.</span></span>

<span data-ttu-id="76ad4-316">Existuje několik nevýhod tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="76ad4-316">There are several disadvantages to this approach:</span></span>

* <span data-ttu-id="76ad4-317">Práce s XML může být náročná a chyba náchylné k chybám.</span><span class="sxs-lookup"><span data-stu-id="76ad4-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="76ad4-318">Analýza kódu XML v databázi, může být náročná na prostředky procesoru.</span><span class="sxs-lookup"><span data-stu-id="76ad4-318">Parsing the XML on the database can be CPU-intensive.</span></span>
* <span data-ttu-id="76ad4-319">Ve většině případů je tato metoda pomalejší než parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="76ad4-320">Z těchto důvodů se nedoporučuje použití XML pro dotazy batch.</span><span class="sxs-lookup"><span data-stu-id="76ad4-320">For these reasons, the use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="76ad4-321">Dávkování aspekty</span><span class="sxs-lookup"><span data-stu-id="76ad4-321">Batching considerations</span></span>
<span data-ttu-id="76ad4-322">Následující části obsahují další pokyny pro použití dávkování v aplikacích databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="76ad4-322">The following sections provide more guidance for the use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="76ad4-323">Kompromisy</span><span class="sxs-lookup"><span data-stu-id="76ad4-323">Tradeoffs</span></span>
<span data-ttu-id="76ad4-324">V závislosti na vaší architektury dávkování může zahrnovat kompromis mezi výkon a odolnost.</span><span class="sxs-lookup"><span data-stu-id="76ad4-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="76ad4-325">Zvažte například scénář, kde vaše role neočekávaně ocitne mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="76ad4-325">For example, consider the scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="76ad4-326">Pokud ztratíte jeden řádek dat, dopad je menší než dopad ztráty velké dávku neodeslané řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-326">If you lose one row of data, the impact is smaller than the impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="76ad4-327">Existuje větší riziko, pokud vyrovnávací paměť řádky před jejich odesláním do databáze v zadaném časovém období.</span><span class="sxs-lookup"><span data-stu-id="76ad4-327">There is a greater risk when you buffer rows before sending them to the database in a specified time window.</span></span>

<span data-ttu-id="76ad4-328">Z důvodu této kompromis Vyhodnoťte typ operací, že batch můžete.</span><span class="sxs-lookup"><span data-stu-id="76ad4-328">Because of this tradeoff, evaluate the type of operations that you batch.</span></span> <span data-ttu-id="76ad4-329">Batch důkladnějšímu (větší dávky a delší dobu windows) s daty, která je méně kritický.</span><span class="sxs-lookup"><span data-stu-id="76ad4-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="76ad4-330">Velikost dávky</span><span class="sxs-lookup"><span data-stu-id="76ad4-330">Batch size</span></span>
<span data-ttu-id="76ad4-331">V našich testech se obvykle žádnou výhodu nejnovější velké dávky na menší bloky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-331">In our tests, there was typically no advantage to breaking large batches into smaller chunks.</span></span> <span data-ttu-id="76ad4-332">Ve skutečnosti tento pododdíl často za následek nižší výkon než jeden velký dávky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="76ad4-333">Zvažte například scénář, kam chcete vložit 1 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-333">For example, consider a scenario where you want to insert 1000 rows.</span></span> <span data-ttu-id="76ad4-334">Následující tabulka ukazuje, jak dlouho Pokud chcete používat parametry s hodnotou tabulky vložit 1 000 řádků při rozdělit do menších dávek.</span><span class="sxs-lookup"><span data-stu-id="76ad4-334">The following table shows how long it takes to use table-valued parameters to insert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="76ad4-335">Velikost dávky</span><span class="sxs-lookup"><span data-stu-id="76ad4-335">Batch size</span></span> | <span data-ttu-id="76ad4-336">Iterace</span><span class="sxs-lookup"><span data-stu-id="76ad4-336">Iterations</span></span> | <span data-ttu-id="76ad4-337">Parametry s hodnotou tabulky (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="76ad4-338">1000</span><span class="sxs-lookup"><span data-stu-id="76ad4-338">1000</span></span> |<span data-ttu-id="76ad4-339">1</span><span class="sxs-lookup"><span data-stu-id="76ad4-339">1</span></span> |<span data-ttu-id="76ad4-340">347</span><span class="sxs-lookup"><span data-stu-id="76ad4-340">347</span></span> |
| <span data-ttu-id="76ad4-341">500</span><span class="sxs-lookup"><span data-stu-id="76ad4-341">500</span></span> |<span data-ttu-id="76ad4-342">2</span><span class="sxs-lookup"><span data-stu-id="76ad4-342">2</span></span> |<span data-ttu-id="76ad4-343">355</span><span class="sxs-lookup"><span data-stu-id="76ad4-343">355</span></span> |
| <span data-ttu-id="76ad4-344">100</span><span class="sxs-lookup"><span data-stu-id="76ad4-344">100</span></span> |<span data-ttu-id="76ad4-345">10</span><span class="sxs-lookup"><span data-stu-id="76ad4-345">10</span></span> |<span data-ttu-id="76ad4-346">465</span><span class="sxs-lookup"><span data-stu-id="76ad4-346">465</span></span> |
| <span data-ttu-id="76ad4-347">50</span><span class="sxs-lookup"><span data-stu-id="76ad4-347">50</span></span> |<span data-ttu-id="76ad4-348">20</span><span class="sxs-lookup"><span data-stu-id="76ad4-348">20</span></span> |<span data-ttu-id="76ad4-349">630</span><span class="sxs-lookup"><span data-stu-id="76ad4-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-350">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-350">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-351">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-351">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-352">Uvidíte, že je všechny najednou odeslat je nejlepší výkon pro 1 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-352">You can see that the best performance for 1000 rows is to submit them all at once.</span></span> <span data-ttu-id="76ad4-353">Jiné testy (není tady zobrazené) bylo malé výkonnější dávce 10000 řádek rozdělit na dvě dávky 5000.</span><span class="sxs-lookup"><span data-stu-id="76ad4-353">In other tests (not shown here) there was a small performance gain to break a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="76ad4-354">Ale schématu tabulky pro tyto testy je poměrně jednoduché, měli byste provést testy na konkrétních dat a velikosti dávky k ověření těchto zjištění.</span><span class="sxs-lookup"><span data-stu-id="76ad4-354">But the table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes to verify these findings.</span></span>

<span data-ttu-id="76ad4-355">Dalším faktorem vzít v úvahu je, že pokud celkový počet batch příliš velká, SQL Database může omezení a odmítnout potvrzení dávky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-355">Another factor to consider is that if the total batch becomes too large, SQL Database might throttle and refuse to commit the batch.</span></span> <span data-ttu-id="76ad4-356">Nejlepších výsledků dosáhnete otestujte konkrétní scénář k určení, jestli je velikost dávky ideální.</span><span class="sxs-lookup"><span data-stu-id="76ad4-356">For the best results, test your specific scenario to determine if there is an ideal batch size.</span></span> <span data-ttu-id="76ad4-357">Velikost dávky konfigurovatelné za běhu, aby povolit rychlé úpravy na základě výkonu nebo chyby.</span><span class="sxs-lookup"><span data-stu-id="76ad4-357">Make the batch size configurable at runtime to enable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="76ad4-358">Nakonec vyrovnávat velikost dávky s riziky spojenými s dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-358">Finally, balance the size of the batch with the risks associated with batching.</span></span> <span data-ttu-id="76ad4-359">Pokud nejsou přechodné chyby nebo roli selže, vezměte v úvahu důsledky opakováním operace nebo ke ztrátě dat v dávce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-359">If there are transient errors or the role fails, consider the consequences of retrying the operation or of losing the data in the batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="76ad4-360">Paralelní zpracování</span><span class="sxs-lookup"><span data-stu-id="76ad4-360">Parallel processing</span></span>
<span data-ttu-id="76ad4-361">Co když trvalo přístup snižuje velikost dávky, ale používá více vláken k provedení práce?</span><span class="sxs-lookup"><span data-stu-id="76ad4-361">What if you took the approach of reducing the batch size but used multiple threads to execute the work?</span></span> <span data-ttu-id="76ad4-362">Naše testy znovu, ukázalo, že několik menších vícevláknové dávek zpravidla dělá horší, než jeden větší batch.</span><span class="sxs-lookup"><span data-stu-id="76ad4-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="76ad4-363">Následující test se pokusí vložit řádky 1000 do jedné nebo více paralelních dávek.</span><span class="sxs-lookup"><span data-stu-id="76ad4-363">The following test attempts to insert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="76ad4-364">Tento test ukazuje, jak více souběžných dávky ve skutečnosti snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="76ad4-365">Velikost dávky [iterací]</span><span class="sxs-lookup"><span data-stu-id="76ad4-365">Batch size [Iterations]</span></span> | <span data-ttu-id="76ad4-366">Dva vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-366">Two threads (ms)</span></span> | <span data-ttu-id="76ad4-367">Čtyři vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-367">Four threads (ms)</span></span> | <span data-ttu-id="76ad4-368">Šest vláken (ms)</span><span class="sxs-lookup"><span data-stu-id="76ad4-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="76ad4-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="76ad4-369">1000 [1]</span></span> |<span data-ttu-id="76ad4-370">277</span><span class="sxs-lookup"><span data-stu-id="76ad4-370">277</span></span> |<span data-ttu-id="76ad4-371">315</span><span class="sxs-lookup"><span data-stu-id="76ad4-371">315</span></span> |<span data-ttu-id="76ad4-372">266</span><span class="sxs-lookup"><span data-stu-id="76ad4-372">266</span></span> |
| <span data-ttu-id="76ad4-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="76ad4-373">500 [2]</span></span> |<span data-ttu-id="76ad4-374">548</span><span class="sxs-lookup"><span data-stu-id="76ad4-374">548</span></span> |<span data-ttu-id="76ad4-375">278</span><span class="sxs-lookup"><span data-stu-id="76ad4-375">278</span></span> |<span data-ttu-id="76ad4-376">256</span><span class="sxs-lookup"><span data-stu-id="76ad4-376">256</span></span> |
| <span data-ttu-id="76ad4-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="76ad4-377">250 [4]</span></span> |<span data-ttu-id="76ad4-378">405</span><span class="sxs-lookup"><span data-stu-id="76ad4-378">405</span></span> |<span data-ttu-id="76ad4-379">329</span><span class="sxs-lookup"><span data-stu-id="76ad4-379">329</span></span> |<span data-ttu-id="76ad4-380">265</span><span class="sxs-lookup"><span data-stu-id="76ad4-380">265</span></span> |
| <span data-ttu-id="76ad4-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="76ad4-381">100 [10]</span></span> |<span data-ttu-id="76ad4-382">488</span><span class="sxs-lookup"><span data-stu-id="76ad4-382">488</span></span> |<span data-ttu-id="76ad4-383">439</span><span class="sxs-lookup"><span data-stu-id="76ad4-383">439</span></span> |<span data-ttu-id="76ad4-384">391</span><span class="sxs-lookup"><span data-stu-id="76ad4-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="76ad4-385">Výsledky nejsou srovnávacích testů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-385">Results are not benchmarks.</span></span> <span data-ttu-id="76ad4-386">Najdete v článku [Poznámka o výsledcích časování v tomto tématu](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="76ad4-386">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="76ad4-387">Existuje několik možných důvodů pro snížení výkonu z důvodu paralelismus:</span><span class="sxs-lookup"><span data-stu-id="76ad4-387">There are several potential reasons for the degradation in performance due to parallelism:</span></span>

* <span data-ttu-id="76ad4-388">Existuje více souběžných sítě volání místo jeden.</span><span class="sxs-lookup"><span data-stu-id="76ad4-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="76ad4-389">Více operací pro jedinou tabulku může způsobit konflikty a blokování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="76ad4-390">Existují režijní náklady spojené s více vláken.</span><span class="sxs-lookup"><span data-stu-id="76ad4-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="76ad4-391">Náklady otevření více připojení převáží výhodou paralelní zpracování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-391">The expense of opening multiple connections outweighs the benefit of parallel processing.</span></span>

<span data-ttu-id="76ad4-392">Pokud cílíte různých tabulek nebo databází, je možné zobrazit některé výkonu získáte pomocí této strategie.</span><span class="sxs-lookup"><span data-stu-id="76ad4-392">If you target different tables or databases, it is possible to see some performance gain with this strategy.</span></span> <span data-ttu-id="76ad4-393">Scénář pro tento postup by horizontálního dělení databáze nebo federace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="76ad4-394">Horizontálního dělení používá více databází a směruje různých datových na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="76ad4-394">Sharding uses multiple databases and routes different data to each database.</span></span> <span data-ttu-id="76ad4-395">Pokud každé malé dávky k jiné databázi, může být efektivnější pak paralelní provádění operací.</span><span class="sxs-lookup"><span data-stu-id="76ad4-395">If each small batch is going to a different database, then performing the operations in parallel can be more efficient.</span></span> <span data-ttu-id="76ad4-396">Zvýšení výkonu však není dostatečně významné chcete použít jako základ pro rozhodnutí a použít horizontálního dělení databáze ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-396">However, the performance gain is not significant enough to use as the basis for a decision to use database sharding in your solution.</span></span>

<span data-ttu-id="76ad4-397">V některé návrhy můžete paralelní provádění menší dávek způsobit lepší propustnost požadavky v rámci systému zatížení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="76ad4-398">V takovém případě i když je rychlejší zpracovat jeden větší batch, více listů paralelní zpracování může být efektivnější.</span><span class="sxs-lookup"><span data-stu-id="76ad4-398">In this case, even though it is quicker to process a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="76ad4-399">Pokud používáte paralelní provádění, vezměte v úvahu řízení maximální počet pracovních vláken.</span><span class="sxs-lookup"><span data-stu-id="76ad4-399">If you do use parallel execution, consider controlling the maximum number of worker threads.</span></span> <span data-ttu-id="76ad4-400">Zmenšete počet může být výsledkem nižší výskyt kolizí a rychlejší dobu provádění.</span><span class="sxs-lookup"><span data-stu-id="76ad4-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="76ad4-401">Zvažte také další zátěže, které to umístí na cílovou databázi v připojení a transakce.</span><span class="sxs-lookup"><span data-stu-id="76ad4-401">Also, consider the additional load that this places on the target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="76ad4-402">Faktory související výkonu</span><span class="sxs-lookup"><span data-stu-id="76ad4-402">Related performance factors</span></span>
<span data-ttu-id="76ad4-403">Dávkování ovlivní také typické pokyny na výkon databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="76ad4-404">Můžete například vložit pro tabulky, které mají velký primární klíč, nebo mnoho neclusterované indexy je snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="76ad4-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="76ad4-405">Pokud parametry s hodnotou tabulky pomocí uložené procedury, můžete použít příkaz **SET NOCOUNT ON** na začátku procesu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-405">If table-valued parameters use a stored procedure, you can use the command **SET NOCOUNT ON** at the beginning of the procedure.</span></span> <span data-ttu-id="76ad4-406">Tento příkaz potlačí návrat počet ovlivněných řádků v postupu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-406">This statement suppresses the return of the count of the affected rows in the procedure.</span></span> <span data-ttu-id="76ad4-407">Ale v našich testech se použití **SET NOCOUNT ON** nemělo žádný vliv nebo snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-407">However, in our tests, the use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="76ad4-408">Test uložené procedury bylo jednoduché s jedním **vložit** příkazu z parametru s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-408">The test stored procedure was simple with a single **INSERT** command from the table-valued parameter.</span></span> <span data-ttu-id="76ad4-409">Je možné, že by složitější uložené procedury těžit z tohoto prohlášení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="76ad4-410">Ale Nepředpokládejte, že přidání **SET NOCOUNT ON** uložené procedury automaticky zvyšuje výkon.</span><span class="sxs-lookup"><span data-stu-id="76ad4-410">But don’t assume that adding **SET NOCOUNT ON** to your stored procedure automatically improves performance.</span></span> <span data-ttu-id="76ad4-411">Chcete-li pochopení dopadu, otestovat vaše uložené procedury s i bez **SET NOCOUNT ON** příkaz.</span><span class="sxs-lookup"><span data-stu-id="76ad4-411">To understand the effect, test your stored procedure with and without the **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="76ad4-412">Dávkování scénáře</span><span class="sxs-lookup"><span data-stu-id="76ad4-412">Batching scenarios</span></span>
<span data-ttu-id="76ad4-413">Následující části popisují, jak používat parametry s hodnotou tabulky v tři scénáře aplikací.</span><span class="sxs-lookup"><span data-stu-id="76ad4-413">The following sections describe how to use table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="76ad4-414">První scénář popisuje, jak ukládání do vyrovnávací paměti a dávkování vzájemně spolupracují.</span><span class="sxs-lookup"><span data-stu-id="76ad4-414">The first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="76ad4-415">Druhý scénář zlepšuje výkon provádění operací s podrobnostmi v jedné uložené procedury volání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-415">The second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="76ad4-416">Poslední scénář popisuje, jak používat parametry s hodnotou tabulky v operace "UPSERT".</span><span class="sxs-lookup"><span data-stu-id="76ad4-416">The final scenario shows how to use table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="76ad4-417">Ukládání do vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="76ad4-417">Buffering</span></span>
<span data-ttu-id="76ad4-418">I když je několik scénářů, které jsou zřejmé kandidáta pro dávkování, existuje mnoho scénářů, které může využívat výhod dávkování zpožděné zpracování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="76ad4-419">Zpožděné zpracování však představuje větší riziko, že data jsou v případě neočekávaného selhání ztraceny.</span><span class="sxs-lookup"><span data-stu-id="76ad4-419">However, delayed processing also carries a greater risk that the data is lost in the event of an unexpected failure.</span></span> <span data-ttu-id="76ad4-420">Je důležité pochopit toto riziko a zvážit důsledky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-420">It is important to understand this risk and consider the consequences.</span></span>

<span data-ttu-id="76ad4-421">Představte si třeba webové aplikace, která sleduje navigační historii jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-421">For example, consider a web application that tracks the navigation history of each user.</span></span> <span data-ttu-id="76ad4-422">S každým požadavkem stránky může aplikace Změna databáze volání k zaznamenání zobrazení stránky uživatele.</span><span class="sxs-lookup"><span data-stu-id="76ad4-422">On each page request, the application could make a database call to record the user’s page view.</span></span> <span data-ttu-id="76ad4-423">Ale vyšší výkon a škálovatelnost lze dosáhnout ukládání do vyrovnávací paměti navigační aktivity uživatelů a pak odešle tato data do databáze v dávkách.</span><span class="sxs-lookup"><span data-stu-id="76ad4-423">But higher performance and scalability can be achieved by buffering the users’ navigation activities and then sending this data to the database in batches.</span></span> <span data-ttu-id="76ad4-424">Můžete aktivovat aktualizaci databáze uplynulý čas nebo velikost vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="76ad4-424">You can trigger the database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="76ad4-425">Pravidlo může například určit, že by měl po 20 sekund nebo pokud vyrovnávací paměť dosáhne 1000 položek zpracování dávky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-425">For example, a rule could specify that the batch should be processed after 20 seconds or when the buffer reaches 1000 items.</span></span>

<span data-ttu-id="76ad4-426">Následující příklad kódu používá [reaktivní rozšíření - Rx](https://msdn.microsoft.com/data/gg577609) ke zpracování ve vyrovnávací paměti události vyvolané službou třída monitorování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-426">The following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) to process buffered events raised by a monitoring class.</span></span> <span data-ttu-id="76ad4-427">Když vyrovnávací vyplní celé nebo dosaženo časového limitu, je odeslána dávky dat uživatele do databáze s parametr s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-427">When the buffer fills or a timeout is reached, the batch of user data is sent to the database with a table-valued parameter.</span></span>

<span data-ttu-id="76ad4-428">Následující třídy NavHistoryData modelů navigační podrobností o uživateli.</span><span class="sxs-lookup"><span data-stu-id="76ad4-428">The following NavHistoryData class models the user navigation details.</span></span> <span data-ttu-id="76ad4-429">Obsahuje základní informace, jako je identifikátor uživatele, adresu URL přístup a doba přístupu k.</span><span class="sxs-lookup"><span data-stu-id="76ad4-429">It contains basic information such as the user identifier, the URL accessed, and the access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="76ad4-430">Třída NavHistoryDataMonitor je zodpovědná za ukládání do vyrovnávací paměti navigační data uživatele do databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-430">The NavHistoryDataMonitor class is responsible for buffering the user navigation data to the database.</span></span> <span data-ttu-id="76ad4-431">Obsahuje metody, RecordUserNavigationEntry, který odpovídá zobrazením **OnAdded** událostí.</span><span class="sxs-lookup"><span data-stu-id="76ad4-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="76ad4-432">Následující kód ukazuje konstruktor logiky, která používá Rx můžete vytvořit kolekci pozorovatelné založené na události.</span><span class="sxs-lookup"><span data-stu-id="76ad4-432">The following code shows the constructor logic that uses Rx to create an observable collection based on the event.</span></span> <span data-ttu-id="76ad4-433">Pak přihlásí se k této kolekci pozorovatelné s metodou vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="76ad4-433">It then subscribes to this observable collection with the Buffer method.</span></span> <span data-ttu-id="76ad4-434">Přetížení Určuje, že vyrovnávací paměti by měly být odeslány každých 20 sekund nebo 1 000 položek.</span><span class="sxs-lookup"><span data-stu-id="76ad4-434">The overload specifies that the buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="76ad4-435">Obslužná rutina převede všechny položky ve vyrovnávací paměti typu s hodnotou tabulky a pak předá tento typ uložené procedury, která zpracovává dávky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-435">The handler converts all of the buffered items into a table-valued type and then passes this type to a stored procedure that processes the batch.</span></span> <span data-ttu-id="76ad4-436">Následující kód ukazuje dokončení definice pro NavHistoryDataEventArgs i NavHistoryDataMonitor třídy.</span><span class="sxs-lookup"><span data-stu-id="76ad4-436">The following code shows the complete definition for both the NavHistoryDataEventArgs and the NavHistoryDataMonitor classes.</span></span>

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

<span data-ttu-id="76ad4-437">Pokud chcete používat tuto třídu vyrovnávací paměti, aplikace vytvoří objekt statické NavHistoryDataMonitor.</span><span class="sxs-lookup"><span data-stu-id="76ad4-437">To use this buffering class, the application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="76ad4-438">Pokaždé, když uživatel přistupuje k na stránce aplikace volá metodu NavHistoryDataMonitor.RecordUserNavigationEntry.</span><span class="sxs-lookup"><span data-stu-id="76ad4-438">Each time a user accesses a page, the application calls the NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="76ad4-439">Abyste dbali odesláním těchto položek do databáze v dávkách pokračuje logice vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="76ad4-439">The buffering logic proceeds to take care of sending these entries to the database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="76ad4-440">Hlavní podrobností</span><span class="sxs-lookup"><span data-stu-id="76ad4-440">Master detail</span></span>
<span data-ttu-id="76ad4-441">Parametry s hodnotou tabulky jsou užitečné pro jednoduché scénáře INSERT.</span><span class="sxs-lookup"><span data-stu-id="76ad4-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="76ad4-442">Však může být více náročné dávkového vložení, které zahrnují více než jedna tabulka.</span><span class="sxs-lookup"><span data-stu-id="76ad4-442">However, it can be more challenging to batch inserts that involve more than one table.</span></span> <span data-ttu-id="76ad4-443">Scénář "hlavního a podrobného" je dobrým příkladem.</span><span class="sxs-lookup"><span data-stu-id="76ad4-443">The “master/detail” scenario is a good example.</span></span> <span data-ttu-id="76ad4-444">Hlavní tabulka obsahuje primární entity.</span><span class="sxs-lookup"><span data-stu-id="76ad4-444">The master table identifies the primary entity.</span></span> <span data-ttu-id="76ad4-445">Minimálně jedna tabulka podrobností uložit víc dat o entitě.</span><span class="sxs-lookup"><span data-stu-id="76ad4-445">One or more detail tables store more data about the entity.</span></span> <span data-ttu-id="76ad4-446">V tomto scénáři vynutit relace cizích klíčů relace podrobnosti jedinečný hlavní entity.</span><span class="sxs-lookup"><span data-stu-id="76ad4-446">In this scenario, foreign key relationships enforce the relationship of details to a unique master entity.</span></span> <span data-ttu-id="76ad4-447">Vezměte v úvahu zjednodušenou verzi PurchaseOrder tabulka a její přidružené OrderDetail tabulkou.</span><span class="sxs-lookup"><span data-stu-id="76ad4-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="76ad4-448">Následující příkaz Transact-SQL vytvoří tabulku PurchaseOrder s čtyři sloupce: OrderID, OrderDate, CustomerID a stav.</span><span class="sxs-lookup"><span data-stu-id="76ad4-448">The following Transact-SQL creates the PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="76ad4-449">Každý pořadí obsahuje jeden nebo více nákupy produktu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="76ad4-450">Tyto informace se zaznamená v tabulce PurchaseOrderDetail.</span><span class="sxs-lookup"><span data-stu-id="76ad4-450">This information is captured in the PurchaseOrderDetail table.</span></span> <span data-ttu-id="76ad4-451">Následující příkaz Transact-SQL vytvoří tabulku PurchaseOrderDetail s pěti sloupce: OrderID, OrderDetailID, ProductID, UnitPrice a OrderQty.</span><span class="sxs-lookup"><span data-stu-id="76ad4-451">The following Transact-SQL creates the PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="76ad4-452">Sloupce OrderID v tabulce PurchaseOrderDetail musíte odkázat pořadí z tabulky PurchaseOrder.</span><span class="sxs-lookup"><span data-stu-id="76ad4-452">The OrderID column in the PurchaseOrderDetail table must reference an order from the PurchaseOrder table.</span></span> <span data-ttu-id="76ad4-453">Následující definice cizího klíče vynucuje toto omezení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-453">The following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="76ad4-454">Aby bylo možné používat parametry s hodnotou tabulky, musí mít jeden typ uživatelem definovaná tabulka pro každou cílovou tabulku.</span><span class="sxs-lookup"><span data-stu-id="76ad4-454">In order to use table-valued parameters, you must have one user-defined table type for each target table.</span></span>

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

<span data-ttu-id="76ad4-455">Poté definujte uložené procedury, která přijímá tabulky z těchto typů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="76ad4-456">Tento postup umožňuje aplikaci místně dávky sadu objednávek a podrobnosti o pořadí v jediném volání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-456">This procedure allows an application to locally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="76ad4-457">Následující příkaz Transact-SQL poskytuje deklaraci dokončení uložené procedury v tomto příkladu pořadí nákupu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-457">The following Transact-SQL provides the complete stored procedure declaration for this purchase order example.</span></span>

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

<span data-ttu-id="76ad4-458">V tomto příkladu místně definované @IdentityLink ukládá skutečnými hodnotami OrderID z nově vložené řádky tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-458">In this example, the locally defined @IdentityLink table stores the actual OrderID values from the newly inserted rows.</span></span> <span data-ttu-id="76ad4-459">Tyto identifikátory pořadí se liší od dočasné hodnoty OrderID @orders a @details parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-459">These order identifiers are different from the temporary OrderID values in the @orders and @details table-valued parameters.</span></span> <span data-ttu-id="76ad4-460">Z tohoto důvodu @IdentityLink pak připojí OrderID hodnoty z tabulky @orders parametr skutečné hodnoty OrderID pro nové řádky v tabulce PurchaseOrder.</span><span class="sxs-lookup"><span data-stu-id="76ad4-460">For this reason, the @IdentityLink table then connects the OrderID values from the @orders parameter to the real OrderID values for the new rows in the PurchaseOrder table.</span></span> <span data-ttu-id="76ad4-461">Po provedení tohoto kroku @IdentityLink tabulky můžete usnadnit vkládání podrobnosti pořadí s skutečné OrderID, který splňuje omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="76ad4-461">After this step, the @IdentityLink table can facilitate inserting the order details with the actual OrderID that satisfies the foreign key constraint.</span></span>

<span data-ttu-id="76ad4-462">Tuto uloženou proceduru lze z kódu nebo jiná volání jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="76ad4-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="76ad4-463">Parametry s hodnotou tabulky části tohoto dokumentu příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-463">See the table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="76ad4-464">Následující příkaz Transact-SQL ukazuje způsob volání sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="76ad4-464">The following Transact-SQL shows how to call the sp_InsertOrdersBatch.</span></span>

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

<span data-ttu-id="76ad4-465">Toto řešení umožňuje každé dávky používat sadu OrderID hodnoty, které začínají znakem 1.</span><span class="sxs-lookup"><span data-stu-id="76ad4-465">This solution allows each batch to use a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="76ad4-466">Tyto hodnoty dočasné OrderID popisu relací v dávce, ale skutečný OrderID hodnoty jsou určeny v době operace insert.</span><span class="sxs-lookup"><span data-stu-id="76ad4-466">These temporary OrderID values describe the relationships in the batch, but the actual OrderID values are determined at the time of the insert operation.</span></span> <span data-ttu-id="76ad4-467">Můžete spustit stejné příkazy v předchozím příkladu opakovaně a generovat jedinečný objednávky v databázi.</span><span class="sxs-lookup"><span data-stu-id="76ad4-467">You can run the same statements in the previous example repeatedly and generate unique orders in the database.</span></span> <span data-ttu-id="76ad4-468">Z tohoto důvodu je vhodné přidat další kód nebo databáze logiku, která brání duplicitní objednávky při použití tohoto dávkování techniku.</span><span class="sxs-lookup"><span data-stu-id="76ad4-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="76ad4-469">Tento příklad ukazuje, že i složitější databázových operací, jako je například seznam podrobnosti operace, může zpracovat v dávce pomocí parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="76ad4-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="76ad4-470">UPSERT</span></span>
<span data-ttu-id="76ad4-471">Jiné dávkování scénář zahrnuje současně aktualizaci existujících řádků a vkládání nových řádků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="76ad4-472">Tato operace se někdy označuje jako "UPSERT" (aktualizace + insert) operaci.</span><span class="sxs-lookup"><span data-stu-id="76ad4-472">This operation is sometimes referred to as an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="76ad4-473">Místo samostatné volání k vložení a aktualizace, je nejvhodnější pro tuto úlohu příkazu MERGE.</span><span class="sxs-lookup"><span data-stu-id="76ad4-473">Rather than making separate calls to INSERT and UPDATE, the MERGE statement is best suited to this task.</span></span> <span data-ttu-id="76ad4-474">Příkaz MERGE můžete provést i insert a operace v jednom volání aktualizace.</span><span class="sxs-lookup"><span data-stu-id="76ad4-474">The MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="76ad4-475">Parametry s hodnotou tabulky můžete použít s příkazem MERGE k provádění aktualizací a vložení.</span><span class="sxs-lookup"><span data-stu-id="76ad4-475">Table-valued parameters can be used with the MERGE statement to perform updates and inserts.</span></span> <span data-ttu-id="76ad4-476">Představte si třeba zjednodušené tabulky zaměstnanců, která obsahuje následující sloupce: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="76ad4-476">For example, consider a simplified Employee table that contains the following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="76ad4-477">V tomto příkladu můžete skutečnost, že je SocialSecurityNumber jedinečný ke sloučení více zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="76ad4-477">In this example, you can use the fact that the SocialSecurityNumber is unique to perform a MERGE of multiple employees.</span></span> <span data-ttu-id="76ad4-478">Nejprve vytvořte typ uživatelem definovaná tabulka:</span><span class="sxs-lookup"><span data-stu-id="76ad4-478">First, create the user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="76ad4-479">Dále vytvořte uloženou proceduru nebo napsat kód, který používá příkaz MERGE k provádění aktualizace a vložit.</span><span class="sxs-lookup"><span data-stu-id="76ad4-479">Next, create a stored procedure or write code that uses the MERGE statement to perform the update and insert.</span></span> <span data-ttu-id="76ad4-480">Následující příklad používá příkazu MERGE na parametr s hodnotou tabulky @employees, typu EmployeeTableType.</span><span class="sxs-lookup"><span data-stu-id="76ad4-480">The following example uses the MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="76ad4-481">Obsah @employees tabulky nejsou zobrazeny zde.</span><span class="sxs-lookup"><span data-stu-id="76ad4-481">The contents of the @employees table are not shown here.</span></span>

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

<span data-ttu-id="76ad4-482">Další informace najdete v dokumentaci a příklady příkazu MERGE.</span><span class="sxs-lookup"><span data-stu-id="76ad4-482">For more information, see the documentation and examples for the MERGE statement.</span></span> <span data-ttu-id="76ad4-483">I když můžete provést stejný pracovní v kroku více uložené volání procedury s oddělené INSERT a operace aktualizace, příkazu MERGE je efektivnější.</span><span class="sxs-lookup"><span data-stu-id="76ad4-483">Although the same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, the MERGE statement is more efficient.</span></span> <span data-ttu-id="76ad4-484">Kód databáze můžete také vytvořit volání jazyka Transact-SQL, která pomocí příkazu MERGE přímo bez nutnosti dvě volání databáze pro příkaz INSERT a UPDATE.</span><span class="sxs-lookup"><span data-stu-id="76ad4-484">Database code can also construct Transact-SQL calls that use the MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="76ad4-485">Souhrnná doporučení</span><span class="sxs-lookup"><span data-stu-id="76ad4-485">Recommendation summary</span></span>
<span data-ttu-id="76ad4-486">Následující seznam obsahuje souhrn dávkování doporučení popsané v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="76ad4-486">The following list provides a summary of the batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="76ad4-487">Pokud chcete zvýšit výkon a škálovatelnost databáze SQL aplikace pomocí ukládání do vyrovnávací paměti a dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-487">Use buffering and batching to increase the performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="76ad4-488">Pochopení kompromisy mezi dávkování nebo ukládání do vyrovnávací paměti a odolnost.</span><span class="sxs-lookup"><span data-stu-id="76ad4-488">Understand the tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="76ad4-489">Při selhání role riziko ztráty nezpracované dávku důležitých podnikových dat vyváží výkon výhodou dávkování.</span><span class="sxs-lookup"><span data-stu-id="76ad4-489">During a role failure, the risk of losing an unprocessed batch of business-critical data might outweigh the performance benefit of batching.</span></span>
* <span data-ttu-id="76ad4-490">Pokus zachovat všechna volání do databáze v rámci jednoho datového centra ke snížení latence.</span><span class="sxs-lookup"><span data-stu-id="76ad4-490">Attempt to keep all calls to the database within a single datacenter to reduce latency.</span></span>
* <span data-ttu-id="76ad4-491">Pokud si zvolíte jednu dávkování techniku, parametry s hodnotou tabulky nabízí nejlepší výkon a flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-491">If you choose a single batching technique, table-valued parameters offer the best performance and flexibility.</span></span>
* <span data-ttu-id="76ad4-492">Nejrychlejší vložení výkonu postupujte podle následujících obecných pokynů ale otestovat váš scénář:</span><span class="sxs-lookup"><span data-stu-id="76ad4-492">For the fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="76ad4-493">< 100 řádků pomocí jedné parametrizovaného příkaz INSERT.</span><span class="sxs-lookup"><span data-stu-id="76ad4-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="76ad4-494">< 1 000 řádků použijte parametry s hodnotou tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ad4-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="76ad4-495">Pro > = 1 000 řádků, použijte SqlBulkCopy.</span><span class="sxs-lookup"><span data-stu-id="76ad4-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="76ad4-496">Pro aktualizaci a operace odstranění, použijte parametry s hodnotou tabulky s logiky uložené procedury, která určuje správné operaci na každý řádek v tabulce parametru.</span><span class="sxs-lookup"><span data-stu-id="76ad4-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines the correct operation on each row in the table parameter.</span></span>
* <span data-ttu-id="76ad4-497">Pokyny pro velikost dávky:</span><span class="sxs-lookup"><span data-stu-id="76ad4-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="76ad4-498">Použijte největší velikosti dávky, které dávají smysl pro vaše aplikace a podnikových požadavků.</span><span class="sxs-lookup"><span data-stu-id="76ad4-498">Use the largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="76ad4-499">Vyrovnávat zvýšení výkonu velkých dávek s rizika dočasné nebo závažné selhání.</span><span class="sxs-lookup"><span data-stu-id="76ad4-499">Balance the performance gain of large batches with the risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="76ad4-500">Co je důsledkem opakování nebo ztrátě dat v dávce?</span><span class="sxs-lookup"><span data-stu-id="76ad4-500">What is the consequence of retries or loss of the data in the batch?</span></span> 
  * <span data-ttu-id="76ad4-501">Otestujte největší velikost dávky k ověření, že databáze SQL není odmítnout ho.</span><span class="sxs-lookup"><span data-stu-id="76ad4-501">Test the largest batch size to verify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="76ad4-502">Vytvořte nastavení konfigurace tohoto ovládacího prvku dávkování, jako je například velikost dávky nebo vyrovnávací paměti časový interval.</span><span class="sxs-lookup"><span data-stu-id="76ad4-502">Create configuration settings that control batching, such as the batch size or the buffering time window.</span></span> <span data-ttu-id="76ad4-503">Tato nastavení poskytují flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="76ad4-503">These settings provide flexibility.</span></span> <span data-ttu-id="76ad4-504">Dávkování chování v produkčním prostředí můžete změnit bez opětovného nasazení cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="76ad4-504">You can change the batching behavior in production without redeploying the cloud service.</span></span>
* <span data-ttu-id="76ad4-505">Vyhněte se paralelní zpracování dávek, které působí na jednotlivé tabulky v jedné databáze.</span><span class="sxs-lookup"><span data-stu-id="76ad4-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="76ad4-506">Pokud si zvolíte jedné dávkové rozdělit mezi několik pracovních vláken, spusťte testy můžete určit ideální počet vláken.</span><span class="sxs-lookup"><span data-stu-id="76ad4-506">If you do choose to divide a single batch across multiple worker threads, run tests to determine the ideal number of threads.</span></span> <span data-ttu-id="76ad4-507">Po neurčené prahová hodnota další podprocesy bude snížit výkon, a nikoli zvýšit ji.</span><span class="sxs-lookup"><span data-stu-id="76ad4-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="76ad4-508">Vezměte v úvahu ukládání do vyrovnávací paměti na velikost a čas jako způsob implementace dávkování pro více scénářů.</span><span class="sxs-lookup"><span data-stu-id="76ad4-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76ad4-509">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76ad4-509">Next steps</span></span>
<span data-ttu-id="76ad4-510">Tento článek zaměřuje na jak návrhu databáze a kódování techniky související s dávkování může zlepšit výkon aplikace a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="76ad4-510">This article focused on how database design and coding techniques related to batching can improve your application performance and scalability.</span></span> <span data-ttu-id="76ad4-511">Ale toto je pouze jediný faktor v vaše celková strategie.</span><span class="sxs-lookup"><span data-stu-id="76ad4-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="76ad4-512">Další způsoby, jak zvýšit výkon a škálovatelnost, najdete v části [Azure SQL Database – Průvodce výkonem pro izolované databáze](sql-database-performance-guidance.md) a [cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="76ad4-512">For more ways to improve performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

