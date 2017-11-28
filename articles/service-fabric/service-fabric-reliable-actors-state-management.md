---
title: "aaaReliable aktéři stavu správy | Microsoft Docs"
description: "Popisuje způsob správy, jako trvalý a pro zajištění vysoké dostupnosti replikují Reliable Actors stavu."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 346d92426b1890617d108a9504afb179e463bded
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-state-management"></a><span data-ttu-id="2f3da-103">Spolehlivé aktéři řízení stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-103">Reliable Actors state management</span></span>
<span data-ttu-id="2f3da-104">Reliable Actors jsou jedním podprocesem objekty, které může zapouzdřit logiku a stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-104">Reliable Actors are single-threaded objects that can encapsulate both logic and state.</span></span> <span data-ttu-id="2f3da-105">Protože aktéři spustit na spolehlivé služby, se může stavu spolehlivě udržovat pomocí hello stejné trvalosti a replikace mechanismy, které používá spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="2f3da-105">Because actors run on Reliable Services, they can maintain state reliably by using hello same persistence and replication mechanisms that Reliable Services uses.</span></span> <span data-ttu-id="2f3da-106">Tímto způsobem Neztraťte aktéři jejich stav po selhání při opětovné aktivaci po uvolnění paměti, nebo při jejich přesunu mezi uzly v clusteru s podporou kvůli vyrovnávání tooresource nebo upgradu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-106">This way, actors don't lose their state after failures, upon reactivation after garbage collection, or when they are moved around between nodes in a cluster due tooresource balancing or upgrades.</span></span>

## <a name="state-persistence-and-replication"></a><span data-ttu-id="2f3da-107">Trvalost stavu a replikace</span><span class="sxs-lookup"><span data-stu-id="2f3da-107">State persistence and replication</span></span>
<span data-ttu-id="2f3da-108">Jsou považovány za všechny Reliable Actors *stateful* protože každá instance objektu actor mapuje tooa jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f3da-108">All Reliable Actors are considered *stateful* because each actor instance maps tooa unique ID.</span></span> <span data-ttu-id="2f3da-109">To znamená, že opakovaná volání toohello se stejným ID objektu actor směrovány toohello stejnou instanci objektu actor.</span><span class="sxs-lookup"><span data-stu-id="2f3da-109">This means that repeated calls toohello same actor ID are routed toohello same actor instance.</span></span> <span data-ttu-id="2f3da-110">V systému bezstavové naopak volání klienta se nezaručuje, že toobe směrovány toohello stejný server pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="2f3da-110">In a stateless system, by contrast, client calls are not guaranteed toobe routed toohello same server every time.</span></span> <span data-ttu-id="2f3da-111">Z tohoto důvodu služby objektu actor jsou vždy stavové služby.</span><span class="sxs-lookup"><span data-stu-id="2f3da-111">For this reason, actor services are always stateful services.</span></span>

<span data-ttu-id="2f3da-112">I když aktéři jsou považovány za stateful, který neznamená, že musí spolehlivě uložení stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-112">Even though actors are considered stateful, that does not mean they must store state reliably.</span></span> <span data-ttu-id="2f3da-113">Aktéři můžete vybrat úroveň hello trvalost stavu a replikace na základě jejich dat požadavky na úložiště:</span><span class="sxs-lookup"><span data-stu-id="2f3da-113">Actors can choose hello level of state persistence and replication based on their data storage requirements:</span></span>

* <span data-ttu-id="2f3da-114">**Trvalý stav**: stav je trvalý toodisk a replikované too3 nebo další repliky.</span><span class="sxs-lookup"><span data-stu-id="2f3da-114">**Persisted state**: State is persisted toodisk and is replicated too3 or more replicas.</span></span> <span data-ttu-id="2f3da-115">Toto je hello nejvíce trvanlivá možnost úložiště stavu, kde můžete zachovat stav prostřednictvím clusterů výpadku.</span><span class="sxs-lookup"><span data-stu-id="2f3da-115">This is hello most durable state storage option, where state can persist through complete cluster outage.</span></span>
* <span data-ttu-id="2f3da-116">**Volatile stavu**: stav replikované too3 nebo více replik a pouze uchovává se v paměti.</span><span class="sxs-lookup"><span data-stu-id="2f3da-116">**Volatile state**: State is replicated too3 or more replicas and only kept in memory.</span></span> <span data-ttu-id="2f3da-117">To zajišťuje odolnost proti selhání uzlu a selhání objektu actor a během upgradu a vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="2f3da-117">This provides resilience against node failure and actor failure, and during upgrades and resource balancing.</span></span> <span data-ttu-id="2f3da-118">Stav však není trvalý toodisk.</span><span class="sxs-lookup"><span data-stu-id="2f3da-118">However, state is not persisted toodisk.</span></span> <span data-ttu-id="2f3da-119">Proto všechny repliky byly ztraceny najednou, hello stavu dojde ke ztrátě také.</span><span class="sxs-lookup"><span data-stu-id="2f3da-119">So if all replicas are lost at once, hello state is lost as well.</span></span>
* <span data-ttu-id="2f3da-120">**Žádné trvalého stavu**: není replikovat nebo zapisují toodisk stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-120">**No persisted state**: State is not replicated or written toodisk.</span></span> <span data-ttu-id="2f3da-121">Tato úroveň je actor je jednoduše toomaintain stavu spolehlivě nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="2f3da-121">This level is for actors that simply don't need toomaintain state reliably.</span></span>

<span data-ttu-id="2f3da-122">Každou úroveň trvalost je jednoduše jiné *zprostředkovatele stavu* a *replikace* konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="2f3da-122">Each level of persistence is simply a different *state provider* and *replication* configuration of your service.</span></span> <span data-ttu-id="2f3da-123">Zda je zapsán stavu toodisk závisí na zprostředkovatele stavu hello – hello součástí spolehlivá služba, která ukládá stav.</span><span class="sxs-lookup"><span data-stu-id="2f3da-123">Whether or not state is written toodisk depends on hello state provider--hello component in a reliable service that stores state.</span></span> <span data-ttu-id="2f3da-124">Replikace závisí na tom, kolik repliky může služba nasazený s.</span><span class="sxs-lookup"><span data-stu-id="2f3da-124">Replication depends on how many replicas a service is deployed with.</span></span> <span data-ttu-id="2f3da-125">Stejně jako u spolehlivé služby i hello zprostředkovatele stavu a počet replik jde snadno nastavit ručně.</span><span class="sxs-lookup"><span data-stu-id="2f3da-125">As with Reliable Services, both hello state provider and replica count can easily be set manually.</span></span> <span data-ttu-id="2f3da-126">Hello objektu actor framework poskytuje atribut, že při použití v objektu actor automaticky vybere výchozí poskytovatel stavu a automaticky vygeneruje nastavení pro tooachieve počet replik, jednu z těchto tří nastavení trvalosti.</span><span class="sxs-lookup"><span data-stu-id="2f3da-126">hello actor framework provides an attribute that, when used on an actor, automatically selects a default state provider and automatically generates settings for replica count tooachieve one of these three persistence settings.</span></span> <span data-ttu-id="2f3da-127">atribut StatePersistence Hello není zdědí odvozené třídy, každý typ objektu Actor musíte zadat jeho StatePersistence úroveň.</span><span class="sxs-lookup"><span data-stu-id="2f3da-127">hello StatePersistence attribute is not inherited by derived class, each Actor type must provide its StatePersistence level.</span></span>

### <a name="persisted-state"></a><span data-ttu-id="2f3da-128">Trvalého stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-128">Persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
<span data-ttu-id="2f3da-129">Toto nastavení využívá zprostředkovatele stavu, která ukládá data na disku a automaticky nastaví too3 počet replik služby hello.</span><span class="sxs-lookup"><span data-stu-id="2f3da-129">This setting uses a state provider that stores data on disk and automatically sets hello service replica count too3.</span></span>

### <a name="volatile-state"></a><span data-ttu-id="2f3da-130">Volatile stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-130">Volatile state</span></span>
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="2f3da-131">Toto nastavení používá poskytovatele v paměti jen stavu a nastaví hello too3 počet replik.</span><span class="sxs-lookup"><span data-stu-id="2f3da-131">This setting uses an in-memory-only state provider and sets hello replica count too3.</span></span>

### <a name="no-persisted-state"></a><span data-ttu-id="2f3da-132">Žádné trvalého stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-132">No persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="2f3da-133">Toto nastavení používá poskytovatele v paměti jen stavu a nastaví hello too1 počet replik.</span><span class="sxs-lookup"><span data-stu-id="2f3da-133">This setting uses an in-memory-only state provider and sets hello replica count too1.</span></span>

### <a name="defaults-and-generated-settings"></a><span data-ttu-id="2f3da-134">Výchozí hodnoty a vygenerované nastavení</span><span class="sxs-lookup"><span data-stu-id="2f3da-134">Defaults and generated settings</span></span>
<span data-ttu-id="2f3da-135">Pokud používáte hello `StatePersistence` atribut zprostředkovatele stavu je automaticky vybrána pro za běhu při spuštění služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="2f3da-135">When you're using hello `StatePersistence` attribute, a state provider is automatically selected for you at runtime when hello actor service starts.</span></span> <span data-ttu-id="2f3da-136">Hello repliky, ale nastavení počtu v době kompilace pomocí hello objektu actor v sadě Visual Studio nástroje sestavení.</span><span class="sxs-lookup"><span data-stu-id="2f3da-136">hello replica count, however, is set at compile time by hello Visual Studio actor build tools.</span></span> <span data-ttu-id="2f3da-137">Hello nástroje sestavení automaticky generovat *výchozí služba* služby objektu actor hello ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="2f3da-137">hello build tools automatically generate a *default service* for hello actor service in ApplicationManifest.xml.</span></span> <span data-ttu-id="2f3da-138">Vytvoří se pro parametry **sady replik minimální velikost** a **cíl repliky nastavit velikost**.</span><span class="sxs-lookup"><span data-stu-id="2f3da-138">Parameters are created for **min replica set size** and **target replica set size**.</span></span>

<span data-ttu-id="2f3da-139">Tyto parametry můžete ručně změnit.</span><span class="sxs-lookup"><span data-stu-id="2f3da-139">You can change these parameters manually.</span></span> <span data-ttu-id="2f3da-140">Ale každý čas hello `StatePersistence` změně atributu, hello parametry jsou nastaveny toohello výchozí sadu repliky velikost hodnoty pro vybrané hello `StatePersistence` atribut přepsání žádné předchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2f3da-140">But each time hello `StatePersistence` attribute is changed, hello parameters are set toohello default replica set size values for hello selected `StatePersistence` attribute, overriding any previous values.</span></span> <span data-ttu-id="2f3da-141">Jinými slovy, jsou hello hodnoty, které jste nastavili v ServiceManifest.xml *pouze* přepsat v čase vytvoření buildu, když změníte hello `StatePersistence` hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-141">In other words, hello values that you set in ServiceManifest.xml are *only* overridden at build time when you change hello `StatePersistence` attribute value.</span></span>

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a><span data-ttu-id="2f3da-142">Správce stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-142">State manager</span></span>
<span data-ttu-id="2f3da-143">Všechny instance objektu actor má svou vlastní správce stavu: jako slovník datová struktura, která spolehlivě uchová dvojice klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="2f3da-143">Every actor instance has its own state manager: a dictionary-like data structure that reliably stores key/value pairs.</span></span> <span data-ttu-id="2f3da-144">Správce stavu Hello je obálku kolem zprostředkovatele stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-144">hello state manager is a wrapper around a state provider.</span></span> <span data-ttu-id="2f3da-145">Můžete ho toostore dat bez ohledu na to, které trvalost nastavení se používá.</span><span class="sxs-lookup"><span data-stu-id="2f3da-145">You can use it toostore data regardless of which persistence setting is used.</span></span> <span data-ttu-id="2f3da-146">Neposkytuje se, že žádné záruky, že spuštěné služby objektu actor lze změnit z nastavení tooa volatile (v paměti jen) stavu trvalé nastavení stavu prostřednictvím postupného upgradu zachovat při data.</span><span class="sxs-lookup"><span data-stu-id="2f3da-146">It does not provide any guarantees that a running actor service can be changed from a volatile (in-memory-only) state setting tooa persisted state setting through a rolling upgrade while preserving data.</span></span> <span data-ttu-id="2f3da-147">Je však možné toochange počet replik pro spuštěnou službu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-147">However, it is possible toochange replica count for a running service.</span></span>

<span data-ttu-id="2f3da-148">Správce stavu klíče musí být řetězce.</span><span class="sxs-lookup"><span data-stu-id="2f3da-148">State manager keys must be strings.</span></span> <span data-ttu-id="2f3da-149">Hodnoty jsou obecné a mohou být jakéhokoli typu, včetně vlastních typů.</span><span class="sxs-lookup"><span data-stu-id="2f3da-149">Values are generic and can be any type, including custom types.</span></span> <span data-ttu-id="2f3da-150">Hodnotami uloženými v správce stavu hello musí být serializovatelný kontrakt dat, protože může přenést přes uzly tooother hello sítě během replikace a může být zapsána toodisk, v závislosti na nastavení trvalost stavu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="2f3da-150">Values stored in hello state manager must be data contract serializable because they might be transmitted over hello network tooother nodes during replication and might be written toodisk, depending on an actor's state persistence setting.</span></span>

<span data-ttu-id="2f3da-151">Správce stavu Hello zpřístupní běžné metody slovník pro správu stavu, podobně jako toothose najít v spolehlivé slovníku.</span><span class="sxs-lookup"><span data-stu-id="2f3da-151">hello state manager exposes common dictionary methods for managing state, similar toothose found in Reliable Dictionary.</span></span>

### <a name="accessing-state"></a><span data-ttu-id="2f3da-152">Přístup ke stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-152">Accessing state</span></span>
<span data-ttu-id="2f3da-153">Stav je přístupná prostřednictvím Správce stavu hello podle klíče.</span><span class="sxs-lookup"><span data-stu-id="2f3da-153">State can be accessed through hello state manager by key.</span></span> <span data-ttu-id="2f3da-154">Metody správce stavu jsou všechny asynchronní, protože v/v disku může vyžadují při aktéři držena formou stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-154">State manager methods are all asynchronous because they might require disk I/O when actors have persisted state.</span></span> <span data-ttu-id="2f3da-155">Při prvním přístupu jsou uložená v mezipaměti objektů stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-155">Upon first access, state objects are cached in memory.</span></span> <span data-ttu-id="2f3da-156">Opakujte přístup operations přístup k objektům přímo z paměti a vrátí synchronně, aniž by docházelo k vstupně-výstupní diskové nebo asynchronní režií přepínání kontextu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-156">Repeat access operations access objects directly from memory and return synchronously without incurring disk I/O or asynchronous context-switching overhead.</span></span> <span data-ttu-id="2f3da-157">Stav objektu se odebere z mezipaměti hello v následujících případech hello:</span><span class="sxs-lookup"><span data-stu-id="2f3da-157">A state object is removed from hello cache in hello following cases:</span></span>

* <span data-ttu-id="2f3da-158">Po ho načte objekt z správce stavu hello, vyvolá metoda objektu actor k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="2f3da-158">An actor method throws an unhandled exception after it retrieves an object from hello state manager.</span></span>
* <span data-ttu-id="2f3da-159">Objekt actor je znovu aktivovat, po právě deaktivována nebo po selhání.</span><span class="sxs-lookup"><span data-stu-id="2f3da-159">An actor is reactivated, either after being deactivated or after failure.</span></span>
* <span data-ttu-id="2f3da-160">stránky zprostředkovatele stavu Hello stavu toodisk.</span><span class="sxs-lookup"><span data-stu-id="2f3da-160">hello state provider pages state toodisk.</span></span> <span data-ttu-id="2f3da-161">Toto chování závisí na implementaci zprostředkovatele stavu hello.</span><span class="sxs-lookup"><span data-stu-id="2f3da-161">This behavior depends on hello state provider implementation.</span></span> <span data-ttu-id="2f3da-162">Zprostředkovatel stavu Hello výchozí pro hello `Persisted` nastavení je toto chování.</span><span class="sxs-lookup"><span data-stu-id="2f3da-162">hello default state provider for hello `Persisted` setting has this behavior.</span></span>

<span data-ttu-id="2f3da-163">Stav můžete načíst pomocí standardního *získat* operace, která vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java), pokud položka neexistuje pro klíč hello:</span><span class="sxs-lookup"><span data-stu-id="2f3da-163">You can retrieve state by using a standard *Get* operation that throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) if an entry does not exist for hello key:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

<span data-ttu-id="2f3da-164">Můžete také načíst stav pomocí *TryGet* metoda, která nevyvolá výjimku pokud záznam neexistuje pro klíč:</span><span class="sxs-lookup"><span data-stu-id="2f3da-164">You can also retrieve state by using a *TryGet* method that does not throw if an entry does not exist for a key:</span></span>

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a><span data-ttu-id="2f3da-165">Ukládání stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-165">Saving state</span></span>
<span data-ttu-id="2f3da-166">metod načítání správce stavu Hello vrátí referenční tooan objekt v místní paměti.</span><span class="sxs-lookup"><span data-stu-id="2f3da-166">hello state manager retrieval methods return a reference tooan object in local memory.</span></span> <span data-ttu-id="2f3da-167">Úprava tento objekt v místní paměti samostatně nezpůsobí ho toobe bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="2f3da-167">Modifying this object in local memory alone does not cause it toobe saved durably.</span></span> <span data-ttu-id="2f3da-168">Pokud objekt je načtena z správce stavu hello a měnit, musí znovu vložit do hello stavu manager toobe bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="2f3da-168">When an object is retrieved from hello state manager and modified, it must be reinserted into hello state manager toobe saved durably.</span></span>

<span data-ttu-id="2f3da-169">Můžete vložit stavu pomocí nepodmíněné *nastavit*, což je hello ekvivalentní hello `dictionary["key"] = value` syntaxe:</span><span class="sxs-lookup"><span data-stu-id="2f3da-169">You can insert state by using an unconditional *Set*, which is hello equivalent of hello `dictionary["key"] = value` syntax:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

<span data-ttu-id="2f3da-170">Stav můžete přidat pomocí *přidat* metoda.</span><span class="sxs-lookup"><span data-stu-id="2f3da-170">You can add state by using an *Add* method.</span></span> <span data-ttu-id="2f3da-171">Tato metoda vyvolá `InvalidOperationException`(C#) nebo `IllegalStateException`(Java), když se ho pokusí tooadd klíč, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="2f3da-171">This method throws `InvalidOperationException`(C#) or `IllegalStateException`(Java) when it tries tooadd a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

<span data-ttu-id="2f3da-172">Můžete také přidat stavu pomocí *TryAdd* metoda.</span><span class="sxs-lookup"><span data-stu-id="2f3da-172">You can also add state by using a *TryAdd* method.</span></span> <span data-ttu-id="2f3da-173">Tato metoda nevyvolá výjimku, když se ho pokusí tooadd klíč, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="2f3da-173">This method does not throw when it tries tooadd a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

<span data-ttu-id="2f3da-174">Na konci hello metody objektu actor správce stavu hello automaticky uloží všechny hodnoty, které byly přidány nebo upraveném operace insert nebo update.</span><span class="sxs-lookup"><span data-stu-id="2f3da-174">At hello end of an actor method, hello state manager automatically saves any values that have been added or modified by an insert or update operation.</span></span> <span data-ttu-id="2f3da-175">"Uložení" může obsahovat zachování toodisk a replikace, v závislosti na nastavení hello používá.</span><span class="sxs-lookup"><span data-stu-id="2f3da-175">A "save" can include persisting toodisk and replication, depending on hello settings used.</span></span> <span data-ttu-id="2f3da-176">Hodnoty, které nebyly upraveny nejsou jako trvalý, nebo replikovat.</span><span class="sxs-lookup"><span data-stu-id="2f3da-176">Values that have not been modified are not persisted or replicated.</span></span> <span data-ttu-id="2f3da-177">Pokud byly změněny žádné hodnoty, hello operace uložení neprovede žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="2f3da-177">If no values have been modified, hello save operation does nothing.</span></span> <span data-ttu-id="2f3da-178">Pokud ukládání selže, hello upravený stav jsou data zahozena a je znovu hello původního stavu.</span><span class="sxs-lookup"><span data-stu-id="2f3da-178">If saving fails, hello modified state is discarded and hello original state is reloaded.</span></span>

<span data-ttu-id="2f3da-179">Můžete také uložit stav ručně pomocí volání hello `SaveStateAsync` metodu objektu actor hello základní:</span><span class="sxs-lookup"><span data-stu-id="2f3da-179">You can also save state manually by calling hello `SaveStateAsync` method on hello actor base:</span></span>

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a><span data-ttu-id="2f3da-180">Odebrání stavu</span><span class="sxs-lookup"><span data-stu-id="2f3da-180">Removing state</span></span>
<span data-ttu-id="2f3da-181">Můžete odebrat stavu trvale ze Správce stavu objektu actor tak volání hello *odebrat* metoda.</span><span class="sxs-lookup"><span data-stu-id="2f3da-181">You can remove state permanently from an actor's state manager by calling hello *Remove* method.</span></span> <span data-ttu-id="2f3da-182">Tato metoda vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java), když se ho pokusí tooremove klíč, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2f3da-182">This method throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) when it tries tooremove a key that doesn't exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

<span data-ttu-id="2f3da-183">Rovněž můžete odebrat stavu trvale pomocí hello *TryRemove* metoda.</span><span class="sxs-lookup"><span data-stu-id="2f3da-183">You can also remove state permanently by using hello *TryRemove* method.</span></span> <span data-ttu-id="2f3da-184">Tato metoda nevyvolá výjimku, když se ho pokusí tooremove klíč, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2f3da-184">This method does not throw when it tries tooremove a key that does not exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="2f3da-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f3da-185">Next steps</span></span>

<span data-ttu-id="2f3da-186">Stav, který je uložen v Reliable Actors musí být serializován před jeho napsané toodisk a replikovat pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="2f3da-186">State that's stored in Reliable Actors must be serialized before its written toodisk and replicated for high availability.</span></span> <span data-ttu-id="2f3da-187">Další informace o [serializace typu objektu Actor](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="2f3da-187">Learn more about [Actor type serialization](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span>

<span data-ttu-id="2f3da-188">V dalším kroku Další informace o [objektu Actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="2f3da-188">Next, learn more about [Actor diagnostics and performance monitoring](service-fabric-reliable-actors-diagnostics.md).</span></span>
