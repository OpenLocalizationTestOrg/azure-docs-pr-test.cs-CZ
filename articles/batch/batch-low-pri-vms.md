---
title: "úloh služby Azure Batch aaaRun na virtuálních počítačích nákladově efektivní s nízkou prioritou (Preview) | Microsoft Docs"
description: "Zjistěte, jak tooprovision nízkou prioritu virtuální počítače tooreduce hello náklady úloh Azure Batch."
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
ms.openlocfilehash: 91a5e89a819d05583e6b146932d925e217b4be4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="820e1-103">Použití nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)</span><span class="sxs-lookup"><span data-stu-id="820e1-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="820e1-104">Azure Batch nabízí nízkou priorty virtuální počítače (VM) tooreduce hello náklady úloh služby Batch.</span><span class="sxs-lookup"><span data-stu-id="820e1-104">Azure Batch offers low-priorty virtual machines (VMs) tooreduce hello cost of Batch workloads.</span></span> <span data-ttu-id="820e1-105">Virtuální počítače s nízkou prioritou umožňují nové typy úloh služby Batch, tím, že poskytuje velké množství výpočetního výkonu, která je také ekonomické.</span><span class="sxs-lookup"><span data-stu-id="820e1-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="820e1-106">Virtuální počítače s nízkou prioritou využít výhod kapacitní v Azure.</span><span class="sxs-lookup"><span data-stu-id="820e1-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="820e1-107">Když zadáte nízkou prioritu virtuálních počítačů ve fondech, Azure Batch mohou automaticky používat tento přebytek, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="820e1-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="820e1-108">Hello kompromis pro používání virtuálních počítačů nízkou prioritu je, že tyto virtuální počítače může být zrušené při žádné kapacitní je k dispozici v Azure.</span><span class="sxs-lookup"><span data-stu-id="820e1-108">hello tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="820e1-109">Z toho důvodu jsou nejvhodnější pro některé typy úloh virtuálních počítačů s nízkou prioritou.</span><span class="sxs-lookup"><span data-stu-id="820e1-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="820e1-110">Použijte virtuální počítače nízkou prioritu pro batch a asynchronní zpracování úloh, kde čas dokončení úlohy hello je flexibilní a pracovní hello je distribuován do mnoha virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="820e1-110">Use low-priority VMs for batch and asynchronous processing workloads where hello job completion time is flexible and hello work is distributed across many VMs.</span></span>

<span data-ttu-id="820e1-111">Virtuální počítače s nízkou prioritou jsou výrazně levnější než vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="820e1-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="820e1-112">Podrobnosti o cenách najdete v části [ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="820e1-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="820e1-113">Další informace o virtuálních počítačích nízkou prioritu, najdete v blogovém příspěvku hello: [dávky computing za zlomek ceny hello](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="820e1-113">For an additional discussion of low-priority VMs, see hello blog post announcement: [Batch computing at a fraction of hello price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="820e1-114">Nízkou prioritu virtuální počítače jsou aktuálně ve verzi preview a jsou dostupné pouze pro úlohy běžící v dávce.</span><span class="sxs-lookup"><span data-stu-id="820e1-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="820e1-115">Případy použití pro virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="820e1-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="820e1-116">Zadané vlastnosti hello nízkou prioritu virtuálních počítačů, jaké úlohy může nebo nemůže používat je?</span><span class="sxs-lookup"><span data-stu-id="820e1-116">Given hello characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="820e1-117">Obecně platí zpracování úloh služby batch jsou vhodné, úlohy jsou rozdělená do mnoho paralelních úloh nebo existuje mnoho úloh, které jsou škálovat na více systémů a distribuován do mnoha virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="820e1-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="820e1-118">použití toomaximize kapacitní v Azure, vhodný úloh můžete škálovat.</span><span class="sxs-lookup"><span data-stu-id="820e1-118">toomaximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="820e1-119">Příležitostně virtuální počítače nemusí být k dispozici, nebo bude možné zrušené, který bude mít za následek menší kapacitu pro úlohy a může způsobit přerušení tootask a opakování.</span><span class="sxs-lookup"><span data-stu-id="820e1-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead tootask interruption and reruns.</span></span> <span data-ttu-id="820e1-120">Proto musí být v době hello jejich zajištění může trvat toorun flexibilní úlohy.</span><span class="sxs-lookup"><span data-stu-id="820e1-120">Jobs must therefore be flexible in hello time they can take toorun.</span></span>

-   <span data-ttu-id="820e1-121">Úlohy se delší úloh může být ovlivněno více, pokud dojde k přerušení.</span><span class="sxs-lookup"><span data-stu-id="820e1-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="820e1-122">Pokud dlouho běžící, že úlohy implementovat vytváření kontrolních bodů průběhu toosave jako jejich provedení, pak dopad přerušení bude mnohem menší.</span><span class="sxs-lookup"><span data-stu-id="820e1-122">If long-running tasks implement checkpointing toosave progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="820e1-123">Jako hello dopad přerušení je mnohem méně úlohy s kratší časy spouštění nejlépe zpravidla toowork s virtuálními počítači nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="820e1-123">Tasks with shorter execution times tend toowork best with low-priority VMs as hello impact of interruption is far less.</span></span>

-   <span data-ttu-id="820e1-124">Dlouhotrvajících úloh MPI, které využívají víc virtuálních počítačů nejsou dobře hodí toouse nízkou prioritu virtuální počítače jako jeden zrušené virtuální počítač bude nejspíš realizace toohello celou úlohy s toobe spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="820e1-124">Long-running MPI jobs that utilize multiple VMs are not well suited toouse low-priority VMs as one preempted VM will likely lead toohello whole job having toobe run again.</span></span>

<span data-ttu-id="820e1-125">Některé příklady dávkové zpracování použijte případech skvěle hodí toouse, které jsou virtuální počítače nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="820e1-125">Some examples of batch processing use cases well suited toouse low-priority VMs are:</span></span>

-   <span data-ttu-id="820e1-126">**Vývoj a testování**: zejména, pokud jsou vyvíjených rozsáhlé řešení výrazné úspory může být dosaženo.</span><span class="sxs-lookup"><span data-stu-id="820e1-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="820e1-127">Všechny typy testování může přinést výhody, ale ve velkém měřítku zátěžové testování a testování regrese jsou skvělý používá.</span><span class="sxs-lookup"><span data-stu-id="820e1-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="820e1-128">**Dodávání kapacity na vyžádání**: virtuální počítače s nízkou prioritou lze použít k doplnění regular vyhrazených virtuálních počítačích – Pokud je k dispozici, můžete škálovat a proto dokončení rychlejší nižší náklady úlohy; Pokud není k dispozici, hello účaří vyhrazené virtuální počítače jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="820e1-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, hello baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="820e1-129">**Čas spuštění úlohy flexibilní**: Pokud je flexibilitu v hello čas úlohy mají toocomplete, pak lze tolerovat potenciální vyřazuje kapacity, však s hello přidání nízkou prioritu úlohy virtuálních počítačů se často spustí rychleji a s nižšími náklady.</span><span class="sxs-lookup"><span data-stu-id="820e1-129">**Flexible job execution time**: If there is flexibility in hello time jobs have toocomplete, then potential drops in capacity can be tolerated; however, with hello addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="820e1-130">Fondy batch může být nakonfigurované toouse nízkou prioritu virtuálních počítačů v několika různými způsoby v závislosti na hello flexibilitu při úlohy čas spuštění:</span><span class="sxs-lookup"><span data-stu-id="820e1-130">Batch pools can be configured toouse low-priority VMs in a few ways, depending on hello flexibility in job execution time:</span></span>

-   <span data-ttu-id="820e1-131">Virtuální počítače nízkou prioritu lze použít pouze ve fondu a Batch bude jednoduše obnovit všechny preempted kapacity, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="820e1-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="820e1-132">Toto je hello nejlevnější způsob tooexecute úlohy se používají pouze nízkou prioritu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="820e1-132">This is hello cheapest way tooexecute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="820e1-133">Virtuální počítače s nízkou prioritou lze použít ve spojení s pevnou směrného plánu vyhrazených virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="820e1-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="820e1-134">Hello pevný počet vyhrazených virtuálních počítačích zajišťuje, že je vždy některé kapacity tookeep úlohy pokročíte.</span><span class="sxs-lookup"><span data-stu-id="820e1-134">hello fixed number of dedicated VMs ensures there is always some capacity tookeep a job progressing.</span></span>

-   <span data-ttu-id="820e1-135">Může být dynamické směs vyhrazené a nízkou prioritu virtuální počítače, aby levnější nízkou prioritu virtuálních počítačů se používají výhradně, pokud je k dispozici, ale za cenu úplné hello vyhrazené virtuální počítače jsou rozšířit tak, pokud jsou povinné, tookeep minimální množství kapacity k dispozici tookeep hello úlohy pokročíte.</span><span class="sxs-lookup"><span data-stu-id="820e1-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but hello full-priced dedicated VMs are scaled up when required, tookeep a minimum amount of capacity available tookeep hello jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="820e1-136">Batch podporu pro virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="820e1-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="820e1-137">Azure Batch poskytuje několik možností, které bylo snadné tooconsume a těžit z virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="820e1-137">Azure Batch provides several capabilities that make it easy tooconsume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="820e1-138">Fondy batch může obsahovat vyhrazených virtuálních počítačích a virtuální počítače s nízkou prioritou.</span><span class="sxs-lookup"><span data-stu-id="820e1-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="820e1-139">Hello počty jednotlivých typů virtuálních počítačů lze zadat, pokud fond je vytvořené nebo změněné kdykoli pro existující fond, pomocí operace změny velikosti explicitní hello nebo pomocí automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="820e1-139">hello number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using hello explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="820e1-140">Odeslání úlohy a úlohy může zůstat beze změny a nemusíte dělat starosti s typy hello virtuálního počítače ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="820e1-140">Job and task submission can remain unchanged and do not need to be concerned with hello VM types in hello pool.</span></span> <span data-ttu-id="820e1-141">Je také možné toohave fond úplně pomocí nízkou prioritu virtuální počítače toorun úloh jako možný, ale otočení až vyhrazených virtuálních počítačích levné Pokud hello kapacity klesne pod minimální prahové hodnoty, aby spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="820e1-141">It is also possible toohave a pool completely use low-priority VMs toorun jobs as cheaply as possible, but spin up dedicated VMs if hello capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="820e1-142">Fondy batch automaticky hledat toohello cílový počet virtuálních počítačů, nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="820e1-142">Batch pools automatically seek toohello target number of low-priority VMs.</span></span> <span data-ttu-id="820e1-143">Pokud přerušené virtuální počítače, se pokusí Batch tooreplace hello ztráty kapacity a návratové toohello cíle.</span><span class="sxs-lookup"><span data-stu-id="820e1-143">If VMs are preempted, then Batch will attempt tooreplace hello lost capacity and return toohello target.</span></span>

-   <span data-ttu-id="820e1-144">V případě hello úloh se přerušena Batch rozpozná a automaticky znovu toobe úlohy znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="820e1-144">In hello case of tasks being interrupted, Batch will detect and automatically requeue tasks toobe run again.</span></span>

-   <span data-ttu-id="820e1-145">Virtuální počítače s nízkou prioritou mají kvóta na jádra, která se liší od vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="820e1-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="820e1-146">Hello uvozovky pro virtuální počítače s nízkou prioritou je vyšší než u vyhrazených virtuálních počítačích, protože virtuální počítače s nízkou prioritou nižší náklady.</span><span class="sxs-lookup"><span data-stu-id="820e1-146">hello quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="820e1-147">V tématu [Batch, kvóty a omezení služby](batch-quota-limit.md#resource-quotas) Další informace.</span><span class="sxs-lookup"><span data-stu-id="820e1-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="820e1-148">Virtuální počítače s nízkou prioritou nejsou aktuálně podporovány pro účty Batch, kde hello fondu přidělení režim je nastaven příliš[uživatele předplatné](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="820e1-148">Low-priority VMs are not currently supported for Batch accounts where hello pool allocation mode is set too[User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="820e1-149">Vytváření a aktualizaci fondy</span><span class="sxs-lookup"><span data-stu-id="820e1-149">Create and update pools</span></span>

<span data-ttu-id="820e1-150">Fondu služby Batch může obsahovat vyhrazené a nízkou prioritu virtuální počítače (taky odkazované tooas výpočetní uzly).</span><span class="sxs-lookup"><span data-stu-id="820e1-150">A Batch pool can contain both dedicated and low-priority VMs (also referred tooas compute nodes).</span></span> <span data-ttu-id="820e1-151">Pro vyhrazené i nízkou prioritu virtuální počítače můžete nastavit hello cílovým počtem výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="820e1-151">You can set hello target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="820e1-152">Hello cílový počet uzlů určuje hello počet virtuálních počítačů, na které má toohave ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="820e1-152">hello target number of nodes specifies hello number of VMs you want toohave in hello pool.</span></span>

<span data-ttu-id="820e1-153">Například toocreate fondu pomocí služby Azure cloud virtuálních počítačů s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="820e1-153">For example, toocreate a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="820e1-154">toocreate fondu pomocí virtuální počítače Azure (v tomto případě virtuální počítače s Linuxem) s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:</span><span class="sxs-lookup"><span data-stu-id="820e1-154">toocreate a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="820e1-155">Aktuální počet uzlů hello můžete získat pro vyhrazené i nízkou prioritu virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="820e1-155">You can get hello current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="820e1-156">Uzly fondu mají vlastnost tooindicate, pokud uzel hello je vyhrazené nebo nízkou prioritu virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="820e1-156">Pool nodes have a property tooindicate if hello node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="820e1-157">Přerušené jeden nebo více uzlů ve fondu, operace výpisu uzlů ve fondu pořád vrátí tyto uzly, aktuální počet uzlů nízkou prioritu hello zůstanou nezměněna, ale tyto uzly budou mít jejich stav nastaven toothe **přepnuto**stavu.</span><span class="sxs-lookup"><span data-stu-id="820e1-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, hello current number of low-priority nodes will remain unchanged, but those nodes will have their state set toothe **Preempted** state.</span></span> <span data-ttu-id="820e1-158">Batch se pokusí toofind nahrazení virtuální počítače, a pokud bylo úspěšné, bude projít hello uzly **vytváření** a potom **počáteční** stavy poté jsou dostupné pro provádění úkolů, stejně jako nové uzly.</span><span class="sxs-lookup"><span data-stu-id="820e1-158">Batch will attempt toofind replacement VMs and, if successful, hello nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="820e1-159">Škálování fond, který obsahuje virtuální počítače s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="820e1-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="820e1-160">Stejně jako u fondy výhradně skládající se z vyhrazených virtuálních počítačích, je možné tooscale fondu obsahujícího nízkou prioritu virtuálních počítačů pomocí volání metody změny velikosti hello nebo pomocí automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="820e1-160">As with pools solely consisting of dedicated VMs, it is possible tooscale a pool containing low-priority VMs by calling hello Resize method or by using auto-scale.</span></span>

<span data-ttu-id="820e1-161">Hello operace změny velikosti fondu trvá druhý volitelný parametr, který aktualizuje hodnotu **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="820e1-161">hello pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="820e1-162">Vzorec automatickému škálování fondu Hello podporuje virtuální počítače s nízkou prioritou následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="820e1-162">hello pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="820e1-163">Můžete získat nebo nastavit hello hodnotu proměnné definované služby hello **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="820e1-163">You can get or set hello value of hello service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="820e1-164">Můžete získat hello hodnoty proměnné definované služby hello **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="820e1-164">You can get hello value of hello service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="820e1-165">Můžete získat hello hodnoty proměnné definované služby hello **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="820e1-165">You can get hello value of hello service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="820e1-166">Tato proměnná vrátí hello počet uzlů v hello zrušené stavu a umožňuje škálování nahoru nebo dolů hello počet vyhrazených uzlů, v závislosti na počtu hello zrušené uzly, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="820e1-166">This variable returns hello number of nodes in hello preempted state and allows you to scale up or down hello number of dedicated nodes, depending on hello number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="820e1-167">Úlohy a úkoly</span><span class="sxs-lookup"><span data-stu-id="820e1-167">Jobs and tasks</span></span>

<span data-ttu-id="820e1-168">Úlohy a úkoly vyžadují velmi malé podporu pro uzly nízkou prioritu; podporuje se jen Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="820e1-168">Jobs and tasks require very little support for low-priority nodes; hello only support is as follows:</span></span>

-   <span data-ttu-id="820e1-169">Hello JobManagerTask vlastnost úlohy má novou vlastnost **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="820e1-169">hello JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="820e1-170">Pokud tato vlastnost hodnotu true, hello úkolu Správce úloh bude naplánována na vyhrazené nebo nízkou prioritu uzlu.</span><span class="sxs-lookup"><span data-stu-id="820e1-170">When this property is true, hello job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="820e1-171">Pokud je tato vlastnost hodnotu false, hello úkolu Správce úloh bude naplánované tooa pouze vyhrazené uzlu.</span><span class="sxs-lookup"><span data-stu-id="820e1-171">If this property is false, hello job manager task will be scheduled tooa dedicated node only.</span></span>

-   <span data-ttu-id="820e1-172">[Proměnnou prostředí](batch-compute-node-environment-variables.md) je k dispozici tooa úloh aplikace, aby mohla určit, zda je spuštěn na nízkou prioritu, nebo vyhrazený uzlu.</span><span class="sxs-lookup"><span data-stu-id="820e1-172">An [environment variable](batch-compute-node-environment-variables.md) is available tooa task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="820e1-173">Proměnná prostředí Hello je AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="820e1-173">hello environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="820e1-174">Zpracování přerušení</span><span class="sxs-lookup"><span data-stu-id="820e1-174">Handling preemption</span></span>

<span data-ttu-id="820e1-175">Virtuální počítače může být někdy zrušené; v takovém případě Batch hello následující:</span><span class="sxs-lookup"><span data-stu-id="820e1-175">VMs may occasionally be preempted; when this happens, Batch does hello following:</span></span>

-   <span data-ttu-id="820e1-176">Hello zrušené virtuální počítače mají jejich stav aktualizovat příliš**přepnuto**.</span><span class="sxs-lookup"><span data-stu-id="820e1-176">hello preempted VMs have their state updated too**Preempted**.</span></span>
-   <span data-ttu-id="820e1-177">Pokud úlohy běžely hello zabránilo uzlu virtuální počítače, pak jsou tyto úlohy zařazena a znovu spusťte.</span><span class="sxs-lookup"><span data-stu-id="820e1-177">If tasks were running on hello preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="820e1-178">Odstranit je efektivně Hello virtuálních počítačů, úvodní tooany data uložená místně na hello ke ztrátě virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="820e1-178">hello VM is effectively deleted, leading tooany data stored locally on hello VM being lost.</span></span>
-   <span data-ttu-id="820e1-179">fond Hello průběžně pokusí tooreach hello cílový počet uzlů nízkou prioritu k dispozici.</span><span class="sxs-lookup"><span data-stu-id="820e1-179">hello pool continually attempts tooreach hello target number of low-priority nodes available.</span></span> <span data-ttu-id="820e1-180">Když se najde náhradní kapacity uzly zachovat jejich ID, ale jsou znovu inicializovat, projít **vytváření** a **počáteční** stavy předtím, než jsou k dispozici pro plánování úkolů.</span><span class="sxs-lookup"><span data-stu-id="820e1-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="820e1-181">Přerušování počty jsou k dispozici jako metriky v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="820e1-181">Preemption counts are available as a metric in hello Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="820e1-182">Metriky</span><span class="sxs-lookup"><span data-stu-id="820e1-182">Metrics</span></span>

<span data-ttu-id="820e1-183">Nové metriky, které jsou k dispozici v hello [portál Azure](https://portal.azure.com) pro uzly nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="820e1-183">New metrics are available in hello [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="820e1-184">Jsou tyto metriky:</span><span class="sxs-lookup"><span data-stu-id="820e1-184">These metrics are:</span></span>

- <span data-ttu-id="820e1-185">Počet uzlů s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="820e1-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="820e1-186">Počet jader s nízkou prioritou</span><span class="sxs-lookup"><span data-stu-id="820e1-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="820e1-187">Zrušené počet uzlů</span><span class="sxs-lookup"><span data-stu-id="820e1-187">Preempted Node Count</span></span>

<span data-ttu-id="820e1-188">Metrika tooview v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="820e1-188">tooview metrics in hello Azure portal:</span></span>

1. <span data-ttu-id="820e1-189">Přejděte tooyour účtu Batch na portálu hello a zobrazit hello nastavení vašeho účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="820e1-189">Navigate tooyour Batch account in hello portal, and view hello settings for your Batch account.</span></span>
2. <span data-ttu-id="820e1-190">Vyberte **metriky** z hello **monitorování** části.</span><span class="sxs-lookup"><span data-stu-id="820e1-190">Select **Metrics** from hello **Monitoring** section.</span></span>
3. <span data-ttu-id="820e1-191">Vybrat metriky hello požadavky z hello **dostupné metriky** seznamu.</span><span class="sxs-lookup"><span data-stu-id="820e1-191">Select hello metrics you desire from hello **Available Metrics** list.</span></span>

![Metriky pro uzly s nízkou prioritou](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="820e1-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="820e1-193">Next steps</span></span>

* <span data-ttu-id="820e1-194">Čtení hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md), základní informace pro každý, kdo Příprava toouse dávky.</span><span class="sxs-lookup"><span data-stu-id="820e1-194">Read hello [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing toouse Batch.</span></span> <span data-ttu-id="820e1-195">Hello článek obsahuje podrobnější informace o prostředky služby Batch, například fondy, uzlů, úlohy a úkoly a hello mnoho funkcí rozhraní API, které můžete použít při vytváření aplikace Batch.</span><span class="sxs-lookup"><span data-stu-id="820e1-195">hello article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and hello many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="820e1-196">Další informace o hello [nástroje a rozhraní API služby Batch](batch-apis-tools.md) dostupné pro vytváření řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="820e1-196">Learn about hello [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
