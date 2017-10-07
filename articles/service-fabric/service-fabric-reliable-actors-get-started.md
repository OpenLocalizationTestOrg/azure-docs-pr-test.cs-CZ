---
title: "aaaCreate vaše první na základě objektu actor Azure mikroslužbu v jazyce C# | Microsoft Docs"
description: "Tento kurz vás provede kroky hello při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
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
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="ca1c2-103">Začínáme s Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="ca1c2-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca1c2-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="ca1c2-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="ca1c2-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="ca1c2-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="ca1c2-106">Tento článek vysvětluje základy hello Azure Service Fabric Reliable Actors a provede vás vytváření, ladění a nasazování jednoduchou aplikaci spolehlivé objektu Actor v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating, debugging, and deploying a simple Reliable Actor application in Visual Studio.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="ca1c2-107">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="ca1c2-107">Installation and setup</span></span>
<span data-ttu-id="ca1c2-108">Než začnete, ujistěte se, zda máte na počítači nastavit hello Service Fabric vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-108">Before you start, ensure that you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="ca1c2-109">Pokud potřebujete tooset ho, najdete podrobné pokyny na [jak tooset hello vývojového prostředí](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ca1c2-109">If you need tooset it up, see detailed instructions on [how tooset up hello development environment](service-fabric-get-started.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="ca1c2-110">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="ca1c2-110">Basic concepts</span></span>
<span data-ttu-id="ca1c2-111">tooget začít s Reliable Actors, můžete pouze potřebovat toounderstand několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="ca1c2-112">**Služby objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-112">**Actor service**.</span></span> <span data-ttu-id="ca1c2-113">Spolehlivé služby, které můžou být nasazené v infrastruktuře Service Fabric hello jsou součástí Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="ca1c2-114">Instance objektu actor aktivují v instanci služby s názvem.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="ca1c2-115">**Registrace objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-115">**Actor registration**.</span></span> <span data-ttu-id="ca1c2-116">Jako služba spolehlivé objektu Actor potřebuje se službami Reliable Services toobe zaregistrována modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="ca1c2-117">Typ objektu actor hello kromě toho musí toobe zaregistrována hello objektu Actor runtime.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="ca1c2-118">**Rozhraní objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-118">**Actor interface**.</span></span> <span data-ttu-id="ca1c2-119">rozhraní objektu actor Hello je použité toodefine silného typu veřejné rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="ca1c2-120">Rozhraní objektu actor hello v hello terminologie modelu objektu Actor spolehlivé, definuje hello typy zpráv, které hello objektu actor můžete pochopit a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="ca1c2-121">rozhraní objektu actor Hello používá jiné aktéři a klientské aplikace příliš "Odeslat" (asynchronně) objektu actor toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="ca1c2-122">Reliable Actors můžete implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="ca1c2-123">**Třída ActorProxy**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-123">**ActorProxy class**.</span></span> <span data-ttu-id="ca1c2-124">používá Hello ActorProxy třídy klienta aplikace tooinvoke hello metody vystavenou přes rozhraní objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="ca1c2-125">Hello ActorProxy třída poskytuje dvě důležité funkce:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="ca1c2-126">Překlad názvů: je možné toolocate hello objektu actor v clusteru hello (Najít hello uzel hello clusteru, který je hostitelem).</span><span class="sxs-lookup"><span data-stu-id="ca1c2-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="ca1c2-127">Zpracování selhání: můžete opakujte volání metod a znovu přeložit umístění objektu actor hello po, například selhání, který vyžaduje hello objektu actor toobe přemístění tooanother uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="ca1c2-128">Hello následující pravidla, které se týkají tooactor rozhraní jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="ca1c2-129">Metody rozhraní objektu actor nemohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="ca1c2-130">Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="ca1c2-131">Obecná rozhraní nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-131">Generic interfaces are not supported.</span></span>

## <a name="create-a-new-project-in-visual-studio"></a><span data-ttu-id="ca1c2-132">Vytvořte nový projekt v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca1c2-132">Create a new project in Visual Studio</span></span>
<span data-ttu-id="ca1c2-133">Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-133">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project:</span></span>

![Service Fabric nástrojů pro Visual Studio – nový projekt][1]

<span data-ttu-id="ca1c2-135">V hello další dialogové okno můžete si zvolit hello typ projektu, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-135">In hello next dialog box, you can choose hello type of project that you want toocreate.</span></span>

![Šablony projektů Service Fabric][5]

<span data-ttu-id="ca1c2-137">Pro projekt Hello World hello použijeme služby Service Fabric Reliable Actors hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-137">For hello HelloWorld project, let's use hello Service Fabric Reliable Actors service.</span></span>

<span data-ttu-id="ca1c2-138">Po vytvoření hello řešení, měli byste vidět hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-138">After you have created hello solution, you should see hello following structure:</span></span>

![Struktura projektu Service Fabric][2]

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="ca1c2-140">Spolehlivé aktéři základních stavebních bloků</span><span class="sxs-lookup"><span data-stu-id="ca1c2-140">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="ca1c2-141">Typické Reliable Actors řešení se skládá ze tří projektů:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-141">A typical Reliable Actors solution is composed of three projects:</span></span>

* <span data-ttu-id="ca1c2-142">**projekt aplikace Hello (MyActorApplication)**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-142">**hello application project (MyActorApplication)**.</span></span> <span data-ttu-id="ca1c2-143">Toto je hello projekt, který balíčky všechny společně hello služby pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-143">This is hello project that packages all of hello services together for deployment.</span></span> <span data-ttu-id="ca1c2-144">Obsahuje hello *ApplicationManifest.xml* a skriptů prostředí PowerShell pro správu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-144">It contains hello *ApplicationManifest.xml* and PowerShell scripts for managing hello application.</span></span>
* <span data-ttu-id="ca1c2-145">**Hello rozhraní projektu (MyActor.Interfaces)**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-145">**hello interface project (MyActor.Interfaces)**.</span></span> <span data-ttu-id="ca1c2-146">Toto je hello projekt, který obsahuje definici rozhraní hello objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-146">This is hello project that contains hello interface definition for hello actor.</span></span> <span data-ttu-id="ca1c2-147">V projektu MyActor.Interfaces hello můžete definovat hello rozhraní, které se použijí hello aktéři v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-147">In hello MyActor.Interfaces project, you can define hello interfaces that will be used by hello actors in hello solution.</span></span> <span data-ttu-id="ca1c2-148">Vašich rozhraní objektu actor lze definovat v jakékoli projektu s libovolným názvem, ale hello rozhraní definuje kontrakt objektu actor hello, který sdílí implementace objektu actor hello a hello klientům volání objektu actor hello, takže obvykle má smysl toodefine v sestavení, které je oddělené od implementace objektu actor hello a může být sdílen více další projekty.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-148">Your actor interfaces can be defined in any project with any name, however hello interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in an assembly that is separate from hello actor implementation and can be shared by multiple other projects.</span></span>

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* <span data-ttu-id="ca1c2-149">**projekt služby objektu actor Hello (MyActor)**.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-149">**hello actor service project (MyActor)**.</span></span> <span data-ttu-id="ca1c2-150">Toto je hello projektu používá toodefine hello Service Fabric služba, která se má toohost hello objektu actor.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-150">This is hello project used toodefine hello Service Fabric service that is going toohost hello actor.</span></span> <span data-ttu-id="ca1c2-151">Obsahuje hello implementace objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-151">It contains hello implementation of hello actor.</span></span> <span data-ttu-id="ca1c2-152">Implementace objektu actor je třída, která je odvozena od základního typu hello `Actor` a implementuje hello alespoň jedno rozhraní, která jsou definována v projektu MyActor.Interfaces hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-152">An actor implementation is a class that derives from hello base type `Actor` and implements hello interface(s) that are defined in hello MyActor.Interfaces project.</span></span> <span data-ttu-id="ca1c2-153">Třídu objektu actor musí také implementovat konstruktor, který přijímá `ActorService` instance a `ActorId` a předá je toohello základní `Actor` třídy.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-153">An actor class must also implement a constructor that accepts an `ActorService` instance and an `ActorId` and passes them toohello base `Actor` class.</span></span> <span data-ttu-id="ca1c2-154">To umožňuje vkládání závislostí konstruktor platformy závislostí.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-154">This allows for constructor dependency injection of platform dependencies.</span></span>

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

<span data-ttu-id="ca1c2-155">služby objektu actor Hello musí být zaregistrován s typem služby v modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-155">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="ca1c2-156">V pořadí pro hello služby objektu Actor toorun vaše instance objektu actor, vašeho typu objektu actor musí být zaregistrovaná taky hello služby objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-156">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="ca1c2-157">Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-157">hello `ActorRuntime` registration method performs this work for actors.</span></span>

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

<span data-ttu-id="ca1c2-158">Pokud spustíte z nový projekt v sadě Visual Studio a máte jenom jednu definici objektu actor, hello registrace je zahrnuta ve výchozím nastavení v hello kód, který generuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-158">If you start from a new project in Visual Studio and you have only one actor definition, hello registration is included by default in hello code that Visual Studio generates.</span></span> <span data-ttu-id="ca1c2-159">Pokud definujete jiných aktéři v hello služby, je třeba tooadd hello objektu actor registrace pomocí:</span><span class="sxs-lookup"><span data-stu-id="ca1c2-159">If you define other actors in hello service, you need tooadd hello actor registration by using:</span></span>

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> <span data-ttu-id="ca1c2-160">Hello modulu runtime Service Fabric aktéři vysílá některé [události a čítače výkonu související metody tooactor](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="ca1c2-160">hello Service Fabric Actors runtime emits some [events and performance counters related tooactor methods](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span></span> <span data-ttu-id="ca1c2-161">Jsou užitečné v Diagnostika a sledování výkonu.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-161">They are useful in diagnostics and performance monitoring.</span></span>
> 
> 

## <a name="debugging"></a><span data-ttu-id="ca1c2-162">Ladění</span><span class="sxs-lookup"><span data-stu-id="ca1c2-162">Debugging</span></span>
<span data-ttu-id="ca1c2-163">Hello Service Fabric tools pro Visual Studio podporují ladění na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-163">hello Service Fabric tools for Visual Studio support debugging on your local machine.</span></span> <span data-ttu-id="ca1c2-164">Můžete začít relaci ladění pomocí hello stiskne klávesu F5.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-164">You can start a debugging session by hitting hello F5 key.</span></span> <span data-ttu-id="ca1c2-165">Visual Studio vytvoří (v případě potřeby) balíčky.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-165">Visual Studio builds (if necessary) packages.</span></span> <span data-ttu-id="ca1c2-166">Také nasadí aplikaci hello na místní cluster Service Fabric hello a připojí ladicí program hello.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-166">It also deploys hello application on hello local Service Fabric cluster and attaches hello debugger.</span></span>

<span data-ttu-id="ca1c2-167">Během procesu nasazení hello, uvidíte průběh hello hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="ca1c2-167">During hello deployment process, you can see hello progress in hello **Output** window.</span></span>

![Service Fabric ladicích výstup – okno][3]

## <a name="next-steps"></a><span data-ttu-id="ca1c2-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca1c2-169">Next steps</span></span>
<span data-ttu-id="ca1c2-170">Další informace o [jak Reliable Actors pomocí platformy Service Fabric hello](service-fabric-reliable-actors-platform.md).</span><span class="sxs-lookup"><span data-stu-id="ca1c2-170">Learn more about [how Reliable Actors use hello Service Fabric platform](service-fabric-reliable-actors-platform.md).</span></span>

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
