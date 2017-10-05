---
title: Reliable Actors v Service Fabric | Microsoft Docs
description: "Popisuje, jak jsou vrstvu na spolehlivé služby Reliable Actors a pomocí funkcí platformy Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: 0a12da52b6e74c721cd25f89e7cde3c07153a396
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a><span data-ttu-id="5950e-103">Jak objekty Reliable Actors využívají platformu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5950e-103">How Reliable Actors use the Service Fabric platform</span></span>
<span data-ttu-id="5950e-104">Tento článek vysvětluje, jak fungují Reliable Actors na platformě Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5950e-104">This article explains how Reliable Actors work on the Azure Service Fabric platform.</span></span> <span data-ttu-id="5950e-105">Spustit v rozhraní, které je hostován v implementaci stavové spolehlivé služby Reliable Actors názvem *služby objektu actor*.</span><span class="sxs-lookup"><span data-stu-id="5950e-105">Reliable Actors run in a framework that is hosted in an implementation of a stateful reliable service called the *actor service*.</span></span> <span data-ttu-id="5950e-106">Služby objektu actor obsahuje všechny komponenty potřebné ke správě životního cyklu a odeslání pro vaše aktéři zpráva:</span><span class="sxs-lookup"><span data-stu-id="5950e-106">The actor service contains all the components necessary to manage the lifecycle and message dispatching for your actors:</span></span>

* <span data-ttu-id="5950e-107">Modul Runtime objektu Actor spravuje životní cyklus, uvolňování paměti a vynucuje jednovláknové přístup.</span><span class="sxs-lookup"><span data-stu-id="5950e-107">The Actor Runtime manages lifecycle, garbage collection, and enforces single-threaded access.</span></span>
* <span data-ttu-id="5950e-108">Naslouchací proces vzdálené komunikace služby objektu actor přijímá volání aktéři vzdáleného přístupu a je odešle do dispečera směrovat na příslušné objektu actor instanci.</span><span class="sxs-lookup"><span data-stu-id="5950e-108">An actor service remoting listener accepts remote access calls to actors and sends them to a dispatcher to route to the appropriate actor instance.</span></span>
* <span data-ttu-id="5950e-109">Zprostředkovatel stavu objektu Actor zabalí zprostředkovatelů stavu (např. zprostředkovatele stavu spolehlivé kolekce) a poskytuje adaptér pro správu stavu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-109">The Actor State Provider wraps state providers (such as the Reliable Collections state provider) and provides an adapter for actor state management.</span></span>

<span data-ttu-id="5950e-110">Tyto součásti společně tvoří spolehlivé objektu Actor framework.</span><span class="sxs-lookup"><span data-stu-id="5950e-110">These components together form the Reliable Actor framework.</span></span>

## <a name="service-layering"></a><span data-ttu-id="5950e-111">Služba vrstvení</span><span class="sxs-lookup"><span data-stu-id="5950e-111">Service layering</span></span>
<span data-ttu-id="5950e-112">Protože samotné služby objektu actor je spolehlivá služba, všechny [aplikačního modelu](service-fabric-application-model.md), životního cyklu, [balení](service-fabric-package-apps.md), [nasazení](service-fabric-deploy-remove-applications.md), upgrade a škálování koncepty spolehlivé služby použít stejným způsobem jako do služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-112">Because the actor service itself is a reliable service, all the [application model](service-fabric-application-model.md), lifecycle, [packaging](service-fabric-package-apps.md), [deployment](service-fabric-deploy-remove-applications.md), upgrade, and scaling concepts of Reliable Services apply the same way to actor services.</span></span> 

![Rozvrstvení služby objektu actor][1]

<span data-ttu-id="5950e-114">Předchozí diagram znázorňuje vztah mezi architektury aplikace Service Fabric a uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="5950e-114">The preceding diagram shows the relationship between the Service Fabric application frameworks and user code.</span></span> <span data-ttu-id="5950e-115">Modré elementy reprezentují rozhraní spolehlivé služby, oranžová představuje rozhraní objektu Actor spolehlivé a zelená představuje uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="5950e-115">Blue elements represent the Reliable Services application framework, orange represents the Reliable Actor framework, and green represents user code.</span></span>

<span data-ttu-id="5950e-116">V spolehlivé služby, služby dědí `StatefulService` třídy.</span><span class="sxs-lookup"><span data-stu-id="5950e-116">In Reliable Services, your service inherits the `StatefulService` class.</span></span> <span data-ttu-id="5950e-117">Tato třída je sám odvozené od `StatefulServiceBase` (nebo `StatelessService` pro bezstavové služby).</span><span class="sxs-lookup"><span data-stu-id="5950e-117">This class is itself derived from `StatefulServiceBase` (or `StatelessService` for stateless services).</span></span> <span data-ttu-id="5950e-118">V Reliable Actors pomocí služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-118">In Reliable Actors, you use the actor service.</span></span> <span data-ttu-id="5950e-119">Je na různé implementace služby objektu actor `StatefulServiceBase` třídu, která implementuje vzor objektu actor, kde vaše aktéři spustit.</span><span class="sxs-lookup"><span data-stu-id="5950e-119">The actor service is a different implementation of the `StatefulServiceBase` class that implements the actor pattern where your actors run.</span></span> <span data-ttu-id="5950e-120">Protože je právě implementací samotné služby objektu actor `StatefulServiceBase`, můžete napsat vlastní službu, která je odvozena z `ActorService` a implementovat funkce úrovně služeb stejným způsobem jako při dědění `StatefulService`, jako například:</span><span class="sxs-lookup"><span data-stu-id="5950e-120">Because the actor service itself is just an implementation of `StatefulServiceBase`, you can write your own service that derives from `ActorService` and implement service-level features the same way you would when inheriting `StatefulService`, such as:</span></span>

* <span data-ttu-id="5950e-121">Služba zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="5950e-121">Service backup and restore.</span></span>
* <span data-ttu-id="5950e-122">Sdílené funkce pro všechny účastníky, například jistič.</span><span class="sxs-lookup"><span data-stu-id="5950e-122">Shared functionality for all actors, for example, a circuit breaker.</span></span>
* <span data-ttu-id="5950e-123">Vzdálená volání procedur v samotné služby objektu actor a v každé jednotlivé objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-123">Remote procedure calls on the actor service itself and on each individual actor.</span></span>

> [!NOTE]
> <span data-ttu-id="5950e-124">Stavové služby nejsou aktuálně podporované v jazyce Java nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="5950e-124">Stateful services are not currently supported in Java/Linux.</span></span>

### <a name="using-the-actor-service"></a><span data-ttu-id="5950e-125">Pomocí služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-125">Using the actor service</span></span>
<span data-ttu-id="5950e-126">Instance objektu actor mít přístup ke službě objektu actor, ve kterém běží.</span><span class="sxs-lookup"><span data-stu-id="5950e-126">Actor instances have access to the actor service in which they are running.</span></span> <span data-ttu-id="5950e-127">Prostřednictvím služby objektu actor instancí objektu actor prostřednictvím kódu programu získat kontext služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-127">Through the actor service, actor instances can programmatically obtain the service context.</span></span> <span data-ttu-id="5950e-128">Kontext služby má ID oddílu, název služby, název aplikace a další informace specifické pro platformu Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="5950e-128">The service context has the partition ID, service name, application name, and other Service Fabric platform-specific information:</span></span>

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


<span data-ttu-id="5950e-129">Podobně jako všechny spolehlivé služby musí být zaregistrován služby objektu actor s typem služby v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5950e-129">Like all Reliable Services, the actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="5950e-130">Služby objektu actor ke spuštění vaše instance objektu actor musí být typu vašeho objektu actor také zaregistrován u služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-130">For the actor service to run your actor instances, your actor type must also be registered with the actor service.</span></span> <span data-ttu-id="5950e-131">`ActorRuntime` Metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="5950e-131">The `ActorRuntime` registration method performs this work for actors.</span></span> <span data-ttu-id="5950e-132">V nejjednodušším případě zaregistrujete na typ vašeho objektu actor a služby objektu actor s výchozím nastavením se implicitně použije:</span><span class="sxs-lookup"><span data-stu-id="5950e-132">In the simplest case, you can just register your actor type, and the actor service with default settings will implicitly be used:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

<span data-ttu-id="5950e-133">Alternativně můžete lambda poskytované registrace metoda služby objektu actor vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="5950e-133">Alternatively, you can use a lambda provided by the registration method to construct the actor service yourself.</span></span> <span data-ttu-id="5950e-134">Můžete pak nakonfigurovat služby objektu actor a explicitně vytvořit vaše objektu actor instance, kde můžete vložit závislosti na vaší objektu actor prostřednictvím jeho konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="5950e-134">You can then configure the actor service and explicitly construct your actor instances, where you can inject dependencies to your actor through its constructor:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a><span data-ttu-id="5950e-135">Metody služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-135">Actor service methods</span></span>
<span data-ttu-id="5950e-136">Implementuje služby objektu Actor `IActorService` (C#) nebo `ActorService` (Java), který naopak implementuje `IService` (C#) nebo `Service` (Java).</span><span class="sxs-lookup"><span data-stu-id="5950e-136">The Actor service implements `IActorService` (C#) or `ActorService` (Java), which in turn implements `IService` (C#) or `Service` (Java).</span></span> <span data-ttu-id="5950e-137">Toto je rozhraní používá vzdálenou komunikaci spolehlivé služby, které umožňuje vzdálené volání procedur u metod služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-137">This is the interface used by Reliable Services remoting, which allows remote procedure calls on service methods.</span></span> <span data-ttu-id="5950e-138">Obsahuje metody úrovni služby, které je možné volat vzdáleně přes vzdálenou komunikaci služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-138">It contains service-level methods that can be called remotely via service remoting.</span></span>

#### <a name="enumerating-actors"></a><span data-ttu-id="5950e-139">Vytváření výčtu actors</span><span class="sxs-lookup"><span data-stu-id="5950e-139">Enumerating actors</span></span>
<span data-ttu-id="5950e-140">Služby objektu actor umožňuje klientovi výčet metadata o aktéři, která je hostitelem služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-140">The actor service allows a client to enumerate metadata about the actors that the service is hosting.</span></span> <span data-ttu-id="5950e-141">Protože služby objektu actor oddílů stavové služby, výčet se provádí na oddíl.</span><span class="sxs-lookup"><span data-stu-id="5950e-141">Because the actor service is a partitioned stateful service, enumeration is performed per partition.</span></span> <span data-ttu-id="5950e-142">Protože každý oddíl může obsahovat mnoho aktéři, výčtu se vrátí jako sadu stránkových výsledků.</span><span class="sxs-lookup"><span data-stu-id="5950e-142">Because each partition might contain many actors, the enumeration is returned as a set of paged results.</span></span> <span data-ttu-id="5950e-143">Na stránkách jsou smyčce přes, dokud se číst všechny stránky.</span><span class="sxs-lookup"><span data-stu-id="5950e-143">The pages are looped over until all pages are read.</span></span> <span data-ttu-id="5950e-144">Následující příklad ukazuje, jak vytvořit seznam všech active aktéři v jednom oddílu služby objektu actor:</span><span class="sxs-lookup"><span data-stu-id="5950e-144">The following example shows how to create a list of all active actors in one partition of an actor service:</span></span>

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a><span data-ttu-id="5950e-145">Odstranění actors</span><span class="sxs-lookup"><span data-stu-id="5950e-145">Deleting actors</span></span>
<span data-ttu-id="5950e-146">Služby objektu actor také poskytuje funkce pro odstranění aktéři:</span><span class="sxs-lookup"><span data-stu-id="5950e-146">The actor service also provides a function for deleting actors:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="5950e-147">Další informace o odstranění aktéři a jejich stavu najdete v tématu [objektu actor životního cyklu dokumentaci](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="5950e-147">For more information on deleting actors and their state, see the [actor lifecycle documentation](service-fabric-reliable-actors-lifecycle.md).</span></span>

### <a name="custom-actor-service"></a><span data-ttu-id="5950e-148">Služba vlastní objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-148">Custom actor service</span></span>
<span data-ttu-id="5950e-149">Pomocí lambda registrace objektu actor můžete zaregistrovat vlastní vlastní objektu actor služba, která je odvozena z `ActorService` (C#) a `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="5950e-149">By using the actor registration lambda, you can register your own custom actor service that derives from `ActorService` (C#) and `FabricActorService` (Java).</span></span> <span data-ttu-id="5950e-150">V rámci této služby objektu actor vlastní můžete implementovat vlastní úroveň služby funkce napsáním třídu služby, který dědí `ActorService` (C#) nebo `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="5950e-150">In this custom actor service, you can implement your own service-level functionality by writing a service class that inherits `ActorService` (C#) or `FabricActorService` (Java).</span></span> <span data-ttu-id="5950e-151">Služby objektu actor vlastní dědí všechny funkcionalita modulu runtime objektu actor z `ActorService` (C#) nebo `FabricActorService` (Java) a lze použít k implementaci vlastních metod služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-151">A custom actor service inherits all the actor runtime functionality from `ActorService` (C#) or `FabricActorService` (Java) and can be used to implement your own service methods.</span></span>

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a><span data-ttu-id="5950e-152">Implementace objektu actor zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="5950e-152">Implementing actor backup and restore</span></span>
 <span data-ttu-id="5950e-153">V následujícím příkladu služby objektu actor vlastní zpřístupní metodu zálohování dat objektu actor využívat výhod naslouchacího procesu vzdálené komunikace, která je již v `ActorService`:</span><span class="sxs-lookup"><span data-stu-id="5950e-153">In the following example, the custom actor service exposes a method to back up actor data by taking advantage of the remoting listener already present in `ActorService`:</span></span>

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


<span data-ttu-id="5950e-154">V tomto příkladu `IMyActorService` je Vzdálená komunikace kontrakt, který implementuje `IService` (C#) a `Service` (Java) a pak je implementováno modulem `MyActorService`.</span><span class="sxs-lookup"><span data-stu-id="5950e-154">In this example, `IMyActorService` is a remoting contract that implements `IService` (C#) and `Service` (Java), and is then implemented by `MyActorService`.</span></span> <span data-ttu-id="5950e-155">Přidáním této smlouvy vzdálenou komunikaci, metody na `IMyActorService` jsou nyní k dispozici také ke klientovi vytvořením proxy vzdálenou komunikaci prostřednictvím `ActorServiceProxy`:</span><span class="sxs-lookup"><span data-stu-id="5950e-155">By adding this remoting contract, methods on `IMyActorService` are now also available to a client by creating a remoting proxy via `ActorServiceProxy`:</span></span>

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a><span data-ttu-id="5950e-156">Aplikační model</span><span class="sxs-lookup"><span data-stu-id="5950e-156">Application model</span></span>
<span data-ttu-id="5950e-157">Služby objektu actor jsou spolehlivé služby, takže aplikačního modelu je stejný.</span><span class="sxs-lookup"><span data-stu-id="5950e-157">Actor services are Reliable Services, so the application model is the same.</span></span> <span data-ttu-id="5950e-158">Však nástroje sestavení objektu actor framework generovat některé soubory modelu aplikace za vás.</span><span class="sxs-lookup"><span data-stu-id="5950e-158">However, the actor framework build tools generate some of the application model files for you.</span></span>

### <a name="service-manifest"></a><span data-ttu-id="5950e-159">Manifest služby</span><span class="sxs-lookup"><span data-stu-id="5950e-159">Service manifest</span></span>
<span data-ttu-id="5950e-160">Nástroje sestavení objektu actor framework automaticky generovat obsah souboru ServiceManifest.xml služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-160">The actor framework build tools automatically generate the contents of your actor service's ServiceManifest.xml file.</span></span> <span data-ttu-id="5950e-161">Tento soubor obsahuje:</span><span class="sxs-lookup"><span data-stu-id="5950e-161">This file includes:</span></span>

* <span data-ttu-id="5950e-162">Typ služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-162">Actor service type.</span></span> <span data-ttu-id="5950e-163">Název typu je vytvořen na základě názvu objektu actor projektu.</span><span class="sxs-lookup"><span data-stu-id="5950e-163">The type name is generated based on your actor's project name.</span></span> <span data-ttu-id="5950e-164">Založená na atributu trvalost na vaše objektu actor, je HasPersistedState také nastavený příznak odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5950e-164">Based on the persistence attribute on your actor, the HasPersistedState flag is also set accordingly.</span></span>
* <span data-ttu-id="5950e-165">Balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="5950e-165">Code package.</span></span>
* <span data-ttu-id="5950e-166">Konfigurační balíček.</span><span class="sxs-lookup"><span data-stu-id="5950e-166">Config package.</span></span>
* <span data-ttu-id="5950e-167">Koncové body a prostředky.</span><span class="sxs-lookup"><span data-stu-id="5950e-167">Resources and endpoints.</span></span>

### <a name="application-manifest"></a><span data-ttu-id="5950e-168">Manifest aplikace</span><span class="sxs-lookup"><span data-stu-id="5950e-168">Application manifest</span></span>
<span data-ttu-id="5950e-169">Nástroje sestavení objektu actor framework automaticky vytvoří definici výchozí služby pro služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-169">The actor framework build tools automatically create a default service definition for your actor service.</span></span> <span data-ttu-id="5950e-170">Nástroje pro sestavení naplnit výchozí vlastnosti služby:</span><span class="sxs-lookup"><span data-stu-id="5950e-170">The build tools populate the default service properties:</span></span>

* <span data-ttu-id="5950e-171">Počet sady replik je určen podle atribut trvalost na vaše objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5950e-171">Replica set count is determined by the persistence attribute on your actor.</span></span> <span data-ttu-id="5950e-172">Pokaždé, když trvalost atribut na vaše objektu actor mění, počet sady replik v definici výchozí služby se resetuje odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5950e-172">Each time the persistence attribute on your actor is changed, the replica set count in the default service definition is reset accordingly.</span></span>
* <span data-ttu-id="5950e-173">Schéma oddílu a rozsah jsou nastaveny na Uniform Int64 s plný rozsah klíče Int64.</span><span class="sxs-lookup"><span data-stu-id="5950e-173">Partition scheme and range are set to Uniform Int64 with the full Int64 key range.</span></span>

## <a name="service-fabric-partition-concepts-for-actors"></a><span data-ttu-id="5950e-174">Koncepty oddílu Service Fabric actors</span><span class="sxs-lookup"><span data-stu-id="5950e-174">Service Fabric partition concepts for actors</span></span>
<span data-ttu-id="5950e-175">Služby objektu actor jsou oddílů stavové služby.</span><span class="sxs-lookup"><span data-stu-id="5950e-175">Actor services are partitioned stateful services.</span></span> <span data-ttu-id="5950e-176">Každý oddíl služby objektu actor obsahuje sadu aktéři.</span><span class="sxs-lookup"><span data-stu-id="5950e-176">Each partition of an actor service contains a set of actors.</span></span> <span data-ttu-id="5950e-177">Oddílů služby je automaticky distribuovaná přes více uzlů v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5950e-177">Service partitions are automatically distributed over multiple nodes in Service Fabric.</span></span> <span data-ttu-id="5950e-178">Instance objektu actor jsou distribuovány v důsledku.</span><span class="sxs-lookup"><span data-stu-id="5950e-178">Actor instances are distributed as a result.</span></span>

![Objekt actor vytváření oddílů a distribuci][5]

<span data-ttu-id="5950e-180">Spolehlivé služby můžete vytvořit s schémata jiný oddíl a oddíl rozsahy klíčů.</span><span class="sxs-lookup"><span data-stu-id="5950e-180">Reliable Services can be created with different partition schemes and partition key ranges.</span></span> <span data-ttu-id="5950e-181">Služby objektu actor používá schéma rozdělení oddílů Int64 s plný rozsah klíče Int64 k mapování aktéři na oddíly.</span><span class="sxs-lookup"><span data-stu-id="5950e-181">The actor service uses the Int64 partitioning scheme with the full Int64 key range to map actors to partitions.</span></span>

### <a name="actor-id"></a><span data-ttu-id="5950e-182">ID objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-182">Actor ID</span></span>
<span data-ttu-id="5950e-183">Každý objekt actor, který se vytvoří ve službě má jedinečné ID přidružený reprezentována `ActorId` třídy.</span><span class="sxs-lookup"><span data-stu-id="5950e-183">Each actor that's created in the service has a unique ID associated with it, represented by the `ActorId` class.</span></span> <span data-ttu-id="5950e-184">`ActorId`je neprůhledné hodnoty ID, které je možné pro rovnoměrné aktéři mezi oddílů služby tak, že generuje náhodné ID:</span><span class="sxs-lookup"><span data-stu-id="5950e-184">`ActorId` is an opaque ID value that can be used for uniform distribution of actors across the service partitions by generating random IDs:</span></span>

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


<span data-ttu-id="5950e-185">Každý `ActorId` se rozdělí na datovém typu Int64.</span><span class="sxs-lookup"><span data-stu-id="5950e-185">Every `ActorId` is hashed to an Int64.</span></span> <span data-ttu-id="5950e-186">Z tohoto důvodu služby objektu actor musí používat schéma rozdělení oddílů Int64 s plný rozsah klíče Int64.</span><span class="sxs-lookup"><span data-stu-id="5950e-186">This is why the actor service must use an Int64 partitioning scheme with the full Int64 key range.</span></span> <span data-ttu-id="5950e-187">Však můžete použít vlastní hodnoty ID pro `ActorID`, včetně GUID/UUID, řetězce a Int64s.</span><span class="sxs-lookup"><span data-stu-id="5950e-187">However, custom ID values can be used for an `ActorID`, including GUIDs/UUIDs, strings, and Int64s.</span></span>

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

<span data-ttu-id="5950e-188">Pokud používáte GUID/UUID a řetězce, hodnoty rozdělí na datovém typu Int64.</span><span class="sxs-lookup"><span data-stu-id="5950e-188">When you're using GUIDs/UUIDs and strings, the values are hashed to an Int64.</span></span> <span data-ttu-id="5950e-189">Ale když explicitně zadáváte Int64 pro `ActorId`, Int64 se mapování přímo na oddíl bez další algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="5950e-189">However, when you're explicitly providing an Int64 to an `ActorId`, the Int64 will map directly to a partition without further hashing.</span></span> <span data-ttu-id="5950e-190">Tento postup na ovládací prvek oddíl, který aktéři jsou umístěny v můžete použít.</span><span class="sxs-lookup"><span data-stu-id="5950e-190">You can use this technique to control which partition the actors are placed in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5950e-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5950e-191">Next steps</span></span>
* [<span data-ttu-id="5950e-192">Řízení stavu objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-192">Actor state management</span></span>](service-fabric-reliable-actors-state-management.md)
* [<span data-ttu-id="5950e-193">Kolekce paměti a životního cyklu objektu actor</span><span class="sxs-lookup"><span data-stu-id="5950e-193">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="5950e-194">Referenční dokumentace rozhraní API actors</span><span class="sxs-lookup"><span data-stu-id="5950e-194">Actors API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="5950e-195">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="5950e-195">.NET sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="5950e-196">Ukázkový kód Java</span><span class="sxs-lookup"><span data-stu-id="5950e-196">Java sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
