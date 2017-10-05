---
title: "Vytvoření vaší první na základě objektu actor Azure mikroslužbu v jazyce C# | Microsoft Docs"
description: "Tento kurz vás provede kroky při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="76162-103">Začínáme s Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="76162-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="76162-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="76162-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="76162-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="76162-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="76162-106">Tento článek vysvětluje základy Azure Service Fabric Reliable Actors a provede vás vytváření, ladění a nasazování jednoduchou aplikaci spolehlivé objektu Actor v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76162-106">This article explains the basics of Azure Service Fabric Reliable Actors and walks you through creating, debugging, and deploying a simple Reliable Actor application in Visual Studio.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="76162-107">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="76162-107">Installation and setup</span></span>
<span data-ttu-id="76162-108">Než začnete, ujistěte se, abyste měli vývojového prostředí Service Fabric na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="76162-108">Before you start, ensure that you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="76162-109">Pokud potřebujete nastavit tak, najdete podrobné pokyny na [postup nastavení vývojového prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="76162-109">If you need to set it up, see detailed instructions on [how to set up the development environment](service-fabric-get-started.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="76162-110">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="76162-110">Basic concepts</span></span>
<span data-ttu-id="76162-111">Začít s Reliable Actors, potřebujete jenom pochopit několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="76162-111">To get started with Reliable Actors, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="76162-112">**Služby objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="76162-112">**Actor service**.</span></span> <span data-ttu-id="76162-113">Spolehlivé služby, které můžou být nasazené v Service Fabric infrastruktury jsou součástí Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="76162-113">Reliable Actors are packaged in Reliable Services that can be deployed in the Service Fabric infrastructure.</span></span> <span data-ttu-id="76162-114">Instance objektu actor aktivují v instanci služby s názvem.</span><span class="sxs-lookup"><span data-stu-id="76162-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="76162-115">**Registrace objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="76162-115">**Actor registration**.</span></span> <span data-ttu-id="76162-116">Jako se službami Reliable Services spolehlivé objektu Actor služby musí být registrováno v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="76162-116">As with Reliable Services, a Reliable Actor service needs to be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="76162-117">Typ objektu actor kromě toho musí být registrováno s modulem runtime objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="76162-117">In addition, the actor type needs to be registered with the Actor runtime.</span></span>
* <span data-ttu-id="76162-118">**Rozhraní objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="76162-118">**Actor interface**.</span></span> <span data-ttu-id="76162-119">Rozhraní objektu actor se používá k definování silného typu veřejné rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="76162-119">The actor interface is used to define a strongly typed public interface of an actor.</span></span> <span data-ttu-id="76162-120">Rozhraní objektu actor v terminologii modelu objektu Actor spolehlivé, definuje typy zprávy, které můžete porozumět objektu actor a proces.</span><span class="sxs-lookup"><span data-stu-id="76162-120">In the Reliable Actor model terminology, the actor interface defines the types of messages that the actor can understand and process.</span></span> <span data-ttu-id="76162-121">Rozhraní objektu actor slouží ostatní aktéři a klientské aplikace "Odeslat" (asynchronně) zpráv do objektu actor.</span><span class="sxs-lookup"><span data-stu-id="76162-121">The actor interface is used by other actors and client applications to "send" (asynchronously) messages to the actor.</span></span> <span data-ttu-id="76162-122">Reliable Actors můžete implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="76162-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="76162-123">**Třída ActorProxy**.</span><span class="sxs-lookup"><span data-stu-id="76162-123">**ActorProxy class**.</span></span> <span data-ttu-id="76162-124">Třída ActorProxy se používá klientskými aplikacemi volat metody vystavenou přes rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="76162-124">The ActorProxy class is used by client applications to invoke the methods exposed through the actor interface.</span></span> <span data-ttu-id="76162-125">Třída ActorProxy poskytuje dvě důležité funkce:</span><span class="sxs-lookup"><span data-stu-id="76162-125">The ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="76162-126">Překlad názvů: je možné najít objektu actor v clusteru (Najít uzlu clusteru, který je hostitelem).</span><span class="sxs-lookup"><span data-stu-id="76162-126">Name resolution: It is able to locate the actor in the cluster (find the node of the cluster where it is hosted).</span></span>
  * <span data-ttu-id="76162-127">Zpracování selhání: mohou zkuste volání metod a znovu přeložit umístění objektu actor po, například selhání vyžadující objektu actor pro přemístit do jiného uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="76162-127">Failure handling: It can retry method invocations and re-resolve the actor location after, for example, a failure that requires the actor to be relocated to another node in the cluster.</span></span>

<span data-ttu-id="76162-128">Následující pravidla, které se týkají objektu actor rozhraní jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="76162-128">The following rules that pertain to actor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="76162-129">Metody rozhraní objektu actor nemohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="76162-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="76162-130">Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="76162-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="76162-131">Obecná rozhraní nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="76162-131">Generic interfaces are not supported.</span></span>

## <a name="create-a-new-project-in-visual-studio"></a><span data-ttu-id="76162-132">Vytvořte nový projekt v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76162-132">Create a new project in Visual Studio</span></span>
<span data-ttu-id="76162-133">Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="76162-133">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project:</span></span>

![Service Fabric nástrojů pro Visual Studio – nový projekt][1]

<span data-ttu-id="76162-135">V dialogovém okně Další můžete si zvolit typ projektu, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="76162-135">In the next dialog box, you can choose the type of project that you want to create.</span></span>

![Šablony projektů Service Fabric][5]

<span data-ttu-id="76162-137">Pro projekt HelloWorld použijeme služby Service Fabric Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="76162-137">For the HelloWorld project, let's use the Service Fabric Reliable Actors service.</span></span>

<span data-ttu-id="76162-138">Po vytvoření řešení, byste měli vidět následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="76162-138">After you have created the solution, you should see the following structure:</span></span>

![Struktura projektu Service Fabric][2]

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="76162-140">Spolehlivé aktéři základních stavebních bloků</span><span class="sxs-lookup"><span data-stu-id="76162-140">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="76162-141">Typické Reliable Actors řešení se skládá ze tří projektů:</span><span class="sxs-lookup"><span data-stu-id="76162-141">A typical Reliable Actors solution is composed of three projects:</span></span>

* <span data-ttu-id="76162-142">**Projekt aplikace (MyActorApplication)**.</span><span class="sxs-lookup"><span data-stu-id="76162-142">**The application project (MyActorApplication)**.</span></span> <span data-ttu-id="76162-143">Toto je projekt, který balíčky všechny společně služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="76162-143">This is the project that packages all of the services together for deployment.</span></span> <span data-ttu-id="76162-144">Obsahuje *ApplicationManifest.xml* a skriptů prostředí PowerShell pro správu aplikace.</span><span class="sxs-lookup"><span data-stu-id="76162-144">It contains the *ApplicationManifest.xml* and PowerShell scripts for managing the application.</span></span>
* <span data-ttu-id="76162-145">**Rozhraní projektu (MyActor.Interfaces)**.</span><span class="sxs-lookup"><span data-stu-id="76162-145">**The interface project (MyActor.Interfaces)**.</span></span> <span data-ttu-id="76162-146">Toto je projekt, který obsahuje definici rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="76162-146">This is the project that contains the interface definition for the actor.</span></span> <span data-ttu-id="76162-147">V projektu MyActor.Interfaces můžete definovat rozhraní, která se použije aktéři v řešení.</span><span class="sxs-lookup"><span data-stu-id="76162-147">In the MyActor.Interfaces project, you can define the interfaces that will be used by the actors in the solution.</span></span> <span data-ttu-id="76162-148">Lze definovat vašich rozhraní objektu actor v jakékoli projektu s libovolným názvem, ale rozhraní definuje kontrakt objektu actor, který sdílí implementace objektu actor a klienti volání objektu actor, proto obvykle má smysl definovat v sestavení, které jsou oddělené od objektu actor implementaci a může být sdílen více další projekty.</span><span class="sxs-lookup"><span data-stu-id="76162-148">Your actor interfaces can be defined in any project with any name, however the interface defines the actor contract that is shared by the actor implementation and the clients calling the actor, so it typically makes sense to define it in an assembly that is separate from the actor implementation and can be shared by multiple other projects.</span></span>

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* <span data-ttu-id="76162-149">**Projekt služby objektu actor (MyActor)**.</span><span class="sxs-lookup"><span data-stu-id="76162-149">**The actor service project (MyActor)**.</span></span> <span data-ttu-id="76162-150">Toto je používá k definování služba Service Fabric, která bude hostitelem objektu actor projektu.</span><span class="sxs-lookup"><span data-stu-id="76162-150">This is the project used to define the Service Fabric service that is going to host the actor.</span></span> <span data-ttu-id="76162-151">Obsahuje implementace objektu actor.</span><span class="sxs-lookup"><span data-stu-id="76162-151">It contains the implementation of the actor.</span></span> <span data-ttu-id="76162-152">Implementace objektu actor je třída, která je odvozena ze základního typu `Actor` a implementuje alespoň jedno rozhraní, která jsou definována v MyActor.Interfaces projektu.</span><span class="sxs-lookup"><span data-stu-id="76162-152">An actor implementation is a class that derives from the base type `Actor` and implements the interface(s) that are defined in the MyActor.Interfaces project.</span></span> <span data-ttu-id="76162-153">Třídu objektu actor musí také implementovat konstruktor, který přijímá `ActorService` instance a `ActorId` a předává je základní `Actor` třídy.</span><span class="sxs-lookup"><span data-stu-id="76162-153">An actor class must also implement a constructor that accepts an `ActorService` instance and an `ActorId` and passes them to the base `Actor` class.</span></span> <span data-ttu-id="76162-154">To umožňuje vkládání závislostí konstruktor platformy závislostí.</span><span class="sxs-lookup"><span data-stu-id="76162-154">This allows for constructor dependency injection of platform dependencies.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

<span data-ttu-id="76162-155">Služby objektu actor musí být zaregistrován s typem služby v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="76162-155">The actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="76162-156">V pořadí pro službu objektu Actor ke spuštění vaše instance objektu actor musí být typu vašeho objektu actor také zaregistrován u služby objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="76162-156">In order for the Actor Service to run your actor instances, your actor type must also be registered with the Actor Service.</span></span> <span data-ttu-id="76162-157">`ActorRuntime` Metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="76162-157">The `ActorRuntime` registration method performs this work for actors.</span></span>

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

<span data-ttu-id="76162-158">Pokud spustíte z nový projekt v sadě Visual Studio a máte jenom jednu definici objektu actor, registrace je zahrnuta ve výchozím nastavení v kódu, který generuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76162-158">If you start from a new project in Visual Studio and you have only one actor definition, the registration is included by default in the code that Visual Studio generates.</span></span> <span data-ttu-id="76162-159">Pokud definujete jiných aktéři ve službě, budete muset přidat objektu actor registraci pomocí:</span><span class="sxs-lookup"><span data-stu-id="76162-159">If you define other actors in the service, you need to add the actor registration by using:</span></span>

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> <span data-ttu-id="76162-160">Modul runtime Service Fabric aktéři vysílá některé [události a čítače výkonu související s metody objektu actor](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="76162-160">The Service Fabric Actors runtime emits some [events and performance counters related to actor methods](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span></span> <span data-ttu-id="76162-161">Jsou užitečné v Diagnostika a sledování výkonu.</span><span class="sxs-lookup"><span data-stu-id="76162-161">They are useful in diagnostics and performance monitoring.</span></span>
> 
> 

## <a name="debugging"></a><span data-ttu-id="76162-162">Ladění</span><span class="sxs-lookup"><span data-stu-id="76162-162">Debugging</span></span>
<span data-ttu-id="76162-163">Service Fabric tools pro Visual Studio podporují ladění na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="76162-163">The Service Fabric tools for Visual Studio support debugging on your local machine.</span></span> <span data-ttu-id="76162-164">Můžete začít relaci ladění pomocí stiskne klávesu F5.</span><span class="sxs-lookup"><span data-stu-id="76162-164">You can start a debugging session by hitting the F5 key.</span></span> <span data-ttu-id="76162-165">Visual Studio vytvoří (v případě potřeby) balíčky.</span><span class="sxs-lookup"><span data-stu-id="76162-165">Visual Studio builds (if necessary) packages.</span></span> <span data-ttu-id="76162-166">Také nasadí aplikaci na místní cluster Service Fabric a připojí ladicí program.</span><span class="sxs-lookup"><span data-stu-id="76162-166">It also deploys the application on the local Service Fabric cluster and attaches the debugger.</span></span>

<span data-ttu-id="76162-167">Během procesu nasazení můžete sledovat průběh v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="76162-167">During the deployment process, you can see the progress in the **Output** window.</span></span>

![Service Fabric ladicích výstup – okno][3]

## <a name="next-steps"></a><span data-ttu-id="76162-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76162-169">Next steps</span></span>
<span data-ttu-id="76162-170">Další informace o [jak Reliable Actors pomocí platformy Service Fabric](service-fabric-reliable-actors-platform.md).</span><span class="sxs-lookup"><span data-stu-id="76162-170">Learn more about [how Reliable Actors use the Service Fabric platform](service-fabric-reliable-actors-platform.md).</span></span>

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
