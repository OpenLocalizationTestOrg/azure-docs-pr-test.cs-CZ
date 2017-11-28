---
title: "Přehled o výkonu dotazu pro Azure SQL Database | Microsoft Docs"
description: "Monitorování výkonu dotazu identifikuje většinu využívání procesoru dotazů pro databázi SQL Azure."
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
ms.openlocfilehash: 1925d4ff8f5b16a0df56de987f8653cfd8441c52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-query-performance-insight"></a><span data-ttu-id="f41fd-103">Informace o výkonu dotazů databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="f41fd-103">Azure SQL Database Query Performance Insight</span></span>
<span data-ttu-id="f41fd-104">Správa a ladění výkonu relačních databází je náročné úlohu, která vyžaduje značné znalosti a investice čas.</span><span class="sxs-lookup"><span data-stu-id="f41fd-104">Managing and tuning the performance of relational databases is a challenging task that requires significant expertise and time investment.</span></span> <span data-ttu-id="f41fd-105">Informace o výkonu dotazů umožňuje trávit méně času řešení potíží s výkonem databáze tím, že poskytuje následující:</span><span class="sxs-lookup"><span data-stu-id="f41fd-105">Query Performance Insight allows you to spend less time troubleshooting database performance by providing the following:</span></span>

* <span data-ttu-id="f41fd-106">Podrobnější přehled o vaší spotřeby prostředků (DTU) databází.</span><span class="sxs-lookup"><span data-stu-id="f41fd-106">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="f41fd-107">Nejčastějších dotazů podle počtu procesorů a doba trvání nebo provádění, který lze ladit potenciálně pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-107">The top queries by CPU/Duration/Execution count, which can potentially be tuned for improved performance.</span></span>
* <span data-ttu-id="f41fd-108">Možnost k podrobnostem na podrobné informace o dotazu, zobrazit jeho historie využití prostředků a text.</span><span class="sxs-lookup"><span data-stu-id="f41fd-108">The ability to drill down into the details of a query, view its text and history of resource utilization.</span></span> 
* <span data-ttu-id="f41fd-109">Poznámky, které se zobrazí akce prováděné při ladění výkonu [Advisor databáze SQL Azure](sql-database-advisor.md)</span><span class="sxs-lookup"><span data-stu-id="f41fd-109">Performance tuning annotations that show actions performed by [SQL Azure Database Advisor](sql-database-advisor.md)</span></span>  



## <a name="prerequisites"></a><span data-ttu-id="f41fd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f41fd-110">Prerequisites</span></span>
* <span data-ttu-id="f41fd-111">Query Performance Insight vyžaduje, aby [úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx) je aktivní databáze.</span><span class="sxs-lookup"><span data-stu-id="f41fd-111">Query Performance Insight requires that [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) is active on your database.</span></span> <span data-ttu-id="f41fd-112">Pokud není spuštěné úložiště dotazů, vyzve k portálu můžete zapnout.</span><span class="sxs-lookup"><span data-stu-id="f41fd-112">If Query Store is not running, the portal prompts you to turn it on.</span></span>

## <a name="permissions"></a><span data-ttu-id="f41fd-113">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="f41fd-113">Permissions</span></span>
<span data-ttu-id="f41fd-114">Následující [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) používat informace o výkonu dotazů jsou potřeba oprávnění:</span><span class="sxs-lookup"><span data-stu-id="f41fd-114">The following [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions are required to use Query Performance Insight:</span></span> 

* <span data-ttu-id="f41fd-115">**Čtečka**, **vlastníka**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **Přispěvatel serveru SQL** se vyžadují oprávnění k zobrazení nejvyšší prostředku využívání dotazy a grafy.</span><span class="sxs-lookup"><span data-stu-id="f41fd-115">**Reader**, **Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required to view the top resource consuming queries and charts.</span></span> 
* <span data-ttu-id="f41fd-116">**Vlastník**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **SQL serveru Přispěvatel** oprávnění jsou vyžadována k zobrazení text dotazu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-116">**Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required to view query text.</span></span>

## <a name="using-query-performance-insight"></a><span data-ttu-id="f41fd-117">Pomocí Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="f41fd-117">Using Query Performance Insight</span></span>
<span data-ttu-id="f41fd-118">Informace o výkonu dotazů se snadno používá:</span><span class="sxs-lookup"><span data-stu-id="f41fd-118">Query Performance Insight is easy to use:</span></span>

* <span data-ttu-id="f41fd-119">Otevřete [portál Azure](https://portal.azure.com/) a najít databázi, které chcete prověřit.</span><span class="sxs-lookup"><span data-stu-id="f41fd-119">Open [Azure portal](https://portal.azure.com/) and find database that you want to examine.</span></span> 
  * <span data-ttu-id="f41fd-120">Z nabídky na levé straně, v rámci podpory a řešení potíží vyberte "Query Performance Insight".</span><span class="sxs-lookup"><span data-stu-id="f41fd-120">From left-hand side menu, under support and troubleshooting, select “Query Performance Insight”.</span></span>
* <span data-ttu-id="f41fd-121">Na první kartě projděte si seznam nejčastějších dotazů využívání prostředků.</span><span class="sxs-lookup"><span data-stu-id="f41fd-121">On the first tab, review the list of top resource-consuming queries.</span></span>
* <span data-ttu-id="f41fd-122">Vyberte jednotlivé dotazy zobrazíte její podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f41fd-122">Select an individual query to view its details.</span></span>
* <span data-ttu-id="f41fd-123">Otevřete [Poradce pro funkci SQL Azure Database](sql-database-advisor.md) a zkontrolujte, zda jsou k dispozici žádná doporučení.</span><span class="sxs-lookup"><span data-stu-id="f41fd-123">Open [SQL Azure Database Advisor](sql-database-advisor.md) and check if any recommendations are available.</span></span>
* <span data-ttu-id="f41fd-124">Pomocí posuvníků nebo zvětšení ikon do zjištěnou interval změnit.</span><span class="sxs-lookup"><span data-stu-id="f41fd-124">Use sliders or zoom icons to change observed interval.</span></span>
  
    ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> <span data-ttu-id="f41fd-126">Několik hodin dat musí být zachyceny úložiště dotazů pro databázi SQL zajistit přehled o výkonu dotazu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-126">A couple hours of data needs to be captured by Query Store for SQL Database to provide query performance insights.</span></span> <span data-ttu-id="f41fd-127">Pokud databáze nemá žádnou aktivitu nebo úložiště dotazů nebyl aktivní během určitého časového období, grafy bude prázdný, při zobrazení toto časové období.</span><span class="sxs-lookup"><span data-stu-id="f41fd-127">If the database has no activity or Query Store was not active during a certain time period, the charts will be empty when displaying that time period.</span></span> <span data-ttu-id="f41fd-128">Pokud není spuštěna, může kdykoli povolit úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-128">You may enable Query Store at any time if it is not running.</span></span>   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a><span data-ttu-id="f41fd-129">Zkontrolujte nejvíce využívají dotazy procesor</span><span class="sxs-lookup"><span data-stu-id="f41fd-129">Review top CPU consuming queries</span></span>
<span data-ttu-id="f41fd-130">V [portál](http://portal.azure.com) postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f41fd-130">In the [portal](http://portal.azure.com) do the following:</span></span>

1. <span data-ttu-id="f41fd-131">Přejděte do databáze SQL a klikněte na tlačítko **všechna nastavení** > **podporu + Poradce při potížích s** > **dotaz na informace o výkonu**.</span><span class="sxs-lookup"><span data-stu-id="f41fd-131">Browse to a SQL database and click **All settings** > **Support + Troubleshooting** > **Query performance insight**.</span></span> 
   
    ![Query Performance Insight][1]
   
    <span data-ttu-id="f41fd-133">Otevře zobrazení nejčastějších dotazů a jsou uvedeny nejčastějších dotazů využívání procesoru.</span><span class="sxs-lookup"><span data-stu-id="f41fd-133">The top queries view opens and the top CPU consuming queries are listed.</span></span>
2. <span data-ttu-id="f41fd-134">Klikněte na kolem grafu podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f41fd-134">Click around the chart for details.</span></span><br><span data-ttu-id="f41fd-135">Na začátek řádku zobrazuje celkový počet jednotek DTU % pro databázi, zatímco pruhy zobrazit využití procesoru % spotřebovávají vybrané dotazy během vybrané intervalu (například pokud **za minulý týden** je vybrána každý řádek představuje jeden den).</span><span class="sxs-lookup"><span data-stu-id="f41fd-135">The top line shows overall DTU% for the database, while the bars show CPU% consumed by the selected queries during the selected interval (for example, if **Past week** is selected each bar represents one day).</span></span>
   
    ![nejčastějších dotazů][2]
   
    <span data-ttu-id="f41fd-137">Mřížky dolní představuje souhrnné informace pro viditelné dotazy.</span><span class="sxs-lookup"><span data-stu-id="f41fd-137">The bottom grid represents aggregated information for the visible queries.</span></span>
   
   * <span data-ttu-id="f41fd-138">ID dotazu – jedinečný identifikátor dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="f41fd-138">Query ID - unique identifier of query inside database.</span></span>
   * <span data-ttu-id="f41fd-139">Procesor na dotaz během pozorovatelné intervalu (záleží na agregační funkce).</span><span class="sxs-lookup"><span data-stu-id="f41fd-139">CPU per query during observable interval (depends on aggregation function).</span></span>
   * <span data-ttu-id="f41fd-140">Doba trvání za dotazu (záleží na agregační funkce).</span><span class="sxs-lookup"><span data-stu-id="f41fd-140">Duration per query (depends on aggregation function).</span></span>
   * <span data-ttu-id="f41fd-141">Celkový počet spuštěních pro konkrétní dotaz.</span><span class="sxs-lookup"><span data-stu-id="f41fd-141">Total number of executions for a particular query.</span></span>
     
     <span data-ttu-id="f41fd-142">Vyberte nebo zrušte jednotlivé dotazy zahrnout nebo vyloučit z grafu pomocí zaškrtávacích políček.</span><span class="sxs-lookup"><span data-stu-id="f41fd-142">Select or clear individual queries to include or exclude them from the chart using checkboxes.</span></span>
3. <span data-ttu-id="f41fd-143">Pokud vaše data zastaralá, klikněte na tlačítko **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f41fd-143">If your data becomes stale, click the **Refresh** button.</span></span>
4. <span data-ttu-id="f41fd-144">Pomocí posuvníků a přiblížení tlačítka změnit interval pozorování a prozkoumat špičky: ![nastavení](./media/sql-database-query-performance/zoom.png)</span><span class="sxs-lookup"><span data-stu-id="f41fd-144">You can use sliders and zoom buttons to change observation interval and investigate spikes:  ![settings](./media/sql-database-query-performance/zoom.png)</span></span>
5. <span data-ttu-id="f41fd-145">Případně, pokud chcete jiné zobrazení, můžete vybrat **vlastní** kartě a nastavte:</span><span class="sxs-lookup"><span data-stu-id="f41fd-145">Optionally, if you want a different view, you can select **Custom** tab and set:</span></span>
   
   * <span data-ttu-id="f41fd-146">Metrika (procesor, doba trvání, počet provedení)</span><span class="sxs-lookup"><span data-stu-id="f41fd-146">Metric (CPU, duration, execution count)</span></span>
   * <span data-ttu-id="f41fd-147">Časový interval (poslední týden minulý měsíc posledních 24 hodin).</span><span class="sxs-lookup"><span data-stu-id="f41fd-147">Time interval (Last 24 hours, Past week, Past month).</span></span> 
   * <span data-ttu-id="f41fd-148">Počet dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-148">Number of queries.</span></span>
   * <span data-ttu-id="f41fd-149">Agregační funkce.</span><span class="sxs-lookup"><span data-stu-id="f41fd-149">Aggregation function.</span></span>
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a><span data-ttu-id="f41fd-151">Zobrazení podrobností o jednotlivých dotazů</span><span class="sxs-lookup"><span data-stu-id="f41fd-151">Viewing individual query details</span></span>
<span data-ttu-id="f41fd-152">Chcete-li zobrazit podrobnosti o dotazu:</span><span class="sxs-lookup"><span data-stu-id="f41fd-152">To view query details:</span></span>

1. <span data-ttu-id="f41fd-153">Klikněte na jakýkoli dotaz ve seznam nejčastějších dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-153">Click any query in the list of top queries.</span></span>
   
    ![Podrobnosti](./media/sql-database-query-performance/details.png)
2. <span data-ttu-id="f41fd-155">Otevře zobrazení podrobností a počet procesorů energie a doba trvání nebo provádění dotazů je rozdělena v čase.</span><span class="sxs-lookup"><span data-stu-id="f41fd-155">The details view opens and the queries CPU consumption/Duration/Execution count is broken down over time.</span></span>
3. <span data-ttu-id="f41fd-156">Klikněte na kolem grafu podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="f41fd-156">Click around the chart for details.</span></span>
   
   * <span data-ttu-id="f41fd-157">Horní graf znázorňuje řádek s celkovou % DTU databáze a pruhy jsou využívány službou vybraný dotaz % využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="f41fd-157">Top chart shows line with overall database DTU%, and the bars are CPU% consumed by the selected query.</span></span>
   * <span data-ttu-id="f41fd-158">Druhý graf zobrazuje celkovou dobu trvání podle vybraný dotaz.</span><span class="sxs-lookup"><span data-stu-id="f41fd-158">Second chart shows total duration by the selected query.</span></span>
   * <span data-ttu-id="f41fd-159">Dolní graf zobrazuje celkový počet spuštěních podle vybraný dotaz.</span><span class="sxs-lookup"><span data-stu-id="f41fd-159">Bottom chart shows total number of executions by the selected query.</span></span>
     
     ![Podrobnosti o dotazu][3]
4. <span data-ttu-id="f41fd-161">V případě potřeby pomocí posuvníků, přiblíží tlačítka nebo klikněte na tlačítko **nastavení** přizpůsobit zobrazení dat dotazu, nebo vyberte jiné časové období.</span><span class="sxs-lookup"><span data-stu-id="f41fd-161">Optionally, use sliders, zoom buttons or click **Settings** to customize how query data is displayed, or to pick a different time period.</span></span>

## <a name="review-top-queries-per-duration"></a><span data-ttu-id="f41fd-162">Zkontrolujte nejčastějších dotazů za doba trvání</span><span class="sxs-lookup"><span data-stu-id="f41fd-162">Review top queries per duration</span></span>
<span data-ttu-id="f41fd-163">V poslední aktualizaci Query Performance Insight, zavedli jsme dvě nové metriky, které pomáhají identifikovat potenciální problémová místa: doba trvání a provádění count.</span><span class="sxs-lookup"><span data-stu-id="f41fd-163">In the recent update of Query Performance Insight, we introduced two new metrics that can help you identify potential bottlenecks: duration and execution count.</span></span><br>

<span data-ttu-id="f41fd-164">Dlouho běžící dotazy mít největší potenciálně pro uzamčení prostředky delší blokování jiných uživatelů a omezení škálovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="f41fd-164">Long-running queries have the greatest potential for locking resources longer, blocking other users, and limiting scalability.</span></span> <span data-ttu-id="f41fd-165">Jsou také nejlepší kandidáty pro optimalizaci.</span><span class="sxs-lookup"><span data-stu-id="f41fd-165">They are also the best candidates for optimization.</span></span><br>

<span data-ttu-id="f41fd-166">K identifikaci dlouho běžící dotazy:</span><span class="sxs-lookup"><span data-stu-id="f41fd-166">To identify long running queries:</span></span>

1. <span data-ttu-id="f41fd-167">Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze</span><span class="sxs-lookup"><span data-stu-id="f41fd-167">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="f41fd-168">Změnit metriky být **doba trvání**</span><span class="sxs-lookup"><span data-stu-id="f41fd-168">Change metrics to be **duration**</span></span>
3. <span data-ttu-id="f41fd-169">Vyberte počet dotazů a pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-169">Select number of queries and observation interval</span></span>
4. <span data-ttu-id="f41fd-170">Vyberte funkci agregace</span><span class="sxs-lookup"><span data-stu-id="f41fd-170">Select aggregation function</span></span>
   
   * <span data-ttu-id="f41fd-171">**Součet** sečte všechny doba provádění dotazu během celého pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-171">**Sum** adds up all query execution time during whole observation interval.</span></span>
   * <span data-ttu-id="f41fd-172">**Maximální počet** vyhledá dotazy byl v celé pozorovacím intervalu maximální dobu spuštění.</span><span class="sxs-lookup"><span data-stu-id="f41fd-172">**Max** finds queries which execution time was maximum at whole observation interval.</span></span>
   * <span data-ttu-id="f41fd-173">**Průměr** najde průměrný čas provádění všech spuštění dotazu a zobrazit je nejlepší mimo tyto průměry.</span><span class="sxs-lookup"><span data-stu-id="f41fd-173">**Avg** finds average execution time of all query executions and show you the top out of these averages.</span></span> 
     
     ![Doba trvání dotazu][4]

## <a name="review-top-queries-per-execution-count"></a><span data-ttu-id="f41fd-175">Zkontrolujte nejčastějších dotazů na počet provedení</span><span class="sxs-lookup"><span data-stu-id="f41fd-175">Review top queries per execution count</span></span>
<span data-ttu-id="f41fd-176">Vysoký počet spuštěních nemusí ovlivňovat samotná databáze a může být využití prostředků nízké, ale může získat celkový aplikace pomalé.</span><span class="sxs-lookup"><span data-stu-id="f41fd-176">High number of executions might not be affecting database itself and resources usage can be low, but overall application might get slow.</span></span>

<span data-ttu-id="f41fd-177">V některých případech počet velmi vysoká provedení může vést ke zvýšení z síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-177">In some cases, very high execution count may lead to increase of network round trips.</span></span> <span data-ttu-id="f41fd-178">Odezev výrazně ovlivnit výkon.</span><span class="sxs-lookup"><span data-stu-id="f41fd-178">Round trips significantly affect performance.</span></span> <span data-ttu-id="f41fd-179">Jsou předmětem latence sítě a podřízený server latence.</span><span class="sxs-lookup"><span data-stu-id="f41fd-179">They are subject to network latency and to downstream server latency.</span></span> 

<span data-ttu-id="f41fd-180">Například mnoho řízené daty weby výraznou přístup k databázi pro každý požadavek uživatele.</span><span class="sxs-lookup"><span data-stu-id="f41fd-180">For example, many data-driven Web sites heavily access the database for every user request.</span></span> <span data-ttu-id="f41fd-181">Při připojení sdružování pomůže, zvýšená síťový provoz a zpracování na serveru databáze může nepříznivě ovlivnit výkon.</span><span class="sxs-lookup"><span data-stu-id="f41fd-181">While connection pooling helps, the increased network traffic and processing load on the database server can adversely affect performance.</span></span>  <span data-ttu-id="f41fd-182">Obecné Rady, jak se má zachovat odezev na absolutním minimu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-182">General advice is to keep round trips to an absolute minimum.</span></span>

<span data-ttu-id="f41fd-183">Chcete-li identifikovat často spustit dotazy dotazy ("chatty"):</span><span class="sxs-lookup"><span data-stu-id="f41fd-183">To identify frequently executed queries (“chatty”) queries:</span></span>

1. <span data-ttu-id="f41fd-184">Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze</span><span class="sxs-lookup"><span data-stu-id="f41fd-184">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="f41fd-185">Změnit metriky být **počet provedení**</span><span class="sxs-lookup"><span data-stu-id="f41fd-185">Change metrics to be **execution count**</span></span>
3. <span data-ttu-id="f41fd-186">Vyberte počet dotazů a pozorovacím intervalu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-186">Select number of queries and observation interval</span></span>
   
    ![počet provedení dotazu][5]

## <a name="understanding-performance-tuning-annotations"></a><span data-ttu-id="f41fd-188">Principy poznámky vyladění výkonu</span><span class="sxs-lookup"><span data-stu-id="f41fd-188">Understanding performance tuning annotations</span></span>
<span data-ttu-id="f41fd-189">Při prohlížení zatížení Query Performance Insight, můžete si všimnout ikony s svislá čára nad grafu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-189">While exploring your workload in Query Performance Insight, you might notice icons with vertical line on top of the chart.</span></span><br>

<span data-ttu-id="f41fd-190">Tyto ikony jsou poznámky; představují výkonu by to ovlivnilo provedené akce [Poradce pro funkci SQL Azure Database](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="f41fd-190">These icons are annotations; they represent performance affecting actions performed by [SQL Azure Database Advisor](sql-database-advisor.md).</span></span> <span data-ttu-id="f41fd-191">Ve výběru ukázáním poznámky získáte základní informace o akci:</span><span class="sxs-lookup"><span data-stu-id="f41fd-191">By hovering annotation, you get basic information about the action:</span></span>

![dotaz – Poznámka][6]

<span data-ttu-id="f41fd-193">Pokud chcete další informace nebo použít doporučení služby advisor, klikněte na ikonu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-193">If you want to know more or apply advisor recommendation, click the icon.</span></span> <span data-ttu-id="f41fd-194">Otevře se podrobnosti akce.</span><span class="sxs-lookup"><span data-stu-id="f41fd-194">It will open details of action.</span></span> <span data-ttu-id="f41fd-195">Pokud je aktivní doporučení můžete provést okamžitou pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="f41fd-195">If it’s an active recommendation you can apply it straight away using command.</span></span>

![Podrobnosti o dotazu – Poznámka][7]

### <a name="multiple-annotations"></a><span data-ttu-id="f41fd-197">Více poznámek.</span><span class="sxs-lookup"><span data-stu-id="f41fd-197">Multiple annotations.</span></span>
<span data-ttu-id="f41fd-198">Je možné, že kvůli úroveň přiblížení, bude získat poznámky, které mezi sebou blíží sbalené do jednoho.</span><span class="sxs-lookup"><span data-stu-id="f41fd-198">It’s possible, that because of zoom level, annotations that are close to each other will get collapsed into one.</span></span> <span data-ttu-id="f41fd-199">To bude reprezentovat speciální ikonou, kliknutím ji bude otevřete nové okno, kde seznam seskupené poznámky se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f41fd-199">This will be represented by special icon, clicking it will open new blade where list of grouped annotations will be shown.</span></span>
<span data-ttu-id="f41fd-200">Korelace dotazy a ladění akce výkonu vám může pomoct lépe pochopit, vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="f41fd-200">Correlating queries and performance tuning actions can help to better understand your workload.</span></span> 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a><span data-ttu-id="f41fd-201">Optimalizace konfigurace úložiště dotazů pro Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="f41fd-201">Optimizing the Query Store configuration for Query Performance Insight</span></span>
<span data-ttu-id="f41fd-202">Během používání Query Performance Insight může dojít k následující zprávy úložiště dotazů:</span><span class="sxs-lookup"><span data-stu-id="f41fd-202">During your use of Query Performance Insight, you might encounter the following Query Store messages:</span></span>

* <span data-ttu-id="f41fd-203">"Úložiště dotazů není správně nakonfigurována v této databázi.</span><span class="sxs-lookup"><span data-stu-id="f41fd-203">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="f41fd-204">Kliknutím sem získáte další informace."</span><span class="sxs-lookup"><span data-stu-id="f41fd-204">Click here to learn more."</span></span>
* <span data-ttu-id="f41fd-205">"Úložiště dotazů není správně nakonfigurována v této databázi.</span><span class="sxs-lookup"><span data-stu-id="f41fd-205">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="f41fd-206">Kliknutím zde proveďte změnu nastavení."</span><span class="sxs-lookup"><span data-stu-id="f41fd-206">Click here to change settings."</span></span> 

<span data-ttu-id="f41fd-207">Tyto zprávy se obvykle zobrazují, když úložiště dotazů není moct shromažďovat nová data.</span><span class="sxs-lookup"><span data-stu-id="f41fd-207">These messages usually appear when Query Store is not able to collect new data.</span></span> 

<span data-ttu-id="f41fd-208">Prvním případě se stane, když úložiště dotazů je ve stavu jen pro čtení a parametry nejsou nastavené optimálně.</span><span class="sxs-lookup"><span data-stu-id="f41fd-208">First case happens when Query Store is in Read-Only state and parameters are set optimally.</span></span> <span data-ttu-id="f41fd-209">Můžete opravit zvýšit velikost úložiště dotazů nebo vymazání úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-209">You can fix this by increasing size of Query Store or clearing Query Store.</span></span>

![tlačítko qds][8]

<span data-ttu-id="f41fd-211">Druhém případě se stane, když úložiště dotazů je vypnutý nebo parametry nejsou nastavené optimálně.</span><span class="sxs-lookup"><span data-stu-id="f41fd-211">Second case happens when Query Store is Off or parameters aren’t set optimally.</span></span> <br><span data-ttu-id="f41fd-212">Můžete změnit zásady uchovávání informací a zachycení a povolit úložiště dotazů spuštěním níže uvedené příkazy nebo přímo z portálu:</span><span class="sxs-lookup"><span data-stu-id="f41fd-212">You can change the Retention and Capture policy and enable Query Store by executing commands below or directly from portal:</span></span>

![tlačítko qds][9]

### <a name="recommended-retention-and-capture-policy"></a><span data-ttu-id="f41fd-214">Doporučené zásady uchovávání informací a zachycení</span><span class="sxs-lookup"><span data-stu-id="f41fd-214">Recommended retention and capture policy</span></span>
<span data-ttu-id="f41fd-215">Existují dva typy zásad uchovávání informací:</span><span class="sxs-lookup"><span data-stu-id="f41fd-215">There are two types of retention policies:</span></span>

* <span data-ttu-id="f41fd-216">Velikost na základě - li nastavit na hodnotu AUTO ho bude vyčistit data automaticky, když je dosaženo blíží maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="f41fd-216">Size based - if set to AUTO it will clean data automatically when near max size is reached.</span></span>
* <span data-ttu-id="f41fd-217">Na základě času – ve výchozím nastavení, které jsme bude nastavena na 30 dní, což znamená, pokud úložiště dotazů se dostatek místa, odstraní se dotazu informace, které jsou starší než 30 dní</span><span class="sxs-lookup"><span data-stu-id="f41fd-217">Time based - by default we will set it to 30 days, which means, if Query Store will run out of space, it will delete query information older than 30 days</span></span>

<span data-ttu-id="f41fd-218">Zaznamenat zásad může být nastaven na:</span><span class="sxs-lookup"><span data-stu-id="f41fd-218">Capture policy could be set to:</span></span>

* <span data-ttu-id="f41fd-219">**Všechny** -zaznamená všechny dotazy.</span><span class="sxs-lookup"><span data-stu-id="f41fd-219">**All** - Captures all queries.</span></span>
* <span data-ttu-id="f41fd-220">**Automatické** -málo časté dotazy a dotazy s zanedbatelný kompilace a provádění trvání jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="f41fd-220">**Auto** - Infrequent queries and queries with insignificant compile and execution duration are ignored.</span></span> <span data-ttu-id="f41fd-221">Interně jsou určit prahové hodnoty pro počet, kompilace a runtime dobu provádění.</span><span class="sxs-lookup"><span data-stu-id="f41fd-221">Thresholds for execution count, compile and runtime duration are internally determined.</span></span> <span data-ttu-id="f41fd-222">Toto je výchozí možnost.</span><span class="sxs-lookup"><span data-stu-id="f41fd-222">This is the default option.</span></span>
* <span data-ttu-id="f41fd-223">**Žádný** -úložiště dotazů zastaví sběr nové dotazy, ale jsou stále shromážděných statistik modulu runtime pro již zaznamenané dotazy.</span><span class="sxs-lookup"><span data-stu-id="f41fd-223">**None** - Query Store stops capturing new queries, however runtime stats for already captured queries are still collected.</span></span>

<span data-ttu-id="f41fd-224">Doporučujeme nastavení všech zásad AUTOMATICKY a vyčištění zásad do 30 dnů:</span><span class="sxs-lookup"><span data-stu-id="f41fd-224">We recommend setting all policies to AUTO and clean policy to 30 days:</span></span>

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

<span data-ttu-id="f41fd-225">Zvětšete velikost úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-225">Increase size of Query Store.</span></span> <span data-ttu-id="f41fd-226">To můžete provést připojení k databázi a vydání následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="f41fd-226">This could be performed by connecting to a database and issuing following query:</span></span>

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

<span data-ttu-id="f41fd-227">Použití těchto nastavení budou nakonec úložiště dotazů shromažďování nové dotazy, ale pokud nechcete čekat můžete vymazat úložiště dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-227">Applying these settings will eventually make Query Store collecting new queries, however if you don’t want to wait you can clear Query Store.</span></span> 

> [!NOTE]
> <span data-ttu-id="f41fd-228">Provádění následující dotaz odstraní všechny aktuální informace v úložišti dotazů.</span><span class="sxs-lookup"><span data-stu-id="f41fd-228">Executing following query will delete all current information in the Query Store.</span></span> 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a><span data-ttu-id="f41fd-229">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f41fd-229">Summary</span></span>
<span data-ttu-id="f41fd-230">Informace o výkonu dotazů vám pomůže pochopit dopad dotazu úlohy a jak se týká spotřeby prostředků databáze.</span><span class="sxs-lookup"><span data-stu-id="f41fd-230">Query Performance Insight helps you understand the impact of your query workload and how it relates to database resource consumption.</span></span> <span data-ttu-id="f41fd-231">Pomocí této funkce se další informace o nejnáročnější dotazy a snadno identifikovat ty opravit dřív, než narostou problém.</span><span class="sxs-lookup"><span data-stu-id="f41fd-231">With this feature, you will learn about the top consuming queries, and easily identify the ones to fix before they become a problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f41fd-232">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f41fd-232">Next steps</span></span>
<span data-ttu-id="f41fd-233">Další doporučení týkající se vylepšení výkonu SQL databáze, klikněte na tlačítko [doporučení](sql-database-advisor.md) na **Query Performance Insight** okno.</span><span class="sxs-lookup"><span data-stu-id="f41fd-233">For additional recommendations about improving the performance of your SQL database, click [Recommendations](sql-database-advisor.md) on the **Query Performance Insight** blade.</span></span>

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

