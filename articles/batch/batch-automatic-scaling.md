---
title: "Automatické škálování výpočetních uzlů ve fondu Azure Batch | Microsoft Docs"
description: "Povolte automatické škálování ve fondu cloudu dynamicky upravit počet výpočetních uzlů ve fondu."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e49cd8a64a48c53f5b6104703164a597c797f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a><span data-ttu-id="305a5-103">Vytvořit automatické škálování vzorec škálování výpočetních uzlů ve fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="305a5-103">Create an automatic scaling formula for scaling compute nodes in a Batch pool</span></span>

<span data-ttu-id="305a5-104">Azure Batch může automaticky škálovat fondů na základě parametrů, které definujete.</span><span class="sxs-lookup"><span data-stu-id="305a5-104">Azure Batch can automatically scale pools based on parameters that you define.</span></span> <span data-ttu-id="305a5-105">Automatické škálování, Batch uzlů dynamicky přidá k fondu jako zvýšení požadavky úloh a odebere výpočetní uzly, jak se snížit.</span><span class="sxs-lookup"><span data-stu-id="305a5-105">With automatic scaling, Batch dynamically adds nodes to a pool as task demands increase, and removes compute nodes as they decrease.</span></span> <span data-ttu-id="305a5-106">Čas a peníze můžete uložit tak, že automaticky upraví počet výpočetních uzlů, které používá vaše aplikace Batch.</span><span class="sxs-lookup"><span data-stu-id="305a5-106">You can save both time and money by automatically adjusting the number of compute nodes used by your Batch application.</span></span> 

<span data-ttu-id="305a5-107">Povolit automatické škálování ve fondu výpočetních uzlů tím, že přidružíte ho *vzorec škálování* , které definujete.</span><span class="sxs-lookup"><span data-stu-id="305a5-107">You enable automatic scaling on a pool of compute nodes by associating with it an *autoscale formula* that you define.</span></span> <span data-ttu-id="305a5-108">Služba Batch používá vzorec škálování můžete určit počet výpočetních uzlů, které jsou potřebné k provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="305a5-108">The Batch service uses the autoscale formula to determine the number of compute nodes that are needed to execute your workload.</span></span> <span data-ttu-id="305a5-109">Výpočetní uzly může být vyhrazený uzly nebo [nízkou prioritu uzly](batch-low-pri-vms.md).</span><span class="sxs-lookup"><span data-stu-id="305a5-109">Compute nodes may be dedicated nodes or [low-priority nodes](batch-low-pri-vms.md).</span></span> <span data-ttu-id="305a5-110">Batch odpoví na data metriky služby, která se shromažďují pravidelně.</span><span class="sxs-lookup"><span data-stu-id="305a5-110">Batch responds to service metrics data that is collected periodically.</span></span> <span data-ttu-id="305a5-111">Na základě těchto dat metriky Batch upraví počet výpočetních uzlů ve fondu v závislosti na vaší vzorec a v konfigurovatelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="305a5-111">Using this metrics data, Batch adjusts the number of compute nodes in the pool based on your formula and at a configurable interval.</span></span>

<span data-ttu-id="305a5-112">Můžete povolit automatické škálování buď při vytváření fondu, nebo na existující fond.</span><span class="sxs-lookup"><span data-stu-id="305a5-112">You can enable automatic scaling either when a pool is created, or on an existing pool.</span></span> <span data-ttu-id="305a5-113">Můžete také změnit existující vzorec ve fondu, který je nakonfigurován pro automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-113">You can also change an existing formula on a pool that is configured for autoscaling.</span></span> <span data-ttu-id="305a5-114">Dávky můžete vyhodnotit vzorcích před jejich přiřazení do fondů a monitorovat stav spuštění automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-114">Batch enables you to evaluate your formulas before assigning them to pools and to monitor the status of automatic scaling runs.</span></span>

<span data-ttu-id="305a5-115">Tento článek popisuje různé entitami, které tvoří vaše vzorce automatického škálování, včetně proměnné, operátory, operace a funkce.</span><span class="sxs-lookup"><span data-stu-id="305a5-115">This article discusses the various entities that make up your autoscale formulas, including variables, operators, operations, and functions.</span></span> <span data-ttu-id="305a5-116">Probereme, jak získat různé výpočetní prostředek a úloha metriky v rámci dávky.</span><span class="sxs-lookup"><span data-stu-id="305a5-116">We discuss how to obtain various compute resource and task metrics within Batch.</span></span> <span data-ttu-id="305a5-117">Tyto metriky můžete upravit počet uzlů váš fond na základě využití a úlohy stavu prostředků.</span><span class="sxs-lookup"><span data-stu-id="305a5-117">You can use these metrics to adjust your pool's node count based on resource usage and task status.</span></span> <span data-ttu-id="305a5-118">Potom jsme popisují, jak vytvořit vzorec a povolte automatické škálování ve fondu pomocí Batch REST a rozhraní API technologie .NET.</span><span class="sxs-lookup"><span data-stu-id="305a5-118">We then describe how to construct a formula and enable automatic scaling on a pool by using both the Batch REST and .NET APIs.</span></span> <span data-ttu-id="305a5-119">Dokončíme nakonec, se několik vzorci příklad.</span><span class="sxs-lookup"><span data-stu-id="305a5-119">Finally, we finish up with a few example formulas.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="305a5-120">Při vytváření účtu Batch, můžete zadat [konfigurace účtu](batch-api-basics.md#account), která určuje, zda jsou fondy přidělené v předplatném služby Batch (výchozí) nebo ve vašem předplatném uživatele.</span><span class="sxs-lookup"><span data-stu-id="305a5-120">When you create a Batch account, you can specify the [account configuration](batch-api-basics.md#account), which determines whether pools are allocated in a Batch service subscription (the default), or in your user subscription.</span></span> <span data-ttu-id="305a5-121">Pokud jste vytvořili vašeho účtu Batch s výchozí konfiguraci služby Batch, váš účet je omezený na maximální počet jader, které lze použít ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="305a5-121">If you created your Batch account with the default Batch Service configuration, then your account is limited to a maximum number of cores that can be used for processing.</span></span> <span data-ttu-id="305a5-122">Služba Batch škáluje výpočetní uzly pouze do tohoto limitu jádra.</span><span class="sxs-lookup"><span data-stu-id="305a5-122">The Batch service scales compute nodes only up to that core limit.</span></span> <span data-ttu-id="305a5-123">Z tohoto důvodu nemusí kontaktovat službu Batch cílovým počtem výpočetních uzlů určeného vzorec škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-123">For this reason, the Batch service may not reach the target number of compute nodes specified by an autoscale formula.</span></span> <span data-ttu-id="305a5-124">V tématu [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md) informace o prohlížení a zvýšení kvóty vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="305a5-124">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for information on viewing and increasing your account quotas.</span></span>
>
><span data-ttu-id="305a5-125">Pokud jste vytvořili účet s konfigurace uživatele předplatného, váš účet sdílí v základní kvóta pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="305a5-125">If you created your account with the User Subscription configuration, then your account shares in the core quota for the subscription.</span></span> <span data-ttu-id="305a5-126">Další informace najdete v tématu [Omezení virtuálních počítačů](../azure-subscription-service-limits.md#virtual-machines-limits) v tématu [Limity, kvóty a omezení předplatného a služeb Azure](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="305a5-126">For more information, see [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>
>
>

## <a name="automatic-scaling-formulas"></a><span data-ttu-id="305a5-127">Automatické škálování vzorce</span><span class="sxs-lookup"><span data-stu-id="305a5-127">Automatic scaling formulas</span></span>
<span data-ttu-id="305a5-128">Vzorec automatického škálování je řetězcová hodnota, kterou definujete, který obsahuje jeden nebo více příkazů.</span><span class="sxs-lookup"><span data-stu-id="305a5-128">An automatic scaling formula is a string value that you define that contains one or more statements.</span></span> <span data-ttu-id="305a5-129">Vzorec automatického škálování je přiřazen fondu [autoScaleFormula] [ rest_autoscaleformula] – element (Batch REST) nebo [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] vlastnost (Batch .NET).</span><span class="sxs-lookup"><span data-stu-id="305a5-129">The autoscale formula is assigned to a pool's [autoScaleFormula][rest_autoscaleformula] element (Batch REST) or [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula] property (Batch .NET).</span></span> <span data-ttu-id="305a5-130">Služba Batch používá vzorec k určení cílovým počtem výpočetních uzlů ve fondu pro další interval zpracování.</span><span class="sxs-lookup"><span data-stu-id="305a5-130">The Batch service uses your formula to determine the target number of compute nodes in the pool for the next interval of processing.</span></span> <span data-ttu-id="305a5-131">Vzorce řetězec nesmí být delší než 8 KB, může obsahovat až 100 příkazy, které jsou odděleny středníky a může obsahovat konce řádků a komentáře.</span><span class="sxs-lookup"><span data-stu-id="305a5-131">The formula string cannot exceed 8 KB, can include up to 100 statements that are separated by semicolons, and can include line breaks and comments.</span></span>

<span data-ttu-id="305a5-132">Automatické škálování vzorců si můžete představit jako Batch škálování "jazyk."</span><span class="sxs-lookup"><span data-stu-id="305a5-132">You can think of automatic scaling formulas as a Batch autoscale "language."</span></span> <span data-ttu-id="305a5-133">Vzorce příkazy jsou volné formátovaných výrazů, které můžou zahrnovat proměnných definovaných servisu (proměnné definované ve službě Batch) a uživatelem definované proměnné (proměnné, které definujete).</span><span class="sxs-lookup"><span data-stu-id="305a5-133">Formula statements are free-formed expressions that can include both service-defined variables (variables defined by the Batch service) and user-defined variables (variables that you define).</span></span> <span data-ttu-id="305a5-134">Pomocí předdefinovaných typů, operátory a funkce můžou dělat různé operace na tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="305a5-134">They can perform various operations on these values by using built-in types, operators, and functions.</span></span> <span data-ttu-id="305a5-135">Příkaz například může mít tento tvar:</span><span class="sxs-lookup"><span data-stu-id="305a5-135">For example, a statement might take the following form:</span></span>

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

<span data-ttu-id="305a5-136">Vzorce obvykle obsahují více příkazů, které provádějí operace na hodnotách, které byly získány v předchozích příkazech.</span><span class="sxs-lookup"><span data-stu-id="305a5-136">Formulas generally contain multiple statements that perform operations on values that are obtained in previous statements.</span></span> <span data-ttu-id="305a5-137">Například nejprve jsme získat hodnotu `variable1`, předejte ji do funkce k naplnění `variable2`:</span><span class="sxs-lookup"><span data-stu-id="305a5-137">For example, first we obtain a value for `variable1`, then pass it to a function to populate `variable2`:</span></span>

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

<span data-ttu-id="305a5-138">Zahrňte tyto příkazy vzorec škálování přijaty ve cílovým počtem výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-138">Include these statements in your autoscale formula to arrive at a target number of compute nodes.</span></span> <span data-ttu-id="305a5-139">Vyhrazené uzlů a uzly nízkou prioritu mít vlastní nastavení cílové tak, aby můžete definovat cíl pro každý typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="305a5-139">Dedicated nodes and low-priority nodes each have their own target settings, so that you can define a target for each type of node.</span></span> <span data-ttu-id="305a5-140">Vzorec škálování může zahrnovat cílovou hodnotu pro vyhrazené uzly a cílovou hodnotu pro uzly nízkou prioritu.</span><span class="sxs-lookup"><span data-stu-id="305a5-140">An autoscale formula can include a target value for dedicated nodes, a target value for low-priority nodes, or both.</span></span>

<span data-ttu-id="305a5-141">Cílový počet uzlů může být vyšší, nižší nebo stejná jako aktuální počet uzlů typu ve fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-141">The target number of nodes may be higher, lower, or the same as the current number of nodes of that type in the pool.</span></span> <span data-ttu-id="305a5-142">Batch vyhodnocuje vzorec škálování fondu v určitých intervalech (viz [automatického škálování intervaly](#automatic-scaling-interval)).</span><span class="sxs-lookup"><span data-stu-id="305a5-142">Batch evaluates a pool's autoscale formula at a specific interval (see [automatic scaling intervals](#automatic-scaling-interval)).</span></span> <span data-ttu-id="305a5-143">Batch upraví cílový počet každý typ uzlu ve fondu na číslo, které určuje vzorec škálování v době vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="305a5-143">Batch adjusts the target number of each type of node in the pool to the number that your autoscale formula specifies at the time of evaluation.</span></span>

### <a name="sample-autoscale-formula"></a><span data-ttu-id="305a5-144">Vzorec škálování ukázka</span><span class="sxs-lookup"><span data-stu-id="305a5-144">Sample autoscale formula</span></span>

<span data-ttu-id="305a5-145">Tady je příklad vzorec škálování, který lze upravit fungovat pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="305a5-145">Here is an example of an autoscale formula that can be adjusted to work for most scenarios.</span></span> <span data-ttu-id="305a5-146">Proměnné `startingNumberOfVMs` a `maxNumberofVMs` v příkladu lze upravit vzorec vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="305a5-146">The variables `startingNumberOfVMs` and `maxNumberofVMs` in the example formula can be adjusted to your needs.</span></span> <span data-ttu-id="305a5-147">Tento vzorec škáluje vyhrazené uzly, ale můžete upravit tak, aby platí pro škálování nízkou prioritu také uzly.</span><span class="sxs-lookup"><span data-stu-id="305a5-147">This formula scales dedicated nodes, but can be modified to apply to scale low-priority nodes as well.</span></span> 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

<span data-ttu-id="305a5-148">Pomocí tohoto vzorce automatického škálování fondu původně vytvořena s jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="305a5-148">With this autoscale formula, the pool is initially created with a single VM.</span></span> <span data-ttu-id="305a5-149">`$PendingTasks` Metrika definuje počet úloh, které jsou spuštěné nebo zařazené ve frontě.</span><span class="sxs-lookup"><span data-stu-id="305a5-149">The `$PendingTasks` metric defines the number of tasks that are running or queued.</span></span> <span data-ttu-id="305a5-150">Vzorec najde průměrný počet úkolů čekajících na zpracování v posledních 180 sekund a nastaví `$TargetDedicatedNodes` proměnná odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="305a5-150">The formula finds the average number of pending tasks in the last 180 seconds and sets the `$TargetDedicatedNodes` variable accordingly.</span></span> <span data-ttu-id="305a5-151">Vzorec zajistí, že cílový počet vyhrazených uzlů nikdy nepřesáhne 25 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="305a5-151">The formula ensures that the target number of dedicated nodes never exceeds 25 VMs.</span></span> <span data-ttu-id="305a5-152">Nové úkoly jsou předloženy, fondu automaticky rozšiřovat.</span><span class="sxs-lookup"><span data-stu-id="305a5-152">As new tasks are submitted, the pool automatically grows.</span></span> <span data-ttu-id="305a5-153">Jako dokončení úkolů virtuální počítače se stanou volné jeden po druhém a vzorec automatického škálování zmenšuje fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-153">As tasks complete, VMs become free one by one and the autoscaling formula shrinks the pool.</span></span>

## <a name="variables"></a><span data-ttu-id="305a5-154">Proměnné</span><span class="sxs-lookup"><span data-stu-id="305a5-154">Variables</span></span>
<span data-ttu-id="305a5-155">Můžete je používat **služby definované** a **uživatelem definované** proměnné ve vzorcích škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-155">You can use both **service-defined** and **user-defined** variables in your autoscale formulas.</span></span> <span data-ttu-id="305a5-156">Proměnné definované služby jsou součástí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="305a5-156">The service-defined variables are built in to the Batch service.</span></span> <span data-ttu-id="305a5-157">Některé proměnné definované služby jsou pro čtení a zápis a některé jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="305a5-157">Some service-defined variables are read-write, and some are read-only.</span></span> <span data-ttu-id="305a5-158">Uživatelem definované proměnné jsou proměnné, které definujete.</span><span class="sxs-lookup"><span data-stu-id="305a5-158">User-defined variables are variables that you define.</span></span> <span data-ttu-id="305a5-159">Ve vzorci příklad uvedené v předchozí části `$TargetDedicatedNodes` a `$PendingTasks` jsou proměnných definovaných servisu.</span><span class="sxs-lookup"><span data-stu-id="305a5-159">In the example formula shown in the previous section, `$TargetDedicatedNodes` and `$PendingTasks` are service-defined variables.</span></span> <span data-ttu-id="305a5-160">Proměnné `startingNumberOfVMs` a `maxNumberofVMs` jsou uživatelem definované proměnné.</span><span class="sxs-lookup"><span data-stu-id="305a5-160">Variables `startingNumberOfVMs` and `maxNumberofVMs` are user-defined variables.</span></span>

> [!NOTE]
> <span data-ttu-id="305a5-161">Proměnné definované služby jsou vždy uvedeny znak dolaru ($).</span><span class="sxs-lookup"><span data-stu-id="305a5-161">Service-defined variables are always preceded by a dollar sign ($).</span></span> <span data-ttu-id="305a5-162">Uživatelem definované proměnné znak dolaru je volitelné.</span><span class="sxs-lookup"><span data-stu-id="305a5-162">For user-defined variables, the dollar sign is optional.</span></span>
>
>

<span data-ttu-id="305a5-163">Proměnné pro čtení i zápis i jen pro čtení, které jsou definovány pomocí služby Batch naleznete v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="305a5-163">The following tables show both read-write and read-only variables that are defined by the Batch service.</span></span>

<span data-ttu-id="305a5-164">Můžete získat a nastavit hodnoty těchto proměnných definovaných služby ke správě počet výpočetních uzlů ve fondu:</span><span class="sxs-lookup"><span data-stu-id="305a5-164">You can get and set the values of these service-defined variables to manage the number of compute nodes in a pool:</span></span>

| <span data-ttu-id="305a5-165">Proměnné definované služby pro čtení a zápis</span><span class="sxs-lookup"><span data-stu-id="305a5-165">Read-write service-defined variables</span></span> | <span data-ttu-id="305a5-166">Popis</span><span class="sxs-lookup"><span data-stu-id="305a5-166">Description</span></span> |
| --- | --- |
| <span data-ttu-id="305a5-167">$TargetDedicatedNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-167">$TargetDedicatedNodes</span></span> |<span data-ttu-id="305a5-168">Cílový počet vyhrazených výpočetních uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-168">The target number of dedicated compute nodes for the pool.</span></span> <span data-ttu-id="305a5-169">Počet vyhrazených uzlů je zadán jako cíl, protože fond nemusí vždy dosáhnout požadovaného počtu uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-169">The number of dedicated nodes is specified as a target because a pool may not always achieve the desired number of nodes.</span></span> <span data-ttu-id="305a5-170">Například pokud cílový počet vyhrazených uzlů je upraveném škálování zkušební verzi, než fondu dosáhl počáteční cíl, pak fondu nemusí mít přístup cíl.</span><span class="sxs-lookup"><span data-stu-id="305a5-170">For example, if the target number of dedicated nodes is modified by an autoscale evaluation before the pool has reached the initial target, then the pool may not reach the target.</span></span> <br /><br /> <span data-ttu-id="305a5-171">Fond na účtu vytvoří s konfigurací služby Batch nemusí dosáhnout cíli, pokud cílový překročí kvótu uzlu nebo core účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="305a5-171">A pool in an account created with the Batch Service configuration may not achieve its target if the target exceeds a Batch account node or core quota.</span></span> <span data-ttu-id="305a5-172">Fond na účtu vytvořené pomocí konfigurace předplatného uživatele nemusí dosáhnout cíli, pokud cílový překročí kvótu sdílené jader pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="305a5-172">A pool in an account created with the User Subscription configuration may not achieve its target if the target exceeds the shared core quota for the subscription.</span></span>|
| <span data-ttu-id="305a5-173">$TargetLowPriorityNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-173">$TargetLowPriorityNodes</span></span> |<span data-ttu-id="305a5-174">Cílový počet nízkou prioritu výpočetních uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-174">The target number of low-priority compute nodes for the pool.</span></span> <span data-ttu-id="305a5-175">Počet uzlů nízkou prioritu je zadán jako cíl, protože fond nemusí vždy dosáhnout požadovaného počtu uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-175">The number of low-priority nodes is specified as a target because a pool may not always achieve the desired number of nodes.</span></span> <span data-ttu-id="305a5-176">Například pokud cílový počet uzlů nízkou prioritu je upraveném škálování zkušební verzi, než fondu dosáhl počáteční cíl, pak fondu nemusí mít přístup cíl.</span><span class="sxs-lookup"><span data-stu-id="305a5-176">For example, if the target number of low-priority nodes is modified by an autoscale evaluation before the pool has reached the initial target, then the pool may not reach the target.</span></span> <span data-ttu-id="305a5-177">Fond nemusí také dosáhnout cíli, pokud cílový překročí kvótu uzlu nebo core účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="305a5-177">A pool may also not achieve its target if the target exceeds a Batch account node or core quota.</span></span> <br /><br /> <span data-ttu-id="305a5-178">Další informace na nízkou prioritu výpočetních uzlů najdete v tématu [použít nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)](batch-low-pri-vms.md).</span><span class="sxs-lookup"><span data-stu-id="305a5-178">For more information on low-priority compute nodes, see [Use low-priority VMs with Batch (Preview)](batch-low-pri-vms.md).</span></span> |
| <span data-ttu-id="305a5-179">$NodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="305a5-179">$NodeDeallocationOption</span></span> |<span data-ttu-id="305a5-180">Akce, která nastane, když se odebere z fondu, výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-180">The action that occurs when compute nodes are removed from a pool.</span></span> <span data-ttu-id="305a5-181">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="305a5-181">Possible values are:</span></span><ul><li><span data-ttu-id="305a5-182">**requeue**– okamžitě ukončí úlohy a uloží je zpět na fronty úloh tak, aby se přeplánovat.</span><span class="sxs-lookup"><span data-stu-id="305a5-182">**requeue**--Terminates tasks immediately and puts them back on the job queue so that they are rescheduled.</span></span><li><span data-ttu-id="305a5-183">**Ukončit**– okamžitě ukončí úlohy a odebere je ze fronty úloh.</span><span class="sxs-lookup"><span data-stu-id="305a5-183">**terminate**--Terminates tasks immediately and removes them from the job queue.</span></span><li><span data-ttu-id="305a5-184">**taskcompletion**– čeká aktuálně spuštěných úloh na dokončení a poté odstraní uzel z fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-184">**taskcompletion**--Waits for currently running tasks to finish and then removes the node from the pool.</span></span><li><span data-ttu-id="305a5-185">**retaineddata**– čeká na všechny místní data zachována úkolů na uzlu, který má být vyčištěna, před odebráním uzel z fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-185">**retaineddata**--Waits for all the local task-retained data on the node to be cleaned up before removing the node from the pool.</span></span></ul> |

<span data-ttu-id="305a5-186">Můžete získat hodnotu tyto proměnné definované služby provádět úpravy, které jsou založeny na metriky ze služby Batch:</span><span class="sxs-lookup"><span data-stu-id="305a5-186">You can get the value of these service-defined variables to make adjustments that are based on metrics from the Batch service:</span></span>

| <span data-ttu-id="305a5-187">Jen pro čtení služby definované proměnné</span><span class="sxs-lookup"><span data-stu-id="305a5-187">Read-only service-defined variables</span></span> | <span data-ttu-id="305a5-188">Popis</span><span class="sxs-lookup"><span data-stu-id="305a5-188">Description</span></span> |
| --- | --- |
| <span data-ttu-id="305a5-189">$CPUPercent</span><span class="sxs-lookup"><span data-stu-id="305a5-189">$CPUPercent</span></span> |<span data-ttu-id="305a5-190">Průměrná procento využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="305a5-190">The average percentage of CPU usage.</span></span> |
| <span data-ttu-id="305a5-191">$WallClockSeconds</span><span class="sxs-lookup"><span data-stu-id="305a5-191">$WallClockSeconds</span></span> |<span data-ttu-id="305a5-192">Počet sekund využívat.</span><span class="sxs-lookup"><span data-stu-id="305a5-192">The number of seconds consumed.</span></span> |
| <span data-ttu-id="305a5-193">$MemoryBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-193">$MemoryBytes</span></span> |<span data-ttu-id="305a5-194">Průměrný počet MB použít.</span><span class="sxs-lookup"><span data-stu-id="305a5-194">The average number of megabytes used.</span></span> |
| <span data-ttu-id="305a5-195">$DiskBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-195">$DiskBytes</span></span> |<span data-ttu-id="305a5-196">Průměrný počet gigabajtů použít na místních discích.</span><span class="sxs-lookup"><span data-stu-id="305a5-196">The average number of gigabytes used on the local disks.</span></span> |
| <span data-ttu-id="305a5-197">$DiskReadBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-197">$DiskReadBytes</span></span> |<span data-ttu-id="305a5-198">Počet přečtených bajtů.</span><span class="sxs-lookup"><span data-stu-id="305a5-198">The number of bytes read.</span></span> |
| <span data-ttu-id="305a5-199">$DiskWriteBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-199">$DiskWriteBytes</span></span> |<span data-ttu-id="305a5-200">Počet zapsaných bajtů.</span><span class="sxs-lookup"><span data-stu-id="305a5-200">The number of bytes written.</span></span> |
| <span data-ttu-id="305a5-201">$DiskReadOps</span><span class="sxs-lookup"><span data-stu-id="305a5-201">$DiskReadOps</span></span> |<span data-ttu-id="305a5-202">Počet operací čtení disku provést.</span><span class="sxs-lookup"><span data-stu-id="305a5-202">The count of read disk operations performed.</span></span> |
| <span data-ttu-id="305a5-203">$DiskWriteOps</span><span class="sxs-lookup"><span data-stu-id="305a5-203">$DiskWriteOps</span></span> |<span data-ttu-id="305a5-204">Počet operací zápisu disku provést.</span><span class="sxs-lookup"><span data-stu-id="305a5-204">The count of write disk operations performed.</span></span> |
| <span data-ttu-id="305a5-205">$NetworkInBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-205">$NetworkInBytes</span></span> |<span data-ttu-id="305a5-206">Počet přijatých bajtů.</span><span class="sxs-lookup"><span data-stu-id="305a5-206">The number of inbound bytes.</span></span> |
| <span data-ttu-id="305a5-207">$NetworkOutBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-207">$NetworkOutBytes</span></span> |<span data-ttu-id="305a5-208">Počet bajtů.</span><span class="sxs-lookup"><span data-stu-id="305a5-208">The number of outbound bytes.</span></span> |
| <span data-ttu-id="305a5-209">$SampleNodeCount</span><span class="sxs-lookup"><span data-stu-id="305a5-209">$SampleNodeCount</span></span> |<span data-ttu-id="305a5-210">Počet výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-210">The count of compute nodes.</span></span> |
| <span data-ttu-id="305a5-211">$ActiveTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-211">$ActiveTasks</span></span> |<span data-ttu-id="305a5-212">Počet úloh, které jsou připravené k provedení, ale ještě nejsou provádění.</span><span class="sxs-lookup"><span data-stu-id="305a5-212">The number of tasks that are ready to execute but are not yet executing.</span></span> <span data-ttu-id="305a5-213">Počet $ActiveTasks zahrnuje všechny úlohy, které jsou v aktivním stavu a jehož závislosti byly splněny.</span><span class="sxs-lookup"><span data-stu-id="305a5-213">The $ActiveTasks count includes all tasks that are in the active state and whose dependencies have been satisfied.</span></span> <span data-ttu-id="305a5-214">Všechny úlohy, které jsou v aktivním stavu, ale nebyly splněny jehož závislosti jsou vyloučeny z $ActiveTasks počet.</span><span class="sxs-lookup"><span data-stu-id="305a5-214">Any tasks that are in the active state but whose dependencies have not been satisfied are excluded from the $ActiveTasks count.</span></span>|
| <span data-ttu-id="305a5-215">$RunningTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-215">$RunningTasks</span></span> |<span data-ttu-id="305a5-216">Počet úloh ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="305a5-216">The number of tasks in a running state.</span></span> |
| <span data-ttu-id="305a5-217">$PendingTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-217">$PendingTasks</span></span> |<span data-ttu-id="305a5-218">Součet $ActiveTasks a $RunningTasks.</span><span class="sxs-lookup"><span data-stu-id="305a5-218">The sum of $ActiveTasks and $RunningTasks.</span></span> |
| <span data-ttu-id="305a5-219">$SucceededTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-219">$SucceededTasks</span></span> |<span data-ttu-id="305a5-220">Počet úloh, které bylo úspěšně dokončeno.</span><span class="sxs-lookup"><span data-stu-id="305a5-220">The number of tasks that finished successfully.</span></span> |
| <span data-ttu-id="305a5-221">$FailedTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-221">$FailedTasks</span></span> |<span data-ttu-id="305a5-222">Počet úloh, které se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="305a5-222">The number of tasks that failed.</span></span> |
| <span data-ttu-id="305a5-223">$CurrentDedicatedNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-223">$CurrentDedicatedNodes</span></span> |<span data-ttu-id="305a5-224">Aktuální počet vyhrazených výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-224">The current number of dedicated compute nodes.</span></span> |
| <span data-ttu-id="305a5-225">$CurrentLowPriorityNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-225">$CurrentLowPriorityNodes</span></span> |<span data-ttu-id="305a5-226">Aktuální počet nízkou prioritu výpočetní uzly, včetně všech uzlech, které byly zrušeny.</span><span class="sxs-lookup"><span data-stu-id="305a5-226">The current number of low-priority compute nodes, including any nodes that have been pre-empted.</span></span> |
| <span data-ttu-id="305a5-227">$PreemptedNodeCount</span><span class="sxs-lookup"><span data-stu-id="305a5-227">$PreemptedNodeCount</span></span> | <span data-ttu-id="305a5-228">Počet uzlů ve fondu, které jsou ve stavu, zrušeny.</span><span class="sxs-lookup"><span data-stu-id="305a5-228">The number of nodes in the pool that are in a pre-empted state.</span></span> |

> [!TIP]
> <span data-ttu-id="305a5-229">Jen pro čtení, služby definované proměnné, které jsou uvedeny v předchozí tabulce jsou *objekty* které poskytují různé metody pro přístup k datům spojené s každou.</span><span class="sxs-lookup"><span data-stu-id="305a5-229">The read-only, service-defined variables that are shown in the previous table are *objects* that provide various methods to access data associated with each.</span></span> <span data-ttu-id="305a5-230">Další informace najdete v tématu [získat ukázková data](#getsampledata) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="305a5-230">For more information, see [Obtain sample data](#getsampledata) later in this article.</span></span>
>
>

## <a name="types"></a><span data-ttu-id="305a5-231">Typy</span><span class="sxs-lookup"><span data-stu-id="305a5-231">Types</span></span>
<span data-ttu-id="305a5-232">Tyto typy jsou podporovány v vzorec:</span><span class="sxs-lookup"><span data-stu-id="305a5-232">These types are supported in a formula:</span></span>

* <span data-ttu-id="305a5-233">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-233">double</span></span>
* <span data-ttu-id="305a5-234">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-234">doubleVec</span></span>
* <span data-ttu-id="305a5-235">doubleVecList</span><span class="sxs-lookup"><span data-stu-id="305a5-235">doubleVecList</span></span>
* <span data-ttu-id="305a5-236">Řetězec</span><span class="sxs-lookup"><span data-stu-id="305a5-236">string</span></span>
* <span data-ttu-id="305a5-237">časové razítko – časové razítko je složené struktura, která obsahuje následující členy:</span><span class="sxs-lookup"><span data-stu-id="305a5-237">timestamp--timestamp is a compound structure that contains the following members:</span></span>

  * <span data-ttu-id="305a5-238">Rok</span><span class="sxs-lookup"><span data-stu-id="305a5-238">year</span></span>
  * <span data-ttu-id="305a5-239">měsíc (1-12)</span><span class="sxs-lookup"><span data-stu-id="305a5-239">month (1-12)</span></span>
  * <span data-ttu-id="305a5-240">den (1-31)</span><span class="sxs-lookup"><span data-stu-id="305a5-240">day (1-31)</span></span>
  * <span data-ttu-id="305a5-241">den v týdnu (ve formátu číslo; příklad: 1 pro pondělí)</span><span class="sxs-lookup"><span data-stu-id="305a5-241">weekday (in the format of number; for example, 1 for Monday)</span></span>
  * <span data-ttu-id="305a5-242">hodinu (ve 24hodinovém formátu číslo; například 13 znamená 13: 00)</span><span class="sxs-lookup"><span data-stu-id="305a5-242">hour (in 24-hour number format; for example, 13 means 1 PM)</span></span>
  * <span data-ttu-id="305a5-243">minut (00-59)</span><span class="sxs-lookup"><span data-stu-id="305a5-243">minute (00-59)</span></span>
  * <span data-ttu-id="305a5-244">druhý (00-59)</span><span class="sxs-lookup"><span data-stu-id="305a5-244">second (00-59)</span></span>
* <span data-ttu-id="305a5-245">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-245">timeinterval</span></span>

  * <span data-ttu-id="305a5-246">TimeInterval_Zero</span><span class="sxs-lookup"><span data-stu-id="305a5-246">TimeInterval_Zero</span></span>
  * <span data-ttu-id="305a5-247">TimeInterval_100ns</span><span class="sxs-lookup"><span data-stu-id="305a5-247">TimeInterval_100ns</span></span>
  * <span data-ttu-id="305a5-248">TimeInterval_Microsecond</span><span class="sxs-lookup"><span data-stu-id="305a5-248">TimeInterval_Microsecond</span></span>
  * <span data-ttu-id="305a5-249">TimeInterval_Millisecond</span><span class="sxs-lookup"><span data-stu-id="305a5-249">TimeInterval_Millisecond</span></span>
  * <span data-ttu-id="305a5-250">TimeInterval_Second</span><span class="sxs-lookup"><span data-stu-id="305a5-250">TimeInterval_Second</span></span>
  * <span data-ttu-id="305a5-251">TimeInterval_Minute</span><span class="sxs-lookup"><span data-stu-id="305a5-251">TimeInterval_Minute</span></span>
  * <span data-ttu-id="305a5-252">TimeInterval_Hour</span><span class="sxs-lookup"><span data-stu-id="305a5-252">TimeInterval_Hour</span></span>
  * <span data-ttu-id="305a5-253">TimeInterval_Day</span><span class="sxs-lookup"><span data-stu-id="305a5-253">TimeInterval_Day</span></span>
  * <span data-ttu-id="305a5-254">TimeInterval_Week</span><span class="sxs-lookup"><span data-stu-id="305a5-254">TimeInterval_Week</span></span>
  * <span data-ttu-id="305a5-255">TimeInterval_Year</span><span class="sxs-lookup"><span data-stu-id="305a5-255">TimeInterval_Year</span></span>

## <a name="operations"></a><span data-ttu-id="305a5-256">Operace</span><span class="sxs-lookup"><span data-stu-id="305a5-256">Operations</span></span>
<span data-ttu-id="305a5-257">Tyto operace jsou povoleny na typy, které jsou uvedeny v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="305a5-257">These operations are allowed on the types that are listed in the previous section.</span></span>

| <span data-ttu-id="305a5-258">Operace</span><span class="sxs-lookup"><span data-stu-id="305a5-258">Operation</span></span> | <span data-ttu-id="305a5-259">Podporované operátory</span><span class="sxs-lookup"><span data-stu-id="305a5-259">Supported operators</span></span> | <span data-ttu-id="305a5-260">Typ výsledku</span><span class="sxs-lookup"><span data-stu-id="305a5-260">Result type</span></span> |
| --- | --- | --- |
| <span data-ttu-id="305a5-261">dvojité *operátor* double</span><span class="sxs-lookup"><span data-stu-id="305a5-261">double *operator* double</span></span> |<span data-ttu-id="305a5-262">+, -, *, /</span><span class="sxs-lookup"><span data-stu-id="305a5-262">+, -, *, /</span></span> |<span data-ttu-id="305a5-263">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-263">double</span></span> |
| <span data-ttu-id="305a5-264">dvojité *operátor* timeinterval</span><span class="sxs-lookup"><span data-stu-id="305a5-264">double *operator* timeinterval</span></span> |* |<span data-ttu-id="305a5-265">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-265">timeinterval</span></span> |
| <span data-ttu-id="305a5-266">doubleVec *operátor* double</span><span class="sxs-lookup"><span data-stu-id="305a5-266">doubleVec *operator* double</span></span> |<span data-ttu-id="305a5-267">+, -, *, /</span><span class="sxs-lookup"><span data-stu-id="305a5-267">+, -, *, /</span></span> |<span data-ttu-id="305a5-268">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-268">doubleVec</span></span> |
| <span data-ttu-id="305a5-269">doubleVec *operátor* doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-269">doubleVec *operator* doubleVec</span></span> |<span data-ttu-id="305a5-270">+, -, *, /</span><span class="sxs-lookup"><span data-stu-id="305a5-270">+, -, *, /</span></span> |<span data-ttu-id="305a5-271">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-271">doubleVec</span></span> |
| <span data-ttu-id="305a5-272">TimeInterval *operátor* double</span><span class="sxs-lookup"><span data-stu-id="305a5-272">timeinterval *operator* double</span></span> |<span data-ttu-id="305a5-273">*, /</span><span class="sxs-lookup"><span data-stu-id="305a5-273">*, /</span></span> |<span data-ttu-id="305a5-274">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-274">timeinterval</span></span> |
| <span data-ttu-id="305a5-275">TimeInterval *operátor* timeinterval</span><span class="sxs-lookup"><span data-stu-id="305a5-275">timeinterval *operator* timeinterval</span></span> |<span data-ttu-id="305a5-276">+, -</span><span class="sxs-lookup"><span data-stu-id="305a5-276">+, -</span></span> |<span data-ttu-id="305a5-277">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-277">timeinterval</span></span> |
| <span data-ttu-id="305a5-278">TimeInterval *operátor* časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-278">timeinterval *operator* timestamp</span></span> |+ |<span data-ttu-id="305a5-279">časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-279">timestamp</span></span> |
| <span data-ttu-id="305a5-280">časové razítko *operátor* timeinterval</span><span class="sxs-lookup"><span data-stu-id="305a5-280">timestamp *operator* timeinterval</span></span> |+ |<span data-ttu-id="305a5-281">časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-281">timestamp</span></span> |
| <span data-ttu-id="305a5-282">časové razítko *operátor* časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-282">timestamp *operator* timestamp</span></span> |- |<span data-ttu-id="305a5-283">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-283">timeinterval</span></span> |
| <span data-ttu-id="305a5-284">*operátor*double</span><span class="sxs-lookup"><span data-stu-id="305a5-284">*operator*double</span></span> |<span data-ttu-id="305a5-285">-, !</span><span class="sxs-lookup"><span data-stu-id="305a5-285">-, !</span></span> |<span data-ttu-id="305a5-286">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-286">double</span></span> |
| <span data-ttu-id="305a5-287">*operátor*timeinterval</span><span class="sxs-lookup"><span data-stu-id="305a5-287">*operator*timeinterval</span></span> |- |<span data-ttu-id="305a5-288">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="305a5-288">timeinterval</span></span> |
| <span data-ttu-id="305a5-289">dvojité *operátor* double</span><span class="sxs-lookup"><span data-stu-id="305a5-289">double *operator* double</span></span> |<span data-ttu-id="305a5-290"><, <=, ==, >=, >, !=</span><span class="sxs-lookup"><span data-stu-id="305a5-290"><, <=, ==, >=, >, !=</span></span> |<span data-ttu-id="305a5-291">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-291">double</span></span> |
| <span data-ttu-id="305a5-292">řetězec *operátor* řetězec</span><span class="sxs-lookup"><span data-stu-id="305a5-292">string *operator* string</span></span> |<span data-ttu-id="305a5-293"><, <=, ==, >=, >, !=</span><span class="sxs-lookup"><span data-stu-id="305a5-293"><, <=, ==, >=, >, !=</span></span> |<span data-ttu-id="305a5-294">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-294">double</span></span> |
| <span data-ttu-id="305a5-295">časové razítko *operátor* časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-295">timestamp *operator* timestamp</span></span> |<span data-ttu-id="305a5-296"><, <=, ==, >=, >, !=</span><span class="sxs-lookup"><span data-stu-id="305a5-296"><, <=, ==, >=, >, !=</span></span> |<span data-ttu-id="305a5-297">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-297">double</span></span> |
| <span data-ttu-id="305a5-298">TimeInterval *operátor* timeinterval</span><span class="sxs-lookup"><span data-stu-id="305a5-298">timeinterval *operator* timeinterval</span></span> |<span data-ttu-id="305a5-299"><, <=, ==, >=, >, !=</span><span class="sxs-lookup"><span data-stu-id="305a5-299"><, <=, ==, >=, >, !=</span></span> |<span data-ttu-id="305a5-300">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-300">double</span></span> |
| <span data-ttu-id="305a5-301">dvojité *operátor* double</span><span class="sxs-lookup"><span data-stu-id="305a5-301">double *operator* double</span></span> |<span data-ttu-id="305a5-302">&&, &#124;&#124;</span><span class="sxs-lookup"><span data-stu-id="305a5-302">&&, &#124;&#124;</span></span> |<span data-ttu-id="305a5-303">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-303">double</span></span> |

<span data-ttu-id="305a5-304">Při testování dvojitou s Ternární operátor (`double ? statement1 : statement2`), nenulové hodnoty je **true**, a je nula **false**.</span><span class="sxs-lookup"><span data-stu-id="305a5-304">When testing a double with a ternary operator (`double ? statement1 : statement2`), nonzero is **true**, and zero is **false**.</span></span>

## <a name="functions"></a><span data-ttu-id="305a5-305">Funkce</span><span class="sxs-lookup"><span data-stu-id="305a5-305">Functions</span></span>
<span data-ttu-id="305a5-306">Tyto předdefinované **funkce** jsou k dispozici pro použití k definování vzorec automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-306">These predefined **functions** are available for you to use in defining an automatic scaling formula.</span></span>

| <span data-ttu-id="305a5-307">Funkce</span><span class="sxs-lookup"><span data-stu-id="305a5-307">Function</span></span> | <span data-ttu-id="305a5-308">Návratový typ</span><span class="sxs-lookup"><span data-stu-id="305a5-308">Return type</span></span> | <span data-ttu-id="305a5-309">Popis</span><span class="sxs-lookup"><span data-stu-id="305a5-309">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="305a5-310">AVG(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-310">avg(doubleVecList)</span></span> |<span data-ttu-id="305a5-311">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-311">double</span></span> |<span data-ttu-id="305a5-312">Vrátí průměrnou hodnotu pro všechny hodnoty v doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-312">Returns the average value for all values in the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-313">Len(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-313">len(doubleVecList)</span></span> |<span data-ttu-id="305a5-314">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-314">double</span></span> |<span data-ttu-id="305a5-315">Vrátí délku vektor, který je vytvořený z doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-315">Returns the length of the vector that is created from the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-316">LG(Double)</span><span class="sxs-lookup"><span data-stu-id="305a5-316">lg(double)</span></span> |<span data-ttu-id="305a5-317">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-317">double</span></span> |<span data-ttu-id="305a5-318">Vrátí protokol základní 2 dvojitou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="305a5-318">Returns the log base 2 of the double.</span></span> |
| <span data-ttu-id="305a5-319">LG(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-319">lg(doubleVecList)</span></span> |<span data-ttu-id="305a5-320">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-320">doubleVec</span></span> |<span data-ttu-id="305a5-321">Vrátí základní 2 doubleVecList component-wise protokolu.</span><span class="sxs-lookup"><span data-stu-id="305a5-321">Returns the component-wise log base 2 of the doubleVecList.</span></span> <span data-ttu-id="305a5-322">Pro parametr musí být explicitně předán vec(double).</span><span class="sxs-lookup"><span data-stu-id="305a5-322">A vec(double) must be explicitly passed for the parameter.</span></span> <span data-ttu-id="305a5-323">Jinak se předpokládá, double lg(double) verze.</span><span class="sxs-lookup"><span data-stu-id="305a5-323">Otherwise, the double lg(double) version is assumed.</span></span> |
| <span data-ttu-id="305a5-324">ln(Double)</span><span class="sxs-lookup"><span data-stu-id="305a5-324">ln(double)</span></span> |<span data-ttu-id="305a5-325">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-325">double</span></span> |<span data-ttu-id="305a5-326">Vrátí přirozené protokol dvojitou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="305a5-326">Returns the natural log of the double.</span></span> |
| <span data-ttu-id="305a5-327">ln(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-327">ln(doubleVecList)</span></span> |<span data-ttu-id="305a5-328">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-328">doubleVec</span></span> |<span data-ttu-id="305a5-329">Vrátí základní 2 doubleVecList component-wise protokolu.</span><span class="sxs-lookup"><span data-stu-id="305a5-329">Returns the component-wise log base 2 of the doubleVecList.</span></span> <span data-ttu-id="305a5-330">Pro parametr musí být explicitně předán vec(double).</span><span class="sxs-lookup"><span data-stu-id="305a5-330">A vec(double) must be explicitly passed for the parameter.</span></span> <span data-ttu-id="305a5-331">Jinak se předpokládá, double lg(double) verze.</span><span class="sxs-lookup"><span data-stu-id="305a5-331">Otherwise, the double lg(double) version is assumed.</span></span> |
| <span data-ttu-id="305a5-332">log(Double)</span><span class="sxs-lookup"><span data-stu-id="305a5-332">log(double)</span></span> |<span data-ttu-id="305a5-333">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-333">double</span></span> |<span data-ttu-id="305a5-334">Vrátí protokol základní 10 dvojitou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="305a5-334">Returns the log base 10 of the double.</span></span> |
| <span data-ttu-id="305a5-335">log(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-335">log(doubleVecList)</span></span> |<span data-ttu-id="305a5-336">doubleVec</span><span class="sxs-lookup"><span data-stu-id="305a5-336">doubleVec</span></span> |<span data-ttu-id="305a5-337">Vrátí základní 10 doubleVecList component-wise protokolu.</span><span class="sxs-lookup"><span data-stu-id="305a5-337">Returns the component-wise log base 10 of the doubleVecList.</span></span> <span data-ttu-id="305a5-338">Pro parametr jeden double musí být explicitně předán vec(double).</span><span class="sxs-lookup"><span data-stu-id="305a5-338">A vec(double) must be explicitly passed for the single double parameter.</span></span> <span data-ttu-id="305a5-339">Jinak se předpokládá, double log(double) verze.</span><span class="sxs-lookup"><span data-stu-id="305a5-339">Otherwise, the double log(double) version is assumed.</span></span> |
| <span data-ttu-id="305a5-340">Max(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-340">max(doubleVecList)</span></span> |<span data-ttu-id="305a5-341">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-341">double</span></span> |<span data-ttu-id="305a5-342">Vrátí maximální hodnotu ve doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-342">Returns the maximum value in the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-343">min(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-343">min(doubleVecList)</span></span> |<span data-ttu-id="305a5-344">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-344">double</span></span> |<span data-ttu-id="305a5-345">Vrátí minimální hodnotu ve doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-345">Returns the minimum value in the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-346">Norm(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-346">norm(doubleVecList)</span></span> |<span data-ttu-id="305a5-347">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-347">double</span></span> |<span data-ttu-id="305a5-348">Vrátí dva norm vektoru, který je vytvořený z doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-348">Returns the two-norm of the vector that is created from the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-349">percentil (doubleVec v dvojité p)</span><span class="sxs-lookup"><span data-stu-id="305a5-349">percentile(doubleVec v, double p)</span></span> |<span data-ttu-id="305a5-350">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-350">double</span></span> |<span data-ttu-id="305a5-351">Vrátí element percentilu vektoru v.</span><span class="sxs-lookup"><span data-stu-id="305a5-351">Returns the percentile element of the vector v.</span></span> |
| <span data-ttu-id="305a5-352">rand()</span><span class="sxs-lookup"><span data-stu-id="305a5-352">rand()</span></span> |<span data-ttu-id="305a5-353">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-353">double</span></span> |<span data-ttu-id="305a5-354">Vrátí náhodná hodnota mezi 0,0 a 1,0.</span><span class="sxs-lookup"><span data-stu-id="305a5-354">Returns a random value between 0.0 and 1.0.</span></span> |
| <span data-ttu-id="305a5-355">Range(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-355">range(doubleVecList)</span></span> |<span data-ttu-id="305a5-356">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-356">double</span></span> |<span data-ttu-id="305a5-357">Vrátí rozdíl mezi minimální a maximální hodnoty doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-357">Returns the difference between the min and max values in the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-358">STD(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-358">std(doubleVecList)</span></span> |<span data-ttu-id="305a5-359">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-359">double</span></span> |<span data-ttu-id="305a5-360">Vrátí směrodatnou odchylku ukázkové hodnoty v doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-360">Returns the sample standard deviation of the values in the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-361">stop()</span><span class="sxs-lookup"><span data-stu-id="305a5-361">stop()</span></span> | |<span data-ttu-id="305a5-362">Zastaví vyhodnocování výrazu automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-362">Stops evaluation of the autoscaling expression.</span></span> |
| <span data-ttu-id="305a5-363">SUM(doubleVecList)</span><span class="sxs-lookup"><span data-stu-id="305a5-363">sum(doubleVecList)</span></span> |<span data-ttu-id="305a5-364">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-364">double</span></span> |<span data-ttu-id="305a5-365">Vrátí součet všech součástí doubleVecList.</span><span class="sxs-lookup"><span data-stu-id="305a5-365">Returns the sum of all the components of the doubleVecList.</span></span> |
| <span data-ttu-id="305a5-366">čas (řetězce data a času = "")</span><span class="sxs-lookup"><span data-stu-id="305a5-366">time(string dateTime="")</span></span> |<span data-ttu-id="305a5-367">časové razítko</span><span class="sxs-lookup"><span data-stu-id="305a5-367">timestamp</span></span> |<span data-ttu-id="305a5-368">Vrátí časového razítka aktuální čas, pokud jsou předány bez parametrů nebo časového razítka řetězce data a času, je-li předán.</span><span class="sxs-lookup"><span data-stu-id="305a5-368">Returns the time stamp of the current time if no parameters are passed, or the time stamp of the dateTime string if it is passed.</span></span> <span data-ttu-id="305a5-369">Data a času podporované formáty jsou W3C DTF a RFC 1123.</span><span class="sxs-lookup"><span data-stu-id="305a5-369">Supported dateTime formats are W3C-DTF and RFC 1123.</span></span> |
| <span data-ttu-id="305a5-370">Val (doubleVec v dvojité i)</span><span class="sxs-lookup"><span data-stu-id="305a5-370">val(doubleVec v, double i)</span></span> |<span data-ttu-id="305a5-371">Double</span><span class="sxs-lookup"><span data-stu-id="305a5-371">double</span></span> |<span data-ttu-id="305a5-372">Vrátí hodnotu elementu, který je v umístění i u funkce vector v, počáteční index nula.</span><span class="sxs-lookup"><span data-stu-id="305a5-372">Returns the value of the element that is at location i in vector v, with a starting index of zero.</span></span> |

<span data-ttu-id="305a5-373">Některé funkce, které jsou popsané v předchozí tabulce může přijmout seznam jako argument.</span><span class="sxs-lookup"><span data-stu-id="305a5-373">Some of the functions that are described in the previous table can accept a list as an argument.</span></span> <span data-ttu-id="305a5-374">Čárkami oddělený seznam je libovolnou kombinaci *dvojité* a *doubleVec*.</span><span class="sxs-lookup"><span data-stu-id="305a5-374">The comma-separated list is any combination of *double* and *doubleVec*.</span></span> <span data-ttu-id="305a5-375">Například:</span><span class="sxs-lookup"><span data-stu-id="305a5-375">For example:</span></span>

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

<span data-ttu-id="305a5-376">*DoubleVecList* hodnota je převedena na jednu *doubleVec* před vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="305a5-376">The *doubleVecList* value is converted to a single *doubleVec* before evaluation.</span></span> <span data-ttu-id="305a5-377">Například pokud `v = [1,2,3]`, pak volání `avg(v)` je ekvivalentní volání `avg(1,2,3)`.</span><span class="sxs-lookup"><span data-stu-id="305a5-377">For example, if `v = [1,2,3]`, then calling `avg(v)` is equivalent to calling `avg(1,2,3)`.</span></span> <span data-ttu-id="305a5-378">Volání metody `avg(v, 7)` je ekvivalentní volání `avg(1,2,3,7)`.</span><span class="sxs-lookup"><span data-stu-id="305a5-378">Calling `avg(v, 7)` is equivalent to calling `avg(1,2,3,7)`.</span></span>

## <span data-ttu-id="305a5-379"><a name="getsampledata"></a>Získat ukázková data</span><span class="sxs-lookup"><span data-stu-id="305a5-379"><a name="getsampledata"></a>Obtain sample data</span></span>
<span data-ttu-id="305a5-380">Vzorce automatického škálování fungují na metriky dat (Ukázky), které poskytuje služba Batch.</span><span class="sxs-lookup"><span data-stu-id="305a5-380">Autoscale formulas act on metrics data (samples) that is provided by the Batch service.</span></span> <span data-ttu-id="305a5-381">Vzorec zvětšováním nebo zmenšováním velikost fondu na základě hodnot, které se získá od služby.</span><span class="sxs-lookup"><span data-stu-id="305a5-381">A formula grows or shrinks pool size based on the values that it obtains from the service.</span></span> <span data-ttu-id="305a5-382">Proměnné definované služby, které bylo popsáno dříve jsou objekty, které poskytují různé metody pro přístup k datům, která souvisí s Tento objekt.</span><span class="sxs-lookup"><span data-stu-id="305a5-382">The service-defined variables that were described previously are objects that provide various methods to access data that is associated with that object.</span></span> <span data-ttu-id="305a5-383">Například následující výraz zobrazuje požadavek na načtení za posledních pět minut využití procesoru:</span><span class="sxs-lookup"><span data-stu-id="305a5-383">For example, the following expression shows a request to get the last five minutes of CPU usage:</span></span>

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| <span data-ttu-id="305a5-384">Metoda</span><span class="sxs-lookup"><span data-stu-id="305a5-384">Method</span></span> | <span data-ttu-id="305a5-385">Popis</span><span class="sxs-lookup"><span data-stu-id="305a5-385">Description</span></span> |
| --- | --- |
| <span data-ttu-id="305a5-386">GetSample()</span><span class="sxs-lookup"><span data-stu-id="305a5-386">GetSample()</span></span> |<span data-ttu-id="305a5-387">`GetSample()` Metoda vrátí Vektor vzorků dat.</span><span class="sxs-lookup"><span data-stu-id="305a5-387">The `GetSample()` method returns a vector of data samples.</span></span><br/><br/><span data-ttu-id="305a5-388">Ukázka je 30 sekund vhodné dat metriky.</span><span class="sxs-lookup"><span data-stu-id="305a5-388">A sample is 30 seconds worth of metrics data.</span></span> <span data-ttu-id="305a5-389">Jinými slovy ukázky jsou získávány každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="305a5-389">In other words, samples are obtained every 30 seconds.</span></span> <span data-ttu-id="305a5-390">Ale dole uvedených položek, dochází ke zpoždění mezi při shromažďování ukázku a kdy je k dispozici pro vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-390">But as noted below, there is a delay between when a sample is collected and when it is available to a formula.</span></span> <span data-ttu-id="305a5-391">Ne všechny ukázky pro dané časové období jako takový, může být dostupné pro vyhodnocení podle vzorce.</span><span class="sxs-lookup"><span data-stu-id="305a5-391">As such, not all samples for a given time period may be available for evaluation by a formula.</span></span><ul><li>`doubleVec GetSample(double count)`<br/><span data-ttu-id="305a5-392">Určuje počet vzorků získat z nejnovější vzorků, které byly shromážděny.</span><span class="sxs-lookup"><span data-stu-id="305a5-392">Specifies the number of samples to obtain from the most recent samples that were collected.</span></span><br/><br/><span data-ttu-id="305a5-393">`GetSample(1)`vrací posledního vzorku k dispozici.</span><span class="sxs-lookup"><span data-stu-id="305a5-393">`GetSample(1)` returns the last available sample.</span></span> <span data-ttu-id="305a5-394">Pro metriky `$CPUPercent`, ale to nesmí použít, protože není možné vědět *při* vzorku nebyla shromážděna.</span><span class="sxs-lookup"><span data-stu-id="305a5-394">For metrics like `$CPUPercent`, however, this should not be used because it is impossible to know *when* the sample was collected.</span></span> <span data-ttu-id="305a5-395">Může to být poslední, nebo kvůli problémům s systému, může být mnohem starší.</span><span class="sxs-lookup"><span data-stu-id="305a5-395">It might be recent, or, because of system issues, it might be much older.</span></span> <span data-ttu-id="305a5-396">Je lepší v takových případech používat časový interval, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="305a5-396">It is better in such cases to use a time interval as shown below.</span></span><li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/><span data-ttu-id="305a5-397">Určuje časový rámec pro shromažďování ukázková data.</span><span class="sxs-lookup"><span data-stu-id="305a5-397">Specifies a time frame for gathering sample data.</span></span> <span data-ttu-id="305a5-398">Volitelně můžete také určuje procento vzorků, které musí být k dispozici v požadovaný časový rámec.</span><span class="sxs-lookup"><span data-stu-id="305a5-398">Optionally, it also specifies the percentage of samples that must be available in the requested time frame.</span></span><br/><br/><span data-ttu-id="305a5-399">`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Vrátí 20 vzorků v případě se nacházejí v historii CPUPercent všechny ukázky pro posledních 10 minut.</span><span class="sxs-lookup"><span data-stu-id="305a5-399">`$CPUPercent.GetSample(TimeInterval_Minute * 10)` would return 20 samples if all samples for the last 10 minutes are present in the CPUPercent history.</span></span> <span data-ttu-id="305a5-400">Pokud poslední minutu historie nebyl k dispozici, ale pouze 18 ukázky by byla vrácena.</span><span class="sxs-lookup"><span data-stu-id="305a5-400">If the last minute of history was not available, however, only 18 samples would be returned.</span></span> <span data-ttu-id="305a5-401">V takovém případě:</span><span class="sxs-lookup"><span data-stu-id="305a5-401">In this case:</span></span><br/><br/><span data-ttu-id="305a5-402">`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`selže, protože nejsou k dispozici pouze 90 procent ukázky.</span><span class="sxs-lookup"><span data-stu-id="305a5-402">`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)` would fail because only 90 percent of the samples are available.</span></span><br/><br/><span data-ttu-id="305a5-403">`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`mohla být úspěšná.</span><span class="sxs-lookup"><span data-stu-id="305a5-403">`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)` would succeed.</span></span><li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/><span data-ttu-id="305a5-404">Určuje časový rámec pro shromažďování dat, s počáteční čas a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="305a5-404">Specifies a time frame for gathering data, with both a start time and an end time.</span></span><br/><br/><span data-ttu-id="305a5-405">Jak je uvedeno nahoře, dochází ke zpoždění mezi při shromažďování ukázku a kdy je k dispozici pro vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-405">As mentioned above, there is a delay between when a sample is collected and when it is available to a formula.</span></span> <span data-ttu-id="305a5-406">Vezměte v úvahu tato prodleva při použití `GetSample` metoda.</span><span class="sxs-lookup"><span data-stu-id="305a5-406">Consider this delay when you use the `GetSample` method.</span></span> <span data-ttu-id="305a5-407">V tématu `GetSamplePercent` níže.</span><span class="sxs-lookup"><span data-stu-id="305a5-407">See `GetSamplePercent` below.</span></span> |
| <span data-ttu-id="305a5-408">GetSamplePeriod()</span><span class="sxs-lookup"><span data-stu-id="305a5-408">GetSamplePeriod()</span></span> |<span data-ttu-id="305a5-409">Vrátí dobu vzorků, které byly provedeny v datové sadě historických ukázka.</span><span class="sxs-lookup"><span data-stu-id="305a5-409">Returns the period of samples that were taken in a historical sample data set.</span></span> |
| <span data-ttu-id="305a5-410">Count()</span><span class="sxs-lookup"><span data-stu-id="305a5-410">Count()</span></span> |<span data-ttu-id="305a5-411">Vrátí celkový počet vzorků, které se v historii metriky.</span><span class="sxs-lookup"><span data-stu-id="305a5-411">Returns the total number of samples in the metric history.</span></span> |
| <span data-ttu-id="305a5-412">HistoryBeginTime()</span><span class="sxs-lookup"><span data-stu-id="305a5-412">HistoryBeginTime()</span></span> |<span data-ttu-id="305a5-413">Vrátí časového razítka nejstarší ukázková data k dispozici pro metriku.</span><span class="sxs-lookup"><span data-stu-id="305a5-413">Returns the time stamp of the oldest available data sample for the metric.</span></span> |
| <span data-ttu-id="305a5-414">GetSamplePercent()</span><span class="sxs-lookup"><span data-stu-id="305a5-414">GetSamplePercent()</span></span> |<span data-ttu-id="305a5-415">Vrátí podíl vzorků, které jsou k dispozici v daném časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="305a5-415">Returns the percentage of samples that are available for a given time interval.</span></span> <span data-ttu-id="305a5-416">Například:</span><span class="sxs-lookup"><span data-stu-id="305a5-416">For example:</span></span><br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/><span data-ttu-id="305a5-417">Protože `GetSample` metoda selže, pokud procento vzorků, které se vrátí, je menší než `samplePercent` zadána, můžete použít `GetSamplePercent` metoda nejprve ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="305a5-417">Because the `GetSample` method fails if the percentage of samples returned is less than the `samplePercent` specified, you can use the `GetSamplePercent` method to check first.</span></span> <span data-ttu-id="305a5-418">Poté můžete provádět alternativní akce když dostatek ukázky jsou přítomny, bez zastavení automatické škálování vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="305a5-418">Then you can perform an alternate action if insufficient samples are present, without halting the automatic scaling evaluation.</span></span> |

### <a name="samples-sample-percentage-and-the-getsample-method"></a><span data-ttu-id="305a5-419">Ukázky, ukázka procento a *GetSample()* – metoda</span><span class="sxs-lookup"><span data-stu-id="305a5-419">Samples, sample percentage, and the *GetSample()* method</span></span>
<span data-ttu-id="305a5-420">Základní provoz od vzorce automatického škálování je získat data úlohy a prostředku metriky a upravte velikost fondu podle data.</span><span class="sxs-lookup"><span data-stu-id="305a5-420">The core operation of an autoscale formula is to obtain task and resource metric data and then adjust pool size based on that data.</span></span> <span data-ttu-id="305a5-421">Jako takový je důležité mít vědět, jak vzorce automatického škálování pracovat s daty metrik (Ukázky).</span><span class="sxs-lookup"><span data-stu-id="305a5-421">As such, it is important to have a clear understanding of how autoscale formulas interact with metrics data (samples).</span></span>

<span data-ttu-id="305a5-422">**Ukázky**</span><span class="sxs-lookup"><span data-stu-id="305a5-422">**Samples**</span></span>

<span data-ttu-id="305a5-423">Služba Batch pravidelně trvá ukázky úlohy a prostředku metriky a zpřístupňuje je vaše vzorce automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-423">The Batch service periodically takes samples of task and resource metrics and makes them available to your autoscale formulas.</span></span> <span data-ttu-id="305a5-424">Tyto ukázky se službou Batch zaznamenávají každých 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="305a5-424">These samples are recorded every 30 seconds by the Batch service.</span></span> <span data-ttu-id="305a5-425">Je však obvykle zpoždění mezi při nebyly zaznamenány tyto ukázky a když jsou zpřístupněné (a mohou být přečteny) vzorcích škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-425">However, there is typically a delay between when those samples were recorded and when they are made available to (and can be read by) your autoscale formulas.</span></span> <span data-ttu-id="305a5-426">Kromě toho z důvodu různých faktorech, například sítě nebo jiných problémů s infrastrukturou, nemusí být ukázky zaznamenány pro konkrétním intervalu.</span><span class="sxs-lookup"><span data-stu-id="305a5-426">Additionally, due to various factors such as network or other infrastructure issues, samples may not be recorded for a particular interval.</span></span>

<span data-ttu-id="305a5-427">**Ukázka procento**</span><span class="sxs-lookup"><span data-stu-id="305a5-427">**Sample percentage**</span></span>

<span data-ttu-id="305a5-428">Při `samplePercent` je předán `GetSample()` metoda nebo `GetSamplePercent()` metoda je volána, _procent_ odkazuje na porovnání mezi celkový možný počet vzorků, které se zaznamenávají službou Batch a počet ukázky, které jsou k dispozici pro vzorec škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-428">When `samplePercent` is passed to the `GetSample()` method or the `GetSamplePercent()` method is called, _percent_ refers to a comparison between the total possible number of samples that are recorded by the Batch service and the number of samples that are available to your autoscale formula.</span></span>

<span data-ttu-id="305a5-429">Podívejme se na časový interval 10 minut jako příklad.</span><span class="sxs-lookup"><span data-stu-id="305a5-429">Let's look at a 10-minute timespan as an example.</span></span> <span data-ttu-id="305a5-430">Protože vzorky se zaznamenávají každých 30 sekund v rámci časový interval 10 minut, bude maximální celkový počet vzorků, které se zaznamenávají dávkou 20 vzorků (2 za minutu).</span><span class="sxs-lookup"><span data-stu-id="305a5-430">Because samples are recorded every 30 seconds within a 10-minute timespan, the maximum total number of samples that are recorded by Batch would be 20 samples (2 per minute).</span></span> <span data-ttu-id="305a5-431">Však z důvodu čekací doby vyplývajících mechanismus vytváření sestav a další otázky v rámci Azure, může existovat pouze 15 vzorků, které jsou k dispozici pro vzorec škálování pro čtení.</span><span class="sxs-lookup"><span data-stu-id="305a5-431">However, due to the inherent latency of the reporting mechanism and other issues within Azure, there may be only 15 samples that are available to your autoscale formula for reading.</span></span> <span data-ttu-id="305a5-432">Ano například tuto dobu 10 minut 75 % celkového počtu vzorků, které zaznamenávají může být dostupné pro vzorce.</span><span class="sxs-lookup"><span data-stu-id="305a5-432">So, for example, for that 10-minute period, only 75% of the total number of samples recorded may be available to your formula.</span></span>

<span data-ttu-id="305a5-433">**GetSample() a ukázkové rozsahů**</span><span class="sxs-lookup"><span data-stu-id="305a5-433">**GetSample() and sample ranges**</span></span>

<span data-ttu-id="305a5-434">Vaše vzorce automatického škálování se chystáte se zvětšování a zmenšování fondech &mdash; uzly pro přidání nebo odebrání uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-434">Your autoscale formulas are going to be growing and shrinking your pools &mdash; adding nodes or removing nodes.</span></span> <span data-ttu-id="305a5-435">Vzhledem k tomu, že uzly náklady peníze, chcete zajistit, že vaše vzorce použijte metodu inteligentního analýzy, která je založená na dostatečného množství dat.</span><span class="sxs-lookup"><span data-stu-id="305a5-435">Because nodes cost you money, you want to ensure that your formulas use an intelligent method of analysis that is based on sufficient data.</span></span> <span data-ttu-id="305a5-436">Proto doporučujeme použít k analýze trendů typu ve vzorcích.</span><span class="sxs-lookup"><span data-stu-id="305a5-436">Therefore, we recommend that you use a trending-type analysis in your formulas.</span></span> <span data-ttu-id="305a5-437">Tento typ zvětšování a zmenší fondech na základě rozsahu shromažďovaných vzorků.</span><span class="sxs-lookup"><span data-stu-id="305a5-437">This type grows and shrinks your pools based on a range of collected samples.</span></span>

<span data-ttu-id="305a5-438">Chcete-li to provést, použijte `GetSample(interval look-back start, interval look-back end)` vrátit vektoru vzorků:</span><span class="sxs-lookup"><span data-stu-id="305a5-438">To do so, use `GetSample(interval look-back start, interval look-back end)` to return a vector of samples:</span></span>

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

<span data-ttu-id="305a5-439">Při výše řádku je vyhodnocován Batch, vrátí oblast ukázky vektor hodnot.</span><span class="sxs-lookup"><span data-stu-id="305a5-439">When the above line is evaluated by Batch, it returns a range of samples as a vector of values.</span></span> <span data-ttu-id="305a5-440">Například:</span><span class="sxs-lookup"><span data-stu-id="305a5-440">For example:</span></span>

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

<span data-ttu-id="305a5-441">Jakmile jste získány vektoru vzorků, pak můžete použít funkce jako `min()`, `max()`, a `avg()` odvodit smysluplné hodnoty od shromážděných rozsahu.</span><span class="sxs-lookup"><span data-stu-id="305a5-441">Once you've collected the vector of samples, you can then use functions like `min()`, `max()`, and `avg()` to derive meaningful values from the collected range.</span></span>

<span data-ttu-id="305a5-442">Pro dodatečné zabezpečení můžete vynutit vzorců nezdaří, pokud je nižší než určité procento ukázka je dostupná pro konkrétní časové období.</span><span class="sxs-lookup"><span data-stu-id="305a5-442">For additional security, you can force a formula evaluation to fail if less than a certain sample percentage is available for a particular time period.</span></span> <span data-ttu-id="305a5-443">Pokud vynutíte vzorců selhání, dáte pokyn dávky na vyhodnocení vzorec ustanou další, pokud zadané procento vzorků, které není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="305a5-443">When you force a formula evaluation to fail, you instruct Batch to cease further evaluation of the formula if the specified percentage of samples is not available.</span></span> <span data-ttu-id="305a5-444">V takovém případě je provedena žádná změna velikosti fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-444">In this case, no change is made to the pool size.</span></span> <span data-ttu-id="305a5-445">Pokud chcete zadat požadované procento ukázky pro vyhodnocení proběhla úspěšně, je zadat jako třetí parametr `GetSample()`.</span><span class="sxs-lookup"><span data-stu-id="305a5-445">To specify a required percentage of samples for the evaluation to succeed, specify it as the third parameter to `GetSample()`.</span></span> <span data-ttu-id="305a5-446">Zde je zadán požadavek 75 procent ukázky:</span><span class="sxs-lookup"><span data-stu-id="305a5-446">Here, a requirement of 75 percent of samples is specified:</span></span>

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

<span data-ttu-id="305a5-447">Protože ukázka dostupnosti může dojít ke zpoždění, je důležité vždy zadejte časový interval s časem zahájení vzhled zpět, která jsou starší než jedna minuta.</span><span class="sxs-lookup"><span data-stu-id="305a5-447">Because there may be a delay in sample availability, it is important to always specify a time range with a look-back start time that is older than one minute.</span></span> <span data-ttu-id="305a5-448">Ho trvá přibližně jednu minutu ukázek rozšíří v rámci systému, takže ukázky v rozsahu `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="305a5-448">It takes approximately one minute for samples to propagate through the system, so samples in the range `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` may not be available.</span></span> <span data-ttu-id="305a5-449">Znovu, můžete použít parametr procento `GetSample()` vynutit požadavek procento konkrétní vzorek.</span><span class="sxs-lookup"><span data-stu-id="305a5-449">Again, you can use the percentage parameter of `GetSample()` to force a particular sample percentage requirement.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="305a5-450">Jsme **důrazně doporučujeme** , které **se spoléhat *pouze* na `GetSample(1)` ve vzorcích škálování**.</span><span class="sxs-lookup"><span data-stu-id="305a5-450">We **strongly recommend** that you **avoid relying *only* on `GetSample(1)` in your autoscale formulas**.</span></span> <span data-ttu-id="305a5-451">Důvodem je, že `GetSample(1)` v podstatě je uveden ve službě Batch, "Udělení mi poslední ukázkové, budete mít, bez ohledu na to, jak dávno načíst."</span><span class="sxs-lookup"><span data-stu-id="305a5-451">This is because `GetSample(1)` essentially says to the Batch service, "Give me the last sample you have, no matter how long ago you retrieved it."</span></span> <span data-ttu-id="305a5-452">Vzhledem k tomu, že je pouze jeden vzorku a může být starší ukázkové, nemusí být zástupce větší přehled o poslední úlohu nebo stav prostředku.</span><span class="sxs-lookup"><span data-stu-id="305a5-452">Since it is only a single sample, and it may be an older sample, it may not be representative of the larger picture of recent task or resource state.</span></span> <span data-ttu-id="305a5-453">Pokud používáte `GetSample(1)`, ujistěte se, že je součástí větší prohlášení a není pouze datový bod, který vzorec spoléhá na.</span><span class="sxs-lookup"><span data-stu-id="305a5-453">If you do use `GetSample(1)`, make sure that it's part of a larger statement and not the only data point that your formula relies on.</span></span>
>
>

## <a name="metrics"></a><span data-ttu-id="305a5-454">Metriky</span><span class="sxs-lookup"><span data-stu-id="305a5-454">Metrics</span></span>
<span data-ttu-id="305a5-455">Prostředek a úloha metriky můžete použít, když definujete vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-455">You can use both resource and task metrics when you're defining a formula.</span></span> <span data-ttu-id="305a5-456">Můžete upravit cílový počet vyhrazených uzlů ve fondu na základě metriky dat, který můžete získat a vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="305a5-456">You adjust the target number of dedicated nodes in the pool based on the metrics data that you obtain and evaluate.</span></span> <span data-ttu-id="305a5-457">Najdete v článku [proměnné](#variables) části výše pro další informace o jednotlivé metriky.</span><span class="sxs-lookup"><span data-stu-id="305a5-457">See the [Variables](#variables) section above for more information on each metric.</span></span>

<table>
  <tr>
    <th><span data-ttu-id="305a5-458">Metrika</span><span class="sxs-lookup"><span data-stu-id="305a5-458">Metric</span></span></th>
    <th><span data-ttu-id="305a5-459">Popis</span><span class="sxs-lookup"><span data-stu-id="305a5-459">Description</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="305a5-460"><b>Prostředek</b></span><span class="sxs-lookup"><span data-stu-id="305a5-460"><b>Resource</b></span></span></td>
    <td><p><span data-ttu-id="305a5-461">Metrika prostředků jsou založené na procesoru, šířky pásma, využití paměti výpočetních uzlů a počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-461">Resource metrics are based on the CPU, the bandwidth, the memory usage of compute nodes, and the number of nodes.</span></span></p>
        <p> <span data-ttu-id="305a5-462">Tyto proměnné definované služby jsou užitečné pro provádění úprav na základě počtu uzlu:</span><span class="sxs-lookup"><span data-stu-id="305a5-462">These service-defined variables are useful for making adjustments based on node count:</span></span></p>
    <p><ul>
            <li><span data-ttu-id="305a5-463">$TargetDedicatedNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-463">$TargetDedicatedNodes</span></span></li>
            <li><span data-ttu-id="305a5-464">$TargetLowPriorityNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-464">$TargetLowPriorityNodes</span></span></li>
            <li><span data-ttu-id="305a5-465">$CurrentDedicatedNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-465">$CurrentDedicatedNodes</span></span></li>
            <li><span data-ttu-id="305a5-466">$CurrentLowPriorityNodes</span><span class="sxs-lookup"><span data-stu-id="305a5-466">$CurrentLowPriorityNodes</span></span></li>
            <li><span data-ttu-id="305a5-467">$preemptedNodeCount</span><span class="sxs-lookup"><span data-stu-id="305a5-467">$preemptedNodeCount</span></span></li>
            <li><span data-ttu-id="305a5-468">$SampleNodeCount</span><span class="sxs-lookup"><span data-stu-id="305a5-468">$SampleNodeCount</span></span></li>
    </ul></p>
    <p><span data-ttu-id="305a5-469">Tyto proměnné definované služby jsou užitečné pro provádění úprav na základě využití prostředků uzlu:</span><span class="sxs-lookup"><span data-stu-id="305a5-469">These service-defined variables are useful for making adjustments based on node resource usage:</span></span></p>
    <p><ul>
      <li><span data-ttu-id="305a5-470">$CPUPercent</span><span class="sxs-lookup"><span data-stu-id="305a5-470">$CPUPercent</span></span></li>
      <li><span data-ttu-id="305a5-471">$WallClockSeconds</span><span class="sxs-lookup"><span data-stu-id="305a5-471">$WallClockSeconds</span></span></li>
      <li><span data-ttu-id="305a5-472">$MemoryBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-472">$MemoryBytes</span></span></li>
      <li><span data-ttu-id="305a5-473">$DiskBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-473">$DiskBytes</span></span></li>
      <li><span data-ttu-id="305a5-474">$DiskReadBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-474">$DiskReadBytes</span></span></li>
      <li><span data-ttu-id="305a5-475">$DiskWriteBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-475">$DiskWriteBytes</span></span></li>
      <li><span data-ttu-id="305a5-476">$DiskReadOps</span><span class="sxs-lookup"><span data-stu-id="305a5-476">$DiskReadOps</span></span></li>
      <li><span data-ttu-id="305a5-477">$DiskWriteOps</span><span class="sxs-lookup"><span data-stu-id="305a5-477">$DiskWriteOps</span></span></li>
      <li><span data-ttu-id="305a5-478">$NetworkInBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-478">$NetworkInBytes</span></span></li>
      <li><span data-ttu-id="305a5-479">$NetworkOutBytes</span><span class="sxs-lookup"><span data-stu-id="305a5-479">$NetworkOutBytes</span></span></li></ul></p>
  </tr>
  <tr>
    <td><span data-ttu-id="305a5-480"><b>Úkol</b></span><span class="sxs-lookup"><span data-stu-id="305a5-480"><b>Task</b></span></span></td>
    <td><p><span data-ttu-id="305a5-481">Metriky úloh jsou na základě stavu úkolů, například aktivní, čeká na vyřízení a byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="305a5-481">Task metrics are based on the status of tasks, such as Active, Pending, and Completed.</span></span> <span data-ttu-id="305a5-482">Následující proměnné definované služby jsou užitečné pro provádění úprav velikost fondu na základě metriky úloh:</span><span class="sxs-lookup"><span data-stu-id="305a5-482">The following service-defined variables are useful for making pool-size adjustments based on task metrics:</span></span></p>
    <p><ul>
      <li><span data-ttu-id="305a5-483">$ActiveTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-483">$ActiveTasks</span></span></li>
      <li><span data-ttu-id="305a5-484">$RunningTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-484">$RunningTasks</span></span></li>
      <li><span data-ttu-id="305a5-485">$PendingTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-485">$PendingTasks</span></span></li>
      <li><span data-ttu-id="305a5-486">$SucceededTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-486">$SucceededTasks</span></span></li>
            <li><span data-ttu-id="305a5-487">$FailedTasks</span><span class="sxs-lookup"><span data-stu-id="305a5-487">$FailedTasks</span></span></li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a><span data-ttu-id="305a5-488">Zápis vzorec škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-488">Write an autoscale formula</span></span>
<span data-ttu-id="305a5-489">Sestavení se vzorec škálování vytvořením příkazy, které používají komponenty výše a potom kombinace těchto příkazů do dokončení vzorce.</span><span class="sxs-lookup"><span data-stu-id="305a5-489">You build an autoscale formula by forming statements that use the above components, then combine those statements into a complete formula.</span></span> <span data-ttu-id="305a5-490">V této části vytvoříme vzorec škálování příklad, který může provádět některé reálného škálování rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="305a5-490">In this section, we create an example autoscale formula that can perform some real-world scaling decisions.</span></span>

<span data-ttu-id="305a5-491">Nejdříve definujme požadavky pro naše nové vzorec škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-491">First, let's define the requirements for our new autoscale formula.</span></span> <span data-ttu-id="305a5-492">Vzorec proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="305a5-492">The formula should:</span></span>

1. <span data-ttu-id="305a5-493">Zvýšit číslo cílového vyhrazené výpočetních uzlů ve fondu, pokud využití CPU je vysoké.</span><span class="sxs-lookup"><span data-stu-id="305a5-493">Increase the target number of dedicated compute nodes in a pool if CPU usage is high.</span></span>
2. <span data-ttu-id="305a5-494">Při nízkém využití procesoru, snížit cílový počet vyhrazených výpočetních uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-494">Decrease the target number of dedicated compute nodes in a pool when CPU usage is low.</span></span>
3. <span data-ttu-id="305a5-495">Maximální počet vyhrazených uzlů vždy omezte na 400.</span><span class="sxs-lookup"><span data-stu-id="305a5-495">Always restrict the maximum number of dedicated nodes to 400.</span></span>

<span data-ttu-id="305a5-496">Pokud chcete zvýšit počet uzlů během vysoké využití procesoru, zadejte příkaz, který naplní uživatelem definované proměnné (`$totalDedicatedNodes`) s hodnotou, která je 110 procent aktuální cílový počet vyhrazených uzlů, ale jenom v případě minimální průměrné využití procesoru během posledních 10 minut byla vyšší než 70 procent.</span><span class="sxs-lookup"><span data-stu-id="305a5-496">To increase the number of nodes during high CPU usage, define the statement that populates a user-defined variable (`$totalDedicatedNodes`) with a value that is 110 percent of the current target number of dedicated nodes, but only if the minimum average CPU usage during the last 10 minutes was above 70 percent.</span></span> <span data-ttu-id="305a5-497">Jinak použijte hodnotu pro aktuální počet vyhrazených uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-497">Otherwise, use the value for the current number of dedicated nodes.</span></span>

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

<span data-ttu-id="305a5-498">K *snížit* počet vyhrazených uzlů při nízkém zatížení procesoru, další příkaz v našem vzorec nastaví stejné `$totalDedicatedNodes` proměnné až 90 procent aktuální cílový počet vyhrazených uzlů, pokud průměrné využití procesoru za posledních 60 minut se v části 20 procent.</span><span class="sxs-lookup"><span data-stu-id="305a5-498">To *decrease* the number of dedicated nodes during low CPU usage, the next statement in our formula sets the same `$totalDedicatedNodes` variable to 90 percent of the current target number of dedicated nodes if the average CPU usage in the past 60 minutes was under 20 percent.</span></span> <span data-ttu-id="305a5-499">Jinak použijte aktuální hodnota `$totalDedicatedNodes` , jsme naplněno v příkazu výše.</span><span class="sxs-lookup"><span data-stu-id="305a5-499">Otherwise, use the current value of `$totalDedicatedNodes` that we populated in the statement above.</span></span>

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

<span data-ttu-id="305a5-500">Nyní omezte cílový počet vyhrazených výpočetní uzly maximálně 400:</span><span class="sxs-lookup"><span data-stu-id="305a5-500">Now limit the target number of dedicated compute nodes to a maximum of 400:</span></span>

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

<span data-ttu-id="305a5-501">Tady je kompletní vzorec:</span><span class="sxs-lookup"><span data-stu-id="305a5-501">Here's the complete formula:</span></span>

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a><span data-ttu-id="305a5-502">Vytvoření fondu povoleno škálování s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="305a5-502">Create an autoscale-enabled pool with .NET</span></span>

<span data-ttu-id="305a5-503">K vytvoření fondu pomocí automatické škálování povolené v rozhraní .NET, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="305a5-503">To create a pool with autoscaling enabled in .NET, follow these steps:</span></span>

1. <span data-ttu-id="305a5-504">Vytvoření fondu s [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).</span><span class="sxs-lookup"><span data-stu-id="305a5-504">Create the pool with [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).</span></span>
2. <span data-ttu-id="305a5-505">Nastavte [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="305a5-505">Set the [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) property to `true`.</span></span>
3. <span data-ttu-id="305a5-506">Nastavte [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) vlastnost s vzorec škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-506">Set the [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) property with your autoscale formula.</span></span>
4. <span data-ttu-id="305a5-507">(Volitelné) Nastavte [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) vlastnosti (výchozí hodnota je 15 minut).</span><span class="sxs-lookup"><span data-stu-id="305a5-507">(Optional) Set the [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) property (default is 15 minutes).</span></span>
5. <span data-ttu-id="305a5-508">Potvrdit fondu s [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) nebo [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).</span><span class="sxs-lookup"><span data-stu-id="305a5-508">Commit the pool with [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) or [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).</span></span>

<span data-ttu-id="305a5-509">Následující fragment kódu vytvoří fond škálování povolené v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="305a5-509">The following code snippet creates an autoscale-enabled pool in .NET.</span></span> <span data-ttu-id="305a5-510">Vzorec škálování fondu nastaví cílový počet vyhrazených uzlů až 5 pro pondělí a 1 na každý den v týdnu.</span><span class="sxs-lookup"><span data-stu-id="305a5-510">The pool's autoscale formula sets the target number of dedicated nodes to 5 on Mondays, and 1 on every other day of the week.</span></span> <span data-ttu-id="305a5-511">[Interval automatického škálování](#automatic-scaling-interval) nastavena na 30 minut.</span><span class="sxs-lookup"><span data-stu-id="305a5-511">The [automatic scaling interval](#automatic-scaling-interval) is set to 30 minutes.</span></span> <span data-ttu-id="305a5-512">V tomto a dalších C# fragmenty kódu v tomto článku `myBatchClient` je správně inicializována instanci [BatchClient] [ net_batchclient] třídy.</span><span class="sxs-lookup"><span data-stu-id="305a5-512">In this and the other C# snippets in this article, `myBatchClient` is a properly initialized instance of the [BatchClient][net_batchclient] class.</span></span>

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="305a5-513">Když vytvoříte fond povoleno automatické škálování, nezadávejte _targetDedicatedComputeNodes_ parametr nebo _targetLowPriorityComputeNodes_ parametr při volání **CreatePool** .</span><span class="sxs-lookup"><span data-stu-id="305a5-513">When you create an autoscale-enabled pool, do not specify the _targetDedicatedComputeNodes_ parameter or the _targetLowPriorityComputeNodes_ parameter on the call to **CreatePool**.</span></span> <span data-ttu-id="305a5-514">Místo toho zadat **AutoScaleEnabled** a **AutoScaleFormula** vlastnosti ve fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-514">Instead, specify the **AutoScaleEnabled** and **AutoScaleFormula** properties on the pool.</span></span> <span data-ttu-id="305a5-515">Hodnoty pro tyto vlastnosti určují cílový počet každý typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="305a5-515">The values for these properties determine the target number of each type of node.</span></span> <span data-ttu-id="305a5-516">Také do ručně škálování povoleným změny velikosti fondu (například s [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), první **zakázat** automatické škálování na fond a jeho velikost.</span><span class="sxs-lookup"><span data-stu-id="305a5-516">Also, to manually resize an autoscale-enabled pool (for example, with [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), first **disable** automatic scaling on the pool, then resize it.</span></span>
>
>

<span data-ttu-id="305a5-517">Kromě Batch .NET, můžete použít některý z dalších [SDK služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [rutiny prostředí PowerShell Batch](batch-powershell-cmdlets-get-started.md)a [Batch CLI](batch-cli-get-started.md) na Konfigurace automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-517">In addition to Batch .NET, you can use any of the other [Batch SDKs](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell cmdlets](batch-powershell-cmdlets-get-started.md), and the [Batch CLI](batch-cli-get-started.md) to configure autoscaling.</span></span>


### <a name="automatic-scaling-interval"></a><span data-ttu-id="305a5-518">Interval automatického škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-518">Automatic scaling interval</span></span>
<span data-ttu-id="305a5-519">Ve výchozím nastavení služba Batch upraví velikost fondu podle jeho vzorec škálování každých 15 minut.</span><span class="sxs-lookup"><span data-stu-id="305a5-519">By default, the Batch service adjusts a pool's size according to its autoscale formula every 15 minutes.</span></span> <span data-ttu-id="305a5-520">Tento interval je možné konfigurovat pomocí následujících vlastností fondu:</span><span class="sxs-lookup"><span data-stu-id="305a5-520">This interval is configurable by using the following pool properties:</span></span>

* <span data-ttu-id="305a5-521">[CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)</span><span class="sxs-lookup"><span data-stu-id="305a5-521">[CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval] (Batch .NET)</span></span>
* <span data-ttu-id="305a5-522">[autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)</span><span class="sxs-lookup"><span data-stu-id="305a5-522">[autoScaleEvaluationInterval][rest_autoscaleinterval] (REST API)</span></span>

<span data-ttu-id="305a5-523">Minimální interval je pět minut a maximální hodnota je 168 hodin.</span><span class="sxs-lookup"><span data-stu-id="305a5-523">The minimum interval is five minutes, and the maximum is 168 hours.</span></span> <span data-ttu-id="305a5-524">Pokud je zadán interval mimo tento rozsah, služba Batch vrátí chybu chybný požadavek (400).</span><span class="sxs-lookup"><span data-stu-id="305a5-524">If an interval outside this range is specified, the Batch service returns a Bad Request (400) error.</span></span>

> [!NOTE]
> <span data-ttu-id="305a5-525">Automatické škálování není určen aktuálně reagovat na změny v méně než minutu, ale je určený upravit velikost fondu postupně jako při spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="305a5-525">Autoscaling is not currently intended to respond to changes in less than a minute, but rather is intended to adjust the size of your pool gradually as you run a workload.</span></span>
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a><span data-ttu-id="305a5-526">Povolit automatické škálování na existující fond</span><span class="sxs-lookup"><span data-stu-id="305a5-526">Enable autoscaling on an existing pool</span></span>

<span data-ttu-id="305a5-527">Každý Batch SDK poskytuje způsob, jak povolit automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-527">Each Batch SDK provides a way to enable autoscaling.</span></span> <span data-ttu-id="305a5-528">Například:</span><span class="sxs-lookup"><span data-stu-id="305a5-528">For example:</span></span>

* <span data-ttu-id="305a5-529">[BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)</span><span class="sxs-lookup"><span data-stu-id="305a5-529">[BatchClient.PoolOperations.EnableAutoScaleAsync][net_enableautoscaleasync] (Batch .NET)</span></span>
* <span data-ttu-id="305a5-530">[Povolit automatické škálování ve fondu] [ rest_enableautoscale] (REST API)</span><span class="sxs-lookup"><span data-stu-id="305a5-530">[Enable automatic scaling on a pool][rest_enableautoscale] (REST API)</span></span>

<span data-ttu-id="305a5-531">Když povolíte automatické škálování na existující fond, mějte na paměti následující body:</span><span class="sxs-lookup"><span data-stu-id="305a5-531">When you enable autoscaling on an existing pool, keep in mind the following points:</span></span>

* <span data-ttu-id="305a5-532">Když automatické škálování je aktuálně zakázaná ve fondu Pokud vydáte požadavek na povolení automatického škálování, je nutné zadat platný škálování vzorec Pokud vydáte požadavek.</span><span class="sxs-lookup"><span data-stu-id="305a5-532">If automatic scaling is currently disabled on the pool when you issue the request to enable autoscaling, you must specify a valid autoscale formula when you issue the request.</span></span> <span data-ttu-id="305a5-533">Volitelně můžete zadat interval testování škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-533">You can optionally specify an autoscale evaluation interval.</span></span> <span data-ttu-id="305a5-534">Pokud nezadáte intervalu, je použita výchozí hodnota 15 minut.</span><span class="sxs-lookup"><span data-stu-id="305a5-534">If you do not specify an interval, the default value of 15 minutes is used.</span></span>
* <span data-ttu-id="305a5-535">Pokud automatického škálování je aktuálně povoleno ve fondu, můžete vzorec škálování, intervalu vyhodnocení nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="305a5-535">If autoscale is currently enabled on the pool, you can specify an autoscale formula, an evaluation interval, or both.</span></span> <span data-ttu-id="305a5-536">Musíte zadat alespoň jednu z těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="305a5-536">You must specify at least one of these properties.</span></span>

  * <span data-ttu-id="305a5-537">Pokud zadáte nový vyhodnocení interval škálování, je zastavena existující plán vyhodnocení a spuštění nový plán.</span><span class="sxs-lookup"><span data-stu-id="305a5-537">If you specify a new autoscale evaluation interval, then the existing evaluation schedule is stopped and a new schedule is started.</span></span> <span data-ttu-id="305a5-538">Nový plán spuštění je čas, kdy byl vydán požadavek na povolení automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-538">The new schedule's start time is the time at which the request to enable autoscaling was issued.</span></span>
  * <span data-ttu-id="305a5-539">Pokud vynecháte vzorec škálování nebo vyhodnocení interval, služba Batch budou nadále používat aktuální hodnota tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="305a5-539">If you omit either the autoscale formula or evaluation interval, the Batch service continues to use the current value of that setting.</span></span>

> [!NOTE]
> <span data-ttu-id="305a5-540">Pokud jste zadali hodnoty *targetDedicatedComputeNodes* nebo *targetLowPriorityComputeNodes* parametry **CreatePool** metoda při vytváření fondu v Rozhraní .NET, nebo pro porovnatelný z hlediska parametrů v jiném jazyce, pak tyto hodnoty jsou ignorovány, když vzorec automatického škálování je vyhodnocena.</span><span class="sxs-lookup"><span data-stu-id="305a5-540">If you specified values for the *targetDedicatedComputeNodes* or *targetLowPriorityComputeNodes* parameters of the **CreatePool** method when you created the pool in .NET, or for the comparable parameters in another language, then those values are ignored when the automatic scaling formula is evaluated.</span></span>
>
>

<span data-ttu-id="305a5-541">Používá tento fragment kódu jazyka C# [Batch .NET] [ net_api] knihovnu, která má-li povolit automatické škálování na existující fond:</span><span class="sxs-lookup"><span data-stu-id="305a5-541">This C# code snippet uses the [Batch .NET][net_api] library to enable autoscaling on an existing pool:</span></span>

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a><span data-ttu-id="305a5-542">Aktualizace se vzorec škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-542">Update an autoscale formula</span></span>

<span data-ttu-id="305a5-543">Aktualizovat vzorec na existující fond povoleno automatické škálování, voláním operace povolili automatické škálování znovu s novou vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-543">To update the formula on an existing autoscale-enabled pool, call the operation to enable autoscaling again with the new formula.</span></span> <span data-ttu-id="305a5-544">Například, pokud je již zapnuta automatické škálování `myexistingpool` při spuštění následující kódu .NET, jeho vzorec škálování se nahradí obsah `myNewFormula`.</span><span class="sxs-lookup"><span data-stu-id="305a5-544">For example, if autoscaling is already enabled on `myexistingpool` when the following .NET code is executed, its autoscale formula is replaced with the contents of `myNewFormula`.</span></span>

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a><span data-ttu-id="305a5-545">Aktualizovat interval škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-545">Update the autoscale interval</span></span>

<span data-ttu-id="305a5-546">Aktualizovat interval škálování vyhodnocení existujícího fondu povoleno automatické škálování, voláním operace povolili automatické škálování znovu s novou intervalu.</span><span class="sxs-lookup"><span data-stu-id="305a5-546">To update the autoscale evaluation interval of an existing autoscale-enabled pool, call the operation to enable autoscaling again with the new interval.</span></span> <span data-ttu-id="305a5-547">Chcete-li například nastavit interval testování škálování na 60 minut pro fond, který je již povolen škálování v rozhraní .NET:</span><span class="sxs-lookup"><span data-stu-id="305a5-547">For example, to set the autoscale evaluation interval to 60 minutes for a pool that's already autoscale-enabled in .NET:</span></span>

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a><span data-ttu-id="305a5-548">Vyhodnocení se vzorec škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-548">Evaluate an autoscale formula</span></span>

<span data-ttu-id="305a5-549">Vzorce můžete vyhodnotit před každým jejím použitím do fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-549">You can evaluate a formula before applying it to a pool.</span></span> <span data-ttu-id="305a5-550">Tímto způsobem můžete otestovat vzorec zobrazíte, jak vyhodnotit jeho příkazy před zahájením vzorec uvedení do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="305a5-550">In this way, you can test the formula to see how its statements evaluate before you put the formula into production.</span></span>

<span data-ttu-id="305a5-551">Abyste mohli vyhodnotit vzorec škálování, je třeba nejprve povolit automatické škálování ve fondu s platný vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-551">To evaluate an autoscale formula, you must first enable autoscaling on the pool with a valid formula.</span></span> <span data-ttu-id="305a5-552">K otestování vzorec na fond, který ještě nemá povoleno automatické škálování, použijte vzorec jednořádkové `$TargetDedicatedNodes = 0` když poprvé povolíte automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-552">To test a formula on a pool that doesn't yet have autoscaling enabled, use the one-line formula `$TargetDedicatedNodes = 0` when you first enable autoscaling.</span></span> <span data-ttu-id="305a5-553">Pak použijte jednu z následujících vyhodnotit vzorec, který chcete testovat:</span><span class="sxs-lookup"><span data-stu-id="305a5-553">Then, use one of the following to evaluate the formula you want to test:</span></span>

* <span data-ttu-id="305a5-554">[BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) nebo [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)</span><span class="sxs-lookup"><span data-stu-id="305a5-554">[BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) or [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)</span></span>

    <span data-ttu-id="305a5-555">Tyto metody Batch .NET, vyžadují ID existující fond a řetězec obsahující vzorec škálování k vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="305a5-555">These Batch .NET methods require the ID of an existing pool and a string containing the autoscale formula to evaluate.</span></span>

* [<span data-ttu-id="305a5-556">Vyhodnocení vzorec automatické škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-556">Evaluate an automatic scaling formula</span></span>](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    <span data-ttu-id="305a5-557">V této žádosti o volání rozhraní REST API, zadejte ID fondu v identifikátoru URI a vzorec škálování v *autoScaleFormula* element textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="305a5-557">In this REST API request, specify the pool ID in the URI, and the autoscale formula in the *autoScaleFormula* element of the request body.</span></span> <span data-ttu-id="305a5-558">Odpověď operace obsahuje veškeré informace o chybě, která se může vztahovat na vzorec.</span><span class="sxs-lookup"><span data-stu-id="305a5-558">The response of the operation contains any error information that might be related to the formula.</span></span>

<span data-ttu-id="305a5-559">V tomto [Batch .NET] [ net_api] fragment kódu, vyhodnocení se vzorec škálování.</span><span class="sxs-lookup"><span data-stu-id="305a5-559">In this [Batch .NET][net_api] code snippet, we evaluate an autoscale formula.</span></span> <span data-ttu-id="305a5-560">Pokud fond nemá povoleno automatické škálování, jsme ji povolit nejdřív.</span><span class="sxs-lookup"><span data-stu-id="305a5-560">If the pool does not have autoscaling enabled, we enable it first.</span></span>

```csharp
// First obtain a reference to an existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this code does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

<span data-ttu-id="305a5-561">Úspěšné vyhodnocení vzorec tento fragment kódu ukazuje vytváří výsledky podobně jako:</span><span class="sxs-lookup"><span data-stu-id="305a5-561">Successful evaluation of the formula shown in this code snippet produces results similar to:</span></span>

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a><span data-ttu-id="305a5-562">Získat informace o spuštění automatického škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-562">Get information about autoscale runs</span></span>

<span data-ttu-id="305a5-563">Aby se zajistilo, že vzorec funguje podle očekávání, doporučujeme, abyste pravidelně Kontrola výsledků spustí automatické škálování, které Batch provádí fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-563">To ensure that your formula is performing as expected, we recommend that you periodically check the results of the autoscaling runs that Batch performs on your pool.</span></span> <span data-ttu-id="305a5-564">Ano, získat (nebo aktualizujte) odkaz do fondu a zkontrolujte vlastnosti jeho poslední škálování spustit.</span><span class="sxs-lookup"><span data-stu-id="305a5-564">To do so, get (or refresh) a reference to the pool, and examine the properties of its last autoscale run.</span></span>

<span data-ttu-id="305a5-565">V Batch .NET [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) vlastnost má několik vlastností, které obsahují informace o nejnovější automatické škálování spustit provádět ve fondu:</span><span class="sxs-lookup"><span data-stu-id="305a5-565">In Batch .NET, the [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) property has several properties that provide information about the latest automatic scaling run performed on the pool:</span></span>

* [<span data-ttu-id="305a5-566">AutoScaleRun.Timestamp</span><span class="sxs-lookup"><span data-stu-id="305a5-566">AutoScaleRun.Timestamp</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [<span data-ttu-id="305a5-567">AutoScaleRun.Results</span><span class="sxs-lookup"><span data-stu-id="305a5-567">AutoScaleRun.Results</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [<span data-ttu-id="305a5-568">AutoScaleRun.Error</span><span class="sxs-lookup"><span data-stu-id="305a5-568">AutoScaleRun.Error</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

<span data-ttu-id="305a5-569">V rozhraní API REST [získat informace o fondu](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) požadavek vrací informace o fondu, který obsahuje nejnovější automatické škálování spouštění informace [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="305a5-569">In the REST API, the [Get information about a pool](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) request returns information about the pool, which includes the latest automatic scaling run information in the [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) property.</span></span>

<span data-ttu-id="305a5-570">Následující fragment kódu jazyka C# pomocí knihovny Batch .NET tisknout informace o poslední automatické škálování, spusťte na fond _myPool_:</span><span class="sxs-lookup"><span data-stu-id="305a5-570">The following C# code snippet uses the Batch .NET library to print information about the last autoscaling run on pool _myPool_:</span></span>

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

<span data-ttu-id="305a5-571">Ukázkový výstup předchozí fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="305a5-571">Sample output of the preceding snippet:</span></span>

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a><span data-ttu-id="305a5-572">Příklad vzorce automatického škálování</span><span class="sxs-lookup"><span data-stu-id="305a5-572">Example autoscale formulas</span></span>
<span data-ttu-id="305a5-573">Podívejme se na několik vzorce, které jsou ukázány různé způsoby, jak nastavit šířku výpočetní prostředky ve fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-573">Let's look at a few formulas that show different ways to adjust the amount of compute resources in a pool.</span></span>

### <a name="example-1-time-based-adjustment"></a><span data-ttu-id="305a5-574">Příklad 1: Založené na čase úpravy</span><span class="sxs-lookup"><span data-stu-id="305a5-574">Example 1: Time-based adjustment</span></span>
<span data-ttu-id="305a5-575">Předpokládejme, že chcete upravovat velikost fondu na základě den v týdnu a čas.</span><span class="sxs-lookup"><span data-stu-id="305a5-575">Suppose you want to adjust the pool size based on the day of the week and time of day.</span></span> <span data-ttu-id="305a5-576">Tento příklad ukazuje, jak zvýšení nebo snížení počtu uzlů ve fondu odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="305a5-576">This example shows how to increase or decrease the number of nodes in the pool accordingly.</span></span>

<span data-ttu-id="305a5-577">Vzorec nejdřív získá aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="305a5-577">The formula first obtains the current time.</span></span> <span data-ttu-id="305a5-578">Pokud je jeden den v týdnu (1-5) a v rámci pracovní dobu (8: 00 do 18: 00), velikost cílového fondu nastavena na 20 uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-578">If it's a weekday (1-5) and within working hours (8 AM to 6 PM), the target pool size is set to 20 nodes.</span></span> <span data-ttu-id="305a5-579">Jinak je nastavena na 10 uzly.</span><span class="sxs-lookup"><span data-stu-id="305a5-579">Otherwise, it's set to 10 nodes.</span></span>

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a><span data-ttu-id="305a5-580">Příklad 2: Úprava založený na úlohách</span><span class="sxs-lookup"><span data-stu-id="305a5-580">Example 2: Task-based adjustment</span></span>
<span data-ttu-id="305a5-581">V tomto příkladu se upraví velikost fondu na základě počtu úloh ve frontě.</span><span class="sxs-lookup"><span data-stu-id="305a5-581">In this example, the pool size is adjusted based on the number of tasks in the queue.</span></span> <span data-ttu-id="305a5-582">Komentáře a zalomení řádků jsou přípustné v řetězcích vzorce.</span><span class="sxs-lookup"><span data-stu-id="305a5-582">Both comments and line breaks are acceptable in formula strings.</span></span>

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a><span data-ttu-id="305a5-583">Příklad 3: Monitorování účtů pro paralelní úlohy</span><span class="sxs-lookup"><span data-stu-id="305a5-583">Example 3: Accounting for parallel tasks</span></span>
<span data-ttu-id="305a5-584">Tento příklad upraví velikost fondu na základě počtu úloh.</span><span class="sxs-lookup"><span data-stu-id="305a5-584">This example adjusts the pool size based on the number of tasks.</span></span> <span data-ttu-id="305a5-585">Tento vzorec také bere v úvahu [MaxTasksPerComputeNode] [ net_maxtasks] hodnotu, která je nastavená pro fond.</span><span class="sxs-lookup"><span data-stu-id="305a5-585">This formula also takes into account the [MaxTasksPerComputeNode][net_maxtasks] value that has been set for the pool.</span></span> <span data-ttu-id="305a5-586">Tento přístup je užitečné v situacích, kde [paralelní provádění úkolů](batch-parallel-node-tasks.md) bylo povoleno na váš fond.</span><span class="sxs-lookup"><span data-stu-id="305a5-586">This approach is useful in situations where [parallel task execution](batch-parallel-node-tasks.md) has been enabled on your pool.</span></span>

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a><span data-ttu-id="305a5-587">Příklad 4: Nastavení velikosti počáteční fondu</span><span class="sxs-lookup"><span data-stu-id="305a5-587">Example 4: Setting an initial pool size</span></span>
<span data-ttu-id="305a5-588">Tento příklad ukazuje fragmentu kódu C# pomocí vzorce automatického škálování, která nastaví velikost fondu na zadaný počet uzlů na počáteční čas období.</span><span class="sxs-lookup"><span data-stu-id="305a5-588">This example shows a C# code snippet with an autoscale formula that sets the pool size to a specified number of nodes for an initial time period.</span></span> <span data-ttu-id="305a5-589">Pak se upraví velikost fondu na základě počtu spuštěná a aktivní úlohy po uplynutí počáteční časové období.</span><span class="sxs-lookup"><span data-stu-id="305a5-589">Then it adjusts the pool size based on the number of running and active tasks after the initial time period has elapsed.</span></span>

<span data-ttu-id="305a5-590">Vzorec v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="305a5-590">The formula in the following code snippet:</span></span>

* <span data-ttu-id="305a5-591">Nastaví velikost počáteční fondu čtyř uzlů.</span><span class="sxs-lookup"><span data-stu-id="305a5-591">Sets the initial pool size to four nodes.</span></span>
* <span data-ttu-id="305a5-592">V rámci prvních 10 minut životního cyklu fondu nepřizpůsobí velikost fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-592">Does not adjust the pool size within the first 10 minutes of the pool's lifecycle.</span></span>
* <span data-ttu-id="305a5-593">Po 10 minutách, získá maximální hodnotu počtu spuštěných a aktivní úlohy v rámci za posledních 60 minut.</span><span class="sxs-lookup"><span data-stu-id="305a5-593">After 10 minutes, obtains the max value of the number of running and active tasks within the past 60 minutes.</span></span>
  * <span data-ttu-id="305a5-594">Pokud jsou obě hodnoty 0 (což znamená, že žádné úlohy byly spuštěné nebo aktivní za posledních 60 minut), velikost fondu je nastavena na hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="305a5-594">If both values are 0 (indicating that no tasks were running or active in the last 60 minutes), the pool size is set to 0.</span></span>
  * <span data-ttu-id="305a5-595">Pokud je buď hodnota větší než nula, nebudou provedeny žádné změny.</span><span class="sxs-lookup"><span data-stu-id="305a5-595">If either value is greater than zero, no change is made.</span></span>

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a><span data-ttu-id="305a5-596">Další kroky</span><span class="sxs-lookup"><span data-stu-id="305a5-596">Next steps</span></span>
* <span data-ttu-id="305a5-597">[Maximalizovat využití prostředků Azure Batch výpočetní uzel souběžných úloh](batch-parallel-node-tasks.md) obsahuje podrobnosti o tom, jak můžete spustit více úkolů současně na výpočetních uzlech ve fondu.</span><span class="sxs-lookup"><span data-stu-id="305a5-597">[Maximize Azure Batch compute resource usage with concurrent node tasks](batch-parallel-node-tasks.md) contains details about how you can execute multiple tasks simultaneously on the compute nodes in your pool.</span></span> <span data-ttu-id="305a5-598">Kromě automatického škálování tato funkce může pomoci snížit dobu trvání úlohy pro některé úlohy, ušetřit peníze.</span><span class="sxs-lookup"><span data-stu-id="305a5-598">In addition to autoscaling, this feature may help to lower job duration for some workloads, saving you money.</span></span>
* <span data-ttu-id="305a5-599">Pro jiné podpůrná efektivitu zajistěte, aby aplikace Batch dotazuje služba Batch optimální způsobem.</span><span class="sxs-lookup"><span data-stu-id="305a5-599">For another efficiency booster, ensure that your Batch application queries the Batch service in the most optimal way.</span></span> <span data-ttu-id="305a5-600">V tématu [efektivní dotazování na službu Azure Batch](batch-efficient-list-queries.md) se dozvíte, jak omezit množství dat, která protne sítě při dotazu na stav potenciálně tisíce výpočetních uzlů nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="305a5-600">See [Query the Azure Batch service efficiently](batch-efficient-list-queries.md) to learn how to limit the amount of data that crosses the wire when you query the status of potentially thousands of compute nodes or tasks.</span></span>

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
