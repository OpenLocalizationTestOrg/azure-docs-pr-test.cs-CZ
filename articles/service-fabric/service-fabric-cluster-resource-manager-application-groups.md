---
title: "Správce prostředků clusteru služby Fabric - skupin aplikací | Microsoft Docs"
description: "Přehled funkcí skupiny aplikací na portálu Service Fabric clusteru Resource Manager"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7fc61731c8df2a0c3dc5b77ae718b41c240f9233
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-application-groups"></a><span data-ttu-id="4e141-103">Úvod do skupiny aplikací</span><span class="sxs-lookup"><span data-stu-id="4e141-103">Introduction to Application Groups</span></span>
<span data-ttu-id="4e141-104">Správce prostředků clusteru Service Fabric obvykle spravuje prostředky clusteru tak, že se zatížení (reprezentované prostřednictvím [metriky](service-fabric-cluster-resource-manager-metrics.md)) rovnoměrně v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e141-104">Service Fabric's Cluster Resource Manager typically manages cluster resources by spreading the load (represented via [Metrics](service-fabric-cluster-resource-manager-metrics.md)) evenly throughout the cluster.</span></span> <span data-ttu-id="4e141-105">Service Fabric spravuje kapacitu uzly v clusteru a clusteru jako celek prostřednictvím [kapacity](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="4e141-105">Service Fabric manages the capacity of the nodes in the cluster and the cluster as a whole via [capacity](service-fabric-cluster-resource-manager-cluster-description.md).</span></span> <span data-ttu-id="4e141-106">Metriky a kapacity pracovní velká pro řadu úloh, ale vzorů, které hodně využívají různé instance aplikace Service Fabric někdy předány další požadavky.</span><span class="sxs-lookup"><span data-stu-id="4e141-106">Metrics and capacity work great for many workloads, but patterns that make heavy use of different Service Fabric Application Instances sometimes bring in additional requirements.</span></span> <span data-ttu-id="4e141-107">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="4e141-107">For example you may want to:</span></span>

- <span data-ttu-id="4e141-108">Záložní některé kapacita v uzlech v clusteru pro služby v rámci některé instance s názvem aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-108">Reserve some capacity on the nodes in the cluster for the services within some named application instance</span></span>
- <span data-ttu-id="4e141-109">Omezit celkový počet uzlů, které služby v rámci instance s názvem aplikace běží na (namísto šíří je po celý cluster)</span><span class="sxs-lookup"><span data-stu-id="4e141-109">Limit the total number of nodes that the services within a named application instance run on (instead of spreading them out over the entire cluster)</span></span>
- <span data-ttu-id="4e141-110">Definování kapacity v samotné omezit počet služeb nebo spotřeby celkový počet prostředků služeb je uvnitř instanci s názvem aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-110">Define capacities on the named application instance itself to limit the number of services or total resource consumption of the services inside it</span></span>

<span data-ttu-id="4e141-111">Pro splnění těchto požadavků, správce prostředků clusteru Service Fabric podporuje funkci skupiny aplikací.</span><span class="sxs-lookup"><span data-stu-id="4e141-111">To meet these requirements, the Service Fabric Cluster Resource Manager supports a feature called Application Groups.</span></span>

## <a name="limiting-the-maximum-number-of-nodes"></a><span data-ttu-id="4e141-112">Omezení maximální počet uzlů</span><span class="sxs-lookup"><span data-stu-id="4e141-112">Limiting the maximum number of nodes</span></span>
<span data-ttu-id="4e141-113">Nejjednodušší případ použití kapacity aplikace je při instanci aplikace musí být omezeno na určité maximální počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="4e141-113">The simplest use case for Application capacity is when an application instance needs to be limited to a certain maximum number of nodes.</span></span> <span data-ttu-id="4e141-114">Tím dojde ke konsolidaci všechny služby v rámci této instance aplikace na se stanoveným počtem počítačů.</span><span class="sxs-lookup"><span data-stu-id="4e141-114">This consolidates all services within that application instance onto a set number of machines.</span></span> <span data-ttu-id="4e141-115">Konsolidace je užitečné, když se pokoušíte předpovědi nebo cap využití fyzických prostředků prostřednictvím služeb v rámci dané aplikace s názvem instance.</span><span class="sxs-lookup"><span data-stu-id="4e141-115">Consolidation is useful when you're trying to predict or cap physical resource use by the services within that named application instance.</span></span> 

<span data-ttu-id="4e141-116">Následující obrázek znázorňuje instance aplikace s i bez maximální počet uzlů, které jsou definované:</span><span class="sxs-lookup"><span data-stu-id="4e141-116">The following image shows an application instance with and without a maximum number of nodes defined:</span></span>

<span data-ttu-id="4e141-117"><center>
![Instance aplikace definování maximální počet uzlů][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="4e141-117"><center>
![Application Instance Defining Maximum Number of Nodes][Image1]
</center></span></span>

<span data-ttu-id="4e141-118">V levém příkladu aplikace nemá maximální počet uzlů, které jsou definované a má tři služby.</span><span class="sxs-lookup"><span data-stu-id="4e141-118">In the left example, the application doesn’t have a maximum number of nodes defined, and it has three services.</span></span> <span data-ttu-id="4e141-119">Správce prostředků clusteru má na všech replik rozloženy šesti dostupných uzlů k dosažení nejlepší vyrovnávání v clusteru (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="4e141-119">The Cluster Resource Manager has spread out all replicas across six available nodes to achieve the best balance in the cluster (the default behavior).</span></span> <span data-ttu-id="4e141-120">V pravém příkladu vidíte stejnou aplikaci omezena na tři uzly.</span><span class="sxs-lookup"><span data-stu-id="4e141-120">In the right example, we see the same application limited to three nodes.</span></span>

<span data-ttu-id="4e141-121">Parametr, který určuje toto chování se nazývá MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="4e141-121">The parameter that controls this behavior is called MaximumNodes.</span></span> <span data-ttu-id="4e141-122">Tento parametr můžete nastavit při vytváření aplikace, nebo aktualizovat pro instanci aplikace, která je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4e141-122">This parameter can be set during application creation, or updated for an application instance that was already running.</span></span>

<span data-ttu-id="4e141-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e141-123">Powershell</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

<span data-ttu-id="4e141-124">C#</span><span class="sxs-lookup"><span data-stu-id="4e141-124">C#</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

<span data-ttu-id="4e141-125">V rámci sada uzlů správce prostředků clusteru není zárukou toho, jaké objekty služby získat umístit společně nebo získat uzlů, které používá.</span><span class="sxs-lookup"><span data-stu-id="4e141-125">Within the set of nodes, the Cluster Resource Manager doesn't guarantee which service objects get placed together or which nodes get used.</span></span>

## <a name="application-metrics-load-and-capacity"></a><span data-ttu-id="4e141-126">Aplikace metriky zatížení a kapacity</span><span class="sxs-lookup"><span data-stu-id="4e141-126">Application Metrics, Load, and Capacity</span></span>
<span data-ttu-id="4e141-127">Skupiny aplikací, které umožňují definovat metriky, které jsou přidružené k dané aplikaci s názvem instance a tuto instanci aplikace kapacity pro tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="4e141-127">Application Groups also allow you to define metrics associated with a given named application instance, and that application instance's capacity for those metrics.</span></span> <span data-ttu-id="4e141-128">Metriky aplikace vám umožňují sledovat, rezervace a omezit spotřeby prostředků služeb v této instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-128">Application metrics allow you to track, reserve, and limit the resource consumption of the services inside that application instance.</span></span>

<span data-ttu-id="4e141-129">Pro jednotlivé aplikace metriky existují dvě hodnoty, které lze nastavit:</span><span class="sxs-lookup"><span data-stu-id="4e141-129">For each application metric, there are two values that can be set:</span></span>

- <span data-ttu-id="4e141-130">**Celková kapacita aplikace** – toto nastavení představuje celkovou kapacitu aplikace pro konkrétní metriky.</span><span class="sxs-lookup"><span data-stu-id="4e141-130">**Total Application Capacity** – This setting represents the total capacity of the application for a particular metric.</span></span> <span data-ttu-id="4e141-131">Správce prostředků clusteru zakáže vytváření všech nových služeb v rámci této instance aplikace, které by způsobily celkové zatížení překročí tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e141-131">The Cluster Resource Manager disallows the creation of any new services within this application instance that would cause total load to exceed this value.</span></span> <span data-ttu-id="4e141-132">Řekněme například, instance aplikace měla kapacitou 10 a již obsahuje pět zatížení.</span><span class="sxs-lookup"><span data-stu-id="4e141-132">For example, let's say the application instance had a capacity of 10 and already had load of five.</span></span> <span data-ttu-id="4e141-133">Vytvoření služby se zatížením celkový výchozí 10 by povoleny.</span><span class="sxs-lookup"><span data-stu-id="4e141-133">The creation of a service with a total default load of 10 would be disallowed.</span></span>
- <span data-ttu-id="4e141-134">**Maximální kapacita uzlu** – toto nastavení určuje maximální celkové zatížení pro danou aplikaci na jednom uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e141-134">**Maximum Node Capacity** – This setting specifies the maximum total load for the application on a single node.</span></span> <span data-ttu-id="4e141-135">Pokud zatížení prochází přes tuto kapacitu, správce prostředků clusteru přesune repliky do dalších uzlů, tak, aby snížení zatížení.</span><span class="sxs-lookup"><span data-stu-id="4e141-135">If load goes over this capacity, the Cluster Resource Manager moves replicas to other nodes so that the load decreases.</span></span>


<span data-ttu-id="4e141-136">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4e141-136">Powershell:</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

<span data-ttu-id="4e141-137">C#:</span><span class="sxs-lookup"><span data-stu-id="4e141-137">C#:</span></span>

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a><span data-ttu-id="4e141-138">Rezervaci kapacity</span><span class="sxs-lookup"><span data-stu-id="4e141-138">Reserving Capacity</span></span>
<span data-ttu-id="4e141-139">Jiné běžně používá pro skupiny aplikací, které je potřeba zajistit, že prostředky v rámci clusteru jsou vyhrazené pro instanci dané aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e141-139">Another common use for application groups is to ensure that resources within the cluster are reserved for a given application instance.</span></span> <span data-ttu-id="4e141-140">Místo je vyhrazená vždy při vytvoření instance aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-140">The space is always reserved when the application instance is created.</span></span>

<span data-ttu-id="4e141-141">Pro aplikace, se stane okamžitě vyhrazením místa v clusteru i v případě:</span><span class="sxs-lookup"><span data-stu-id="4e141-141">Reserving space in the cluster for the application happens immediately even when:</span></span>
- <span data-ttu-id="4e141-142">instance aplikace je vytvořena, ale nemá žádné služby, v něm ještě</span><span class="sxs-lookup"><span data-stu-id="4e141-142">the application instance is created but doesn't have any services within it yet</span></span>
- <span data-ttu-id="4e141-143">počet služeb v rámci instance aplikace na změní pokaždé, když</span><span class="sxs-lookup"><span data-stu-id="4e141-143">the number of services within the application instance changes every time</span></span> 
- <span data-ttu-id="4e141-144">služby existuje, ale nejsou využívání prostředků</span><span class="sxs-lookup"><span data-stu-id="4e141-144">the services exist but aren't consuming the resources</span></span> 

<span data-ttu-id="4e141-145">Rezervování prostředků pro instanci aplikace vyžaduje zadání další dva parametry: *MinimumNodes* a *NodeReservationCapacity*</span><span class="sxs-lookup"><span data-stu-id="4e141-145">Reserving resources for an application instance requires specifying two additional parameters: *MinimumNodes* and *NodeReservationCapacity*</span></span>

- <span data-ttu-id="4e141-146">**MinimumNodes** -Určuje minimální počet uzlů, které by neměl být spuštěný instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-146">**MinimumNodes** - Defines the minimum number of nodes that the application instance should run on.</span></span>  
- <span data-ttu-id="4e141-147">**NodeReservationCapacity** – toto nastavení je za Metrika pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e141-147">**NodeReservationCapacity** - This setting is per metric for the application.</span></span> <span data-ttu-id="4e141-148">Hodnota je velikost této metrika vyhrazené pro aplikace v každém uzlu kde, spouštění služeb v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e141-148">The value is the amount of that metric reserved for the application on any node where that the services in that application run.</span></span>

<span data-ttu-id="4e141-149">Kombinování **MinimumNodes** a **NodeReservationCapacity** zaručuje rezervace minimální zatížení pro danou aplikaci v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e141-149">Combining **MinimumNodes** and **NodeReservationCapacity** guarantees a minimum load reservation for the application within the cluster.</span></span> <span data-ttu-id="4e141-150">Pokud existuje méně zbývající kapacity v clusteru než celkový rezervace vyžaduje, vytvoření aplikace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4e141-150">If there's less remaining capacity in the cluster than the total reservation required, creation of the application fails.</span></span> 

<span data-ttu-id="4e141-151">Podívejme se na příklad rezervaci kapacity:</span><span class="sxs-lookup"><span data-stu-id="4e141-151">Let's look at an example of capacity reservation:</span></span>

<span data-ttu-id="4e141-152"><center>
![Instance aplikace definování rezervované kapacity][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="4e141-152"><center>
![Application Instances Defining Reserved Capacity][Image2]
</center></span></span>

<span data-ttu-id="4e141-153">V levém příkladu aplikace nemají žádné aplikace kapacity definované.</span><span class="sxs-lookup"><span data-stu-id="4e141-153">In the left example, applications do not have any Application Capacity defined.</span></span> <span data-ttu-id="4e141-154">Správce prostředků clusteru vyrovnává všechno podle běžných pravidel.</span><span class="sxs-lookup"><span data-stu-id="4e141-154">The Cluster Resource Manager balances everything according to normal rules.</span></span>

<span data-ttu-id="4e141-155">V příkladu na pravé straně Řekněme, že Application1 byl vytvořen s následujícími nastaveními:</span><span class="sxs-lookup"><span data-stu-id="4e141-155">In the example on the right, let's say that Application1 was created with the following settings:</span></span>

- <span data-ttu-id="4e141-156">MinimumNodes nastavena na dvě</span><span class="sxs-lookup"><span data-stu-id="4e141-156">MinimumNodes set to two</span></span>
- <span data-ttu-id="4e141-157">Metrika definovaná pomocí aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-157">An application Metric defined with</span></span>
  - <span data-ttu-id="4e141-158">NodeReservationCapacity 20</span><span class="sxs-lookup"><span data-stu-id="4e141-158">NodeReservationCapacity of 20</span></span>

<span data-ttu-id="4e141-159">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4e141-159">Powershell</span></span>

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

<span data-ttu-id="4e141-160">C#</span><span class="sxs-lookup"><span data-stu-id="4e141-160">C#</span></span>

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

<span data-ttu-id="4e141-161">Service Fabric rezervuje kapacitu na dva uzly pro Application1 a neumožňuje služby z Application2 využívat tuto kapacitu, i když nejsou že žádné zatížení spotřebovává služby uvnitř Application1 v době.</span><span class="sxs-lookup"><span data-stu-id="4e141-161">Service Fabric reserves capacity on two nodes for Application1, and doesn't allow services from Application2 to consume that capacity even if there are no load is being consumed by the services inside Application1 at the time.</span></span> <span data-ttu-id="4e141-162">Tato vyhrazená aplikace kapacita považuje za spotřebované a započítává zbývající kapacity v tomto uzlu a v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e141-162">This reserved application capacity is considered consumed  and counts against the remaining capacity on that node and within the cluster.</span></span>  <span data-ttu-id="4e141-163">Rezervace bude odečtena z zbývající kapacity clusteru okamžitě, ale odečtením vyhrazené spotřeby z kapacity konkrétním uzlu jenom v případě, že je alespoň jedna služba objekt je umístěn na něm.</span><span class="sxs-lookup"><span data-stu-id="4e141-163">The reservation is deducted from the remaining cluster capacity immediately, however the reserved consumption is deducted from the capacity of a specific node only when at least one service object is placed on it.</span></span> <span data-ttu-id="4e141-164">Tento novější rezervace umožňuje flexibilitu a lepší využití prostředků vzhledem k tomu, že prostředky jsou vyhrazeny pouze na uzlech v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4e141-164">This later reservation allows for flexibility and better resource utilization since resources are only reserved on nodes when needed.</span></span>

## <a name="obtaining-the-application-load-information"></a><span data-ttu-id="4e141-165">Získávání informací o zatížení aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-165">Obtaining the application load information</span></span>
<span data-ttu-id="4e141-166">Pro každou aplikaci, která má kapacitu aplikace definovaná pro jeden nebo více metriky, které můžete získat informace o agregační zatížení hlášené repliky jeho služby.</span><span class="sxs-lookup"><span data-stu-id="4e141-166">For each application that has an Application Capacity defined for one or more metrics you can obtain the information about the aggregate load reported by replicas of its services.</span></span>

<span data-ttu-id="4e141-167">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4e141-167">Powershell:</span></span>

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

<span data-ttu-id="4e141-168">C#</span><span class="sxs-lookup"><span data-stu-id="4e141-168">C#</span></span>

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

<span data-ttu-id="4e141-169">ApplicationLoad dotaz vrátí základní informace o kapacitě aplikace, která byla zadaná pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e141-169">The ApplicationLoad query returns the basic information about Application Capacity that was specified for the application.</span></span> <span data-ttu-id="4e141-170">Tyto informace zahrnují informace, které uzly minimální a maximální počet uzlů a číslo, který je aktuálně zabírá aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-170">This information includes the Minimum Nodes and Maximum Nodes info, and the number that the application is currently occupying.</span></span> <span data-ttu-id="4e141-171">Zahrnuje taky informace o jednotlivých metrika zatížení aplikací, včetně:</span><span class="sxs-lookup"><span data-stu-id="4e141-171">It also includes information about each application load metric, including:</span></span>

* <span data-ttu-id="4e141-172">Název metriky: Název metriky.</span><span class="sxs-lookup"><span data-stu-id="4e141-172">Metric Name: Name of the metric.</span></span>
* <span data-ttu-id="4e141-173">Rezervaci kapacity: Kapacity clusteru, která je vyhrazena v clusteru pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e141-173">Reservation Capacity: Cluster Capacity that is reserved in the cluster for this Application.</span></span>
* <span data-ttu-id="4e141-174">Zatížení aplikací: Celkový počet zatížení replik podřízené této aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-174">Application Load: Total Load of this Application’s child replicas.</span></span>
* <span data-ttu-id="4e141-175">Aplikace kapacity: Maximální povolená hodnota zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e141-175">Application Capacity: Maximum permitted value of Application Load.</span></span>

## <a name="removing-application-capacity"></a><span data-ttu-id="4e141-176">Odebrání aplikace kapacity</span><span class="sxs-lookup"><span data-stu-id="4e141-176">Removing Application Capacity</span></span>
<span data-ttu-id="4e141-177">Jakmile aplikace kapacity parametry jsou nastavené pro aplikace, budou se dá odebrat pomocí rozhraní API pro aktualizaci aplikace nebo rutiny Powershellu.</span><span class="sxs-lookup"><span data-stu-id="4e141-177">Once the Application Capacity parameters are set for an application, they can be removed using Update Application APIs or PowerShell cmdlets.</span></span> <span data-ttu-id="4e141-178">Například:</span><span class="sxs-lookup"><span data-stu-id="4e141-178">For example:</span></span>

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

<span data-ttu-id="4e141-179">Tento příkaz odebere z instance aplikace na všechny aplikace kapacity správy parametry.</span><span class="sxs-lookup"><span data-stu-id="4e141-179">This command removes all Application capacity management parameters from the application instance.</span></span> <span data-ttu-id="4e141-180">To zahrnuje MinimumNodes, MaximumNodes a metriky aplikace, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="4e141-180">This includes MinimumNodes, MaximumNodes, and the Application's metrics, if any.</span></span> <span data-ttu-id="4e141-181">Efekt příkazu se okamžitě.</span><span class="sxs-lookup"><span data-stu-id="4e141-181">The effect of the command is immediate.</span></span> <span data-ttu-id="4e141-182">Po dokončení tohoto příkazu, správce prostředků clusteru použije výchozí chování pro správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="4e141-182">After this command completes, the Cluster Resource Manager uses the default behavior for managing applications.</span></span> <span data-ttu-id="4e141-183">Parametry kapacity aplikace mohou být zadané znovu prostřednictvím `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span><span class="sxs-lookup"><span data-stu-id="4e141-183">Application Capacity parameters can be specified again via `Update-ServiceFabricApplication`/`System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span></span>

### <a name="restrictions-on-application-capacity"></a><span data-ttu-id="4e141-184">Omezení kapacity aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-184">Restrictions on Application Capacity</span></span>
<span data-ttu-id="4e141-185">Existuje několik omezení kapacity aplikace parametry, které je nutné dodržovat.</span><span class="sxs-lookup"><span data-stu-id="4e141-185">There are several restrictions on Application Capacity parameters that must be respected.</span></span> <span data-ttu-id="4e141-186">Pokud nejsou chyby ověření žádné změny proběhnout.</span><span class="sxs-lookup"><span data-stu-id="4e141-186">If there are validation errors no changes take place.</span></span>

- <span data-ttu-id="4e141-187">Všechny parametry celé číslo musí být nezáporné číslo.</span><span class="sxs-lookup"><span data-stu-id="4e141-187">All integer parameters must be non-negative numbers.</span></span>
- <span data-ttu-id="4e141-188">MinimumNodes, nikdy musí být větší než MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="4e141-188">MinimumNodes must never be greater than MaximumNodes.</span></span>
- <span data-ttu-id="4e141-189">Pokud jsou definovány kapacity pro metrika zatížení, musí se postupujte tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="4e141-189">If capacities for a load metric are defined, then they must follow these rules:</span></span>
  - <span data-ttu-id="4e141-190">Uzel rezervaci kapacity nesmí být větší než maximální kapacita uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e141-190">Node Reservation Capacity must not be greater than Maximum Node Capacity.</span></span> <span data-ttu-id="4e141-191">Nelze například omezení kapacity pro metriku "Procesoru" v uzlu a dvě jednotky a akci tak, aby vyhradil tři jednotky na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e141-191">For example, you cannot limit the capacity for the metric “CPU” on the node to two units and try to reserve three units on each node.</span></span>
  - <span data-ttu-id="4e141-192">Pokud je zadán MaximumNodes, nesmí být větší než celková kapacita aplikace produktu MaximumNodes a maximální kapacita uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e141-192">If MaximumNodes is specified, then the product of MaximumNodes and Maximum Node Capacity must not be greater than Total Application Capacity.</span></span> <span data-ttu-id="4e141-193">Předpokládejme například že maximální kapacita uzlu pro metrika zatížení, "Procesor" je nastavena na 8.</span><span class="sxs-lookup"><span data-stu-id="4e141-193">For example, let's say the Maximum Node Capacity for load metric “CPU” is set to eight.</span></span> <span data-ttu-id="4e141-194">Také se stát, že nastavíte maximální počet uzlů na 10.</span><span class="sxs-lookup"><span data-stu-id="4e141-194">Let's also say you set the Maximum Nodes to 10.</span></span> <span data-ttu-id="4e141-195">V takovém případě celková kapacita aplikace musí být větší než 80 pro tato metrika zatížení.</span><span class="sxs-lookup"><span data-stu-id="4e141-195">In this case, Total Application Capacity must be greater than 80 for this load metric.</span></span>

<span data-ttu-id="4e141-196">Omezení se vynucují, jak při vytváření aplikací a aktualizací.</span><span class="sxs-lookup"><span data-stu-id="4e141-196">The restrictions are enforced both during application creation and updates.</span></span>

## <a name="how-not-to-use-application-capacity"></a><span data-ttu-id="4e141-197">Jak nepoužívat kapacity aplikace</span><span class="sxs-lookup"><span data-stu-id="4e141-197">How not to use Application Capacity</span></span>
- <span data-ttu-id="4e141-198">Nepokoušejte se použít funkce skupiny aplikací k omezení aplikace _konkrétní_ dílčí sadu uzlů.</span><span class="sxs-lookup"><span data-stu-id="4e141-198">Do not try to use the Application Group features to constrain the application to a _specific_ subset of nodes.</span></span> <span data-ttu-id="4e141-199">Jinými slovy, můžete určit, že je aplikace spuštěná na nejvíce pět uzlů, ale není konkrétní pět uzlů, které v clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e141-199">In other words, you can specify that the application runs on at most five nodes, but not which specific five nodes in the cluster.</span></span> <span data-ttu-id="4e141-200">Chovaly aplikaci ke konkrétní uzly lze dosáhnout pomocí omezení umístění pro služby.</span><span class="sxs-lookup"><span data-stu-id="4e141-200">Constraining an application to specific nodes can be achieved using placement constraints for services.</span></span>
- <span data-ttu-id="4e141-201">Nepokoušejte se použít aplikaci kapacitu k zajištění, že dvě služby z stejná aplikace budou umístěny v stejným uzlům.</span><span class="sxs-lookup"><span data-stu-id="4e141-201">Do not try to use the Application Capacity to ensure that two services from the same application are placed on the same nodes.</span></span> <span data-ttu-id="4e141-202">Místo toho použít [spřažení](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) nebo [omezení umístění](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span><span class="sxs-lookup"><span data-stu-id="4e141-202">Instead use [affinity](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) or [placement constraints](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e141-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e141-203">Next steps</span></span>
- <span data-ttu-id="4e141-204">Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="4e141-204">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="4e141-205">Chcete-li zjistit, o tom, jak správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru, podívejte se na článek na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="4e141-205">To find out about how the Cluster Resource Manager manages and balances load in the cluster, check out the article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="4e141-206">Začít od začátku a [získejte Úvod do Service Fabric clusteru správce prostředků](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="4e141-206">Start from the beginning and [get an Introduction to the Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="4e141-207">Další informace o fungování metriky obecně, přečtěte si [metriky zatížení Service Fabric](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="4e141-207">For more information on how metrics work generally, read up on [Service Fabric Load Metrics](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="4e141-208">Správce prostředků clusteru má mnoho možností pro popis clusteru.</span><span class="sxs-lookup"><span data-stu-id="4e141-208">The Cluster Resource Manager has many options for describing the cluster.</span></span> <span data-ttu-id="4e141-209">Další informace o nich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="4e141-209">To find out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
