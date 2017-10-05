---
title: "Vytvoření první aplikace Service Fabric v jazyce C# | Microsoft Docs"
description: "Úvod do vytváření aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
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
ms.openlocfilehash: 813021d6239ae3cf79bb84b78f77e39c9e0783f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="e4d4a-103">Začínáme s Reliable Services</span><span class="sxs-lookup"><span data-stu-id="e4d4a-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e4d4a-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="e4d4a-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="e4d4a-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="e4d4a-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
> 
> 

<span data-ttu-id="e4d4a-106">Aplikace Azure Service Fabric obsahuje jednu nebo více služeb, které spustíte kód.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-106">An Azure Service Fabric application contains one or more services that run your code.</span></span> <span data-ttu-id="e4d4a-107">Tento průvodce vám ukáže, jak vytvořit bezzstavovými i stavovými aplikací Service Fabric pomocí [spolehlivé služby](service-fabric-reliable-services-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e4d4a-107">This guide shows you how to create both stateless and stateful Service Fabric applications with [Reliable Services](service-fabric-reliable-services-introduction.md).</span></span>  <span data-ttu-id="e4d4a-108">Video tento Microsoft Virtual Academy také ukazuje postup vytvoření bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span><span class="sxs-lookup"><span data-stu-id="e4d4a-108">This Microsoft Virtual Academy video also shows you how to create a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a><span data-ttu-id="e4d4a-109">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="e4d4a-109">Basic concepts</span></span>
<span data-ttu-id="e4d4a-110">Pokud chcete začít se službami Reliable Services, potřebujete jenom pochopit několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-110">To get started with Reliable Services, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="e4d4a-111">**Typ služby**: Toto je implementace služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-111">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="e4d4a-112">Je definována v třídě napíšete, který rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-112">It is defined by the class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="e4d4a-113">**S názvem instance služby**: ke spuštění služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-113">**Named service instance**: To run your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="e4d4a-114">Instance služby má název ve formě identifikátoru URI pomocí "fabric: /", jako například scheme "fabric: / MyApp/Moje_služba".</span><span class="sxs-lookup"><span data-stu-id="e4d4a-114">A service instance has a name in the form of a URI using the "fabric:/" scheme, such as "fabric:/MyApp/MyService".</span></span>
* <span data-ttu-id="e4d4a-115">**Hostitel služby**: pojmenované instance vytvoříte muset spustit v hostitelském procesu.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-115">**Service host**: The named service instances you create need to run inside a host process.</span></span> <span data-ttu-id="e4d4a-116">Hostitel služby je právě proces, kde můžete spustit instance služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-116">The service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="e4d4a-117">**Registrace služby**: registrace soustřeďuje všechny informace dohromady.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-117">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="e4d4a-118">Typ služby musí být zaregistrován u modulu runtime Service Fabric v hostitele služby umožňující Service Fabric k vytvoření instance ho spustit.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-118">The service type must be registered with the Service Fabric runtime in a service host to allow Service Fabric to create instances of it to run.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="e4d4a-119">Vytvoření bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="e4d4a-119">Create a stateless service</span></span>
<span data-ttu-id="e4d4a-120">Bezstavové služby je typ služby, který je aktuálně norm v cloudových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-120">A stateless service is a type of service that is currently the norm in cloud applications.</span></span> <span data-ttu-id="e4d4a-121">Považuje bezstavové, protože samotné služby neobsahuje data, která musí být uložené spolehlivě nebo vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-121">It is considered stateless because the service itself does not contain data that needs to be stored reliably or made highly available.</span></span> <span data-ttu-id="e4d4a-122">Pokud dojde instanci bezstavové služby, všechny jeho vnitřní stav bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-122">If an instance of a stateless service shuts down, all of its internal state is lost.</span></span> <span data-ttu-id="e4d4a-123">V tomto typu služby musí být stav ukládaný na externí úložiště, jako jsou tabulky Azure nebo databázi SQL, mohla provést vysoce dostupné a spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-123">In this type of service, state must be persisted to an external store, such as Azure Tables or a SQL database, for it to be made highly available and reliable.</span></span>

<span data-ttu-id="e4d4a-124">Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric s názvem *HelloWorld*:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-124">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project named *HelloWorld*:</span></span>

![Pomocí dialogového okna Nový projekt pro vytvoření nové aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

<span data-ttu-id="e4d4a-126">Pak vytvořte bezstavové služby projektu s názvem *HelloWorldStateless*:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-126">Then create a stateless service project named *HelloWorldStateless*:</span></span>

![V dialogovém okně druhý vytvořte projekt bezstavové služby](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

<span data-ttu-id="e4d4a-128">Řešení nyní obsahuje dva projekty:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-128">Your solution now contains two projects:</span></span>

* <span data-ttu-id="e4d4a-129">*Hello World*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-129">*HelloWorld*.</span></span> <span data-ttu-id="e4d4a-130">Toto je *aplikace* projekt, který obsahuje vaše *služby*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-130">This is the *application* project that contains your *services*.</span></span> <span data-ttu-id="e4d4a-131">Obsahuje taky manifest aplikace, která popisuje aplikace a také řadu skriptů prostředí PowerShell, které pomáhají při nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-131">It also contains the application manifest that describes the application, as well as a number of PowerShell scripts that help you to deploy your application.</span></span>
* <span data-ttu-id="e4d4a-132">*HelloWorldStateless*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-132">*HelloWorldStateless*.</span></span> <span data-ttu-id="e4d4a-133">Toto je projekt služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-133">This is the service project.</span></span> <span data-ttu-id="e4d4a-134">Obsahuje implementace bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-134">It contains the stateless service implementation.</span></span>

## <a name="implement-the-service"></a><span data-ttu-id="e4d4a-135">Tuto službu implementovat</span><span class="sxs-lookup"><span data-stu-id="e4d4a-135">Implement the service</span></span>
<span data-ttu-id="e4d4a-136">Otevřete **HelloWorldStateless.cs** souboru v projektu služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-136">Open the **HelloWorldStateless.cs** file in the service project.</span></span> <span data-ttu-id="e4d4a-137">V Service Fabric můžete službu spustit veškeré obchodní logiky.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-137">In Service Fabric, a service can run any business logic.</span></span> <span data-ttu-id="e4d4a-138">Rozhraní API služby obsahuje dvě vstupní body pro váš kód:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-138">The service API provides two entry points for your code:</span></span>

* <span data-ttu-id="e4d4a-139">Metodu zprostředkovává vstupního bodu, názvem *RunAsync*, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-139">An open-ended entry point method, called *RunAsync*, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* <span data-ttu-id="e4d4a-140">Bod položka komunikace, kde můžete zařadit do zásobníku komunikace podle vlastní volby, jako je ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-140">A communication entry point where you can plug in your communication stack of choice, such as ASP.NET Core.</span></span> <span data-ttu-id="e4d4a-141">Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-141">This is where you can start receiving requests from users and other services.</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

<span data-ttu-id="e4d4a-142">V tomto kurzu se zaměříme na `RunAsync()` metody vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-142">In this tutorial, we will focus on the `RunAsync()` entry point method.</span></span> <span data-ttu-id="e4d4a-143">Toto je, kde můžete okamžitě začít kód spuštěný.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-143">This is where you can immediately start running your code.</span></span>
<span data-ttu-id="e4d4a-144">Šablona projektu obsahuje ukázkové provádění `RunAsync()` který zvětší kumulativní počet.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-144">The project template includes a sample implementation of `RunAsync()` that increments a rolling count.</span></span>

> [!NOTE]
> <span data-ttu-id="e4d4a-145">Podrobnosti o tom, jak pracovat s komunikačního balíku najdete v tématu [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md)</span><span class="sxs-lookup"><span data-stu-id="e4d4a-145">For details about how to work with a communication stack, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md)</span></span>
> 
> 

### <a name="runasync"></a><span data-ttu-id="e4d4a-146">RunAsync</span><span class="sxs-lookup"><span data-stu-id="e4d4a-146">RunAsync</span></span>
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
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

<span data-ttu-id="e4d4a-147">Platforma volá tuto metodu, když je umístěný a připravené ke spuštění instance služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-147">The platform calls this method when an instance of a service is placed and ready to execute.</span></span> <span data-ttu-id="e4d4a-148">Bezstavové služby, která jednoduše znamená, když je otevřen v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-148">For a stateless service, that simply means when the service instance is opened.</span></span> <span data-ttu-id="e4d4a-149">Token zrušení je k dispozici pro koordinaci při instanci služby musí být uzavřen.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-149">A cancellation token is provided to coordinate when your service instance needs to be closed.</span></span> <span data-ttu-id="e4d4a-150">V Service Fabric tento cyklus otevření nebo uzavření instance služby může docházet k tolikrát, kolikrát za dobu existence služby jako celek.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-150">In Service Fabric, this open/close cycle of a service instance can occur many times over the lifetime of the service as a whole.</span></span> <span data-ttu-id="e4d4a-151">Toto může nastat z různých důvodů, včetně:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-151">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="e4d4a-152">Systém přesune vaší instance služby pro vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-152">The system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="e4d4a-153">K chybám dochází v kódu.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-153">Faults occur in your code.</span></span>
* <span data-ttu-id="e4d4a-154">Aplikace nebo systému byla upgradována.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-154">The application or system is upgraded.</span></span>
* <span data-ttu-id="e4d4a-155">Základní hardware dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-155">The underlying hardware experiences an outage.</span></span>

<span data-ttu-id="e4d4a-156">Tato orchestration je spravován systémem vysoce dostupné a správně vyrovnáváním zachovat služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-156">This orchestration is managed by the system to keep your service highly available and properly balanced.</span></span>

<span data-ttu-id="e4d4a-157">`RunAsync()`by neměly blokovat synchronně.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-157">`RunAsync()` should not block synchronously.</span></span> <span data-ttu-id="e4d4a-158">Implementaci RunAsync by měla vrátit úlohu nebo await na jakékoli operace dlouhotrvající nebo blokování umožňující pokračovat modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-158">Your implementation of RunAsync should return a Task or await on any long-running or blocking operations to allow the runtime to continue.</span></span> <span data-ttu-id="e4d4a-159">Poznámka: v `while(true)` smyčky v předchozím příkladu, úloha vrácení `await Task.Delay()` se používá.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-159">Note in the `while(true)` loop in the previous example, a Task-returning `await Task.Delay()` is used.</span></span> <span data-ttu-id="e4d4a-160">Pokud vaše úlohy musí blokovat synchronně, měli byste naplánovat novou úlohu s `Task.Run()` ve vaší `RunAsync` implementace.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-160">If your workload must block synchronously, you should schedule a new Task with `Task.Run()` in your `RunAsync` implementation.</span></span>

<span data-ttu-id="e4d4a-161">Zrušení úlohy je spolupráci úsilí řízená token poskytnutý zrušení.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-161">Cancellation of your workload is a cooperative effort orchestrated by the provided cancellation token.</span></span> <span data-ttu-id="e4d4a-162">Systém bude počkejte na ukončení (podle úspěšné dokončení, zrušení nebo selhání), než ji přesune vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-162">The system will wait for your task to end (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="e4d4a-163">Je důležité respektovat token zrušení, Dokončit veškerou práci a ukončete `RunAsync()` provést co nejrychleji, pokud systém požadavky zrušení.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-163">It is important to honor the cancellation token, finish any work, and exit `RunAsync()` as quickly as possible when the system requests cancellation.</span></span>

<span data-ttu-id="e4d4a-164">V tomto příkladu bezstavové služby je počet uložené v místní proměnné.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-164">In this stateless service example, the count is stored in a local variable.</span></span> <span data-ttu-id="e4d4a-165">Ale protože je bezstavové služby, hodnotu, která je uložená existuje pouze pro aktuální životní cyklus jeho instanci služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-165">But because this is a stateless service, the value that's stored exists only for the current lifecycle of its service instance.</span></span> <span data-ttu-id="e4d4a-166">Pokud službu přesune nebo restartuje, hodnota je ztraceny.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-166">When the service moves or restarts, the value is lost.</span></span>

## <a name="create-a-stateful-service"></a><span data-ttu-id="e4d4a-167">Vytvoření stavové služby</span><span class="sxs-lookup"><span data-stu-id="e4d4a-167">Create a stateful service</span></span>
<span data-ttu-id="e4d4a-168">Service Fabric zavádí nový typ služby, která je stavový.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-168">Service Fabric introduces a new kind of service that is stateful.</span></span> <span data-ttu-id="e4d4a-169">Stavové služby můžete udržovat stav spolehlivě v rámci služby samostatně, umístěn společně s kód, který se používá.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-169">A stateful service can maintain state reliably within the service itself, co-located with the code that's using it.</span></span> <span data-ttu-id="e4d4a-170">Stav je vysoké dostupnosti pomocí Service Fabric bez nutnosti zachování stavu na externím obchodu.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-170">State is made highly available by Service Fabric without the need to persist state to an external store.</span></span>

<span data-ttu-id="e4d4a-171">Chcete-li převést hodnotu čítače bezstavové na vysoce dostupné a trvalé, i když služba přesune nebo restartování, je třeba stavové služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-171">To convert a counter value from stateless to highly available and persistent, even when the service moves or restarts, you need a stateful service.</span></span>

<span data-ttu-id="e4d4a-172">Ve stejném *HelloWorld* aplikace, můžete přidat nové služby tak, že kliknete pravým tlačítkem na službách odkazů v projektu aplikace a výběr **Přidat -> Nový Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-172">In the same *HelloWorld* application, you can add a new service by right-clicking on the Services references in the application project and selecting **Add ->  New Service Fabric Service**.</span></span>

![Přidání služby do aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

<span data-ttu-id="e4d4a-174">Vyberte **stavové služby** a pojmenujte ji *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-174">Select **Stateful Service** and name it *HelloWorldStateful*.</span></span> <span data-ttu-id="e4d4a-175">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-175">Click **OK**.</span></span>

![Pomocí dialogového okna Nový projekt pro vytvoření nové stavové služby Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

<span data-ttu-id="e4d4a-177">Aplikace musí mít teď dvě služby: bezstavové služby *HelloWorldStateless* a stavové služby *HelloWorldStateful*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-177">Your application should now have two services: the stateless service *HelloWorldStateless* and the stateful service *HelloWorldStateful*.</span></span>

<span data-ttu-id="e4d4a-178">Stavová služba má stejné vstupní body jako bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-178">A stateful service has the same entry points as a stateless service.</span></span> <span data-ttu-id="e4d4a-179">Hlavní rozdíl je dostupnost *zprostředkovatele stavu* který spolehlivě uložení stavu.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-179">The main difference is the availability of a *state provider* that can store state reliably.</span></span> <span data-ttu-id="e4d4a-180">Service Fabric se dodává s implementace zprostředkovatele stav názvem [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md), což umožňuje vytváření replikované datové struktury prostřednictvím spolehlivé správce stavu.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-180">Service Fabric comes with a state provider implementation called [Reliable Collections](service-fabric-reliable-services-reliable-collections.md), which lets you create replicated data structures through the Reliable State Manager.</span></span> <span data-ttu-id="e4d4a-181">Stavové spolehlivé služby pomocí zprostředkovatele stavu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-181">A stateful Reliable Service uses this state provider by default.</span></span>

<span data-ttu-id="e4d4a-182">Otevřete **HelloWorldStateful.cs** v *HelloWorldStateful*, který obsahuje následující metodě RunAsync:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-182">Open **HelloWorldStateful.cs** in *HelloWorldStateful*, which contains the following RunAsync method:</span></span>

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following sample code with your own logic
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

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a><span data-ttu-id="e4d4a-183">RunAsync</span><span class="sxs-lookup"><span data-stu-id="e4d4a-183">RunAsync</span></span>
<span data-ttu-id="e4d4a-184">`RunAsync()`funguje podobně jako v stavová a Bezstavová služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-184">`RunAsync()` operates similarly in stateful and stateless services.</span></span> <span data-ttu-id="e4d4a-185">Ale v stavové služby platformy provede další práci vaším jménem předtím, než se provede `RunAsync()`.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-185">However, in a stateful service, the platform performs additional work on your behalf before it executes `RunAsync()`.</span></span> <span data-ttu-id="e4d4a-186">Tento pracovní mohou zahrnovat kontrolu, spolehlivé správce stavu a spolehlivé kolekce jsou připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-186">This work can include ensuring that the Reliable State Manager and Reliable Collections are ready to use.</span></span>

### <a name="reliable-collections-and-the-reliable-state-manager"></a><span data-ttu-id="e4d4a-187">Spolehlivé kolekce a spolehlivé správce stavu</span><span class="sxs-lookup"><span data-stu-id="e4d4a-187">Reliable Collections and the Reliable State Manager</span></span>
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

<span data-ttu-id="e4d4a-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) je implementace slovník, který můžete použít k spolehlivě uložení stavu ve službě.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-188">[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) is a dictionary implementation that you can use to reliably store state in the service.</span></span> <span data-ttu-id="e4d4a-189">Service Fabric a spolehlivé kolekcí mohou ukládat data přímo ve službě bez nutnosti externí trvalého úložiště.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-189">With Service Fabric and Reliable Collections, you can store data directly in your service without the need for an external persistent store.</span></span> <span data-ttu-id="e4d4a-190">Spolehlivé kolekce dají vašim datům vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-190">Reliable Collections make your data highly available.</span></span> <span data-ttu-id="e4d4a-191">Service Fabric dosahuje tak, že vytváření a správu více *repliky* služby za vás.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-191">Service Fabric accomplishes this by creating and managing multiple *replicas* of your service for you.</span></span> <span data-ttu-id="e4d4a-192">Také poskytuje rozhraní API, které abstrahuje rychle složitosti správy tyto repliky a jejich přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-192">It also provides an API that abstracts away the complexities of managing those replicas and their state transitions.</span></span>

<span data-ttu-id="e4d4a-193">Spolehlivé kolekce můžete ukládat jakýkoli typ rozhraní .NET, včetně vlastních typů, pomocí několika upozornění:</span><span class="sxs-lookup"><span data-stu-id="e4d4a-193">Reliable Collections can store any .NET type, including your custom types, with a couple of caveats:</span></span>

* <span data-ttu-id="e4d4a-194">Service Fabric umožňuje vašemu stavu vysoce dostupné podle *replikace* stavu napříč uzly a spolehlivé kolekce ukládat data na místní disk na jednotlivé repliky.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-194">Service Fabric makes your state highly available by *replicating* state across nodes, and Reliable Collections store your data to local disk on each replica.</span></span> <span data-ttu-id="e4d4a-195">To znamená, že vše, co je uložen v spolehlivé kolekce musí být *serializovatelný*.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-195">This means that everything that is stored in Reliable Collections must be *serializable*.</span></span> <span data-ttu-id="e4d4a-196">Ve výchozím nastavení, použijte spolehlivé kolekce [kontraktu](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) pro serializaci, proto je důležité se ujistit, že jsou vaše typy [nepodporuje serializátor kontraktu dat](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) při použití výchozí serializátor.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-196">By default, Reliable Collections use [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) for serialization, so it's important to make sure that your types are [supported by the Data Contract Serializer](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) when you use the default serializer.</span></span>
* <span data-ttu-id="e4d4a-197">Objekty jsou replikovány pro zajištění vysoké dostupnosti při potvrzení transakce na spolehlivé kolekce.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-197">Objects are replicated for high availability when you commit transactions on Reliable Collections.</span></span> <span data-ttu-id="e4d4a-198">Objekty uložené v kolekcích spolehlivé udržovaly v místní paměti ve službě.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-198">Objects stored in Reliable Collections are kept in local memory in your service.</span></span> <span data-ttu-id="e4d4a-199">To znamená, že máte místní odkaz na objekt.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-199">This means that you have a local reference to the object.</span></span>
  
   <span data-ttu-id="e4d4a-200">Je důležité, neprovádějte místní instancí těchto objektů bez provádění operace aktualizace na kolekci spolehlivé v transakci.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-200">It is important that you do not mutate local instances of those objects without performing an update operation on the reliable collection in a transaction.</span></span> <span data-ttu-id="e4d4a-201">To je proto nebude automaticky replikovat změny místní instance objektů.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-201">This is because changes to local instances of objects will not be replicated automatically.</span></span> <span data-ttu-id="e4d4a-202">Musíte znovu vložit objekt zpět do slovníku nebo použijte jednu z *aktualizace* metody ve slovníku.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-202">You must re-insert the object back into the dictionary or use one of the *update* methods on the dictionary.</span></span>

<span data-ttu-id="e4d4a-203">Spolehlivé správce stavu spravuje spolehlivé kolekce za vás.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-203">The Reliable State Manager manages Reliable Collections for you.</span></span> <span data-ttu-id="e4d4a-204">Můžete jednoduše pokládat spolehlivé správce stavu pro kolekci spolehlivé podle názvu kdykoli a kdekoli v službě.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-204">You can simply ask the Reliable State Manager for a reliable collection by name at any time and at any place in your service.</span></span> <span data-ttu-id="e4d4a-205">Spolehlivé správce stavu zajišťuje, získejte odkaz na zpět.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-205">The Reliable State Manager ensures that you get a reference back.</span></span> <span data-ttu-id="e4d4a-206">Doporučujeme si uložit odkazy na spolehlivé kolekci instancí v člen třídy, proměnné nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-206">We don't recommended that you save references to reliable collection instances in class member variables or properties.</span></span> <span data-ttu-id="e4d4a-207">Musí dát zvláštní pozor zajistit, že je odkaz nastavený na instanci za všech okolností v průběhu životního cyklu služby.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-207">Special care must be taken to ensure that the reference is set to an instance at all times in the service lifecycle.</span></span> <span data-ttu-id="e4d4a-208">Spolehlivé správce stavu zpracuje tato práce pro uživatele a je optimalizovaný pro opakování návštěvách.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-208">The Reliable State Manager handles this work for you, and it's optimized for repeat visits.</span></span>

### <a name="transactional-and-asynchronous-operations"></a><span data-ttu-id="e4d4a-209">Transakční a asynchronní operace</span><span class="sxs-lookup"><span data-stu-id="e4d4a-209">Transactional and asynchronous operations</span></span>
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

<span data-ttu-id="e4d4a-210">Spolehlivé kolekce mají mnoho z operací, která jejich `System.Collections.Generic` a `System.Collections.Concurrent` svými protějšky, s výjimkou LINQ.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-210">Reliable Collections have many of the same operations that their `System.Collections.Generic` and `System.Collections.Concurrent` counterparts do, except LINQ.</span></span> <span data-ttu-id="e4d4a-211">Operace na spolehlivé kolekce jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-211">Operations on Reliable Collections are asynchronous.</span></span> <span data-ttu-id="e4d4a-212">Je to proto, že operace zápisu ke kolekcím, spolehlivé provádění vstupně-výstupních operací se budou replikovat a zachovat data na disk.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-212">This is because write operations with Reliable Collections perform I/O operations to replicate and persist data to disk.</span></span>

<span data-ttu-id="e4d4a-213">Spolehlivé operace kolekce jsou *transakcí*, takže můžete zachovat stav konzistentní napříč více spolehlivé kolekce a operace.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-213">Reliable Collection operations are *transactional*, so that you can keep state consistent across multiple Reliable Collections and operations.</span></span> <span data-ttu-id="e4d4a-214">Může například dequeue – pracovní položky z fronty spolehlivé, proveďte operaci na něm a výsledek uložit jako slovník spolehlivé, vše v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-214">For example, you may dequeue a work item from a Reliable Queue, perform an operation on it, and save the result in a Reliable Dictionary, all within a single transaction.</span></span> <span data-ttu-id="e4d4a-215">To je považován za atomická operace, a zaručuje, že buď celé operace proběhne úspěšně, nebo celou operaci vrátíte zpět.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-215">This is treated as an atomic operation, and it guarantees that either the entire operation will succeed or the entire operation will roll back.</span></span> <span data-ttu-id="e4d4a-216">Pokud dojde k chybě po dequeue položku, ale před uložením výsledek, celá transakce bude vrácena zpět a položka zůstane ve frontě pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-216">If an error occurs after you dequeue the item but before you save the result, the entire transaction is rolled back and the item remains in the queue for processing.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="e4d4a-217">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e4d4a-217">Run the application</span></span>
<span data-ttu-id="e4d4a-218">Nemůžeme se teď vrátit do *HelloWorld* aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-218">We now return to the *HelloWorld* application.</span></span> <span data-ttu-id="e4d4a-219">Teď můžete sestavit a nasazení služeb.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-219">You can now build and deploy your services.</span></span> <span data-ttu-id="e4d4a-220">Po stisknutí klávesy **F5**, vaše aplikace bude vytvořené a nasazené na místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-220">When you press **F5**, your application will be built and deployed to your local cluster.</span></span>

<span data-ttu-id="e4d4a-221">Po službu spustit, můžete zobrazit vygenerované události trasování událostí pro Windows (ETW) v **diagnostických událostí** okno.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-221">After the services start running, you can view the generated Event Tracing for Windows (ETW) events in a **Diagnostic Events** window.</span></span> <span data-ttu-id="e4d4a-222">Všimněte si, že zobrazených událostí z službu bezstavové a stavové služby v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-222">Note that the events displayed are from both the stateless service and the stateful service in the application.</span></span> <span data-ttu-id="e4d4a-223">Datový proud je možné pozastavit kliknutím **pozastavit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-223">You can pause the stream by clicking the **Pause** button.</span></span> <span data-ttu-id="e4d4a-224">Poté můžete prozkoumat podrobnosti zprávy rozbalením této zprávě.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-224">You can then examine the details of a message by expanding that message.</span></span>

> [!NOTE]
> <span data-ttu-id="e4d4a-225">Než spustíte aplikaci, ujistěte se, že máte místního vývojového clusteru se systémem.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-225">Before you run the application, make sure that you have a local development cluster running.</span></span> <span data-ttu-id="e4d4a-226">Podívejte se [Příručka Začínáme](service-fabric-get-started.md) informace o nastavení vašeho místního prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4d4a-226">Check out the [getting started guide](service-fabric-get-started.md) for information on setting up your local environment.</span></span>
> 
> 

![Zobrazení diagnostických událostí v sadě Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a><span data-ttu-id="e4d4a-228">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4d4a-228">Next steps</span></span>
[<span data-ttu-id="e4d4a-229">Ladění aplikace Service Fabric v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4d4a-229">Debug your Service Fabric application in Visual Studio</span></span>](service-fabric-debugging-your-application.md)

[<span data-ttu-id="e4d4a-230">Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN</span><span class="sxs-lookup"><span data-stu-id="e4d4a-230">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>](service-fabric-reliable-services-communication-webapi.md)

[<span data-ttu-id="e4d4a-231">Další informace o spolehlivé kolekce</span><span class="sxs-lookup"><span data-stu-id="e4d4a-231">Learn more about Reliable Collections</span></span>](service-fabric-reliable-services-reliable-collections.md)

[<span data-ttu-id="e4d4a-232">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="e4d4a-232">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[<span data-ttu-id="e4d4a-233">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="e4d4a-233">Application upgrade</span></span>](service-fabric-application-upgrade.md)

[<span data-ttu-id="e4d4a-234">Referenční informace pro vývojáře pro spolehlivé služby</span><span class="sxs-lookup"><span data-stu-id="e4d4a-234">Developer reference for Reliable Services</span></span>](https://msdn.microsoft.com/library/azure/dn706529.aspx)

