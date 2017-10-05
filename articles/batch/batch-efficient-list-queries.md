---
title: "Návrh efektivní seznamu dotazy – Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: a80b207f591bd888d4749287527013c5e554fb6e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a><span data-ttu-id="df633-103">Efektivně vytvářet dotazy ke prostředky Batch seznamu</span><span class="sxs-lookup"><span data-stu-id="df633-103">Create queries to list Batch resources efficiently</span></span>

<span data-ttu-id="df633-104">Zde se dozvíte, jak zvýšit výkon aplikace Azure Batch snižuje množství dat, která je vrácena službou při dotazování úlohy a úlohy a výpočetní uzly se [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="df633-104">Here you'll learn how to increase your Azure Batch application's performance by reducing the amount of data that is returned by the service when you query jobs, tasks, and compute nodes with the [Batch .NET][api_net] library.</span></span>

<span data-ttu-id="df633-105">Téměř všechny aplikace Batch je potřeba provést nějaký typ monitorování nebo jiné operace, který se dotazuje služby Batch, často v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="df633-105">Nearly all Batch applications need to perform some type of monitoring or other operation that queries the Batch service, often at regular intervals.</span></span> <span data-ttu-id="df633-106">Pokud chcete zjistit, zda jsou všechny ve frontě úloh zbývající v rámci úlohy, například musí získat data na každý úkol v úloze.</span><span class="sxs-lookup"><span data-stu-id="df633-106">For example, to determine whether there are any queued tasks remaining in a job, you must get data on every task in the job.</span></span> <span data-ttu-id="df633-107">Pokud chcete zjistit stav uzly ve fondu, musíte získat data na každý uzel ve fondu.</span><span class="sxs-lookup"><span data-stu-id="df633-107">To determine the status of nodes in your pool, you must get data on every node in the pool.</span></span> <span data-ttu-id="df633-108">Tento článek vysvětluje, jak provést takové dotazy nejefektivnějším způsobem.</span><span class="sxs-lookup"><span data-stu-id="df633-108">This article explains how to execute such queries in the most efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="df633-109">Služba Batch poskytuje speciální podporu rozhraní API pro běžný scénář inventur úkoly v úloze.</span><span class="sxs-lookup"><span data-stu-id="df633-109">The Batch service provides special API support for the common scenario of counting tasks in a job.</span></span> <span data-ttu-id="df633-110">Místo použití seznamu dotazu pro tyto, můžete zavolat [získat počty úloh] [ rest_get_task_counts] operaci.</span><span class="sxs-lookup"><span data-stu-id="df633-110">Instead of using a list query for these, you can call the [Get Task Counts][rest_get_task_counts] operation.</span></span> <span data-ttu-id="df633-111">Počet úloh Get Určuje, kolik úlohy čekají na vyřízení, spuštění nebo dokončení, a mít kolik úlohy byla úspěšná nebo neúspěšná.</span><span class="sxs-lookup"><span data-stu-id="df633-111">Get Task Counts indicates how many tasks are pending, running or complete, and how many tasks have succeeded or failed.</span></span> <span data-ttu-id="df633-112">Spočítá počet úloh GET je efektivnější než seznamu dotazu.</span><span class="sxs-lookup"><span data-stu-id="df633-112">Get Task Counts is more efficient than a list query.</span></span> <span data-ttu-id="df633-113">Další informace najdete v tématu [počet úloh pro úlohu podle stavu (Preview)](batch-get-task-counts.md).</span><span class="sxs-lookup"><span data-stu-id="df633-113">For more information, see [Count tasks for a job by state (Preview)](batch-get-task-counts.md).</span></span> 
>
> <span data-ttu-id="df633-114">Operace získání úkolů počty starší než 2017-06-01.5.1 není k dispozici ve verzích služby Batch.</span><span class="sxs-lookup"><span data-stu-id="df633-114">The Get Task Counts operation is not available in Batch service versions earlier than 2017-06-01.5.1.</span></span> <span data-ttu-id="df633-115">Pokud používáte starší verzi služby, potom pomocí seznamu dotazu místo počet úkoly v úloze.</span><span class="sxs-lookup"><span data-stu-id="df633-115">If you are using an older version of the service, then use a list query to count tasks in a job instead.</span></span>
>
> 

## <a name="meet-the-detaillevel"></a><span data-ttu-id="df633-116">Splňovat DetailLevel</span><span class="sxs-lookup"><span data-stu-id="df633-116">Meet the DetailLevel</span></span>
<span data-ttu-id="df633-117">V produkční aplikaci služby Batch můžete v tisíců čísel entity jako úlohy, úlohy a výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="df633-117">In a production Batch application, entities like jobs, tasks, and compute nodes can number in the thousands.</span></span> <span data-ttu-id="df633-118">Pokud budete požadovat informace o těchto prostředků, potenciálně velkého množství dat musí "křížová sítě" ze služby Batch do vaší aplikace na každý dotaz.</span><span class="sxs-lookup"><span data-stu-id="df633-118">When you request information on these resources, a potentially large amount of data must "cross the wire" from the Batch service to your application on each query.</span></span> <span data-ttu-id="df633-119">Tím, že omezí počet položek a typu informací, které je vrácených dotazem, můžete zvýšit rychlost své dotazy a proto výkon vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="df633-119">By limiting the number of items and type of information that is returned by a query, you can increase the speed of your queries, and therefore the performance of your application.</span></span>

<span data-ttu-id="df633-120">To [Batch .NET] [ api_net] seznamy fragmentu kódu rozhraní API *každých* úlohu, která je spojená s úlohou, spolu s *všechny* vlastností jednotlivých úloh:</span><span class="sxs-lookup"><span data-stu-id="df633-120">This [Batch .NET][api_net] API code snippet lists *every* task that is associated with a job, along with *all* of the properties of each task:</span></span>

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

<span data-ttu-id="df633-121">Mnohem efektivnější dotaz, můžete však provést použitím "úroveň podrobností" do dotazu.</span><span class="sxs-lookup"><span data-stu-id="df633-121">You can perform a much more efficient list query, however, by applying a "detail level" to your query.</span></span> <span data-ttu-id="df633-122">To provedete zadáním [ODATADetailLevel] [ odata] do objektu [JobOperations.ListTasks] [ net_list_tasks] metoda.</span><span class="sxs-lookup"><span data-stu-id="df633-122">You do this by supplying an [ODATADetailLevel][odata] object to the [JobOperations.ListTasks][net_list_tasks] method.</span></span> <span data-ttu-id="df633-123">Tento fragment kódu vrátí pouze ID, příkazový řádek a výpočetní uzel informace vlastnosti dokončených úloh:</span><span class="sxs-lookup"><span data-stu-id="df633-123">This snippet returns only the ID, command line, and compute node information properties of completed tasks:</span></span>

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

<span data-ttu-id="df633-124">V tomto ukázkovém scénáři, pokud existují tisíce úkolů do úlohy výsledky z druhého dotazu bude obvykle vrácen mnohem rychlejší než první.</span><span class="sxs-lookup"><span data-stu-id="df633-124">In this example scenario, if there are thousands of tasks in the job, the results from the second query will typically be returned much quicker than the first.</span></span> <span data-ttu-id="df633-125">Další informace o používání ODATADetailLevel při seznamu položek s Batch .NET API je součástí [pod](#efficient-querying-in-batch-net).</span><span class="sxs-lookup"><span data-stu-id="df633-125">More information about using ODATADetailLevel when you list items with the Batch .NET API is included [below](#efficient-querying-in-batch-net).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df633-126">Důrazně doporučujeme, aby vám *vždy* zadat objekt ODATADetailLevel tak, aby voláními rozhraní API .NET seznamu zajistit maximální efektivity a výkonu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="df633-126">We highly recommend that you *always* supply an ODATADetailLevel object to your .NET API list calls to ensure maximum efficiency and performance of your application.</span></span> <span data-ttu-id="df633-127">Zadáním úroveň podrobností můžete pomoct snížit dobu odezvy služby Batch, zvýšit využití sítě a minimalizovat využití paměti v klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="df633-127">By specifying a detail level, you can help to lower Batch service response times, improve network utilization, and minimize memory usage by client applications.</span></span>
> 
> 

## <a name="filter-select-and-expand"></a><span data-ttu-id="df633-128">Filtrovat, vyberte a rozbalte</span><span class="sxs-lookup"><span data-stu-id="df633-128">Filter, select, and expand</span></span>
<span data-ttu-id="df633-129">[Batch .NET] [ api_net] a [Batch REST] [ api_rest] rozhraní API nabízejí možnost snížit i počet položek, které jsou vráceny v seznamu, a také množství informací, které se vrátí pro každou.</span><span class="sxs-lookup"><span data-stu-id="df633-129">The [Batch .NET][api_net] and [Batch REST][api_rest] APIs provide the ability to reduce both the number of items that are returned in a list, as well as the amount of information that is returned for each.</span></span> <span data-ttu-id="df633-130">To uděláte tak, že zadáte **filtru**, **vyberte**, a **rozbalte řetězce** při provádění dotazů seznamu.</span><span class="sxs-lookup"><span data-stu-id="df633-130">You do so by specifying **filter**, **select**, and **expand strings** when performing list queries.</span></span>

### <a name="filter"></a><span data-ttu-id="df633-131">Filtr</span><span class="sxs-lookup"><span data-stu-id="df633-131">Filter</span></span>
<span data-ttu-id="df633-132">Řetězec filtru je výraz, který snižuje počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="df633-132">The filter string is an expression that reduces the number of items that are returned.</span></span> <span data-ttu-id="df633-133">Například seznam pouze spuštěné úlohy pro úlohu, nebo seznam pouze výpočetní uzly, které jsou připravené ke spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="df633-133">For example, list only the running tasks for a job, or list only compute nodes that are ready to run tasks.</span></span>

* <span data-ttu-id="df633-134">Řetězec filtru se skládá z jednoho nebo více výrazů s výrazem, který obsahuje název vlastnosti, operátor a hodnotu.</span><span class="sxs-lookup"><span data-stu-id="df633-134">The filter string consists of one or more expressions, with an expression that consists of a property name, operator, and value.</span></span> <span data-ttu-id="df633-135">Vlastnosti, které lze zadat jsou specifické pro každý typ entity, které můžete zadat dotaz, jako jsou operátory, které jsou podporovány pro každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="df633-135">The properties that can be specified are specific to each entity type that you query, as are the operators that are supported for each property.</span></span>
* <span data-ttu-id="df633-136">Více výrazů lze spojovat pomocí logických operátorů `and` a `or`.</span><span class="sxs-lookup"><span data-stu-id="df633-136">Multiple expressions can be combined by using the logical operators `and` and `or`.</span></span>
* <span data-ttu-id="df633-137">Tento příklad filtrování seznamů řetězec jen spuštění úlohy "vykreslení": `(state eq 'running') and startswith(id, 'renderTask')`.</span><span class="sxs-lookup"><span data-stu-id="df633-137">This example filter string lists only the running "render" tasks: `(state eq 'running') and startswith(id, 'renderTask')`.</span></span>

### <a name="select"></a><span data-ttu-id="df633-138">Vyberte</span><span class="sxs-lookup"><span data-stu-id="df633-138">Select</span></span>
<span data-ttu-id="df633-139">Vyberte řetězec omezení hodnoty vlastností, které se vrátí pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="df633-139">The select string limits the property values that are returned for each item.</span></span> <span data-ttu-id="df633-140">Zadejte seznam názvů vlastností a pouze hodnoty těchto vlastností jsou vráceny pro položky v výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="df633-140">You specify a list of property names, and only those property values are returned for the items in the query results.</span></span>

* <span data-ttu-id="df633-141">Vyberte řetězec tvořený seznam názvů vlastností oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="df633-141">The select string consists of a comma-separated list of property names.</span></span> <span data-ttu-id="df633-142">Můžete určit všechny vlastnosti pro typ entity, který se dotazuje.</span><span class="sxs-lookup"><span data-stu-id="df633-142">You can specify any of the properties for the entity type you are querying.</span></span>
* <span data-ttu-id="df633-143">Tento příklad vyberte řetězec Určuje, že má být vrácen pouze tři hodnoty vlastností pro každou úlohu: `id, state, stateTransitionTime`.</span><span class="sxs-lookup"><span data-stu-id="df633-143">This example select string specifies that only three property values should be returned for each task: `id, state, stateTransitionTime`.</span></span>

### <a name="expand"></a><span data-ttu-id="df633-144">Rozbalit</span><span class="sxs-lookup"><span data-stu-id="df633-144">Expand</span></span>
<span data-ttu-id="df633-145">Rozbalte řetězec snižuje počet volání rozhraní API, které jsou nutné k získání určité informace.</span><span class="sxs-lookup"><span data-stu-id="df633-145">The expand string reduces the number of API calls that are required to obtain certain information.</span></span> <span data-ttu-id="df633-146">Použijete-li řetězec rozbalte, nelze získat další informace o jednotlivých položkách na základě jednoho volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df633-146">When you use an expand string, more information about each item can be obtained with a single API call.</span></span> <span data-ttu-id="df633-147">Místo první získání seznamu entity, pak vyžadování informací o pro každou položku v seznamu, použijte řetězec rozbalte získat stejné informace v jediném volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="df633-147">Rather than first obtaining the list of entities, then requesting information for each item in the list, you use an expand string to obtain the same information in a single API call.</span></span> <span data-ttu-id="df633-148">Menší volání rozhraní API znamená vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="df633-148">Less API calls means better performance.</span></span>

* <span data-ttu-id="df633-149">Podobně jako u vyberte řetězec, řetězec rozbalte Určuje, jestli některá data je zahrnuta v seznamu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="df633-149">Similar to the select string, the expand string controls whether certain data is included in list query results.</span></span>
* <span data-ttu-id="df633-150">Rozbalte řetězec je podporována pouze v případě se používá v seznam úloh, plány úloh, úlohy a fondy.</span><span class="sxs-lookup"><span data-stu-id="df633-150">The expand string is only supported when it is used in listing jobs, job schedules, tasks, and pools.</span></span> <span data-ttu-id="df633-151">V současné době podporuje pouze statistické informace.</span><span class="sxs-lookup"><span data-stu-id="df633-151">Currently, it only supports statistics information.</span></span>
* <span data-ttu-id="df633-152">Pokud se všechny vlastnosti požadované a není zadán žádný vyberte řetězec, řetězec rozbalte *musí* použít k získání statistické informace.</span><span class="sxs-lookup"><span data-stu-id="df633-152">When all properties are required and no select string is specified, the expand string *must* be used to get statistics information.</span></span> <span data-ttu-id="df633-153">Pokud vyberte řetězec se používá k získání podmnožiny vlastností, pak `stats` lze zadat v řetězci vyberte a rozbalte řetězec není potřeba zadat.</span><span class="sxs-lookup"><span data-stu-id="df633-153">If a select string is used to obtain a subset of properties, then `stats` can be specified in the select string, and the expand string does not need to be specified.</span></span>
* <span data-ttu-id="df633-154">Tento příklad rozbalte položku řetězec Určuje, zda má být vrácen statistické informace pro každou položku v seznamu: `stats`.</span><span class="sxs-lookup"><span data-stu-id="df633-154">This example expand string specifies that statistics information should be returned for each item in the list: `stats`.</span></span>

> [!NOTE]
> <span data-ttu-id="df633-155">Při vytváření jakýkoli z typů řetězec dotazu tři (filtrování, vyberte a rozbalte možnost), ujistěte se, že názvy vlastností a případ shodovat s jejich protějšky element REST API.</span><span class="sxs-lookup"><span data-stu-id="df633-155">When constructing any of the three query string types (filter, select, and expand), you must ensure that the property names and case match that of their REST API element counterparts.</span></span> <span data-ttu-id="df633-156">Například při práci s .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) třídy, je nutné zadat **stavu** místo **stavu**, i když je vlastnost .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span><span class="sxs-lookup"><span data-stu-id="df633-156">For example, when working with the .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) class, you must specify **state** instead of **State**, even though the .NET property is [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state).</span></span> <span data-ttu-id="df633-157">Podívejte se na tabulky pod pro mapování vlastností mezi .NET a REST API.</span><span class="sxs-lookup"><span data-stu-id="df633-157">See the tables below for property mappings between the .NET and REST APIs.</span></span>
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a><span data-ttu-id="df633-158">Pravidla pro filtr, vyberte a rozbalte řetězce</span><span class="sxs-lookup"><span data-stu-id="df633-158">Rules for filter, select, and expand strings</span></span>
* <span data-ttu-id="df633-159">Názvy vlastností ve filtru, vyberte a rozbalte řetězce by se měla zobrazit jako ve [Batch REST] [ api_rest] rozhraní API – i v případě, že používáte [Batch .NET] [ api_net] nebo jeden z ostatních Batch sad SDK.</span><span class="sxs-lookup"><span data-stu-id="df633-159">Properties names in filter, select, and expand strings should appear as they do in the [Batch REST][api_rest] API--even when you use [Batch .NET][api_net] or one of the other Batch SDKs.</span></span>
* <span data-ttu-id="df633-160">Všechny názvy vlastností malá a velká písmena, ale hodnoty vlastností se malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="df633-160">All property names are case-sensitive, but property values are case insensitive.</span></span>
* <span data-ttu-id="df633-161">Datum a čas řetězců může mít jednu ze dvou formátů a musí předcházet `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="df633-161">Date/time strings can be one of two formats, and must be preceded with `DateTime`.</span></span>
  
  * <span data-ttu-id="df633-162">Příklad formátu W3C DTF:`creationTime gt DateTime'2011-05-08T08:49:37Z'`</span><span class="sxs-lookup"><span data-stu-id="df633-162">W3C-DTF format example: `creationTime gt DateTime'2011-05-08T08:49:37Z'`</span></span>
  * <span data-ttu-id="df633-163">Příklad formátu RFC 1123:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span><span class="sxs-lookup"><span data-stu-id="df633-163">RFC 1123 format example: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`</span></span>
* <span data-ttu-id="df633-164">Logická hodnota řetězce jsou buď `true` nebo `false`.</span><span class="sxs-lookup"><span data-stu-id="df633-164">Boolean strings are either `true` or `false`.</span></span>
* <span data-ttu-id="df633-165">Pokud je zadána neplatná vlastnost nebo operátor, `400 (Bad Request)` bude výsledkem chyba.</span><span class="sxs-lookup"><span data-stu-id="df633-165">If an invalid property or operator is specified, a `400 (Bad Request)` error will result.</span></span>

## <a name="efficient-querying-in-batch-net"></a><span data-ttu-id="df633-166">Efektivní dotazování v Batch .NET</span><span class="sxs-lookup"><span data-stu-id="df633-166">Efficient querying in Batch .NET</span></span>
<span data-ttu-id="df633-167">V rámci [Batch .NET] [ api_net] rozhraní API, [ODATADetailLevel] [ odata] třída se používá pro zadávání filtru, vyberte a rozbalte seznam způsobů řetězce.</span><span class="sxs-lookup"><span data-stu-id="df633-167">Within the [Batch .NET][api_net] API, the [ODATADetailLevel][odata] class is used for supplying filter, select, and expand strings to list operations.</span></span> <span data-ttu-id="df633-168">Třída ODataDetailLevel má tři řetězec veřejné vlastnosti, které lze zadaným v konstruktoru, nebo nastavit přímo na objekt.</span><span class="sxs-lookup"><span data-stu-id="df633-168">The ODataDetailLevel class has three public string properties that can be specified in the constructor, or set directly on the object.</span></span> <span data-ttu-id="df633-169">Můžete poté předat objekt ODataDetailLevel jako parametr různé operace výpisu, jako [ListPools][net_list_pools], [ListJobs][net_list_jobs], a [ListTasks][net_list_tasks].</span><span class="sxs-lookup"><span data-stu-id="df633-169">You then pass the ODataDetailLevel object as a parameter to the various list operations such as [ListPools][net_list_pools], [ListJobs][net_list_jobs], and [ListTasks][net_list_tasks].</span></span>

* <span data-ttu-id="df633-170">[ODATADetailLevel][odata].[ FilterClause][odata_filter]: omezit počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="df633-170">[ODATADetailLevel][odata].[FilterClause][odata_filter]: Limit the number of items that are returned.</span></span>
* <span data-ttu-id="df633-171">[ODATADetailLevel][odata].[ SelectClause][odata_select]: Zadejte každou položku jsou vráceny hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="df633-171">[ODATADetailLevel][odata].[SelectClause][odata_select]: Specify which property values are returned with each item.</span></span>
* <span data-ttu-id="df633-172">[ODATADetailLevel][odata].[ ExpandClause][odata_expand]: načtení dat pro všechny položky v jediného rozhraní API volat místo samostatné volání pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="df633-172">[ODATADetailLevel][odata].[ExpandClause][odata_expand]: Retrieve data for all items in a single API call instead of separate calls for each item.</span></span>

<span data-ttu-id="df633-173">Následující fragment kódu používá rozhraní Batch .NET API k efektivní dotazování na službu Batch pro statistiku konkrétní sadu fondů.</span><span class="sxs-lookup"><span data-stu-id="df633-173">The following code snippet uses the Batch .NET API to efficiently query the Batch service for the statistics of a specific set of pools.</span></span> <span data-ttu-id="df633-174">V tomto scénáři má uživatel Batch testovací a produkční fondů.</span><span class="sxs-lookup"><span data-stu-id="df633-174">In this scenario, the Batch user has both test and production pools.</span></span> <span data-ttu-id="df633-175">Test fondu ID mají předponu "test" a fondu produkční ID mají předponu "produkčnímu".</span><span class="sxs-lookup"><span data-stu-id="df633-175">The test pool IDs are prefixed with "test", and the production pool IDs are prefixed with "prod".</span></span> <span data-ttu-id="df633-176">V tomto fragmentu kódu *myBatchClient* je správně inicializována instanci [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) třídy.</span><span class="sxs-lookup"><span data-stu-id="df633-176">In the snippet, *myBatchClient* is a properly initialized instance of the [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) class.</span></span>

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> <span data-ttu-id="df633-177">Instance [ODATADetailLevel] [ odata] nakonfigurovaný s vyberte a rozbalte klauzule lze také předat příslušné metody Get, jako například [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), chcete-li omezit množství dat, která je vrácena.</span><span class="sxs-lookup"><span data-stu-id="df633-177">An instance of [ODATADetailLevel][odata] that is configured with Select and Expand clauses can also be passed to appropriate Get methods, such as [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), to limit the amount of data that is returned.</span></span>
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a><span data-ttu-id="df633-178">Batch REST pro mapování rozhraní API .NET</span><span class="sxs-lookup"><span data-stu-id="df633-178">Batch REST to .NET API mappings</span></span>
<span data-ttu-id="df633-179">Názvy vlastností ve filtru, vyberte a rozbalte řetězce *musí* odráží jejich protějšky REST API, v názvu i případu.</span><span class="sxs-lookup"><span data-stu-id="df633-179">Property names in filter, select, and expand strings *must* reflect their REST API counterparts, both in name and case.</span></span> <span data-ttu-id="df633-180">Následující tabulky poskytují mapování mezi svými protějšky .NET a REST API.</span><span class="sxs-lookup"><span data-stu-id="df633-180">The tables below provide mappings between the .NET and REST API counterparts.</span></span>

### <a name="mappings-for-filter-strings"></a><span data-ttu-id="df633-181">Mapování pro filtr řetězce</span><span class="sxs-lookup"><span data-stu-id="df633-181">Mappings for filter strings</span></span>
* <span data-ttu-id="df633-182">**Seznam metod rozhraní .NET**: každá z metod rozhraní API .NET v tomto sloupci přijímá [ODATADetailLevel] [ odata] objekt jako parametr.</span><span class="sxs-lookup"><span data-stu-id="df633-182">**.NET list methods**: Each of the .NET API methods in this column accepts an [ODATADetailLevel][odata] object as a parameter.</span></span>
* <span data-ttu-id="df633-183">**REST seznamu požadavků**: stránka každý REST API propojené v tomto sloupci obsahuje tabulku, která určuje vlastnosti a operace, které jsou povoleny v *filtru* řetězce.</span><span class="sxs-lookup"><span data-stu-id="df633-183">**REST list requests**: Each REST API page linked to in this column contains a table that specifies the properties and operations that are allowed in *filter* strings.</span></span> <span data-ttu-id="df633-184">Když vytvoříte budou používat tyto operace a názvy vlastností [ODATADetailLevel.FilterClause] [ odata_filter] řetězec.</span><span class="sxs-lookup"><span data-stu-id="df633-184">You will use these property names and operations when you construct an [ODATADetailLevel.FilterClause][odata_filter] string.</span></span>

| <span data-ttu-id="df633-185">Seznam metod rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="df633-185">.NET list methods</span></span> | <span data-ttu-id="df633-186">REST seznamu požadavků</span><span class="sxs-lookup"><span data-stu-id="df633-186">REST list requests</span></span> |
| --- | --- |
| <span data-ttu-id="df633-187">[CertificateOperations.ListCertificates][net_list_certs]</span><span class="sxs-lookup"><span data-stu-id="df633-187">[CertificateOperations.ListCertificates][net_list_certs]</span></span> |<span data-ttu-id="df633-188">[Seznam certifikátů na účtu][rest_list_certs]</span><span class="sxs-lookup"><span data-stu-id="df633-188">[List the certificates in an account][rest_list_certs]</span></span> |
| <span data-ttu-id="df633-189">[CloudTask.ListNodeFiles][net_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="df633-189">[CloudTask.ListNodeFiles][net_list_task_files]</span></span> |<span data-ttu-id="df633-190">[Seznam soubory spojené s úlohami][rest_list_task_files]</span><span class="sxs-lookup"><span data-stu-id="df633-190">[List the files associated with a task][rest_list_task_files]</span></span> |
| <span data-ttu-id="df633-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="df633-191">[JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status]</span></span> |<span data-ttu-id="df633-192">[Seznam stav úlohy přípravy a uvolnění úloh pro úlohu][rest_list_jobprep_status]</span><span class="sxs-lookup"><span data-stu-id="df633-192">[List the status of the job preparation and job release tasks for a job][rest_list_jobprep_status]</span></span> |
| <span data-ttu-id="df633-193">[JobOperations.ListJobs][net_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="df633-193">[JobOperations.ListJobs][net_list_jobs]</span></span> |<span data-ttu-id="df633-194">[Seznam úloh na účtu][rest_list_jobs]</span><span class="sxs-lookup"><span data-stu-id="df633-194">[List the jobs in an account][rest_list_jobs]</span></span> |
| <span data-ttu-id="df633-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="df633-195">[JobOperations.ListNodeFiles][net_list_nodefiles]</span></span> |<span data-ttu-id="df633-196">[Seznam souborů v uzlu][rest_list_nodefiles]</span><span class="sxs-lookup"><span data-stu-id="df633-196">[List the files on a node][rest_list_nodefiles]</span></span> |
| <span data-ttu-id="df633-197">[JobOperations.ListTasks][net_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="df633-197">[JobOperations.ListTasks][net_list_tasks]</span></span> |<span data-ttu-id="df633-198">[Seznam úkoly spojené s úlohou][rest_list_tasks]</span><span class="sxs-lookup"><span data-stu-id="df633-198">[List the tasks associated with a job][rest_list_tasks]</span></span> |
| <span data-ttu-id="df633-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="df633-199">[JobScheduleOperations.ListJobSchedules][net_list_job_schedules]</span></span> |<span data-ttu-id="df633-200">[Seznam plánů úloh na účtu][rest_list_job_schedules]</span><span class="sxs-lookup"><span data-stu-id="df633-200">[List the job schedules in an account][rest_list_job_schedules]</span></span> |
| <span data-ttu-id="df633-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="df633-201">[JobScheduleOperations.ListJobs][net_list_schedule_jobs]</span></span> |<span data-ttu-id="df633-202">[Seznam úloh, které jsou přidružené k plánu úlohy][rest_list_schedule_jobs]</span><span class="sxs-lookup"><span data-stu-id="df633-202">[List the jobs associated with a job schedule][rest_list_schedule_jobs]</span></span> |
| <span data-ttu-id="df633-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="df633-203">[PoolOperations.ListComputeNodes][net_list_compute_nodes]</span></span> |<span data-ttu-id="df633-204">[Seznam výpočetní uzly ve fondu][rest_list_compute_nodes]</span><span class="sxs-lookup"><span data-stu-id="df633-204">[List the compute nodes in a pool][rest_list_compute_nodes]</span></span> |
| <span data-ttu-id="df633-205">[PoolOperations.ListPools][net_list_pools]</span><span class="sxs-lookup"><span data-stu-id="df633-205">[PoolOperations.ListPools][net_list_pools]</span></span> |<span data-ttu-id="df633-206">[Zobrazí seznam fondů na účtu][rest_list_pools]</span><span class="sxs-lookup"><span data-stu-id="df633-206">[List the pools in an account][rest_list_pools]</span></span> |

### <a name="mappings-for-select-strings"></a><span data-ttu-id="df633-207">Mapování pro vyberte řetězce</span><span class="sxs-lookup"><span data-stu-id="df633-207">Mappings for select strings</span></span>
* <span data-ttu-id="df633-208">**Batch .NET typy**: typy Batch .NET API.</span><span class="sxs-lookup"><span data-stu-id="df633-208">**Batch .NET types**: Batch .NET API types.</span></span>
* <span data-ttu-id="df633-209">**Rozhraní REST API entity**: obsahuje jednu nebo více tabulek, které seznam názvů vlastností rozhraní REST API pro typ každé stránce v tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="df633-209">**REST API entities**: Each page in this column contains one or more tables that list the REST API property names for the type.</span></span> <span data-ttu-id="df633-210">Tyto názvy vlastností se používají při vytváření *vyberte* řetězce.</span><span class="sxs-lookup"><span data-stu-id="df633-210">These property names are used when you construct *select* strings.</span></span> <span data-ttu-id="df633-211">Tyto stejné názvy vlastností použijete při vytváření [ODATADetailLevel.SelectClause] [ odata_select] řetězec.</span><span class="sxs-lookup"><span data-stu-id="df633-211">You will use these same property names when you construct an [ODATADetailLevel.SelectClause][odata_select] string.</span></span>

| <span data-ttu-id="df633-212">Typy batch .NET</span><span class="sxs-lookup"><span data-stu-id="df633-212">Batch .NET types</span></span> | <span data-ttu-id="df633-213">Rozhraní REST API entity</span><span class="sxs-lookup"><span data-stu-id="df633-213">REST API entities</span></span> |
| --- | --- |
| <span data-ttu-id="df633-214">[Certifikát][net_cert]</span><span class="sxs-lookup"><span data-stu-id="df633-214">[Certificate][net_cert]</span></span> |<span data-ttu-id="df633-215">[Získání informací o certifikát][rest_get_cert]</span><span class="sxs-lookup"><span data-stu-id="df633-215">[Get information about a certificate][rest_get_cert]</span></span> |
| <span data-ttu-id="df633-216">[CloudJob][net_job]</span><span class="sxs-lookup"><span data-stu-id="df633-216">[CloudJob][net_job]</span></span> |<span data-ttu-id="df633-217">[Získání informací o úlohy][rest_get_job]</span><span class="sxs-lookup"><span data-stu-id="df633-217">[Get information about a job][rest_get_job]</span></span> |
| <span data-ttu-id="df633-218">[CloudJobSchedule][net_schedule]</span><span class="sxs-lookup"><span data-stu-id="df633-218">[CloudJobSchedule][net_schedule]</span></span> |<span data-ttu-id="df633-219">[Získat informace o plánu úlohy][rest_get_schedule]</span><span class="sxs-lookup"><span data-stu-id="df633-219">[Get information about a job schedule][rest_get_schedule]</span></span> |
| <span data-ttu-id="df633-220">[ComputeNode][net_node]</span><span class="sxs-lookup"><span data-stu-id="df633-220">[ComputeNode][net_node]</span></span> |<span data-ttu-id="df633-221">[Získání informací o uzlu][rest_get_node]</span><span class="sxs-lookup"><span data-stu-id="df633-221">[Get information about a node][rest_get_node]</span></span> |
| <span data-ttu-id="df633-222">[CloudPool][net_pool]</span><span class="sxs-lookup"><span data-stu-id="df633-222">[CloudPool][net_pool]</span></span> |<span data-ttu-id="df633-223">[Získat informace o fondu][rest_get_pool]</span><span class="sxs-lookup"><span data-stu-id="df633-223">[Get information about a pool][rest_get_pool]</span></span> |
| <span data-ttu-id="df633-224">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="df633-224">[CloudTask][net_task]</span></span> |<span data-ttu-id="df633-225">[Získat informace o úkolu][rest_get_task]</span><span class="sxs-lookup"><span data-stu-id="df633-225">[Get information about a task][rest_get_task]</span></span> |

## <a name="example-construct-a-filter-string"></a><span data-ttu-id="df633-226">Příklad: vytvoření řetězec filtru</span><span class="sxs-lookup"><span data-stu-id="df633-226">Example: construct a filter string</span></span>
<span data-ttu-id="df633-227">Když vytvoříte řetězec filtru pro [ODATADetailLevel.FilterClause][odata_filter], najdete v tabulce výše v části "Mapování pro filtr řetězce" na stránce dokumentace najít rozhraní REST API, která odpovídá seznamu operaci, kterou chcete provést.</span><span class="sxs-lookup"><span data-stu-id="df633-227">When you construct a filter string for [ODATADetailLevel.FilterClause][odata_filter], consult the table above under "Mappings for filter strings" to find the REST API documentation page that corresponds to the list operation that you wish to perform.</span></span> <span data-ttu-id="df633-228">V první tabulce více řádků na této stránce najdete filtrování vlastností a jejich podporované operátory.</span><span class="sxs-lookup"><span data-stu-id="df633-228">You will find the filterable properties and their supported operators in the first multirow table on that page.</span></span> <span data-ttu-id="df633-229">Pokud chcete načíst všechny úlohy, jejichž ukončovací kód procesu je nenulové hodnoty, například tento řádek na [seznamu úkoly spojené s úlohou] [ rest_list_tasks] určuje povolené operátory a použít vlastnost řetězec:</span><span class="sxs-lookup"><span data-stu-id="df633-229">If you wish to retrieve all tasks whose exit code was nonzero, for example, this row on [List the tasks associated with a job][rest_list_tasks] specifies the applicable property string and allowable operators:</span></span>

| <span data-ttu-id="df633-230">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="df633-230">Property</span></span> | <span data-ttu-id="df633-231">Povolené operace</span><span class="sxs-lookup"><span data-stu-id="df633-231">Operations allowed</span></span> | <span data-ttu-id="df633-232">Typ</span><span class="sxs-lookup"><span data-stu-id="df633-232">Type</span></span> |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

<span data-ttu-id="df633-233">Proto by byl řetězec filtru pro výpis všech úloh s nenulový ukončovací kód:</span><span class="sxs-lookup"><span data-stu-id="df633-233">Thus, the filter string for listing all tasks with a nonzero exit code would be:</span></span>

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a><span data-ttu-id="df633-234">Příklad: sestavit vyberte řetězec</span><span class="sxs-lookup"><span data-stu-id="df633-234">Example: construct a select string</span></span>
<span data-ttu-id="df633-235">K vytvoření [ODATADetailLevel.SelectClause][odata_select], projděte si v tabulce výše v části "Mapování pro vyberte řetězce" a přejděte na stránku REST API, která odpovídá typ entity, která jsou výpis.</span><span class="sxs-lookup"><span data-stu-id="df633-235">To construct [ODATADetailLevel.SelectClause][odata_select], consult the table above under "Mappings for select strings" and navigate to the REST API page that corresponds to the type of entity that you are listing.</span></span> <span data-ttu-id="df633-236">V první tabulce více řádků na této stránce najdete volitelný vlastnostmi a jejich podporované operátory.</span><span class="sxs-lookup"><span data-stu-id="df633-236">You will find the selectable properties and their supported operators in the first multirow table on that page.</span></span> <span data-ttu-id="df633-237">Pokud chcete načíst pouze ID a příkazový řádek pro každý úkol v seznamu, například zjistíte, tyto řádky v tabulce použít na [získat informace o úkolu][rest_get_task]:</span><span class="sxs-lookup"><span data-stu-id="df633-237">If you wish to retrieve only the ID and command line for each task in a list, for example, you will find these rows in the applicable table on [Get information about a task][rest_get_task]:</span></span>

| <span data-ttu-id="df633-238">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="df633-238">Property</span></span> | <span data-ttu-id="df633-239">Typ</span><span class="sxs-lookup"><span data-stu-id="df633-239">Type</span></span> | <span data-ttu-id="df633-240">Poznámky</span><span class="sxs-lookup"><span data-stu-id="df633-240">Notes</span></span> |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

<span data-ttu-id="df633-241">Poté budou vyberte řetězec pro včetně pouze ID a příkazového řádku u jednotlivých uvedených úkolů:</span><span class="sxs-lookup"><span data-stu-id="df633-241">The select string for including only the ID and command line with each listed task would then be:</span></span>

`id, commandLine`

## <a name="code-samples"></a><span data-ttu-id="df633-242">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="df633-242">Code samples</span></span>
### <a name="efficient-list-queries-code-sample"></a><span data-ttu-id="df633-243">Ukázka kódu dotazy efektivní seznamu</span><span class="sxs-lookup"><span data-stu-id="df633-243">Efficient list queries code sample</span></span>
<span data-ttu-id="df633-244">Podívejte se [EfficientListQueries] [ efficient_query_sample] ukázkového projektu na Githubu, které chcete zobrazit, jak efektivní dotazování seznamu může ovlivnit výkon v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="df633-244">Check out the [EfficientListQueries][efficient_query_sample] sample project on GitHub to see how efficient list querying can affect performance in an application.</span></span> <span data-ttu-id="df633-245">Tato Konzolová aplikace C# vytvoří a přidá velkého počtu úkolů do úlohy.</span><span class="sxs-lookup"><span data-stu-id="df633-245">This C# console application creates and adds a large number of tasks to a job.</span></span> <span data-ttu-id="df633-246">Pak umožňuje více volání [JobOperations.ListTasks] [ net_list_tasks] metoda a předává [ODATADetailLevel] [ odata] objekty, které jsou nakonfigurovány s jinou vlastnost hodnoty pro množství dat, který se má vrátit se liší.</span><span class="sxs-lookup"><span data-stu-id="df633-246">Then, it makes multiple calls to the [JobOperations.ListTasks][net_list_tasks] method and passes [ODATADetailLevel][odata] objects that are configured with different property values to vary the amount of data to be returned.</span></span> <span data-ttu-id="df633-247">Vyvolá výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="df633-247">It produces output similar to the following:</span></span>

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

<span data-ttu-id="df633-248">Jak je znázorněno v procentuálně dobu, můžete výrazně snížit dobu odezvy na dotazy omezením vlastnosti a počet položek, které jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="df633-248">As shown in the elapsed times, you can greatly lower query response times by limiting the properties and the number of items that are returned.</span></span> <span data-ttu-id="df633-249">Tato a Další ukázkové projekty v lze najít [azure-batch-samples] [ github_samples] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="df633-249">You can find this and other sample projects in the [azure-batch-samples][github_samples] repository on GitHub.</span></span>

### <a name="batchmetrics-library-and-code-sample"></a><span data-ttu-id="df633-250">Ukázka BatchMetrics knihovny a kódu</span><span class="sxs-lookup"><span data-stu-id="df633-250">BatchMetrics library and code sample</span></span>
<span data-ttu-id="df633-251">Kromě EfficientListQueries kód ukázce výše můžete najít [BatchMetrics] [ batch_metrics] v projektu [azure-batch-samples] [ github_samples] úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="df633-251">In addition to the EfficientListQueries code sample above, you can find the [BatchMetrics][batch_metrics] project in the [azure-batch-samples][github_samples] GitHub repository.</span></span> <span data-ttu-id="df633-252">Ukázkový projekt BatchMetrics ukazuje, jak efektivně sledovat průběh úlohy Azure Batch pomocí rozhraní API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="df633-252">The BatchMetrics sample project demonstrates how to efficiently monitor Azure Batch job progress using the Batch API.</span></span>

<span data-ttu-id="df633-253">[BatchMetrics] [ batch_metrics] ukázka zahrnuje projektu knihovny tříd rozhraní .NET, která můžete začlenit do vašich vlastních projektů a jednoduchý příkazového řádku programu prověření a Ukázka použití knihovny.</span><span class="sxs-lookup"><span data-stu-id="df633-253">The [BatchMetrics][batch_metrics] sample includes a .NET class library project which you can incorporate into your own projects, and a simple command-line program to exercise and demonstrate the use of the library.</span></span>

<span data-ttu-id="df633-254">Ukázkovou aplikaci v rámci projektu ukazuje následující operace:</span><span class="sxs-lookup"><span data-stu-id="df633-254">The sample application within the project demonstrates the following operations:</span></span>

1. <span data-ttu-id="df633-255">Chcete-li stáhnout pouze vlastnosti, které potřebujete výběr konkrétní atributy</span><span class="sxs-lookup"><span data-stu-id="df633-255">Selecting specific attributes in order to download only the properties you need</span></span>
2. <span data-ttu-id="df633-256">Chcete-li stáhnout pouze změny od poslední dotaz s filtrování doby přechodu stavu</span><span class="sxs-lookup"><span data-stu-id="df633-256">Filtering on state transition times in order to download only changes since the last query</span></span>

<span data-ttu-id="df633-257">V knihovně BatchMetrics se například zobrazí následující metodu.</span><span class="sxs-lookup"><span data-stu-id="df633-257">For example, the following method appears in the BatchMetrics library.</span></span> <span data-ttu-id="df633-258">Vrátí ODATADetailLevel, která určuje, že pouze `id` a `state` by měl získat vlastnosti pro entity, které jsou předmětem dotazování.</span><span class="sxs-lookup"><span data-stu-id="df633-258">It returns an ODATADetailLevel that specifies that only the `id` and `state` properties should be obtained for the entities that are queried.</span></span> <span data-ttu-id="df633-259">Také určuje, že jenom entity, jejichž stav se změnil od zadaného `DateTime` parametr má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="df633-259">It also specifies that only entities whose state has changed since the specified `DateTime` parameter should be returned.</span></span>

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a><span data-ttu-id="df633-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df633-260">Next steps</span></span>
### <a name="parallel-node-tasks"></a><span data-ttu-id="df633-261">Uzel paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="df633-261">Parallel node tasks</span></span>
<span data-ttu-id="df633-262">[Maximalizovat využití prostředků Azure Batch výpočetní uzel souběžných úloh](batch-parallel-node-tasks.md) je jiný článek související s výkonem aplikací Batch.</span><span class="sxs-lookup"><span data-stu-id="df633-262">[Maximize Azure Batch compute resource usage with concurrent node tasks](batch-parallel-node-tasks.md) is another article related to Batch application performance.</span></span> <span data-ttu-id="df633-263">Některé typy úloh využívat výhod spouštění paralelní úlohy na větší – ale méně – výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="df633-263">Some types of workloads can benefit from executing parallel tasks on larger--but fewer--compute nodes.</span></span> <span data-ttu-id="df633-264">Podívejte se [ukázkový scénář](batch-parallel-node-tasks.md#example-scenario) v článku podrobnosti o tento případ.</span><span class="sxs-lookup"><span data-stu-id="df633-264">Check out the [example scenario](batch-parallel-node-tasks.md#example-scenario) in the article for details on such a scenario.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="df633-265">Fórum batch</span><span class="sxs-lookup"><span data-stu-id="df633-265">Batch Forum</span></span>
<span data-ttu-id="df633-266">[Fóru služby Azure Batch] [ forum] na webu MSDN je skvělým místem popisují Batch a klást otázky týkající se služby.</span><span class="sxs-lookup"><span data-stu-id="df633-266">The [Azure Batch Forum][forum] on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="df633-267">HEAD na přes pro užitečné "rychlé" příspěvky a při jejich vzniku při sestavování řešení Batch zveřejněte svoje otázky.</span><span class="sxs-lookup"><span data-stu-id="df633-267">Head on over for helpful "sticky" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

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