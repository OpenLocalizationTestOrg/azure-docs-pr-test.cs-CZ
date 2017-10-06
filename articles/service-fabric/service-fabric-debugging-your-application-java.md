---
title: "aaaDebug aplikace Azure Service Fabric v prostředí Eclipse | Microsoft Docs"
description: "Vylepšit hello spolehlivost a výkon vašich služeb vývoji a ladění je v prostředí Eclipse v místní vývojový cluster."
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
ms.openlocfilehash: ab86254a5c312db40fd631746c89aab0bbb9d1a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a><span data-ttu-id="c3ccc-103">Ladění aplikace Java Service Fabric pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="c3ccc-103">Debug your Java Service Fabric application using Eclipse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c3ccc-104">Visual Studio nebo CSharp</span><span class="sxs-lookup"><span data-stu-id="c3ccc-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="c3ccc-105">Eclipse nebo Java</span><span class="sxs-lookup"><span data-stu-id="c3ccc-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
> 

1. <span data-ttu-id="c3ccc-106">Spuštění místního vývojového clusteru podle následujících kroků hello v [nastavení vývojového prostředí Service Fabric](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-106">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).</span></span>

2. <span data-ttu-id="c3ccc-107">Aktualizovat entryPoint.sh služby hello chcete toodebug, tak, aby spouštěl procesu java hello s parametry vzdáleného ladění.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-107">Update entryPoint.sh of hello service you wish toodebug, so that it starts hello java process with remote debug parameters.</span></span> <span data-ttu-id="c3ccc-108">Tento soubor se nachází v hello následující umístění: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-108">This file can be found at hello following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span></span> <span data-ttu-id="c3ccc-109">Port 8001 nastaven pro ladění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-109">Port 8001 is set for debugging in this example.</span></span>

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. <span data-ttu-id="c3ccc-110">Aktualizovat hello Application Manifest nastavením počet instancí hello nebo hello počet replik pro hello službu, která je ladit too1.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-110">Update hello Application Manifest by setting hello instance count or hello replica count for hello service that is being debugged too1.</span></span> <span data-ttu-id="c3ccc-111">Toto nastavení se vyhnete konfliktům pro hello port, který se používá pro ladění.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-111">This setting avoids conflicts for hello port that is used for debugging.</span></span> <span data-ttu-id="c3ccc-112">Například pro bezstavové služby, nastavte ``InstanceCount="1"`` a pro stavové služby sady hello cíle a minimální sady replik velikosti too1 následujícím způsobem: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-112">For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set hello target and min replica set sizes too1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span></span>

4. <span data-ttu-id="c3ccc-113">Nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-113">Deploy hello application.</span></span>

5. <span data-ttu-id="c3ccc-114">V Eclipse IDE hello, vyberte **spustit -> Konfigurace ladění -> vzdálené aplikace Java a zadejte vlastnosti připojení** a nastavte vlastnosti hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c3ccc-114">In hello Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set hello properties as follows:</span></span>

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  <span data-ttu-id="c3ccc-115">Nastavte zarážky v požadované body a ladění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-115">Set breakpoints at desired points and debug hello application.</span></span>

<span data-ttu-id="c3ccc-116">Pokud aplikace hello selhává, můžete také tooenable coredumps.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-116">If hello application is crashing, you may also want tooenable coredumps.</span></span> <span data-ttu-id="c3ccc-117">Spuštění ``ulimit -c`` v prostředí a pokud vrátí hodnotu 0, pak coredumps nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-117">Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled.</span></span> <span data-ttu-id="c3ccc-118">tooenable neomezená coredumps provést hello následující příkaz: ``ulimit -c unlimited``.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-118">tooenable unlimited coredumps, execute hello following command: ``ulimit -c unlimited``.</span></span> <span data-ttu-id="c3ccc-119">Můžete si taky ověřit stav hello pomocí příkazu hello ``ulimit -a``.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-119">You can also verify hello status using hello command ``ulimit -a``.</span></span>  <span data-ttu-id="c3ccc-120">Pokud jste chtěli tooupdate hello coredump generování cestu, spouštění ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span><span class="sxs-lookup"><span data-stu-id="c3ccc-120">If you wanted tooupdate hello coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span></span> 

### <a name="next-steps"></a><span data-ttu-id="c3ccc-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c3ccc-121">Next steps</span></span>

* <span data-ttu-id="c3ccc-122">[Shromažďování protokolů pomocí Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-122">[Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span></span>
* <span data-ttu-id="c3ccc-123">[Monitorování a Diagnostika služby místně](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c3ccc-123">[Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span></span>
