---
title: "aaaCreate vaší první aplikace Service Fabric v jazyce C# | Microsoft Docs"
description: "Úvod toocreating aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: e95e67cc84be1b83c936b250cae9112ddc77b963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="8133e-103">Začínáme s Reliable Services</span><span class="sxs-lookup"><span data-stu-id="8133e-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8133e-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="8133e-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="8133e-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="8133e-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
> 
> 

<span data-ttu-id="8133e-106">Aplikace Azure Service Fabric obsahuje jednu nebo více služeb, které spustíte kód.</span><span class="sxs-lookup"><span data-stu-id="8133e-106">An Azure Service Fabric application contains one or more services that run your code.</span></span> <span data-ttu-id="8133e-107">Tento průvodce vám ukáže, jak toocreate bezzstavovými i stavovými aplikací Service Fabric pomocí [spolehlivé služby](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8133e-107">This guide shows you how toocreate both stateless and stateful Service Fabric applications with [Reliable Services](service-fabric-reliable-services-introduction.md).</span></span>  <span data-ttu-id="8133e-108">Toto video Microsoft Virtual Academy také ukazuje, jak toocreate bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span><span class="sxs-lookup"><span data-stu-id="8133e-108">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a><span data-ttu-id="8133e-109">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="8133e-109">Basic concepts</span></span>
<span data-ttu-id="8133e-110">spuštění se službami Reliable Services, můžete pouze tooget potřebovat toounderstand pár základních konceptech:</span><span class="sxs-lookup"><span data-stu-id="8133e-110">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="8133e-111">**Typ služby**: Toto je implementace služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-111">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="8133e-112">Je definována pomocí třídy hello napíšete, která rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.</span><span class="sxs-lookup"><span data-stu-id="8133e-112">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="8133e-113">**S názvem instance služby**: toorun vaší služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy.</span><span class="sxs-lookup"><span data-stu-id="8133e-113">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="8133e-114">Instance služby má název v podobě hello identifikátoru URI pomocí hello "fabric: /", jako například scheme "fabric: / MyApp/Moje_služba".</span><span class="sxs-lookup"><span data-stu-id="8133e-114">A service instance has a name in hello form of a URI using hello "fabric:/" scheme, such as "fabric:/MyApp/MyService".</span></span>
* <span data-ttu-id="8133e-115">**Hostitel služby**: hello s názvem instance služby vytvoříte toorun nutné v hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="8133e-115">**Service host**: hello named service instances you create need toorun inside a host process.</span></span> <span data-ttu-id="8133e-116">Hostitel služby Hello je právě proces, kde můžete spustit instance služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-116">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="8133e-117">**Registrace služby**: registrace soustřeďuje všechny informace dohromady.</span><span class="sxs-lookup"><span data-stu-id="8133e-117">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="8133e-118">Hello typ služby musí být zaregistrován s hello Service Fabric runtime ve službě hostitele tooallow Service Fabric toocreate instance ho toorun.</span><span class="sxs-lookup"><span data-stu-id="8133e-118">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="8133e-119">Vytvoření bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="8133e-119">Create a stateless service</span></span>
<span data-ttu-id="8133e-120">Bezstavové služby je typ služby, který je aktuálně hello norm v cloudových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="8133e-120">A stateless service is a type of service that is currently hello norm in cloud applications.</span></span> <span data-ttu-id="8133e-121">Považuje bezstavové, protože samotnou službu hello neobsahuje data, která potřebují toobe uložené spolehlivě nebo vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8133e-121">It is considered stateless because hello service itself does not contain data that needs toobe stored reliably or made highly available.</span></span> <span data-ttu-id="8133e-122">Pokud dojde instanci bezstavové služby, všechny jeho vnitřní stav bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="8133e-122">If an instance of a stateless service shuts down, all of its internal state is lost.</span></span> <span data-ttu-id="8133e-123">V tomto typu služby stavu musí být trvalý tooan externí úložiště, jako je Azure tabulky nebo databázi SQL pro něj toobe provedené vysoce dostupné a spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="8133e-123">In this type of service, state must be persisted tooan external store, such as Azure Tables or a SQL database, for it toobe made highly available and reliable.</span></span>

<span data-ttu-id="8133e-124">Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric s názvem *HelloWorld*:</span><span class="sxs-lookup"><span data-stu-id="8133e-124">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project named *HelloWorld*:</span></span>

![Použít hello nový projekt dialogové okno pole toocreate nové aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

<span data-ttu-id="8133e-126">Pak vytvořte bezstavové služby projektu s názvem *HelloWorldStateless*:</span><span class="sxs-lookup"><span data-stu-id="8133e-126">Then create a stateless service project named *HelloWorldStateless*:</span></span>

![V hello druhé dialogové okno vytvořte projekt bezstavové služby](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

<span data-ttu-id="8133e-128">Řešení nyní obsahuje dva projekty:</span><span class="sxs-lookup"><span data-stu-id="8133e-128">Your solution now contains two projects:</span></span>

* <span data-ttu-id="8133e-129">*Hello World*.</span><span class="sxs-lookup"><span data-stu-id="8133e-129">*HelloWorld*.</span></span> <span data-ttu-id="8133e-130">Toto je hello *aplikace* projekt, který obsahuje vaše *služby*.</span><span class="sxs-lookup"><span data-stu-id="8133e-130">This is hello *application* project that contains your *services*.</span></span> <span data-ttu-id="8133e-131">Obsahuje taky hello manifest aplikace, která popisuje hello aplikace a také řadu skriptů prostředí PowerShell, které vám pomůžou toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8133e-131">It also contains hello application manifest that describes hello application, as well as a number of PowerShell scripts that help you toodeploy your application.</span></span>
* <span data-ttu-id="8133e-132">*HelloWorldStateless*.</span><span class="sxs-lookup"><span data-stu-id="8133e-132">*HelloWorldStateless*.</span></span> <span data-ttu-id="8133e-133">Toto je projekt služby hello.</span><span class="sxs-lookup"><span data-stu-id="8133e-133">This is hello service project.</span></span> <span data-ttu-id="8133e-134">Obsahuje implementace hello bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-134">It contains hello stateless service implementation.</span></span>

## <a name="implement-hello-service"></a><span data-ttu-id="8133e-135">Implementace služby hello</span><span class="sxs-lookup"><span data-stu-id="8133e-135">Implement hello service</span></span>
<span data-ttu-id="8133e-136">Otevřete hello **HelloWorldStateless.cs** souboru v projektu služby hello.</span><span class="sxs-lookup"><span data-stu-id="8133e-136">Open hello **HelloWorldStateless.cs** file in hello service project.</span></span> <span data-ttu-id="8133e-137">V Service Fabric můžete službu spustit veškeré obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="8133e-137">In Service Fabric, a service can run any business logic.</span></span> <span data-ttu-id="8133e-138">rozhraní API služby Hello poskytuje dva vstupní body kódu:</span><span class="sxs-lookup"><span data-stu-id="8133e-138">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="8133e-139">Metodu zprostředkovává vstupního bodu, názvem *RunAsync*, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="8133e-139">An open-ended entry point method, called *RunAsync*, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* <span data-ttu-id="8133e-140">Bod položka komunikace, kde můžete zařadit do zásobníku komunikace podle vlastní volby, jako je ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8133e-140">A communication entry point where you can plug in your communication stack of choice, such as ASP.NET Core.</span></span> <span data-ttu-id="8133e-141">Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="8133e-141">This is where you can start receiving requests from users and other services.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

<span data-ttu-id="8133e-142">V tomto kurzu se zaměříme na hello `RunAsync()` metody vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="8133e-142">In this tutorial, we will focus on hello `RunAsync()` entry point method.</span></span> <span data-ttu-id="8133e-143">Toto je, kde můžete okamžitě začít kód spuštěný.</span><span class="sxs-lookup"><span data-stu-id="8133e-143">This is where you can immediately start running your code.</span></span>
<span data-ttu-id="8133e-144">Šablona projektu Hello obsahuje ukázkové provádění `RunAsync()` který zvětší kumulativní počet.</span><span class="sxs-lookup"><span data-stu-id="8133e-144">hello project template includes a sample implementation of `RunAsync()` that increments a rolling count.</span></span>

> [!NOTE]
> <span data-ttu-id="8133e-145">Podrobnosti o způsobu toowork s komunikaci ve formě zásobníku najdete v tématu [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md)</span><span class="sxs-lookup"><span data-stu-id="8133e-145">For details about how toowork with a communication stack, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md)</span></span>
> 
> 

### <a name="runasync"></a><span data-ttu-id="8133e-146">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8133e-146">RunAsync</span></span>
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

<span data-ttu-id="8133e-147">Platforma Hello volá tuto metodu, pokud instance služby je umístěný a připravené tooexecute.</span><span class="sxs-lookup"><span data-stu-id="8133e-147">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="8133e-148">Bezstavové služby, která jednoduše znamená po otevření hello instance služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-148">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="8133e-149">Token zrušení získáte toocoordinate při instanci služby musí toobe uzavřen.</span><span class="sxs-lookup"><span data-stu-id="8133e-149">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="8133e-150">V Service Fabric může tento cyklus otevření nebo uzavření instance služby k mnohokrát průběhu životnosti hello hello služby jako celek.</span><span class="sxs-lookup"><span data-stu-id="8133e-150">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="8133e-151">Toto může nastat z různých důvodů, včetně:</span><span class="sxs-lookup"><span data-stu-id="8133e-151">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="8133e-152">Hello systém přesune vaší instance služby pro vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="8133e-152">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="8133e-153">K chybám dochází v kódu.</span><span class="sxs-lookup"><span data-stu-id="8133e-153">Faults occur in your code.</span></span>
* <span data-ttu-id="8133e-154">Hello aplikace nebo systému byla upgradována.</span><span class="sxs-lookup"><span data-stu-id="8133e-154">hello application or system is upgraded.</span></span>
* <span data-ttu-id="8133e-155">základní hardware Hello dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="8133e-155">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="8133e-156">Tato orchestration spravuje hello systému tookeep služby vysoce dostupné a správně vyrovnáváním.</span><span class="sxs-lookup"><span data-stu-id="8133e-156">This orchestration is managed by hello system tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="8133e-157">`RunAsync()`by neměly blokovat synchronně.</span><span class="sxs-lookup"><span data-stu-id="8133e-157">`RunAsync()` should not block synchronously.</span></span> <span data-ttu-id="8133e-158">Implementaci RunAsync by měla vrátit úlohu nebo await na všechny operace dlouho běžící nebo blokování tooallow hello runtime toocontinue.</span><span class="sxs-lookup"><span data-stu-id="8133e-158">Your implementation of RunAsync should return a Task or await on any long-running or blocking operations tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="8133e-159">Poznámka: v hello `while(true)` smyčky v předchozí příklad hello úloh vrácení `await Task.Delay()` se používá.</span><span class="sxs-lookup"><span data-stu-id="8133e-159">Note in hello `while(true)` loop in hello previous example, a Task-returning `await Task.Delay()` is used.</span></span> <span data-ttu-id="8133e-160">Pokud vaše úlohy musí blokovat synchronně, měli byste naplánovat novou úlohu s `Task.Run()` ve vaší `RunAsync` implementace.</span><span class="sxs-lookup"><span data-stu-id="8133e-160">If your workload must block synchronously, you should schedule a new Task with `Task.Run()` in your `RunAsync` implementation.</span></span>

<span data-ttu-id="8133e-161">Zrušení úlohy je spolupráci úsilí řízená hello zadaný token zrušení.</span><span class="sxs-lookup"><span data-stu-id="8133e-161">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="8133e-162">Hello systém bude čekat vaše úloha tooend (podle úspěšné dokončení, zrušení nebo selhání) předtím, než ji přesune.</span><span class="sxs-lookup"><span data-stu-id="8133e-162">hello system will wait for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="8133e-163">Je důležité toohonor hello zrušení tokenu, dokončit všechny fungovat a ukončete `RunAsync()` provést co nejrychleji při hello systému požadavky zrušení.</span><span class="sxs-lookup"><span data-stu-id="8133e-163">It is important toohonor hello cancellation token, finish any work, and exit `RunAsync()` as quickly as possible when hello system requests cancellation.</span></span>

<span data-ttu-id="8133e-164">V tomto příkladu bezstavové služby je počet hello uložené v místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="8133e-164">In this stateless service example, hello count is stored in a local variable.</span></span> <span data-ttu-id="8133e-165">Ale protože je bezstavové služby, hello hodnotu, která je uložená existuje pouze pro hello aktuální životní cyklus jeho instanci služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-165">But because this is a stateless service, hello value that's stored exists only for hello current lifecycle of its service instance.</span></span> <span data-ttu-id="8133e-166">Když služba hello přesune nebo restartování, dojde ke ztrátě hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8133e-166">When hello service moves or restarts, hello value is lost.</span></span>

## <a name="create-a-stateful-service"></a><span data-ttu-id="8133e-167">Vytvoření stavové služby</span><span class="sxs-lookup"><span data-stu-id="8133e-167">Create a stateful service</span></span>
<span data-ttu-id="8133e-168">Service Fabric zavádí nový typ služby, která je stavový.</span><span class="sxs-lookup"><span data-stu-id="8133e-168">Service Fabric introduces a new kind of service that is stateful.</span></span> <span data-ttu-id="8133e-169">Stavové služby můžete udržovat stav spolehlivě v rámci služby hello, samostatně, umístěn společně s hello kód, který se používá.</span><span class="sxs-lookup"><span data-stu-id="8133e-169">A stateful service can maintain state reliably within hello service itself, co-located with hello code that's using it.</span></span> <span data-ttu-id="8133e-170">Stav je vysoké dostupnosti pomocí Service Fabric bez hello nutné toopersist stavu tooan externím obchodu.</span><span class="sxs-lookup"><span data-stu-id="8133e-170">State is made highly available by Service Fabric without hello need toopersist state tooan external store.</span></span>

<span data-ttu-id="8133e-171">tooconvert hodnota čítače z bezstavové toohighly dostupné a trvalé, i když služba hello přesune nebo restartuje, musíte stavové služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-171">tooconvert a counter value from stateless toohighly available and persistent, even when hello service moves or restarts, you need a stateful service.</span></span>

<span data-ttu-id="8133e-172">V hello stejné *HelloWorld* aplikace, můžete přidat novou službu kliknutím pravým tlačítkem na hello služby odkazů v projektu aplikace hello a výběr **Přidat -> Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="8133e-172">In hello same *HelloWorld* application, you can add a new service by right-clicking on hello Services references in hello application project and selecting **Add ->  New Service Fabric Service**.</span></span>

![Přidání služby tooyour aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

<span data-ttu-id="8133e-174">Vyberte **stavové služby** a pojmenujte ji *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="8133e-174">Select **Stateful Service** and name it *HelloWorldStateful*.</span></span> <span data-ttu-id="8133e-175">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8133e-175">Click **OK**.</span></span>

![Použít hello nový projekt dialogové okno pole toocreate nové stavové služby Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

<span data-ttu-id="8133e-177">Aplikace musí mít teď dvě služby: hello bezstavové služby *HelloWorldStateless* a stavové služby hello *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="8133e-177">Your application should now have two services: hello stateless service *HelloWorldStateless* and hello stateful service *HelloWorldStateful*.</span></span>

<span data-ttu-id="8133e-178">Stavová služba má hello stejné vstupní body jako bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-178">A stateful service has hello same entry points as a stateless service.</span></span> <span data-ttu-id="8133e-179">Hlavní rozdíl Hello je hello dostupnost *zprostředkovatele stavu* který spolehlivě uložení stavu.</span><span class="sxs-lookup"><span data-stu-id="8133e-179">hello main difference is hello availability of a *state provider* that can store state reliably.</span></span> <span data-ttu-id="8133e-180">Service Fabric se dodává s implementace zprostředkovatele stav názvem [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md), která vám umožňuje vytvořit struktury replikovaná data prostřednictvím hello spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="8133e-180">Service Fabric comes with a state provider implementation called [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), which lets you create replicated data structures through hello Reliable State Manager.</span></span> <span data-ttu-id="8133e-181">Stavové spolehlivé služby pomocí zprostředkovatele stavu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8133e-181">A stateful Reliable Service uses this state provider by default.</span></span>

<span data-ttu-id="8133e-182">Otevřete **HelloWorldStateful.cs** v *HelloWorldStateful*, který obsahuje následující metodě RunAsync hello:</span><span class="sxs-lookup"><span data-stu-id="8133e-182">Open **HelloWorldStateful.cs** in *HelloWorldStateful*, which contains hello following RunAsync method:</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, hello transaction aborts, all changes are
            // discarded, and nothing is saved toohello secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a><span data-ttu-id="8133e-183">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8133e-183">RunAsync</span></span>
<span data-ttu-id="8133e-184">`RunAsync()`funguje podobně jako v stavová a Bezstavová služby.</span><span class="sxs-lookup"><span data-stu-id="8133e-184">`RunAsync()` operates similarly in stateful and stateless services.</span></span> <span data-ttu-id="8133e-185">Však v stavové služby platformy hello provede další práci vaším jménem předtím, než se provede `RunAsync()`.</span><span class="sxs-lookup"><span data-stu-id="8133e-185">However, in a stateful service, hello platform performs additional work on your behalf before it executes `RunAsync()`.</span></span> <span data-ttu-id="8133e-186">Tento pracovní může obsahovat zajistit, že hello spolehlivé správce stavu a spolehlivé kolekce jsou připravené toouse.</span><span class="sxs-lookup"><span data-stu-id="8133e-186">This work can include ensuring that hello Reliable State Manager and Reliable Collections are ready toouse.</span></span>

### <a name="reliable-collections-and-hello-reliable-state-manager"></a><span data-ttu-id="8133e-187">Spolehlivé kolekce a hello spolehlivé správce stavu</span><span class="sxs-lookup"><span data-stu-id="8133e-187">Reliable Collections and hello Reliable State Manager</span></span>
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

<span data-ttu-id="8133e-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) je slovník implementace, které můžete použít tooreliably ukládání stavu ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="8133e-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is a dictionary implementation that you can use tooreliably store state in hello service.</span></span> <span data-ttu-id="8133e-189">Service Fabric a spolehlivé kolekcí mohou ukládat data přímo ve službě bez nutnosti hello externí trvalého úložiště.</span><span class="sxs-lookup"><span data-stu-id="8133e-189">With Service Fabric and Reliable Collections, you can store data directly in your service without hello need for an external persistent store.</span></span> <span data-ttu-id="8133e-190">Spolehlivé kolekce dají vašim datům vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="8133e-190">Reliable Collections make your data highly available.</span></span> <span data-ttu-id="8133e-191">Service Fabric dosahuje tak, že vytváření a správu více *repliky* služby za vás.</span><span class="sxs-lookup"><span data-stu-id="8133e-191">Service Fabric accomplishes this by creating and managing multiple *replicas* of your service for you.</span></span> <span data-ttu-id="8133e-192">Také poskytuje rozhraní API, které abstrahuje tokeny hello složitosti správy tyto repliky a jejich přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="8133e-192">It also provides an API that abstracts away hello complexities of managing those replicas and their state transitions.</span></span>

<span data-ttu-id="8133e-193">Spolehlivé kolekce můžete ukládat jakýkoli typ rozhraní .NET, včetně vlastních typů, pomocí několika upozornění:</span><span class="sxs-lookup"><span data-stu-id="8133e-193">Reliable Collections can store any .NET type, including your custom types, with a couple of caveats:</span></span>

* <span data-ttu-id="8133e-194">Service Fabric umožňuje vašemu stavu vysoce dostupné podle *replikace* úložiště stavu napříč uzly a spolehlivé kolekce datový disk toolocal na jednotlivé repliky.</span><span class="sxs-lookup"><span data-stu-id="8133e-194">Service Fabric makes your state highly available by *replicating* state across nodes, and Reliable Collections store your data toolocal disk on each replica.</span></span> <span data-ttu-id="8133e-195">To znamená, že vše, co je uložen v spolehlivé kolekce musí být *serializovatelný*.</span><span class="sxs-lookup"><span data-stu-id="8133e-195">This means that everything that is stored in Reliable Collections must be *serializable*.</span></span> <span data-ttu-id="8133e-196">Ve výchozím nastavení, použijte spolehlivé kolekce [kontraktu](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) pro serializaci, proto je důležité toomake se, že jsou vaše typy [nepodporuje hello serializátor kontraktu dat](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) při použití výchozí hello serializátor.</span><span class="sxs-lookup"><span data-stu-id="8133e-196">By default, Reliable Collections use [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) for serialization, so it's important toomake sure that your types are [supported by hello Data Contract Serializer](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) when you use hello default serializer.</span></span>
* <span data-ttu-id="8133e-197">Objekty jsou replikovány pro zajištění vysoké dostupnosti při potvrzení transakce na spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="8133e-197">Objects are replicated for high availability when you commit transactions on Reliable Collections.</span></span> <span data-ttu-id="8133e-198">Objekty uložené v kolekcích spolehlivé udržovaly v místní paměti ve službě.</span><span class="sxs-lookup"><span data-stu-id="8133e-198">Objects stored in Reliable Collections are kept in local memory in your service.</span></span> <span data-ttu-id="8133e-199">To znamená, že máte toohello místní referenční objekt.</span><span class="sxs-lookup"><span data-stu-id="8133e-199">This means that you have a local reference toohello object.</span></span>
  
   <span data-ttu-id="8133e-200">Je důležité, neprovádějte místní instancí těchto objektů bez provádění operace aktualizace na hello spolehlivé kolekce v transakci.</span><span class="sxs-lookup"><span data-stu-id="8133e-200">It is important that you do not mutate local instances of those objects without performing an update operation on hello reliable collection in a transaction.</span></span> <span data-ttu-id="8133e-201">To je proto nebude automaticky replikovat změny toolocal instance objektů.</span><span class="sxs-lookup"><span data-stu-id="8133e-201">This is because changes toolocal instances of objects will not be replicated automatically.</span></span> <span data-ttu-id="8133e-202">Musíte znovu vložit objekt hello zpět do slovníku hello nebo použijte jednu z hello *aktualizace* metody na hello slovníku.</span><span class="sxs-lookup"><span data-stu-id="8133e-202">You must re-insert hello object back into hello dictionary or use one of hello *update* methods on hello dictionary.</span></span>

<span data-ttu-id="8133e-203">Hello spolehlivé správce stavu spravuje spolehlivé kolekce za vás.</span><span class="sxs-lookup"><span data-stu-id="8133e-203">hello Reliable State Manager manages Reliable Collections for you.</span></span> <span data-ttu-id="8133e-204">Můžete jednoduše pokládat hello spolehlivé správce stavu pro kolekci spolehlivé podle názvu kdykoli a kdekoli v službě.</span><span class="sxs-lookup"><span data-stu-id="8133e-204">You can simply ask hello Reliable State Manager for a reliable collection by name at any time and at any place in your service.</span></span> <span data-ttu-id="8133e-205">Hello spolehlivé správce stavu zajišťuje, získejte odkaz na zpět.</span><span class="sxs-lookup"><span data-stu-id="8133e-205">hello Reliable State Manager ensures that you get a reference back.</span></span> <span data-ttu-id="8133e-206">Není vhodné uložit odkazy tooreliable kolekci instancí třídy členské proměnné nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8133e-206">We don't recommended that you save references tooreliable collection instances in class member variables or properties.</span></span> <span data-ttu-id="8133e-207">Musí dát zvláštní pozor tooensure nastavený hello odkaz tooan instance v případech, v průběhu životního cyklu služby hello.</span><span class="sxs-lookup"><span data-stu-id="8133e-207">Special care must be taken tooensure that hello reference is set tooan instance at all times in hello service lifecycle.</span></span> <span data-ttu-id="8133e-208">Hello spolehlivé správce stavu zpracuje tato práce pro uživatele a je optimalizovaný pro opakování návštěvách.</span><span class="sxs-lookup"><span data-stu-id="8133e-208">hello Reliable State Manager handles this work for you, and it's optimized for repeat visits.</span></span>

### <a name="transactional-and-asynchronous-operations"></a><span data-ttu-id="8133e-209">Transakční a asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="8133e-209">Transactional and asynchronous operations</span></span>
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

<span data-ttu-id="8133e-210">Spolehlivé kolekce mají mnoho hello stejné operace, jejich `System.Collections.Generic` a `System.Collections.Concurrent` svými protějšky, s výjimkou LINQ.</span><span class="sxs-lookup"><span data-stu-id="8133e-210">Reliable Collections have many of hello same operations that their `System.Collections.Generic` and `System.Collections.Concurrent` counterparts do, except LINQ.</span></span> <span data-ttu-id="8133e-211">Operace na spolehlivé kolekce jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="8133e-211">Operations on Reliable Collections are asynchronous.</span></span> <span data-ttu-id="8133e-212">Je to proto, že operace zápisu ke kolekcím, spolehlivé provádění vstupně-výstupní operace tooreplicate a zachovat data toodisk.</span><span class="sxs-lookup"><span data-stu-id="8133e-212">This is because write operations with Reliable Collections perform I/O operations tooreplicate and persist data toodisk.</span></span>

<span data-ttu-id="8133e-213">Spolehlivé operace kolekce jsou *transakcí*, takže můžete zachovat stav konzistentní napříč více spolehlivé kolekce a operace.</span><span class="sxs-lookup"><span data-stu-id="8133e-213">Reliable Collection operations are *transactional*, so that you can keep state consistent across multiple Reliable Collections and operations.</span></span> <span data-ttu-id="8133e-214">Může například dequeue – pracovní položky z fronty spolehlivé, proveďte operaci na něm a uložit výsledek hello slovník spolehlivé, vše v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="8133e-214">For example, you may dequeue a work item from a Reliable Queue, perform an operation on it, and save hello result in a Reliable Dictionary, all within a single transaction.</span></span> <span data-ttu-id="8133e-215">To je považován za atomická operace, a zaručuje, že buď hello celé operace proběhne úspěšně nebo celou operaci hello vrátíte zpět.</span><span class="sxs-lookup"><span data-stu-id="8133e-215">This is treated as an atomic operation, and it guarantees that either hello entire operation will succeed or hello entire operation will roll back.</span></span> <span data-ttu-id="8133e-216">Pokud dojde k chybě po dequeue hello položku, ale před uložením hello výsledek, hello celá transakce bude vrácena zpět a hello položka zůstane ve frontě hello ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="8133e-216">If an error occurs after you dequeue hello item but before you save hello result, hello entire transaction is rolled back and hello item remains in hello queue for processing.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="8133e-217">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8133e-217">Run hello application</span></span>
<span data-ttu-id="8133e-218">Nemůžeme se teď vrátit toohello *HelloWorld* aplikace.</span><span class="sxs-lookup"><span data-stu-id="8133e-218">We now return toohello *HelloWorld* application.</span></span> <span data-ttu-id="8133e-219">Teď můžete sestavit a nasazení služeb.</span><span class="sxs-lookup"><span data-stu-id="8133e-219">You can now build and deploy your services.</span></span> <span data-ttu-id="8133e-220">Po stisknutí klávesy **F5**, bude vaše aplikace vytvořené a nasazené tooyour místní cluster.</span><span class="sxs-lookup"><span data-stu-id="8133e-220">When you press **F5**, your application will be built and deployed tooyour local cluster.</span></span>

<span data-ttu-id="8133e-221">Po hello služby spuštění, spuštění, můžete zobrazit události trasování událostí pro Windows (ETW) hello vygenerované v **diagnostických událostí** okno.</span><span class="sxs-lookup"><span data-stu-id="8133e-221">After hello services start running, you can view hello generated Event Tracing for Windows (ETW) events in a **Diagnostic Events** window.</span></span> <span data-ttu-id="8133e-222">Upozorňujeme, že zobrazených událostí hello jsou z hello bezstavové služby i hello stavové služby v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="8133e-222">Note that hello events displayed are from both hello stateless service and hello stateful service in hello application.</span></span> <span data-ttu-id="8133e-223">Je možné pozastavit datový proud hello kliknutím hello **pozastavit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8133e-223">You can pause hello stream by clicking hello **Pause** button.</span></span> <span data-ttu-id="8133e-224">Poté můžete prozkoumat hello podrobnosti zprávy rozbalením této zprávě.</span><span class="sxs-lookup"><span data-stu-id="8133e-224">You can then examine hello details of a message by expanding that message.</span></span>

> [!NOTE]
> <span data-ttu-id="8133e-225">Před spuštěním aplikace hello, ujistěte se, že máte místního vývojového clusteru se systémem.</span><span class="sxs-lookup"><span data-stu-id="8133e-225">Before you run hello application, make sure that you have a local development cluster running.</span></span> <span data-ttu-id="8133e-226">Podívejte se na hello [Příručka Začínáme](service-fabric-get-started.md) informace o nastavení vašeho místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8133e-226">Check out hello [getting started guide](service-fabric-get-started.md) for information on setting up your local environment.</span></span>
> 
> 

![Zobrazení diagnostických událostí v sadě Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a><span data-ttu-id="8133e-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8133e-228">Next steps</span></span>
[<span data-ttu-id="8133e-229">Ladění aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8133e-229">Debug your Service Fabric application in Visual Studio</span></span>](service-fabric-debugging-your-application.md)

[<span data-ttu-id="8133e-230">Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="8133e-230">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>](service-fabric-reliable-services-communication-webapi.md)

[<span data-ttu-id="8133e-231">Další informace o spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="8133e-231">Learn more about Reliable Collections</span></span>](service-fabric-reliable-services-reliable-collections.md)

[<span data-ttu-id="8133e-232">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="8133e-232">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[<span data-ttu-id="8133e-233">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="8133e-233">Application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="8133e-234">Referenční informace pro vývojáře pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="8133e-234">Developer reference for Reliable Services</span></span>](https://msdn.microsoft.com/library/azure/dn706529.aspx)

