---
title: "aaaStart a zastavení Clusterové uzly tootest Azure mikroslužeb | Microsoft Docs"
description: "Zjistěte, jak toouse poruch vkládání tootest aplikace Service Fabric pomocí spuštění a zastavení uzly clusteru."
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: 
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 7d3f5147328e6233a67533fbfb2a525aa5fc060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replacing-hello-start-node-and-stop-node-apis-with-hello-node-transition-api"></a><span data-ttu-id="9e327-103">Nahraďte hello uzlu spuštění a zastavení uzlu rozhraní API hello rozhraní API pro přechod k uzlu</span><span class="sxs-lookup"><span data-stu-id="9e327-103">Replacing hello Start Node and Stop node APIs with hello Node Transition API</span></span>

## <a name="what-do-hello-stop-node-and-start-node-apis-do"></a><span data-ttu-id="9e327-104">Co hello zastavení uzlu a použít rozhraní API pro spuštění uzlu?</span><span class="sxs-lookup"><span data-stu-id="9e327-104">What do hello Stop Node and Start Node APIs do?</span></span>

<span data-ttu-id="9e327-105">Hello zastavení uzlu rozhraní API (spravované: [StopNodeAsync()][stopnode], prostředí PowerShell: [Stop-ServiceFabricNode][stopnodeps]) zastaví uzlem Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9e327-105">hello Stop Node API (managed: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stops a Service Fabric node.</span></span>  <span data-ttu-id="9e327-106">Uzel Service Fabric je proces, není virtuálního počítače nebo počítače – hello virtuálního počítače nebo počítače bude stále spuštěna.</span><span class="sxs-lookup"><span data-stu-id="9e327-106">A Service Fabric node is process, not a VM or machine – hello VM or machine will still be running.</span></span>  <span data-ttu-id="9e327-107">Pro hello zbytek dokumentu hello "uzel" znamená Service Fabric uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-107">For hello rest of hello document "node" will mean Service Fabric node.</span></span>  <span data-ttu-id="9e327-108">Zastavení uzlu vloží je do *zastavena* stavu, kde není členem clusteru hello a nemůže být hostitelem služby, proto simulaci *dolů* uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-108">Stopping a node puts it into a *stopped* state where it is not a member of hello cluster and cannot host services, thus simulating a *down* node.</span></span>  <span data-ttu-id="9e327-109">To je užitečné pro vložení chyb do hello systému tootest vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e327-109">This is useful for injecting faults into hello system tootest your application.</span></span>  <span data-ttu-id="9e327-110">Hello spuštění uzlu rozhraní API (spravované: [StartNodeAsync()][startnode], prostředí PowerShell: [Start-ServiceFabricNode][startnodeps]]) zpětná hello zastavit rozhraní API uzlu  která přináší hello uzel back tooa normálním stavu.</span><span class="sxs-lookup"><span data-stu-id="9e327-110">hello Start Node API (managed: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) reverses hello Stop Node API,  which brings hello node back tooa normal state.</span></span>

## <a name="why-are-we-replacing-these"></a><span data-ttu-id="9e327-111">Proč to jsme nahradit?</span><span class="sxs-lookup"><span data-stu-id="9e327-111">Why are we replacing these?</span></span>

<span data-ttu-id="9e327-112">Jak je popsáno výše, *zastavena* Service Fabric uzlu je uzel záměrně cílené pomocí hello zastavení uzlu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9e327-112">As described earlier, a *stopped* Service Fabric node is a node intentionally targeted using hello Stop Node API.</span></span>  <span data-ttu-id="9e327-113">A *dolů* uzel je uzel, který je mimo provoz z jiného důvodu (například hello virtuálního počítače nebo počítač je vypnutý).</span><span class="sxs-lookup"><span data-stu-id="9e327-113">A *down* node is a node that is down for any other reason (e.g. hello VM or machine is off).</span></span>  <span data-ttu-id="9e327-114">S hello zastavení uzlu rozhraní API systému hello nevystavuje toodifferentiate informace mezi *zastavena* uzly a *dolů* uzlů.</span><span class="sxs-lookup"><span data-stu-id="9e327-114">With hello Stop Node API, hello system does not expose information toodifferentiate between *stopped* nodes and *down* nodes.</span></span>

<span data-ttu-id="9e327-115">Kromě toho nejsou jako popisný také možné, některé chyby vrácené systémem tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9e327-115">In addition, some errors returned by these APIs are not as descriptive as they could be.</span></span>  <span data-ttu-id="9e327-116">Například vyvolání hello zastavit API uzlu na element již *zastavena* uzlu vrátí chybu hello *InvalidAddress*.</span><span class="sxs-lookup"><span data-stu-id="9e327-116">For example, invoking hello Stop Node API on an already *stopped* node will return hello error *InvalidAddress*.</span></span>  <span data-ttu-id="9e327-117">Toto prostředí lze zlepšit.</span><span class="sxs-lookup"><span data-stu-id="9e327-117">This experience could be improved.</span></span>

<span data-ttu-id="9e327-118">Doba trvání hello uzlu je zastaveny pro je také "nekonečné" dokud hello při vyvolání spuštění uzlu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9e327-118">Also, hello duration a node is stopped for is “infinite” until hello Start Node API is invoked.</span></span>  <span data-ttu-id="9e327-119">Jsme zjistili to může způsobit problémy a mohou být náchylné k chybě.</span><span class="sxs-lookup"><span data-stu-id="9e327-119">We’ve found this can cause problems and may be error-prone.</span></span>  <span data-ttu-id="9e327-120">Například jste viděli problémy, kde může uživatel vyvolat hello zastavit API uzel v uzlu a pak zapomněli jste o něm.</span><span class="sxs-lookup"><span data-stu-id="9e327-120">For example, we’ve seen problems where a user invoked hello Stop Node API on a node and then forgot about it.</span></span>  <span data-ttu-id="9e327-121">Později, bylo jasné, pokud byl uzel hello *dolů* nebo *zastavena*.</span><span class="sxs-lookup"><span data-stu-id="9e327-121">Later, it was unclear if hello node was *down* or *stopped*.</span></span>


## <a name="introducing-hello-node-transition-apis"></a><span data-ttu-id="9e327-122">Představení hello rozhraní API pro přechod k uzlu</span><span class="sxs-lookup"><span data-stu-id="9e327-122">Introducing hello Node Transition APIs</span></span>

<span data-ttu-id="9e327-123">Zaměřili jsme se tyto problémy výše v novou sadu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9e327-123">We’ve addressed these issues above in a new set of APIs.</span></span>  <span data-ttu-id="9e327-124">Hello nové rozhraní API přechod uzlu (spravované: [StartNodeTransitionAsync()][snt]) může být použité tootransition tooa uzel Service Fabric *zastavena* stavu nebo tootransition ho z *zastavena* stavu tooa stavem Normální.</span><span class="sxs-lookup"><span data-stu-id="9e327-124">hello new Node Transition API (managed: [StartNodeTransitionAsync()][snt]) may be used tootransition a Service Fabric node tooa *stopped* state, or tootransition it from a *stopped* state tooa normal up state.</span></span>  <span data-ttu-id="9e327-125">Upozorňujeme, že tento hello "Start" hello název hello API neodkazuje toostarting uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-125">Please note that hello “Start” in hello name of hello API does not refer toostarting a node.</span></span>  <span data-ttu-id="9e327-126">Odkazuje toobeginning asynchronní operaci, bude systém hello spouštění tootransition hello uzlu tooeither *zastavena* nebo spuštění stavu.</span><span class="sxs-lookup"><span data-stu-id="9e327-126">It refers toobeginning an asynchronous operation that hello system will execute tootransition hello node tooeither *stopped* or started state.</span></span>

<span data-ttu-id="9e327-127">**Využití**</span><span class="sxs-lookup"><span data-stu-id="9e327-127">**Usage**</span></span>

<span data-ttu-id="9e327-128">Pokud hello uzlu přechod API nevyvolá výjimku při vyvolání, pak hello systému přijal hello asynchronní operaci a spustí ho.</span><span class="sxs-lookup"><span data-stu-id="9e327-128">If hello Node Transition API does not throw an exception when invoked, then hello system has accepted hello asynchronous operation, and will execute it.</span></span>  <span data-ttu-id="9e327-129">Úspěšné volání neznamená, že ještě dokončení operace hello.</span><span class="sxs-lookup"><span data-stu-id="9e327-129">A successful call does not imply hello operation is finished yet.</span></span>  <span data-ttu-id="9e327-130">tooget informace o aktuálním stavu hello hello operace, hello volání rozhraní API průběh přechod uzlu (spravované: [GetNodeTransitionProgressAsync()][gntp]) s identifikátorem guid hello používá při vyvolání uzlu Přechod rozhraní API pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="9e327-130">tooget information about hello current state of hello operation, call hello Node Transition Progress API (managed: [GetNodeTransitionProgressAsync()][gntp]) with hello guid used when invoking Node Transition API for this operation.</span></span>  <span data-ttu-id="9e327-131">Hello uzlu přechod průběh API vrátí objekt NodeTransitionProgress.</span><span class="sxs-lookup"><span data-stu-id="9e327-131">hello Node Transition Progress API returns an NodeTransitionProgress object.</span></span>  <span data-ttu-id="9e327-132">Vlastnost stav tohoto objektu určuje aktuální stav hello hello operace.</span><span class="sxs-lookup"><span data-stu-id="9e327-132">This object’s State property specifies hello current state of hello operation.</span></span>  <span data-ttu-id="9e327-133">Pokud stav hello je "systém" hello operace provádí.</span><span class="sxs-lookup"><span data-stu-id="9e327-133">If hello state is “Running” then hello operation is executing.</span></span>  <span data-ttu-id="9e327-134">Pokud je dokončena, hello operace dokončena bez chyby.</span><span class="sxs-lookup"><span data-stu-id="9e327-134">If it is Completed, hello operation finished without error.</span></span>  <span data-ttu-id="9e327-135">Pokud je s chybou, došlo k potížím, provádění operace hello.</span><span class="sxs-lookup"><span data-stu-id="9e327-135">If it is Faulted, there was a problem executing hello operation.</span></span>  <span data-ttu-id="9e327-136">byla výjimka vlastnost výsledek Hello vlastnost označí, jaké hello vydání.</span><span class="sxs-lookup"><span data-stu-id="9e327-136">hello Result property’s Exception property will indicate what hello issue was.</span></span>  <span data-ttu-id="9e327-137">V tématu https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate Další informace o stavu vlastnost hello a části "Využití vzorků" hello níže příklady kódu.</span><span class="sxs-lookup"><span data-stu-id="9e327-137">See https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate for more information about hello State property, and hello “Sample Usage” section below for code examples.</span></span>


<span data-ttu-id="9e327-138">**Rozdíl mezi zastaven uzel a uzel dolů** Pokud uzel je *zastavena* pomocí rozhraní API přechod uzlu, výstup hello uzel dotazu hello (spravované: [GetNodeListAsync()] [ nodequery], Prostředí PowerShell: [Get-ServiceFabricNode][nodequeryps]) se zobrazí, zda má tento uzel *IsStopped* vlastnost hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="9e327-138">**Differentiating between a stopped node and a down node** If a node is *stopped* using hello Node Transition API, hello output of a node query (managed: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) will show that this node has an *IsStopped* property value of true.</span></span>  <span data-ttu-id="9e327-139">Poznámka: Tento soubor se liší od hodnoty hello hello *NodeStatus* vlastnosti, která se dozvíte *dolů*.</span><span class="sxs-lookup"><span data-stu-id="9e327-139">Note this is different from hello value of hello *NodeStatus* property, which will say *Down*.</span></span>  <span data-ttu-id="9e327-140">Pokud hello *NodeStatus* vlastnost má hodnotu *dolů*, ale *IsStopped* je nastavena hodnota false, pak hello uzlu nebyla zastavena, pomocí rozhraní API pro přechod k uzlu hello a  *Dolů* kvůli z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="9e327-140">If hello *NodeStatus* property has a value of *Down*, but *IsStopped* is false, then hello node was not stopped using hello Node Transition API, and is *Down* due some other reason.</span></span>  <span data-ttu-id="9e327-141">Pokud hello *IsStopped* vlastnost má hodnotu true a hello *NodeStatus* vlastnost je *dolů*, pak byla zastavena, pomocí hello rozhraní API pro přechod k uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-141">If hello *IsStopped* property is true, and hello *NodeStatus* property is *Down*, then it was stopped using hello Node Transition API.</span></span>

<span data-ttu-id="9e327-142">Spuštění *zastavena* uzlu pomocí hello uzlu přechod API vrátí ho toofunction jako normální člena clusteru hello znovu.</span><span class="sxs-lookup"><span data-stu-id="9e327-142">Starting a *stopped* node using hello Node Transition API will return it toofunction as a normal member of hello cluster again.</span></span>  <span data-ttu-id="9e327-143">Hello výstup hello uzel dotazu API bude zobrazovat *IsStopped* jako hodnotu false, a *NodeStatus* jako něco, co není dolů (například nahoru).</span><span class="sxs-lookup"><span data-stu-id="9e327-143">hello output of hello node query API will show *IsStopped* as false, and *NodeStatus* as something that is not Down (e.g. Up).</span></span>


<span data-ttu-id="9e327-144">**Omezené trvání** při použití hello uzlu přechod API toostop uzlu, jeden z hello požadované parametry, *stopNodeDurationInSeconds*, představuje hello dobu v sekundách tookeep hello uzlu  *Zastavit*.</span><span class="sxs-lookup"><span data-stu-id="9e327-144">**Limited Duration** When using hello Node Transition API toostop a node, one of hello required parameters, *stopNodeDurationInSeconds*, represents hello amount of time in seconds tookeep hello node *stopped*.</span></span>  <span data-ttu-id="9e327-145">Tato hodnota musí být v hello povolený rozsah, který má minimálně 600 a maximálně 14400.</span><span class="sxs-lookup"><span data-stu-id="9e327-145">This value must be in hello allowed range, which has a minimum of 600, and a maximum of 14400.</span></span>  <span data-ttu-id="9e327-146">Po vypršení této doby hello uzlu se sám do stavem automaticky restartuje.</span><span class="sxs-lookup"><span data-stu-id="9e327-146">After this time expires, hello node will restart itself into Up state automatically.</span></span>  <span data-ttu-id="9e327-147">Příklad použití naleznete tooSample 1 níže.</span><span class="sxs-lookup"><span data-stu-id="9e327-147">Refer tooSample 1 below for an example of usage.</span></span>

> [!WARNING]
> <span data-ttu-id="9e327-148">Vyhněte se kombinování rozhraní API pro přechod k uzlu a hello API uzlu spuštění a zastavení uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-148">Avoid mixing Node Transition APIs and hello Stop Node and Start Node APIs.</span></span>  <span data-ttu-id="9e327-149">Hello doporučuje se příliš používat pouze hello rozhraní API pro přechod k uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-149">hello recommendation is too use hello Node Transition API only.</span></span>  <span data-ttu-id="9e327-150">> Pokud uzel byl již byla zastavena, pomocí hello zastavení uzlu rozhraní API, ho by měl být spuštěn pomocí hello spustit API uzlu nejprve před použitím hello > rozhraní API pro přechod k uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-150">> If a node has been already been stopped using hello Stop Node API, it should be started using hello Start Node API first before using hello > Node Transition APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="9e327-151">Hello několik rozhraní API uzlu přechodu, které volání nelze provádět na stejném uzlu paralelně.</span><span class="sxs-lookup"><span data-stu-id="9e327-151">Multiple Node Transition APIs calls cannot be made on hello same node in parallel.</span></span>  <span data-ttu-id="9e327-152">V takovém případě bude hello rozhraní API pro přechod k uzlu > throw FabricException má hodnotu vlastnosti ErrorCode NodeTransitionInProgress.</span><span class="sxs-lookup"><span data-stu-id="9e327-152">In such a situation, hello Node Transition API will    > throw a FabricException with an ErrorCode property value of NodeTransitionInProgress.</span></span>  <span data-ttu-id="9e327-153">Po přechodu uzlu v konkrétním uzlu má > byla spuštěna, měli počkat, až operace hello dosáhne stavu terminálu (dokončeno, Faulted nebo ForceCancelled) před spuštěním > nový přechod na hello stejný uzel.</span><span class="sxs-lookup"><span data-stu-id="9e327-153">Once a node transition on a specific node has  > been started, you should wait until hello operation reaches a terminal state (Completed, Faulted, or ForceCancelled) before starting a  > new transition on hello same node.</span></span>  <span data-ttu-id="9e327-154">Paralelní uzlu volání přechod na jiné uzly jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="9e327-154">Parallel node transition calls on different nodes are allowed.</span></span>


#### <a name="sample-usage"></a><span data-ttu-id="9e327-155">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="9e327-155">Sample Usage</span></span>


<span data-ttu-id="9e327-156">**Příklad 1** -hello hello následující ukázka používá rozhraní API pro přechod k uzlu toostop uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-156">**Sample 1** - hello following sample uses hello Node Transition API toostop a node.</span></span>

```csharp
        // Helper function tooget information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect hello progress object's Result.Exception.HResult tooget hello error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStopDescription from above, which will stop hello target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

<span data-ttu-id="9e327-157">**Příklad 2** – hello následující ukázka spustí *zastavena* uzlu.</span><span class="sxs-lookup"><span data-stu-id="9e327-157">**Sample 2** - hello following sample starts a *stopped* node.</span></span>  <span data-ttu-id="9e327-158">Některé metody helper od první vzorku hello používá.</span><span class="sxs-lookup"><span data-stu-id="9e327-158">It uses some helper methods from hello first sample.</span></span>

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses hello GetNodeListAsync() API tooget information about hello target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

<span data-ttu-id="9e327-159">**Ukázka 3** – hello následující příklad ukazuje nesprávné použití.</span><span class="sxs-lookup"><span data-stu-id="9e327-159">**Sample 3** - hello following sample shows incorrect usage.</span></span>  <span data-ttu-id="9e327-160">Toto použití je nesprávný protože hello *stopDurationInSeconds* poskytuje je větší než hello povolený rozsah.</span><span class="sxs-lookup"><span data-stu-id="9e327-160">This usage is incorrect because hello *stopDurationInSeconds* it provides is greater than hello allowed range.</span></span>  <span data-ttu-id="9e327-161">Vzhledem k tomu, že StartNodeTransitionAsync() se nezdaří s závažné chybě, hello operaci nebyl přijat a rozhraní API průběh hello by neměl být volán.</span><span class="sxs-lookup"><span data-stu-id="9e327-161">Since StartNodeTransitionAsync() will fail with a fatal error, hello operation was not accepted, and hello progress API should not be called.</span></span>  <span data-ttu-id="9e327-162">Tato ukázka používá některé metody helper od první vzorku hello.</span><span class="sxs-lookup"><span data-stu-id="9e327-162">This sample uses some helper methods from hello first sample.</span></span>

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds toodemonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

<span data-ttu-id="9e327-163">**Ukázka 4** – hello níže uvedená ukázka uvádí informace o chybě hello, který bude vrácen z hello uzlu přechod průběh rozhraní API při operaci hello iniciovaná hello rozhraní API pro přechod k uzlu byla přijata, ale později při provádění se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9e327-163">**Sample 4** - hello following sample shows hello error information that will be returned from hello Node Transition Progress API when hello operation initiated by hello Node Transition API is accepted, but fails later while executing.</span></span>  <span data-ttu-id="9e327-164">V případě hello se nezdaří, protože hello API přechod uzel pokusí toostart uzel, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="9e327-164">In hello case, it fails because hello Node Transition API attempts toostart a node that does not exist.</span></span>  <span data-ttu-id="9e327-165">Tato ukázka používá některé metody helper od první vzorku hello.</span><span class="sxs-lookup"><span data-stu-id="9e327-165">This sample uses some helper methods from hello first sample.</span></span>

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with hello NodeStartDescription from above, which will start hello target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hello desired state is reached.  In this case, it will end up in hello Faulted state since hello node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect hello progress object's Result.Exception.HResult tooget hello error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_
