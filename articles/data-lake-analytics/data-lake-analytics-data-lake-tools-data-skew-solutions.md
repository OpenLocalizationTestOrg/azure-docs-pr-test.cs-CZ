---
title: "Data zkosení problémy můžete vyřešit pomocí nástroje Azure Data Lake pro Visual Studio | Microsoft Docs"
description: "Řešení potíží s možná řešení problémů zkosení dat pomocí nástroje Azure Data Lake pro Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 9b284ef33be4b935569fc368d81ddf040b2c2b7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="580dd-103">Data zkosení problémy můžete vyřešit pomocí nástroje Azure Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="580dd-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="580dd-104">Co je dat zkreslit?</span><span class="sxs-lookup"><span data-stu-id="580dd-104">What is data skew?</span></span>

<span data-ttu-id="580dd-105">Stručně jsme uvedli, zkosení dat je přepsání reprezentována hodnota.</span><span class="sxs-lookup"><span data-stu-id="580dd-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="580dd-106">Představte si, zda jste přiřadili 50 referentů daň auditovat daň vrátí, jeden revizor pro každý stav USA.</span><span class="sxs-lookup"><span data-stu-id="580dd-106">Imagine that you have assigned 50 tax examiners to audit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="580dd-107">Wyoming referentů, protože naplňování existuje je malý, má málo udělat.</span><span class="sxs-lookup"><span data-stu-id="580dd-107">The Wyoming examiner, because the population there is small, has little to do.</span></span> <span data-ttu-id="580dd-108">V kalifornské ale zkoušející je udržováno velmi zaneprázdněny z důvodu stavu velké naplnění.</span><span class="sxs-lookup"><span data-stu-id="580dd-108">In California, however, the examiner is kept very busy because of the state's large population.</span></span>
    <span data-ttu-id="580dd-109">![Příklad dat zkosení problém](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="580dd-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="580dd-110">V našem scénáři data se nerovnoměrně distribuuje mezi všechny daň kontrolorů, což znamená, že některé referentů musí fungovat více než jiné.</span><span class="sxs-lookup"><span data-stu-id="580dd-110">In our scenario, the data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="580dd-111">Ve vaší vlastní úloze setkáte často situacích jako v příkladu daň revizor sem.</span><span class="sxs-lookup"><span data-stu-id="580dd-111">In your own job, you frequently experience situations like the tax-examiner example here.</span></span> <span data-ttu-id="580dd-112">V další technické podmínky jednoho vrcholu získá mnohem víc dat, než jeho partnerské uzly, situaci, která vytváří Vrchol pracovní víc než ostatní a že nakonec zpomaluje celé úlohy.</span><span class="sxs-lookup"><span data-stu-id="580dd-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes the vertex work more than the others and that eventually slows down an entire job.</span></span> <span data-ttu-id="580dd-113">Co je zhoršení, úloha se nemusí podařit, protože může mít vrcholy, například 5 hodin runtime omezení a omezení 6 GB paměti.</span><span class="sxs-lookup"><span data-stu-id="580dd-113">What's worse, the job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="580dd-114">Řešení problémů s zkosení dat</span><span class="sxs-lookup"><span data-stu-id="580dd-114">Resolving data-skew problems</span></span>

<span data-ttu-id="580dd-115">Azure nástrojů Data Lake pro Visual Studio může pomoct zjistit, jestli vaše úlohy došlo k problému s zkosení data.</span><span class="sxs-lookup"><span data-stu-id="580dd-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="580dd-116">Pokud existuje problém, abyste ho mohli vyřešit tak, že zkusíte řešení v této části.</span><span class="sxs-lookup"><span data-stu-id="580dd-116">If a problem exists, you can resolve it by trying the solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="580dd-117">Řešení 1: Zlepšení, vytváření oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="580dd-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a><span data-ttu-id="580dd-118">Možnost 1: Předem filtrovat zkreslilo hodnota klíče</span><span class="sxs-lookup"><span data-stu-id="580dd-118">Option 1: Filter the skewed key value in advance</span></span>

<span data-ttu-id="580dd-119">Pokud nemá vliv na obchodní logiku, můžete předem filtrovat hodnoty vyšší frekvence.</span><span class="sxs-lookup"><span data-stu-id="580dd-119">If it does not affect your business logic, you can filter the higher-frequency values in advance.</span></span> <span data-ttu-id="580dd-120">Například pokud je celá řada 000 000 000 ve sloupci identifikátoru GUID, nemusí chcete agregovat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="580dd-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want to aggregate that value.</span></span> <span data-ttu-id="580dd-121">Předtím, než agregační, můžete napsat "kde GUID! ="000 000 000"" filtrovat hodnotu Vysoká frekvence.</span><span class="sxs-lookup"><span data-stu-id="580dd-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” to filter the high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="580dd-122">Možnost 2: Vyberte jiný klíč oddílu, nebo distribuce</span><span class="sxs-lookup"><span data-stu-id="580dd-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="580dd-123">V předchozím příkladu Pokud chcete jenom zkontrolovat zatížení daň auditu po celém země, můžete zlepšit distribuci dat, výběrem číslo ID jako klíč.</span><span class="sxs-lookup"><span data-stu-id="580dd-123">In the preceding example, if you want only to check the tax-audit workload all over the country, you can improve the data distribution by selecting the ID number as your key.</span></span> <span data-ttu-id="580dd-124">Výběr jiný oddíl nebo distribučního klíče můžete někdy distribuovat data více rovnoměrně, ale musíte zajistit, že tato volba nemá vliv obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="580dd-124">Picking a different partition or distribution key can sometimes distribute the data more evenly, but you need to make sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="580dd-125">Například k výpočtu daň součet pro každý stav, můžete určit _stavu_ jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="580dd-125">For instance, to calculate the tax sum for each state, you might want to designate _State_ as the partition key.</span></span> <span data-ttu-id="580dd-126">Pokud budete pokračovat k tomuto problému dojde, použijte možnost 3.</span><span class="sxs-lookup"><span data-stu-id="580dd-126">If you continue to experience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="580dd-127">Možnost 3: Přidejte další oddíl nebo distribučního klíče</span><span class="sxs-lookup"><span data-stu-id="580dd-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="580dd-128">Místo použití pouze _stavu_ jako klíč oddílu, můžete použít více než jeden klíč pro dělení.</span><span class="sxs-lookup"><span data-stu-id="580dd-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="580dd-129">Zvažte například přidávání _PSČ_ jako klíčem další oddíl a snížení velikosti dat oddílu a více rovnoměrně distribuovat data.</span><span class="sxs-lookup"><span data-stu-id="580dd-129">For example, consider adding _ZIP Code_ as an additional partition key to reduce data-partition sizes and distribute the data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="580dd-130">Možnost 4: Použití distribučních kruhového dotazování</span><span class="sxs-lookup"><span data-stu-id="580dd-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="580dd-131">Pokud nemůžete najít příslušný klíč pro oddíl a distribuci, můžete zkusit použít distribuci kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="580dd-131">If you cannot find an appropriate key for partition and distribution, you can try to use round-robin distribution.</span></span> <span data-ttu-id="580dd-132">Kruhové dotazování distribuční zpracovává všechny řádky stejně a náhodně umístí je do odpovídajících intervalů.</span><span class="sxs-lookup"><span data-stu-id="580dd-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="580dd-133">Získá rovnoměrně data, ale ztratí polohu informace, nevýhodou, který může také snížit výkon úlohy pro některé operace.</span><span class="sxs-lookup"><span data-stu-id="580dd-133">The data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="580dd-134">Kromě toho agregace zkreslilo klíče chcete přesto provést, problém zkosení dat zachová.</span><span class="sxs-lookup"><span data-stu-id="580dd-134">Additionally, if you are doing aggregation for the skewed key anyway, the data-skew problem will persist.</span></span> <span data-ttu-id="580dd-135">Další informace o distribuci kruhového dotazování, najdete v části distribuce tabulky U-SQL v [CREATE TABLE (U-SQL): vytvoření tabulky se schématem](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="580dd-135">To learn more about round-robin distribution, see the U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-the-query-plan"></a><span data-ttu-id="580dd-136">Řešení 2: Plán dotazu zlepšit</span><span class="sxs-lookup"><span data-stu-id="580dd-136">Solution 2: Improve the query plan</span></span>

### <a name="option-1-use-the-create-statistics-statement"></a><span data-ttu-id="580dd-137">Možnost 1: Použití příkazu CREATE STATISTICS</span><span class="sxs-lookup"><span data-stu-id="580dd-137">Option 1: Use the CREATE STATISTICS statement</span></span>

<span data-ttu-id="580dd-138">U-SQL obsahuje příkaz CREATE STATISTICS tabulky.</span><span class="sxs-lookup"><span data-stu-id="580dd-138">U-SQL provides the CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="580dd-139">Tento příkaz poskytuje další informace k Optimalizátor dotazů o data charakteristiky, třeba hodnotu rozdělení, které jsou uložené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="580dd-139">This statement gives more information to the query optimizer about the data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="580dd-140">Pro většinu dotazů Optimalizátor dotazů již generuje nezbytné statistiku plán dotazu vysoké kvality.</span><span class="sxs-lookup"><span data-stu-id="580dd-140">For most queries, the query optimizer already generates the necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="580dd-141">V některých případech budete muset zlepšit výkon dotazu tak, že vytvoříte další statistiky s CREATE STATISTICS nebo změnou návrhu dotazu.</span><span class="sxs-lookup"><span data-stu-id="580dd-141">Occasionally, you might need to improve query performance by creating additional statistics with CREATE STATISTICS or by modifying the query design.</span></span> <span data-ttu-id="580dd-142">Další informace najdete v tématu [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="580dd-142">For more information, see the [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="580dd-143">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="580dd-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="580dd-144">Statistické informace se automaticky neaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="580dd-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="580dd-145">Pokud aktualizujete data v tabulce bez nutnosti znovu vytvářet statistiku, může odmítnout výkon dotazů.</span><span class="sxs-lookup"><span data-stu-id="580dd-145">If you update the data in a table without re-creating the statistics, the query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="580dd-146">Možnost 2: Použijte SKEWFACTOR</span><span class="sxs-lookup"><span data-stu-id="580dd-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="580dd-147">Pokud chcete součet daň pro každý stav, musíte použít Seskupit podle stavu, postup, který není vyhnout potížím zkosení data.</span><span class="sxs-lookup"><span data-stu-id="580dd-147">If you want to sum the tax for each state, you must use GROUP BY state, an approach that doesn't avoid the data-skew problem.</span></span> <span data-ttu-id="580dd-148">Však může poskytnout nápovědu dat v dotazu k identifikaci dat zkosení v klíče, aby Optimalizátor můžete připravit plán spuštění za vás.</span><span class="sxs-lookup"><span data-stu-id="580dd-148">However, you can provide a data hint in your query to identify data skew in keys so that the optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="580dd-149">Obvykle můžete nastavit parametr jako 0,5 a 1, s 0,5 znamená velkou zkosení nevěnuje význam zkosení a 1.</span><span class="sxs-lookup"><span data-stu-id="580dd-149">Usually, you can set the parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="580dd-150">Protože pomocný parametr ovlivňuje plán spuštění optimalizace pro aktuální příkaz a všechny podřízené příkazy, ujistěte se, že jste před potenciálně nesouměrně rozdělí key-wise agregace Přidat pomocný parametr.</span><span class="sxs-lookup"><span data-stu-id="580dd-150">Because the hint affects execution-plan optimization for the current statement and all downstream statements, be sure to add the hint before the potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="580dd-151">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="580dd-151">Code example:</span></span>

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a><span data-ttu-id="580dd-152">Možnost 3: Použití počtu řádků</span><span class="sxs-lookup"><span data-stu-id="580dd-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="580dd-153">Kromě SKEWFACTOR konkrétní nesouměrně rozdělí klíč připojení v případech, pokud víte, že druhá sada připojené k řádku malé, můžete zjistit Optimalizátor přidáním pomocný parametr počtu řádků v příkazu U-SQL před spojení.</span><span class="sxs-lookup"><span data-stu-id="580dd-153">In addition to SKEWFACTOR, for specific skewed-key join cases, if you know that the other joined row set is small, you can tell the optimizer by adding a ROWCOUNT hint in the U-SQL statement before JOIN.</span></span> <span data-ttu-id="580dd-154">Tímto způsobem můžete Optimalizátor všesměrového vysílání spojení strategie pro zlepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="580dd-154">This way, optimizer can choose a broadcast join strategy to help improve performance.</span></span> <span data-ttu-id="580dd-155">Uvědomte si, že atribut ROWCOUNT nepodaří data zkosení problém vyřešit, ale může nabídnout další pomoc.</span><span class="sxs-lookup"><span data-stu-id="580dd-155">Be aware that ROWCOUNT does not resolve the data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="580dd-156">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="580dd-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a><span data-ttu-id="580dd-157">Řešení 3: Zlepšení uživatelem definované reduktorem a kombinační</span><span class="sxs-lookup"><span data-stu-id="580dd-157">Solution 3: Improve the user-defined reducer and combiner</span></span>

<span data-ttu-id="580dd-158">Někdy může zapisovat uživatelem definovaný operátor řešit složité proces logiky a kvalitně reduktorem a kombinační může zmírnit data zkosení problém v některých případech.</span><span class="sxs-lookup"><span data-stu-id="580dd-158">You can sometimes write a user-defined operator to deal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="580dd-159">Možnost 1: Použití rekurzivní reduktorem, pokud je to možné</span><span class="sxs-lookup"><span data-stu-id="580dd-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="580dd-160">Ve výchozím nastavení uživatelem definované reduktorem běží v režimu tohoto nerekurzivního, což znamená, že snížit pracovní pro klíč je distribuován do jednoho vrcholu.</span><span class="sxs-lookup"><span data-stu-id="580dd-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="580dd-161">Ale pokud vaše data se nesouměrně rozdělí, obrovských sad dat mohou být zpracovány v jednom vrchol a spustit po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="580dd-161">But if your data is skewed, the huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="580dd-162">Chcete-li zvýšit výkon, můžete přidat atribut ve vašem kódu k definování reduktorem spouštění v režimu rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="580dd-162">To improve performance, you can add an attribute in your code to define reducer to run in recursive mode.</span></span> <span data-ttu-id="580dd-163">Potom obrovských sad dat můžete distribuovat do více vrcholy a spustit souběžně, což urychlí úlohu.</span><span class="sxs-lookup"><span data-stu-id="580dd-163">Then, the huge data sets can be distributed to multiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="580dd-164">Chcete-li změnit reduktorem tohoto nerekurzivního na rekurzivní, ujistěte se, že je vaše algoritmus asociativní.</span><span class="sxs-lookup"><span data-stu-id="580dd-164">To change a non-recursive reducer to recursive, you need to make sure that your algorithm is associative.</span></span> <span data-ttu-id="580dd-165">Například je součet asociativní a Medián není.</span><span class="sxs-lookup"><span data-stu-id="580dd-165">For example, the sum is associative, and the median is not.</span></span> <span data-ttu-id="580dd-166">Budete také muset Ujistěte se, že vstupní a výstupní pro reduktorem zachovat stejné schéma.</span><span class="sxs-lookup"><span data-stu-id="580dd-166">You also need to make sure that the input and output for reducer keep the same schema.</span></span>

<span data-ttu-id="580dd-167">Atribut rekurzivní reduktorem:</span><span class="sxs-lookup"><span data-stu-id="580dd-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="580dd-168">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="580dd-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="580dd-169">Možnost 2: Použití režimu kombinační na úrovni řádků, pokud je to možné</span><span class="sxs-lookup"><span data-stu-id="580dd-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="580dd-170">Podobně jako pomocný parametr počtu řádků pro konkrétní připojení k nesouměrně rozdělí klíč případy, kombinační režimu se pokusí distribuovat nastaví velký nesouměrně rozdělí klíč hodnota k více vrcholy tak, aby práce mohou být provedeny souběžně.</span><span class="sxs-lookup"><span data-stu-id="580dd-170">Similar to the ROWCOUNT hint for specific skewed-key join cases, combiner mode tries to distribute huge skewed-key value sets to multiple vertices so that the work can be executed concurrently.</span></span> <span data-ttu-id="580dd-171">Režim kombinační nelze vyřešit problémy zkosení data, ale může nabídnout některé další nápovědu k nastaví velký nesouměrně rozdělí klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="580dd-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="580dd-172">Ve výchozím režimu kombinační je úplná, což znamená, že nemohou být odděleny sady řádků levého a pravého řádku sady.</span><span class="sxs-lookup"><span data-stu-id="580dd-172">By default, the combiner mode is Full, which means that the left row set and right row set cannot be separated.</span></span> <span data-ttu-id="580dd-173">Nastavení režimu jako doleva nebo doprava nebo vnitřní umožňuje připojení k úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="580dd-173">Setting the mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="580dd-174">Systém odděluje odpovídající sady řádků a distribuuje je do více vrcholy, které spustit souběžně.</span><span class="sxs-lookup"><span data-stu-id="580dd-174">The system separates the corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="580dd-175">Než začnete konfigurovat kombinační režimu, ale buďte opatrní zajistit, že je možné oddělit odpovídající sady řádků.</span><span class="sxs-lookup"><span data-stu-id="580dd-175">However, before you configure the combiner mode, be careful to ensure that the corresponding row sets can be separated.</span></span>

<span data-ttu-id="580dd-176">Následující příklad ukazuje sadu oddělených levého řádku.</span><span class="sxs-lookup"><span data-stu-id="580dd-176">The example that follows shows a separated left row set.</span></span> <span data-ttu-id="580dd-177">Každý řádek výstupu závisí na jednom řádku vstupní zleva a potenciálně závisí na všechny řádky z pravé se stejnou hodnotou klíče.</span><span class="sxs-lookup"><span data-stu-id="580dd-177">Each output row depends on a single input row from the left, and it potentially depends on all rows from the right with the same key value.</span></span> <span data-ttu-id="580dd-178">Pokud nastavíte režim kombinační jako vlevo, systém odděluje obrovské doleva řádek nastavení do malé a přiřadí více vrcholy.</span><span class="sxs-lookup"><span data-stu-id="580dd-178">If you set the combiner mode as left, the system separates the huge left-row set into small ones and assigns them to multiple vertices.</span></span>

![Obrázek kombinační režimu](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="580dd-180">Pokud nastavíte režim nesprávný kombinační, kombinace sice méně efektivní a výsledky mohou být další potíže.</span><span class="sxs-lookup"><span data-stu-id="580dd-180">If you set the wrong combiner mode, the combination is less efficient, and the results might be wrong.</span></span>

<span data-ttu-id="580dd-181">Atributy kombinační režimu:</span><span class="sxs-lookup"><span data-stu-id="580dd-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- <span data-ttu-id="580dd-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Každý řádek výstupu závisí na jeden vstupní řádek z doleva (a potenciálně všechny řádky z pravé se stejnou hodnotou klíče).</span><span class="sxs-lookup"><span data-stu-id="580dd-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from the left (and potentially all rows from the right with the same key value).</span></span>

- <span data-ttu-id="580dd-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): každý řádek výstupu závisí na jeden vstupní řádek z vpravo (a potenciálně všechny řádky z levé straně se stejnou hodnotou klíče).</span><span class="sxs-lookup"><span data-stu-id="580dd-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from the right (and potentially all rows from the left with the same key value).</span></span>

- <span data-ttu-id="580dd-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Každý řádek výstupu závisí na jednom řádku vstupní vlevo a vpravo se stejnou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="580dd-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from the left and the right with the same value.</span></span>

<span data-ttu-id="580dd-185">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="580dd-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
