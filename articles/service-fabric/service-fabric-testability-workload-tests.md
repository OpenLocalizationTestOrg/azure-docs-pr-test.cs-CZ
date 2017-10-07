---
title: "aaaSimulate chyb v Azure mikroslužeb | Microsoft Docs"
description: "Jak tooharden vašim službám proti selhání řádně a vynuceném."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: 
ms.assetid: 44af01f0-ed73-4c31-8ac0-d9d65b4ad2d6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: 05467e291dfc0f12a021955f8ea540881ec10746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="simulate-failures-during-service-workloads"></a><span data-ttu-id="d64ca-103">Simulace chyb během zatížení služeb</span><span class="sxs-lookup"><span data-stu-id="d64ca-103">Simulate failures during service workloads</span></span>
<span data-ttu-id="d64ca-104">Hello testovatelnosti scénáře v Azure Service Fabric umožňují vývojářům toonot obavy o práci s jednotlivých chyb.</span><span class="sxs-lookup"><span data-stu-id="d64ca-104">hello testability scenarios in Azure Service Fabric enable developers toonot worry about dealing with individual faults.</span></span> <span data-ttu-id="d64ca-105">Existují scénáře, ale kde explicitní prokládání zatížení klienta a selhání může být potřeba.</span><span class="sxs-lookup"><span data-stu-id="d64ca-105">There are scenarios, however, where an explicit interleaving of client workload and failures might be needed.</span></span> <span data-ttu-id="d64ca-106">Hello prokládání zatížení klienta a chyb zajistí, že služby hello ve skutečnosti provádí některé akce v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="d64ca-106">hello interleaving of client workload and faults ensures that hello service is actually performing some action when failure happens.</span></span> <span data-ttu-id="d64ca-107">Zadané hello úroveň ovládací prvek, který poskytuje možnosti testování, může jít v přesné body hello provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="d64ca-107">Given hello level of control that testability provides, these could be at precise points of hello workload execution.</span></span> <span data-ttu-id="d64ca-108">Tato indukční chyb v různých stavů v aplikaci hello můžete najít chyby a zlepšení kvality.</span><span class="sxs-lookup"><span data-stu-id="d64ca-108">This induction of faults at different states in hello application can find bugs and improve quality.</span></span>

## <a name="sample-custom-scenario"></a><span data-ttu-id="d64ca-109">Vlastní vzorový scénář</span><span class="sxs-lookup"><span data-stu-id="d64ca-109">Sample custom scenario</span></span>
<span data-ttu-id="d64ca-110">Tento test ukazuje scénář, že interleaves hello firmy zatížení s [selhání řádně a vynuceném](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions).</span><span class="sxs-lookup"><span data-stu-id="d64ca-110">This test shows a scenario that interleaves hello business workload with [graceful and ungraceful failures](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions).</span></span> <span data-ttu-id="d64ca-111">Hello chyb by měl být vyvolané uprostřed hello operací služby nebo výpočetní pro dosažení co nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="d64ca-111">hello faults should be induced in hello middle of service operations or compute for best results.</span></span>

<span data-ttu-id="d64ca-112">Příklad služby, který zveřejňuje čtyři úlohy projděme: A, B, C a D. každé odpovídá tooa sadu pracovních postupů a může být výpočetní, úložiště nebo jejich kombinace.</span><span class="sxs-lookup"><span data-stu-id="d64ca-112">Let's walk through an example of a service that exposes four workloads: A, B, C, and D. Each corresponds tooa set of workflows and could be compute, storage, or a mix.</span></span> <span data-ttu-id="d64ca-113">Pro hello zájmu jednoduchost jsme se abstraktní out hello úloh v našem příkladu.</span><span class="sxs-lookup"><span data-stu-id="d64ca-113">For hello sake of simplicity, we will abstract out hello workloads in our example.</span></span> <span data-ttu-id="d64ca-114">Hello různých chyb, provést v tomto příkladu jsou:</span><span class="sxs-lookup"><span data-stu-id="d64ca-114">hello different faults executed in this example are:</span></span>

* <span data-ttu-id="d64ca-115">RestartNode: Toosimulate vynuceném selhání počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="d64ca-115">RestartNode: Ungraceful fault toosimulate a machine restart.</span></span>
* <span data-ttu-id="d64ca-116">RestartDeployedCodePackage: Vynuceném selhání toosimulate služby hostitelský proces dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="d64ca-116">RestartDeployedCodePackage: Ungraceful fault toosimulate service host process crashes.</span></span>
* <span data-ttu-id="d64ca-117">RemoveReplica: Odebrání řádně selhání toosimulate repliky.</span><span class="sxs-lookup"><span data-stu-id="d64ca-117">RemoveReplica: Graceful fault toosimulate replica removal.</span></span>
* <span data-ttu-id="d64ca-118">Operace MovePrimary: Repliky toosimulate řádně selhání přesune spouštěná nástrojem pro vyrovnávání zatížení Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="d64ca-118">MovePrimary: Graceful fault toosimulate replica moves triggered by hello Service Fabric load balancer.</span></span>

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with hello actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time toowait for a service toostabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want tooexecute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start hello workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While hello task is running, induce faults into hello service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from hello given service tootest.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run hello selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate hello health and stability of hello service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for hello workload toofinish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that hello faults induced while your service workload is running will
        // fault hello primary service. Hence, you will need tooreconnect toocomplete or check
        // hello status of hello workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
