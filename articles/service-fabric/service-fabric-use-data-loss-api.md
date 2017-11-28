---
title: "aaaHow tooInvoke ztrátě dat na služby prostředků infrastruktury služby | Microsoft Docs"
description: "Popisuje, jak toouse hello ztráty dat rozhraní api"
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
ms.openlocfilehash: 014c7ebfd2c42d79a5fe1802ecc3fa0c1f26f9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinvoke-data-loss-on-services"></a><span data-ttu-id="8299a-103">Jak tooInvoke ztrátě dat na služby</span><span class="sxs-lookup"><span data-stu-id="8299a-103">How tooInvoke Data Loss on Services</span></span>
> [!WARNING]
> <span data-ttu-id="8299a-104">Tento dokument popisuje jak toocause ztrátě dat v službách a mělo by se dát pozor.</span><span class="sxs-lookup"><span data-stu-id="8299a-104">This document describe how toocause data loss in your services, and should be used with care.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="8299a-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="8299a-105">Introduction</span></span>
<span data-ttu-id="8299a-106">Můžete vyvolat ztrátě dat v oddílu služby Service Fabric pomocí volání StartPartitionDataLossAsync().</span><span class="sxs-lookup"><span data-stu-id="8299a-106">You can invoke data loss on a partition of your Service Fabric Service by calling StartPartitionDataLossAsync().</span></span>  <span data-ttu-id="8299a-107">Toto rozhraní api používá hello pravděpodobnost vkládání a služby Analysis Service tooperform hello pracovní toocause data ztrátě podmínky.</span><span class="sxs-lookup"><span data-stu-id="8299a-107">This api uses hello Fault Injection and Analysis Service tooperform hello work toocause data loss conditions.</span></span>

## <a name="using-hello-fault-injection-and-analysis-service"></a><span data-ttu-id="8299a-108">Pomocí hello pravděpodobnost vkládání a službu Analysis Services</span><span class="sxs-lookup"><span data-stu-id="8299a-108">Using hello Fault Injection and Analysis Service</span></span>
<span data-ttu-id="8299a-109">Hello pravděpodobnost vkládání a služby Analysis Service aktuálně podporuje hello následující rozhraní API v grafu hello níže.</span><span class="sxs-lookup"><span data-stu-id="8299a-109">hello Fault Injection and Analysis Service currently supports hello following APIs in hello chart below.</span></span>  <span data-ttu-id="8299a-110">Hello pravé straně hello graf zobrazuje hello odpovídající rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8299a-110">hello right side of hello chart shows hello corresponding PowerShell cmdlet.</span></span>  <span data-ttu-id="8299a-111">Další informace o každé z nich naleznete v dokumentaci msdn toohello na každé rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8299a-111">Please refer toohello msdn documentation on each API for more information on each one.</span></span>

| <span data-ttu-id="8299a-112">ROZHRANÍ API JAZYKA C#</span><span class="sxs-lookup"><span data-stu-id="8299a-112">C# API</span></span> | <span data-ttu-id="8299a-113">Rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8299a-113">PowerShell Cmdlet</span></span> |
| --- | ---:|
| <span data-ttu-id="8299a-114">[StartPartitionDataLossAsync][dl]</span><span class="sxs-lookup"><span data-stu-id="8299a-114">[StartPartitionDataLossAsync][dl]</span></span> |<span data-ttu-id="8299a-115">[Počáteční ServiceFabricPartitionDataLoss][psdl]</span><span class="sxs-lookup"><span data-stu-id="8299a-115">[Start-ServiceFabricPartitionDataLoss][psdl]</span></span> |
| <span data-ttu-id="8299a-116">[StartPartitionQuorumLossAsync][ql]</span><span class="sxs-lookup"><span data-stu-id="8299a-116">[StartPartitionQuorumLossAsync][ql]</span></span> |<span data-ttu-id="8299a-117">[Počáteční ServiceFabricPartitionQuorumLoss][psql]</span><span class="sxs-lookup"><span data-stu-id="8299a-117">[Start-ServiceFabricPartitionQuorumLoss][psql]</span></span> |
| <span data-ttu-id="8299a-118">[StartPartitionRestartAsync][rp]</span><span class="sxs-lookup"><span data-stu-id="8299a-118">[StartPartitionRestartAsync][rp]</span></span> |<span data-ttu-id="8299a-119">[Počáteční ServiceFabricPartitionRestart][psrp]</span><span class="sxs-lookup"><span data-stu-id="8299a-119">[Start-ServiceFabricPartitionRestart][psrp]</span></span> |

## <a name="conceptual-overview-of-running-a-command"></a><span data-ttu-id="8299a-120">Koncepční přehled spuštění příkazu</span><span class="sxs-lookup"><span data-stu-id="8299a-120">Conceptual Overview of Running a Command</span></span>
<span data-ttu-id="8299a-121">Hello pravděpodobnost vkládání a služby Analysis Service používá model asynchronní, kde spustit hello příkaz s jedno rozhraní API, označuje tooas hello "Start" rozhraní API v tomto dokumentu, pak průběh hello kontroly tento příkaz pomocí rozhraní API "GetProgress", dokud nedosáhne terminál stavu, nebo dokud ho zrušit.</span><span class="sxs-lookup"><span data-stu-id="8299a-121">hello Fault Injection and Analysis Service uses an asynchronous model where you start hello command with one API, referred tooas hello “Start” API in this document, then checks hello progress of this command using a “GetProgress” API until it has reached a terminal state, or until you cancel it.</span></span>
<span data-ttu-id="8299a-122">toostart příkazu volání rozhraní API "Start" hello pro hello odpovídající rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8299a-122">toostart a command, call hello “Start” API for hello corresponding API.</span></span>  <span data-ttu-id="8299a-123">Toto rozhraní API vrátí, pokud hello pravděpodobnost vkládání a služby Analysis Service přijal žádost hello.</span><span class="sxs-lookup"><span data-stu-id="8299a-123">This API returns when hello Fault Injection and Analysis Service has accepted hello request.</span></span>  <span data-ttu-id="8299a-124">Ale ho neindikuje, jak daleko je spustit příkaz, nebo i v případě, že má dosud spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8299a-124">However, it does not indicate how far a command has run, or even if it has started yet.</span></span>  <span data-ttu-id="8299a-125">V průběhu toocheck pořadí příkazu volejte hello "GetProgress" rozhraní API, která odpovídá dříve volat rozhraní API "Start" toohello.</span><span class="sxs-lookup"><span data-stu-id="8299a-125">In order toocheck progress of a command, call hello “GetProgress” API that corresponds toohello “Start” API previously called.</span></span>  <span data-ttu-id="8299a-126">Hello "GetProgress" rozhraní API vrátí objekt označující hello aktuální stav příkazu hello uvnitř jeho vlastnost stavu.</span><span class="sxs-lookup"><span data-stu-id="8299a-126">hello “GetProgress” API will return an object indicating hello current status of hello command inside its State property.</span></span>  <span data-ttu-id="8299a-127">Příkaz spustí po neomezenou dobu až:</span><span class="sxs-lookup"><span data-stu-id="8299a-127">A command runs indefinitely until:</span></span>

1. <span data-ttu-id="8299a-128">Úspěšně dokončí.</span><span class="sxs-lookup"><span data-stu-id="8299a-128">It completes successfully.</span></span>  <span data-ttu-id="8299a-129">Pokud v tomto případě zavoláte "GetProgress" na něm, stav objektu průběh hello se dokončí.</span><span class="sxs-lookup"><span data-stu-id="8299a-129">If you call “GetProgress” on it in this case, hello progress object’s State will be Completed.</span></span>
2. <span data-ttu-id="8299a-130">Ho dojde k závažné chybě.</span><span class="sxs-lookup"><span data-stu-id="8299a-130">It encounters a fatal error.</span></span>  <span data-ttu-id="8299a-131">Pokud v tomto případě zavoláte "GetProgress" na něm, dojde k chybě stav objektu průběh hello</span><span class="sxs-lookup"><span data-stu-id="8299a-131">If you call “GetProgress” on it in this case, hello progress object’s State will be Faulted</span></span>
3. <span data-ttu-id="8299a-132">Zrušení prostřednictvím hello [CancelTestCommandAsync] [ cancel] rozhraní API, nebo [Stop-ServiceFabricTestCommand] [ cancelps] rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8299a-132">You cancel it through hello [CancelTestCommandAsync][cancel] API, or [Stop-ServiceFabricTestCommand][cancelps] PowerShell cmdlet.</span></span>  <span data-ttu-id="8299a-133">Pokud v tomto případě zavoláte "GetProgress" na něm, hello stav průběhu objektu bude zrušeno nebo ForceCancelled, v závislosti na argumentu toothat rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8299a-133">If you call “GetProgress” on it in this case, hello progress object’s State will be either Cancelled or ForceCancelled, depending on an argument toothat API.</span></span>  <span data-ttu-id="8299a-134">Naleznete v dokumentaci k hello [CancelTestCommandAsync] [ cancel] další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="8299a-134">See hello documentation for [CancelTestCommandAsync][cancel] for more details.</span></span>

## <a name="details-of-running-a-command"></a><span data-ttu-id="8299a-135">Podrobnosti o spuštění příkazu</span><span class="sxs-lookup"><span data-stu-id="8299a-135">Details of Running a Command</span></span>
<span data-ttu-id="8299a-136">V pořadí toostart příkaz volejte hello spustit rozhraní API s argumenty hello očekává.</span><span class="sxs-lookup"><span data-stu-id="8299a-136">In order toostart a command, call hello Start API with hello expected arguments.</span></span>  <span data-ttu-id="8299a-137">Všechna rozhraní API spustit mít argumentem Guid s názvem operationId.</span><span class="sxs-lookup"><span data-stu-id="8299a-137">All Start APIs have a Guid argument named operationId.</span></span>  <span data-ttu-id="8299a-138">Jste měli udržování přehledu o hello operationId argument, protože se používá tootrack průběh tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="8299a-138">You should keep track of hello operationId argument, since it is used tootrack progress of this command.</span></span>  <span data-ttu-id="8299a-139">Toto musí být předán do hello "GetProgress" rozhraní API v průběhu tootrack pořadí příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="8299a-139">This must be passed into hello “GetProgress” API in order tootrack progress of hello command.</span></span>  <span data-ttu-id="8299a-140">Hello operationId musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8299a-140">hello operationId must be unique.</span></span>

<span data-ttu-id="8299a-141">Po úspěšném volání metody hello spustit API hello GetProgress rozhraní API by měla být volána ve smyčce, dokud hello vrátil průběh dokončení vlastnost stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="8299a-141">After successfully calling hello Start API, hello GetProgress API should be called in a loop until hello returned progress object’s State property is Completed.</span></span>  <span data-ttu-id="8299a-142">Všechny [na FabricTransientException] [ fte] a OperationCanceledException je třeba opakovat.</span><span class="sxs-lookup"><span data-stu-id="8299a-142">All [FabricTransientException’s][fte] and OperationCanceledException’s should be retried.</span></span>
<span data-ttu-id="8299a-143">Příkaz hello se dosáhne stavu terminálu (dokončeno, Faulted nebo zrušeno), hello vrátit vlastnost výsledek průběh objektu bude mít další informace.</span><span class="sxs-lookup"><span data-stu-id="8299a-143">When hello command has reached a terminal state (Completed, Faulted, or Cancelled), hello returned progress object’s Result property will have additional information.</span></span>  <span data-ttu-id="8299a-144">Pokud stav hello je dokončena, bude obsahovat Result.SelectedPartition.PartitionId hello id oddílu, který byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="8299a-144">If hello state is Completed, Result.SelectedPartition.PartitionId will contain hello partition id that was selected.</span></span>  <span data-ttu-id="8299a-145">Result.Exception bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8299a-145">Result.Exception will be null.</span></span>  <span data-ttu-id="8299a-146">Pokud došlo k chybě v hello stavu, bude mít Result.Exception hello hello důvod selhání vkládání a příkaz hello službu Analysis Services došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="8299a-146">If hello state is Faulted, Result.Exception will have hello reason hello Fault Injection and Analysis Service faulted hello command.</span></span>  <span data-ttu-id="8299a-147">Result.SelectedPartition.PartitionId bude mít hello id oddílu, který byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="8299a-147">Result.SelectedPartition.PartitionId will have hello partition id that was selected.</span></span>  <span data-ttu-id="8299a-148">V některých případech nemusí hello příkaz pokračovat daleko dostatek toochoose oddílu.</span><span class="sxs-lookup"><span data-stu-id="8299a-148">In some situations, hello command may not have proceeded far enough toochoose a partition.</span></span>  <span data-ttu-id="8299a-149">V takovém případě bude hello PartitionId 0.</span><span class="sxs-lookup"><span data-stu-id="8299a-149">In that case, hello PartitionId will be 0.</span></span>  <span data-ttu-id="8299a-150">Pokud stav hello byla zrušena, Result.Exception bude mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8299a-150">If hello state is Cancelled, Result.Exception will be null.</span></span>  <span data-ttu-id="8299a-151">Podobně jako hello Faulted případ Result.SelectedPartition.PartitionId bude mít hello id oddílu, který jste vybrali, ale pokud příkaz hello nedosáhne daleko dostatek toodo tak, bude 0.</span><span class="sxs-lookup"><span data-stu-id="8299a-151">Like hello Faulted case, Result.SelectedPartition.PartitionId will have hello partition id that was chosen, but if hello command has not proceeded far enough toodo so, it will be 0.</span></span>  <span data-ttu-id="8299a-152">Následující ukázka toohello naleznete také.</span><span class="sxs-lookup"><span data-stu-id="8299a-152">Please also refer toohello sample below.</span></span>

<span data-ttu-id="8299a-153">Hello následující ukázkový kód ukazuje, jak toostart pak kontrola průběhu ztrátu dat toocause příkaz do určitého oddílu.</span><span class="sxs-lookup"><span data-stu-id="8299a-153">hello sample code below shows how toostart then check progress on a command toocause data loss on a specific partition.</span></span>

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // hello id of hello target partition inside hello target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
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

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.        

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

                // In a terminal state .Result.SelectedPartition.PartitionId will have hello chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
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

<span data-ttu-id="8299a-154">Následující ukázka Hello ukazuje, jak toouse hello partitionselector nejde toochoose náhodných oddílu určitou službu:</span><span class="sxs-lookup"><span data-stu-id="8299a-154">hello sample below shows how toouse hello PartitionSelector toochoose a random partition of a specified service:</span></span>

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for hello command below
        Guid operationId = Guid.NewGuid();

        // Note: Use hello appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // hello name of hello target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have hello Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start hello command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means hello Fault Injection and Analysis Service has saved hello intent tooperform this work.  It does not say anything about hello progress
        // of hello command.
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

        // Poll hello progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // hello command won't be cancelled.

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
                // If State is Faulted, hello progress object's Result property's Exception property will have hello reason why.
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

## <a name="history-and-truncation"></a><span data-ttu-id="8299a-155">Historie a zkrácení</span><span class="sxs-lookup"><span data-stu-id="8299a-155">History and Truncation</span></span>
<span data-ttu-id="8299a-156">Po příkazu bylo dosaženo stavu terminálu, jeho metadata zůstane v hello pravděpodobnost vkládání a služby Analysis Service nějakou dobu, než bude odebrán toosave místa.</span><span class="sxs-lookup"><span data-stu-id="8299a-156">After a command has reached a terminal state, its metadata will remain in hello Fault Injection and Analysis Service for a certain time, before it will be removed toosave space.</span></span>  <span data-ttu-id="8299a-157">Pokud "GetProgress" je volána po odebrání pomocí hello operationId příkazu, vrátí FabricException s ErrorCode KeyNotFound.</span><span class="sxs-lookup"><span data-stu-id="8299a-157">If “GetProgress” is called using hello operationId of a command after it has been removed, it will return a FabricException with an ErrorCode of KeyNotFound.</span></span>

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
