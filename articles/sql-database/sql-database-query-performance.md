---
title: "Přehled o výkonu aaaQuery pro Azure SQL Database | Microsoft Docs"
description: "Monitorování výkonu dotazu identifikuje hello většina využívání procesoru dotazy pro Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 01cca26f85193c679365585cd676449c9db00e1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-query-performance-insight"></a><span data-ttu-id="62ea7-103">Informace o výkonu dotazů databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="62ea7-103">Azure SQL Database Query Performance Insight</span></span>
<span data-ttu-id="62ea7-104">Správa a ladění výkonu hello relačních databází je náročné úlohu, která vyžaduje značné znalosti a investice čas.</span><span class="sxs-lookup"><span data-stu-id="62ea7-104">Managing and tuning hello performance of relational databases is a challenging task that requires significant expertise and time investment.</span></span> <span data-ttu-id="62ea7-105">Informace o výkonu dotazů můžete toospend méně času řešení potíží s výkonem databáze tím, že poskytuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="62ea7-105">Query Performance Insight allows you toospend less time troubleshooting database performance by providing hello following:</span></span>

* <span data-ttu-id="62ea7-106">Podrobnější přehled o vaší spotřeby prostředků (DTU) databází.</span><span class="sxs-lookup"><span data-stu-id="62ea7-106">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="62ea7-107">Hello nejčastějších dotazů podle počtu procesorů a doba trvání nebo provádění, který lze ladit potenciálně pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-107">hello top queries by CPU/Duration/Execution count, which can potentially be tuned for improved performance.</span></span>
* <span data-ttu-id="62ea7-108">Dobrý den možnost toodrill dolů do hello podrobnosti o dotazu, zobrazit historii využití prostředků a text.</span><span class="sxs-lookup"><span data-stu-id="62ea7-108">hello ability toodrill down into hello details of a query, view its text and history of resource utilization.</span></span> 
* <span data-ttu-id="62ea7-109">Poznámky, které se zobrazí akce prováděné při ladění výkonu [Advisor databáze SQL Azure](sql-database-advisor.md)</span><span class="sxs-lookup"><span data-stu-id="62ea7-109">Performance tuning annotations that show actions performed by [SQL Azure Database Advisor](sql-database-advisor.md)</span></span>  



## <a name="prerequisites"></a><span data-ttu-id="62ea7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="62ea7-110">Prerequisites</span></span>
* <span data-ttu-id="62ea7-111">Query Performance Insight vyžaduje, aby [úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx) je aktivní databáze.</span><span class="sxs-lookup"><span data-stu-id="62ea7-111">Query Performance Insight requires that [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) is active on your database.</span></span> <span data-ttu-id="62ea7-112">Pokud není spuštěné úložiště dotazů, portál hello vás vyzve k tooturn ho na.</span><span class="sxs-lookup"><span data-stu-id="62ea7-112">If Query Store is not running, hello portal prompts you tooturn it on.</span></span>

## <a name="permissions"></a><span data-ttu-id="62ea7-113">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="62ea7-113">Permissions</span></span>
<span data-ttu-id="62ea7-114">Následující Hello [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění jsou požadované toouse Query Performance Insight:</span><span class="sxs-lookup"><span data-stu-id="62ea7-114">hello following [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions are required toouse Query Performance Insight:</span></span> 

* <span data-ttu-id="62ea7-115">**Čtečka**, **vlastníka**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **Přispěvatel serveru SQL** jsou oprávnění vyžaduje tooview hello nejvyšší na prostředky dotazy a grafy.</span><span class="sxs-lookup"><span data-stu-id="62ea7-115">**Reader**, **Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview hello top resource consuming queries and charts.</span></span> 
* <span data-ttu-id="62ea7-116">**Vlastník**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **SQL serveru Přispěvatel** oprávnění jsou požadované tooview text dotazu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-116">**Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview query text.</span></span>

## <a name="using-query-performance-insight"></a><span data-ttu-id="62ea7-117">Pomocí Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="62ea7-117">Using Query Performance Insight</span></span>
<span data-ttu-id="62ea7-118">Informace o výkonu dotazů je snadno toouse:</span><span class="sxs-lookup"><span data-stu-id="62ea7-118">Query Performance Insight is easy toouse:</span></span>

* <span data-ttu-id="62ea7-119">Otevřete [portál Azure](https://portal.azure.com/) a najít databáze, které chcete tooexamine.</span><span class="sxs-lookup"><span data-stu-id="62ea7-119">Open [Azure portal](https://portal.azure.com/) and find database that you want tooexamine.</span></span> 
  * <span data-ttu-id="62ea7-120">Z nabídky na levé straně, v rámci podpory a řešení potíží vyberte "Query Performance Insight".</span><span class="sxs-lookup"><span data-stu-id="62ea7-120">From left-hand side menu, under support and troubleshooting, select “Query Performance Insight”.</span></span>
* <span data-ttu-id="62ea7-121">Na první kartě hello projděte si hello seznam nejčastějších dotazů využívání prostředků.</span><span class="sxs-lookup"><span data-stu-id="62ea7-121">On hello first tab, review hello list of top resource-consuming queries.</span></span>
* <span data-ttu-id="62ea7-122">Vyberte konkrétní dotaz tooview její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62ea7-122">Select an individual query tooview its details.</span></span>
* <span data-ttu-id="62ea7-123">Otevřete [Poradce pro funkci SQL Azure Database](sql-database-advisor.md) a zkontrolujte, zda jsou k dispozici žádná doporučení.</span><span class="sxs-lookup"><span data-stu-id="62ea7-123">Open [SQL Azure Database Advisor](sql-database-advisor.md) and check if any recommendations are available.</span></span>
* <span data-ttu-id="62ea7-124">Pomocí posuvníků nebo zvětšení ikony toochange zjištěnými intervalu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-124">Use sliders or zoom icons toochange observed interval.</span></span>
  
    ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> <span data-ttu-id="62ea7-126">Několik hodin dat. musí toobe zachycenou úložiště dotazů pro databázi SQL tooprovide query performance Insight.</span><span class="sxs-lookup"><span data-stu-id="62ea7-126">A couple hours of data needs toobe captured by Query Store for SQL Database tooprovide query performance insights.</span></span> <span data-ttu-id="62ea7-127">Pokud nemá žádnou aktivitu hello databáze nebo úložiště dotazů nebyl aktivní během určitého časového období, grafy hello bude prázdný, při zobrazení toto časové období.</span><span class="sxs-lookup"><span data-stu-id="62ea7-127">If hello database has no activity or Query Store was not active during a certain time period, hello charts will be empty when displaying that time period.</span></span> <span data-ttu-id="62ea7-128">Pokud není spuštěna, může kdykoli povolit úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-128">You may enable Query Store at any time if it is not running.</span></span>   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a><span data-ttu-id="62ea7-129">Zkontrolujte nejvíce využívají dotazy procesor</span><span class="sxs-lookup"><span data-stu-id="62ea7-129">Review top CPU consuming queries</span></span>
<span data-ttu-id="62ea7-130">V hello [portál](http://portal.azure.com) hello následující:</span><span class="sxs-lookup"><span data-stu-id="62ea7-130">In hello [portal](http://portal.azure.com) do hello following:</span></span>

1. <span data-ttu-id="62ea7-131">Procházet tooa SQL database a klikněte na tlačítko **všechna nastavení** > **podporu + Poradce při potížích s** > **dotaz na informace o výkonu**.</span><span class="sxs-lookup"><span data-stu-id="62ea7-131">Browse tooa SQL database and click **All settings** > **Support + Troubleshooting** > **Query performance insight**.</span></span> 
   
    ![Query Performance Insight][1]
   
    <span data-ttu-id="62ea7-133">Otevře zobrazení Hello nejčastějších dotazů a jsou uvedeny hello nejvyšší procesoru náročné dotazy.</span><span class="sxs-lookup"><span data-stu-id="62ea7-133">hello top queries view opens and hello top CPU consuming queries are listed.</span></span>
2. <span data-ttu-id="62ea7-134">Klikněte na kolem grafu hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62ea7-134">Click around hello chart for details.</span></span><br><span data-ttu-id="62ea7-135">Hello horní řádek zobrazuje celkový počet jednotek DTU % pro databázi hello, zatímco hello řádky zobrazit využití procesoru % spotřebovávají hello vybrané dotazy během intervalu vybrané hello (například pokud **za minulý týden** je vybrána každý řádek představuje jeden den).</span><span class="sxs-lookup"><span data-stu-id="62ea7-135">hello top line shows overall DTU% for hello database, while hello bars show CPU% consumed by hello selected queries during hello selected interval (for example, if **Past week** is selected each bar represents one day).</span></span>
   
    ![nejčastějších dotazů][2]
   
    <span data-ttu-id="62ea7-137">Hello dolní mřížky představuje souhrnné informace pro dotazy viditelné hello.</span><span class="sxs-lookup"><span data-stu-id="62ea7-137">hello bottom grid represents aggregated information for hello visible queries.</span></span>
   
   * <span data-ttu-id="62ea7-138">ID dotazu – jedinečný identifikátor dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="62ea7-138">Query ID - unique identifier of query inside database.</span></span>
   * <span data-ttu-id="62ea7-139">Procesor na dotaz během pozorovatelné intervalu (záleží na agregační funkce).</span><span class="sxs-lookup"><span data-stu-id="62ea7-139">CPU per query during observable interval (depends on aggregation function).</span></span>
   * <span data-ttu-id="62ea7-140">Doba trvání za dotazu (záleží na agregační funkce).</span><span class="sxs-lookup"><span data-stu-id="62ea7-140">Duration per query (depends on aggregation function).</span></span>
   * <span data-ttu-id="62ea7-141">Celkový počet spuštěních pro konkrétní dotaz.</span><span class="sxs-lookup"><span data-stu-id="62ea7-141">Total number of executions for a particular query.</span></span>
     
     <span data-ttu-id="62ea7-142">Vyberte nebo zrušte tooinclude jednotlivé dotazy nebo vyloučit z hello grafu pomocí zaškrtávacích políček.</span><span class="sxs-lookup"><span data-stu-id="62ea7-142">Select or clear individual queries tooinclude or exclude them from hello chart using checkboxes.</span></span>
3. <span data-ttu-id="62ea7-143">Pokud vaše data zastaralá, klikněte na tlačítko hello **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="62ea7-143">If your data becomes stale, click hello **Refresh** button.</span></span>
4. <span data-ttu-id="62ea7-144">Pomocí posuvníků a přiblížení tlačítka toochange pozorovacím intervalu a prozkoumat špičky: ![nastavení](./media/sql-database-query-performance/zoom.png)</span><span class="sxs-lookup"><span data-stu-id="62ea7-144">You can use sliders and zoom buttons toochange observation interval and investigate spikes:  ![settings](./media/sql-database-query-performance/zoom.png)</span></span>
5. <span data-ttu-id="62ea7-145">Případně, pokud chcete jiné zobrazení, můžete vybrat **vlastní** kartě a nastavte:</span><span class="sxs-lookup"><span data-stu-id="62ea7-145">Optionally, if you want a different view, you can select **Custom** tab and set:</span></span>
   
   * <span data-ttu-id="62ea7-146">Metrika (procesor, doba trvání, počet provedení)</span><span class="sxs-lookup"><span data-stu-id="62ea7-146">Metric (CPU, duration, execution count)</span></span>
   * <span data-ttu-id="62ea7-147">Časový interval (poslední týden minulý měsíc posledních 24 hodin).</span><span class="sxs-lookup"><span data-stu-id="62ea7-147">Time interval (Last 24 hours, Past week, Past month).</span></span> 
   * <span data-ttu-id="62ea7-148">Počet dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-148">Number of queries.</span></span>
   * <span data-ttu-id="62ea7-149">Agregační funkce.</span><span class="sxs-lookup"><span data-stu-id="62ea7-149">Aggregation function.</span></span>
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a><span data-ttu-id="62ea7-151">Zobrazení podrobností o jednotlivých dotazů</span><span class="sxs-lookup"><span data-stu-id="62ea7-151">Viewing individual query details</span></span>
<span data-ttu-id="62ea7-152">Podrobnosti o tooview dotazu:</span><span class="sxs-lookup"><span data-stu-id="62ea7-152">tooview query details:</span></span>

1. <span data-ttu-id="62ea7-153">Klikněte na jakýkoli dotaz ve hello seznam nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-153">Click any query in hello list of top queries.</span></span>
   
    ![Podrobnosti](./media/sql-database-query-performance/details.png)
2. <span data-ttu-id="62ea7-155">Otevře zobrazení podrobností Hello a počet energie a doba trvání nebo provádění procesorů dotazy hello je rozdělena v čase.</span><span class="sxs-lookup"><span data-stu-id="62ea7-155">hello details view opens and hello queries CPU consumption/Duration/Execution count is broken down over time.</span></span>
3. <span data-ttu-id="62ea7-156">Klikněte na kolem grafu hello podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="62ea7-156">Click around hello chart for details.</span></span>
   
   * <span data-ttu-id="62ea7-157">Horní graf znázorňuje řádek s celkovou % DTU databáze a hello řádky jsou využívány službou hello vybraný dotaz % využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="62ea7-157">Top chart shows line with overall database DTU%, and hello bars are CPU% consumed by hello selected query.</span></span>
   * <span data-ttu-id="62ea7-158">Druhý graf zobrazuje celkovou dobu trvání podle hello vybraný dotaz.</span><span class="sxs-lookup"><span data-stu-id="62ea7-158">Second chart shows total duration by hello selected query.</span></span>
   * <span data-ttu-id="62ea7-159">Dolní graf zobrazuje celkový počet spuštěních podle hello vybraný dotaz.</span><span class="sxs-lookup"><span data-stu-id="62ea7-159">Bottom chart shows total number of executions by hello selected query.</span></span>
     
     ![Podrobnosti o dotazu][3]
4. <span data-ttu-id="62ea7-161">V případě potřeby pomocí posuvníků, přiblíží tlačítka nebo klikněte na tlačítko **nastavení** toocustomize zobrazení dotaz na data nebo toopick na jiné časové období.</span><span class="sxs-lookup"><span data-stu-id="62ea7-161">Optionally, use sliders, zoom buttons or click **Settings** toocustomize how query data is displayed, or toopick a different time period.</span></span>

## <a name="review-top-queries-per-duration"></a><span data-ttu-id="62ea7-162">Zkontrolujte nejčastějších dotazů za doba trvání</span><span class="sxs-lookup"><span data-stu-id="62ea7-162">Review top queries per duration</span></span>
<span data-ttu-id="62ea7-163">V poslední aktualizaci hello sady Query Performance Insight, zavedli jsme dvě nové metriky, které pomáhají identifikovat potenciální problémová místa: doba trvání a provádění count.</span><span class="sxs-lookup"><span data-stu-id="62ea7-163">In hello recent update of Query Performance Insight, we introduced two new metrics that can help you identify potential bottlenecks: duration and execution count.</span></span><br>

<span data-ttu-id="62ea7-164">Dlouho běžící dotazy mají hello největší případným zamykání prostředky delší blokování jiných uživatelů a omezení škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="62ea7-164">Long-running queries have hello greatest potential for locking resources longer, blocking other users, and limiting scalability.</span></span> <span data-ttu-id="62ea7-165">Jsou i hello nejlepší kandidáty pro optimalizaci.</span><span class="sxs-lookup"><span data-stu-id="62ea7-165">They are also hello best candidates for optimization.</span></span><br>

<span data-ttu-id="62ea7-166">tooidentify dlouho běžící dotazy:</span><span class="sxs-lookup"><span data-stu-id="62ea7-166">tooidentify long running queries:</span></span>

1. <span data-ttu-id="62ea7-167">Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze</span><span class="sxs-lookup"><span data-stu-id="62ea7-167">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="62ea7-168">Změnit metriky toobe **doba trvání**</span><span class="sxs-lookup"><span data-stu-id="62ea7-168">Change metrics toobe **duration**</span></span>
3. <span data-ttu-id="62ea7-169">Vyberte počet dotazů a pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-169">Select number of queries and observation interval</span></span>
4. <span data-ttu-id="62ea7-170">Vyberte funkci agregace</span><span class="sxs-lookup"><span data-stu-id="62ea7-170">Select aggregation function</span></span>
   
   * <span data-ttu-id="62ea7-171">**Součet** sečte všechny doba provádění dotazu během celého pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-171">**Sum** adds up all query execution time during whole observation interval.</span></span>
   * <span data-ttu-id="62ea7-172">**Maximální počet** vyhledá dotazy byl v celé pozorovacím intervalu maximální dobu spuštění.</span><span class="sxs-lookup"><span data-stu-id="62ea7-172">**Max** finds queries which execution time was maximum at whole observation interval.</span></span>
   * <span data-ttu-id="62ea7-173">**Průměr** najde Průměrná doba provádění všech dotazování spuštěních a ukazují hello horní mimo tyto průměry.</span><span class="sxs-lookup"><span data-stu-id="62ea7-173">**Avg** finds average execution time of all query executions and show you hello top out of these averages.</span></span> 
     
     ![Doba trvání dotazu][4]

## <a name="review-top-queries-per-execution-count"></a><span data-ttu-id="62ea7-175">Zkontrolujte nejčastějších dotazů na počet provedení</span><span class="sxs-lookup"><span data-stu-id="62ea7-175">Review top queries per execution count</span></span>
<span data-ttu-id="62ea7-176">Vysoký počet spuštěních nemusí ovlivňovat samotná databáze a může být využití prostředků nízké, ale může získat celkový aplikace pomalé.</span><span class="sxs-lookup"><span data-stu-id="62ea7-176">High number of executions might not be affecting database itself and resources usage can be low, but overall application might get slow.</span></span>

<span data-ttu-id="62ea7-177">V některých případech může vést provádění velmi vysoký počet tooincrease síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-177">In some cases, very high execution count may lead tooincrease of network round trips.</span></span> <span data-ttu-id="62ea7-178">Odezev výrazně ovlivnit výkon.</span><span class="sxs-lookup"><span data-stu-id="62ea7-178">Round trips significantly affect performance.</span></span> <span data-ttu-id="62ea7-179">Jsou to subjektu toonetwork latenci a dobu toodownstream serveru.</span><span class="sxs-lookup"><span data-stu-id="62ea7-179">They are subject toonetwork latency and toodownstream server latency.</span></span> 

<span data-ttu-id="62ea7-180">Například mnoho řízené daty weby výraznou přístup hello databázi pro každý požadavek uživatele.</span><span class="sxs-lookup"><span data-stu-id="62ea7-180">For example, many data-driven Web sites heavily access hello database for every user request.</span></span> <span data-ttu-id="62ea7-181">Při připojení sdružování pomůže, hello více síťových přenosů a zatížení na serveru databáze hello může nepříznivě ovlivnit výkon.</span><span class="sxs-lookup"><span data-stu-id="62ea7-181">While connection pooling helps, hello increased network traffic and processing load on hello database server can adversely affect performance.</span></span>  <span data-ttu-id="62ea7-182">Obecné Rady, jak je tookeep zaokrouhlí absolutní minimální tooan služebních cest.</span><span class="sxs-lookup"><span data-stu-id="62ea7-182">General advice is tookeep round trips tooan absolute minimum.</span></span>

<span data-ttu-id="62ea7-183">tooidentify provést nejčastější dotazy ("chatty") dotazy:</span><span class="sxs-lookup"><span data-stu-id="62ea7-183">tooidentify frequently executed queries (“chatty”) queries:</span></span>

1. <span data-ttu-id="62ea7-184">Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze</span><span class="sxs-lookup"><span data-stu-id="62ea7-184">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="62ea7-185">Změnit metriky toobe **počet provedení**</span><span class="sxs-lookup"><span data-stu-id="62ea7-185">Change metrics toobe **execution count**</span></span>
3. <span data-ttu-id="62ea7-186">Vyberte počet dotazů a pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-186">Select number of queries and observation interval</span></span>
   
    ![počet provedení dotazu][5]

## <a name="understanding-performance-tuning-annotations"></a><span data-ttu-id="62ea7-188">Principy poznámky vyladění výkonu</span><span class="sxs-lookup"><span data-stu-id="62ea7-188">Understanding performance tuning annotations</span></span>
<span data-ttu-id="62ea7-189">Při prohlížení zatížení Query Performance Insight, můžete si všimnout ikony s svislá čára nad hello grafu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-189">While exploring your workload in Query Performance Insight, you might notice icons with vertical line on top of hello chart.</span></span><br>

<span data-ttu-id="62ea7-190">Tyto ikony jsou poznámky; představují výkonu by to ovlivnilo provedené akce [Poradce pro funkci SQL Azure Database](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="62ea7-190">These icons are annotations; they represent performance affecting actions performed by [SQL Azure Database Advisor](sql-database-advisor.md).</span></span> <span data-ttu-id="62ea7-191">Ve výběru ukázáním poznámky získáte základní informace o hello akce:</span><span class="sxs-lookup"><span data-stu-id="62ea7-191">By hovering annotation, you get basic information about hello action:</span></span>

![dotaz – Poznámka][6]

<span data-ttu-id="62ea7-193">Chcete-li další tooknow nebo použít doporučení služby advisor, klikněte na ikonu hello.</span><span class="sxs-lookup"><span data-stu-id="62ea7-193">If you want tooknow more or apply advisor recommendation, click hello icon.</span></span> <span data-ttu-id="62ea7-194">Otevře se podrobnosti akce.</span><span class="sxs-lookup"><span data-stu-id="62ea7-194">It will open details of action.</span></span> <span data-ttu-id="62ea7-195">Pokud je aktivní doporučení můžete provést okamžitou pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="62ea7-195">If it’s an active recommendation you can apply it straight away using command.</span></span>

![Podrobnosti o dotazu – Poznámka][7]

### <a name="multiple-annotations"></a><span data-ttu-id="62ea7-197">Více poznámek.</span><span class="sxs-lookup"><span data-stu-id="62ea7-197">Multiple annotations.</span></span>
<span data-ttu-id="62ea7-198">Je možné, že kvůli úroveň přiblížení, bude získat poznámky, které jsou zavřít tooeach jiných sbalené do jednoho.</span><span class="sxs-lookup"><span data-stu-id="62ea7-198">It’s possible, that because of zoom level, annotations that are close tooeach other will get collapsed into one.</span></span> <span data-ttu-id="62ea7-199">To bude reprezentovat speciální ikonou, kliknutím ji bude otevřete nové okno, kde seznam seskupené poznámky se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="62ea7-199">This will be represented by special icon, clicking it will open new blade where list of grouped annotations will be shown.</span></span>
<span data-ttu-id="62ea7-200">Korelace dotazy a ladění akce výkonu může pomoci toobetter pochopit vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="62ea7-200">Correlating queries and performance tuning actions can help toobetter understand your workload.</span></span> 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a><span data-ttu-id="62ea7-201">Optimalizace hello konfigurace úložiště dotazů pro Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="62ea7-201">Optimizing hello Query Store configuration for Query Performance Insight</span></span>
<span data-ttu-id="62ea7-202">Během používání Query Performance Insight může dojít k hello následující úložiště dotazů zprávy:</span><span class="sxs-lookup"><span data-stu-id="62ea7-202">During your use of Query Performance Insight, you might encounter hello following Query Store messages:</span></span>

* <span data-ttu-id="62ea7-203">"Úložiště dotazů není správně nakonfigurována v této databázi.</span><span class="sxs-lookup"><span data-stu-id="62ea7-203">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="62ea7-204">Kliknutím sem toolearn Další."</span><span class="sxs-lookup"><span data-stu-id="62ea7-204">Click here toolearn more."</span></span>
* <span data-ttu-id="62ea7-205">"Úložiště dotazů není správně nakonfigurována v této databázi.</span><span class="sxs-lookup"><span data-stu-id="62ea7-205">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="62ea7-206">Kliknutím sem nastavení toochange."</span><span class="sxs-lookup"><span data-stu-id="62ea7-206">Click here toochange settings."</span></span> 

<span data-ttu-id="62ea7-207">Tyto zprávy se obvykle zobrazují, když úložiště dotazů není možné toocollect nová data.</span><span class="sxs-lookup"><span data-stu-id="62ea7-207">These messages usually appear when Query Store is not able toocollect new data.</span></span> 

<span data-ttu-id="62ea7-208">Prvním případě se stane, když úložiště dotazů je ve stavu jen pro čtení a parametry nejsou nastavené optimálně.</span><span class="sxs-lookup"><span data-stu-id="62ea7-208">First case happens when Query Store is in Read-Only state and parameters are set optimally.</span></span> <span data-ttu-id="62ea7-209">Můžete opravit zvýšit velikost úložiště dotazů nebo vymazání úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-209">You can fix this by increasing size of Query Store or clearing Query Store.</span></span>

![tlačítko qds][8]

<span data-ttu-id="62ea7-211">Druhém případě se stane, když úložiště dotazů je vypnutý nebo parametry nejsou nastavené optimálně.</span><span class="sxs-lookup"><span data-stu-id="62ea7-211">Second case happens when Query Store is Off or parameters aren’t set optimally.</span></span> <br><span data-ttu-id="62ea7-212">Hello uchovávání a zachycení zásady a povolit úložiště dotazů, můžete změnit tak, že spustíte následující příkazy nebo přímo z portálu:</span><span class="sxs-lookup"><span data-stu-id="62ea7-212">You can change hello Retention and Capture policy and enable Query Store by executing commands below or directly from portal:</span></span>

![tlačítko qds][9]

### <a name="recommended-retention-and-capture-policy"></a><span data-ttu-id="62ea7-214">Doporučené zásady uchovávání informací a zachycení</span><span class="sxs-lookup"><span data-stu-id="62ea7-214">Recommended retention and capture policy</span></span>
<span data-ttu-id="62ea7-215">Existují dva typy zásad uchovávání informací:</span><span class="sxs-lookup"><span data-stu-id="62ea7-215">There are two types of retention policies:</span></span>

* <span data-ttu-id="62ea7-216">Na základě velikosti – když je dosaženo tooAUTO sadu vyčistí dat automaticky při skoro maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="62ea7-216">Size based - if set tooAUTO it will clean data automatically when near max size is reached.</span></span>
* <span data-ttu-id="62ea7-217">Čas na základě – ve výchozím nastavení jsme bude nastavena too30 dní, což znamená, pokud úložiště dotazů se dostatek místa, odstraní se dotazovat informací, které jsou starší než 30 dní</span><span class="sxs-lookup"><span data-stu-id="62ea7-217">Time based - by default we will set it too30 days, which means, if Query Store will run out of space, it will delete query information older than 30 days</span></span>

<span data-ttu-id="62ea7-218">Zaznamenat zásad může být nastaven na:</span><span class="sxs-lookup"><span data-stu-id="62ea7-218">Capture policy could be set to:</span></span>

* <span data-ttu-id="62ea7-219">**Všechny** -zaznamená všechny dotazy.</span><span class="sxs-lookup"><span data-stu-id="62ea7-219">**All** - Captures all queries.</span></span>
* <span data-ttu-id="62ea7-220">**Automatické** -málo časté dotazy a dotazy s zanedbatelný kompilace a provádění trvání jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="62ea7-220">**Auto** - Infrequent queries and queries with insignificant compile and execution duration are ignored.</span></span> <span data-ttu-id="62ea7-221">Interně jsou určit prahové hodnoty pro počet, kompilace a runtime dobu provádění.</span><span class="sxs-lookup"><span data-stu-id="62ea7-221">Thresholds for execution count, compile and runtime duration are internally determined.</span></span> <span data-ttu-id="62ea7-222">Toto je výchozí možnost hello.</span><span class="sxs-lookup"><span data-stu-id="62ea7-222">This is hello default option.</span></span>
* <span data-ttu-id="62ea7-223">**Žádný** -úložiště dotazů zastaví sběr nové dotazy, ale jsou stále shromážděných statistik modulu runtime pro již zaznamenané dotazy.</span><span class="sxs-lookup"><span data-stu-id="62ea7-223">**None** - Query Store stops capturing new queries, however runtime stats for already captured queries are still collected.</span></span>

<span data-ttu-id="62ea7-224">Doporučujeme nastavení všechny zásady tooAUTO a vyčištění zásad too30 dnů:</span><span class="sxs-lookup"><span data-stu-id="62ea7-224">We recommend setting all policies tooAUTO and clean policy too30 days:</span></span>

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

<span data-ttu-id="62ea7-225">Zvětšete velikost úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-225">Increase size of Query Store.</span></span> <span data-ttu-id="62ea7-226">To může mít provádět při připojování tooa databáze a vystavují následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="62ea7-226">This could be performed by connecting tooa database and issuing following query:</span></span>

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

<span data-ttu-id="62ea7-227">Použití těchto nastavení budou nakonec úložiště dotazů shromažďování nové dotazy, ale pokud nechcete, aby toowait můžete vymazat úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-227">Applying these settings will eventually make Query Store collecting new queries, however if you don’t want toowait you can clear Query Store.</span></span> 

> [!NOTE]
> <span data-ttu-id="62ea7-228">Provádění následující dotaz odstraní všechny informace aktuální v hello úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="62ea7-228">Executing following query will delete all current information in hello Query Store.</span></span> 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a><span data-ttu-id="62ea7-229">Souhrn</span><span class="sxs-lookup"><span data-stu-id="62ea7-229">Summary</span></span>
<span data-ttu-id="62ea7-230">Informace o výkonu dotazů vám pomůže pochopit hello dopad dotazu úlohy a jak se týká toodatabase spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="62ea7-230">Query Performance Insight helps you understand hello impact of your query workload and how it relates toodatabase resource consumption.</span></span> <span data-ttu-id="62ea7-231">Pomocí této funkce se další informace o hello nejnáročnější dotazy a snadno identifikovat hello ty, které jsou toofix dřív, než narostou problém.</span><span class="sxs-lookup"><span data-stu-id="62ea7-231">With this feature, you will learn about hello top consuming queries, and easily identify hello ones toofix before they become a problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62ea7-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62ea7-232">Next steps</span></span>
<span data-ttu-id="62ea7-233">Další doporučení týkající se vylepšení výkonu hello vaší databáze SQL, klikněte na tlačítko [doporučení](sql-database-advisor.md) na hello **Query Performance Insight** okno.</span><span class="sxs-lookup"><span data-stu-id="62ea7-233">For additional recommendations about improving hello performance of your SQL database, click [Recommendations](sql-database-advisor.md) on hello **Query Performance Insight** blade.</span></span>

![Poradce pro výkon](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

