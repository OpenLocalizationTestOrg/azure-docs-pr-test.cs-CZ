---
title: "dotazy efektivní seznamu aaaDesign – Azure Batch | Microsoft Docs"
description: "Zvyšuje výkon filtrování vašich dotazů při žádosti o Batch prostředkům, jako jsou fondy, úlohy, úlohy a výpočetní uzly."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 08/02/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7e554119ec9d0e9e8007ccfb1ca80fe142a5e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-queries-toolist-batch-resources-efficiently"></a><span data-ttu-id="f3712-103">Vytváření dotazů toolist prostředky Batch efektivně</span><span class="sxs-lookup"><span data-stu-id="f3712-103">Create queries toolist Batch resources efficiently</span></span>

<span data-ttu-id="f3712-104">Zde dozvíte, jak tooincrease výkon aplikace Azure Batch snížením hello množství dat, která je vrácena službou hello po dotazu úlohy, úkoly a výpočetní uzly s hello [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="f3712-104">Here you'll learn how tooincrease your Azure Batch application's performance by reducing hello amount of data that is returned by hello service when you query jobs, tasks, and compute nodes with hello [Batch .NET][api_net] library.</span></span>

<span data-ttu-id="f3712-105">Téměř všechny aplikace Batch nutné tooperform nějaký typ monitorování nebo jiné operace, který se dotazuje služby Batch hello, často v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="f3712-105">Nearly all Batch applications need tooperform some type of monitoring or other operation that queries hello Batch service, often at regular intervals.</span></span> <span data-ttu-id="f3712-106">Toodetermine například, zda jsou všechny ve frontě úloh zbývající v rámci úlohy, musíte získat data na každý úkol v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-106">For example, toodetermine whether there are any queued tasks remaining in a job, you must get data on every task in hello job.</span></span> <span data-ttu-id="f3712-107">Stav hello toodetermine uzly ve fondu, musíte získat data na každý uzel ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-107">toodetermine hello status of nodes in your pool, you must get data on every node in hello pool.</span></span> <span data-ttu-id="f3712-108">Tento článek vysvětluje, jak tooexecute například dotazy v hello nejefektivnějším způsobem.</span><span class="sxs-lookup"><span data-stu-id="f3712-108">This article explains how tooexecute such queries in hello most efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="f3712-109">Hello služba Batch poskytuje speciální podporu rozhraní API pro běžné scénáře hello počítání úkoly v úloze.</span><span class="sxs-lookup"><span data-stu-id="f3712-109">hello Batch service provides special API support for hello common scenario of counting tasks in a job.</span></span> <span data-ttu-id="f3712-110">Místo použití seznamu dotazu pro tyto, můžete volat hello [získat počty úloh] [ rest_get_task_counts] operaci.</span><span class="sxs-lookup"><span data-stu-id="f3712-110">Instead of using a list query for these, you can call hello [Get Task Counts][rest_get_task_counts] operation.</span></span> <span data-ttu-id="f3712-111">Počet úloh Get Určuje, kolik úlohy čekají na vyřízení, spuštění nebo dokončení, a mít kolik úlohy byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="f3712-111">Get Task Counts indicates how many tasks are pending, running or complete, and how many tasks have succeeded or failed.</span></span> <span data-ttu-id="f3712-112">Spočítá počet úloh GET je efektivnější než seznamu dotazu.</span><span class="sxs-lookup"><span data-stu-id="f3712-112">Get Task Counts is more efficient than a list query.</span></span> <span data-ttu-id="f3712-113">Další informace najdete v tématu [počet úloh pro úlohu podle stavu (Preview)](batch-get-task-counts.md).</span><span class="sxs-lookup"><span data-stu-id="f3712-113">For more information, see [Count tasks for a job by state (Preview)](batch-get-task-counts.md).</span></span> 
>
> <span data-ttu-id="f3712-114">Hello získat počty úloh operaci starší než 2017-06-01.5.1 není k dispozici ve verzích služby Batch.</span><span class="sxs-lookup"><span data-stu-id="f3712-114">hello Get Task Counts operation is not available in Batch service versions earlier than 2017-06-01.5.1.</span></span> <span data-ttu-id="f3712-115">Pokud používáte starší verzi hello služby, potom použijte úlohy toocount dotaz seznam v úlohu.</span><span class="sxs-lookup"><span data-stu-id="f3712-115">If you are using an older version of hello service, then use a list query toocount tasks in a job instead.</span></span>
>
> 

## <a name="meet-hello-detaillevel"></a><span data-ttu-id="f3712-116">Splňovat hello DetailLevel</span><span class="sxs-lookup"><span data-stu-id="f3712-116">Meet hello DetailLevel</span></span>
<span data-ttu-id="f3712-117">V produkční aplikaci služby Batch můžete v hello tisíc čísel entity jako úlohy, úlohy a výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="f3712-117">In a production Batch application, entities like jobs, tasks, and compute nodes can number in hello thousands.</span></span> <span data-ttu-id="f3712-118">Pokud budete požadovat informace o těchto prostředků, potenciálně velkého množství dat musí "křížová přenosová hello" z aplikace tooyour služby Batch hello na každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="f3712-118">When you request information on these resources, a potentially large amount of data must "cross hello wire" from hello Batch service tooyour application on each query.</span></span> <span data-ttu-id="f3712-119">Omezením hello počet položek a typu informací, které je vrácených dotazem můžete zvýšit rychlost hello své dotazy a proto hello výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3712-119">By limiting hello number of items and type of information that is returned by a query, you can increase hello speed of your queries, and therefore hello performance of your application.</span></span>

<span data-ttu-id="f3712-120">To [Batch .NET] [ api_net] seznamy fragmentu kódu rozhraní API *každých* úlohu, která je spojená s úlohou, spolu s *všechny* hello vlastností jednotlivých úloha:</span><span class="sxs-lookup"><span data-stu-id="f3712-120">This [Batch .NET][api_net] API code snippet lists *every* task that is associated with a job, along with *all* of hello properties of each task:</span></span>

```csharp
// Get a collection of all of hello tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

<span data-ttu-id="f3712-121">Mnohem efektivnější seznamu dotazu, můžete však provést použitím tooyour dotazu "úroveň podrobností".</span><span class="sxs-lookup"><span data-stu-id="f3712-121">You can perform a much more efficient list query, however, by applying a "detail level" tooyour query.</span></span> <span data-ttu-id="f3712-122">To provedete zadáním [ODATADetailLevel] [ odata] objektu toohello [JobOperations.ListTasks] [ net_list_tasks] metoda.</span><span class="sxs-lookup"><span data-stu-id="f3712-122">You do this by supplying an [ODATADetailLevel][odata] object toohello [JobOperations.ListTasks][net_list_tasks] method.</span></span> <span data-ttu-id="f3712-123">Tento fragment kódu vrátí pouze hello ID, příkazový řádek a výpočetní uzel informace vlastnosti dokončených úloh:</span><span class="sxs-lookup"><span data-stu-id="f3712-123">This snippet returns only hello ID, command line, and compute node information properties of completed tasks:</span></span>

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties tooreturn
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply hello ODATADetailLevel toohello ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

<span data-ttu-id="f3712-124">V tomto ukázkovém scénáři, pokud existují tisíce úkolů v úloze hello hello výsledky z dotazu druhý hello obvykle bude vrácena jako mnohem rychlejší než hello první.</span><span class="sxs-lookup"><span data-stu-id="f3712-124">In this example scenario, if there are thousands of tasks in hello job, hello results from hello second query will typically be returned much quicker than hello first.</span></span> <span data-ttu-id="f3712-125">Další informace o používání ODATADetailLevel při seznamu položek s hello Batch .NET API je součástí [pod](#efficient-querying-in-batch-net).</span><span class="sxs-lookup"><span data-stu-id="f3712-125">More information about using ODATADetailLevel when you list items with hello Batch .NET API is included [below](#efficient-querying-in-batch-net).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3712-126">Důrazně doporučujeme, aby vám *vždy* dodávky seznam ODATADetailLevel tooyour objektů .NET API volá tooensure maximální efektivity a výkonu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3712-126">We highly recommend that you *always* supply an ODATADetailLevel object tooyour .NET API list calls tooensure maximum efficiency and performance of your application.</span></span> <span data-ttu-id="f3712-127">Zadáním úroveň podrobností můžete pomoct toolower dávky služby odezvy, zvýšit využití sítě a minimalizovat využití paměti v klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="f3712-127">By specifying a detail level, you can help toolower Batch service response times, improve network utilization, and minimize memory usage by client applications.</span></span>
> 
> 

## <a name="filter-select-and-expand"></a><span data-ttu-id="f3712-128">Filtrovat, vyberte a rozbalte</span><span class="sxs-lookup"><span data-stu-id="f3712-128">Filter, select, and expand</span></span>
<span data-ttu-id="f3712-129">Hello [Batch .NET] [ api_net] a [Batch REST] [ api_rest] rozhraní API nabízejí možnost tooreduce hello obou hello počet položek, které jsou vráceny v seznamu, a také hello objem informací, které se vrátí pro každou.</span><span class="sxs-lookup"><span data-stu-id="f3712-129">hello [Batch .NET][api_net] and [Batch REST][api_rest] APIs provide hello ability tooreduce both hello number of items that are returned in a list, as well as hello amount of information that is returned for each.</span></span> <span data-ttu-id="f3712-130">To uděláte tak, že zadáte **filtru**, **vyberte**, a **rozbalte řetězce** při provádění dotazů seznamu.</span><span class="sxs-lookup"><span data-stu-id="f3712-130">You do so by specifying **filter**, **select**, and **expand strings** when performing list queries.</span></span>

### <a name="filter"></a><span data-ttu-id="f3712-131">Filtr</span><span class="sxs-lookup"><span data-stu-id="f3712-131">Filter</span></span>
<span data-ttu-id="f3712-132">řetězec filtru Hello je výraz, který snižuje hello počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="f3712-132">hello filter string is an expression that reduces hello number of items that are returned.</span></span> <span data-ttu-id="f3712-133">Například seznam pouze hello spuštěných úloh pro úlohu nebo seznamu pouze výpočetní uzly, které jsou připravené toorun úlohy.</span><span class="sxs-lookup"><span data-stu-id="f3712-133">For example, list only hello running tasks for a job, or list only compute nodes that are ready toorun tasks.</span></span>

* <span data-ttu-id="f3712-134">řetězec filtru Hello se skládá z jednoho nebo více výrazů s výrazem, který obsahuje název vlastnosti, operátor a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f3712-134">hello filter string consists of one or more expressions, with an expression that consists of a property name, operator, and value.</span></span> <span data-ttu-id="f3712-135">Hello vlastnosti, které lze zadat jsou konkrétní tooeach typu entity, který dotazování, jako jsou hello operátory, které jsou podporovány pro každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f3712-135">hello properties that can be specified are specific tooeach entity type that you query, as are hello operators that are supported for each property.</span></span>
* <span data-ttu-id="f3712-136">Více výrazů lze spojovat pomocí logických operátorů hello `and` a `or`.</span><span class="sxs-lookup"><span data-stu-id="f3712-136">Multiple expressions can be combined by using hello logical operators `and` and `or`.</span></span>
* <span data-ttu-id="f3712-137">Tento příklad filtrování seznamů řetězec jen hello spuštění úlohy "vykreslení": `(state eq 'running') and startswith(id, 'renderTask')`.</span><span class="sxs-lookup"><span data-stu-id="f3712-137">This example filter string lists only hello running "render" tasks: `(state eq 'running') and startswith(id, 'renderTask')`.</span></span>

### <a name="select"></a><span data-ttu-id="f3712-138">Vyberte</span><span class="sxs-lookup"><span data-stu-id="f3712-138">Select</span></span>
<span data-ttu-id="f3712-139">Vyberte řetězec Hello omezuje hello hodnoty vlastností, které se vrátí pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="f3712-139">hello select string limits hello property values that are returned for each item.</span></span> <span data-ttu-id="f3712-140">Zadejte seznam názvů vlastností a pouze hodnoty těchto vlastností jsou vráceny hello položek ve výsledcích dotazů hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-140">You specify a list of property names, and only those property values are returned for hello items in hello query results.</span></span>

* <span data-ttu-id="f3712-141">Vyberte řetězec Hello se skládá z seznam názvů vlastností oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="f3712-141">hello select string consists of a comma-separated list of property names.</span></span> <span data-ttu-id="f3712-142">Můžete zadat jakýkoli hello vlastnosti pro typ entity hello, který se dotazuje.</span><span class="sxs-lookup"><span data-stu-id="f3712-142">You can specify any of hello properties for hello entity type you are querying.</span></span>
* <span data-ttu-id="f3712-143">Tento příklad vyberte řetězec Určuje, že má být vrácen pouze tři hodnoty vlastností pro každou úlohu: `id, state, stateTransitionTime`.</span><span class="sxs-lookup"><span data-stu-id="f3712-143">This example select string specifies that only three property values should be returned for each task: `id, state, stateTransitionTime`.</span></span>

### <a name="expand"></a><span data-ttu-id="f3712-144">Rozbalit</span><span class="sxs-lookup"><span data-stu-id="f3712-144">Expand</span></span>
<span data-ttu-id="f3712-145">Hello rozbalte položku řetězec snižuje hello počet volání rozhraní API, které jsou požadované tooobtain určité informace.</span><span class="sxs-lookup"><span data-stu-id="f3712-145">hello expand string reduces hello number of API calls that are required tooobtain certain information.</span></span> <span data-ttu-id="f3712-146">Použijete-li řetězec rozbalte, nelze získat další informace o jednotlivých položkách na základě jednoho volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f3712-146">When you use an expand string, more information about each item can be obtained with a single API call.</span></span> <span data-ttu-id="f3712-147">Místo první získání hello seznamu entity, pak se žádajícího informace pro každou položku v seznamu hello využít řetězec rozbalte tooobtain hello stejné informace v jediném volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f3712-147">Rather than first obtaining hello list of entities, then requesting information for each item in hello list, you use an expand string tooobtain hello same information in a single API call.</span></span> <span data-ttu-id="f3712-148">Menší volání rozhraní API znamená vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="f3712-148">Less API calls means better performance.</span></span>

* <span data-ttu-id="f3712-149">Podobné toohello vyberte řetězec, hello rozbalte řetězec ovládací prvky, jestli některá data je zahrnuta v seznamu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="f3712-149">Similar toohello select string, hello expand string controls whether certain data is included in list query results.</span></span>
* <span data-ttu-id="f3712-150">Hello rozbalte položku řetězec je podporována pouze v případě, že se používá v seznam úloh, plány úloh, úlohy a fondy.</span><span class="sxs-lookup"><span data-stu-id="f3712-150">hello expand string is only supported when it is used in listing jobs, job schedules, tasks, and pools.</span></span> <span data-ttu-id="f3712-151">V současné době podporuje pouze statistické informace.</span><span class="sxs-lookup"><span data-stu-id="f3712-151">Currently, it only supports statistics information.</span></span>
* <span data-ttu-id="f3712-152">Když je zadán žádný vyberte řetězec vyžadují se všechny vlastnosti, rozbalte hello řetězec *musí* být použité tooget statistické informace.</span><span class="sxs-lookup"><span data-stu-id="f3712-152">When all properties are required and no select string is specified, hello expand string *must* be used tooget statistics information.</span></span> <span data-ttu-id="f3712-153">Pokud vyberte řetězec je použité tooobtain podmnožinu vlastností `stats` lze zadat v řetězci hello vyberte a rozbalte hello řetězec nemusí toobe zadán.</span><span class="sxs-lookup"><span data-stu-id="f3712-153">If a select string is used tooobtain a subset of properties, then `stats` can be specified in hello select string, and hello expand string does not need toobe specified.</span></span>
* <span data-ttu-id="f3712-154">Tento příklad rozbalte položku řetězec Určuje, zda má být vrácen statistické informace pro každou položku v seznamu hello: `stats`.</span><span class="sxs-lookup"><span data-stu-id="f3712-154">This example expand string specifies that statistics information should be returned for each item in hello list: `stats`.</span></span>

> [!NOTE]
> <span data-ttu-id="f3712-155">Při vytváření žádné hello tři typy řetězce dotazů (filtrování, vyberte a rozbalte možnost), musí zajistěte, aby názvy vlastností hello a případ odpovídaly s jejich protějšky element REST API.</span><span class="sxs-lookup"><span data-stu-id="f3712-155">When constructing any of hello three query string types (filter, select, and expand), you must ensure that hello property names and case match that of their REST API element counterparts.</span></span> <span data-ttu-id="f3712-156">Například při práci s hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) třídy, je nutné zadat **stavu** místo **stavu**, i když hello vlastnost .NET [ CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span><span class="sxs-lookup"><span data-stu-id="f3712-156">For example, when working with hello .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) class, you must specify **state** instead of **State**, even though hello .NET property is [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span></span> <span data-ttu-id="f3712-157">Najdete v části tabulky hello pod pro vlastnost mapování mezi hello .NET a REST API.</span><span class="sxs-lookup"><span data-stu-id="f3712-157">See hello tables below for property mappings between hello .NET and REST APIs.</span></span>
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a><span data-ttu-id="f3712-158">Pravidla pro filtr, vyberte a rozbalte řetězce</span><span class="sxs-lookup"><span data-stu-id="f3712-158">Rules for filter, select, and expand strings</span></span>
* <span data-ttu-id="f3712-159">Názvy vlastností ve filtru, vyberte a rozbalte řetězce by se měla zobrazit jako ve hello [Batch REST] [ api_rest] rozhraní API – i v případě, že používáte [Batch .NET] [ api_net] nebo jeden z hello jiných SDK služby Batch.</span><span class="sxs-lookup"><span data-stu-id="f3712-159">Properties names in filter, select, and expand strings should appear as they do in hello [Batch REST][api_rest] API--even when you use [Batch .NET][api_net] or one of hello other Batch SDKs.</span></span>
* <span data-ttu-id="f3712-160">Všechny názvy vlastností malá a velká písmena, ale hodnoty vlastností se malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="f3712-160">All property names are case-sensitive, but property values are case insensitive.</span></span>
* <span data-ttu-id="f3712-161">Datum a čas řetězců může mít jednu ze dvou formátů a musí předcházet `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="f3712-161">Date/time strings can be one of two formats, and must be preceded with `DateTime`.</span></span>
  
  * <span data-ttu-id="f3712-162">Příklad formátu W3C DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`</span><span class="sxs-lookup"><span data-stu-id="f3712-162">W3C-DTF format example: `creationTime gt DateTime'2011-05-08T08:49:37Z'`</span></span>
  * <span data-ttu-id="f3712-163">Příklad formátu RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span><span class="sxs-lookup"><span data-stu-id="f3712-163">RFC 1123 format example: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span></span>
* <span data-ttu-id="f3712-164">Logická hodnota řetězce jsou buď `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="f3712-164">Boolean strings are either `true` or `false`.</span></span>
* <span data-ttu-id="f3712-165">Pokud je zadána neplatná vlastnost nebo operátor, `400 (Bad Request)` bude výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="f3712-165">If an invalid property or operator is specified, a `400 (Bad Request)` error will result.</span></span>

## <a name="efficient-querying-in-batch-net"></a><span data-ttu-id="f3712-166">Efektivní dotazování v Batch .NET</span><span class="sxs-lookup"><span data-stu-id="f3712-166">Efficient querying in Batch .NET</span></span>
<span data-ttu-id="f3712-167">V rámci hello [Batch .NET] [ api_net] rozhraní API, hello [ODATADetailLevel] [ odata] třída se používá pro zadávání filtru, vyberte a rozbalte řetězce toolist operace.</span><span class="sxs-lookup"><span data-stu-id="f3712-167">Within hello [Batch .NET][api_net] API, hello [ODATADetailLevel][odata] class is used for supplying filter, select, and expand strings toolist operations.</span></span> <span data-ttu-id="f3712-168">Hello ODataDetailLevel třída má tři vlastnosti veřejné řetězce, které lze zadat v konstruktoru hello nebo nastavit přímo na objekt hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-168">hello ODataDetailLevel class has three public string properties that can be specified in hello constructor, or set directly on hello object.</span></span> <span data-ttu-id="f3712-169">Pak předáte hello ODataDetailLevel objekt jako parametr toohello různých seznam operací, jako [ListPools][net_list_pools], [ListJobs][net_list_jobs], a [ListTasks][net_list_tasks].</span><span class="sxs-lookup"><span data-stu-id="f3712-169">You then pass hello ODataDetailLevel object as a parameter toohello various list operations such as [ListPools][net_list_pools], [ListJobs][net_list_jobs], and [ListTasks][net_list_tasks].</span></span>

* <span data-ttu-id="f3712-170">[ODATADetailLevel][odata].[ FilterClause][odata_filter]: omezit hello počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="f3712-170">[ODATADetailLevel][odata].[FilterClause][odata_filter]: Limit hello number of items that are returned.</span></span>
* <span data-ttu-id="f3712-171">[ODATADetailLevel][odata].[ SelectClause][odata_select]: Zadejte každou položku jsou vráceny hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="f3712-171">[ODATADetailLevel][odata].[SelectClause][odata_select]: Specify which property values are returned with each item.</span></span>
* <span data-ttu-id="f3712-172">[ODATADetailLevel][odata].[ ExpandClause][odata_expand]: načtení dat pro všechny položky v jediného rozhraní API volat místo samostatné volání pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="f3712-172">[ODATADetailLevel][odata].[ExpandClause][odata_expand]: Retrieve data for all items in a single API call instead of separate calls for each item.</span></span>

<span data-ttu-id="f3712-173">Hello následující fragment kódu používá hello Batch .NET API tooefficiently dotazu hello Batch službu pro hello statistiky konkrétní sadu fondů.</span><span class="sxs-lookup"><span data-stu-id="f3712-173">hello following code snippet uses hello Batch .NET API tooefficiently query hello Batch service for hello statistics of a specific set of pools.</span></span> <span data-ttu-id="f3712-174">V tomto scénáři má uživatel Batch hello testovací a produkční fondů.</span><span class="sxs-lookup"><span data-stu-id="f3712-174">In this scenario, hello Batch user has both test and production pools.</span></span> <span data-ttu-id="f3712-175">Hello testovací fondu ID mají předponu "test" a hello produkční fondu ID mají předponu "produkčnímu".</span><span class="sxs-lookup"><span data-stu-id="f3712-175">hello test pool IDs are prefixed with "test", and hello production pool IDs are prefixed with "prod".</span></span> <span data-ttu-id="f3712-176">Ve fragmentu hello *myBatchClient* je správně inicializována instanci hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) třídy.</span><span class="sxs-lookup"><span data-stu-id="f3712-176">In hello snippet, *myBatchClient* is a properly initialized instance of hello [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) class.</span></span>

```csharp
// First we need an ODATADetailLevel instance on which tooset hello filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want toopull only hello "test" pools, so we limit hello number of items returned
// by using a FilterClause and specifying that hello pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// toofurther limit hello data that crosses hello wire, configure hello SelectClause to
// limit hello properties that are returned on each CloudPool object tooonly
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify hello ExpandClause so that hello .NET API pulls hello statistics for the
// CloudPools in a single underlying REST API call. Note that we use hello pool's
// REST API element name "stats" here as opposed too"Statistics" as it appears in
// hello .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing hello amount of data that is returned
// by specifying hello detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> <span data-ttu-id="f3712-177">Instance [ODATADetailLevel] [ odata] nakonfigurovaný s vyberte a rozbalte klauzule lze také předat tooappropriate metody Get, jako například [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) , toolimit hello množství dat, která je vrácena.</span><span class="sxs-lookup"><span data-stu-id="f3712-177">An instance of [ODATADetailLevel][odata] that is configured with Select and Expand clauses can also be passed tooappropriate Get methods, such as [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), toolimit hello amount of data that is returned.</span></span>
> 
> 

## <a name="batch-rest-toonet-api-mappings"></a><span data-ttu-id="f3712-178">Batch REST too.NET API mapování</span><span class="sxs-lookup"><span data-stu-id="f3712-178">Batch REST too.NET API mappings</span></span>
<span data-ttu-id="f3712-179">Názvy vlastností ve filtru, vyberte a rozbalte řetězce *musí* odráží jejich protějšky REST API, v názvu i případu.</span><span class="sxs-lookup"><span data-stu-id="f3712-179">Property names in filter, select, and expand strings *must* reflect their REST API counterparts, both in name and case.</span></span> <span data-ttu-id="f3712-180">Následující tabulky Hello zadejte mapování mezi hello .NET a REST API protějšky.</span><span class="sxs-lookup"><span data-stu-id="f3712-180">hello tables below provide mappings between hello .NET and REST API counterparts.</span></span>

### <a name="mappings-for-filter-strings"></a><span data-ttu-id="f3712-181">Mapování pro filtr řetězce</span><span class="sxs-lookup"><span data-stu-id="f3712-181">Mappings for filter strings</span></span>
* <span data-ttu-id="f3712-182">**Seznam metod rozhraní .NET**: každá z metod .NET API hello v tomto sloupci přijímá [ODATADetailLevel] [ odata] objekt jako parametr.</span><span class="sxs-lookup"><span data-stu-id="f3712-182">**.NET list methods**: Each of hello .NET API methods in this column accepts an [ODATADetailLevel][odata] object as a parameter.</span></span>
* <span data-ttu-id="f3712-183">**REST seznamu požadavků**: každý REST API stránky propojené tooin tento sloupec obsahuje tabulku, která určuje vlastnosti hello a operace, které jsou povoleny v *filtru* řetězce.</span><span class="sxs-lookup"><span data-stu-id="f3712-183">**REST list requests**: Each REST API page linked tooin this column contains a table that specifies hello properties and operations that are allowed in *filter* strings.</span></span> <span data-ttu-id="f3712-184">Když vytvoříte budou používat tyto operace a názvy vlastností [ODATADetailLevel.FilterClause] [ odata_filter] řetězec.</span><span class="sxs-lookup"><span data-stu-id="f3712-184">You will use these property names and operations when you construct an [ODATADetailLevel.FilterClause][odata_filter] string.</span></span>

| <span data-ttu-id="f3712-185">Seznam metod rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f3712-185">.NET list methods</span></span> | <span data-ttu-id="f3712-186">REST seznamu požadavků</span><span class="sxs-lookup"><span data-stu-id="f3712-186">REST list requests</span></span> |
| --- | --- |
| <span data-ttu-id="f3712-187">[CertificateOperations.ListCertificates][net_list_certs]</span><span class="sxs-lookup"><span data-stu-id="f3712-187">[CertificateOperations.ListCertificates][net_list_certs]</span></span> |<span data-ttu-id="f3712-188">[Seznam certifikátů hello na účtu][rest_list_certs]</span><span class="sxs-lookup"><span data-stu-id="f3712-188">[List hello certificates in an account][rest_list_certs]</span></span> |
| <span data-ttu-id="f3712-189">[CloudTask.ListNodeFiles][net_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="f3712-189">[CloudTask.ListNodeFiles][net_list_task_files]</span></span> |<span data-ttu-id="f3712-190">[Seznam hello soubory spojené s úlohami][rest_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="f3712-190">[List hello files associated with a task][rest_list_task_files]</span></span> |
| <span data-ttu-id="f3712-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="f3712-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span></span> |<span data-ttu-id="f3712-192">[Stav seznamu hello hello úlohy přípravy a uvolnění úloh pro úlohu][rest_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="f3712-192">[List hello status of hello job preparation and job release tasks for a job][rest_list_jobprep_status]</span></span> |
| <span data-ttu-id="f3712-193">[JobOperations.ListJobs][net_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="f3712-193">[JobOperations.ListJobs][net_list_jobs]</span></span> |<span data-ttu-id="f3712-194">[Seznam úloh hello v účtu][rest_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="f3712-194">[List hello jobs in an account][rest_list_jobs]</span></span> |
| <span data-ttu-id="f3712-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="f3712-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span></span> |<span data-ttu-id="f3712-196">[Seznam souborů hello na uzlu][rest_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="f3712-196">[List hello files on a node][rest_list_nodefiles]</span></span> |
| <span data-ttu-id="f3712-197">[JobOperations.ListTasks][net_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="f3712-197">[JobOperations.ListTasks][net_list_tasks]</span></span> |<span data-ttu-id="f3712-198">[Seznam hello úkoly spojené s úlohou][rest_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="f3712-198">[List hello tasks associated with a job][rest_list_tasks]</span></span> |
| <span data-ttu-id="f3712-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="f3712-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span></span> |<span data-ttu-id="f3712-200">[Plány úloh hello seznamu v účtu][rest_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="f3712-200">[List hello job schedules in an account][rest_list_job_schedules]</span></span> |
| <span data-ttu-id="f3712-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="f3712-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span></span> |<span data-ttu-id="f3712-202">[Seznam hello úlohy přidružené k plánu úlohy][rest_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="f3712-202">[List hello jobs associated with a job schedule][rest_list_schedule_jobs]</span></span> |
| <span data-ttu-id="f3712-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="f3712-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span></span> |<span data-ttu-id="f3712-204">[Seznam hello výpočetních uzlů ve fondu][rest_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="f3712-204">[List hello compute nodes in a pool][rest_list_compute_nodes]</span></span> |
| <span data-ttu-id="f3712-205">[PoolOperations.ListPools][net_list_pools]</span><span class="sxs-lookup"><span data-stu-id="f3712-205">[PoolOperations.ListPools][net_list_pools]</span></span> |<span data-ttu-id="f3712-206">[Seznam hello fondy v účtu][rest_list_pools]</span><span class="sxs-lookup"><span data-stu-id="f3712-206">[List hello pools in an account][rest_list_pools]</span></span> |

### <a name="mappings-for-select-strings"></a><span data-ttu-id="f3712-207">Mapování pro vyberte řetězce</span><span class="sxs-lookup"><span data-stu-id="f3712-207">Mappings for select strings</span></span>
* <span data-ttu-id="f3712-208">**Batch .NET typy**: typy Batch .NET API.</span><span class="sxs-lookup"><span data-stu-id="f3712-208">**Batch .NET types**: Batch .NET API types.</span></span>
* <span data-ttu-id="f3712-209">**Rozhraní REST API entity**: obsahuje jednu nebo více tabulek, které seznam názvů vlastností hello REST API pro typ hello každé stránce v tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="f3712-209">**REST API entities**: Each page in this column contains one or more tables that list hello REST API property names for hello type.</span></span> <span data-ttu-id="f3712-210">Tyto názvy vlastností se používají při vytváření *vyberte* řetězce.</span><span class="sxs-lookup"><span data-stu-id="f3712-210">These property names are used when you construct *select* strings.</span></span> <span data-ttu-id="f3712-211">Tyto stejné názvy vlastností použijete při vytváření [ODATADetailLevel.SelectClause] [ odata_select] řetězec.</span><span class="sxs-lookup"><span data-stu-id="f3712-211">You will use these same property names when you construct an [ODATADetailLevel.SelectClause][odata_select] string.</span></span>

| <span data-ttu-id="f3712-212">Typy batch .NET</span><span class="sxs-lookup"><span data-stu-id="f3712-212">Batch .NET types</span></span> | <span data-ttu-id="f3712-213">Rozhraní REST API entity</span><span class="sxs-lookup"><span data-stu-id="f3712-213">REST API entities</span></span> |
| --- | --- |
| <span data-ttu-id="f3712-214">[Certifikát][net_cert]</span><span class="sxs-lookup"><span data-stu-id="f3712-214">[Certificate][net_cert]</span></span> |<span data-ttu-id="f3712-215">[Získání informací o certifikát][rest_get_cert]</span><span class="sxs-lookup"><span data-stu-id="f3712-215">[Get information about a certificate][rest_get_cert]</span></span> |
| <span data-ttu-id="f3712-216">[CloudJob][net_job]</span><span class="sxs-lookup"><span data-stu-id="f3712-216">[CloudJob][net_job]</span></span> |<span data-ttu-id="f3712-217">[Získání informací o úlohy][rest_get_job]</span><span class="sxs-lookup"><span data-stu-id="f3712-217">[Get information about a job][rest_get_job]</span></span> |
| <span data-ttu-id="f3712-218">[CloudJobSchedule][net_schedule]</span><span class="sxs-lookup"><span data-stu-id="f3712-218">[CloudJobSchedule][net_schedule]</span></span> |<span data-ttu-id="f3712-219">[Získat informace o plánu úlohy][rest_get_schedule]</span><span class="sxs-lookup"><span data-stu-id="f3712-219">[Get information about a job schedule][rest_get_schedule]</span></span> |
| <span data-ttu-id="f3712-220">[ComputeNode][net_node]</span><span class="sxs-lookup"><span data-stu-id="f3712-220">[ComputeNode][net_node]</span></span> |<span data-ttu-id="f3712-221">[Získání informací o uzlu][rest_get_node]</span><span class="sxs-lookup"><span data-stu-id="f3712-221">[Get information about a node][rest_get_node]</span></span> |
| <span data-ttu-id="f3712-222">[CloudPool][net_pool]</span><span class="sxs-lookup"><span data-stu-id="f3712-222">[CloudPool][net_pool]</span></span> |<span data-ttu-id="f3712-223">[Získat informace o fondu][rest_get_pool]</span><span class="sxs-lookup"><span data-stu-id="f3712-223">[Get information about a pool][rest_get_pool]</span></span> |
| <span data-ttu-id="f3712-224">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="f3712-224">[CloudTask][net_task]</span></span> |<span data-ttu-id="f3712-225">[Získat informace o úkolu][rest_get_task]</span><span class="sxs-lookup"><span data-stu-id="f3712-225">[Get information about a task][rest_get_task]</span></span> |

## <a name="example-construct-a-filter-string"></a><span data-ttu-id="f3712-226">Příklad: vytvoření řetězec filtru</span><span class="sxs-lookup"><span data-stu-id="f3712-226">Example: construct a filter string</span></span>
<span data-ttu-id="f3712-227">Když vytvoříte řetězec filtru pro [ODATADetailLevel.FilterClause][odata_filter], naleznete v tabulce hello výše v části "Mapování pro filtr řetězce" toofind hello REST API dokumentace stránky, která odpovídá operace seznamu toohello chcete tooperform.</span><span class="sxs-lookup"><span data-stu-id="f3712-227">When you construct a filter string for [ODATADetailLevel.FilterClause][odata_filter], consult hello table above under "Mappings for filter strings" toofind hello REST API documentation page that corresponds toohello list operation that you wish tooperform.</span></span> <span data-ttu-id="f3712-228">V první více řádků tabulky hello na této stránce najdete hello filtrování vlastností a jejich podporované operátory.</span><span class="sxs-lookup"><span data-stu-id="f3712-228">You will find hello filterable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="f3712-229">Pokud chcete tooretrieve všechny úlohy, jejichž ukončovací kód procesu je nenulové hodnoty, například tento řádek na [seznamu hello úkoly spojené s úlohou] [ rest_list_tasks] určuje povolené operátory a řetězec příslušné vlastnosti hello:</span><span class="sxs-lookup"><span data-stu-id="f3712-229">If you wish tooretrieve all tasks whose exit code was nonzero, for example, this row on [List hello tasks associated with a job][rest_list_tasks] specifies hello applicable property string and allowable operators:</span></span>

| <span data-ttu-id="f3712-230">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f3712-230">Property</span></span> | <span data-ttu-id="f3712-231">Povolené operace</span><span class="sxs-lookup"><span data-stu-id="f3712-231">Operations allowed</span></span> | <span data-ttu-id="f3712-232">Typ</span><span class="sxs-lookup"><span data-stu-id="f3712-232">Type</span></span> |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

<span data-ttu-id="f3712-233">Proto by být hello řetězec filtru pro výpis všech úloh s nenulový ukončovací kód:</span><span class="sxs-lookup"><span data-stu-id="f3712-233">Thus, hello filter string for listing all tasks with a nonzero exit code would be:</span></span>

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a><span data-ttu-id="f3712-234">Příklad: sestavit vyberte řetězec</span><span class="sxs-lookup"><span data-stu-id="f3712-234">Example: construct a select string</span></span>
<span data-ttu-id="f3712-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], projděte si hello tabulce výše v části "Mapování pro vyberte řetězce" a přejděte na stránku toohello REST API, která odpovídá toohello typ entity, které vytváření seznamu.</span><span class="sxs-lookup"><span data-stu-id="f3712-235">tooconstruct [ODATADetailLevel.SelectClause][odata_select], consult hello table above under "Mappings for select strings" and navigate toohello REST API page that corresponds toohello type of entity that you are listing.</span></span> <span data-ttu-id="f3712-236">V první více řádků tabulky hello na této stránce najdete hello volitelný vlastností a jejich podporované operátory.</span><span class="sxs-lookup"><span data-stu-id="f3712-236">You will find hello selectable properties and their supported operators in hello first multirow table on that page.</span></span> <span data-ttu-id="f3712-237">Pokud chcete pouze ID hello tooretrieve a příkazový řádek pro každý úkol v seznamu, například zjistíte, tyto řádky v tabulce použít hello na [získat informace o úkolu][rest_get_task]:</span><span class="sxs-lookup"><span data-stu-id="f3712-237">If you wish tooretrieve only hello ID and command line for each task in a list, for example, you will find these rows in hello applicable table on [Get information about a task][rest_get_task]:</span></span>

| <span data-ttu-id="f3712-238">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f3712-238">Property</span></span> | <span data-ttu-id="f3712-239">Typ</span><span class="sxs-lookup"><span data-stu-id="f3712-239">Type</span></span> | <span data-ttu-id="f3712-240">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f3712-240">Notes</span></span> |
|:--- |:--- |:--- |
| `id` |`String` |`hello ID of hello task.` |
| `commandLine` |`String` |`hello command line of hello task.` |

<span data-ttu-id="f3712-241">poté budou vyberte řetězec Hello, včetně pouze hello ID a příkazového řádku u jednotlivých uvedených úkolů:</span><span class="sxs-lookup"><span data-stu-id="f3712-241">hello select string for including only hello ID and command line with each listed task would then be:</span></span>

`id, commandLine`

## <a name="code-samples"></a><span data-ttu-id="f3712-242">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="f3712-242">Code samples</span></span>
### <a name="efficient-list-queries-code-sample"></a><span data-ttu-id="f3712-243">Ukázka kódu dotazy efektivní seznamu</span><span class="sxs-lookup"><span data-stu-id="f3712-243">Efficient list queries code sample</span></span>
<span data-ttu-id="f3712-244">Podívejte se na hello [EfficientListQueries] [ efficient_query_sample] ukázkového projektu na Githubu toosee jak efektivní dotazování na seznamu může ovlivnit výkon v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3712-244">Check out hello [EfficientListQueries][efficient_query_sample] sample project on GitHub toosee how efficient list querying can affect performance in an application.</span></span> <span data-ttu-id="f3712-245">Tato Konzolová aplikace C# vytvoří a přidá velký počet úloh tooa úlohy.</span><span class="sxs-lookup"><span data-stu-id="f3712-245">This C# console application creates and adds a large number of tasks tooa job.</span></span> <span data-ttu-id="f3712-246">Pak umožňuje více volání toohello [JobOperations.ListTasks] [ net_list_tasks] metoda a předává [ODATADetailLevel] [ odata] objekty, které jsou nakonfigurován s jinou vlastnost hodnoty toovary hello množství dat toobe vrátila.</span><span class="sxs-lookup"><span data-stu-id="f3712-246">Then, it makes multiple calls toohello [JobOperations.ListTasks][net_list_tasks] method and passes [ODATADetailLevel][odata] objects that are configured with different property values toovary hello amount of data toobe returned.</span></span> <span data-ttu-id="f3712-247">Vyvolá podobné toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f3712-247">It produces output similar toohello following:</span></span>

```
Adding 5000 tasks toojob jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER tooquery tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER toocontinue...
```

<span data-ttu-id="f3712-248">Jak je znázorněno v hello procentuálně dobu, můžete výrazně snížit dobu odezvy na dotazy omezením hello vlastnosti a hello počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="f3712-248">As shown in hello elapsed times, you can greatly lower query response times by limiting hello properties and hello number of items that are returned.</span></span> <span data-ttu-id="f3712-249">Tato a Další ukázkové projekty můžete najít v hello [azure-batch-samples] [ github_samples] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f3712-249">You can find this and other sample projects in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span>

### <a name="batchmetrics-library-and-code-sample"></a><span data-ttu-id="f3712-250">Ukázka BatchMetrics knihovny a kódu</span><span class="sxs-lookup"><span data-stu-id="f3712-250">BatchMetrics library and code sample</span></span>
<span data-ttu-id="f3712-251">Kromě toho ukázka kódu toohello v EfficientListQueries výše, můžete najít hello [BatchMetrics] [ batch_metrics] projekt v hello [azure-batch-samples] [ github_samples] Úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="f3712-251">In addition toohello EfficientListQueries code sample above, you can find hello [BatchMetrics][batch_metrics] project in hello [azure-batch-samples][github_samples] GitHub repository.</span></span> <span data-ttu-id="f3712-252">Ukázkový projekt Hello BatchMetrics ukazuje, jak tooefficiently monitorovat průběh úlohy Azure Batch pomocí rozhraní Batch API hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-252">hello BatchMetrics sample project demonstrates how tooefficiently monitor Azure Batch job progress using hello Batch API.</span></span>

<span data-ttu-id="f3712-253">Hello [BatchMetrics] [ batch_metrics] ukázka zahrnuje projektu knihovny tříd rozhraní .NET, která můžete začlenit do vašich vlastních projektů a jednoduchou příkazového řádku programu tooexercise a ukazují použití hello hello Knihovna.</span><span class="sxs-lookup"><span data-stu-id="f3712-253">hello [BatchMetrics][batch_metrics] sample includes a .NET class library project which you can incorporate into your own projects, and a simple command-line program tooexercise and demonstrate hello use of hello library.</span></span>

<span data-ttu-id="f3712-254">Hello ukázkovou aplikaci v rámci projektu hello ukazuje hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="f3712-254">hello sample application within hello project demonstrates hello following operations:</span></span>

1. <span data-ttu-id="f3712-255">Když vyberete konkrétní atributy v pořadí toodownload jenom hello vlastnosti, které budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="f3712-255">Selecting specific attributes in order toodownload only hello properties you need</span></span>
2. <span data-ttu-id="f3712-256">Filtrování na časy přechod stavu v pořadí toodownload pouze změny od poslední dotaz s hello</span><span class="sxs-lookup"><span data-stu-id="f3712-256">Filtering on state transition times in order toodownload only changes since hello last query</span></span>

<span data-ttu-id="f3712-257">Například následující metoda hello se zobrazí v hello knihovně BatchMetrics.</span><span class="sxs-lookup"><span data-stu-id="f3712-257">For example, hello following method appears in hello BatchMetrics library.</span></span> <span data-ttu-id="f3712-258">Vrátí ODATADetailLevel, která určuje, že pouze hello `id` a `state` vlastnosti by měla být získána hello entit, které jsou předmětem dotazování.</span><span class="sxs-lookup"><span data-stu-id="f3712-258">It returns an ODATADetailLevel that specifies that only hello `id` and `state` properties should be obtained for hello entities that are queried.</span></span> <span data-ttu-id="f3712-259">Také určuje, že pouze entity, jejichž stav se změnil od hello zadaný `DateTime` parametr má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="f3712-259">It also specifies that only entities whose state has changed since hello specified `DateTime` parameter should be returned.</span></span>

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a><span data-ttu-id="f3712-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3712-260">Next steps</span></span>
### <a name="parallel-node-tasks"></a><span data-ttu-id="f3712-261">Uzel paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="f3712-261">Parallel node tasks</span></span>
<span data-ttu-id="f3712-262">[Maximalizovat využití prostředků Azure Batch výpočetní uzel souběžných úloh](batch-parallel-node-tasks.md) je jiný článek související tooBatch výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3712-262">[Maximize Azure Batch compute resource usage with concurrent node tasks](batch-parallel-node-tasks.md) is another article related tooBatch application performance.</span></span> <span data-ttu-id="f3712-263">Některé typy úloh využívat výhod spouštění paralelní úlohy na větší – ale méně – výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="f3712-263">Some types of workloads can benefit from executing parallel tasks on larger--but fewer--compute nodes.</span></span> <span data-ttu-id="f3712-264">Podívejte se na hello [ukázkový scénář](batch-parallel-node-tasks.md#example-scenario) v článku hello podrobnosti o tento případ.</span><span class="sxs-lookup"><span data-stu-id="f3712-264">Check out hello [example scenario](batch-parallel-node-tasks.md#example-scenario) in hello article for details on such a scenario.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="f3712-265">Fórum batch</span><span class="sxs-lookup"><span data-stu-id="f3712-265">Batch Forum</span></span>
<span data-ttu-id="f3712-266">Hello [fóru služby Azure Batch] [ forum] na webu MSDN je skvělá umístit toodiscuss Batch a klást otázky týkající se služby hello.</span><span class="sxs-lookup"><span data-stu-id="f3712-266">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="f3712-267">HEAD na přes pro užitečné "rychlé" příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.</span><span class="sxs-lookup"><span data-stu-id="f3712-267">Head on over for helpful "sticky" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job