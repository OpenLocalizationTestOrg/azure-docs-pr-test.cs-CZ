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
# <a name="how-to-invoke-data-loss-on-services"></a>Postup vyvolání ztrátě dat na služby
> [!WARNING]
> Tento dokument popisují postup ke ztrátě dat v službách a by měla být používána opatrně.
> 
> 

## <a name="introduction"></a>Úvod
Můžete vyvolat ztrátě dat v oddílu služby Service Fabric pomocí volání StartPartitionDataLossAsync().  Toto rozhraní api používá pro práci způsobí podmínky ztráty dat pravděpodobnost vkládání a služby Analysis Service.

## <a name="using-the-fault-injection-and-analysis-service"></a>Pomocí vkládání odolnost a službu Analysis Services
Pravděpodobnost vkládání a služby Analysis Service aktuálně podporuje následující rozhraní API v následující tabulce.  Na pravé straně grafu zobrazí odpovídající rutiny prostředí PowerShell.  Naleznete na webu msdn dokumentaci na každé rozhraní API pro další informace o každé z nich.

| ROZHRANÍ API JAZYKA C# | Rutiny prostředí PowerShell |
| --- | ---:|
| [StartPartitionDataLossAsync][dl] |[Počáteční ServiceFabricPartitionDataLoss][psdl] |
| [StartPartitionQuorumLossAsync][ql] |[Počáteční ServiceFabricPartitionQuorumLoss][psql] |
| [StartPartitionRestartAsync][rp] |[Počáteční ServiceFabricPartitionRestart][psrp] |

## <a name="conceptual-overview-of-running-a-command"></a>Koncepční přehled spuštění příkazu
Pravděpodobnost vkládání a služby Analysis Service používá model asynchronní, kde spustit příkaz s jedno rozhraní API, označuje jako rozhraní API "Start" v tomto dokumentu pak zkontroluje průběh tento příkaz pomocí "GetProgress" rozhraní API, dokud je dosaženo stavu terminálu, nebo ji zrušte.
Spuštění příkazu, volání rozhraní API "Start" pro odpovídající rozhraní API.  Toto rozhraní API vrátí, když pravděpodobnost vkládání a služby Analysis Service přijal žádost.  Ale ho neindikuje, jak daleko je spustit příkaz, nebo i v případě, že má dosud spuštěna.  Chcete-li zkontrolovat průběh příkazu, volání rozhraní API, která odpovídá rozhraní API "Start" označovaly jako "GetProgress".  Rozhraní API "GetProgress" vrátí objekt označující aktuální stav příkazu uvnitř jeho vlastnost stavu.  Příkaz spustí po neomezenou dobu až:

1. Úspěšně dokončí.  Pokud v tomto případě zavoláte "GetProgress" na něm, stav průběhu objektu se dokončí.
2. Ho dojde k závažné chybě.  Pokud v tomto případě zavoláte "GetProgress" na něm, dojde k chybě stav průběhu objektu
3. Přes zrušíte [CancelTestCommandAsync] [ cancel] rozhraní API, nebo [Stop-ServiceFabricTestCommand] [ cancelps] rutiny prostředí PowerShell.  Pokud v tomto případě zavoláte "GetProgress" na něm, bude stav objektu průběh zrušeno nebo ForceCancelled, v závislosti na argument pro toto rozhraní API.  Najdete v dokumentaci k [CancelTestCommandAsync] [ cancel] další podrobnosti.

## <a name="details-of-running-a-command"></a>Podrobnosti o spuštění příkazu
Aby bylo možné spustit příkaz, volání rozhraní API spustit s očekávanou argumenty.  Všechna rozhraní API spustit mít argumentem Guid s názvem operationId.  Můžete měli sledovat určité operationId argument, protože se používá ke sledování průběhu tohoto příkazu.  Toto musí být předán do rozhraní API "GetProgress" Chcete-li sledovat průběh příkazu.  OperationId musí být jedinečný.

Po úspěšném volání rozhraní API spustit, by měla být volána rozhraní API GetProgress ve smyčce, dokud se nedokončí objekt vrácený průběh stavu vlastnost.  Všechny [na FabricTransientException] [ fte] a OperationCanceledException je třeba opakovat.
Příkaz se dosáhne stavu terminálu (dokončeno, Faulted nebo zrušeno), bude mít vlastnost výsledek vrácený průběh objektu Další informace.  Pokud je stav dokončeno, bude obsahovat Result.SelectedPartition.PartitionId id oddílu, který byl vybrán.  Result.Exception bude mít hodnotu null.  Pokud došlo k chybě v stav, Result.Exception bude mít z důvodu chyby vkládání a službu Analysis Services došlo k chybě příkazu.  Result.SelectedPartition.PartitionId bude mít id oddílu, který byl vybrán.  V některých případech nemusí příkaz dostatečně pokračovat výběr oddílu.  V takovém případě bude PartitionId 0.  Pokud byla zrušena, stav, Result.Exception bude mít hodnotu null.  Podobně jako případě Faulted Result.SelectedPartition.PartitionId bude mít id oddílu, který jste vybrali, ale pokud příkaz nedosáhne dostatečně Uděláte to tak, bude 0.  Také naleznete následující ukázka.

Následující ukázkový kód ukazuje, jak začít pak kontrola průběhu příkaz, který má za následek ztrátu dat do určitého oddílu.

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

Následující ukázka ukazuje způsob použití partitionselector nejde zvolit náhodných oddílu určitou službu:

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

## <a name="history-and-truncation"></a>Historie a zkrácení
Poté, co příkaz dosáhne stavu terminálu, jeho metadata zůstane v pravděpodobnost vkládání a služby Analysis Service nějakou dobu, než bude odebrán ušetřit místo.  Pokud "GetProgress" je volána po odebrání pomocí operationId příkazu, vrátí FabricException s ErrorCode KeyNotFound.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
