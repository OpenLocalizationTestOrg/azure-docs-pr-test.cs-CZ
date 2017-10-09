---
title: "aaaService správce prostředků infrastruktury Cluster - skupin aplikací | Microsoft Docs"
description: "Přehled hello funkce skupiny aplikací v hello správce prostředků clusteru Service Fabric"
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
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a><span data-ttu-id="6efc0-103">Úvod tooApplication skupiny</span><span class="sxs-lookup"><span data-stu-id="6efc0-103">Introduction tooApplication Groups</span></span>
<span data-ttu-id="6efc0-104">Správce prostředků clusteru Service Fabric obvykle spravuje prostředky clusteru tak, že se zatížení hello (reprezentované prostřednictvím [metriky](service-fabric-cluster-resource-manager-metrics.md)) rovnoměrně v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-104">Service Fabric's Cluster Resource Manager typically manages cluster resources by spreading hello load (represented via [Metrics](service-fabric-cluster-resource-manager-metrics.md)) evenly throughout hello cluster.</span></span> <span data-ttu-id="6efc0-105">Service Fabric spravuje kapacitu hello hello uzlů v clusteru hello a hello clusteru jako celek prostřednictvím [kapacity](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="6efc0-105">Service Fabric manages hello capacity of hello nodes in hello cluster and hello cluster as a whole via [capacity](service-fabric-cluster-resource-manager-cluster-description.md).</span></span> <span data-ttu-id="6efc0-106">Metriky a kapacity pracovní velká pro řadu úloh, ale vzorů, které hodně využívají různé instance aplikace Service Fabric někdy předány další požadavky.</span><span class="sxs-lookup"><span data-stu-id="6efc0-106">Metrics and capacity work great for many workloads, but patterns that make heavy use of different Service Fabric Application Instances sometimes bring in additional requirements.</span></span> <span data-ttu-id="6efc0-107">Například můžete chtít:</span><span class="sxs-lookup"><span data-stu-id="6efc0-107">For example you may want to:</span></span>

- <span data-ttu-id="6efc0-108">Záložní některé kapacita hello uzlů v clusteru hello hello služeb v rámci některé instance s názvem aplikace</span><span class="sxs-lookup"><span data-stu-id="6efc0-108">Reserve some capacity on hello nodes in hello cluster for hello services within some named application instance</span></span>
- <span data-ttu-id="6efc0-109">Omezit hello celkový počet uzlů, které hello služeb v rámci instance s názvem aplikace běží na (namísto šíří je po celý cluster hello)</span><span class="sxs-lookup"><span data-stu-id="6efc0-109">Limit hello total number of nodes that hello services within a named application instance run on (instead of spreading them out over hello entire cluster)</span></span>
- <span data-ttu-id="6efc0-110">Definovat kapacity na samotnou instanci s názvem aplikace hello toolimit hello počet služeb nebo celková spotřeba prostředků hello služeb je uvnitř</span><span class="sxs-lookup"><span data-stu-id="6efc0-110">Define capacities on hello named application instance itself toolimit hello number of services or total resource consumption of hello services inside it</span></span>

<span data-ttu-id="6efc0-111">toomeet tyto požadavky hello správce prostředků clusteru Service Fabric podporuje funkci skupiny aplikací.</span><span class="sxs-lookup"><span data-stu-id="6efc0-111">toomeet these requirements, hello Service Fabric Cluster Resource Manager supports a feature called Application Groups.</span></span>

## <a name="limiting-hello-maximum-number-of-nodes"></a><span data-ttu-id="6efc0-112">Omezení hello maximální počet uzlů</span><span class="sxs-lookup"><span data-stu-id="6efc0-112">Limiting hello maximum number of nodes</span></span>
<span data-ttu-id="6efc0-113">Hello nejjednodušší případ použití kapacity aplikace je když instance aplikace potřebuje toobe omezené tooa určité maximální počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="6efc0-113">hello simplest use case for Application capacity is when an application instance needs toobe limited tooa certain maximum number of nodes.</span></span> <span data-ttu-id="6efc0-114">Tím dojde ke konsolidaci všechny služby v rámci této instance aplikace na se stanoveným počtem počítačů.</span><span class="sxs-lookup"><span data-stu-id="6efc0-114">This consolidates all services within that application instance onto a set number of machines.</span></span> <span data-ttu-id="6efc0-115">Konsolidace je užitečné, když se snažíte toopredict nebo cap využití fyzických prostředků hello služby v rámci dané aplikace s názvem instance.</span><span class="sxs-lookup"><span data-stu-id="6efc0-115">Consolidation is useful when you're trying toopredict or cap physical resource use by hello services within that named application instance.</span></span> 

<span data-ttu-id="6efc0-116">Hello následující obrázek ukazuje instanci aplikace a bez maximální počet uzlů, které jsou definované:</span><span class="sxs-lookup"><span data-stu-id="6efc0-116">hello following image shows an application instance with and without a maximum number of nodes defined:</span></span>

<span data-ttu-id="6efc0-117"><center>
![Instance aplikace definování maximální počet uzlů][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="6efc0-117"><center>
![Application Instance Defining Maximum Number of Nodes][Image1]
</center></span></span>

<span data-ttu-id="6efc0-118">V levém příkladu hello hello aplikace nemá maximální počet uzlů, které jsou definované a má tři služby.</span><span class="sxs-lookup"><span data-stu-id="6efc0-118">In hello left example, hello application doesn’t have a maximum number of nodes defined, and it has three services.</span></span> <span data-ttu-id="6efc0-119">Hello správce prostředků clusteru má na všech replik rozloženy šesti dostupných uzlů tooachieve hello vyvážit v clusteru hello (hello výchozí chování).</span><span class="sxs-lookup"><span data-stu-id="6efc0-119">hello Cluster Resource Manager has spread out all replicas across six available nodes tooachieve hello best balance in hello cluster (hello default behavior).</span></span> <span data-ttu-id="6efc0-120">V pravém příkladu hello vidíme hello stejnou aplikaci omezený počet uzlů toothree.</span><span class="sxs-lookup"><span data-stu-id="6efc0-120">In hello right example, we see hello same application limited toothree nodes.</span></span>

<span data-ttu-id="6efc0-121">Hello parametr, který určuje toto chování se nazývá MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="6efc0-121">hello parameter that controls this behavior is called MaximumNodes.</span></span> <span data-ttu-id="6efc0-122">Tento parametr můžete nastavit při vytváření aplikace, nebo aktualizovat pro instanci aplikace, která je již spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6efc0-122">This parameter can be set during application creation, or updated for an application instance that was already running.</span></span>

<span data-ttu-id="6efc0-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6efc0-123">Powershell</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

<span data-ttu-id="6efc0-124">C#</span><span class="sxs-lookup"><span data-stu-id="6efc0-124">C#</span></span>

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

<span data-ttu-id="6efc0-125">V rámci hello sada uzlů hello správce prostředků clusteru není zárukou toho, jaké objekty služby získat umístit společně nebo získat uzlů, které používá.</span><span class="sxs-lookup"><span data-stu-id="6efc0-125">Within hello set of nodes, hello Cluster Resource Manager doesn't guarantee which service objects get placed together or which nodes get used.</span></span>

## <a name="application-metrics-load-and-capacity"></a><span data-ttu-id="6efc0-126">Aplikace metriky zatížení a kapacity</span><span class="sxs-lookup"><span data-stu-id="6efc0-126">Application Metrics, Load, and Capacity</span></span>
<span data-ttu-id="6efc0-127">Skupiny aplikací také umožní toodefine metriky přidružené k dané aplikaci s názvem instance a tuto instanci aplikace kapacity pro tyto metriky.</span><span class="sxs-lookup"><span data-stu-id="6efc0-127">Application Groups also allow you toodefine metrics associated with a given named application instance, and that application instance's capacity for those metrics.</span></span> <span data-ttu-id="6efc0-128">Metriky aplikace povolí tootrack rezervy a omezování spotřeby prostředků hello hello služeb v této instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6efc0-128">Application metrics allow you tootrack, reserve, and limit hello resource consumption of hello services inside that application instance.</span></span>

<span data-ttu-id="6efc0-129">Pro jednotlivé aplikace metriky existují dvě hodnoty, které lze nastavit:</span><span class="sxs-lookup"><span data-stu-id="6efc0-129">For each application metric, there are two values that can be set:</span></span>

- <span data-ttu-id="6efc0-130">**Celková kapacita aplikace** – toto nastavení představuje celkové kapacity hello hello aplikace pro konkrétní metriky.</span><span class="sxs-lookup"><span data-stu-id="6efc0-130">**Total Application Capacity** – This setting represents hello total capacity of hello application for a particular metric.</span></span> <span data-ttu-id="6efc0-131">Hello správce prostředků clusteru zakáže hello vytváření všech nových služeb v rámci této instance aplikace, které by způsobily celkové zatížení tooexceed tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-131">hello Cluster Resource Manager disallows hello creation of any new services within this application instance that would cause total load tooexceed this value.</span></span> <span data-ttu-id="6efc0-132">Řekněme například, instanci aplikace hello měl kapacitou 10 a již obsahuje pět zatížení.</span><span class="sxs-lookup"><span data-stu-id="6efc0-132">For example, let's say hello application instance had a capacity of 10 and already had load of five.</span></span> <span data-ttu-id="6efc0-133">Vytvoření Hello služby se zatížením celkový výchozí 10 by povoleny.</span><span class="sxs-lookup"><span data-stu-id="6efc0-133">hello creation of a service with a total default load of 10 would be disallowed.</span></span>
- <span data-ttu-id="6efc0-134">**Maximální kapacita uzlu** – toto nastavení určuje maximální celkové zatížení hello hello aplikace v jednom uzlu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-134">**Maximum Node Capacity** – This setting specifies hello maximum total load for hello application on a single node.</span></span> <span data-ttu-id="6efc0-135">Pokud zatížení prochází přes tuto kapacitu, hello správce prostředků clusteru přesune uzly tooother repliky tak, aby snížení technologie hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="6efc0-135">If load goes over this capacity, hello Cluster Resource Manager moves replicas tooother nodes so that hello load decreases.</span></span>


<span data-ttu-id="6efc0-136">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6efc0-136">Powershell:</span></span>

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

<span data-ttu-id="6efc0-137">C#:</span><span class="sxs-lookup"><span data-stu-id="6efc0-137">C#:</span></span>

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

## <a name="reserving-capacity"></a><span data-ttu-id="6efc0-138">Rezervaci kapacity</span><span class="sxs-lookup"><span data-stu-id="6efc0-138">Reserving Capacity</span></span>
<span data-ttu-id="6efc0-139">Jiné běžně používá pro skupin aplikací je tooensure že hello prostředků v rámci clusteru jsou vyhrazené pro instanci dané aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6efc0-139">Another common use for application groups is tooensure that resources within hello cluster are reserved for a given application instance.</span></span> <span data-ttu-id="6efc0-140">místo Hello je vyhrazena vždy při vytvoření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-140">hello space is always reserved when hello application instance is created.</span></span>

<span data-ttu-id="6efc0-141">Pro aplikace hello se stane okamžitě vyhrazením místa v clusteru hello i v případě:</span><span class="sxs-lookup"><span data-stu-id="6efc0-141">Reserving space in hello cluster for hello application happens immediately even when:</span></span>
- <span data-ttu-id="6efc0-142">instance aplikace Hello je vytvořen, ale nemá žádné služby, v něm ještě</span><span class="sxs-lookup"><span data-stu-id="6efc0-142">hello application instance is created but doesn't have any services within it yet</span></span>
- <span data-ttu-id="6efc0-143">pokaždé, když se změní Hello počet služeb v rámci instance aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6efc0-143">hello number of services within hello application instance changes every time</span></span> 
- <span data-ttu-id="6efc0-144">Hello služby existuje, ale nejsou spotřebovávat hello prostředků</span><span class="sxs-lookup"><span data-stu-id="6efc0-144">hello services exist but aren't consuming hello resources</span></span> 

<span data-ttu-id="6efc0-145">Rezervování prostředků pro instanci aplikace vyžaduje zadání další dva parametry: *MinimumNodes* a *NodeReservationCapacity*</span><span class="sxs-lookup"><span data-stu-id="6efc0-145">Reserving resources for an application instance requires specifying two additional parameters: *MinimumNodes* and *NodeReservationCapacity*</span></span>

- <span data-ttu-id="6efc0-146">**MinimumNodes** -definuje hello minimální počet uzlů, které aplikace hello by neměl být spuštěný instance.</span><span class="sxs-lookup"><span data-stu-id="6efc0-146">**MinimumNodes** - Defines hello minimum number of nodes that hello application instance should run on.</span></span>  
- <span data-ttu-id="6efc0-147">**NodeReservationCapacity** – toto nastavení je za Metrika pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-147">**NodeReservationCapacity** - This setting is per metric for hello application.</span></span> <span data-ttu-id="6efc0-148">Hodnota Hello je hello množství tuto metriku vyhrazené pro aplikace hello v každém uzlu, kde to hello služby v této aplikaci spusťte.</span><span class="sxs-lookup"><span data-stu-id="6efc0-148">hello value is hello amount of that metric reserved for hello application on any node where that hello services in that application run.</span></span>

<span data-ttu-id="6efc0-149">Kombinování **MinimumNodes** a **NodeReservationCapacity** zaručuje rezervace minimální zatížení pro hello aplikací v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-149">Combining **MinimumNodes** and **NodeReservationCapacity** guarantees a minimum load reservation for hello application within hello cluster.</span></span> <span data-ttu-id="6efc0-150">Pokud je menší zbývající kapacity hello clusteru než hello celkový rezervace je povinná, vytvoření aplikace hello nezdaří.</span><span class="sxs-lookup"><span data-stu-id="6efc0-150">If there's less remaining capacity in hello cluster than hello total reservation required, creation of hello application fails.</span></span> 

<span data-ttu-id="6efc0-151">Podívejme se na příklad rezervaci kapacity:</span><span class="sxs-lookup"><span data-stu-id="6efc0-151">Let's look at an example of capacity reservation:</span></span>

<span data-ttu-id="6efc0-152"><center>
![Instance aplikace definování rezervované kapacity][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="6efc0-152"><center>
![Application Instances Defining Reserved Capacity][Image2]
</center></span></span>

<span data-ttu-id="6efc0-153">V levém příkladu hello aplikace nemají žádné aplikace kapacity definované.</span><span class="sxs-lookup"><span data-stu-id="6efc0-153">In hello left example, applications do not have any Application Capacity defined.</span></span> <span data-ttu-id="6efc0-154">Hello správce prostředků clusteru vyrovnává všechno podle toonormal pravidla.</span><span class="sxs-lookup"><span data-stu-id="6efc0-154">hello Cluster Resource Manager balances everything according toonormal rules.</span></span>

<span data-ttu-id="6efc0-155">V příkladu hello na pravém hello Řekněme, že Application1 byl vytvořen s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="6efc0-155">In hello example on hello right, let's say that Application1 was created with hello following settings:</span></span>

- <span data-ttu-id="6efc0-156">MinimumNodes nastavit tootwo</span><span class="sxs-lookup"><span data-stu-id="6efc0-156">MinimumNodes set tootwo</span></span>
- <span data-ttu-id="6efc0-157">Metrika definovaná pomocí aplikace</span><span class="sxs-lookup"><span data-stu-id="6efc0-157">An application Metric defined with</span></span>
  - <span data-ttu-id="6efc0-158">NodeReservationCapacity 20</span><span class="sxs-lookup"><span data-stu-id="6efc0-158">NodeReservationCapacity of 20</span></span>

<span data-ttu-id="6efc0-159">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6efc0-159">Powershell</span></span>

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

<span data-ttu-id="6efc0-160">C#</span><span class="sxs-lookup"><span data-stu-id="6efc0-160">C#</span></span>

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

<span data-ttu-id="6efc0-161">Service Fabric rezervuje kapacitu na dva uzly pro Application1 a neumožňuje služby od Application2 tooconsume tuto kapacitu i v případě, že nejsou že žádné zatížení je spotřebovávanou službami hello uvnitř Application1 během hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-161">Service Fabric reserves capacity on two nodes for Application1, and doesn't allow services from Application2 tooconsume that capacity even if there are no load is being consumed by hello services inside Application1 at hello time.</span></span> <span data-ttu-id="6efc0-162">Tato vyhrazená aplikace kapacita považuje za spotřebované a započítává hello zbývající kapacity v tomto uzlu a v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-162">This reserved application capacity is considered consumed  and counts against hello remaining capacity on that node and within hello cluster.</span></span>  <span data-ttu-id="6efc0-163">Hello rezervace je odečtena od hello zbývající kapacita clusteru okamžitě, ale hello vyhrazené spotřeba je odečtena od hello kapacitu konkrétním uzlu jenom v případě, že je alespoň jedna služba objekt je umístěn na něm.</span><span class="sxs-lookup"><span data-stu-id="6efc0-163">hello reservation is deducted from hello remaining cluster capacity immediately, however hello reserved consumption is deducted from hello capacity of a specific node only when at least one service object is placed on it.</span></span> <span data-ttu-id="6efc0-164">Tento novější rezervace umožňuje flexibilitu a lepší využití prostředků vzhledem k tomu, že prostředky jsou vyhrazeny pouze na uzlech v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6efc0-164">This later reservation allows for flexibility and better resource utilization since resources are only reserved on nodes when needed.</span></span>

## <a name="obtaining-hello-application-load-information"></a><span data-ttu-id="6efc0-165">Získávání informací o zatížení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6efc0-165">Obtaining hello application load information</span></span>
<span data-ttu-id="6efc0-166">Pro každou aplikaci, která má kapacitu aplikace definovaná pro jeden nebo více metriky můžete získat hello informace o hello agregační zatížení hlášené repliky jeho služby.</span><span class="sxs-lookup"><span data-stu-id="6efc0-166">For each application that has an Application Capacity defined for one or more metrics you can obtain hello information about hello aggregate load reported by replicas of its services.</span></span>

<span data-ttu-id="6efc0-167">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6efc0-167">Powershell:</span></span>

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

<span data-ttu-id="6efc0-168">C#</span><span class="sxs-lookup"><span data-stu-id="6efc0-168">C#</span></span>

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

<span data-ttu-id="6efc0-169">Hello ApplicationLoad dotaz vrátí hello základní informace o kapacitě aplikace, která byla zadaná pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-169">hello ApplicationLoad query returns hello basic information about Application Capacity that was specified for hello application.</span></span> <span data-ttu-id="6efc0-170">Tyto informace zahrnují informace o hello uzly minimální a maximální počet uzlů a hello číslo, které je aktuálně zabírá aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-170">This information includes hello Minimum Nodes and Maximum Nodes info, and hello number that hello application is currently occupying.</span></span> <span data-ttu-id="6efc0-171">Zahrnuje taky informace o jednotlivých metrika zatížení aplikací, včetně:</span><span class="sxs-lookup"><span data-stu-id="6efc0-171">It also includes information about each application load metric, including:</span></span>

* <span data-ttu-id="6efc0-172">Název metriky: Název metriky hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-172">Metric Name: Name of hello metric.</span></span>
* <span data-ttu-id="6efc0-173">Rezervaci kapacity: Clusteru rezervace kapacity v hello clusteru pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6efc0-173">Reservation Capacity: Cluster Capacity that is reserved in hello cluster for this Application.</span></span>
* <span data-ttu-id="6efc0-174">Zatížení aplikací: Celkový počet zatížení replik podřízené této aplikace.</span><span class="sxs-lookup"><span data-stu-id="6efc0-174">Application Load: Total Load of this Application’s child replicas.</span></span>
* <span data-ttu-id="6efc0-175">Aplikace kapacity: Maximální povolená hodnota zatížení aplikace.</span><span class="sxs-lookup"><span data-stu-id="6efc0-175">Application Capacity: Maximum permitted value of Application Load.</span></span>

## <a name="removing-application-capacity"></a><span data-ttu-id="6efc0-176">Odebrání aplikace kapacity</span><span class="sxs-lookup"><span data-stu-id="6efc0-176">Removing Application Capacity</span></span>
<span data-ttu-id="6efc0-177">Jakmile hello kapacity aplikace parametry jsou nastavené pro aplikace, budou se dá odebrat pomocí rozhraní API pro aktualizaci aplikace nebo rutiny Powershellu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-177">Once hello Application Capacity parameters are set for an application, they can be removed using Update Application APIs or PowerShell cmdlets.</span></span> <span data-ttu-id="6efc0-178">Například:</span><span class="sxs-lookup"><span data-stu-id="6efc0-178">For example:</span></span>

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

<span data-ttu-id="6efc0-179">Tento příkaz odebere všechny parametry kapacity správy aplikací z instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-179">This command removes all Application capacity management parameters from hello application instance.</span></span> <span data-ttu-id="6efc0-180">To zahrnuje MinimumNodes, MaximumNodes a metriky aplikace hello, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="6efc0-180">This includes MinimumNodes, MaximumNodes, and hello Application's metrics, if any.</span></span> <span data-ttu-id="6efc0-181">účinek Hello hello příkazu se okamžitě.</span><span class="sxs-lookup"><span data-stu-id="6efc0-181">hello effect of hello command is immediate.</span></span> <span data-ttu-id="6efc0-182">Po dokončení tohoto příkazu hello správce prostředků clusteru používá hello výchozí chování pro správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="6efc0-182">After this command completes, hello Cluster Resource Manager uses hello default behavior for managing applications.</span></span> <span data-ttu-id="6efc0-183">Parametry kapacity aplikace mohou být zadané znovu prostřednictvím `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span><span class="sxs-lookup"><span data-stu-id="6efc0-183">Application Capacity parameters can be specified again via `Update-ServiceFabricApplication`/`System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.</span></span>

### <a name="restrictions-on-application-capacity"></a><span data-ttu-id="6efc0-184">Omezení kapacity aplikace</span><span class="sxs-lookup"><span data-stu-id="6efc0-184">Restrictions on Application Capacity</span></span>
<span data-ttu-id="6efc0-185">Existuje několik omezení kapacity aplikace parametry, které je nutné dodržovat.</span><span class="sxs-lookup"><span data-stu-id="6efc0-185">There are several restrictions on Application Capacity parameters that must be respected.</span></span> <span data-ttu-id="6efc0-186">Pokud nejsou chyby ověření žádné změny proběhnout.</span><span class="sxs-lookup"><span data-stu-id="6efc0-186">If there are validation errors no changes take place.</span></span>

- <span data-ttu-id="6efc0-187">Všechny parametry celé číslo musí být nezáporné číslo.</span><span class="sxs-lookup"><span data-stu-id="6efc0-187">All integer parameters must be non-negative numbers.</span></span>
- <span data-ttu-id="6efc0-188">MinimumNodes, nikdy musí být větší než MaximumNodes.</span><span class="sxs-lookup"><span data-stu-id="6efc0-188">MinimumNodes must never be greater than MaximumNodes.</span></span>
- <span data-ttu-id="6efc0-189">Pokud jsou definovány kapacity pro metrika zatížení, musí se postupujte tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="6efc0-189">If capacities for a load metric are defined, then they must follow these rules:</span></span>
  - <span data-ttu-id="6efc0-190">Uzel rezervaci kapacity nesmí být větší než maximální kapacita uzlu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-190">Node Reservation Capacity must not be greater than Maximum Node Capacity.</span></span> <span data-ttu-id="6efc0-191">Například nelze omezit hello kapacity pro metriku hello "Procesoru" v hello uzlu tootwo jednotky a akci tooreserve tři jednotky na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-191">For example, you cannot limit hello capacity for hello metric “CPU” on hello node tootwo units and try tooreserve three units on each node.</span></span>
  - <span data-ttu-id="6efc0-192">Pokud MaximumNodes není zadaný, nesmí být větší než celková kapacita aplikace hello produktu MaximumNodes a maximální kapacita uzlu.</span><span class="sxs-lookup"><span data-stu-id="6efc0-192">If MaximumNodes is specified, then hello product of MaximumNodes and Maximum Node Capacity must not be greater than Total Application Capacity.</span></span> <span data-ttu-id="6efc0-193">Předpokládejme například že hello maximální kapacita uzlu pro metrika zatížení, "Procesor" je nastavena tooeight.</span><span class="sxs-lookup"><span data-stu-id="6efc0-193">For example, let's say hello Maximum Node Capacity for load metric “CPU” is set tooeight.</span></span> <span data-ttu-id="6efc0-194">Také se stát, že nastavíte hello too10 maximální počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="6efc0-194">Let's also say you set hello Maximum Nodes too10.</span></span> <span data-ttu-id="6efc0-195">V takovém případě celková kapacita aplikace musí být větší než 80 pro tato metrika zatížení.</span><span class="sxs-lookup"><span data-stu-id="6efc0-195">In this case, Total Application Capacity must be greater than 80 for this load metric.</span></span>

<span data-ttu-id="6efc0-196">jak při vytváření aplikací a aktualizací, vynutí se omezení Hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-196">hello restrictions are enforced both during application creation and updates.</span></span>

## <a name="how-not-toouse-application-capacity"></a><span data-ttu-id="6efc0-197">Jak toouse kapacity aplikace</span><span class="sxs-lookup"><span data-stu-id="6efc0-197">How not toouse Application Capacity</span></span>
- <span data-ttu-id="6efc0-198">Nepokoušejte toouse hello skupiny aplikací funkce tooconstrain hello aplikace tooa _konkrétní_ dílčí sadu uzlů.</span><span class="sxs-lookup"><span data-stu-id="6efc0-198">Do not try toouse hello Application Group features tooconstrain hello application tooa _specific_ subset of nodes.</span></span> <span data-ttu-id="6efc0-199">Jinými slovy, můžete zadat, že aplikace hello funguje na nejvíce pět uzlů, ale není konkrétní pět uzlů, které v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6efc0-199">In other words, you can specify that hello application runs on at most five nodes, but not which specific five nodes in hello cluster.</span></span> <span data-ttu-id="6efc0-200">Chovaly aplikaci toospecific uzly lze dosáhnout pomocí omezení umístění pro služby.</span><span class="sxs-lookup"><span data-stu-id="6efc0-200">Constraining an application toospecific nodes can be achieved using placement constraints for services.</span></span>
- <span data-ttu-id="6efc0-201">Nepokoušejte tooensure kapacity aplikace hello toouse, že dvě služby z hello stejná aplikace se umístí na hello stejným uzlům.</span><span class="sxs-lookup"><span data-stu-id="6efc0-201">Do not try toouse hello Application Capacity tooensure that two services from hello same application are placed on hello same nodes.</span></span> <span data-ttu-id="6efc0-202">Místo toho použít [spřažení](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) nebo [omezení umístění](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span><span class="sxs-lookup"><span data-stu-id="6efc0-202">Instead use [affinity](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) or [placement constraints](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6efc0-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6efc0-203">Next steps</span></span>
- <span data-ttu-id="6efc0-204">Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="6efc0-204">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="6efc0-205">toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="6efc0-205">toofind out about how hello Cluster Resource Manager manages and balances load in hello cluster, check out hello article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>
- <span data-ttu-id="6efc0-206">Začít od začátku hello a [získat Úvod toohello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="6efc0-206">Start from hello beginning and [get an Introduction toohello Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
- <span data-ttu-id="6efc0-207">Další informace o fungování metriky obecně, přečtěte si [metriky zatížení Service Fabric](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="6efc0-207">For more information on how metrics work generally, read up on [Service Fabric Load Metrics](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="6efc0-208">Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6efc0-208">hello Cluster Resource Manager has many options for describing hello cluster.</span></span> <span data-ttu-id="6efc0-209">toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="6efc0-209">toofind out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
