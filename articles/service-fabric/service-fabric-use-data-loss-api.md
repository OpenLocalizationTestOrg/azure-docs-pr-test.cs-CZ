---
title: "Postup vyvolání ztrátě dat na služby prostředků infrastruktury služby | Microsoft Docs"
description: "Popisuje způsob použití ztrátou dat rozhraní api"
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
ms.date: 09/19/2016
ms.author: lemai
redirect_url: /azure/service-fabric/service-fabric-testability-overview
ms.openlocfilehash: 0c4791e56f84d0df38783a13c8d8c564fd25f55f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-invoke-data-loss-on-services"></a><span data-ttu-id="2ef33-103">Postup vyvolání ztrátě dat na služby</span><span class="sxs-lookup"><span data-stu-id="2ef33-103">How to Invoke Data Loss on Services</span></span>
> [!WARNING]
> <span data-ttu-id="2ef33-104">Tento dokument popisují postup ke ztrátě dat v službách a by měla být používána opatrně.</span><span class="sxs-lookup"><span data-stu-id="2ef33-104">This document describe how to cause data loss in your services, and should be used with care.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="2ef33-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="2ef33-105">Introduction</span></span>
<span data-ttu-id="2ef33-106">Můžete vyvolat ztrátě dat v oddílu služby Service Fabric pomocí volání StartPartitionDataLossAsync().</span><span class="sxs-lookup"><span data-stu-id="2ef33-106">You can invoke data loss on a partition of your Service Fabric Service by calling StartPartitionDataLossAsync().</span></span>  <span data-ttu-id="2ef33-107">Toto rozhraní api používá pro práci způsobí podmínky ztráty dat pravděpodobnost vkládání a služby Analysis Service.</span><span class="sxs-lookup"><span data-stu-id="2ef33-107">This api uses the Fault Injection and Analysis Service to perform the work to cause data loss conditions.</span></span>

## <a name="using-the-fault-injection-and-analysis-service"></a><span data-ttu-id="2ef33-108">Pomocí vkládání odolnost a službu Analysis Services</span><span class="sxs-lookup"><span data-stu-id="2ef33-108">Using the Fault Injection and Analysis Service</span></span>
<span data-ttu-id="2ef33-109">Pravděpodobnost vkládání a služby Analysis Service aktuálně podporuje následující rozhraní API v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="2ef33-109">The Fault Injection and Analysis Service currently supports the following APIs in the chart below.</span></span>  <span data-ttu-id="2ef33-110">Na pravé straně grafu zobrazí odpovídající rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2ef33-110">The right side of the chart shows the corresponding PowerShell cmdlet.</span></span>  <span data-ttu-id="2ef33-111">Naleznete na webu msdn dokumentaci na každé rozhraní API pro další informace o každé z nich.</span><span class="sxs-lookup"><span data-stu-id="2ef33-111">Please refer to the msdn documentation on each API for more information on each one.</span></span>

| <span data-ttu-id="2ef33-112">ROZHRANÍ API JAZYKA C#</span><span class="sxs-lookup"><span data-stu-id="2ef33-112">C# API</span></span> | <span data-ttu-id="2ef33-113">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ef33-113">PowerShell Cmdlet</span></span> |
| --- | ---:|
| <span data-ttu-id="2ef33-114">[StartPartitionDataLossAsync][dl]</span><span class="sxs-lookup"><span data-stu-id="2ef33-114">[StartPartitionDataLossAsync][dl]</span></span> |<span data-ttu-id="2ef33-115">[Počáteční ServiceFabricPartitionDataLoss][psdl]</span><span class="sxs-lookup"><span data-stu-id="2ef33-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span></span> |
| <span data-ttu-id="2ef33-116">[StartPartitionQuorumLossAsync][ql]</span><span class="sxs-lookup"><span data-stu-id="2ef33-116">[StartPartitionQuorumLossAsync][ql]</span></span> |<span data-ttu-id="2ef33-117">[Počáteční ServiceFabricPartitionQuorumLoss][psql]</span><span class="sxs-lookup"><span data-stu-id="2ef33-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span></span> |
| <span data-ttu-id="2ef33-118">[StartPartitionRestartAsync][rp]</span><span class="sxs-lookup"><span data-stu-id="2ef33-118">[StartPartitionRestartAsync][rp]</span></span> |<span data-ttu-id="2ef33-119">[Počáteční ServiceFabricPartitionRestart][psrp]</span><span class="sxs-lookup"><span data-stu-id="2ef33-119">[Start-ServiceFabricPartitionRestart][psrp]</span></span> |

## <a name="conceptual-overview-of-running-a-command"></a><span data-ttu-id="2ef33-120">Koncepční přehled spuštění příkazu</span><span class="sxs-lookup"><span data-stu-id="2ef33-120">Conceptual Overview of Running a Command</span></span>
<span data-ttu-id="2ef33-121">Pravděpodobnost vkládání a služby Analysis Service používá model asynchronní, kde spustit příkaz s jedno rozhraní API, označuje jako rozhraní API "Start" v tomto dokumentu pak zkontroluje průběh tento příkaz pomocí "GetProgress" rozhraní API, dokud je dosaženo stavu terminálu, nebo ji zrušte.</span><span class="sxs-lookup"><span data-stu-id="2ef33-121">The Fault Injection and Analysis Service uses an asynchronous model where you start the command with one API, referred to as the “Start” API in this document, then checks the progress of this command using a “GetProgress” API until it has reached a terminal state, or until you cancel it.</span></span>
<span data-ttu-id="2ef33-122">Spuštění příkazu, volání rozhraní API "Start" pro odpovídající rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2ef33-122">To start a command, call the “Start” API for the corresponding API.</span></span>  <span data-ttu-id="2ef33-123">Toto rozhraní API vrátí, když pravděpodobnost vkládání a služby Analysis Service přijal žádost.</span><span class="sxs-lookup"><span data-stu-id="2ef33-123">This API returns when the Fault Injection and Analysis Service has accepted the request.</span></span>  <span data-ttu-id="2ef33-124">Ale ho neindikuje, jak daleko je spustit příkaz, nebo i v případě, že má dosud spuštěna.</span><span class="sxs-lookup"><span data-stu-id="2ef33-124">However, it does not indicate how far a command has run, or even if it has started yet.</span></span>  <span data-ttu-id="2ef33-125">Chcete-li zkontrolovat průběh příkazu, volání rozhraní API, která odpovídá rozhraní API "Start" označovaly jako "GetProgress".</span><span class="sxs-lookup"><span data-stu-id="2ef33-125">In order to check progress of a command, call the “GetProgress” API that corresponds to the “Start” API previously called.</span></span>  <span data-ttu-id="2ef33-126">Rozhraní API "GetProgress" vrátí objekt označující aktuální stav příkazu uvnitř jeho vlastnost stavu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-126">The “GetProgress” API will return an object indicating the current status of the command inside its State property.</span></span>  <span data-ttu-id="2ef33-127">Příkaz spustí po neomezenou dobu až:</span><span class="sxs-lookup"><span data-stu-id="2ef33-127">A command runs indefinitely until:</span></span>

1. <span data-ttu-id="2ef33-128">Úspěšně dokončí.</span><span class="sxs-lookup"><span data-stu-id="2ef33-128">It completes successfully.</span></span>  <span data-ttu-id="2ef33-129">Pokud v tomto případě zavoláte "GetProgress" na něm, stav průběhu objektu se dokončí.</span><span class="sxs-lookup"><span data-stu-id="2ef33-129">If you call “GetProgress” on it in this case, the progress object’s State will be Completed.</span></span>
2. <span data-ttu-id="2ef33-130">Ho dojde k závažné chybě.</span><span class="sxs-lookup"><span data-stu-id="2ef33-130">It encounters a fatal error.</span></span>  <span data-ttu-id="2ef33-131">Pokud v tomto případě zavoláte "GetProgress" na něm, dojde k chybě stav průběhu objektu</span><span class="sxs-lookup"><span data-stu-id="2ef33-131">If you call “GetProgress” on it in this case, the progress object’s State will be Faulted</span></span>
3. <span data-ttu-id="2ef33-132">Přes zrušíte [CancelTestCommandAsync] [ cancel] rozhraní API, nebo [Stop-ServiceFabricTestCommand] [ cancelps] rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2ef33-132">You cancel it through the [CancelTestCommandAsync][cancel] API, or [Stop-ServiceFabricTestCommand][cancelps] PowerShell cmdlet.</span></span>  <span data-ttu-id="2ef33-133">Pokud v tomto případě zavoláte "GetProgress" na něm, bude stav objektu průběh zrušeno nebo ForceCancelled, v závislosti na argument pro toto rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2ef33-133">If you call “GetProgress” on it in this case, the progress object’s State will be either Cancelled or ForceCancelled, depending on an argument to that API.</span></span>  <span data-ttu-id="2ef33-134">Najdete v dokumentaci k [CancelTestCommandAsync] [ cancel] další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2ef33-134">See the documentation for [CancelTestCommandAsync][cancel] for more details.</span></span>

## <a name="details-of-running-a-command"></a><span data-ttu-id="2ef33-135">Podrobnosti o spuštění příkazu</span><span class="sxs-lookup"><span data-stu-id="2ef33-135">Details of Running a Command</span></span>
<span data-ttu-id="2ef33-136">Aby bylo možné spustit příkaz, volání rozhraní API spustit s očekávanou argumenty.</span><span class="sxs-lookup"><span data-stu-id="2ef33-136">In order to start a command, call the Start API with the expected arguments.</span></span>  <span data-ttu-id="2ef33-137">Všechna rozhraní API spustit mít argumentem Guid s názvem operationId.</span><span class="sxs-lookup"><span data-stu-id="2ef33-137">All Start APIs have a Guid argument named operationId.</span></span>  <span data-ttu-id="2ef33-138">Můžete měli sledovat určité operationId argument, protože se používá ke sledování průběhu tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-138">You should keep track of the operationId argument, since it is used to track progress of this command.</span></span>  <span data-ttu-id="2ef33-139">Toto musí být předán do rozhraní API "GetProgress" Chcete-li sledovat průběh příkazu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-139">This must be passed into the “GetProgress” API in order to track progress of the command.</span></span>  <span data-ttu-id="2ef33-140">OperationId musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2ef33-140">The operationId must be unique.</span></span>

<span data-ttu-id="2ef33-141">Po úspěšném volání rozhraní API spustit, by měla být volána rozhraní API GetProgress ve smyčce, dokud se nedokončí objekt vrácený průběh stavu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2ef33-141">After successfully calling the Start API, the GetProgress API should be called in a loop until the returned progress object’s State property is Completed.</span></span>  <span data-ttu-id="2ef33-142">Všechny [na FabricTransientException] [ fte] a OperationCanceledException je třeba opakovat.</span><span class="sxs-lookup"><span data-stu-id="2ef33-142">All [FabricTransientException’s][fte] and OperationCanceledException’s should be retried.</span></span>
<span data-ttu-id="2ef33-143">Příkaz se dosáhne stavu terminálu (dokončeno, Faulted nebo zrušeno), bude mít vlastnost výsledek vrácený průběh objektu Další informace.</span><span class="sxs-lookup"><span data-stu-id="2ef33-143">When the command has reached a terminal state (Completed, Faulted, or Cancelled), the returned progress object’s Result property will have additional information.</span></span>  <span data-ttu-id="2ef33-144">Pokud je stav dokončeno, bude obsahovat Result.SelectedPartition.PartitionId id oddílu, který byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="2ef33-144">If the state is Completed, Result.SelectedPartition.PartitionId will contain the partition id that was selected.</span></span>  <span data-ttu-id="2ef33-145">Result.Exception bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2ef33-145">Result.Exception will be null.</span></span>  <span data-ttu-id="2ef33-146">Pokud došlo k chybě v stav, Result.Exception bude mít z důvodu chyby vkládání a službu Analysis Services došlo k chybě příkazu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-146">If the state is Faulted, Result.Exception will have the reason the Fault Injection and Analysis Service faulted the command.</span></span>  <span data-ttu-id="2ef33-147">Result.SelectedPartition.PartitionId bude mít id oddílu, který byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="2ef33-147">Result.SelectedPartition.PartitionId will have the partition id that was selected.</span></span>  <span data-ttu-id="2ef33-148">V některých případech nemusí příkaz dostatečně pokračovat výběr oddílu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-148">In some situations, the command may not have proceeded far enough to choose a partition.</span></span>  <span data-ttu-id="2ef33-149">V takovém případě bude PartitionId 0.</span><span class="sxs-lookup"><span data-stu-id="2ef33-149">In that case, the PartitionId will be 0.</span></span>  <span data-ttu-id="2ef33-150">Pokud byla zrušena, stav, Result.Exception bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2ef33-150">If the state is Cancelled, Result.Exception will be null.</span></span>  <span data-ttu-id="2ef33-151">Podobně jako případě Faulted Result.SelectedPartition.PartitionId bude mít id oddílu, který jste vybrali, ale pokud příkaz nedosáhne dostatečně Uděláte to tak, bude 0.</span><span class="sxs-lookup"><span data-stu-id="2ef33-151">Like the Faulted case, Result.SelectedPartition.PartitionId will have the partition id that was chosen, but if the command has not proceeded far enough to do so, it will be 0.</span></span>  <span data-ttu-id="2ef33-152">Také naleznete následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="2ef33-152">Please also refer to the sample below.</span></span>

<span data-ttu-id="2ef33-153">Následující ukázkový kód ukazuje, jak začít pak kontrola průběhu příkaz, který má za následek ztrátu dat do určitého oddílu.</span><span class="sxs-lookup"><span data-stu-id="2ef33-153">The sample code below shows how to start then check progress on a command to cause data loss on a specific partition.</span></span>

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

<span data-ttu-id="2ef33-154">Následující ukázka ukazuje způsob použití partitionselector nejde zvolit náhodných oddílu určitou službu:</span><span class="sxs-lookup"><span data-stu-id="2ef33-154">The sample below shows how to use the PartitionSelector to choose a random partition of a specified service:</span></span>

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a><span data-ttu-id="2ef33-155">Historie a zkrácení</span><span class="sxs-lookup"><span data-stu-id="2ef33-155">History and Truncation</span></span>
<span data-ttu-id="2ef33-156">Poté, co příkaz dosáhne stavu terminálu, jeho metadata zůstane v pravděpodobnost vkládání a služby Analysis Service nějakou dobu, než bude odebrán ušetřit místo.</span><span class="sxs-lookup"><span data-stu-id="2ef33-156">After a command has reached a terminal state, its metadata will remain in the Fault Injection and Analysis Service for a certain time, before it will be removed to save space.</span></span>  <span data-ttu-id="2ef33-157">Pokud "GetProgress" je volána po odebrání pomocí operationId příkazu, vrátí FabricException s ErrorCode KeyNotFound.</span><span class="sxs-lookup"><span data-stu-id="2ef33-157">If “GetProgress” is called using the operationId of a command after it has been removed, it will return a FabricException with an ErrorCode of KeyNotFound.</span></span>

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
