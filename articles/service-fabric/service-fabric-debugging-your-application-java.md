---
title: "Ladění aplikace Azure Service Fabric v prostředí Eclipse | Microsoft Docs"
description: "Vylepšit spolehlivost a výkon vašich služeb vývoji a ladění je v prostředí Eclipse v místní vývojový cluster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: f3bcee3794de35005bd387ecfae7e6707f3cb5ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a><span data-ttu-id="12882-103">Ladění aplikace Java Service Fabric pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="12882-103">Debug your Java Service Fabric application using Eclipse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="12882-104">Visual Studio nebo CSharp</span><span class="sxs-lookup"><span data-stu-id="12882-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="12882-105">Eclipse nebo Java</span><span class="sxs-lookup"><span data-stu-id="12882-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
> 

1. <span data-ttu-id="12882-106">Spuštění místního vývojového clusteru podle kroků v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="12882-106">Start a local development cluster by following the steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).</span></span>

2. <span data-ttu-id="12882-107">Aktualizujte entryPoint.sh služby, kterou chcete ladit, tak, aby spouštěl procesu java s parametry vzdáleného ladění.</span><span class="sxs-lookup"><span data-stu-id="12882-107">Update entryPoint.sh of the service you wish to debug, so that it starts the java process with remote debug parameters.</span></span> <span data-ttu-id="12882-108">Tento soubor se nachází v následujícím umístění: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span><span class="sxs-lookup"><span data-stu-id="12882-108">This file can be found at the following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span></span> <span data-ttu-id="12882-109">Port 8001 nastaven pro ladění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="12882-109">Port 8001 is set for debugging in this example.</span></span>

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. <span data-ttu-id="12882-110">Nastavení počtu instancí nebo počet replik pro službu, která je laděné 1 Aktualizujte Manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="12882-110">Update the Application Manifest by setting the instance count or the replica count for the service that is being debugged to 1.</span></span> <span data-ttu-id="12882-111">Toto nastavení se vyhnete konfliktům pro port, který se používá pro ladění.</span><span class="sxs-lookup"><span data-stu-id="12882-111">This setting avoids conflicts for the port that is used for debugging.</span></span> <span data-ttu-id="12882-112">Například pro bezstavové služby, nastavte ``InstanceCount="1"`` a stavové služby nastavit cíl a minimální sady replik velikosti 1 následujícím způsobem: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span><span class="sxs-lookup"><span data-stu-id="12882-112">For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set the target and min replica set sizes to 1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span></span>

4. <span data-ttu-id="12882-113">Nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="12882-113">Deploy the application.</span></span>

5. <span data-ttu-id="12882-114">V integrovaném vývojovém prostředí Eclipse, vyberte **spustit -> Konfigurace ladění -> vzdálené aplikace Java a zadejte vlastnosti připojení** a nastavte vlastnosti takto:</span><span class="sxs-lookup"><span data-stu-id="12882-114">In the Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set the properties as follows:</span></span>

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  <span data-ttu-id="12882-115">Nastavte zarážky v požadované body a ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="12882-115">Set breakpoints at desired points and debug the application.</span></span>

<span data-ttu-id="12882-116">Pokud aplikace selhává, můžete také povolit coredumps.</span><span class="sxs-lookup"><span data-stu-id="12882-116">If the application is crashing, you may also want to enable coredumps.</span></span> <span data-ttu-id="12882-117">Spuštění ``ulimit -c`` v prostředí a pokud vrátí hodnotu 0, pak coredumps nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="12882-117">Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled.</span></span> <span data-ttu-id="12882-118">Pokud chcete povolit neomezená coredumps, spusťte následující příkaz: ``ulimit -c unlimited``.</span><span class="sxs-lookup"><span data-stu-id="12882-118">To enable unlimited coredumps, execute the following command: ``ulimit -c unlimited``.</span></span> <span data-ttu-id="12882-119">Můžete si taky ověřit stav pomocí příkazu ``ulimit -a``.</span><span class="sxs-lookup"><span data-stu-id="12882-119">You can also verify the status using the command ``ulimit -a``.</span></span>  <span data-ttu-id="12882-120">Pokud jste chtěli aktualizovat generování cestu coredump, spouštění ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span><span class="sxs-lookup"><span data-stu-id="12882-120">If you wanted to update the coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span></span> 

### <a name="next-steps"></a><span data-ttu-id="12882-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12882-121">Next steps</span></span>

* <span data-ttu-id="12882-122">[Shromažďování protokolů pomocí Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span><span class="sxs-lookup"><span data-stu-id="12882-122">[Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span></span>
* <span data-ttu-id="12882-123">[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span><span class="sxs-lookup"><span data-stu-id="12882-123">[Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span></span>
