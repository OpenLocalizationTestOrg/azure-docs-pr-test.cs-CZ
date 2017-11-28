---
title: "aaaRun úlohy v paralelní toouse výpočetní prostředky efektivně - Azure Batch | Microsoft Docs"
description: "Zvýšení efektivity a snížení nákladů pomocí méně výpočetních uzlů a spuštění souběžných úkolů na každém uzlu ve fondu Azure Batch"
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05df4b7d8e0bc595168a97faa231b7c90fe81980
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-tasks-concurrently-toomaximize-usage-of-batch-compute-nodes"></a><span data-ttu-id="383e5-103">Spuštění úlohy souběžně toomaximize využití Batch výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="383e5-103">Run tasks concurrently toomaximize usage of Batch compute nodes</span></span> 

<span data-ttu-id="383e5-104">Při spuštění více než jeden úkol současně na každém výpočetním uzlu ve fondu Azure Batch, můžete zvýšit využití prostředků na menší počet uzlů ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="383e5-104">By running more than one task simultaneously on each compute node in your Azure Batch pool, you can maximize resource usage on a smaller number of nodes in hello pool.</span></span> <span data-ttu-id="383e5-105">Pro některé úlohy to může způsobit kratší časy úloh a nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="383e5-105">For some workloads, this can result in shorter job times and lower cost.</span></span>

<span data-ttu-id="383e5-106">Při některých scénářích těžit z vyhradit všechny uzlu prostředky tooa jeden úkol, několik situací využívat povolení více úloh tooshare tyto prostředky:</span><span class="sxs-lookup"><span data-stu-id="383e5-106">While some scenarios benefit from dedicating all of a node's resources tooa single task, several situations benefit from allowing multiple tasks tooshare those resources:</span></span>

* <span data-ttu-id="383e5-107">**Minimalizace přenos dat** úkoly, které mají mít tooshare data.</span><span class="sxs-lookup"><span data-stu-id="383e5-107">**Minimizing data transfer** when tasks are able tooshare data.</span></span> <span data-ttu-id="383e5-108">V tomto scénáři můžete výrazně snížit poplatky za přenos dat tak, že zkopírujete sdílených dat tooa menší počet uzlů a provádění úloh paralelně na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="383e5-108">In this scenario, you can dramatically reduce data transfer charges by copying shared data tooa smaller number of nodes and executing tasks in parallel on each node.</span></span> <span data-ttu-id="383e5-109">To platí hlavně Pokud uzel zkopírovaný tooeach toobe hello dat je třeba přenést mezi zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="383e5-109">This especially applies if hello data toobe copied tooeach node must be transferred between geographic regions.</span></span>
* <span data-ttu-id="383e5-110">**Tím se maximalizuje využití paměti** při úlohy vyžadovat velké množství paměti, ale pouze během krátkodobě dobu a v proměnné dobu během provádění.</span><span class="sxs-lookup"><span data-stu-id="383e5-110">**Maximizing memory usage** when tasks require a large amount of memory, but only during short periods of time, and at variable times during execution.</span></span> <span data-ttu-id="383e5-111">Můžete využívat méně, ale větší, výpočetní uzly s více paměti tooefficiently zpracovat takové špičky.</span><span class="sxs-lookup"><span data-stu-id="383e5-111">You can employ fewer, but larger, compute nodes with more memory tooefficiently handle such spikes.</span></span> <span data-ttu-id="383e5-112">Tyto uzly by mít více úloh s paralelně na jednotlivých uzlech spuštěné, ale každý úkol by využívat bohaté paměti hello uzly v různých časech.</span><span class="sxs-lookup"><span data-stu-id="383e5-112">These nodes would have multiple tasks running in parallel on each node, but each task would take advantage of hello nodes' plentiful memory at different times.</span></span>
* <span data-ttu-id="383e5-113">**Zmírnění číslo omezení uzlu** při komunikaci mezi uzly vyžádáním v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-113">**Mitigating node number limits** when inter-node communication is required within a pool.</span></span> <span data-ttu-id="383e5-114">Fondy, které jsou nakonfigurované pro komunikaci mezi uzly v současné době jsou omezené too50 výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="383e5-114">Currently, pools configured for inter-node communication are limited too50 compute nodes.</span></span> <span data-ttu-id="383e5-115">Pokud každý uzel v takové fondu je možné tooexecute paralelních úkolů, můžete současně spustit větší počet úloh.</span><span class="sxs-lookup"><span data-stu-id="383e5-115">If each node in such a pool is able tooexecute tasks in parallel, a greater number of tasks can be executed simultaneously.</span></span>
* <span data-ttu-id="383e5-116">**Replikace místní výpočetního clusteru**, například když je nejprve přesunout tooAzure výpočetního prostředí.</span><span class="sxs-lookup"><span data-stu-id="383e5-116">**Replicating an on-premises compute cluster**, such as when you first move a compute environment tooAzure.</span></span> <span data-ttu-id="383e5-117">Pokud vaše aktuální řešení místní provede několik úkolů na výpočetním uzlu, můžete zvýšit maximální počet hello uzlu úloh toomore úzce zrcadlení pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="383e5-117">If your current on-premises solution executes multiple tasks per compute node, you can increase hello maximum number of node tasks toomore closely mirror that configuration.</span></span>

## <a name="example-scenario"></a><span data-ttu-id="383e5-118">Příklad scénáře</span><span class="sxs-lookup"><span data-stu-id="383e5-118">Example scenario</span></span>
<span data-ttu-id="383e5-119">Jako tooillustrate příklad hello výhod provádění paralelních úkolů, Řekněme, že aplikace úkolů má požadavky na procesor a paměť tak, aby [standardní\_D1](../cloud-services/cloud-services-sizes-specs.md) postačí uzly.</span><span class="sxs-lookup"><span data-stu-id="383e5-119">As an example tooillustrate hello benefits of parallel task execution, let's say that your task application has CPU and memory requirements such that [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) nodes are sufficient.</span></span> <span data-ttu-id="383e5-120">Ale v pořadí úloh hello toofinish v čase hello vyžaduje, jsou potřebné 1000 tyto uzly.</span><span class="sxs-lookup"><span data-stu-id="383e5-120">But, in order toofinish hello job in hello required time, 1,000 of these nodes are needed.</span></span>

<span data-ttu-id="383e5-121">Místo použití standardní\_D1 uzly, které mají 1 jádro procesoru, můžete použít [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzlů, které mají 16 jader a povolit spouštění paralelních úloh.</span><span class="sxs-lookup"><span data-stu-id="383e5-121">Instead of using Standard\_D1 nodes that have 1 CPU core, you could use [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes that have 16 cores each, and enable parallel task execution.</span></span> <span data-ttu-id="383e5-122">Proto *16 časy méně uzly* může – místo 1000 uzly jen 63 by bylo zapotřebí.</span><span class="sxs-lookup"><span data-stu-id="383e5-122">Therefore, *16 times fewer nodes* could be used--instead of 1,000 nodes, only 63 would be required.</span></span> <span data-ttu-id="383e5-123">Kromě toho pokud aplikace velké soubory nebo referenční data jsou vyžadovány pro každý uzel, doba trvání úlohy a efektivitu znovu lepší vzhledem k tomu, že hello data je zkopírovaný tooonly 16 uzlů.</span><span class="sxs-lookup"><span data-stu-id="383e5-123">Additionally, if large application files or reference data are required for each node, job duration and efficiency are again improved since hello data is copied tooonly 16 nodes.</span></span>

## <a name="enable-parallel-task-execution"></a><span data-ttu-id="383e5-124">Povolit provedení paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="383e5-124">Enable parallel task execution</span></span>
<span data-ttu-id="383e5-125">Výpočetní uzly pro provedení paralelní úlohy nakonfigurujete na úrovni fondu hello.</span><span class="sxs-lookup"><span data-stu-id="383e5-125">You configure compute nodes for parallel task execution at hello pool level.</span></span> <span data-ttu-id="383e5-126">Pomocí knihovny Batch .NET hello, nastavte hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost při vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-126">With hello Batch .NET library, set hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property when you create a pool.</span></span> <span data-ttu-id="383e5-127">Pokud používáte hello Batch REST API, nastavte hello [maxTasksPerNode] [ rest_addpool] element v textu žádosti hello během vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-127">If you are using hello Batch REST API, set hello [maxTasksPerNode][rest_addpool] element in hello request body during pool creation.</span></span>

<span data-ttu-id="383e5-128">Azure Batch vám umožní tooset maximální úkolů na uzel nahoru toofour časy (4 x) hello počet jader na uzel.</span><span class="sxs-lookup"><span data-stu-id="383e5-128">Azure Batch allows you tooset maximum tasks per node up toofour times (4x) hello number of node cores.</span></span> <span data-ttu-id="383e5-129">Například pokud hello fond je nakonfigurován s uzly velikosti "Velké" (4 jádra), pak `maxTasksPerNode` může být nastaven too16.</span><span class="sxs-lookup"><span data-stu-id="383e5-129">For example, if hello pool is configured with nodes of size "Large" (four cores), then `maxTasksPerNode` may be set too16.</span></span> <span data-ttu-id="383e5-130">Podrobnosti hello počtu jader pro každý uzel velikostí hello najdete v tématu [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="383e5-130">For details on hello number of cores for each of hello node sizes, see [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span> <span data-ttu-id="383e5-131">Další informace o omezení služby najdete v tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="383e5-131">For more information on service limits, see [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span>

> [!TIP]
> <span data-ttu-id="383e5-132">Být jisti tootake do účtu hello `maxTasksPerNode` hodnota při vytváření [vzorec škálování] [ enable_autoscaling] fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-132">Be sure tootake into account hello `maxTasksPerNode` value when you construct an [autoscale formula][enable_autoscaling] for your pool.</span></span> <span data-ttu-id="383e5-133">Například vzorec, jehož výsledkem `$RunningTasks` může mít vliv výrazně zvýšit úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="383e5-133">For example, a formula that evaluates `$RunningTasks` could be dramatically affected by an increase in tasks per node.</span></span> <span data-ttu-id="383e5-134">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="383e5-134">See [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) for more information.</span></span>
>
>

## <a name="distribution-of-tasks"></a><span data-ttu-id="383e5-135">Rozdělení úkolů</span><span class="sxs-lookup"><span data-stu-id="383e5-135">Distribution of tasks</span></span>
<span data-ttu-id="383e5-136">Když hello výpočetních uzlů ve fondu, můžete současně provést úlohy, je důležité toospecify způsob toobe úlohy hello rozdělené mezi uzly hello ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="383e5-136">When hello compute nodes in a pool can execute tasks concurrently, it's important toospecify how you want hello tasks toobe distributed across hello nodes in hello pool.</span></span>

<span data-ttu-id="383e5-137">Pomocí hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] vlastnost, můžete určit, že úlohy by se měla přiřadit rovnoměrně mezi všechny uzly ve fondu hello ("rozšíření").</span><span class="sxs-lookup"><span data-stu-id="383e5-137">By using hello [CloudPool.TaskSchedulingPolicy][task_schedule] property, you can specify that tasks should be assigned evenly across all nodes in hello pool ("spreading").</span></span> <span data-ttu-id="383e5-138">Nebo můžete zadat, aby co nejvíce úkolů by se měla přiřadit tooeach uzlu před úlohy jsou přiřazeny tooanother uzlu ve fondu hello ("dodací").</span><span class="sxs-lookup"><span data-stu-id="383e5-138">Or you can specify that as many tasks as possible should be assigned tooeach node before tasks are assigned tooanother node in hello pool ("packing").</span></span>

<span data-ttu-id="383e5-139">Jako příklad, jak tato funkce se hodí v situaci, zvažte hello fond [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzly (v předchozím příkladu hello), které je nakonfigurované [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] hodnotu 16.</span><span class="sxs-lookup"><span data-stu-id="383e5-139">As an example of how this feature is valuable, consider hello pool of [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes (in hello example above) that is configured with a [CloudPool.MaxTasksPerComputeNode][maxtasks_net] value of 16.</span></span> <span data-ttu-id="383e5-140">Pokud hello [CloudPool.TaskSchedulingPolicy] [ task_schedule] nastavena [ComputeNodeFillType] [ fill_type] z *Pack*, by maximalizovat využití všech 16 jader každého uzlu a povolit [automatické škálování fondu](batch-automatic-scaling.md) tooprune nepoužívané uzly z fondu hello (uzlů bez přiřazeny žádné úkoly).</span><span class="sxs-lookup"><span data-stu-id="383e5-140">If hello [CloudPool.TaskSchedulingPolicy][task_schedule] is configured with a [ComputeNodeFillType][fill_type] of *Pack*, it would maximize usage of all 16 cores of each node and allow an [autoscaling pool](batch-automatic-scaling.md) tooprune unused nodes from hello pool (nodes without any tasks assigned).</span></span> <span data-ttu-id="383e5-141">To snižuje využití prostředků a úsporu nákladů.</span><span class="sxs-lookup"><span data-stu-id="383e5-141">This minimizes resource usage and saves money.</span></span>

## <a name="batch-net-example"></a><span data-ttu-id="383e5-142">Příklad batch .NET</span><span class="sxs-lookup"><span data-stu-id="383e5-142">Batch .NET example</span></span>
<span data-ttu-id="383e5-143">To [Batch .NET] [ api_net] fragment kódu rozhraní API ukazuje toocreate žádost o fond, který obsahuje čtyři uzly velké s maximálně čtyřmi úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="383e5-143">This [Batch .NET][api_net] API code snippet shows a request toocreate a pool that contains four large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="383e5-144">Určuje úlohu plánování zásad, který naplní každý uzel s úlohy předchozí tooassigning úlohy tooanother uzlu ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="383e5-144">It specifies a task scheduling policy that will fill each node with tasks prior tooassigning tasks tooanother node in hello pool.</span></span> <span data-ttu-id="383e5-145">Další informace o přidání fondů pomocí hello Batch .NET API najdete v tématu [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span><span class="sxs-lookup"><span data-stu-id="383e5-145">For more information on adding pools by using hello Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span></span>

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a><span data-ttu-id="383e5-146">Příklad batch REST</span><span class="sxs-lookup"><span data-stu-id="383e5-146">Batch REST example</span></span>
<span data-ttu-id="383e5-147">To [Batch REST] [ api_rest] rozhraní API fragment kódu ukazuje toocreate žádost o fond, který obsahuje dva uzly velké s maximálně čtyřmi úkolů na uzel.</span><span class="sxs-lookup"><span data-stu-id="383e5-147">This [Batch REST][api_rest] API snippet shows a request toocreate a pool that contains two large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="383e5-148">Další informace o přidání fondů pomocí hello REST API najdete v tématu [přidat účet fondu tooan][rest_addpool].</span><span class="sxs-lookup"><span data-stu-id="383e5-148">For more information on adding pools by using hello REST API, see [Add a pool tooan account][rest_addpool].</span></span>

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> <span data-ttu-id="383e5-149">Můžete nastavit hello `maxTasksPerNode` elementu a [MaxTasksPerComputeNode] [ maxtasks_net] vlastnost pouze při vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-149">You can set hello `maxTasksPerNode` element and [MaxTasksPerComputeNode][maxtasks_net] property only at pool creation time.</span></span> <span data-ttu-id="383e5-150">Nemůže být upraven po fond již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="383e5-150">They cannot be modified after a pool has already been created.</span></span>
>
>

## <a name="code-sample"></a><span data-ttu-id="383e5-151">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="383e5-151">Code sample</span></span>
<span data-ttu-id="383e5-152">Hello [ParallelNodeTasks] [ parallel_tasks_sample] projektu na Githubu ukazuje použití hello hello [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="383e5-152">hello [ParallelNodeTasks][parallel_tasks_sample] project on GitHub illustrates hello use of hello [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property.</span></span>

<span data-ttu-id="383e5-153">Tato Konzolová aplikace C# používá hello [Batch .NET] [ api_net] knihovny toocreate fond s jednou nebo několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="383e5-153">This C# console application uses hello [Batch .NET][api_net] library toocreate a pool with one or more compute nodes.</span></span> <span data-ttu-id="383e5-154">Konfigurovat řadu úloh se provede na těchto uzlech toosimulate proměnné zatížení.</span><span class="sxs-lookup"><span data-stu-id="383e5-154">It executes a configurable number of tasks on those nodes toosimulate variable load.</span></span> <span data-ttu-id="383e5-155">Výstup z aplikace hello Určuje, které uzly provést každý úkol.</span><span class="sxs-lookup"><span data-stu-id="383e5-155">Output from hello application specifies which nodes executed each task.</span></span> <span data-ttu-id="383e5-156">aplikace Hello také poskytuje souhrn hello parametry úlohy a doba trvání.</span><span class="sxs-lookup"><span data-stu-id="383e5-156">hello application also provides a summary of hello job parameters and duration.</span></span> <span data-ttu-id="383e5-157">Souhrn část Hello hello výstupu ze dvou různých spustí hello ukázkové aplikace se zobrazí pod.</span><span class="sxs-lookup"><span data-stu-id="383e5-157">hello summary portion of hello output from two different runs of hello sample application appears below.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

<span data-ttu-id="383e5-158">první spuštění ukázkové aplikace hello Hello ukazuje, že se do jednoho uzlu v hello fondu a výchozí nastavení hello úkolů na uzel, dobu trvání úlohy hello je více než 30 minut.</span><span class="sxs-lookup"><span data-stu-id="383e5-158">hello first execution of hello sample application shows that with a single node in hello pool and hello default setting of one task per node, hello job duration is over 30 minutes.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

<span data-ttu-id="383e5-159">Hello druhý spuštění hello ukázka ukazuje výrazného poklesu dobu trvání úlohy.</span><span class="sxs-lookup"><span data-stu-id="383e5-159">hello second run of hello sample shows a significant decrease in job duration.</span></span> <span data-ttu-id="383e5-160">To je proto hello fondu je nakonfigurovaná s čtyři úkolů na uzlu, který umožňuje pro paralelní úloha spuštění toocomplete hello úlohu v téměř čtvrtletí hello času.</span><span class="sxs-lookup"><span data-stu-id="383e5-160">This is because hello pool was configured with four tasks per node, which allows for parallel task execution toocomplete hello job in nearly a quarter of hello time.</span></span>

> [!NOTE]
> <span data-ttu-id="383e5-161">Hello dob trvání úlohy v hello souhrny výše nezahrnují čas vytvoření fondu.</span><span class="sxs-lookup"><span data-stu-id="383e5-161">hello job durations in hello summaries above do not include pool creation time.</span></span> <span data-ttu-id="383e5-162">Každá z výše uvedených hello úloh byla odeslaná toopreviously vytvořit fondy, jejichž výpočetní uzly se hello *nečinnosti* během odesílání stavu.</span><span class="sxs-lookup"><span data-stu-id="383e5-162">Each of hello jobs above was submitted toopreviously created pools whose compute nodes were in hello *Idle* state at submission time.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="383e5-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="383e5-163">Next steps</span></span>
### <a name="batch-explorer-heat-map"></a><span data-ttu-id="383e5-164">Batch Explorer Heat mapa</span><span class="sxs-lookup"><span data-stu-id="383e5-164">Batch Explorer Heat Map</span></span>
<span data-ttu-id="383e5-165">Hello [Průzkumník Azure Batch][batch_explorer], jeden z hello Azure Batch [ukázkové aplikace][github_samples], obsahuje *Heat mapa* funkce, která poskytuje vizualizaci provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="383e5-165">hello [Azure Batch Explorer][batch_explorer], one of hello Azure Batch [sample applications][github_samples], contains a *Heat Map* feature that provides visualization of task execution.</span></span> <span data-ttu-id="383e5-166">Pokud jste provádění hello [ParallelTasks] [ parallel_tasks_sample] ukázkovou aplikaci, můžete použít hello Heat mapa funkce tooeasily vizualizovat hello provádění paralelních úloh na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="383e5-166">When you're executing hello [ParallelTasks][parallel_tasks_sample] sample application, you can use hello Heat Map feature tooeasily visualize hello execution of parallel tasks on each node.</span></span>

![Batch Explorer Heat mapa][1]

<span data-ttu-id="383e5-168">*Batch Explorer Heat mapa zobrazuje fond čtyři uzly se každý uzel aktuálně spuštěných čtyři úlohy*</span><span class="sxs-lookup"><span data-stu-id="383e5-168">*Batch Explorer Heat Map showing a pool of four nodes, with each node currently executing four tasks*</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
