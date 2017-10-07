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
# <a name="how-tooinvoke-data-loss-on-services"></a>Jak tooInvoke ztrátě dat na služby
> [!WARNING]
> Tento dokument popisuje jak toocause ztrátě dat v službách a mělo by se dát pozor.
> 
> 

## <a name="introduction"></a>Úvod
Můžete vyvolat ztrátě dat v oddílu služby Service Fabric pomocí volání StartPartitionDataLossAsync().  Toto rozhraní api používá hello pravděpodobnost vkládání a služby Analysis Service tooperform hello pracovní toocause data ztrátě podmínky.

## <a name="using-hello-fault-injection-and-analysis-service"></a>Pomocí hello pravděpodobnost vkládání a službu Analysis Services
Hello pravděpodobnost vkládání a služby Analysis Service aktuálně podporuje hello následující rozhraní API v grafu hello níže.  Hello pravé straně hello graf zobrazuje hello odpovídající rutiny prostředí PowerShell.  Další informace o každé z nich naleznete v dokumentaci msdn toohello na každé rozhraní API.

| ROZHRANÍ API JAZYKA C# | Rutiny prostředí PowerShell |
| --- | ---:|
| [StartPartitionDataLossAsync][dl] |[Počáteční ServiceFabricPartitionDataLoss][psdl] |
| [StartPartitionQuorumLossAsync][ql] |[Počáteční ServiceFabricPartitionQuorumLoss][psql] |
| [StartPartitionRestartAsync][rp] |[Počáteční ServiceFabricPartitionRestart][psrp] |

## <a name="conceptual-overview-of-running-a-command"></a>Koncepční přehled spuštění příkazu
Hello pravděpodobnost vkládání a služby Analysis Service používá model asynchronní, kde spustit hello příkaz s jedno rozhraní API, označuje tooas hello "Start" rozhraní API v tomto dokumentu, pak průběh hello kontroly tento příkaz pomocí rozhraní API "GetProgress", dokud nedosáhne terminál stavu, nebo dokud ho zrušit.
toostart příkazu volání rozhraní API "Start" hello pro hello odpovídající rozhraní API.  Toto rozhraní API vrátí, pokud hello pravděpodobnost vkládání a služby Analysis Service přijal žádost hello.  Ale ho neindikuje, jak daleko je spustit příkaz, nebo i v případě, že má dosud spuštěna.  V průběhu toocheck pořadí příkazu volejte hello "GetProgress" rozhraní API, která odpovídá dříve volat rozhraní API "Start" toohello.  Hello "GetProgress" rozhraní API vrátí objekt označující hello aktuální stav příkazu hello uvnitř jeho vlastnost stavu.  Příkaz spustí po neomezenou dobu až:

1. Úspěšně dokončí.  Pokud v tomto případě zavoláte "GetProgress" na něm, stav objektu průběh hello se dokončí.
2. Ho dojde k závažné chybě.  Pokud v tomto případě zavoláte "GetProgress" na něm, dojde k chybě stav objektu průběh hello
3. Zrušení prostřednictvím hello [CancelTestCommandAsync] [ cancel] rozhraní API, nebo [Stop-ServiceFabricTestCommand] [ cancelps] rutiny prostředí PowerShell.  Pokud v tomto případě zavoláte "GetProgress" na něm, hello stav průběhu objektu bude zrušeno nebo ForceCancelled, v závislosti na argumentu toothat rozhraní API.  Naleznete v dokumentaci k hello [CancelTestCommandAsync] [ cancel] další podrobnosti.

## <a name="details-of-running-a-command"></a>Podrobnosti o spuštění příkazu
V pořadí toostart příkaz volejte hello spustit rozhraní API s argumenty hello očekává.  Všechna rozhraní API spustit mít argumentem Guid s názvem operationId.  Jste měli udržování přehledu o hello operationId argument, protože se používá tootrack průběh tento příkaz.  Toto musí být předán do hello "GetProgress" rozhraní API v průběhu tootrack pořadí příkazu hello.  Hello operationId musí být jedinečný.

Po úspěšném volání metody hello spustit API hello GetProgress rozhraní API by měla být volána ve smyčce, dokud hello vrátil průběh dokončení vlastnost stavu objektu.  Všechny [na FabricTransientException] [ fte] a OperationCanceledException je třeba opakovat.
Příkaz hello se dosáhne stavu terminálu (dokončeno, Faulted nebo zrušeno), hello vrátit vlastnost výsledek průběh objektu bude mít další informace.  Pokud stav hello je dokončena, bude obsahovat Result.SelectedPartition.PartitionId hello id oddílu, který byl vybrán.  Result.Exception bude mít hodnotu null.  Pokud došlo k chybě v hello stavu, bude mít Result.Exception hello hello důvod selhání vkládání a příkaz hello službu Analysis Services došlo k chybě.  Result.SelectedPartition.PartitionId bude mít hello id oddílu, který byl vybrán.  V některých případech nemusí hello příkaz pokračovat daleko dostatek toochoose oddílu.  V takovém případě bude hello PartitionId 0.  Pokud stav hello byla zrušena, Result.Exception bude mít hodnotu null.  Podobně jako hello Faulted případ Result.SelectedPartition.PartitionId bude mít hello id oddílu, který jste vybrali, ale pokud příkaz hello nedosáhne daleko dostatek toodo tak, bude 0.  Následující ukázka toohello naleznete také.

Hello následující ukázkový kód ukazuje, jak toostart pak kontrola průběhu ztrátu dat toocause příkaz do určitého oddílu.

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

Následující ukázka Hello ukazuje, jak toouse hello partitionselector nejde toochoose náhodných oddílu určitou službu:

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

## <a name="history-and-truncation"></a>Historie a zkrácení
Po příkazu bylo dosaženo stavu terminálu, jeho metadata zůstane v hello pravděpodobnost vkládání a služby Analysis Service nějakou dobu, než bude odebrán toosave místa.  Pokud "GetProgress" je volána po odebrání pomocí hello operationId příkazu, vrátí FabricException s ErrorCode KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
