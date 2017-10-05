---
title: "Spuštění úlohy paralelní efektivně - používat výpočetní prostředky Azure Batch | Microsoft Docs"
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
ms.openlocfilehash: 6903552d907a1ddb21d3b678e2d224b4b5e35b77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a><span data-ttu-id="53522-103">Spuštění úloh souběžně chcete maximalizovat využití služby Batch výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="53522-103">Run tasks concurrently to maximize usage of Batch compute nodes</span></span> 

<span data-ttu-id="53522-104">Při spuštění více než jeden úkol současně na každém výpočetním uzlu ve fondu Azure Batch, můžete zvýšit využití prostředků na menší počet uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-104">By running more than one task simultaneously on each compute node in your Azure Batch pool, you can maximize resource usage on a smaller number of nodes in the pool.</span></span> <span data-ttu-id="53522-105">Pro některé úlohy to může způsobit kratší časy úloh a nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="53522-105">For some workloads, this can result in shorter job times and lower cost.</span></span>

<span data-ttu-id="53522-106">Při některých scénářích využívat všechny prostředky uzlu vyhradit pro jeden úkol, několika situacích těžit z povolení více úkolů ke sdílení těchto prostředků:</span><span class="sxs-lookup"><span data-stu-id="53522-106">While some scenarios benefit from dedicating all of a node's resources to a single task, several situations benefit from allowing multiple tasks to share those resources:</span></span>

* <span data-ttu-id="53522-107">**Minimalizace přenos dat** úkoly, které mají moci sdílet data.</span><span class="sxs-lookup"><span data-stu-id="53522-107">**Minimizing data transfer** when tasks are able to share data.</span></span> <span data-ttu-id="53522-108">V tomto scénáři můžete výrazně snížit poplatky za přenos dat tak, že sdílená data na menší počet uzlů a provádění úloh paralelně na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="53522-108">In this scenario, you can dramatically reduce data transfer charges by copying shared data to a smaller number of nodes and executing tasks in parallel on each node.</span></span> <span data-ttu-id="53522-109">To platí hlavně pokud data, která mají být zkopírovány do každého uzlu musí být přenesena mezi zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="53522-109">This especially applies if the data to be copied to each node must be transferred between geographic regions.</span></span>
* <span data-ttu-id="53522-110">**Tím se maximalizuje využití paměti** při úlohy vyžadovat velké množství paměti, ale pouze během krátkodobě dobu a v proměnné dobu během provádění.</span><span class="sxs-lookup"><span data-stu-id="53522-110">**Maximizing memory usage** when tasks require a large amount of memory, but only during short periods of time, and at variable times during execution.</span></span> <span data-ttu-id="53522-111">Můžete použít méně, ale větší, výpočetní uzly s vyšším rozsahem paměti pro efektivní zpracování takové špičky.</span><span class="sxs-lookup"><span data-stu-id="53522-111">You can employ fewer, but larger, compute nodes with more memory to efficiently handle such spikes.</span></span> <span data-ttu-id="53522-112">Tyto uzly by mít více úloh s paralelně na jednotlivých uzlech spuštěné, ale každý úkol by využívat bohaté paměti uzlů v různých časech.</span><span class="sxs-lookup"><span data-stu-id="53522-112">These nodes would have multiple tasks running in parallel on each node, but each task would take advantage of the nodes' plentiful memory at different times.</span></span>
* <span data-ttu-id="53522-113">**Zmírnění číslo omezení uzlu** při komunikaci mezi uzly vyžádáním v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-113">**Mitigating node number limits** when inter-node communication is required within a pool.</span></span> <span data-ttu-id="53522-114">Fondy, které jsou nakonfigurované pro komunikaci mezi uzly v současné době jsou omezeny na 50 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="53522-114">Currently, pools configured for inter-node communication are limited to 50 compute nodes.</span></span> <span data-ttu-id="53522-115">Pokud každý uzel v takové fondu je provést v paralelní úlohy, větší počet úloh mohou být provedeny souběžně.</span><span class="sxs-lookup"><span data-stu-id="53522-115">If each node in such a pool is able to execute tasks in parallel, a greater number of tasks can be executed simultaneously.</span></span>
* <span data-ttu-id="53522-116">**Replikace místní výpočetního clusteru**, například když je nejprve přesunout výpočetním prostředí do Azure.</span><span class="sxs-lookup"><span data-stu-id="53522-116">**Replicating an on-premises compute cluster**, such as when you first move a compute environment to Azure.</span></span> <span data-ttu-id="53522-117">Pokud vaše aktuální řešení místní provede několik úkolů na výpočetním uzlu, můžete zvýšit maximální počet úkolů uzel pro přesněji zrcadlení pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="53522-117">If your current on-premises solution executes multiple tasks per compute node, you can increase the maximum number of node tasks to more closely mirror that configuration.</span></span>

## <a name="example-scenario"></a><span data-ttu-id="53522-118">Příklad scénáře</span><span class="sxs-lookup"><span data-stu-id="53522-118">Example scenario</span></span>
<span data-ttu-id="53522-119">Jako příklad k objasnění výhod provedení paralelní úlohy Řekněme, že aplikace úkolů má požadavky na procesor a paměť tak, aby [standardní\_D1](../cloud-services/cloud-services-sizes-specs.md) postačí uzly.</span><span class="sxs-lookup"><span data-stu-id="53522-119">As an example to illustrate the benefits of parallel task execution, let's say that your task application has CPU and memory requirements such that [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) nodes are sufficient.</span></span> <span data-ttu-id="53522-120">Ale, aby bylo možné dokončit úlohy v požadované době, jsou potřeba 1000 tyto uzly.</span><span class="sxs-lookup"><span data-stu-id="53522-120">But, in order to finish the job in the required time, 1,000 of these nodes are needed.</span></span>

<span data-ttu-id="53522-121">Místo použití standardní\_D1 uzly, které mají 1 jádro procesoru, můžete použít [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzlů, které mají 16 jader a povolit spouštění paralelních úloh.</span><span class="sxs-lookup"><span data-stu-id="53522-121">Instead of using Standard\_D1 nodes that have 1 CPU core, you could use [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes that have 16 cores each, and enable parallel task execution.</span></span> <span data-ttu-id="53522-122">Proto *16 časy méně uzly* může – místo 1000 uzly jen 63 by bylo zapotřebí.</span><span class="sxs-lookup"><span data-stu-id="53522-122">Therefore, *16 times fewer nodes* could be used--instead of 1,000 nodes, only 63 would be required.</span></span> <span data-ttu-id="53522-123">Kromě toho pokud aplikace velké soubory nebo referenční data jsou vyžadovány pro každý uzel, doba trvání úlohy a efektivitu znovu lepší vzhledem k tomu, že data budou zkopírována do pouze 16 uzlů.</span><span class="sxs-lookup"><span data-stu-id="53522-123">Additionally, if large application files or reference data are required for each node, job duration and efficiency are again improved since the data is copied to only 16 nodes.</span></span>

## <a name="enable-parallel-task-execution"></a><span data-ttu-id="53522-124">Povolit provedení paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="53522-124">Enable parallel task execution</span></span>
<span data-ttu-id="53522-125">Výpočetní uzly pro provedení paralelní úlohy nakonfigurujete na úrovni fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-125">You configure compute nodes for parallel task execution at the pool level.</span></span> <span data-ttu-id="53522-126">Pomocí knihovny Batch .NET, nastavte [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost při vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-126">With the Batch .NET library, set the [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property when you create a pool.</span></span> <span data-ttu-id="53522-127">Pokud používáte rozhraní REST API služby Batch, nastavte [maxTasksPerNode] [ rest_addpool] element v těle žádosti během vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-127">If you are using the Batch REST API, set the [maxTasksPerNode][rest_addpool] element in the request body during pool creation.</span></span>

<span data-ttu-id="53522-128">Azure Batch umožňuje nastavit maximální úkolů na uzel až čtyřikrát (4 x) počet jader na uzel.</span><span class="sxs-lookup"><span data-stu-id="53522-128">Azure Batch allows you to set maximum tasks per node up to four times (4x) the number of node cores.</span></span> <span data-ttu-id="53522-129">Například pokud fondu je nakonfigurovaný s uzly velikost "Velké" (4 jádra), pak `maxTasksPerNode` může být nastaven na hodnotu 16.</span><span class="sxs-lookup"><span data-stu-id="53522-129">For example, if the pool is configured with nodes of size "Large" (four cores), then `maxTasksPerNode` may be set to 16.</span></span> <span data-ttu-id="53522-130">Podrobnosti na počtu jader pro každý uzel velikosti najdete v tématu [velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="53522-130">For details on the number of cores for each of the node sizes, see [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md).</span></span> <span data-ttu-id="53522-131">Další informace o omezení služby najdete v tématu [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="53522-131">For more information on service limits, see [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span>

> [!TIP]
> <span data-ttu-id="53522-132">Je nutné vzít v úvahu `maxTasksPerNode` hodnota při vytváření [vzorec škálování] [ enable_autoscaling] fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-132">Be sure to take into account the `maxTasksPerNode` value when you construct an [autoscale formula][enable_autoscaling] for your pool.</span></span> <span data-ttu-id="53522-133">Například vzorec, jehož výsledkem `$RunningTasks` může mít vliv výrazně zvýšit úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="53522-133">For example, a formula that evaluates `$RunningTasks` could be dramatically affected by an increase in tasks per node.</span></span> <span data-ttu-id="53522-134">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="53522-134">See [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) for more information.</span></span>
>
>

## <a name="distribution-of-tasks"></a><span data-ttu-id="53522-135">Rozdělení úkolů</span><span class="sxs-lookup"><span data-stu-id="53522-135">Distribution of tasks</span></span>
<span data-ttu-id="53522-136">Když výpočetní uzly ve fondu, můžete současně provést úlohy, je důležité určit, jak má úkoly, které mají být rozdělené mezi uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-136">When the compute nodes in a pool can execute tasks concurrently, it's important to specify how you want the tasks to be distributed across the nodes in the pool.</span></span>

<span data-ttu-id="53522-137">Pomocí [CloudPool.TaskSchedulingPolicy] [ task_schedule] vlastnost, můžete určit, že úlohy by se měla přiřadit rovnoměrně mezi všechny uzly ve fondu ("rozšíření").</span><span class="sxs-lookup"><span data-stu-id="53522-137">By using the [CloudPool.TaskSchedulingPolicy][task_schedule] property, you can specify that tasks should be assigned evenly across all nodes in the pool ("spreading").</span></span> <span data-ttu-id="53522-138">Nebo můžete zadat, aby co nejvíce úkolů by měla být přiřazená každý uzel před úlohy jsou přiřazeny do jiného uzlu ve fondu ("okolních").</span><span class="sxs-lookup"><span data-stu-id="53522-138">Or you can specify that as many tasks as possible should be assigned to each node before tasks are assigned to another node in the pool ("packing").</span></span>

<span data-ttu-id="53522-139">Jako příklad, jak tato funkce se hodí v situaci, zvažte fondu [standardní\_D14](../cloud-services/cloud-services-sizes-specs.md) uzly (v předchozím příkladu), které je nakonfigurované [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] hodnotu 16.</span><span class="sxs-lookup"><span data-stu-id="53522-139">As an example of how this feature is valuable, consider the pool of [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md) nodes (in the example above) that is configured with a [CloudPool.MaxTasksPerComputeNode][maxtasks_net] value of 16.</span></span> <span data-ttu-id="53522-140">Pokud [CloudPool.TaskSchedulingPolicy] [ task_schedule] nastavena [ComputeNodeFillType] [ fill_type] z *Pack*, by maximalizovat využití všech 16 jader každého uzlu a povolit [automatické škálování fondu](batch-automatic-scaling.md) Chcete-li vyřadit nepoužívá uzly z fondu (uzlů bez přiřazeny žádné úkoly).</span><span class="sxs-lookup"><span data-stu-id="53522-140">If the [CloudPool.TaskSchedulingPolicy][task_schedule] is configured with a [ComputeNodeFillType][fill_type] of *Pack*, it would maximize usage of all 16 cores of each node and allow an [autoscaling pool](batch-automatic-scaling.md) to prune unused nodes from the pool (nodes without any tasks assigned).</span></span> <span data-ttu-id="53522-141">To snižuje využití prostředků a úsporu nákladů.</span><span class="sxs-lookup"><span data-stu-id="53522-141">This minimizes resource usage and saves money.</span></span>

## <a name="batch-net-example"></a><span data-ttu-id="53522-142">Příklad batch .NET</span><span class="sxs-lookup"><span data-stu-id="53522-142">Batch .NET example</span></span>
<span data-ttu-id="53522-143">To [Batch .NET] [ api_net] rozhraní API fragment kódu ukazuje požadavek na vytvoření fondu, který obsahuje čtyři uzly velké s maximálně čtyřmi úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="53522-143">This [Batch .NET][api_net] API code snippet shows a request to create a pool that contains four large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="53522-144">Určuje úlohu plánování zásad, který naplní každý uzel úkolech před přiřazením úkolů do jiného uzlu ve fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-144">It specifies a task scheduling policy that will fill each node with tasks prior to assigning tasks to another node in the pool.</span></span> <span data-ttu-id="53522-145">Další informace o přidání fondů pomocí rozhraní Batch .NET API najdete v tématu [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span><span class="sxs-lookup"><span data-stu-id="53522-145">For more information on adding pools by using the Batch .NET API, see [BatchClient.PoolOperations.CreatePool][poolcreate_net].</span></span>

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

## <a name="batch-rest-example"></a><span data-ttu-id="53522-146">Příklad batch REST</span><span class="sxs-lookup"><span data-stu-id="53522-146">Batch REST example</span></span>
<span data-ttu-id="53522-147">To [Batch REST] [ api_rest] rozhraní API fragment kódu ukazuje požadavek na vytvoření fondu, který obsahuje dva uzly velké s maximálně čtyřmi úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="53522-147">This [Batch REST][api_rest] API snippet shows a request to create a pool that contains two large nodes with a maximum of four tasks per node.</span></span> <span data-ttu-id="53522-148">Další informace o přidání fondů pomocí rozhraní REST API najdete v tématu [přidat fond na účet][rest_addpool].</span><span class="sxs-lookup"><span data-stu-id="53522-148">For more information on adding pools by using the REST API, see [Add a pool to an account][rest_addpool].</span></span>

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
> <span data-ttu-id="53522-149">Můžete nastavit `maxTasksPerNode` elementu a [MaxTasksPerComputeNode] [ maxtasks_net] vlastnost pouze při vytváření fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-149">You can set the `maxTasksPerNode` element and [MaxTasksPerComputeNode][maxtasks_net] property only at pool creation time.</span></span> <span data-ttu-id="53522-150">Nemůže být upraven po fond již byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="53522-150">They cannot be modified after a pool has already been created.</span></span>
>
>

## <a name="code-sample"></a><span data-ttu-id="53522-151">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="53522-151">Code sample</span></span>
<span data-ttu-id="53522-152">[ParallelNodeTasks] [ parallel_tasks_sample] projektu na Githubu ukazuje použití [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="53522-152">The [ParallelNodeTasks][parallel_tasks_sample] project on GitHub illustrates the use of the [CloudPool.MaxTasksPerComputeNode][maxtasks_net] property.</span></span>

<span data-ttu-id="53522-153">Tato Konzolová aplikace C# používá [Batch .NET] [ api_net] knihovny a vytvoření fondu pomocí jednoho nebo více výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="53522-153">This C# console application uses the [Batch .NET][api_net] library to create a pool with one or more compute nodes.</span></span> <span data-ttu-id="53522-154">Konfigurovat řadu úloh se provede na těchto uzlech k simulaci proměnné zatížení.</span><span class="sxs-lookup"><span data-stu-id="53522-154">It executes a configurable number of tasks on those nodes to simulate variable load.</span></span> <span data-ttu-id="53522-155">Výstup z aplikace určuje, které uzly provést každý úkol.</span><span class="sxs-lookup"><span data-stu-id="53522-155">Output from the application specifies which nodes executed each task.</span></span> <span data-ttu-id="53522-156">Aplikace také poskytuje souhrn parametry úlohy a doba trvání.</span><span class="sxs-lookup"><span data-stu-id="53522-156">The application also provides a summary of the job parameters and duration.</span></span> <span data-ttu-id="53522-157">Souhrn část výstup ze dvou různých spuštění ukázkové aplikace se zobrazí pod.</span><span class="sxs-lookup"><span data-stu-id="53522-157">The summary portion of the output from two different runs of the sample application appears below.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

<span data-ttu-id="53522-158">První spuštění ukázkové aplikace ukazuje, že se jeden uzel ve fondu a výchozí nastavení pro jeden úkol na uzlu, že dobu trvání úlohy je více než 30 minut.</span><span class="sxs-lookup"><span data-stu-id="53522-158">The first execution of the sample application shows that with a single node in the pool and the default setting of one task per node, the job duration is over 30 minutes.</span></span>

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

<span data-ttu-id="53522-159">Druhý spuštění ukázkové ukazuje výrazného poklesu dobu trvání úlohy.</span><span class="sxs-lookup"><span data-stu-id="53522-159">The second run of the sample shows a significant decrease in job duration.</span></span> <span data-ttu-id="53522-160">Je to proto, že fond byl nakonfigurován s čtyři úkolů na uzlu, který umožňuje provádění paralelních úkolů dokončete úlohu v téměř čtvrtletí času.</span><span class="sxs-lookup"><span data-stu-id="53522-160">This is because the pool was configured with four tasks per node, which allows for parallel task execution to complete the job in nearly a quarter of the time.</span></span>

> [!NOTE]
> <span data-ttu-id="53522-161">Doby trvání úlohy ve výše uvedené souhrny nezahrnují čas vytvoření fondu.</span><span class="sxs-lookup"><span data-stu-id="53522-161">The job durations in the summaries above do not include pool creation time.</span></span> <span data-ttu-id="53522-162">Každá z výše uvedených úloh byla odeslána na dříve vytvořenou fondů, jejichž výpočetní uzly se *nečinnosti* během odesílání stavu.</span><span class="sxs-lookup"><span data-stu-id="53522-162">Each of the jobs above was submitted to previously created pools whose compute nodes were in the *Idle* state at submission time.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="53522-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53522-163">Next steps</span></span>
### <a name="batch-explorer-heat-map"></a><span data-ttu-id="53522-164">Batch Explorer Heat mapa</span><span class="sxs-lookup"><span data-stu-id="53522-164">Batch Explorer Heat Map</span></span>
<span data-ttu-id="53522-165">[Průzkumník Azure Batch][batch_explorer], jeden z Azure Batch [ukázkové aplikace][github_samples], obsahuje *Heat mapa* funkce, která poskytuje vizualizaci provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="53522-165">The [Azure Batch Explorer][batch_explorer], one of the Azure Batch [sample applications][github_samples], contains a *Heat Map* feature that provides visualization of task execution.</span></span> <span data-ttu-id="53522-166">Pokud jste provádění [ParallelTasks] [ parallel_tasks_sample] ukázkovou aplikaci, můžete použít funkci Heat mapa můžete snadno vizualizovat provádění paralelních úloh na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="53522-166">When you're executing the [ParallelTasks][parallel_tasks_sample] sample application, you can use the Heat Map feature to easily visualize the execution of parallel tasks on each node.</span></span>

![Batch Explorer Heat mapa][1]

<span data-ttu-id="53522-168">*Batch Explorer Heat mapa zobrazuje fond čtyři uzly se každý uzel aktuálně spuštěných čtyři úlohy*</span><span class="sxs-lookup"><span data-stu-id="53522-168">*Batch Explorer Heat Map showing a pool of four nodes, with each node currently executing four tasks*</span></span>

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
