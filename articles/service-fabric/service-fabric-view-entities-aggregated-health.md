---
title: aaaHow tooview Azure Service Fabric entit agregovat stavu | Microsoft Docs
description: "Popisuje, jak tooquery, zobrazit a vyhodnotit agregovaný stav entity Azure Service Fabric, prostřednictvím dotazů na stav a obecné dotazy."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: oanapl
ms.openlocfilehash: add810551cac26d2b4ff81b57d94ddd780c2cc2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-service-fabric-health-reports"></a><span data-ttu-id="ef423-103">Zobrazit sestavy stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ef423-103">View Service Fabric health reports</span></span>
<span data-ttu-id="ef423-104">Představuje Azure Service Fabric [stavu modelu](service-fabric-health-introduction.md) s entity stavu, ve které součásti systému a watchdogs můžou sestavy místní podmínky, které jsou monitorování.</span><span class="sxs-lookup"><span data-stu-id="ef423-104">Azure Service Fabric introduces a [health model](service-fabric-health-introduction.md) with health entities on which system components and watchdogs can report local conditions that they are monitoring.</span></span> <span data-ttu-id="ef423-105">Hello [úložiště stavu](service-fabric-health-introduction.md#health-store) slučuje všechny toodetermine data stavu zda entity jsou v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-105">hello [health store](service-fabric-health-introduction.md#health-store) aggregates all health data toodetermine whether entities are healthy.</span></span>

<span data-ttu-id="ef423-106">Hello clusteru se automaticky zadá sestavy o stavu odeslaných hello komponent systému.</span><span class="sxs-lookup"><span data-stu-id="ef423-106">hello cluster is automatically populated with health reports sent by hello system components.</span></span> <span data-ttu-id="ef423-107">Další informace v [stavu systému pomocí sestavy tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span><span class="sxs-lookup"><span data-stu-id="ef423-107">Read more at [Use system health reports tootroubleshoot](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).</span></span>

<span data-ttu-id="ef423-108">Service Fabric nabízí několik způsobů tooget hello agregovat stavu hello entit:</span><span class="sxs-lookup"><span data-stu-id="ef423-108">Service Fabric provides multiple ways tooget hello aggregated health of hello entities:</span></span>

* <span data-ttu-id="ef423-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) nebo jiné vizualizace nástroje</span><span class="sxs-lookup"><span data-stu-id="ef423-109">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) or other visualization tools</span></span>
* <span data-ttu-id="ef423-110">Dotazy na stav (pomocí prostředí PowerShell, rozhraní API nebo REST)</span><span class="sxs-lookup"><span data-stu-id="ef423-110">Health queries (through PowerShell, API, or REST)</span></span>
* <span data-ttu-id="ef423-111">Obecné dotazuje to návratový seznam entit, které mají stav jako jedna z vlastností hello (pomocí prostředí PowerShell, rozhraní API nebo REST)</span><span class="sxs-lookup"><span data-stu-id="ef423-111">General queries that return a list of entities that have health as one of hello properties (through PowerShell, API, or REST)</span></span>

<span data-ttu-id="ef423-112">toodemonstrate tyto možnosti, budeme používat místní cluster s pěti uzly a hello [fabric: / aplikace WordCount](http://aka.ms/servicefabric-wordcountapp).</span><span class="sxs-lookup"><span data-stu-id="ef423-112">toodemonstrate these options, let's use a local cluster with five nodes and hello [fabric:/WordCount application](http://aka.ms/servicefabric-wordcountapp).</span></span> <span data-ttu-id="ef423-113">Hello **fabric: / WordCount** aplikace obsahuje dvě výchozí služby, stavové služby typu `WordCountServiceType`a bezstavové služby typu `WordCountWebServiceType`.</span><span class="sxs-lookup"><span data-stu-id="ef423-113">hello **fabric:/WordCount** application contains two default services, a stateful service of type `WordCountServiceType`, and a stateless service of type `WordCountWebServiceType`.</span></span> <span data-ttu-id="ef423-114">Po změně hello `ApplicationManifest.xml` toorequire sedm cíl replik pro stavové služby hello a jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="ef423-114">I changed hello `ApplicationManifest.xml` toorequire seven target replicas for hello stateful service and one partition.</span></span> <span data-ttu-id="ef423-115">Protože jsou pouze pět uzlů v clusteru hello, součásti systému hello sestavy upozornění oddílu služby hello protože je pod počtu cílových hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-115">Because there are only five nodes in hello cluster, hello system components report a warning on hello service partition because it is below hello target count.</span></span>

```xml
<Service Name="WordCountService">
<<<<<<< HEAD
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="3">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
=======
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
>>>>>>> 5e84dbdd8e45a5d6b36f435a550b7433b873bf11
</Service>
```

## <a name="health-in-service-fabric-explorer"></a><span data-ttu-id="ef423-116">Stav v Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="ef423-116">Health in Service Fabric Explorer</span></span>
<span data-ttu-id="ef423-117">Service Fabric Explorer nabízí vizuální zobrazení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-117">Service Fabric Explorer provides a visual view of hello cluster.</span></span> <span data-ttu-id="ef423-118">V následující hello obrázek můžete uvidíte, že:</span><span class="sxs-lookup"><span data-stu-id="ef423-118">In hello image below, you can see that:</span></span>

* <span data-ttu-id="ef423-119">Hello aplikace **fabric: / WordCount** je červený (v chyba), protože obsahuje událost chyby hlášené **MyWatchdog** pro vlastnost hello **dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="ef423-119">hello application **fabric:/WordCount** is red (in error) because it has an error event reported by **MyWatchdog** for hello property **Availability**.</span></span>
* <span data-ttu-id="ef423-120">Jeden z jejích služeb **fabric: / WordCount/WordCountService** žlutý (v upozornění).</span><span class="sxs-lookup"><span data-stu-id="ef423-120">One of its services, **fabric:/WordCount/WordCountService** is yellow (in warning).</span></span> <span data-ttu-id="ef423-121">Služba Hello je nakonfigurována s sedm repliky a hello cluster obsahuje pět uzlů, takže dvě repicas nemůže být umístěn.</span><span class="sxs-lookup"><span data-stu-id="ef423-121">hello service is configured with seven replicas and hello cluster has five nodes, so two repicas can't be placed.</span></span> <span data-ttu-id="ef423-122">Je sice není vidět zde, oddílu služby hello žlutý kvůli sestavy systému z `System.FM` oznámením, že `Partition is below target replica or instance count`.</span><span class="sxs-lookup"><span data-stu-id="ef423-122">Although it's not shown here, hello service partition is yellow because of a system report from `System.FM` saying that `Partition is below target replica or instance count`.</span></span> <span data-ttu-id="ef423-123">aktivační události žlutý oddílu Hello hello žlutý služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-123">hello yellow partition triggers hello yellow service.</span></span>
* <span data-ttu-id="ef423-124">Hello clusteru je red kvůli hello red aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-124">hello cluster is red because of hello red application.</span></span>

<span data-ttu-id="ef423-125">vyhodnocení Hello používá výchozí zásady z manifestu clusteru hello a manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-125">hello evaluation uses default policies from hello cluster manifest and application manifest.</span></span> <span data-ttu-id="ef423-126">Jsou striktní zásady a není tolerovat žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="ef423-126">They are strict policies and do not tolerate any failure.</span></span>

<span data-ttu-id="ef423-127">Zobrazení hello clusteru Service Fabric Explorer:</span><span class="sxs-lookup"><span data-stu-id="ef423-127">View of hello cluster with Service Fabric Explorer:</span></span>

![Zobrazení hello clusteru Service Fabric Exploreru.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> <span data-ttu-id="ef423-129">Další informace o [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="ef423-129">Read more about [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
>
>

## <a name="health-queries"></a><span data-ttu-id="ef423-130">Dotazy na stav</span><span class="sxs-lookup"><span data-stu-id="ef423-130">Health queries</span></span>
<span data-ttu-id="ef423-131">Service Fabric zpřístupní dotazů na stav pro každý hello podporované [typy entit](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span><span class="sxs-lookup"><span data-stu-id="ef423-131">Service Fabric exposes health queries for each of hello supported [entity types](service-fabric-health-introduction.md#health-entities-and-hierarchy).</span></span> <span data-ttu-id="ef423-132">Je přístupný prostřednictvím rozhraní API, pomocí metody na hello [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), rutiny prostředí PowerShell a REST.</span><span class="sxs-lookup"><span data-stu-id="ef423-132">They can be accessed through hello API, using methods on [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="ef423-133">Tyto dotazy vrátí informace o hello entity stavu dokončení: hello Agregovat stav, události stavu entity, podřízené stavů (v případě potřeby), není v pořádku hodnocení (Pokud hello entity není v pořádku) a podřízené objekty stavu statistiky (při použít).</span><span class="sxs-lookup"><span data-stu-id="ef423-133">These queries return complete health information about hello entity: hello aggregated health state, entity health events, child health states (when applicable), unhealthy evaluations (when hello entity is not healthy), and children health statistics (when applicable).</span></span>

> [!NOTE]
> <span data-ttu-id="ef423-134">Stav entity je vrácena, pokud je plně naplněna v hello health store.</span><span class="sxs-lookup"><span data-stu-id="ef423-134">A health entity is returned when it is fully populated in hello health store.</span></span> <span data-ttu-id="ef423-135">Hello entity musí být aktivní (nebyl odstraněn) a mít sestavy systému.</span><span class="sxs-lookup"><span data-stu-id="ef423-135">hello entity must be active (not deleted) and have a system report.</span></span> <span data-ttu-id="ef423-136">Jeho nadřazené entity v řetězu hello hierarchie musí mít také sestav systému.</span><span class="sxs-lookup"><span data-stu-id="ef423-136">Its parent entities on hello hierarchy chain must also have system reports.</span></span> <span data-ttu-id="ef423-137">Pokud nejsou splněné některé z těchto podmínek, je vrácena hello stavu dotazy [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) s [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` který ukazuje, proč se nevrátí hello entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-137">If any of these conditions are not satisfied, hello health queries return a [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) with [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` that shows why hello entity is not returned.</span></span>
>
>

<span data-ttu-id="ef423-138">dotazy na stav Hello musí projít identifikátor entity hello, což závisí na typu entity hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-138">hello health queries must pass in hello entity identifier, which depends on hello entity type.</span></span> <span data-ttu-id="ef423-139">dotazy Hello přijmout parametry zásad volitelné stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-139">hello queries accept optional health policy parameters.</span></span> <span data-ttu-id="ef423-140">Pokud nejsou zadány žádné zásady stavu, hello [zásady stavu](service-fabric-health-introduction.md#health-policies) z manifestu clusteru nebo aplikace hello slouží k vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="ef423-140">If no health policies are specified, hello [health policies](service-fabric-health-introduction.md#health-policies) from hello cluster or application manifest are used for evaluation.</span></span> <span data-ttu-id="ef423-141">Pokud hello manifesty nesmí obsahovat definice pro zásady stavu, zásady stavu výchozí hello se používají pro vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="ef423-141">If hello manifests don't contain a definition for health policies, hello default health policies are used for evaluation.</span></span> <span data-ttu-id="ef423-142">zásady stavu výchozí Hello není tolerovat případných selhání.</span><span class="sxs-lookup"><span data-stu-id="ef423-142">hello default health policies do not tolerate any failures.</span></span> <span data-ttu-id="ef423-143">dotazy Hello také přijmout filtry pro vrácení pouze částečný podřízené objekty nebo události – hello ty, které jsou respektujících hello zadaných filtrů.</span><span class="sxs-lookup"><span data-stu-id="ef423-143">hello queries also accept filters for returning only partial children or events--hello ones that respect hello specified filters.</span></span> <span data-ttu-id="ef423-144">Jiný filtr povoluje, s výjimkou statistiky hello podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-144">Another filter allows excluding hello children statistics.</span></span>

> [!NOTE]
> <span data-ttu-id="ef423-145">Hello výstupní filtry se použijí na straně serveru hello, tak se snižuje velikost hello zprávy odpovědi.</span><span class="sxs-lookup"><span data-stu-id="ef423-145">hello output filters are applied on hello server side, so hello message reply size is reduced.</span></span> <span data-ttu-id="ef423-146">Doporučujeme použít filtry výstup hello toolimit hello data vrácená, ne však použít filtry na straně klienta hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-146">We recommended that you use hello output filters toolimit hello data returned, rather than apply filters on hello client side.</span></span>
>
>

<span data-ttu-id="ef423-147">Stav entity obsahuje:</span><span class="sxs-lookup"><span data-stu-id="ef423-147">An entity's health contains:</span></span>

* <span data-ttu-id="ef423-148">Hello agregován stav entity hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-148">hello aggregated health state of hello entity.</span></span> <span data-ttu-id="ef423-149">Vypočtená podle hello stavu úložiště na základě sestav stavu entity, podřízené stavů (v případě potřeby) a zásad stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-149">Computed by hello health store based on entity health reports, child health states (when applicable), and health policies.</span></span> <span data-ttu-id="ef423-150">Další informace o [vyhodnocení stavu entity](service-fabric-health-introduction.md#health-evaluation).</span><span class="sxs-lookup"><span data-stu-id="ef423-150">Read more about [entity health evaluation](service-fabric-health-introduction.md#health-evaluation).</span></span>  
* <span data-ttu-id="ef423-151">události stavu Hello u hello entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-151">hello health events on hello entity.</span></span>
* <span data-ttu-id="ef423-152">Hello kolekce stavů všechny podřízené objekty pro hello entity, které může mít podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-152">hello collection of health states of all children for hello entities that can have children.</span></span> <span data-ttu-id="ef423-153">Hello stavů obsahovat identifikátory entity a hello agregovaný stav.</span><span class="sxs-lookup"><span data-stu-id="ef423-153">hello health states contain entity identifiers and hello aggregated health state.</span></span> <span data-ttu-id="ef423-154">dokončení health tooget dítěte, volání stavu hello dotazu pro typ entity podřízené hello a předat hello podřízené identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ef423-154">tooget complete health for a child, call hello query health for hello child entity type and pass in hello child identifier.</span></span>
* <span data-ttu-id="ef423-155">Tento bod toohello sestavy, který není v pořádku hodnocení Hello spuštěna hello stavu entity hello, pokud hello entity není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-155">hello unhealthy evaluations that point toohello report that triggered hello state of hello entity, if hello entity is not healthy.</span></span> <span data-ttu-id="ef423-156">hodnocení Hello jsou rekurzivní, obsahující hello podřízené objekty stavu hodnocení, které spouštějí aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-156">hello evaluations are recursive, containing hello children health evaluations that triggered current health state.</span></span> <span data-ttu-id="ef423-157">Například sledovací zařízení oznámil chybu pro repliku.</span><span class="sxs-lookup"><span data-stu-id="ef423-157">For example, a watchdog reported an error against a replica.</span></span> <span data-ttu-id="ef423-158">Stav aplikace Hello ukazuje, není v pořádku zkušební verzi z důvodu tooan služby není v pořádku; Hello služby není v pořádku kvůli tooa oddílu v chybě; Hello oddílu není v pořádku kvůli tooa repliky v chybě; replika Hello je z důvodu sestava stavu chyb toohello sledovací zařízení není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-158">hello application health shows an unhealthy evaluation due tooan unhealthy service; hello service is unhealthy due tooa partition in error; hello partition is unhealthy due tooa replica in error; hello replica is unhealthy due toohello watchdog error health report.</span></span>
* <span data-ttu-id="ef423-159">Statistika Hello stavu pro všechny podřízené objekty typu hello entity, které mají podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-159">hello health statistics for all children types of hello entities that have children.</span></span> <span data-ttu-id="ef423-160">Například stav clusteru ukazuje hello celkový počet aplikací, služeb, oddíly, repliky a nasadit entity v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-160">For example, cluster health shows hello total number of applications, services, partitions, replicas, and deployed entities in hello cluster.</span></span> <span data-ttu-id="ef423-161">Stav služby zobrazí hello celkový počet oddílů a repliky v části hello zadaná služba.</span><span class="sxs-lookup"><span data-stu-id="ef423-161">Service health shows hello total number of partitions and replicas under hello specified service.</span></span>

## <a name="get-cluster-health"></a><span data-ttu-id="ef423-162">Získání stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="ef423-162">Get cluster health</span></span>
<span data-ttu-id="ef423-163">Vrátí hello stavu entity hello clusteru a obsahuje hello stavů aplikací a uzly (podřízené objekty daného clusteru hello).</span><span class="sxs-lookup"><span data-stu-id="ef423-163">Returns hello health of hello cluster entity and contains hello health states of applications and nodes (children of hello cluster).</span></span> <span data-ttu-id="ef423-164">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-164">Input:</span></span>

* <span data-ttu-id="ef423-165">[Nepovinné] hello zásady stavu clusteru používá tooevaluate hello uzly a události hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-165">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="ef423-166">[Nepovinné] hello mapy zásady stavu aplikace, pomocí zásad stavu hello používá manifestu zásady aplikací toooverride hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-166">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="ef423-167">[Nepovinné] Filtry pro události, uzly a aplikace, které určí položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-167">[Optional] Filters for events, nodes, and applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-168">Všechny události, uzly a aplikace jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-168">All events, nodes, and applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="ef423-169">[Nepovinné] Filtrujte tooexclude stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-169">[Optional] Filter tooexclude health statistics.</span></span>
* <span data-ttu-id="ef423-170">[Nepovinné] Filtrovat tooinclude fabric: / statistiky stavu systému v hello statistiky stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-170">[Optional] Filter tooinclude fabric:/System health statistics in hello health statistics.</span></span> <span data-ttu-id="ef423-171">Platí jenom při nejsou vyloučení hello stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-171">Only applicable when hello health statistics are not excluded.</span></span> <span data-ttu-id="ef423-172">Ve výchozím nastavení hello stavu statistiky obsahuje pouze statistiku pro uživatele aplikace a hello aplikaci systému.</span><span class="sxs-lookup"><span data-stu-id="ef423-172">By default, hello health statistics include only statistics for user applications and not hello System application.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-173">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-173">API</span></span>
<span data-ttu-id="ef423-174">tooget clusteru stavu, vytvořte `FabricClient` a volání hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) metoda na jeho **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="ef423-174">tooget cluster health, create a `FabricClient` and call hello [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) method on its **HealthManager**.</span></span>

<span data-ttu-id="ef423-175">Hello následující volání získá hello clusteru stavu:</span><span class="sxs-lookup"><span data-stu-id="ef423-175">hello following call gets hello cluster health:</span></span>

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

<span data-ttu-id="ef423-176">Hello následující kód získá stav clusteru hello pomocí zásad stavu vlastní clusteru a filtry pro uzly a aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-176">hello following code gets hello cluster health by using a custom cluster health policy and filters for nodes and applications.</span></span> <span data-ttu-id="ef423-177">Se určuje, že hello stavu statistiky obsahovat hello fabric: / System statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-177">It specifies that hello health statistics include hello fabric:/System statistics.</span></span> <span data-ttu-id="ef423-178">Vytvoří [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), který obsahuje vstupní informace hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-178">It creates [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), which contains hello input information.</span></span>

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="ef423-179">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-179">PowerShell</span></span>
<span data-ttu-id="ef423-180">Hello rutiny tooget hello clusteru stav je [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-180">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth).</span></span> <span data-ttu-id="ef423-181">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-181">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-182">Hello stav clusteru hello je pět uzlů, hello systémová aplikace fabric: / WordCount nakonfigurována, jak je popsáno.</span><span class="sxs-lookup"><span data-stu-id="ef423-182">hello state of hello cluster is five nodes, hello system application, and fabric:/WordCount configured as described.</span></span>

<span data-ttu-id="ef423-183">následující rutina Hello získá stav clusteru pomocí výchozích zásad stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-183">hello following cmdlet gets cluster health by using default health policies.</span></span> <span data-ttu-id="ef423-184">Hello agregovaný stav je upozornění, protože hello fabric: / WordCount aplikace je v upozornění.</span><span class="sxs-lookup"><span data-stu-id="ef423-184">hello aggregated health state is warning, because hello fabric:/WordCount application is in warning.</span></span> <span data-ttu-id="ef423-185">Všimněte si, jak hello není v pořádku hodnocení poskytují podrobnosti o hello podmínky, které aktivuje hello agregovat stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-185">Note how hello unhealthy evaluations provide details on hello conditions that triggered hello aggregated health.</span></span>

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                          
                          
NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _Node_0
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

<span data-ttu-id="ef423-186">Hello následující rutiny prostředí PowerShell získá hello stavu hello clusteru pomocí zásad vlastní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef423-186">hello following PowerShell cmdlet gets hello health of hello cluster by using a custom application policy.</span></span> <span data-ttu-id="ef423-187">Filtruje výsledky tooget jenom aplikace a uzly ve chyby nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="ef423-187">It filters results tooget only applications and nodes in error or warning.</span></span> <span data-ttu-id="ef423-188">V důsledku toho jsou vráceny žádné uzly, jako jsou všechny v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-188">As a result, no nodes are returned, as they are all healthy.</span></span> <span data-ttu-id="ef423-189">Pouze hello fabric: / aplikace WordCount respektuje filtr aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-189">Only hello fabric:/WordCount application respects hello applications filter.</span></span> <span data-ttu-id="ef423-190">Protože vlastní zásady hello určuje tooconsider upozornění jako chyby pro hello fabric: / WordCount aplikace, aplikace hello je vyhodnocena jako chyba, a stejně tak hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-190">Because hello custom policy specifies tooconsider warnings as errors for hello fabric:/WordCount application, hello application is evaluated as in error, and so is hello cluster.</span></span>

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.
                          
                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.
                          
                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                          
                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                          
                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                          
                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                          
                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                          
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="rest"></a><span data-ttu-id="ef423-191">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-191">REST</span></span>
<span data-ttu-id="ef423-192">Můžete získat stav clusteru s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-192">You can get cluster health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-node-health"></a><span data-ttu-id="ef423-193">Získat stav uzlu</span><span class="sxs-lookup"><span data-stu-id="ef423-193">Get node health</span></span>
<span data-ttu-id="ef423-194">Vrátí hello stavu uzlu entity a obsahuje události stavu hello ohlášeny hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-194">Returns hello health of a node entity and contains hello health events reported on hello node.</span></span> <span data-ttu-id="ef423-195">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-195">Input:</span></span>

* <span data-ttu-id="ef423-196">Název uzlu hello [požadované], identifikující hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-196">[Required] hello node name that identifies hello node.</span></span>
* <span data-ttu-id="ef423-197">Nastavení zásad stavu clusteru [Nepovinné] hello používá tooevaluate stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-197">[Optional] hello cluster health policy settings used tooevaluate health.</span></span>
* <span data-ttu-id="ef423-198">[Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-198">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-199">Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-199">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-200">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-200">API</span></span>
<span data-ttu-id="ef423-201">stav uzlu tooget prostřednictvím hello rozhraní API, vytvoření `FabricClient` a volání hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-201">tooget node health through hello API, create a `FabricClient` and call hello [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="ef423-202">Hello následující kód získá stav uzlu hello název zadaný uzel hello:</span><span class="sxs-lookup"><span data-stu-id="ef423-202">hello following code gets hello node health for hello specified node name:</span></span>

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

<span data-ttu-id="ef423-203">Hello následující kód získá hello uzlu stavu pro hello zadaný název uzlu a předává v filtr událostí a vlastní zásady prostřednictvím [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="ef423-203">hello following code gets hello node health for hello specified node name and passes in events filter and custom policy through [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):</span></span>

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="ef423-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-204">PowerShell</span></span>
<span data-ttu-id="ef423-205">Hello rutiny tooget hello uzlu stav je [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-205">hello cmdlet tooget hello node health is [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth).</span></span> <span data-ttu-id="ef423-206">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-206">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>
<span data-ttu-id="ef423-207">následující rutiny Hello získá stav uzlu hello pomocí výchozí zásady stavu:</span><span class="sxs-lookup"><span data-stu-id="ef423-207">hello following cmdlet gets hello node health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

<span data-ttu-id="ef423-208">Hello následující rutiny získá hello stav všech uzlů v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="ef423-208">hello following cmdlet gets hello health of all nodes in hello cluster:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a><span data-ttu-id="ef423-209">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-209">REST</span></span>
<span data-ttu-id="ef423-210">Můžete získat stav uzlu s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-210">You can get node health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-application-health"></a><span data-ttu-id="ef423-211">Získání stavu aplikací</span><span class="sxs-lookup"><span data-stu-id="ef423-211">Get application health</span></span>
<span data-ttu-id="ef423-212">Vrátí stav hello entity aplikací.</span><span class="sxs-lookup"><span data-stu-id="ef423-212">Returns hello health of an application entity.</span></span> <span data-ttu-id="ef423-213">Obsahuje hello stavů hello nasazené aplikace a služby dětí.</span><span class="sxs-lookup"><span data-stu-id="ef423-213">It contains hello health states of hello deployed application and service children.</span></span> <span data-ttu-id="ef423-214">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-214">Input:</span></span>

* <span data-ttu-id="ef423-215">[Požadované] hello název aplikace (URI) identifikující aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-215">[Required] hello application name (URI) that identifies hello application.</span></span>
* <span data-ttu-id="ef423-216">[Nepovinné] hello zásady stavu aplikace použít manifestu zásady aplikací toooverride hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-216">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="ef423-217">[Nepovinné] Filtry pro událostí, služeb a nasazené aplikace, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-217">[Optional] Filters for events, services, and deployed applications that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-218">Všechny události, služby a nasazené aplikace jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-218">All events, services, and deployed applications are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="ef423-219">[Nepovinné] Filtrujte tooexclude hello stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-219">[Optional] Filter tooexclude hello health statistics.</span></span> <span data-ttu-id="ef423-220">Pokud není zadaný, zahrnují hello stavu statistiky hello ok, upozornění a počet chyb pro všechny aplikace, děti: nasazené aplikace služby, oddíly, repliky a nasazené balíčky služeb.</span><span class="sxs-lookup"><span data-stu-id="ef423-220">If not specified, hello health statistics include hello ok, warning, and error count for all application children: services, partitions, replicas, deployed applications, and deployed service packages.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-221">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-221">API</span></span>
<span data-ttu-id="ef423-222">Stav aplikace tooget, vytvořit `FabricClient` a volání hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) metodu na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-222">tooget application health, create a `FabricClient` and call hello [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="ef423-223">Hello následující kód získá stav aplikace hello hello zadaný název aplikace (URI):</span><span class="sxs-lookup"><span data-stu-id="ef423-223">hello following code gets hello application health for hello specified application name (URI):</span></span>

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

<span data-ttu-id="ef423-224">Hello následující kód získá stav aplikace hello hello zadaný název aplikace (URI), s filtry a vlastní zásady zadaný prostřednictvím [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="ef423-224">hello following code gets hello application health for hello specified application name (URI), with filters and custom policies specified via [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).</span></span>

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="ef423-225">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-225">PowerShell</span></span>
<span data-ttu-id="ef423-226">Stav aplikací Hello rutiny tooget hello je [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="ef423-226">hello cmdlet tooget hello application health is [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="ef423-227">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-227">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-228">Hello následující rutina vrátí stav hello hello **fabric: / WordCount** aplikace:</span><span class="sxs-lookup"><span data-stu-id="ef423-228">hello following cmdlet returns hello health of hello **fabric:/WordCount** application:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok
                                  
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning
                                  
DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok
                                  
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok
                                  
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
                                  
HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

<span data-ttu-id="ef423-229">Hello následující předává rutiny prostředí PowerShell v vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="ef423-229">hello following PowerShell cmdlet passes in custom policies.</span></span> <span data-ttu-id="ef423-230">Také filtry, děti a události.</span><span class="sxs-lookup"><span data-stu-id="ef423-230">It also filters children and events.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.
                                  
ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error
                                  
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a><span data-ttu-id="ef423-231">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-231">REST</span></span>
<span data-ttu-id="ef423-232">Můžete získat stav aplikací s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-232">You can get application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-service-health"></a><span data-ttu-id="ef423-233">Získání stavu služby</span><span class="sxs-lookup"><span data-stu-id="ef423-233">Get service health</span></span>
<span data-ttu-id="ef423-234">Vrátí stav hello entity služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-234">Returns hello health of a service entity.</span></span> <span data-ttu-id="ef423-235">Obsahuje hello oddílu stavů.</span><span class="sxs-lookup"><span data-stu-id="ef423-235">It contains hello partition health states.</span></span> <span data-ttu-id="ef423-236">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-236">Input:</span></span>

* <span data-ttu-id="ef423-237">[Požadované] hello název služby (URI) identifikující hello služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-237">[Required] hello service name (URI) that identifies hello service.</span></span>
* <span data-ttu-id="ef423-238">[Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.</span><span class="sxs-lookup"><span data-stu-id="ef423-238">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="ef423-239">[Nepovinné] Filtry pro události a oddíly, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-239">[Optional] Filters for events and partitions that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-240">Všechny události a oddíly, které jsou používané tooevaluate hello entity agregovat stavu, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-240">All events and partitions are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="ef423-241">[Nepovinné] Filtrujte tooexclude stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-241">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="ef423-242">Pokud není zadaný, hello stavu statistiky zobrazit hello ok, upozornění a chyb počet pro všechny oddíly a repliky služby hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-242">If not specified, hello health statistics show hello ok, warning, and error count for all partitions and replicas of hello service.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-243">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-243">API</span></span>
<span data-ttu-id="ef423-244">Stav služby tooget prostřednictvím hello rozhraní API, vytvoření `FabricClient` a volání hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-244">tooget service health through hello API, create a `FabricClient` and call hello [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) method on its HealthManager.</span></span>

<span data-ttu-id="ef423-245">Hello následující příklad načte hello stav služby pomocí zadaného názvu služby (URI):</span><span class="sxs-lookup"><span data-stu-id="ef423-245">hello following example gets hello health of a service with specified service name (URI):</span></span>

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

<span data-ttu-id="ef423-246">Hello následující kód získá hello stavu služby pro hello zadaný název služby (URI), filtrů a vlastní zásady prostřednictvím [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span><span class="sxs-lookup"><span data-stu-id="ef423-246">hello following code gets hello service health for hello specified service name (URI), specifying filters and custom policy via [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):</span></span>

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="ef423-247">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-247">PowerShell</span></span>
<span data-ttu-id="ef423-248">Stav služby Hello rutiny tooget hello je [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-248">hello cmdlet tooget hello service health is [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth).</span></span> <span data-ttu-id="ef423-249">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-249">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-250">následující rutiny Hello získá stav služby hello pomocí výchozích zásad stavu:</span><span class="sxs-lookup"><span data-stu-id="ef423-250">hello following cmdlet gets hello service health by using default health policies:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                        
                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.
                        
                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="ef423-251">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-251">REST</span></span>
<span data-ttu-id="ef423-252">Můžete získat stav služby s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-252">You can get service health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-partition-health"></a><span data-ttu-id="ef423-253">Získání stavu oddílu</span><span class="sxs-lookup"><span data-stu-id="ef423-253">Get partition health</span></span>
<span data-ttu-id="ef423-254">Vrátí stav hello oddílu entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-254">Returns hello health of a partition entity.</span></span> <span data-ttu-id="ef423-255">Obsahuje hello repliky stavů.</span><span class="sxs-lookup"><span data-stu-id="ef423-255">It contains hello replica health states.</span></span> <span data-ttu-id="ef423-256">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-256">Input:</span></span>

* <span data-ttu-id="ef423-257">Oddíl [požadované] hello ID (GUID), který identifikuje hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="ef423-257">[Required] hello partition ID (GUID) that identifies hello partition.</span></span>
* <span data-ttu-id="ef423-258">[Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.</span><span class="sxs-lookup"><span data-stu-id="ef423-258">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="ef423-259">[Nepovinné] Filtry pro události a repliky, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-259">[Optional] Filters for events and replicas that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-260">Všechny události a repliky jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-260">All events and replicas are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="ef423-261">[Nepovinné] Filtrujte tooexclude stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-261">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="ef423-262">Pokud není zadaný, hello stavu statistiky zobrazit, kolik repliky byly ve ok, upozornění a chybové stavy.</span><span class="sxs-lookup"><span data-stu-id="ef423-262">If not specified, hello health statistics show how many replicas are in ok, warning, and error states.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-263">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-263">API</span></span>
<span data-ttu-id="ef423-264">Vytvoření stavu oddílu tooget prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-264">tooget partition health through hello API, create a `FabricClient` and call hello [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="ef423-265">toospecify volitelné parametry, vytvořte [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="ef423-265">toospecify optional parameters, create [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).</span></span>

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a><span data-ttu-id="ef423-266">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-266">PowerShell</span></span>
<span data-ttu-id="ef423-267">Hello rutiny tooget hello oddílu stav je [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-267">hello cmdlet tooget hello partition health is [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth).</span></span> <span data-ttu-id="ef423-268">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-268">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-269">Hello následující rutiny získá hello stavu pro všechny oddíly hello **fabric: / WordCount/WordCountService** služby a filtry se stavy repliky:</span><span class="sxs-lookup"><span data-stu-id="ef423-269">hello following cmdlet gets hello health for all partitions of hello **fabric:/WordCount/WordCountService** service and filters out replica health states:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer was unable toofind a placement for one or more of hello Service's Replicas:
                        Secondary replica could not be placed due toohello following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
                        
HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="ef423-270">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-270">REST</span></span>
<span data-ttu-id="ef423-271">Můžete získat stav oddílu s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-271">You can get partition health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-replica-health"></a><span data-ttu-id="ef423-272">Získání stavu repliky</span><span class="sxs-lookup"><span data-stu-id="ef423-272">Get replica health</span></span>
<span data-ttu-id="ef423-273">Vrátí stav hello repliku stavové služby nebo instance bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-273">Returns hello health of a stateful service replica or a stateless service instance.</span></span> <span data-ttu-id="ef423-274">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-274">Input:</span></span>

* <span data-ttu-id="ef423-275">[Požadované] hello ID (GUID) a repliky ID oddílu identifikující hello repliky.</span><span class="sxs-lookup"><span data-stu-id="ef423-275">[Required] hello partition ID (GUID) and replica ID that identifies hello replica.</span></span>
* <span data-ttu-id="ef423-276">Parametry zásady stavu aplikace [Nepovinné] hello používá manifestu zásady aplikací toooverride hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-276">[Optional] hello application health policy parameters used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="ef423-277">[Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-277">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-278">Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-278">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-279">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-279">API</span></span>
<span data-ttu-id="ef423-280">Vytvoření stavu repliky hello tooget prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-280">tooget hello replica health through hello API, create a `FabricClient` and call hello [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) method on its HealthManager.</span></span> <span data-ttu-id="ef423-281">toospecify advanced parametrů, použijte [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="ef423-281">toospecify advanced parameters, use [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).</span></span>

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a><span data-ttu-id="ef423-282">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-282">PowerShell</span></span>
<span data-ttu-id="ef423-283">Hello rutiny tooget hello repliky stav je [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-283">hello cmdlet tooget hello replica health is [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth).</span></span> <span data-ttu-id="ef423-284">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-284">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-285">Hello následující rutiny získá stav hello hello primární repliky pro všechny oddíly hello služby:</span><span class="sxs-lookup"><span data-stu-id="ef423-285">hello following cmdlet gets hello health of hello primary replica for all partitions of hello service:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="ef423-286">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-286">REST</span></span>
<span data-ttu-id="ef423-287">Můžete získat stav repliky se [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-287">You can get replica health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-application-health"></a><span data-ttu-id="ef423-288">Získat stav nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="ef423-288">Get deployed application health</span></span>
<span data-ttu-id="ef423-289">Vrátí stav hello aplikace nasazené na uzlu entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-289">Returns hello health of an application deployed on a node entity.</span></span> <span data-ttu-id="ef423-290">Obsahuje stav balíčku hello nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-290">It contains hello deployed service package health states.</span></span> <span data-ttu-id="ef423-291">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-291">Input:</span></span>

* <span data-ttu-id="ef423-292">Název aplikace [požadované] hello (URI) a název uzlu (string), které identifikují hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-292">[Required] hello application name (URI) and node name (string) that identify hello deployed application.</span></span>
* <span data-ttu-id="ef423-293">[Nepovinné] hello zásady stavu aplikace použít manifestu zásady aplikací toooverride hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-293">[Optional] hello application health policy used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="ef423-294">[Nepovinné] Filtry pro události a nasazené služby balíčky, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-294">[Optional] Filters for events and deployed service packages that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-295">Všechny události a nasazené balíčky služeb se stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-295">All events and deployed service packages are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>
* <span data-ttu-id="ef423-296">[Nepovinné] Filtrujte tooexclude stavu statistiky.</span><span class="sxs-lookup"><span data-stu-id="ef423-296">[Optional] Filter tooexclude health statistics.</span></span> <span data-ttu-id="ef423-297">Pokud není zadaný, zobrazit hello stavu statistiky hello počet nasazené balíčky služeb v ok, upozornění a chybové stavy.</span><span class="sxs-lookup"><span data-stu-id="ef423-297">If not specified, hello health statistics show hello number of deployed service packages in ok, warning, and error health states.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-298">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-298">API</span></span>
<span data-ttu-id="ef423-299">Vytvoření tooget hello stavu aplikace nasazené na uzlu prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-299">tooget hello health of an application deployed on a node through hello API, create a `FabricClient` and call hello [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) method on its HealthManager.</span></span> <span data-ttu-id="ef423-300">Volitelné parametry toospecify, použijte [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="ef423-300">toospecify optional parameters, use [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).</span></span>

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a><span data-ttu-id="ef423-301">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-301">PowerShell</span></span>
<span data-ttu-id="ef423-302">Hello rutiny tooget hello nasazené aplikace stav je [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="ef423-302">hello cmdlet tooget hello deployed application health is [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps).</span></span> <span data-ttu-id="ef423-303">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-303">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="ef423-304">Spustit toofind, na kterém je aplikace nasazena, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) a podívejte se na hello nasazené aplikace podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-304">toofind out where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed application children.</span></span>

<span data-ttu-id="ef423-305">Hello následující rutiny získá stav hello hello **fabric: / WordCount** aplikace nasazené na **to uzel _Node_2**.</span><span class="sxs-lookup"><span data-stu-id="ef423-305">hello following cmdlet gets hello health of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : hello application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM
                                     
HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a><span data-ttu-id="ef423-306">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-306">REST</span></span>
<span data-ttu-id="ef423-307">Můžete získat stav nasazení aplikace s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-307">You can get deployed application health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="get-deployed-service-package-health"></a><span data-ttu-id="ef423-308">Získat stav balíčku nasazené služby</span><span class="sxs-lookup"><span data-stu-id="ef423-308">Get deployed service package health</span></span>
<span data-ttu-id="ef423-309">Vrátí hello stavu entity balíček nasazené služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-309">Returns hello health of a deployed service package entity.</span></span> <span data-ttu-id="ef423-310">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-310">Input:</span></span>

* <span data-ttu-id="ef423-311">Název aplikace [požadované] hello (URI), název uzlu (string) a manifestu název služby (string), které identifikují hello nasadit balíček služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-311">[Required] hello application name (URI), node name (string), and service manifest name (string) that identify hello deployed service package.</span></span>
* <span data-ttu-id="ef423-312">[Nepovinné] hello zásady stavu aplikace použít toooverride hello aplikace manifestu zásad.</span><span class="sxs-lookup"><span data-stu-id="ef423-312">[Optional] hello application health policy used toooverride hello application manifest policy.</span></span>
* <span data-ttu-id="ef423-313">[Nepovinné] Filtry pro události, které určují, položky, které jsou v zájmu a má být vrácen ve výsledku hello (například pouze chyby nebo upozornění i chyby).</span><span class="sxs-lookup"><span data-stu-id="ef423-313">[Optional] Filters for events that specify which entries are of interest and should be returned in hello result (for example, errors only, or both warnings and errors).</span></span> <span data-ttu-id="ef423-314">Všechny události jsou stav entity agregovat hello použité tooevaluate, bez ohledu na to hello filtru.</span><span class="sxs-lookup"><span data-stu-id="ef423-314">All events are used tooevaluate hello entity aggregated health, regardless of hello filter.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-315">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-315">API</span></span>
<span data-ttu-id="ef423-316">Vytvoření tooget hello stavu balíčku nasazené služby prostřednictvím hello rozhraní API, `FabricClient` a volání hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) metoda na jeho HealthManager.</span><span class="sxs-lookup"><span data-stu-id="ef423-316">tooget hello health of a deployed service package through hello API, create a `FabricClient` and call hello [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) method on its HealthManager.</span></span> <span data-ttu-id="ef423-317">Volitelné parametry toospecify, použijte [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span><span class="sxs-lookup"><span data-stu-id="ef423-317">toospecify optional parameters, use [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).</span></span>

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a><span data-ttu-id="ef423-318">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-318">PowerShell</span></span>
<span data-ttu-id="ef423-319">Hello rutiny tooget hello nasazené služby balíček stav je [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span><span class="sxs-lookup"><span data-stu-id="ef423-319">hello cmdlet tooget hello deployed service package health is [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth).</span></span> <span data-ttu-id="ef423-320">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-320">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span> <span data-ttu-id="ef423-321">Spustit toosee, kde je aplikace nasazena, [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) a prohlédněte si hello nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-321">toosee where an application is deployed, run [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) and look at hello deployed applications.</span></span> <span data-ttu-id="ef423-322">toosee, které balíčky služeb jsou v aplikaci, vyhledejte v hello nasazené služby balíček dětech do hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) výstup.</span><span class="sxs-lookup"><span data-stu-id="ef423-322">toosee which service packages are in an application, look at hello deployed service package children in hello [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) output.</span></span>

<span data-ttu-id="ef423-323">Hello následující rutiny získá stav hello hello **WordCountServicePkg** balíček služby hello **fabric: / WordCount** aplikace nasazené na **to uzel _Node_2**.</span><span class="sxs-lookup"><span data-stu-id="ef423-323">hello following cmdlet gets hello health of hello **WordCountServicePkg** service package of hello **fabric:/WordCount** application deployed on **_Node_2**.</span></span> <span data-ttu-id="ef423-324">Hello entita má **System.Hosting** sestavy pro úspěšné aktivaci balíček služby a vstupní bod a úspěšnou registraci typ služby.</span><span class="sxs-lookup"><span data-stu-id="ef423-324">hello entity has **System.Hosting** reports for successful service-package and entry-point activation, and successful service-type registration.</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : hello ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a><span data-ttu-id="ef423-325">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-325">REST</span></span>
<span data-ttu-id="ef423-326">Můžete získat stav balíčku nasazené služby s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) , který obsahuje zásady stavu, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-326">You can get deployed service package health with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) that includes health policies described in hello body.</span></span>

## <a name="health-chunk-queries"></a><span data-ttu-id="ef423-327">Dotazy na stav bloku</span><span class="sxs-lookup"><span data-stu-id="ef423-327">Health chunk queries</span></span>
<span data-ttu-id="ef423-328">Hello stavu bloku dotazy mohou vracet podřízené víceúrovňovou clusteru (rekurzivně), za vstupní filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-328">hello health chunk queries can return multi-level cluster children (recursively), per input filters.</span></span> <span data-ttu-id="ef423-329">Podporuje rozšířené filtry, které umožňují značnou flexibilitu při volbě hello, děti toobe vrátila.</span><span class="sxs-lookup"><span data-stu-id="ef423-329">It supports advanced filters that allow a lot of flexibility in choosing hello children toobe returned.</span></span> <span data-ttu-id="ef423-330">hello jedinečný identifikátor nebo jiné skupiny nebo stavy, můžete zadat filtry Hello podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-330">hello filters can specify children by hello unique identifier or by other group identifiers and/or health states.</span></span> <span data-ttu-id="ef423-331">Ve výchozím nastavení žádné podřízené objekty jsou zahrnuty jako názvem na rozdíl od toohealth příkazy, které vždy zahrnovat první úrovně podřízené objekty.</span><span class="sxs-lookup"><span data-stu-id="ef423-331">By default, no children are included, as opposed toohealth commands that always include first-level children.</span></span>

<span data-ttu-id="ef423-332">Hello [dotazů na stav](service-fabric-view-entities-aggregated-health.md#health-queries) návratový pouze první úroveň podřízených prvků hello zadané entity na požadované filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-332">hello [health queries](service-fabric-view-entities-aggregated-health.md#health-queries) return only first-level children of hello specified entity per required filters.</span></span> <span data-ttu-id="ef423-333">podřízené objekty hello tooget hello podřízených prvků, musí volat další stavu rozhraní API pro každé entity, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="ef423-333">tooget hello children of hello children, you must call additional health APIs for each entity of interest.</span></span> <span data-ttu-id="ef423-334">Podobně tooget hello stav konkrétní entit, je třeba zavolat jednoho stavu rozhraní API pro každou požadovanou entitu.</span><span class="sxs-lookup"><span data-stu-id="ef423-334">Similarly, tooget hello health of specific entities, you must call one health API for each desired entity.</span></span> <span data-ttu-id="ef423-335">Hello bloku dotazu rozšířené filtrování vám umožní toorequest více položek, které vás zajímají v jednom dotazu, minimalizovat velikost zprávy hello a hello počet zpráv.</span><span class="sxs-lookup"><span data-stu-id="ef423-335">hello chunk query advanced filtering allows you toorequest multiple items of interest in one query, minimizing hello message size and hello number of messages.</span></span>

<span data-ttu-id="ef423-336">Hodnota Hello hello bloku dotazu je, že můžete získat stav pro další clusteru entity (potenciálně všechny clusteru entity začínající na požadovaný kořenový) v jednom volání.</span><span class="sxs-lookup"><span data-stu-id="ef423-336">hello value of hello chunk query is that you can get health state for more cluster entities (potentially all cluster entities starting at required root) in one call.</span></span> <span data-ttu-id="ef423-337">Komplexní stavu dotazu lze vyjádřit jako:</span><span class="sxs-lookup"><span data-stu-id="ef423-337">You can express complex health query such as:</span></span>

* <span data-ttu-id="ef423-338">Návratový pouze aplikace, které jsou v chybě a pro tyto aplikace zahrnout všechny služby upozornění nebo chyby.</span><span class="sxs-lookup"><span data-stu-id="ef423-338">Return only applications in error, and for those applications include all services in warning or error.</span></span> <span data-ttu-id="ef423-339">Vrácených služeb zahrnují všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="ef423-339">For returned services, include all partitions.</span></span>
* <span data-ttu-id="ef423-340">Vrátí pouze hello stav čtyři aplikací, určeného jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="ef423-340">Return only hello health of four applications, specified by their names.</span></span>
* <span data-ttu-id="ef423-341">Vrátí pouze hello stav aplikací typu požadované aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-341">Return only hello health of applications of a desired application type.</span></span>
* <span data-ttu-id="ef423-342">Vrátí všechny nasazené entity na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-342">Return all deployed entities on a node.</span></span> <span data-ttu-id="ef423-343">Vrátí všechny aplikace, všechny nasazené aplikace v hello zadaný uzel a všechny hello nasazené balíčky služeb v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-343">Returns all applications, all deployed applications on hello specified node and all hello deployed service packages on that node.</span></span>
* <span data-ttu-id="ef423-344">Vrátí všechny repliky v chybě.</span><span class="sxs-lookup"><span data-stu-id="ef423-344">Return all replicas in error.</span></span> <span data-ttu-id="ef423-345">Vrátí všechny aplikace, služby, oddíly a pouze repliky v chybě.</span><span class="sxs-lookup"><span data-stu-id="ef423-345">Returns all applications, services, partitions, and only replicas in error.</span></span>
* <span data-ttu-id="ef423-346">Vrátí všechny aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-346">Return all applications.</span></span> <span data-ttu-id="ef423-347">Zadaná služba zahrnují všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="ef423-347">For a specified service, include all partitions.</span></span>

<span data-ttu-id="ef423-348">V současné době dotazu bloku hello stavu je vystaven pouze pro entitu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-348">Currently, hello health chunk query is exposed only for hello cluster entity.</span></span> <span data-ttu-id="ef423-349">Vrátí bloku stavu clusteru, který obsahuje:</span><span class="sxs-lookup"><span data-stu-id="ef423-349">It returns a cluster health chunk, which contains:</span></span>

* <span data-ttu-id="ef423-350">Hello clusteru agregován stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-350">hello cluster aggregated health state.</span></span>
* <span data-ttu-id="ef423-351">Hello stavu stavu bloku seznam uzlů, které respektují vstupní filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-351">hello health state chunk list of nodes that respect input filters.</span></span>
* <span data-ttu-id="ef423-352">Hello stavu stavu bloku seznam aplikací, které respektují vstupní filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-352">hello health state chunk list of applications that respect input filters.</span></span> <span data-ttu-id="ef423-353">Každého bloku stavu stavu aplikace obsahuje seznam bloků dat u všech služeb, které respektují vstupní filtry a seznam bloků dat s všechny nasazené aplikace, které respektují hello filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-353">Each application health state chunk contains a chunk list with all services that respect input filters and a chunk list with all deployed applications that respect hello filters.</span></span> <span data-ttu-id="ef423-354">Stejný pro děti hello služeb a nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-354">Same for hello children of services and deployed applications.</span></span> <span data-ttu-id="ef423-355">Tímto způsobem všechny entity v clusteru hello se může potenciálně vracet Pokud vyžaduje, je hierarchický.</span><span class="sxs-lookup"><span data-stu-id="ef423-355">This way, all entities in hello cluster can be potentially returned if requested, in a hierarchical fashion.</span></span>

### <a name="cluster-health-chunk-query"></a><span data-ttu-id="ef423-356">Dotaz bloku stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="ef423-356">Cluster health chunk query</span></span>
<span data-ttu-id="ef423-357">Vrátí hello stavu entity hello clusteru a obsahuje hello hierarchické stavu stavu bloky požadované podřízených prvků.</span><span class="sxs-lookup"><span data-stu-id="ef423-357">Returns hello health of hello cluster entity and contains hello hierarchical health state chunks of required children.</span></span> <span data-ttu-id="ef423-358">Vstup:</span><span class="sxs-lookup"><span data-stu-id="ef423-358">Input:</span></span>

* <span data-ttu-id="ef423-359">[Nepovinné] hello zásady stavu clusteru používá tooevaluate hello uzly a události hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-359">[Optional] hello cluster health policy used tooevaluate hello nodes and hello cluster events.</span></span>
* <span data-ttu-id="ef423-360">[Nepovinné] hello mapy zásady stavu aplikace, pomocí zásad stavu hello používá manifestu zásady aplikací toooverride hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-360">[Optional] hello application health policy map, with hello health policies used toooverride hello application manifest policies.</span></span>
* <span data-ttu-id="ef423-361">[Nepovinné] Filtry pro uzly a aplikace, které určí položky, které jsou v zájmu a má být vrácen ve výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-361">[Optional] Filters for nodes and applications that specify which entries are of interest and should be returned in hello result.</span></span> <span data-ttu-id="ef423-362">Hello filtry jsou konkrétní tooan entity nebo skupiny entit nebo použít tooall entity na této úrovni.</span><span class="sxs-lookup"><span data-stu-id="ef423-362">hello filters are specific tooan entity/group of entities or are applicable tooall entities at that level.</span></span> <span data-ttu-id="ef423-363">Hello seznam filtrů může obsahovat jeden obecné filtr a filtry pro konkrétní identifikátory toofine intervalem entity vrácené dotazem hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-363">hello list of filters can contain one general filter and/or filters for specific identifiers toofine-grain entities returned by hello query.</span></span> <span data-ttu-id="ef423-364">Pokud je prázdný, děti hello nebudou zobrazeny ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ef423-364">If empty, hello children are not returned by default.</span></span>
  <span data-ttu-id="ef423-365">Další informace o hello filtrů při [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) a [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span><span class="sxs-lookup"><span data-stu-id="ef423-365">Read more about hello filters at [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) and [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter).</span></span> <span data-ttu-id="ef423-366">rekurzivní může filtry aplikace Hello zadejte rozšířené filtry pro děti.</span><span class="sxs-lookup"><span data-stu-id="ef423-366">hello application filters can recursively specify advanced filters for children.</span></span>

<span data-ttu-id="ef423-367">výsledek bloku Hello zahrnuje hello podřízené objekty, které respektují hello filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-367">hello chunk result includes hello children that respect hello filters.</span></span>

<span data-ttu-id="ef423-368">V současné době hello bloku dotaz nevrátí, není v pořádku hodnocení nebo události entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-368">Currently, hello chunk query does not return unhealthy evaluations or entity events.</span></span> <span data-ttu-id="ef423-369">Tyto doplňující informace lze získat pomocí hello existující cluster stavu dotaz.</span><span class="sxs-lookup"><span data-stu-id="ef423-369">That extra information can be obtained using hello existing cluster health query.</span></span>

### <a name="api"></a><span data-ttu-id="ef423-370">Rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef423-370">API</span></span>
<span data-ttu-id="ef423-371">stav clusteru tooget bloku dat, vytvořte `FabricClient` a volání hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) metoda na jeho **HealthManager**.</span><span class="sxs-lookup"><span data-stu-id="ef423-371">tooget cluster health chunk, create a `FabricClient` and call hello [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) method on its **HealthManager**.</span></span> <span data-ttu-id="ef423-372">Abyste mohli předávat [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe stavu zásad a rozšířené filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-372">You can pass in [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) toodescribe health policies and advanced filters.</span></span>

<span data-ttu-id="ef423-373">Hello následující kód získá bloku stavu clusteru se rozšířené filtry.</span><span class="sxs-lookup"><span data-stu-id="ef423-373">hello following code gets cluster health chunk with advanced filters.</span></span>

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except hello ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a><span data-ttu-id="ef423-374">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef423-374">PowerShell</span></span>
<span data-ttu-id="ef423-375">Hello rutiny tooget hello clusteru stav je [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span><span class="sxs-lookup"><span data-stu-id="ef423-375">hello cmdlet tooget hello cluster health is [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk).</span></span> <span data-ttu-id="ef423-376">Nejdřív připojit toohello clusteru pomocí hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ef423-376">First, connect toohello cluster by using hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet.</span></span>

<span data-ttu-id="ef423-377">Hello následující kód získá uzly pouze v případě, že se chyba s výjimkou konkrétním uzlu, který má být vždy vrácen.</span><span class="sxs-lookup"><span data-stu-id="ef423-377">hello following code gets nodes only if they are in Error except for a specific node, which should always be returned.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in hello cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1
                               
                               NodeName              : _Node_1
                               HealthState           : Ok
                               
ApplicationHealthStateChunks : None
```

<span data-ttu-id="ef423-378">následující rutiny Hello získá clusteru bloku s filtry aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-378">hello following cmdlet gets cluster chunk with application filters.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1
                               
                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1
                               
                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5
                               
                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok
                               
                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

<span data-ttu-id="ef423-379">Hello následující rutina vrací všechny nasazené entity na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-379">hello following cmdlet returns all deployed entities on a node.</span></span>

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2
                               
                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
                               
                               
                               
                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1
                               
                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1
                               
                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a><span data-ttu-id="ef423-380">REST</span><span class="sxs-lookup"><span data-stu-id="ef423-380">REST</span></span>
<span data-ttu-id="ef423-381">Můžete získat blok stavu clusteru s [požadavek GET](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) nebo [požadavek POST](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) zahrnující zásady stavu a rozšířené filtry, které jsou popsané v textu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-381">You can get cluster health chunk with a [GET request](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) or a [POST request](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) that includes health policies and advanced filters described in hello body.</span></span>

## <a name="general-queries"></a><span data-ttu-id="ef423-382">Obecné dotazy</span><span class="sxs-lookup"><span data-stu-id="ef423-382">General queries</span></span>
<span data-ttu-id="ef423-383">Obecné dotazy vrátí seznam entity prostředků infrastruktury služby zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="ef423-383">General queries return a list of Service Fabric entities of a specified type.</span></span> <span data-ttu-id="ef423-384">Se zveřejňují přes hello rozhraní API (prostřednictvím metody hello na **FabricClient.QueryManager**), rutiny prostředí PowerShell a REST.</span><span class="sxs-lookup"><span data-stu-id="ef423-384">They are exposed through hello API (via hello methods on **FabricClient.QueryManager**), PowerShell cmdlets, and REST.</span></span> <span data-ttu-id="ef423-385">Tyto dotazy agregovat poddotazy z několika součástí.</span><span class="sxs-lookup"><span data-stu-id="ef423-385">These queries aggregate subqueries from multiple components.</span></span> <span data-ttu-id="ef423-386">Jeden z nich je hello [úložiště stavu](service-fabric-health-introduction.md#health-store), který naplní hello Agregovat stav pro každý výsledek dotazu.</span><span class="sxs-lookup"><span data-stu-id="ef423-386">One of them is hello [health store](service-fabric-health-introduction.md#health-store), which populates hello aggregated health state for each query result.</span></span>  

> [!NOTE]
> <span data-ttu-id="ef423-387">Obecné dotazy vrátit stav hello agregovat hello entity a neobsahuje data bohaté stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-387">General queries return hello aggregated health state of hello entity and do not contain rich health data.</span></span> <span data-ttu-id="ef423-388">Pokud entity není v pořádku, můžete sledovat pomocí tooget dotazy stavu všechny jeho stavu informace, včetně událostí, podřízené stavů a není v pořádku hodnocení.</span><span class="sxs-lookup"><span data-stu-id="ef423-388">If an entity is not healthy, you can follow up with health queries tooget all its health information, including events, child health states, and unhealthy evaluations.</span></span>
>
>

<span data-ttu-id="ef423-389">Pokud obecné dotazy vrátí Neznámý stav pro entitu, je možné že toto úložiště stavu hello nemá dokončení data o hello entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-389">If general queries return an unknown health state for an entity, it's possible that hello health store doesn't have complete data about hello entity.</span></span> <span data-ttu-id="ef423-390">Je také možné, že úložiště stavu toohello poddotazu nebyla úspěšná (například došlo k chybě komunikace, nebo byla omezena hello stavu úložiště).</span><span class="sxs-lookup"><span data-stu-id="ef423-390">It's also possible that a subquery toohello health store wasn't successful (for example, there was a communication error, or hello health store was throttled).</span></span> <span data-ttu-id="ef423-391">Pomocí dotazu stavu pro entitu hello následnou akci.</span><span class="sxs-lookup"><span data-stu-id="ef423-391">Follow up with a health query for hello entity.</span></span> <span data-ttu-id="ef423-392">Pokud hello poddotazu došlo k přechodné chyby, například problémy se sítí, může být úspěšné následné dotaz.</span><span class="sxs-lookup"><span data-stu-id="ef423-392">If hello subquery encountered transient errors, such as network issues, this follow-up query may succeed.</span></span> <span data-ttu-id="ef423-393">Ho může také získáte další informace z health store hello o proč nebude vystavena hello entity.</span><span class="sxs-lookup"><span data-stu-id="ef423-393">It may also give you more details from hello health store about why hello entity is not exposed.</span></span>

<span data-ttu-id="ef423-394">Hello dotazů, které obsahují **HealthState** pro entity jsou:</span><span class="sxs-lookup"><span data-stu-id="ef423-394">hello queries that contain **HealthState** for entities are:</span></span>

* <span data-ttu-id="ef423-395">Seznam uzlů: vrátí hello seznamu uzlů v clusteru hello (stránkovaného).</span><span class="sxs-lookup"><span data-stu-id="ef423-395">Node list: Returns hello list nodes in hello cluster (paged).</span></span>
  * <span data-ttu-id="ef423-396">Rozhraní API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-396">API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)</span></span>
  * <span data-ttu-id="ef423-397">Prostředí PowerShell: Get-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="ef423-397">PowerShell: Get-ServiceFabricNode</span></span>
* <span data-ttu-id="ef423-398">Seznam aplikací: vrátí hello seznam aplikací v clusteru hello (stránkovaného).</span><span class="sxs-lookup"><span data-stu-id="ef423-398">Application list: Returns hello list of applications in hello cluster (paged).</span></span>
  * <span data-ttu-id="ef423-399">Rozhraní API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-399">API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)</span></span>
  * <span data-ttu-id="ef423-400">Prostředí PowerShell: Get-ServiceFabricApplication</span><span class="sxs-lookup"><span data-stu-id="ef423-400">PowerShell: Get-ServiceFabricApplication</span></span>
* <span data-ttu-id="ef423-401">Seznam služeb: vrátí hello seznam služeb v aplikaci (stránkovaného).</span><span class="sxs-lookup"><span data-stu-id="ef423-401">Service list: Returns hello list of services in an application (paged).</span></span>
  * <span data-ttu-id="ef423-402">Rozhraní API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-402">API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)</span></span>
  * <span data-ttu-id="ef423-403">Prostředí PowerShell: Get-ServiceFabricService</span><span class="sxs-lookup"><span data-stu-id="ef423-403">PowerShell: Get-ServiceFabricService</span></span>
* <span data-ttu-id="ef423-404">Seznam oddílů: vrátí hello seznam oddílů ve službě (stránkovaného).</span><span class="sxs-lookup"><span data-stu-id="ef423-404">Partition list: Returns hello list of partitions in a service (paged).</span></span>
  * <span data-ttu-id="ef423-405">Rozhraní API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-405">API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)</span></span>
  * <span data-ttu-id="ef423-406">Prostředí PowerShell: Get-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="ef423-406">PowerShell: Get-ServiceFabricPartition</span></span>
* <span data-ttu-id="ef423-407">Seznam replik: vrátí hello seznam replik v oddílu (stránkovaného).</span><span class="sxs-lookup"><span data-stu-id="ef423-407">Replica list: Returns hello list of replicas in a partition (paged).</span></span>
  * <span data-ttu-id="ef423-408">Rozhraní API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-408">API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)</span></span>
  * <span data-ttu-id="ef423-409">Prostředí PowerShell: Get-ServiceFabricReplica</span><span class="sxs-lookup"><span data-stu-id="ef423-409">PowerShell: Get-ServiceFabricReplica</span></span>
* <span data-ttu-id="ef423-410">Nasazení seznam aplikací: vrátí hello seznam nasazené aplikace na uzlu.</span><span class="sxs-lookup"><span data-stu-id="ef423-410">Deployed application list: Returns hello list of deployed applications on a node.</span></span>
  * <span data-ttu-id="ef423-411">Rozhraní API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-411">API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)</span></span>
  * <span data-ttu-id="ef423-412">Prostředí PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="ef423-412">PowerShell: Get-ServiceFabricDeployedApplication</span></span>
* <span data-ttu-id="ef423-413">Nasazení služby seznam balíčků: hello vrátí seznam balíčků služeb v nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-413">Deployed service package list: Returns hello list of service packages in a deployed application.</span></span>
  * <span data-ttu-id="ef423-414">Rozhraní API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span><span class="sxs-lookup"><span data-stu-id="ef423-414">API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)</span></span>
  * <span data-ttu-id="ef423-415">Prostředí PowerShell: Get-ServiceFabricDeployedApplication</span><span class="sxs-lookup"><span data-stu-id="ef423-415">PowerShell: Get-ServiceFabricDeployedApplication</span></span>

> [!NOTE]
> <span data-ttu-id="ef423-416">Některé dotazy hello vrátí stránkových výsledků.</span><span class="sxs-lookup"><span data-stu-id="ef423-416">Some of hello queries return paged results.</span></span> <span data-ttu-id="ef423-417">Hello vrátit těchto dotazů je seznam odvozené od [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span><span class="sxs-lookup"><span data-stu-id="ef423-417">hello return of these queries is a list derived from [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1).</span></span> <span data-ttu-id="ef423-418">Pokud výsledky hello nebudou vyhovovat zprávu, je vrácen pouze na stránce a ContinuationToken, který sleduje kde výčtu zastavena.</span><span class="sxs-lookup"><span data-stu-id="ef423-418">If hello results do not fit a message, only a page is returned and a ContinuationToken that tracks where enumeration stopped.</span></span> <span data-ttu-id="ef423-419">Pokračujte toocall hello stejný dotaz a předejte token pokračování hello z hello předchozí tooget další výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="ef423-419">Continue toocall hello same query and pass in hello continuation token from hello previous query tooget next results.</span></span>
>
>

### <a name="examples"></a><span data-ttu-id="ef423-420">Příklady</span><span class="sxs-lookup"><span data-stu-id="ef423-420">Examples</span></span>
<span data-ttu-id="ef423-421">Hello následující kód získá hello není v pořádku aplikace v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="ef423-421">hello following code gets hello unhealthy applications in hello cluster:</span></span>

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

<span data-ttu-id="ef423-422">Hello následující rutiny získá hello aplikace podrobnosti hello fabric: / WordCount aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-422">hello following cmdlet gets hello application details for hello fabric:/WordCount application.</span></span> <span data-ttu-id="ef423-423">Všimněte si, že stav je v upozornění.</span><span class="sxs-lookup"><span data-stu-id="ef423-423">Notice that health state is at warning.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

<span data-ttu-id="ef423-424">Hello následující rutiny získá hello služby s stav chyby:</span><span class="sxs-lookup"><span data-stu-id="ef423-424">hello following cmdlet gets hello services with a health state of error:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a><span data-ttu-id="ef423-425">Upgrady cluster a aplikace</span><span class="sxs-lookup"><span data-stu-id="ef423-425">Cluster and application upgrades</span></span>
<span data-ttu-id="ef423-426">Během monitorovaných upgrade hello cluster a aplikace Service Fabric kontroluje tooensure stavu, že všechno zůstane v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-426">During a monitored upgrade of hello cluster and application, Service Fabric checks health tooensure that everything remains healthy.</span></span> <span data-ttu-id="ef423-427">Pokud není v pořádku, jak se vyhodnocují se pomocí zásady nakonfigurované stavu entity se hello upgrade platí zásady specifické pro upgrade toodetermine hello další akce.</span><span class="sxs-lookup"><span data-stu-id="ef423-427">If an entity is unhealthy as evaluated by using configured health policies, hello upgrade applies upgrade-specific policies toodetermine hello next action.</span></span> <span data-ttu-id="ef423-428">Hello upgradu může být pozastavený tooallow zásah uživatele (například opravě chybové stavy nebo změna zásad) nebo ji může automaticky vrácení předchozí verze dobrý toohello.</span><span class="sxs-lookup"><span data-stu-id="ef423-428">hello upgrade may be paused tooallow user interaction (such as fixing error conditions or changing policies), or it may automatically roll back toohello previous good version.</span></span>

<span data-ttu-id="ef423-429">Během *clusteru* upgradu, můžete získat stav upgradu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ef423-429">During a *cluster* upgrade, you can get hello cluster upgrade status.</span></span> <span data-ttu-id="ef423-430">Stav upgradu Hello zahrnuje není v pořádku hodnocení, které toowhat bod je v clusteru hello v chybném stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-430">hello upgrade status includes unhealthy evaluations, which point toowhat is unhealthy in hello cluster.</span></span> <span data-ttu-id="ef423-431">Pokud se hello upgrade je vrácena zpět z důvodu toohealth problémy, stav upgradu hello pamatuje hello poslední není v pořádku důvodů.</span><span class="sxs-lookup"><span data-stu-id="ef423-431">If hello upgrade is rolled back due toohealth issues, hello upgrade status remembers hello last unhealthy reasons.</span></span> <span data-ttu-id="ef423-432">Tyto informace mohou pomoci správci prozkoumat, kde došlo k chybě po upgradu hello vrácena nebo byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="ef423-432">This information can help administrators investigate what went wrong after hello upgrade rolled back or stopped.</span></span>

<span data-ttu-id="ef423-433">Podobně při *aplikace* upgrade všech není v pořádku hodnocení jsou obsaženy v stav upgradu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-433">Similarly, during an *application* upgrade, any unhealthy evaluations are contained in hello application upgrade status.</span></span>

<span data-ttu-id="ef423-434">Hello následující ukazuje stav upgradu aplikace hello pro upravené fabric: / WordCount aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef423-434">hello following shows hello application upgrade status for a modified fabric:/WordCount application.</span></span> <span data-ttu-id="ef423-435">Sledovací zařízení ohlásilo chybu na jednom z jejích replik.</span><span class="sxs-lookup"><span data-stu-id="ef423-435">A watchdog reported an error on one of its replicas.</span></span> <span data-ttu-id="ef423-436">Hello upgrade je vrácení zpět, protože nejsou dodržovány kontroly stavu hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-436">hello upgrade is rolling back because hello health checks are not respected.</span></span>

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

<span data-ttu-id="ef423-437">Další informace o hello [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="ef423-437">Read more about hello [Service Fabric application upgrade](service-fabric-application-upgrade.md).</span></span>

## <a name="use-health-evaluations-tootroubleshoot"></a><span data-ttu-id="ef423-438">Použít tootroubleshoot hodnocení stavu</span><span class="sxs-lookup"><span data-stu-id="ef423-438">Use health evaluations tootroubleshoot</span></span>
<span data-ttu-id="ef423-439">Vždy, když nastane problém s hello clusteru nebo aplikaci, podívejte se na toopinpoint stavu clusteru nebo aplikace hello co je nesprávný.</span><span class="sxs-lookup"><span data-stu-id="ef423-439">Whenever there is an issue with hello cluster or an application, look at hello cluster or application health toopinpoint what is wrong.</span></span> <span data-ttu-id="ef423-440">není v pořádku hodnocení Hello obsahují podrobné informace o jaké spouštěná hello stav aktuální není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="ef423-440">hello unhealthy evaluations provide details about what triggered hello current unhealthy state.</span></span> <span data-ttu-id="ef423-441">Pokud potřebujete, můžete k podrobnostem na není v pořádku podřízených entit tooidentify hello hlavní příčinu.</span><span class="sxs-lookup"><span data-stu-id="ef423-441">If you need to, you can drill down into unhealthy child entities tooidentify hello root cause.</span></span>

<span data-ttu-id="ef423-442">Představte si třeba aplikace není v pořádku, protože není zprávu o chybách na jednom z jejích replik.</span><span class="sxs-lookup"><span data-stu-id="ef423-442">For example, consider an application unhealthy because there is an error report on one of its replicas.</span></span> <span data-ttu-id="ef423-443">Hello následující rutiny prostředí Powershell ukazuje, není v pořádku hodnocení hello:</span><span class="sxs-lookup"><span data-stu-id="ef423-443">hello following Powershell cmdlet shows hello unhealthy evaluations:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.
                                  
                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.
                                  
                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.
                                  
                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.
                                  
                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.
                                  
                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.
                                  
                                            Error event: SourceId='MyWatchdog', Property='Memory'.
                                  
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

<span data-ttu-id="ef423-444">Můžete se podívat na hello repliky tooget Další informace:</span><span class="sxs-lookup"><span data-stu-id="ef423-444">You can look at hello replica tooget more information:</span></span>

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
                        
                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="ef423-445">Hello není v pořádku hodnocení zobrazit hello prvním důvodem je hello entity vyhodnotit toocurrent stavu.</span><span class="sxs-lookup"><span data-stu-id="ef423-445">hello unhealthy evaluations show hello first reason hello entity is evaluated toocurrent health state.</span></span> <span data-ttu-id="ef423-446">Může být více událostí, které spouštějí tento stav, ale neprojeví v hello hodnocení.</span><span class="sxs-lookup"><span data-stu-id="ef423-446">There may be multiple other events that trigger this state, but they are not be reflected in hello evaluations.</span></span> <span data-ttu-id="ef423-447">tooget Další informace, rozbalení do hello stavu entity toofigure na všechny sestavy není v pořádku hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ef423-447">tooget more information, drill down into hello health entities toofigure out all hello unhealthy reports in hello cluster.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="ef423-448">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef423-448">Next steps</span></span>
[<span data-ttu-id="ef423-449">Použít tootroubleshoot sestavy stavu systému</span><span class="sxs-lookup"><span data-stu-id="ef423-449">Use system health reports tootroubleshoot</span></span>](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[<span data-ttu-id="ef423-450">Přidat vlastní sestavy stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ef423-450">Add custom Service Fabric health reports</span></span>](service-fabric-report-health.md)

[<span data-ttu-id="ef423-451">Jak tooreport a zkontrolujte stav služby</span><span class="sxs-lookup"><span data-stu-id="ef423-451">How tooreport and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[<span data-ttu-id="ef423-452">Monitorování a Diagnostika služby místně</span><span class="sxs-lookup"><span data-stu-id="ef423-452">Monitor and diagnose services locally</span></span>](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[<span data-ttu-id="ef423-453">Upgrade aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ef423-453">Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)
