---
title: "problémy aaaResolve zkosení dat pomocí nástroje Azure Data Lake pro Visual Studio | Microsoft Docs"
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
ms.openlocfilehash: 3909fbd89eb40f061268cb7128f7fa84a3c33de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="aebe3-103">Data zkosení problémy můžete vyřešit pomocí nástroje Azure Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aebe3-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="aebe3-104">Co je dat zkreslit?</span><span class="sxs-lookup"><span data-stu-id="aebe3-104">What is data skew?</span></span>

<span data-ttu-id="aebe3-105">Stručně jsme uvedli, zkosení dat je přepsání reprezentována hodnota.</span><span class="sxs-lookup"><span data-stu-id="aebe3-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="aebe3-106">Představte si, zda jste přiřadili 50 referentů daň tooaudit daň vrátí, jeden revizor pro každý stav USA.</span><span class="sxs-lookup"><span data-stu-id="aebe3-106">Imagine that you have assigned 50 tax examiners tooaudit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="aebe3-107">referentů Wyoming Hello, protože existuje naplnění hello je malý, má málo toodo.</span><span class="sxs-lookup"><span data-stu-id="aebe3-107">hello Wyoming examiner, because hello population there is small, has little toodo.</span></span> <span data-ttu-id="aebe3-108">V kalifornské ale revizor hello je udržováno velmi zaneprázdněny z důvodu stavu hello velké naplnění.</span><span class="sxs-lookup"><span data-stu-id="aebe3-108">In California, however, hello examiner is kept very busy because of hello state's large population.</span></span>
    <span data-ttu-id="aebe3-109">![Příklad dat zkosení problém](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="aebe3-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="aebe3-110">V našem scénáři hello nerovnoměrně distribuci dat mezi všechny daň kontrolorů, což znamená, že některé referentů musí fungovat více než jiné.</span><span class="sxs-lookup"><span data-stu-id="aebe3-110">In our scenario, hello data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="aebe3-111">Při vlastní úlohu často docházet situacích jako hello daň revizor zde ukázkový.</span><span class="sxs-lookup"><span data-stu-id="aebe3-111">In your own job, you frequently experience situations like hello tax-examiner example here.</span></span> <span data-ttu-id="aebe3-112">V další technické podmínky získá jednoho vrcholu mnohem víc dat než jeho partnerské uzly situaci, která vytváří hello Vrchol pracovní více než jiné – které nakonec zpomaluje celé úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="aebe3-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes hello vertex work more than hello others and that eventually slows down an entire job.</span></span> <span data-ttu-id="aebe3-113">Co je zhoršení, hello úlohy se nemusí podařit, protože může mít vrcholy, například 5 hodin runtime omezení a omezení 6 GB paměti.</span><span class="sxs-lookup"><span data-stu-id="aebe3-113">What's worse, hello job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="aebe3-114">Řešení problémů s zkosení dat</span><span class="sxs-lookup"><span data-stu-id="aebe3-114">Resolving data-skew problems</span></span>

<span data-ttu-id="aebe3-115">Azure nástrojů Data Lake pro Visual Studio může pomoct zjistit, jestli vaše úlohy došlo k problému s zkosení data.</span><span class="sxs-lookup"><span data-stu-id="aebe3-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="aebe3-116">Pokud existuje problém, abyste ho mohli vyřešit tak, že zkusíte hello řešení v této části.</span><span class="sxs-lookup"><span data-stu-id="aebe3-116">If a problem exists, you can resolve it by trying hello solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="aebe3-117">Řešení 1: Zlepšení, vytváření oddílů tabulky</span><span class="sxs-lookup"><span data-stu-id="aebe3-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a><span data-ttu-id="aebe3-118">Možnost 1: Filtr hello nesouměrně rozdělí hodnota klíče předem</span><span class="sxs-lookup"><span data-stu-id="aebe3-118">Option 1: Filter hello skewed key value in advance</span></span>

<span data-ttu-id="aebe3-119">Pokud nemá vliv na obchodní logiku, můžete předem filtrovat hello vyšší frekvence hodnot.</span><span class="sxs-lookup"><span data-stu-id="aebe3-119">If it does not affect your business logic, you can filter hello higher-frequency values in advance.</span></span> <span data-ttu-id="aebe3-120">Například pokud je celá řada 000 000 000 ve sloupci identifikátoru GUID, nemusí chcete tooaggregate tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want tooaggregate that value.</span></span> <span data-ttu-id="aebe3-121">Předtím, než agregační, můžete napsat "kde GUID! ="000 000 000"" toofilter hello vysoká frekvence hodnotu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” toofilter hello high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="aebe3-122">Možnost 2: Vyberte jiný klíč oddílu, nebo distribuce</span><span class="sxs-lookup"><span data-stu-id="aebe3-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="aebe3-123">V předchozím příkladu hello Pokud chcete pouze toocheck hello daň auditu zatížení všechny přes hello země, distribuci dat hello můžete zvýšit tak, že vyberete hello identifikační číslo jako klíč.</span><span class="sxs-lookup"><span data-stu-id="aebe3-123">In hello preceding example, if you want only toocheck hello tax-audit workload all over hello country, you can improve hello data distribution by selecting hello ID number as your key.</span></span> <span data-ttu-id="aebe3-124">Výběr jiný oddíl nebo distribučního klíče můžete někdy distribuovat hello dat více rovnoměrně, ale potřebujete toomake jistotu, že tato volba nemá vliv obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="aebe3-124">Picking a different partition or distribution key can sometimes distribute hello data more evenly, but you need toomake sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="aebe3-125">Například toocalculate hello daň součet pro každý stav, můžete toodesignate _stavu_ jako klíč oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="aebe3-125">For instance, toocalculate hello tax sum for each state, you might want toodesignate _State_ as hello partition key.</span></span> <span data-ttu-id="aebe3-126">Pokud budete pokračovat tooexperience tento problém, použijte možnost 3.</span><span class="sxs-lookup"><span data-stu-id="aebe3-126">If you continue tooexperience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="aebe3-127">Možnost 3: Přidejte další oddíl nebo distribučního klíče</span><span class="sxs-lookup"><span data-stu-id="aebe3-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="aebe3-128">Místo použití pouze _stavu_ jako klíč oddílu, můžete použít více než jeden klíč pro dělení.</span><span class="sxs-lookup"><span data-stu-id="aebe3-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="aebe3-129">Zvažte například přidávání _PSČ_ jako další oddíl klíče tooreduce data-partition velikosti a více rovnoměrně distribuovat hello data.</span><span class="sxs-lookup"><span data-stu-id="aebe3-129">For example, consider adding _ZIP Code_ as an additional partition key tooreduce data-partition sizes and distribute hello data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="aebe3-130">Možnost 4: Použití distribučních kruhového dotazování</span><span class="sxs-lookup"><span data-stu-id="aebe3-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="aebe3-131">Pokud nemůžete najít příslušný klíč pro oddíl a distribuci, můžete zkusit distribuční toouse kruhového dotazování.</span><span class="sxs-lookup"><span data-stu-id="aebe3-131">If you cannot find an appropriate key for partition and distribution, you can try toouse round-robin distribution.</span></span> <span data-ttu-id="aebe3-132">Kruhové dotazování distribuční zpracovává všechny řádky stejně a náhodně umístí je do odpovídajících intervalů.</span><span class="sxs-lookup"><span data-stu-id="aebe3-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="aebe3-133">Získá rovnoměrně Hello data, ale ztratí polohu informace, nevýhodou, který může také snížit výkon úlohy pro některé operace.</span><span class="sxs-lookup"><span data-stu-id="aebe3-133">hello data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="aebe3-134">Kromě toho agregace pro klíč zkreslilo hello Chcete přesto provést, se uchová hello data zkosení problém.</span><span class="sxs-lookup"><span data-stu-id="aebe3-134">Additionally, if you are doing aggregation for hello skewed key anyway, hello data-skew problem will persist.</span></span> <span data-ttu-id="aebe3-135">toolearn Další informace o distribuci kruhového dotazování, distribuce tabulky hello U-SQL najdete v článku tématu [CREATE TABLE (U-SQL): vytvoření tabulky se schématem](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="aebe3-135">toolearn more about round-robin distribution, see hello U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-hello-query-plan"></a><span data-ttu-id="aebe3-136">Řešení 2: Plán dotazu hello zlepšit</span><span class="sxs-lookup"><span data-stu-id="aebe3-136">Solution 2: Improve hello query plan</span></span>

### <a name="option-1-use-hello-create-statistics-statement"></a><span data-ttu-id="aebe3-137">Možnost 1: Příkaz CREATE STATISTICS hello použít</span><span class="sxs-lookup"><span data-stu-id="aebe3-137">Option 1: Use hello CREATE STATISTICS statement</span></span>

<span data-ttu-id="aebe3-138">U-SQL obsahuje příkaz CREATE STATISTICS hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="aebe3-138">U-SQL provides hello CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="aebe3-139">Tento příkaz dává Optimalizátor dotazů toohello Další informace o hello vlastností dat, třeba hodnotu rozdělení, které jsou uložené v tabulce.</span><span class="sxs-lookup"><span data-stu-id="aebe3-139">This statement gives more information toohello query optimizer about hello data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="aebe3-140">Pro většinu dotazů Optimalizátor dotazů hello již generuje hello nezbytné statistiku pro plán dotazu vysoké kvality.</span><span class="sxs-lookup"><span data-stu-id="aebe3-140">For most queries, hello query optimizer already generates hello necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="aebe3-141">V některých případech může být nutné tooimprove výkon dotazů, tak, že vytvoříte další statistiky s CREATE STATISTICS nebo změnou návrhu dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="aebe3-141">Occasionally, you might need tooimprove query performance by creating additional statistics with CREATE STATISTICS or by modifying hello query design.</span></span> <span data-ttu-id="aebe3-142">Další informace najdete v tématu hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) stránky.</span><span class="sxs-lookup"><span data-stu-id="aebe3-142">For more information, see hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="aebe3-143">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="aebe3-144">Statistické informace se automaticky neaktualizuje.</span><span class="sxs-lookup"><span data-stu-id="aebe3-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="aebe3-145">Pokud aktualizujete hello data v tabulce bez nutnosti znovu vytvářet hello statistiky, může odmítnout výkon dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="aebe3-145">If you update hello data in a table without re-creating hello statistics, hello query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="aebe3-146">Možnost 2: Použijte SKEWFACTOR</span><span class="sxs-lookup"><span data-stu-id="aebe3-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="aebe3-147">Pokud chcete toosum hello daň pro každý stav, je nutné použít Seskupit podle stavu, postup, který není vyhnout potížím data zkosení hello.</span><span class="sxs-lookup"><span data-stu-id="aebe3-147">If you want toosum hello tax for each state, you must use GROUP BY state, an approach that doesn't avoid hello data-skew problem.</span></span> <span data-ttu-id="aebe3-148">Však může poskytnout nápovědu dat ve vaší tooidentify dotazu, který data zkreslit v klíče, aby hello Optimalizátor můžete připravit plán spuštění za vás.</span><span class="sxs-lookup"><span data-stu-id="aebe3-148">However, you can provide a data hint in your query tooidentify data skew in keys so that hello optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="aebe3-149">Obvykle můžete nastavit parametr hello jako 0,5 a 1, s 0,5 znamená velkou zkosení nevěnuje význam zkosení a 1.</span><span class="sxs-lookup"><span data-stu-id="aebe3-149">Usually, you can set hello parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="aebe3-150">Protože pomocný parametr hello ovlivňuje plán spuštění optimalizace pro aktuální příkaz hello a všechny podřízené příkazy, před zda pomocný parametr tooadd hello hello potenciální nesouměrně rozdělí key-wise agregace.</span><span class="sxs-lookup"><span data-stu-id="aebe3-150">Because hello hint affects execution-plan optimization for hello current statement and all downstream statements, be sure tooadd hello hint before hello potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="aebe3-151">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-151">Code example:</span></span>

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

### <a name="option-3-use-rowcount"></a><span data-ttu-id="aebe3-152">Možnost 3: Použití počtu řádků</span><span class="sxs-lookup"><span data-stu-id="aebe3-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="aebe3-153">Kromě toho tooSKEWFACTOR pro konkrétního klíče nesouměrně rozdělí připojit případech, pokud víte, že hello ostatní sady řádků připojené k je malá, se dá zjistit hello Optimalizátor přidáním pomocný parametr počtu řádků v příkazu U-SQL hello před spojení.</span><span class="sxs-lookup"><span data-stu-id="aebe3-153">In addition tooSKEWFACTOR, for specific skewed-key join cases, if you know that hello other joined row set is small, you can tell hello optimizer by adding a ROWCOUNT hint in hello U-SQL statement before JOIN.</span></span> <span data-ttu-id="aebe3-154">Tímto způsobem Optimalizátor můžete vybrat všesměrového vysílání spojení strategie toohelp zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="aebe3-154">This way, optimizer can choose a broadcast join strategy toohelp improve performance.</span></span> <span data-ttu-id="aebe3-155">Uvědomte si, že atribut ROWCOUNT hello data zkosení problém nevyřeší, ale může nabídnout další pomoc.</span><span class="sxs-lookup"><span data-stu-id="aebe3-155">Be aware that ROWCOUNT does not resolve hello data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="aebe3-156">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information toodetermine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a><span data-ttu-id="aebe3-157">Řešení 3: Zlepšení hello uživatelem definované reduktorem a kombinační</span><span class="sxs-lookup"><span data-stu-id="aebe3-157">Solution 3: Improve hello user-defined reducer and combiner</span></span>

<span data-ttu-id="aebe3-158">Někdy může zapisovat toodeal uživatelem definovaný operátor s logikou složitý proces a kvalitně reduktorem a kombinační může zmírnit data zkosení problém v některých případech.</span><span class="sxs-lookup"><span data-stu-id="aebe3-158">You can sometimes write a user-defined operator toodeal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="aebe3-159">Možnost 1: Použití rekurzivní reduktorem, pokud je to možné</span><span class="sxs-lookup"><span data-stu-id="aebe3-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="aebe3-160">Ve výchozím nastavení uživatelem definované reduktorem běží v režimu tohoto nerekurzivního, což znamená, že snížit pracovní pro klíč je distribuován do jednoho vrcholu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="aebe3-161">Ale pokud vaše data se nesouměrně rozdělí, hello obrovských sad dat mohou být zpracovány v jednom vrchol a spustit po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-161">But if your data is skewed, hello huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="aebe3-162">tooimprove výkon, můžete přidat atribut ve vašem kódu toodefine reduktorem toorun v režimu rekurzivní.</span><span class="sxs-lookup"><span data-stu-id="aebe3-162">tooimprove performance, you can add an attribute in your code toodefine reducer toorun in recursive mode.</span></span> <span data-ttu-id="aebe3-163">Potom obrovských sad dat hello dají distribuované toomultiple vrcholy a spouštět souběžně, což urychlí úlohu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-163">Then, hello huge data sets can be distributed toomultiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="aebe3-164">toochange toorecursive reduktorem tohoto nerekurzivního, musíte se, že je vaše algoritmus asociativní toomake.</span><span class="sxs-lookup"><span data-stu-id="aebe3-164">toochange a non-recursive reducer toorecursive, you need toomake sure that your algorithm is associative.</span></span> <span data-ttu-id="aebe3-165">Například součet hello je asociativní a hello Medián není.</span><span class="sxs-lookup"><span data-stu-id="aebe3-165">For example, hello sum is associative, and hello median is not.</span></span> <span data-ttu-id="aebe3-166">Musíte taky toomake jistotu, že hello vstup a výstup reduktorem zachovat hello stejné schéma.</span><span class="sxs-lookup"><span data-stu-id="aebe3-166">You also need toomake sure that hello input and output for reducer keep hello same schema.</span></span>

<span data-ttu-id="aebe3-167">Atribut rekurzivní reduktorem:</span><span class="sxs-lookup"><span data-stu-id="aebe3-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="aebe3-168">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="aebe3-169">Možnost 2: Použití režimu kombinační na úrovni řádků, pokud je to možné</span><span class="sxs-lookup"><span data-stu-id="aebe3-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="aebe3-170">Podobně jako toohello ROWCOUNT nápovědu pro konkrétní připojení k nesouměrně rozdělí klíč případů, kombinační režimu pokusí toodistribute obrovské nesouměrně rozdělí klíč hodnota nastaví toomultiple vrcholy tak, aby pracovní hello mohou být provedeny souběžně.</span><span class="sxs-lookup"><span data-stu-id="aebe3-170">Similar toohello ROWCOUNT hint for specific skewed-key join cases, combiner mode tries toodistribute huge skewed-key value sets toomultiple vertices so that hello work can be executed concurrently.</span></span> <span data-ttu-id="aebe3-171">Režim kombinační nelze vyřešit problémy zkosení data, ale může nabídnout některé další nápovědu k nastaví velký nesouměrně rozdělí klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="aebe3-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="aebe3-172">Ve výchozím režimu kombinační hello je úplná, což znamená, že hello zbývajících sady řádků a nemůže být oddělené pravého řádku sady.</span><span class="sxs-lookup"><span data-stu-id="aebe3-172">By default, hello combiner mode is Full, which means that hello left row set and right row set cannot be separated.</span></span> <span data-ttu-id="aebe3-173">Nastavení režimu hello jako doleva nebo doprava nebo vnitřní umožňuje připojení k úrovni řádků.</span><span class="sxs-lookup"><span data-stu-id="aebe3-173">Setting hello mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="aebe3-174">systém Hello odděluje hello odpovídající řádek sady a distribuuje je do více vrcholy, které spustit souběžně.</span><span class="sxs-lookup"><span data-stu-id="aebe3-174">hello system separates hello corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="aebe3-175">Než začnete konfigurovat hello kombinační režimu, dávejte ale pozor, že je možné oddělit tooensure, který hello odpovídající sady řádků.</span><span class="sxs-lookup"><span data-stu-id="aebe3-175">However, before you configure hello combiner mode, be careful tooensure that hello corresponding row sets can be separated.</span></span>

<span data-ttu-id="aebe3-176">Příklad Hello zobrazuje sadu oddělených levého řádku.</span><span class="sxs-lookup"><span data-stu-id="aebe3-176">hello example that follows shows a separated left row set.</span></span> <span data-ttu-id="aebe3-177">Každý řádek výstupu závisí na jeden vstupní řádek zleva hello a potenciálně závisí na všechny řádky z hello přímo s hello stejnou hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="aebe3-177">Each output row depends on a single input row from hello left, and it potentially depends on all rows from hello right with hello same key value.</span></span> <span data-ttu-id="aebe3-178">Pokud nastavíte režim kombinační hello jako vlevo, hello systému odděluje hello obrovské doleva-sada řádků do malé a přiřadí je toomultiple vrcholy.</span><span class="sxs-lookup"><span data-stu-id="aebe3-178">If you set hello combiner mode as left, hello system separates hello huge left-row set into small ones and assigns them toomultiple vertices.</span></span>

![Obrázek kombinační režimu](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="aebe3-180">Pokud jste nastavili hello nesprávný kombinační režimu, kombinace hello sice méně efektivní a hello výsledky mohou být další potíže.</span><span class="sxs-lookup"><span data-stu-id="aebe3-180">If you set hello wrong combiner mode, hello combination is less efficient, and hello results might be wrong.</span></span>

<span data-ttu-id="aebe3-181">Atributy kombinační režimu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- <span data-ttu-id="aebe3-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Každý řádek výstupu závisí na jednom řádku vstupní zleva hello (a potenciálně všechny řádky z hello přímo s hello stejnou hodnotu klíče).</span><span class="sxs-lookup"><span data-stu-id="aebe3-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from hello left (and potentially all rows from hello right with hello same key value).</span></span>

- <span data-ttu-id="aebe3-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): každý řádek výstupu závisí na jeden vstupní řádek z hello vpravo (a potenciálně všechny řádky z levé hello s hello stejnou hodnotu klíče).</span><span class="sxs-lookup"><span data-stu-id="aebe3-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from hello right (and potentially all rows from hello left with hello same key value).</span></span>

- <span data-ttu-id="aebe3-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Každý řádek výstupu závisí na jeden vstupní řádek z hello vlevo a hello přímo s hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="aebe3-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from hello left and hello right with hello same value.</span></span>

<span data-ttu-id="aebe3-185">Příklad kódu:</span><span class="sxs-lookup"><span data-stu-id="aebe3-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }
