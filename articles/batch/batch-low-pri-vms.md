---
title: "Spuštění úloh služby Azure Batch na virtuálních počítačích nákladově efektivní s nízkou prioritou (Preview) | Microsoft Docs"
description: "Zjistěte, jak zřídit virtuální počítače s nízkou prioritou snížit náklady na úloh služby Azure Batch."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.openlocfilehash: 9bf0ac322020d8a8453011c3207c1930175db6d3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="be582-103">Použití nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)</span><span class="sxs-lookup"><span data-stu-id="be582-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="be582-104">Azure Batch nabízí nízkou priorty virtuální počítače (VM) ke snížení nákladů úloh služby Batch.</span><span class="sxs-lookup"><span data-stu-id="be582-104">Azure Batch offers low-priorty virtual machines (VMs) to reduce the cost of Batch workloads.</span></span> <span data-ttu-id="be582-105">Virtuální počítače s nízkou prioritou umožňují nové typy úloh služby Batch, tím, že poskytuje velké množství výpočetního výkonu, která je také ekonomické.</span><span class="sxs-lookup"><span data-stu-id="be582-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="be582-106">Virtuální počítače s nízkou prioritou využít výhod kapacitní v Azure.</span><span class="sxs-lookup"><span data-stu-id="be582-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="be582-107">Když zadáte nízkou prioritu virtuálních počítačů ve fondech, Azure Batch mohou automaticky používat tento přebytek, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="be582-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="be582-108">Kompromis pro používání virtuálních počítačů s nízkou prioritou je, že tyto virtuální počítače může být zrušené při žádné kapacitní je k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="be582-108">The tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="be582-109">Z toho důvodu jsou nejvhodnější pro některé typy úloh virtuálních počítačů s nízkou prioritou.</span><span class="sxs-lookup"><span data-stu-id="be582-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="be582-110">Použijte virtuální počítače nízkou prioritu pro batch a asynchronní zpracování úloh, kde čas dokončení úlohy je flexibilní a práce je distribuován do mnoha virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be582-110">Use low-priority VMs for batch and asynchronous processing workloads where the job completion time is flexible and the work is distributed across many VMs.</span></span>

<span data-ttu-id="be582-111">Virtuální počítače s nízkou prioritou jsou výrazně levnější než vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="be582-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="be582-112">Podrobnosti o cenách najdete v části [ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="be582-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="be582-113">Další informace o virtuálních počítačích nízkou prioritu, najdete v blogovém příspěvku: [computing za zlomek ceny služby Batch](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="be582-113">For an additional discussion of low-priority VMs, see the blog post announcement: [Batch computing at a fraction of the price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be582-114">Nízkou prioritu virtuální počítače jsou aktuálně ve verzi preview a jsou dostupné pouze pro úlohy běžící v dávce.</span><span class="sxs-lookup"><span data-stu-id="be582-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="be582-115">Případy použití pro virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="be582-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="be582-116">Vzhledem k vlastnostem virtuálních počítačů, nízkou prioritu, co úlohy můžete a nelze je použít?</span><span class="sxs-lookup"><span data-stu-id="be582-116">Given the characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="be582-117">Obecně platí zpracování úloh služby batch jsou vhodné, úlohy jsou rozdělená do mnoho paralelních úloh nebo existuje mnoho úloh, které jsou škálovat na více systémů a distribuován do mnoha virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be582-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="be582-118">Pokud chcete maximalizovat využití přebytečných kapacity v Azure, můžete škálovat vhodný úlohy.</span><span class="sxs-lookup"><span data-stu-id="be582-118">To maximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="be582-119">Příležitostně virtuální počítače nemusí být k dispozici, nebo bude možné zrušené, který bude mít za následek menší kapacitu pro úlohy a může dojít k přerušení úloh a opakování.</span><span class="sxs-lookup"><span data-stu-id="be582-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead to task interruption and reruns.</span></span> <span data-ttu-id="be582-120">Proto musí být flexibilní v době, kterou můžete využít ke spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="be582-120">Jobs must therefore be flexible in the time they can take to run.</span></span>

-   <span data-ttu-id="be582-121">Úlohy se delší úloh může být ovlivněno více, pokud dojde k přerušení.</span><span class="sxs-lookup"><span data-stu-id="be582-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="be582-122">Pokud dlouho běžící, že úlohy implementovat vytváření kontrolních bodů uložit průběh jako jejich provedení, pak dopad přerušení bude mnohem menší.</span><span class="sxs-lookup"><span data-stu-id="be582-122">If long-running tasks implement checkpointing to save progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="be582-123">Úlohy s kratší časy spouštění zpravidla nejlépe fungovat s virtuálními počítači nízkou prioritu jako dopad přerušení je mnohem menší.</span><span class="sxs-lookup"><span data-stu-id="be582-123">Tasks with shorter execution times tend to work best with low-priority VMs as the impact of interruption is far less.</span></span>

-   <span data-ttu-id="be582-124">Dlouhotrvajících úloh MPI, které využívají víc virtuálních počítačů nejsou dobře hodí používat virtuální počítače s nízkou prioritou jako jeden zrušené virtuální počítač pravděpodobně povede k celé úlohy museli znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="be582-124">Long-running MPI jobs that utilize multiple VMs are not well suited to use low-priority VMs as one preempted VM will likely lead to the whole job having to be run again.</span></span>

<span data-ttu-id="be582-125">Některé příklady případy použití zpracování dávky dobře vhodné použít nízkou prioritu virtuální počítače jsou:</span><span class="sxs-lookup"><span data-stu-id="be582-125">Some examples of batch processing use cases well suited to use low-priority VMs are:</span></span>

-   <span data-ttu-id="be582-126">**Vývoj a testování**: zejména, pokud jsou vyvíjených rozsáhlé řešení výrazné úspory může být dosaženo.</span><span class="sxs-lookup"><span data-stu-id="be582-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="be582-127">Všechny typy testování může přinést výhody, ale ve velkém měřítku zátěžové testování a testování regrese jsou skvělý používá.</span><span class="sxs-lookup"><span data-stu-id="be582-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="be582-128">**Dodávání kapacity na vyžádání**: virtuální počítače s nízkou prioritou lze použít k doplnění regular vyhrazených virtuálních počítačích – Pokud je k dispozici, můžete škálovat a proto dokončení rychlejší nižší náklady úlohy; Pokud není k dispozici, je použito účaří vyhrazených virtuálních počítačích jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="be582-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, the baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="be582-129">**Čas spuštění úlohy flexibilní**: Pokud je flexibilitu při čas úlohy mají k dokončení, pak potenciální vyřazuje kapacity lze tolerovat, ale s přidáním virtuálních počítačů nízkou prioritu úlohy se často spustí rychleji a s nižšími náklady.</span><span class="sxs-lookup"><span data-stu-id="be582-129">**Flexible job execution time**: If there is flexibility in the time jobs have to complete, then potential drops in capacity can be tolerated; however, with the addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="be582-130">Fondy batch lze nakonfigurovat k využívání virtuálních počítačů nízkou prioritu několika různými způsoby v závislosti na flexibilitu v čase spuštění úlohy:</span><span class="sxs-lookup"><span data-stu-id="be582-130">Batch pools can be configured to use low-priority VMs in a few ways, depending on the flexibility in job execution time:</span></span>

-   <span data-ttu-id="be582-131">Virtuální počítače nízkou prioritu lze použít pouze ve fondu a Batch bude jednoduše obnovit všechny preempted kapacity, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="be582-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="be582-132">To je nejlevnější způsob k provedení úlohy se používají pouze nízkou prioritu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be582-132">This is the cheapest way to execute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="be582-133">Virtuální počítače s nízkou prioritou lze použít ve spojení s pevnou směrného plánu vyhrazených virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="be582-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="be582-134">Pevný počet vyhrazených virtuálních počítačích zajistí, že je vždy některé kapacity zachovat úlohy pokročíte.</span><span class="sxs-lookup"><span data-stu-id="be582-134">The fixed number of dedicated VMs ensures there is always some capacity to keep a job progressing.</span></span>

-   <span data-ttu-id="be582-135">Může být dynamické směs vyhrazené a nízkou prioritu virtuálních počítačů, tak, aby virtuální počítače levnějších nízkou prioritu se používají výhradně, pokud je k dispozici, ale jsou za cenu úplné vyhrazených virtuálních počítačích škálovat, aby minimální množství kapacity, které jsou k dispozici zachovat úlohy pokročíte vyžádání.</span><span class="sxs-lookup"><span data-stu-id="be582-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but the full-priced dedicated VMs are scaled up when required, to keep a minimum amount of capacity available to keep the jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="be582-136">Batch podporu pro virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="be582-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="be582-137">Azure Batch poskytuje několik možností, které usnadňují spotřebovávají a těžit z virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="be582-137">Azure Batch provides several capabilities that make it easy to consume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="be582-138">Fondy batch může obsahovat vyhrazených virtuálních počítačích a virtuální počítače s nízkou prioritou.</span><span class="sxs-lookup"><span data-stu-id="be582-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="be582-139">Počet jednotlivých typů virtuálních počítačů lze zadat, pokud fond je vytvořené nebo změněné kdykoli pro existující fond, pomocí operace změny velikosti explicitní nebo pomocí automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="be582-139">The number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using the explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="be582-140">Odeslání úlohy a úlohy může zůstat beze změny a nemusíte dělat starosti s typy virtuálních počítačů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="be582-140">Job and task submission can remain unchanged and do not need to be concerned with the VM types in the pool.</span></span> <span data-ttu-id="be582-141">Je také možné, že fond úplně použít ke spuštění úloh levné jako možný, ale otočení až vyhrazených virtuálních počítačích, pokud je kapacita klesne pod minimální prahové hodnoty, aby úlohy spuštěné virtuální počítače s nízkou prioritou.</span><span class="sxs-lookup"><span data-stu-id="be582-141">It is also possible to have a pool completely use low-priority VMs to run jobs as cheaply as possible, but spin up dedicated VMs if the capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="be582-142">Fondy batch automaticky snaží cílový počet virtuálních počítačů nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="be582-142">Batch pools automatically seek to the target number of low-priority VMs.</span></span> <span data-ttu-id="be582-143">Pokud přerušené virtuální počítače, se pokusí Batch nahradit ztraceny kapacitu a vrátíte se k cíli.</span><span class="sxs-lookup"><span data-stu-id="be582-143">If VMs are preempted, then Batch will attempt to replace the lost capacity and return to the target.</span></span>

-   <span data-ttu-id="be582-144">V případě úloh se přerušení Batch rozpozná a automaticky znovu úlohy spustit znovu.</span><span class="sxs-lookup"><span data-stu-id="be582-144">In the case of tasks being interrupted, Batch will detect and automatically requeue tasks to be run again.</span></span>

-   <span data-ttu-id="be582-145">Virtuální počítače s nízkou prioritou mají kvóta na jádra, která se liší od vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="be582-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="be582-146">Uvozovky pro virtuální počítače s nízkou prioritou je vyšší než u vyhrazených virtuálních počítačích, protože virtuální počítače s nízkou prioritou nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="be582-146">The quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="be582-147">V tématu [Batch, kvóty a omezení služby](batch-quota-limit.md#resource-quotas) Další informace.</span><span class="sxs-lookup"><span data-stu-id="be582-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="be582-148">Virtuální počítače s nízkou prioritou nejsou aktuálně podporovány pro účty Batch, kde je nastaven režim přidělení fondu na [uživatele předplatné](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="be582-148">Low-priority VMs are not currently supported for Batch accounts where the pool allocation mode is set to [User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="be582-149">Vytváření a aktualizaci fondy</span><span class="sxs-lookup"><span data-stu-id="be582-149">Create and update pools</span></span>

<span data-ttu-id="be582-150">Fondu služby Batch může obsahovat vyhrazené a nízkou prioritu virtuálních počítačů (také označované jako výpočetní uzly).</span><span class="sxs-lookup"><span data-stu-id="be582-150">A Batch pool can contain both dedicated and low-priority VMs (also referred to as compute nodes).</span></span> <span data-ttu-id="be582-151">Pro vyhrazené i nízkou prioritu virtuální počítače můžete nastavit cílovým počtem výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="be582-151">You can set the target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="be582-152">Cílový počet uzlů určuje počet virtuálních počítačů, které chcete mít ve fondu.</span><span class="sxs-lookup"><span data-stu-id="be582-152">The target number of nodes specifies the number of VMs you want to have in the pool.</span></span>

<span data-ttu-id="be582-153">Například vytvoření fondu pomocí služby Azure cloud virtuálních počítačů s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="be582-153">For example, to create a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="be582-154">Vytvoření fondu pomocí virtuální počítače Azure (v tomto případě virtuální počítače s Linuxem) s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="be582-154">To create a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="be582-155">Aktuální počet uzlů můžete získat pro vyhrazené i nízkou prioritu virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="be582-155">You can get the current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="be582-156">Uzly fondu mají vlastnost označující, zda je uzel vyhrazené nebo nízkou prioritu virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="be582-156">Pool nodes have a property to indicate if the node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="be582-157">Přerušené jeden nebo více uzlů ve fondu, operace výpisu uzlů ve fondu pořád vrátí tyto uzly, aktuální počet uzlů nízkou prioritu zůstanou nezměněna, ale tyto uzly budou mít jejich stavu, nastavte na **přepnuto** stav.</span><span class="sxs-lookup"><span data-stu-id="be582-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, the current number of low-priority nodes will remain unchanged, but those nodes will have their state set to the **Preempted** state.</span></span> <span data-ttu-id="be582-158">Batch se pokusí najít nahrazení virtuální počítače, a pokud bylo úspěšné, bude projít uzly **vytváření** a potom **počáteční** stavy poté jsou dostupné pro provádění úkolů, stejně jako nové uzly.</span><span class="sxs-lookup"><span data-stu-id="be582-158">Batch will attempt to find replacement VMs and, if successful, the nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="be582-159">Škálování fond, který obsahuje virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="be582-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="be582-160">Stejně jako u fondy výhradně skládající se z vyhrazených virtuálních počítačích, je možné škálovat obsahující virtuální počítače, nízkou prioritu, voláním metody změny velikosti nebo pomocí automatickému škálování fondu.</span><span class="sxs-lookup"><span data-stu-id="be582-160">As with pools solely consisting of dedicated VMs, it is possible to scale a pool containing low-priority VMs by calling the Resize method or by using auto-scale.</span></span>

<span data-ttu-id="be582-161">Operace změny velikosti fondu trvá druhý volitelný parametr, který aktualizuje hodnotu **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="be582-161">The pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="be582-162">Vzorec automatickému škálování fondu podporuje virtuální počítače s nízkou prioritou následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be582-162">The pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="be582-163">Můžete získat nebo nastavit hodnotu proměnné definované služby **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="be582-163">You can get or set the value of the service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="be582-164">Můžete získat hodnoty proměnné definované služby **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="be582-164">You can get the value of the service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="be582-165">Můžete získat hodnoty proměnné definované služby **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="be582-165">You can get the value of the service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="be582-166">Tato proměnná vrátí počet uzlů v preempted stavu a umožňuje škálování nahoru nebo dolů počet vyhrazených uzlů, v závislosti na počtu zrušené uzlů, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="be582-166">This variable returns the number of nodes in the preempted state and allows you to scale up or down the number of dedicated nodes, depending on the number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="be582-167">Úlohy a úkoly</span><span class="sxs-lookup"><span data-stu-id="be582-167">Jobs and tasks</span></span>

<span data-ttu-id="be582-168">Úlohy a úkoly vyžadují velmi malé podporu pro uzly nízkou prioritu; podporuje se jen vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="be582-168">Jobs and tasks require very little support for low-priority nodes; the only support is as follows:</span></span>

-   <span data-ttu-id="be582-169">Vlastnost JobManagerTask úlohy má novou vlastnost **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="be582-169">The JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="be582-170">Pokud tato vlastnost hodnotu true, úkolu Správce úloh bude naplánována na vyhrazené nebo nízkou prioritu uzlu.</span><span class="sxs-lookup"><span data-stu-id="be582-170">When this property is true, the job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="be582-171">Pokud je tato vlastnost hodnotu false, úkolu Správce úloh bude naplánována na vyhrazené uzel.</span><span class="sxs-lookup"><span data-stu-id="be582-171">If this property is false, the job manager task will be scheduled to a dedicated node only.</span></span>

-   <span data-ttu-id="be582-172">[Proměnnou prostředí](batch-compute-node-environment-variables.md) je k dispozici pro aplikace úkolů, aby mohla určit, zda je spuštěn na nízkou prioritu, nebo vyhrazený uzlu.</span><span class="sxs-lookup"><span data-stu-id="be582-172">An [environment variable](batch-compute-node-environment-variables.md) is available to a task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="be582-173">Proměnná prostředí je AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="be582-173">The environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="be582-174">Zpracování přerušení</span><span class="sxs-lookup"><span data-stu-id="be582-174">Handling preemption</span></span>

<span data-ttu-id="be582-175">Virtuální počítače může být někdy zrušené; v takovém případě Batch provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="be582-175">VMs may occasionally be preempted; when this happens, Batch does the following:</span></span>

-   <span data-ttu-id="be582-176">Preempted virtuální počítače mají jejich stav, aktualizovat, aby **přepnuto**.</span><span class="sxs-lookup"><span data-stu-id="be582-176">The preempted VMs have their state updated to **Preempted**.</span></span>
-   <span data-ttu-id="be582-177">Pokud úlohy byly spuštěné na virtuálních počítačích zrušené uzlu, jsou tyto úlohy zařazena a spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="be582-177">If tasks were running on the preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="be582-178">Virtuální počítač je efektivně odstranit, což všechna data uložená místně na virtuální počítač ke ztrátě.</span><span class="sxs-lookup"><span data-stu-id="be582-178">The VM is effectively deleted, leading to any data stored locally on the VM being lost.</span></span>
-   <span data-ttu-id="be582-179">Fond se průběžně pokusí kontaktovat cílový počet nízkou prioritu uzlů, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="be582-179">The pool continually attempts to reach the target number of low-priority nodes available.</span></span> <span data-ttu-id="be582-180">Když se najde náhradní kapacity uzly zachovat jejich ID, ale jsou znovu inicializovat, projít **vytváření** a **počáteční** stavy předtím, než jsou k dispozici pro plánování úkolů.</span><span class="sxs-lookup"><span data-stu-id="be582-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="be582-181">Přerušování počty jsou k dispozici jako metrika na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be582-181">Preemption counts are available as a metric in the Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="be582-182">Metriky</span><span class="sxs-lookup"><span data-stu-id="be582-182">Metrics</span></span>

<span data-ttu-id="be582-183">Jsou k dispozici v nové metriky [portál Azure](https://portal.azure.com) pro uzly nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="be582-183">New metrics are available in the [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="be582-184">Jsou tyto metriky:</span><span class="sxs-lookup"><span data-stu-id="be582-184">These metrics are:</span></span>

- <span data-ttu-id="be582-185">Počet uzlů s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="be582-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="be582-186">Počet jader s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="be582-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="be582-187">Zrušené počet uzlů</span><span class="sxs-lookup"><span data-stu-id="be582-187">Preempted Node Count</span></span>

<span data-ttu-id="be582-188">Chcete-li zobrazit metriky na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="be582-188">To view metrics in the Azure portal:</span></span>

1. <span data-ttu-id="be582-189">Přejděte na svůj účet Batch na portálu a zobrazit nastavení vašeho účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="be582-189">Navigate to your Batch account in the portal, and view the settings for your Batch account.</span></span>
2. <span data-ttu-id="be582-190">Vyberte **metriky** z **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="be582-190">Select **Metrics** from the **Monitoring** section.</span></span>
3. <span data-ttu-id="be582-191">Vybrat metriky, které mají **dostupné metriky** seznamu.</span><span class="sxs-lookup"><span data-stu-id="be582-191">Select the metrics you desire from the **Available Metrics** list.</span></span>

![Metriky pro uzly s nízkou prioritou](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="be582-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be582-193">Next steps</span></span>

* <span data-ttu-id="be582-194">Přečtěte si téma [Přehled funkcí Batch pro vývojáře](batch-api-basics.md), kde jsou základní informace pro každého, kdo se připravuje použít Batch.</span><span class="sxs-lookup"><span data-stu-id="be582-194">Read the [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing to use Batch.</span></span> <span data-ttu-id="be582-195">Článek obsahuje podrobné informace o prostředcích služby Batch, jako jsou fondy, uzly a úlohy, a mnoha funkcích rozhraní API, které můžete použít při vytváření aplikace Batch.</span><span class="sxs-lookup"><span data-stu-id="be582-195">The article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and the many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="be582-196">Další informace o dostupných [rozhraních API a nástrojích služby Batch](batch-apis-tools.md) pro sestavování řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="be582-196">Learn about the [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
