---
title: "Spuštění a zastavení uzlů clusteru k testování Azure mikroslužeb | Microsoft Docs"
description: "Další informace o použití pravděpodobnost vkládání k testování aplikace Service Fabric pomocí spuštění a zastavení uzly clusteru."
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
ms.openlocfilehash: 850fbc0c74811ec942292da64064dec867cd1b9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a><span data-ttu-id="575db-103">Nahrazení uzel spuštění a zastavení uzlu rozhraní API s rozhraním API pro přechod uzlu</span><span class="sxs-lookup"><span data-stu-id="575db-103">Replacing the Start Node and Stop node APIs with the Node Transition API</span></span>

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a><span data-ttu-id="575db-104">Co rozhraní API uzlu spuštění a zastavení uzlu udělat?</span><span class="sxs-lookup"><span data-stu-id="575db-104">What do the Stop Node and Start Node APIs do?</span></span>

<span data-ttu-id="575db-105">Rozhraní API zastavení uzlu (spravované: [StopNodeAsync()][stopnode], prostředí PowerShell: [Stop-ServiceFabricNode][stopnodeps]) zastaví uzlem Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="575db-105">The Stop Node API (managed: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stops a Service Fabric node.</span></span>  <span data-ttu-id="575db-106">Uzel Service Fabric je proces, není virtuálního počítače nebo počítače – virtuálního počítače nebo počítače bude stále spuštěna.</span><span class="sxs-lookup"><span data-stu-id="575db-106">A Service Fabric node is process, not a VM or machine – the VM or machine will still be running.</span></span>  <span data-ttu-id="575db-107">Pro zbývající části dokumentu "uzel" znamená Service Fabric uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-107">For the rest of the document "node" will mean Service Fabric node.</span></span>  <span data-ttu-id="575db-108">Zastavení uzlu vloží je do *zastavena* stavu, kde není členem clusteru a nemůže být hostitelem služby, proto simulaci *dolů* uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-108">Stopping a node puts it into a *stopped* state where it is not a member of the cluster and cannot host services, thus simulating a *down* node.</span></span>  <span data-ttu-id="575db-109">To je užitečné pro vložení chyb do systému pro testování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="575db-109">This is useful for injecting faults into the system to test your application.</span></span>  <span data-ttu-id="575db-110">Rozhraní API spuštění uzlu (spravované: [StartNodeAsync()][startnode], prostředí PowerShell: [Start-ServiceFabricNode][startnodeps]]) obrátí rozhraní API zastavení uzlu  které přepne uzly zpět k normálním stavu.</span><span class="sxs-lookup"><span data-stu-id="575db-110">The Start Node API (managed: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) reverses the Stop Node API,  which brings the node back to a normal state.</span></span>

## <a name="why-are-we-replacing-these"></a><span data-ttu-id="575db-111">Proč to jsme nahradit?</span><span class="sxs-lookup"><span data-stu-id="575db-111">Why are we replacing these?</span></span>

<span data-ttu-id="575db-112">Jak je popsáno výše, *zastavena* Service Fabric uzlu je uzel záměrně cílené pomocí rozhraní API zastavení uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-112">As described earlier, a *stopped* Service Fabric node is a node intentionally targeted using the Stop Node API.</span></span>  <span data-ttu-id="575db-113">A *dolů* uzel je uzel, který je mimo provoz z jiného důvodu (například virtuální počítač nebo počítač je vypnutý).</span><span class="sxs-lookup"><span data-stu-id="575db-113">A *down* node is a node that is down for any other reason (e.g. the VM or machine is off).</span></span>  <span data-ttu-id="575db-114">S rozhraním API zastavení uzlu systému nevystavuje informace k rozlišení mezi *zastavena* uzly a *dolů* uzlů.</span><span class="sxs-lookup"><span data-stu-id="575db-114">With the Stop Node API, the system does not expose information to differentiate between *stopped* nodes and *down* nodes.</span></span>

<span data-ttu-id="575db-115">Kromě toho nejsou jako popisný také možné, některé chyby vrácené systémem tato rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="575db-115">In addition, some errors returned by these APIs are not as descriptive as they could be.</span></span>  <span data-ttu-id="575db-116">Například na volání rozhraní API zastavení uzlu na již *zastavena* uzlu vrátí chybu *InvalidAddress*.</span><span class="sxs-lookup"><span data-stu-id="575db-116">For example, invoking the Stop Node API on an already *stopped* node will return the error *InvalidAddress*.</span></span>  <span data-ttu-id="575db-117">Toto prostředí lze zlepšit.</span><span class="sxs-lookup"><span data-stu-id="575db-117">This experience could be improved.</span></span>

<span data-ttu-id="575db-118">Doba trvání, který uzel je zastavena pro je také "nekonečné" dokud se vyvolá rozhraní API spuštění uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-118">Also, the duration a node is stopped for is “infinite” until the Start Node API is invoked.</span></span>  <span data-ttu-id="575db-119">Jsme zjistili to může způsobit problémy a mohou být náchylné k chybě.</span><span class="sxs-lookup"><span data-stu-id="575db-119">We’ve found this can cause problems and may be error-prone.</span></span>  <span data-ttu-id="575db-120">Například jste viděli problémy, kde uživatel volá rozhraní API uzlu zastavit v uzlu a poté zapomněli jste o něm.</span><span class="sxs-lookup"><span data-stu-id="575db-120">For example, we’ve seen problems where a user invoked the Stop Node API on a node and then forgot about it.</span></span>  <span data-ttu-id="575db-121">Později, bylo jasné, pokud byl uzel *dolů* nebo *zastavena*.</span><span class="sxs-lookup"><span data-stu-id="575db-121">Later, it was unclear if the node was *down* or *stopped*.</span></span>


## <a name="introducing-the-node-transition-apis"></a><span data-ttu-id="575db-122">Úvod do rozhraní API přechod uzlu</span><span class="sxs-lookup"><span data-stu-id="575db-122">Introducing the Node Transition APIs</span></span>

<span data-ttu-id="575db-123">Zaměřili jsme se tyto problémy výše v novou sadu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="575db-123">We’ve addressed these issues above in a new set of APIs.</span></span>  <span data-ttu-id="575db-124">Nové rozhraní API přechod uzlu (spravované: [StartNodeTransitionAsync()][snt]) může sloužit k přechod uzel do Service Fabric *zastavena* stavu, nebo přechod z *zastavena* stavu normální stav.</span><span class="sxs-lookup"><span data-stu-id="575db-124">The new Node Transition API (managed: [StartNodeTransitionAsync()][snt]) may be used to transition a Service Fabric node to a *stopped* state, or to transition it from a *stopped* state to a normal up state.</span></span>  <span data-ttu-id="575db-125">Upozorňujeme, že "Start" názvu rozhraní API neodkazuje na spuštění uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-125">Please note that the “Start” in the name of the API does not refer to starting a node.</span></span>  <span data-ttu-id="575db-126">Odkazuje na od asynchronní operace, která v systému, budou spuštěny při přechodu uzel buď *zastavena* nebo spuštění stavu.</span><span class="sxs-lookup"><span data-stu-id="575db-126">It refers to beginning an asynchronous operation that the system will execute to transition the node to either *stopped* or started state.</span></span>

<span data-ttu-id="575db-127">**Využití**</span><span class="sxs-lookup"><span data-stu-id="575db-127">**Usage**</span></span>

<span data-ttu-id="575db-128">Pokud rozhraní API přechod uzlu nevyvolá výjimku při vyvolání, pak systém přijal asynchronní operaci a spustí ho.</span><span class="sxs-lookup"><span data-stu-id="575db-128">If the Node Transition API does not throw an exception when invoked, then the system has accepted the asynchronous operation, and will execute it.</span></span>  <span data-ttu-id="575db-129">Úspěšné volání neznamená, že ještě dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="575db-129">A successful call does not imply the operation is finished yet.</span></span>  <span data-ttu-id="575db-130">Pokud chcete získat informace o stavu aktuální operace, volání rozhraní API uzlu přechod průběh (spravované: [GetNodeTransitionProgressAsync()][gntp]) s identifikátorem guid používá při volání rozhraní API pro přechod k uzlu pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="575db-130">To get information about the current state of the operation, call the Node Transition Progress API (managed: [GetNodeTransitionProgressAsync()][gntp]) with the guid used when invoking Node Transition API for this operation.</span></span>  <span data-ttu-id="575db-131">Rozhraní API průběh přechod uzlu vrátí objekt NodeTransitionProgress.</span><span class="sxs-lookup"><span data-stu-id="575db-131">The Node Transition Progress API returns an NodeTransitionProgress object.</span></span>  <span data-ttu-id="575db-132">Vlastnost stav tohoto objektu určuje aktuální stav operace.</span><span class="sxs-lookup"><span data-stu-id="575db-132">This object’s State property specifies the current state of the operation.</span></span>  <span data-ttu-id="575db-133">Pokud stav "spuštěný" operace provádí.</span><span class="sxs-lookup"><span data-stu-id="575db-133">If the state is “Running” then the operation is executing.</span></span>  <span data-ttu-id="575db-134">Pokud je dokončena, operace se dokončila bez chyby.</span><span class="sxs-lookup"><span data-stu-id="575db-134">If it is Completed, the operation finished without error.</span></span>  <span data-ttu-id="575db-135">Pokud je s chybou, došlo k potížím, provádění operace.</span><span class="sxs-lookup"><span data-stu-id="575db-135">If it is Faulted, there was a problem executing the operation.</span></span>  <span data-ttu-id="575db-136">Vlastnost výsledek výjimka vlastnost označí, co byl tento problém.</span><span class="sxs-lookup"><span data-stu-id="575db-136">The Result property’s Exception property will indicate what the issue was.</span></span>  <span data-ttu-id="575db-137">V tématu https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate Další informace o vlastnosti stavu a v části "Využití vzorků" následující příklady kódu.</span><span class="sxs-lookup"><span data-stu-id="575db-137">See https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate for more information about the State property, and the “Sample Usage” section below for code examples.</span></span>


<span data-ttu-id="575db-138">**Rozdíl mezi zastaven uzel a uzel dolů** Pokud uzel je *zastavena* pomocí uzlu přechod API, výstup uzel dotazu (spravované: [GetNodeListAsync()] [ nodequery], Prostředí PowerShell: [Get-ServiceFabricNode][nodequeryps]) se zobrazí, zda má tento uzel *IsStopped* vlastnost hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="575db-138">**Differentiating between a stopped node and a down node** If a node is *stopped* using the Node Transition API, the output of a node query (managed: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) will show that this node has an *IsStopped* property value of true.</span></span>  <span data-ttu-id="575db-139">Poznámka: Tento soubor se liší od hodnoty *NodeStatus* vlastnosti, která se dozvíte *dolů*.</span><span class="sxs-lookup"><span data-stu-id="575db-139">Note this is different from the value of the *NodeStatus* property, which will say *Down*.</span></span>  <span data-ttu-id="575db-140">Pokud *NodeStatus* vlastnost má hodnotu *dolů*, ale *IsStopped* je nastavena hodnota false, pak uzlu nebyla zastavena, pomocí rozhraní API pro přechod uzlu a *dolů*  kvůli z jiného důvodu.</span><span class="sxs-lookup"><span data-stu-id="575db-140">If the *NodeStatus* property has a value of *Down*, but *IsStopped* is false, then the node was not stopped using the Node Transition API, and is *Down* due some other reason.</span></span>  <span data-ttu-id="575db-141">Pokud *IsStopped* vlastnost je nastavena hodnota true a *NodeStatus* vlastnost je *dolů*, pak byla zastavena, pomocí rozhraní API pro přechod uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-141">If the *IsStopped* property is true, and the *NodeStatus* property is *Down*, then it was stopped using the Node Transition API.</span></span>

<span data-ttu-id="575db-142">Spuštění *zastavena* uzlu pomocí rozhraní API přechod uzlu fungování jako normální člena clusteru znovu vrátí.</span><span class="sxs-lookup"><span data-stu-id="575db-142">Starting a *stopped* node using the Node Transition API will return it to function as a normal member of the cluster again.</span></span>  <span data-ttu-id="575db-143">Výstup uzel dotazu API bude zobrazovat *IsStopped* jako hodnotu false, a *NodeStatus* jako něco, co není dolů (například nahoru).</span><span class="sxs-lookup"><span data-stu-id="575db-143">The output of the node query API will show *IsStopped* as false, and *NodeStatus* as something that is not Down (e.g. Up).</span></span>


<span data-ttu-id="575db-144">**Omezené trvání** při zastavení uzlu, jeden z požadovaných parametrů, pomocí rozhraní API přechod uzlu *stopNodeDurationInSeconds*, představuje dobu v sekundách zachovat uzlu *zastavena*.</span><span class="sxs-lookup"><span data-stu-id="575db-144">**Limited Duration** When using the Node Transition API to stop a node, one of the required parameters, *stopNodeDurationInSeconds*, represents the amount of time in seconds to keep the node *stopped*.</span></span>  <span data-ttu-id="575db-145">Tato hodnota musí být v povoleném rozsahu, který má minimálně 600 a maximálně 14400.</span><span class="sxs-lookup"><span data-stu-id="575db-145">This value must be in the allowed range, which has a minimum of 600, and a maximum of 14400.</span></span>  <span data-ttu-id="575db-146">Po vypršení této doby uzlu se sám do stavem automaticky restartuje.</span><span class="sxs-lookup"><span data-stu-id="575db-146">After this time expires, the node will restart itself into Up state automatically.</span></span>  <span data-ttu-id="575db-147">Naleznete příklad 1 níže uvedený příklad použití.</span><span class="sxs-lookup"><span data-stu-id="575db-147">Refer to Sample 1 below for an example of usage.</span></span>

> [!WARNING]
> <span data-ttu-id="575db-148">Vyhněte se kombinování uzlu přechod rozhraní API a rozhraní API uzlu spuštění a zastavení uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-148">Avoid mixing Node Transition APIs and the Stop Node and Start Node APIs.</span></span>  <span data-ttu-id="575db-149">Doporučuje se používat pouze API přechod uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-149">The recommendation is to  use the Node Transition API only.</span></span>  <span data-ttu-id="575db-150">> Pokud uzel byl již byla zastavena, pomocí rozhraní API uzlu zastavit, ho by měl být spuštěn pomocí uzlu spustit rozhraní API před použitím > rozhraní API pro přechod k uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-150">> If a node has been already been stopped using the Stop Node API, it should be started using the Start Node API first before using the > Node Transition APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="575db-151">Více volání rozhraní API pro přechod k uzlu není možné provést na stejném uzlu paralelně.</span><span class="sxs-lookup"><span data-stu-id="575db-151">Multiple Node Transition APIs calls cannot be made on the same node in parallel.</span></span>  <span data-ttu-id="575db-152">V takovém případě bude rozhraní API přechod uzlu > throw FabricException má hodnotu vlastnosti ErrorCode NodeTransitionInProgress.</span><span class="sxs-lookup"><span data-stu-id="575db-152">In such a situation, the Node Transition API will    > throw a FabricException with an ErrorCode property value of NodeTransitionInProgress.</span></span>  <span data-ttu-id="575db-153">Po přechodu uzlu v konkrétním uzlu má > byla spuštěna, byste měli počkat, dokud operaci dosáhne stavu terminálu (dokončeno, Faulted nebo ForceCancelled) před spuštěním > nový přechod na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-153">Once a node transition on a specific node has  > been started, you should wait until the operation reaches a terminal state (Completed, Faulted, or ForceCancelled) before starting a  > new transition on the same node.</span></span>  <span data-ttu-id="575db-154">Paralelní uzlu volání přechod na jiné uzly jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="575db-154">Parallel node transition calls on different nodes are allowed.</span></span>


#### <a name="sample-usage"></a><span data-ttu-id="575db-155">Využití vzorků</span><span class="sxs-lookup"><span data-stu-id="575db-155">Sample Usage</span></span>


<span data-ttu-id="575db-156">**Příklad 1** -následující příklad používá rozhraní API uzlu přechod k zastavení uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-156">**Sample 1** - The following sample uses the Node Transition API to stop a node.</span></span>

```csharp
        // Helper function to get information about a node
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
                        // Inspect the progress object's Result.Exception.HResult to get the error code.
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
            // Uses the GetNodeListAsync() API to get information about the target node
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
                    // Invoke StartNodeTransitionAsync with the NodeStopDescription from above, which will stop the target node.  Retry transient errors.
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

<span data-ttu-id="575db-157">**Příklad 2** -následující ukázka spustí *zastavena* uzlu.</span><span class="sxs-lookup"><span data-stu-id="575db-157">**Sample 2** - The following sample starts a *stopped* node.</span></span>  <span data-ttu-id="575db-158">Používá některé metody helper od první vzorku.</span><span class="sxs-lookup"><span data-stu-id="575db-158">It uses some helper methods from the first sample.</span></span>

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
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
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
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

<span data-ttu-id="575db-159">**Ukázka 3** -následující příklad ukazuje nesprávné použití.</span><span class="sxs-lookup"><span data-stu-id="575db-159">**Sample 3** - The following sample shows incorrect usage.</span></span>  <span data-ttu-id="575db-160">Toto použití je nesprávný protože *stopDurationInSeconds* poskytuje je větší než povolený rozsah.</span><span class="sxs-lookup"><span data-stu-id="575db-160">This usage is incorrect because the *stopDurationInSeconds* it provides is greater than the allowed range.</span></span>  <span data-ttu-id="575db-161">Vzhledem k tomu, že StartNodeTransitionAsync() se nezdaří s závažné chybě, operaci nebyl přijat a rozhraní API průběh by neměl být volán.</span><span class="sxs-lookup"><span data-stu-id="575db-161">Since StartNodeTransitionAsync() will fail with a fatal error, the operation was not accepted, and the progress API should not be called.</span></span>  <span data-ttu-id="575db-162">Tato ukázka používá některé metody helper od první vzorku.</span><span class="sxs-lookup"><span data-stu-id="575db-162">This sample uses some helper methods from the first sample.</span></span>

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds to demonstrate error
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

<span data-ttu-id="575db-163">**Ukázka 4** -níže uvedená ukázka uvádí informace o chybě, který bude vrácen z rozhraní API průběh přechod uzlu při operaci iniciovaná rozhraní API přechod uzlu byla přijata, ale později selže při provádění.</span><span class="sxs-lookup"><span data-stu-id="575db-163">**Sample 4** - The following sample shows the error information that will be returned from the Node Transition Progress API when the operation initiated by the Node Transition API is accepted, but fails later while executing.</span></span>  <span data-ttu-id="575db-164">V případě selže, protože rozhraní API přechod uzel pokusí spustit uzel, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="575db-164">In the case, it fails because the Node Transition API attempts to start a node that does not exist.</span></span>  <span data-ttu-id="575db-165">Tato ukázka používá některé metody helper od první vzorku.</span><span class="sxs-lookup"><span data-stu-id="575db-165">This sample uses some helper methods from the first sample.</span></span>

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
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
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

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.  In this case, it will end up in the Faulted state since the node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect the progress object's Result.Exception.HResult to get the error code.
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
