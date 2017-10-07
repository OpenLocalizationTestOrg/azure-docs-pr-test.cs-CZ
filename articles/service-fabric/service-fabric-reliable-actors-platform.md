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
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a>Jak používat Reliable Actors hello platformy Service Fabric
Tento článek vysvětluje, jak fungují Reliable Actors na platformě Azure Service Fabric hello. Reliable Actors spustit v rozhraní, které je hostován v implementaci stavová spolehlivá služba s názvem hello *služby objektu actor*. služby objektu actor Hello obsahuje všechny hello součásti potřebné toomanage hello životního cyklu a odeslání pro vaše aktéři zpráva:

* Hello objektu Actor Runtime spravuje životní cyklus, uvolňování paměti a vynucuje jednovláknové přístup.
* Naslouchací proces vzdálené komunikace služby objektu actor přijímá tooactors volání vzdáleného přístupu a odešle je tooa dispečera tooroute toohello příslušné objektu actor instance.
* Hello zprostředkovatele stavu objektu Actor zabalí zprostředkovatelů stavu (např. zprostředkovatele stavu spolehlivé kolekce hello) a poskytuje adaptér pro správu stavu objektu actor.

Tyto součásti společně formuláře hello spolehlivé objektu Actor framework.

## <a name="service-layering"></a>Služba vrstvení
Protože samotné služby objektu actor hello je spolehlivá služba, všechny hello [aplikačního modelu](service-fabric-application-model.md), životního cyklu, [balení](service-fabric-package-apps.md), [nasazení](service-fabric-deploy-remove-applications.md), upgrade a škálování koncepty Spolehlivé služby použít hello služby tooactor stejným způsobem. 

![Rozvrstvení služby objektu actor][1]

Hello předchozí diagram znázorňuje hello vztah mezi hello Service Fabric aplikační architektury a uživatelského kódu. Modré elementy reprezentují hello spolehlivé služby aplikační architektury, oranžová představuje spolehlivé objektu Actor framework hello a zelená představuje uživatelského kódu.

V spolehlivé služby, služby dědí hello `StatefulService` třídy. Tato třída je sám odvozené od `StatefulServiceBase` (nebo `StatelessService` pro bezstavové služby). V Reliable Actors pomocí služby objektu actor hello. služby objektu actor Hello je na různé implementace hello `StatefulServiceBase` třídy této implementuje vzor objektu actor hello kde vaše aktéři spustit. Protože samotné služby objektu actor hello je právě implementací `StatefulServiceBase`, můžete napsat vlastní službu, která je odvozena z `ActorService` a funkce na úrovni služby implementace hello stejný způsobem, jakým byste při dědění `StatefulService`, jako například:

* Služba zálohování a obnovení.
* Sdílené funkce pro všechny účastníky, například jistič.
* Vzdálená volání procedur v samotné služby objektu actor hello a v každé jednotlivé objektu actor.

> [!NOTE]
> Stavové služby nejsou aktuálně podporované v jazyce Java nebo Linux.

### <a name="using-hello-actor-service"></a>Pomocí služby objektu actor hello
Instance objektu actor mít služby objektu actor toohello přístup, ve kterém běží. Prostřednictvím služby objektu actor hello můžete získat instancí objektu actor prostřednictvím kódu programu hello kontext služby. kontext služby Hello má ID oddílu hello, název služby, název aplikace a další informace specifické pro platformu Service Fabric:

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


Podobně jako všechny spolehlivé služby musí být zaregistrován hello objektu actor služby s typem služby v modulu runtime Service Fabric hello. Pro služby objektu actor hello, toorun vaše instance objektu actor, musí váš typ objektu actor také zaregistrován u služby objektu actor hello. Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři. V nejjednodušším případě hello zaregistrujete na typ vašeho objektu actor a hello služby objektu actor s výchozím nastavením se implicitně použije:

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

Alternativně můžete použít lambda poskytovaný službou hello registrace metoda tooconstruct hello objektu actor sami. Můžete pak nakonfigurovat služby objektu actor hello a explicitně vytvořit vaše objektu actor instance, kde můžete vložit objektu actor tooyour závislosti prostřednictvím jeho konstruktoru:

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

### <a name="actor-service-methods"></a>Metody služby objektu actor
Hello implementuje služby objektu Actor `IActorService` (C#) nebo `ActorService` (Java), který naopak implementuje `IService` (C#) nebo `Service` (Java). Toto je rozhraní hello používá vzdálenou komunikaci spolehlivé služby, které umožňuje vzdálené volání procedur u metod služby. Obsahuje metody úrovni služby, které je možné volat vzdáleně přes vzdálenou komunikaci služby.

#### <a name="enumerating-actors"></a>Vytváření výčtu actors
služby objektu actor Hello umožňuje klientům tooenumerate metadata o hello aktéři, která je hostitelem služby hello. Vzhledem k tomu, že je služba objektu actor hello oddílů stavové služby, výčet se provádí na oddíl. Protože každý oddíl může obsahovat mnoho aktéři, výčet hello se vrátí jako sadu stránkových výsledků. Hello stránky jsou smyčce přes, dokud se číst všechny stránky. Následující příklad ukazuje, jak Hello toocreate seznam všech active aktéři v jednom oddílu služby objektu actor:

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

#### <a name="deleting-actors"></a>Odstranění actors
služby objektu actor Hello také poskytuje funkce pro odstranění aktéři:

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

Další informace o odstranění aktéři a jejich stavu najdete v tématu hello [objektu actor životního cyklu dokumentaci](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Služba vlastní objektu actor
Pomocí hello objektu actor registrace lambda, můžete zaregistrovat vlastní vlastní objektu actor služba, která je odvozena z `ActorService` (C#) a `FabricActorService` (Java). V rámci této služby objektu actor vlastní můžete implementovat vlastní úroveň služby funkce napsáním třídu služby, který dědí `ActorService` (C#) nebo `FabricActorService` (Java). Služby objektu actor vlastní dědí všechny funkcionalita modulu runtime objektu actor hello z `ActorService` (C#) nebo `FabricActorService` (Java) a může být použité tooimplement vlastní metody služeb.

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

#### <a name="implementing-actor-backup-and-restore"></a>Implementace objektu actor zálohování a obnovení
 V následujícím příkladu hello, zpřístupní služby objektu actor vlastní hello tooback metoda objektu actor data a využívají k naslouchání vzdálené komunikace hello již existuje v `ActorService`:

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


V tomto příkladu `IMyActorService` je Vzdálená komunikace kontrakt, který implementuje `IService` (C#) a `Service` (Java) a pak je implementováno modulem `MyActorService`. Přidáním této smlouvy vzdálenou komunikaci, metody na `IMyActorService` jsou nyní také k dispozici tooa klienta tak, že vytvoříte proxy vzdálenou komunikaci prostřednictvím `ActorServiceProxy`:

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

## <a name="application-model"></a>Aplikační model
Služby objektu actor jsou spolehlivé služby, takže hello aplikačního modelu je hello stejné. Však hello objektu actor framework sestavovací nástroje generovat některé soubory modelu aplikace hello za vás.

### <a name="service-manifest"></a>Manifest služby
Nástroje pro sestavení Hello objektu actor framework automaticky generovat hello obsah souboru ServiceManifest.xml služby objektu actor. Tento soubor obsahuje:

* Typ služby objektu actor. Název typu Hello je vytvořen na základě názvu objektu actor projektu. Na základě hello trvalost atributu na vaše objektu actor, hello HasPersistedState příznak nastavený i odpovídajícím způsobem.
* Balíček kódu.
* Konfigurační balíček.
* Koncové body a prostředky.

### <a name="application-manifest"></a>Manifest aplikace
Nástroje pro sestavení Hello objektu actor framework automaticky vytvoří definici výchozí služby pro služby objektu actor. Nástroje pro sestavení Hello naplnit hello výchozí služby vlastnosti:

* Počet sady replik je určen podle hello trvalost atribut na vaše objektu actor. Každý atribut trvalost hello čas na vaše objektu actor mění, počet sadu hello repliky v definici služby výchozí hello se resetuje odpovídajícím způsobem.
* Schéma oddílu a rozsah se nastavují tooUniform Int64 s hello plný Int64 klíče rozsah.

## <a name="service-fabric-partition-concepts-for-actors"></a>Koncepty oddílu Service Fabric actors
Služby objektu actor jsou oddílů stavové služby. Každý oddíl služby objektu actor obsahuje sadu aktéři. Oddílů služby je automaticky distribuovaná přes více uzlů v Service Fabric. Instance objektu actor jsou distribuovány v důsledku.

![Objekt actor vytváření oddílů a distribuci][5]

Spolehlivé služby můžete vytvořit s schémata jiný oddíl a oddíl rozsahy klíčů. služby objektu actor Hello používá schéma rozdělení oddílů Int64 hello s hello úplné Int64 klíče rozsah toomap aktéři toopartitions.

### <a name="actor-id"></a>ID objektu actor
Každý objektu actor, který je vytvořen v hello služby má jedinečné ID přidružený reprezentována hello `ActorId` třídy. `ActorId`je neprůhledné hodnoty ID, které je možné pro rovnoměrné aktéři mezi oddílů služby hello vygenerováním náhodné ID:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Každý `ActorId` je hash tooan Int64. Z tohoto důvodu služby objektu actor hello musí používat schéma rozdělení oddílů Int64 s hello plný Int64 klíče rozsah. Však můžete použít vlastní hodnoty ID pro `ActorID`, včetně GUID/UUID, řetězce a Int64s.

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

Pokud používáte GUID/UUID a řetězce, jsou hello hodnoty hash tooan Int64. Ale když explicitně zadáváte Int64 tooan `ActorId`, hello Int64 bude přímo mapovat tooa oddílu bez další algoritmu hash. Můžete použít tento toocontrol technika, který aktéři hello oddílu jsou umístěny v.

## <a name="next-steps"></a>Další kroky
* [Řízení stavu objektu actor](service-fabric-reliable-actors-state-management.md)
* [Kolekce paměti a životního cyklu objektu actor](service-fabric-reliable-actors-lifecycle.md)
* [Referenční dokumentace rozhraní API actors](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Ukázkový kód rozhraní .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Ukázkový kód Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
