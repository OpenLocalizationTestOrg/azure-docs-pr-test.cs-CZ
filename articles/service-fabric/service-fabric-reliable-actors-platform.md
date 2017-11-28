---
title: "aaaReliable aktéři v Service Fabric | Microsoft Docs"
description: "Popisuje, jak jsou vrstvu na spolehlivé služby Reliable Actors a používat funkce hello platformy Service Fabric hello."
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
ms.openlocfilehash: ecffb54139f1171c7839b77fed0be60950881198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a><span data-ttu-id="09f9b-103">Jak používat Reliable Actors hello platformy Service Fabric</span><span class="sxs-lookup"><span data-stu-id="09f9b-103">How Reliable Actors use hello Service Fabric platform</span></span>
<span data-ttu-id="09f9b-104">Tento článek vysvětluje, jak fungují Reliable Actors na platformě Azure Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="09f9b-104">This article explains how Reliable Actors work on hello Azure Service Fabric platform.</span></span> <span data-ttu-id="09f9b-105">Reliable Actors spustit v rozhraní, které je hostován v implementaci stavová spolehlivá služba s názvem hello *služby objektu actor*.</span><span class="sxs-lookup"><span data-stu-id="09f9b-105">Reliable Actors run in a framework that is hosted in an implementation of a stateful reliable service called hello *actor service*.</span></span> <span data-ttu-id="09f9b-106">služby objektu actor Hello obsahuje všechny hello součásti potřebné toomanage hello životního cyklu a odeslání pro vaše aktéři zpráva:</span><span class="sxs-lookup"><span data-stu-id="09f9b-106">hello actor service contains all hello components necessary toomanage hello lifecycle and message dispatching for your actors:</span></span>

* <span data-ttu-id="09f9b-107">Hello objektu Actor Runtime spravuje životní cyklus, uvolňování paměti a vynucuje jednovláknové přístup.</span><span class="sxs-lookup"><span data-stu-id="09f9b-107">hello Actor Runtime manages lifecycle, garbage collection, and enforces single-threaded access.</span></span>
* <span data-ttu-id="09f9b-108">Naslouchací proces vzdálené komunikace služby objektu actor přijímá tooactors volání vzdáleného přístupu a odešle je tooa dispečera tooroute toohello příslušné objektu actor instance.</span><span class="sxs-lookup"><span data-stu-id="09f9b-108">An actor service remoting listener accepts remote access calls tooactors and sends them tooa dispatcher tooroute toohello appropriate actor instance.</span></span>
* <span data-ttu-id="09f9b-109">Hello zprostředkovatele stavu objektu Actor zabalí zprostředkovatelů stavu (např. zprostředkovatele stavu spolehlivé kolekce hello) a poskytuje adaptér pro správu stavu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-109">hello Actor State Provider wraps state providers (such as hello Reliable Collections state provider) and provides an adapter for actor state management.</span></span>

<span data-ttu-id="09f9b-110">Tyto součásti společně formuláře hello spolehlivé objektu Actor framework.</span><span class="sxs-lookup"><span data-stu-id="09f9b-110">These components together form hello Reliable Actor framework.</span></span>

## <a name="service-layering"></a><span data-ttu-id="09f9b-111">Služba vrstvení</span><span class="sxs-lookup"><span data-stu-id="09f9b-111">Service layering</span></span>
<span data-ttu-id="09f9b-112">Protože samotné služby objektu actor hello je spolehlivá služba, všechny hello [aplikačního modelu](service-fabric-application-model.md), životního cyklu, [balení](service-fabric-package-apps.md), [nasazení](service-fabric-deploy-remove-applications.md), upgrade a škálování koncepty Spolehlivé služby použít hello služby tooactor stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="09f9b-112">Because hello actor service itself is a reliable service, all hello [application model](service-fabric-application-model.md), lifecycle, [packaging](service-fabric-package-apps.md), [deployment](service-fabric-deploy-remove-applications.md), upgrade, and scaling concepts of Reliable Services apply hello same way tooactor services.</span></span> 

![Rozvrstvení služby objektu actor][1]

<span data-ttu-id="09f9b-114">Hello předchozí diagram znázorňuje hello vztah mezi hello Service Fabric aplikační architektury a uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="09f9b-114">hello preceding diagram shows hello relationship between hello Service Fabric application frameworks and user code.</span></span> <span data-ttu-id="09f9b-115">Modré elementy reprezentují hello spolehlivé služby aplikační architektury, oranžová představuje spolehlivé objektu Actor framework hello a zelená představuje uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="09f9b-115">Blue elements represent hello Reliable Services application framework, orange represents hello Reliable Actor framework, and green represents user code.</span></span>

<span data-ttu-id="09f9b-116">V spolehlivé služby, služby dědí hello `StatefulService` třídy.</span><span class="sxs-lookup"><span data-stu-id="09f9b-116">In Reliable Services, your service inherits hello `StatefulService` class.</span></span> <span data-ttu-id="09f9b-117">Tato třída je sám odvozené od `StatefulServiceBase` (nebo `StatelessService` pro bezstavové služby).</span><span class="sxs-lookup"><span data-stu-id="09f9b-117">This class is itself derived from `StatefulServiceBase` (or `StatelessService` for stateless services).</span></span> <span data-ttu-id="09f9b-118">V Reliable Actors pomocí služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="09f9b-118">In Reliable Actors, you use hello actor service.</span></span> <span data-ttu-id="09f9b-119">služby objektu actor Hello je na různé implementace hello `StatefulServiceBase` třídy této implementuje vzor objektu actor hello kde vaše aktéři spustit.</span><span class="sxs-lookup"><span data-stu-id="09f9b-119">hello actor service is a different implementation of hello `StatefulServiceBase` class that implements hello actor pattern where your actors run.</span></span> <span data-ttu-id="09f9b-120">Protože samotné služby objektu actor hello je právě implementací `StatefulServiceBase`, můžete napsat vlastní službu, která je odvozena z `ActorService` a funkce na úrovni služby implementace hello stejný způsobem, jakým byste při dědění `StatefulService`, jako například:</span><span class="sxs-lookup"><span data-stu-id="09f9b-120">Because hello actor service itself is just an implementation of `StatefulServiceBase`, you can write your own service that derives from `ActorService` and implement service-level features hello same way you would when inheriting `StatefulService`, such as:</span></span>

* <span data-ttu-id="09f9b-121">Služba zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="09f9b-121">Service backup and restore.</span></span>
* <span data-ttu-id="09f9b-122">Sdílené funkce pro všechny účastníky, například jistič.</span><span class="sxs-lookup"><span data-stu-id="09f9b-122">Shared functionality for all actors, for example, a circuit breaker.</span></span>
* <span data-ttu-id="09f9b-123">Vzdálená volání procedur v samotné služby objektu actor hello a v každé jednotlivé objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-123">Remote procedure calls on hello actor service itself and on each individual actor.</span></span>

> [!NOTE]
> <span data-ttu-id="09f9b-124">Stavové služby nejsou aktuálně podporované v jazyce Java nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="09f9b-124">Stateful services are not currently supported in Java/Linux.</span></span>

### <a name="using-hello-actor-service"></a><span data-ttu-id="09f9b-125">Pomocí služby objektu actor hello</span><span class="sxs-lookup"><span data-stu-id="09f9b-125">Using hello actor service</span></span>
<span data-ttu-id="09f9b-126">Instance objektu actor mít služby objektu actor toohello přístup, ve kterém běží.</span><span class="sxs-lookup"><span data-stu-id="09f9b-126">Actor instances have access toohello actor service in which they are running.</span></span> <span data-ttu-id="09f9b-127">Prostřednictvím služby objektu actor hello můžete získat instancí objektu actor prostřednictvím kódu programu hello kontext služby.</span><span class="sxs-lookup"><span data-stu-id="09f9b-127">Through hello actor service, actor instances can programmatically obtain hello service context.</span></span> <span data-ttu-id="09f9b-128">kontext služby Hello má ID oddílu hello, název služby, název aplikace a další informace specifické pro platformu Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="09f9b-128">hello service context has hello partition ID, service name, application name, and other Service Fabric platform-specific information:</span></span>

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


<span data-ttu-id="09f9b-129">Podobně jako všechny spolehlivé služby musí být zaregistrován hello objektu actor služby s typem služby v modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="09f9b-129">Like all Reliable Services, hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="09f9b-130">Pro služby objektu actor hello, toorun vaše instance objektu actor, musí váš typ objektu actor také zaregistrován u služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="09f9b-130">For hello actor service toorun your actor instances, your actor type must also be registered with hello actor service.</span></span> <span data-ttu-id="09f9b-131">Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="09f9b-131">hello `ActorRuntime` registration method performs this work for actors.</span></span> <span data-ttu-id="09f9b-132">V nejjednodušším případě hello zaregistrujete na typ vašeho objektu actor a hello služby objektu actor s výchozím nastavením se implicitně použije:</span><span class="sxs-lookup"><span data-stu-id="09f9b-132">In hello simplest case, you can just register your actor type, and hello actor service with default settings will implicitly be used:</span></span>

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

<span data-ttu-id="09f9b-133">Alternativně můžete použít lambda poskytovaný službou hello registrace metoda tooconstruct hello objektu actor sami.</span><span class="sxs-lookup"><span data-stu-id="09f9b-133">Alternatively, you can use a lambda provided by hello registration method tooconstruct hello actor service yourself.</span></span> <span data-ttu-id="09f9b-134">Můžete pak nakonfigurovat služby objektu actor hello a explicitně vytvořit vaše objektu actor instance, kde můžete vložit objektu actor tooyour závislosti prostřednictvím jeho konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="09f9b-134">You can then configure hello actor service and explicitly construct your actor instances, where you can inject dependencies tooyour actor through its constructor:</span></span>

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

### <a name="actor-service-methods"></a><span data-ttu-id="09f9b-135">Metody služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="09f9b-135">Actor service methods</span></span>
<span data-ttu-id="09f9b-136">Hello implementuje služby objektu Actor `IActorService` (C#) nebo `ActorService` (Java), který naopak implementuje `IService` (C#) nebo `Service` (Java).</span><span class="sxs-lookup"><span data-stu-id="09f9b-136">hello Actor service implements `IActorService` (C#) or `ActorService` (Java), which in turn implements `IService` (C#) or `Service` (Java).</span></span> <span data-ttu-id="09f9b-137">Toto je rozhraní hello používá vzdálenou komunikaci spolehlivé služby, které umožňuje vzdálené volání procedur u metod služby.</span><span class="sxs-lookup"><span data-stu-id="09f9b-137">This is hello interface used by Reliable Services remoting, which allows remote procedure calls on service methods.</span></span> <span data-ttu-id="09f9b-138">Obsahuje metody úrovni služby, které je možné volat vzdáleně přes vzdálenou komunikaci služby.</span><span class="sxs-lookup"><span data-stu-id="09f9b-138">It contains service-level methods that can be called remotely via service remoting.</span></span>

#### <a name="enumerating-actors"></a><span data-ttu-id="09f9b-139">Vytváření výčtu actors</span><span class="sxs-lookup"><span data-stu-id="09f9b-139">Enumerating actors</span></span>
<span data-ttu-id="09f9b-140">služby objektu actor Hello umožňuje klientům tooenumerate metadata o hello aktéři, která je hostitelem služby hello.</span><span class="sxs-lookup"><span data-stu-id="09f9b-140">hello actor service allows a client tooenumerate metadata about hello actors that hello service is hosting.</span></span> <span data-ttu-id="09f9b-141">Vzhledem k tomu, že je služba objektu actor hello oddílů stavové služby, výčet se provádí na oddíl.</span><span class="sxs-lookup"><span data-stu-id="09f9b-141">Because hello actor service is a partitioned stateful service, enumeration is performed per partition.</span></span> <span data-ttu-id="09f9b-142">Protože každý oddíl může obsahovat mnoho aktéři, výčet hello se vrátí jako sadu stránkových výsledků.</span><span class="sxs-lookup"><span data-stu-id="09f9b-142">Because each partition might contain many actors, hello enumeration is returned as a set of paged results.</span></span> <span data-ttu-id="09f9b-143">Hello stránky jsou smyčce přes, dokud se číst všechny stránky.</span><span class="sxs-lookup"><span data-stu-id="09f9b-143">hello pages are looped over until all pages are read.</span></span> <span data-ttu-id="09f9b-144">Následující příklad ukazuje, jak Hello toocreate seznam všech active aktéři v jednom oddílu služby objektu actor:</span><span class="sxs-lookup"><span data-stu-id="09f9b-144">hello following example shows how toocreate a list of all active actors in one partition of an actor service:</span></span>

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

#### <a name="deleting-actors"></a><span data-ttu-id="09f9b-145">Odstranění actors</span><span class="sxs-lookup"><span data-stu-id="09f9b-145">Deleting actors</span></span>
<span data-ttu-id="09f9b-146">služby objektu actor Hello také poskytuje funkce pro odstranění aktéři:</span><span class="sxs-lookup"><span data-stu-id="09f9b-146">hello actor service also provides a function for deleting actors:</span></span>

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

<span data-ttu-id="09f9b-147">Další informace o odstranění aktéři a jejich stavu najdete v tématu hello [objektu actor životního cyklu dokumentaci](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="09f9b-147">For more information on deleting actors and their state, see hello [actor lifecycle documentation](service-fabric-reliable-actors-lifecycle.md).</span></span>

### <a name="custom-actor-service"></a><span data-ttu-id="09f9b-148">Služba vlastní objektu actor</span><span class="sxs-lookup"><span data-stu-id="09f9b-148">Custom actor service</span></span>
<span data-ttu-id="09f9b-149">Pomocí hello objektu actor registrace lambda, můžete zaregistrovat vlastní vlastní objektu actor služba, která je odvozena z `ActorService` (C#) a `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="09f9b-149">By using hello actor registration lambda, you can register your own custom actor service that derives from `ActorService` (C#) and `FabricActorService` (Java).</span></span> <span data-ttu-id="09f9b-150">V rámci této služby objektu actor vlastní můžete implementovat vlastní úroveň služby funkce napsáním třídu služby, který dědí `ActorService` (C#) nebo `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="09f9b-150">In this custom actor service, you can implement your own service-level functionality by writing a service class that inherits `ActorService` (C#) or `FabricActorService` (Java).</span></span> <span data-ttu-id="09f9b-151">Služby objektu actor vlastní dědí všechny funkcionalita modulu runtime objektu actor hello z `ActorService` (C#) nebo `FabricActorService` (Java) a může být použité tooimplement vlastní metody služeb.</span><span class="sxs-lookup"><span data-stu-id="09f9b-151">A custom actor service inherits all hello actor runtime functionality from `ActorService` (C#) or `FabricActorService` (Java) and can be used tooimplement your own service methods.</span></span>

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

#### <a name="implementing-actor-backup-and-restore"></a><span data-ttu-id="09f9b-152">Implementace objektu actor zálohování a obnovení</span><span class="sxs-lookup"><span data-stu-id="09f9b-152">Implementing actor backup and restore</span></span>
 <span data-ttu-id="09f9b-153">V následujícím příkladu hello, zpřístupní služby objektu actor vlastní hello tooback metoda objektu actor data a využívají k naslouchání vzdálené komunikace hello již existuje v `ActorService`:</span><span class="sxs-lookup"><span data-stu-id="09f9b-153">In hello following example, hello custom actor service exposes a method tooback up actor data by taking advantage of hello remoting listener already present in `ActorService`:</span></span>

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
           // store hello contents of backupInfo.Directory
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
           // store hello contents of backupInfo.Directory
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


<span data-ttu-id="09f9b-154">V tomto příkladu `IMyActorService` je Vzdálená komunikace kontrakt, který implementuje `IService` (C#) a `Service` (Java) a pak je implementováno modulem `MyActorService`.</span><span class="sxs-lookup"><span data-stu-id="09f9b-154">In this example, `IMyActorService` is a remoting contract that implements `IService` (C#) and `Service` (Java), and is then implemented by `MyActorService`.</span></span> <span data-ttu-id="09f9b-155">Přidáním této smlouvy vzdálenou komunikaci, metody na `IMyActorService` jsou nyní také k dispozici tooa klienta tak, že vytvoříte proxy vzdálenou komunikaci prostřednictvím `ActorServiceProxy`:</span><span class="sxs-lookup"><span data-stu-id="09f9b-155">By adding this remoting contract, methods on `IMyActorService` are now also available tooa client by creating a remoting proxy via `ActorServiceProxy`:</span></span>

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

## <a name="application-model"></a><span data-ttu-id="09f9b-156">Aplikační model</span><span class="sxs-lookup"><span data-stu-id="09f9b-156">Application model</span></span>
<span data-ttu-id="09f9b-157">Služby objektu actor jsou spolehlivé služby, takže hello aplikačního modelu je hello stejné.</span><span class="sxs-lookup"><span data-stu-id="09f9b-157">Actor services are Reliable Services, so hello application model is hello same.</span></span> <span data-ttu-id="09f9b-158">Však hello objektu actor framework sestavovací nástroje generovat některé soubory modelu aplikace hello za vás.</span><span class="sxs-lookup"><span data-stu-id="09f9b-158">However, hello actor framework build tools generate some of hello application model files for you.</span></span>

### <a name="service-manifest"></a><span data-ttu-id="09f9b-159">Manifest služby</span><span class="sxs-lookup"><span data-stu-id="09f9b-159">Service manifest</span></span>
<span data-ttu-id="09f9b-160">Nástroje pro sestavení Hello objektu actor framework automaticky generovat hello obsah souboru ServiceManifest.xml služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-160">hello actor framework build tools automatically generate hello contents of your actor service's ServiceManifest.xml file.</span></span> <span data-ttu-id="09f9b-161">Tento soubor obsahuje:</span><span class="sxs-lookup"><span data-stu-id="09f9b-161">This file includes:</span></span>

* <span data-ttu-id="09f9b-162">Typ služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-162">Actor service type.</span></span> <span data-ttu-id="09f9b-163">Název typu Hello je vytvořen na základě názvu objektu actor projektu.</span><span class="sxs-lookup"><span data-stu-id="09f9b-163">hello type name is generated based on your actor's project name.</span></span> <span data-ttu-id="09f9b-164">Na základě hello trvalost atributu na vaše objektu actor, hello HasPersistedState příznak nastavený i odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="09f9b-164">Based on hello persistence attribute on your actor, hello HasPersistedState flag is also set accordingly.</span></span>
* <span data-ttu-id="09f9b-165">Balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="09f9b-165">Code package.</span></span>
* <span data-ttu-id="09f9b-166">Konfigurační balíček.</span><span class="sxs-lookup"><span data-stu-id="09f9b-166">Config package.</span></span>
* <span data-ttu-id="09f9b-167">Koncové body a prostředky.</span><span class="sxs-lookup"><span data-stu-id="09f9b-167">Resources and endpoints.</span></span>

### <a name="application-manifest"></a><span data-ttu-id="09f9b-168">Manifest aplikace</span><span class="sxs-lookup"><span data-stu-id="09f9b-168">Application manifest</span></span>
<span data-ttu-id="09f9b-169">Nástroje pro sestavení Hello objektu actor framework automaticky vytvoří definici výchozí služby pro služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-169">hello actor framework build tools automatically create a default service definition for your actor service.</span></span> <span data-ttu-id="09f9b-170">Nástroje pro sestavení Hello naplnit hello výchozí služby vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="09f9b-170">hello build tools populate hello default service properties:</span></span>

* <span data-ttu-id="09f9b-171">Počet sady replik je určen podle hello trvalost atribut na vaše objektu actor.</span><span class="sxs-lookup"><span data-stu-id="09f9b-171">Replica set count is determined by hello persistence attribute on your actor.</span></span> <span data-ttu-id="09f9b-172">Každý atribut trvalost hello čas na vaše objektu actor mění, počet sadu hello repliky v definici služby výchozí hello se resetuje odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="09f9b-172">Each time hello persistence attribute on your actor is changed, hello replica set count in hello default service definition is reset accordingly.</span></span>
* <span data-ttu-id="09f9b-173">Schéma oddílu a rozsah se nastavují tooUniform Int64 s hello plný Int64 klíče rozsah.</span><span class="sxs-lookup"><span data-stu-id="09f9b-173">Partition scheme and range are set tooUniform Int64 with hello full Int64 key range.</span></span>

## <a name="service-fabric-partition-concepts-for-actors"></a><span data-ttu-id="09f9b-174">Koncepty oddílu Service Fabric actors</span><span class="sxs-lookup"><span data-stu-id="09f9b-174">Service Fabric partition concepts for actors</span></span>
<span data-ttu-id="09f9b-175">Služby objektu actor jsou oddílů stavové služby.</span><span class="sxs-lookup"><span data-stu-id="09f9b-175">Actor services are partitioned stateful services.</span></span> <span data-ttu-id="09f9b-176">Každý oddíl služby objektu actor obsahuje sadu aktéři.</span><span class="sxs-lookup"><span data-stu-id="09f9b-176">Each partition of an actor service contains a set of actors.</span></span> <span data-ttu-id="09f9b-177">Oddílů služby je automaticky distribuovaná přes více uzlů v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="09f9b-177">Service partitions are automatically distributed over multiple nodes in Service Fabric.</span></span> <span data-ttu-id="09f9b-178">Instance objektu actor jsou distribuovány v důsledku.</span><span class="sxs-lookup"><span data-stu-id="09f9b-178">Actor instances are distributed as a result.</span></span>

![Objekt actor vytváření oddílů a distribuci][5]

<span data-ttu-id="09f9b-180">Spolehlivé služby můžete vytvořit s schémata jiný oddíl a oddíl rozsahy klíčů.</span><span class="sxs-lookup"><span data-stu-id="09f9b-180">Reliable Services can be created with different partition schemes and partition key ranges.</span></span> <span data-ttu-id="09f9b-181">služby objektu actor Hello používá schéma rozdělení oddílů Int64 hello s hello úplné Int64 klíče rozsah toomap aktéři toopartitions.</span><span class="sxs-lookup"><span data-stu-id="09f9b-181">hello actor service uses hello Int64 partitioning scheme with hello full Int64 key range toomap actors toopartitions.</span></span>

### <a name="actor-id"></a><span data-ttu-id="09f9b-182">ID objektu actor</span><span class="sxs-lookup"><span data-stu-id="09f9b-182">Actor ID</span></span>
<span data-ttu-id="09f9b-183">Každý objektu actor, který je vytvořen v hello služby má jedinečné ID přidružený reprezentována hello `ActorId` třídy.</span><span class="sxs-lookup"><span data-stu-id="09f9b-183">Each actor that's created in hello service has a unique ID associated with it, represented by hello `ActorId` class.</span></span> <span data-ttu-id="09f9b-184">`ActorId`je neprůhledné hodnoty ID, které je možné pro rovnoměrné aktéři mezi oddílů služby hello vygenerováním náhodné ID:</span><span class="sxs-lookup"><span data-stu-id="09f9b-184">`ActorId` is an opaque ID value that can be used for uniform distribution of actors across hello service partitions by generating random IDs:</span></span>

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


<span data-ttu-id="09f9b-185">Každý `ActorId` je hash tooan Int64.</span><span class="sxs-lookup"><span data-stu-id="09f9b-185">Every `ActorId` is hashed tooan Int64.</span></span> <span data-ttu-id="09f9b-186">Z tohoto důvodu služby objektu actor hello musí používat schéma rozdělení oddílů Int64 s hello plný Int64 klíče rozsah.</span><span class="sxs-lookup"><span data-stu-id="09f9b-186">This is why hello actor service must use an Int64 partitioning scheme with hello full Int64 key range.</span></span> <span data-ttu-id="09f9b-187">Však můžete použít vlastní hodnoty ID pro `ActorID`, včetně GUID/UUID, řetězce a Int64s.</span><span class="sxs-lookup"><span data-stu-id="09f9b-187">However, custom ID values can be used for an `ActorID`, including GUIDs/UUIDs, strings, and Int64s.</span></span>

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

<span data-ttu-id="09f9b-188">Pokud používáte GUID/UUID a řetězce, jsou hello hodnoty hash tooan Int64.</span><span class="sxs-lookup"><span data-stu-id="09f9b-188">When you're using GUIDs/UUIDs and strings, hello values are hashed tooan Int64.</span></span> <span data-ttu-id="09f9b-189">Ale když explicitně zadáváte Int64 tooan `ActorId`, hello Int64 bude přímo mapovat tooa oddílu bez další algoritmu hash.</span><span class="sxs-lookup"><span data-stu-id="09f9b-189">However, when you're explicitly providing an Int64 tooan `ActorId`, hello Int64 will map directly tooa partition without further hashing.</span></span> <span data-ttu-id="09f9b-190">Můžete použít tento toocontrol technika, který aktéři hello oddílu jsou umístěny v.</span><span class="sxs-lookup"><span data-stu-id="09f9b-190">You can use this technique toocontrol which partition hello actors are placed in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09f9b-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09f9b-191">Next steps</span></span>
* [<span data-ttu-id="09f9b-192">Řízení stavu objektu actor</span><span class="sxs-lookup"><span data-stu-id="09f9b-192">Actor state management</span></span>](service-fabric-reliable-actors-state-management.md)
* [<span data-ttu-id="09f9b-193">Kolekce paměti a životního cyklu objektu actor</span><span class="sxs-lookup"><span data-stu-id="09f9b-193">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="09f9b-194">Referenční dokumentace rozhraní API actors</span><span class="sxs-lookup"><span data-stu-id="09f9b-194">Actors API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="09f9b-195">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="09f9b-195">.NET sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="09f9b-196">Ukázkový kód Java</span><span class="sxs-lookup"><span data-stu-id="09f9b-196">Java sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
