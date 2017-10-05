---
title: "Stav správy Reliable Actors | Microsoft Docs"
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
ms.openlocfilehash: aca8cf2b94e8b746a5cac6af021c7221a29b7345
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-actors-state-management"></a><span data-ttu-id="680c9-103">Spolehlivé aktéři řízení stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-103">Reliable Actors state management</span></span>
<span data-ttu-id="680c9-104">Reliable Actors jsou jedním podprocesem objekty, které může zapouzdřit logiku a stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-104">Reliable Actors are single-threaded objects that can encapsulate both logic and state.</span></span> <span data-ttu-id="680c9-105">Protože aktéři spustit na spolehlivé služby, jejich stavu udržovat spolehlivé pomocí stejné trvalosti a mechanismech replikace, které používá spolehlivé služby.</span><span class="sxs-lookup"><span data-stu-id="680c9-105">Because actors run on Reliable Services, they can maintain state reliably by using the same persistence and replication mechanisms that Reliable Services uses.</span></span> <span data-ttu-id="680c9-106">Tímto způsobem Neztraťte aktéři jejich stav po selhání při opětovné aktivaci po uvolnění paměti, nebo při jejich přesunu mezi uzly v clusteru s podporou kvůli vyrovnávání prostředků nebo upgradu.</span><span class="sxs-lookup"><span data-stu-id="680c9-106">This way, actors don't lose their state after failures, upon reactivation after garbage collection, or when they are moved around between nodes in a cluster due to resource balancing or upgrades.</span></span>

## <a name="state-persistence-and-replication"></a><span data-ttu-id="680c9-107">Trvalost stavu a replikace</span><span class="sxs-lookup"><span data-stu-id="680c9-107">State persistence and replication</span></span>
<span data-ttu-id="680c9-108">Jsou považovány za všechny Reliable Actors *stateful* vzhledem k tomu, že každá instance objektu actor se mapuje na jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="680c9-108">All Reliable Actors are considered *stateful* because each actor instance maps to a unique ID.</span></span> <span data-ttu-id="680c9-109">To znamená, že opakovaná volání stejné ID objektu actor jsou směrované na stejnou instanci objektu actor.</span><span class="sxs-lookup"><span data-stu-id="680c9-109">This means that repeated calls to the same actor ID are routed to the same actor instance.</span></span> <span data-ttu-id="680c9-110">V systému bezstavové naopak volání klienta není zaručena bezpečnost pro směrované na stejný server pokaždé, když.</span><span class="sxs-lookup"><span data-stu-id="680c9-110">In a stateless system, by contrast, client calls are not guaranteed to be routed to the same server every time.</span></span> <span data-ttu-id="680c9-111">Z tohoto důvodu služby objektu actor jsou vždy stavové služby.</span><span class="sxs-lookup"><span data-stu-id="680c9-111">For this reason, actor services are always stateful services.</span></span>

<span data-ttu-id="680c9-112">I když aktéři jsou považovány za stateful, který neznamená, že musí spolehlivě uložení stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-112">Even though actors are considered stateful, that does not mean they must store state reliably.</span></span> <span data-ttu-id="680c9-113">Aktéři můžete vybrat úroveň trvalost stavu a replikace na základě jejich dat požadavky na úložiště:</span><span class="sxs-lookup"><span data-stu-id="680c9-113">Actors can choose the level of state persistence and replication based on their data storage requirements:</span></span>

* <span data-ttu-id="680c9-114">**Trvalý stav**: je uložen stav disku a se replikují do 3 nebo více replik.</span><span class="sxs-lookup"><span data-stu-id="680c9-114">**Persisted state**: State is persisted to disk and is replicated to 3 or more replicas.</span></span> <span data-ttu-id="680c9-115">Toto je nejvíce trvanlivá možnost úložiště stavu, kde můžete zachovat stav prostřednictvím clusterů výpadku.</span><span class="sxs-lookup"><span data-stu-id="680c9-115">This is the most durable state storage option, where state can persist through complete cluster outage.</span></span>
* <span data-ttu-id="680c9-116">**Volatile stavu**: stav se replikují do 3 nebo více replik a pouze uchovává se v paměti.</span><span class="sxs-lookup"><span data-stu-id="680c9-116">**Volatile state**: State is replicated to 3 or more replicas and only kept in memory.</span></span> <span data-ttu-id="680c9-117">To zajišťuje odolnost proti selhání uzlu a selhání objektu actor a během upgradu a vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="680c9-117">This provides resilience against node failure and actor failure, and during upgrades and resource balancing.</span></span> <span data-ttu-id="680c9-118">Však není trvalý stav na disk.</span><span class="sxs-lookup"><span data-stu-id="680c9-118">However, state is not persisted to disk.</span></span> <span data-ttu-id="680c9-119">Proto všechny repliky byly ztraceny najednou, stav dojde ke ztrátě také.</span><span class="sxs-lookup"><span data-stu-id="680c9-119">So if all replicas are lost at once, the state is lost as well.</span></span>
* <span data-ttu-id="680c9-120">**Žádné trvalého stavu**: stav není replikované nebo zapsaný na disk.</span><span class="sxs-lookup"><span data-stu-id="680c9-120">**No persisted state**: State is not replicated or written to disk.</span></span> <span data-ttu-id="680c9-121">Tato úroveň je actor je jednoduše nemusíte spolehlivě Udržovat stav.</span><span class="sxs-lookup"><span data-stu-id="680c9-121">This level is for actors that simply don't need to maintain state reliably.</span></span>

<span data-ttu-id="680c9-122">Každou úroveň trvalost je jednoduše jiné *zprostředkovatele stavu* a *replikace* konfigurace služby.</span><span class="sxs-lookup"><span data-stu-id="680c9-122">Each level of persistence is simply a different *state provider* and *replication* configuration of your service.</span></span> <span data-ttu-id="680c9-123">Zda je stav zapsán do disku závisí na poskytovateli stavu--součástí spolehlivá služba, která ukládá stav.</span><span class="sxs-lookup"><span data-stu-id="680c9-123">Whether or not state is written to disk depends on the state provider--the component in a reliable service that stores state.</span></span> <span data-ttu-id="680c9-124">Replikace závisí na tom, kolik repliky může služba nasazený s.</span><span class="sxs-lookup"><span data-stu-id="680c9-124">Replication depends on how many replicas a service is deployed with.</span></span> <span data-ttu-id="680c9-125">Stejně jako u spolehlivé služby zprostředkovatele stavu i počet replik jde snadno nastavit ručně.</span><span class="sxs-lookup"><span data-stu-id="680c9-125">As with Reliable Services, both the state provider and replica count can easily be set manually.</span></span> <span data-ttu-id="680c9-126">Rozhraní objektu actor poskytuje atribut, který, když použije na objekt actor, automaticky vybere výchozí poskytovatel stavu a automaticky vygeneruje nastavení pro repliky počet dosáhnout jednoho z těchto tří nastavení trvalosti.</span><span class="sxs-lookup"><span data-stu-id="680c9-126">The actor framework provides an attribute that, when used on an actor, automatically selects a default state provider and automatically generates settings for replica count to achieve one of these three persistence settings.</span></span> <span data-ttu-id="680c9-127">Atribut StatePersistence není zdědí odvozené třídy, každý typ objektu Actor musíte zadat jeho StatePersistence úroveň.</span><span class="sxs-lookup"><span data-stu-id="680c9-127">The StatePersistence attribute is not inherited by derived class, each Actor type must provide its StatePersistence level.</span></span>

### <a name="persisted-state"></a><span data-ttu-id="680c9-128">Trvalého stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-128">Persisted state</span></span>
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
<span data-ttu-id="680c9-129">Toto nastavení využívá zprostředkovatele stavu, která ukládá data na disku a automaticky nastaví počet replik služby 3.</span><span class="sxs-lookup"><span data-stu-id="680c9-129">This setting uses a state provider that stores data on disk and automatically sets the service replica count to 3.</span></span>

### <a name="volatile-state"></a><span data-ttu-id="680c9-130">Volatile stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-130">Volatile state</span></span>
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
<span data-ttu-id="680c9-131">Toto nastavení používá poskytovatele v paměti jen stavu a nastaví počet replik na 3.</span><span class="sxs-lookup"><span data-stu-id="680c9-131">This setting uses an in-memory-only state provider and sets the replica count to 3.</span></span>

### <a name="no-persisted-state"></a><span data-ttu-id="680c9-132">Žádné trvalého stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-132">No persisted state</span></span>
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
<span data-ttu-id="680c9-133">Toto nastavení používá poskytovatele v paměti jen stavu a nastaví počet replik na 1.</span><span class="sxs-lookup"><span data-stu-id="680c9-133">This setting uses an in-memory-only state provider and sets the replica count to 1.</span></span>

### <a name="defaults-and-generated-settings"></a><span data-ttu-id="680c9-134">Výchozí hodnoty a vygenerované nastavení</span><span class="sxs-lookup"><span data-stu-id="680c9-134">Defaults and generated settings</span></span>
<span data-ttu-id="680c9-135">Pokud používáte `StatePersistence` atribut zprostředkovatele stavu je automaticky vybrána pro za běhu při spuštění služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="680c9-135">When you're using the `StatePersistence` attribute, a state provider is automatically selected for you at runtime when the actor service starts.</span></span> <span data-ttu-id="680c9-136">Počet replik, ale je nastavena v době kompilace pomocí nástroje sestavení sady Visual Studio objektu actor.</span><span class="sxs-lookup"><span data-stu-id="680c9-136">The replica count, however, is set at compile time by the Visual Studio actor build tools.</span></span> <span data-ttu-id="680c9-137">Nástroje pro sestavení automaticky generovat *výchozí služba* služby objektu actor v ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="680c9-137">The build tools automatically generate a *default service* for the actor service in ApplicationManifest.xml.</span></span> <span data-ttu-id="680c9-138">Vytvoří se pro parametry **sady replik minimální velikost** a **cíl repliky nastavit velikost**.</span><span class="sxs-lookup"><span data-stu-id="680c9-138">Parameters are created for **min replica set size** and **target replica set size**.</span></span>

<span data-ttu-id="680c9-139">Tyto parametry můžete ručně změnit.</span><span class="sxs-lookup"><span data-stu-id="680c9-139">You can change these parameters manually.</span></span> <span data-ttu-id="680c9-140">Ale pokaždé, když `StatePersistence` změně atributu, parametry jsou nastaveny na výchozí hodnoty velikosti sady replik pro vybranou `StatePersistence` atribut přepsání žádné předchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="680c9-140">But each time the `StatePersistence` attribute is changed, the parameters are set to the default replica set size values for the selected `StatePersistence` attribute, overriding any previous values.</span></span> <span data-ttu-id="680c9-141">Jinými slovy, jsou hodnoty, které se nastavují v ServiceManifest.xml *pouze* přepsat v čase vytvoření buildu, když změníte `StatePersistence` hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="680c9-141">In other words, the values that you set in ServiceManifest.xml are *only* overridden at build time when you change the `StatePersistence` attribute value.</span></span>

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

## <a name="state-manager"></a><span data-ttu-id="680c9-142">Správce stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-142">State manager</span></span>
<span data-ttu-id="680c9-143">Všechny instance objektu actor má svou vlastní správce stavu: jako slovník datová struktura, která spolehlivě uchová dvojice klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="680c9-143">Every actor instance has its own state manager: a dictionary-like data structure that reliably stores key/value pairs.</span></span> <span data-ttu-id="680c9-144">Správce stavu je obálku kolem zprostředkovatele stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-144">The state manager is a wrapper around a state provider.</span></span> <span data-ttu-id="680c9-145">Můžete ho použít k ukládání dat bez ohledu na to, který se používá nastavení trvalosti.</span><span class="sxs-lookup"><span data-stu-id="680c9-145">You can use it to store data regardless of which persistence setting is used.</span></span> <span data-ttu-id="680c9-146">Neposkytuje žádné záruky, spuštěné služby objektu actor můžete změnit z nastavení volatile (v paměti jen) stavu tak, aby nastavení trvalého stavu prostřednictvím postupného upgradu při zachování dat.</span><span class="sxs-lookup"><span data-stu-id="680c9-146">It does not provide any guarantees that a running actor service can be changed from a volatile (in-memory-only) state setting to a persisted state setting through a rolling upgrade while preserving data.</span></span> <span data-ttu-id="680c9-147">Je však možné změnit počet replik pro spuštěnou službu.</span><span class="sxs-lookup"><span data-stu-id="680c9-147">However, it is possible to change replica count for a running service.</span></span>

<span data-ttu-id="680c9-148">Správce stavu klíče musí být řetězce.</span><span class="sxs-lookup"><span data-stu-id="680c9-148">State manager keys must be strings.</span></span> <span data-ttu-id="680c9-149">Hodnoty jsou obecné a mohou být jakéhokoli typu, včetně vlastních typů.</span><span class="sxs-lookup"><span data-stu-id="680c9-149">Values are generic and can be any type, including custom types.</span></span> <span data-ttu-id="680c9-150">Hodnotami uloženými v správce stavu musí být serializovatelný kontrakt dat, protože může přenést přes síť do dalších uzlů během replikace a může zapsat na disk, v závislosti na nastavení trvalost stavu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="680c9-150">Values stored in the state manager must be data contract serializable because they might be transmitted over the network to other nodes during replication and might be written to disk, depending on an actor's state persistence setting.</span></span>

<span data-ttu-id="680c9-151">Správce stavu zpřístupní běžné metody slovník pro správu stavu, podobné těm, které jsou obsaženy ve slovníku pro spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="680c9-151">The state manager exposes common dictionary methods for managing state, similar to those found in Reliable Dictionary.</span></span>

### <a name="accessing-state"></a><span data-ttu-id="680c9-152">Přístup ke stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-152">Accessing state</span></span>
<span data-ttu-id="680c9-153">Stav je přístupná prostřednictvím Správce stavu podle klíče.</span><span class="sxs-lookup"><span data-stu-id="680c9-153">State can be accessed through the state manager by key.</span></span> <span data-ttu-id="680c9-154">Metody správce stavu jsou všechny asynchronní, protože v/v disku může vyžadují při aktéři držena formou stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-154">State manager methods are all asynchronous because they might require disk I/O when actors have persisted state.</span></span> <span data-ttu-id="680c9-155">Při prvním přístupu jsou uložená v mezipaměti objektů stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-155">Upon first access, state objects are cached in memory.</span></span> <span data-ttu-id="680c9-156">Opakujte přístup operations přístup k objektům přímo z paměti a vrátí synchronně, aniž by docházelo k vstupně-výstupní diskové nebo asynchronní režií přepínání kontextu.</span><span class="sxs-lookup"><span data-stu-id="680c9-156">Repeat access operations access objects directly from memory and return synchronously without incurring disk I/O or asynchronous context-switching overhead.</span></span> <span data-ttu-id="680c9-157">Stav objektu se odebere z mezipaměti v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="680c9-157">A state object is removed from the cache in the following cases:</span></span>

* <span data-ttu-id="680c9-158">Po ho načte objekt z správce stavu, vyvolá metoda objektu actor k neošetřené výjimce.</span><span class="sxs-lookup"><span data-stu-id="680c9-158">An actor method throws an unhandled exception after it retrieves an object from the state manager.</span></span>
* <span data-ttu-id="680c9-159">Objekt actor je znovu aktivovat, po právě deaktivována nebo po selhání.</span><span class="sxs-lookup"><span data-stu-id="680c9-159">An actor is reactivated, either after being deactivated or after failure.</span></span>
* <span data-ttu-id="680c9-160">Zprostředkovatel stavu stránky stavu na disk.</span><span class="sxs-lookup"><span data-stu-id="680c9-160">The state provider pages state to disk.</span></span> <span data-ttu-id="680c9-161">Tento postup závisí na implementaci zprostředkovatele stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-161">This behavior depends on the state provider implementation.</span></span> <span data-ttu-id="680c9-162">Výchozí zprostředkovatel stavu pro `Persisted` nastavení je toto chování.</span><span class="sxs-lookup"><span data-stu-id="680c9-162">The default state provider for the `Persisted` setting has this behavior.</span></span>

<span data-ttu-id="680c9-163">Stav můžete načíst pomocí standardního *získat* operace, která vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java), pokud položka neexistuje pro klíč:</span><span class="sxs-lookup"><span data-stu-id="680c9-163">You can retrieve state by using a standard *Get* operation that throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) if an entry does not exist for the key:</span></span>

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

<span data-ttu-id="680c9-164">Můžete také načíst stav pomocí *TryGet* metoda, která nevyvolá výjimku pokud záznam neexistuje pro klíč:</span><span class="sxs-lookup"><span data-stu-id="680c9-164">You can also retrieve state by using a *TryGet* method that does not throw if an entry does not exist for a key:</span></span>

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

### <a name="saving-state"></a><span data-ttu-id="680c9-165">Ukládání stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-165">Saving state</span></span>
<span data-ttu-id="680c9-166">Metody načtení stavu manager vrátí odkaz na objekt v místní paměti.</span><span class="sxs-lookup"><span data-stu-id="680c9-166">The state manager retrieval methods return a reference to an object in local memory.</span></span> <span data-ttu-id="680c9-167">Úprava tento objekt v místní paměti samostatně nezpůsobí, je bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="680c9-167">Modifying this object in local memory alone does not cause it to be saved durably.</span></span> <span data-ttu-id="680c9-168">Pokud objekt je načtena z správce stavu a upravit, musíte znovu vložit do Správce stavu bezpečně uložit.</span><span class="sxs-lookup"><span data-stu-id="680c9-168">When an object is retrieved from the state manager and modified, it must be reinserted into the state manager to be saved durably.</span></span>

<span data-ttu-id="680c9-169">Můžete vložit stavu pomocí nepodmíněné *nastavit*, což je ekvivalentem `dictionary["key"] = value` syntaxe:</span><span class="sxs-lookup"><span data-stu-id="680c9-169">You can insert state by using an unconditional *Set*, which is the equivalent of the `dictionary["key"] = value` syntax:</span></span>

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

<span data-ttu-id="680c9-170">Stav můžete přidat pomocí *přidat* metoda.</span><span class="sxs-lookup"><span data-stu-id="680c9-170">You can add state by using an *Add* method.</span></span> <span data-ttu-id="680c9-171">Tato metoda vyvolá `InvalidOperationException`(C#) nebo `IllegalStateException`(Java) při pokusu o přidání klíče, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="680c9-171">This method throws `InvalidOperationException`(C#) or `IllegalStateException`(Java) when it tries to add a key that already exists.</span></span>

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

<span data-ttu-id="680c9-172">Můžete také přidat stavu pomocí *TryAdd* metoda.</span><span class="sxs-lookup"><span data-stu-id="680c9-172">You can also add state by using a *TryAdd* method.</span></span> <span data-ttu-id="680c9-173">Tato metoda nevyvolá výjimku při pokusu o přidání klíče, který již existuje.</span><span class="sxs-lookup"><span data-stu-id="680c9-173">This method does not throw when it tries to add a key that already exists.</span></span>

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

<span data-ttu-id="680c9-174">Na konci metodu objektu actor správce stavu automaticky uloží všechny hodnoty, které byly přidány nebo upraveném operace insert nebo update.</span><span class="sxs-lookup"><span data-stu-id="680c9-174">At the end of an actor method, the state manager automatically saves any values that have been added or modified by an insert or update operation.</span></span> <span data-ttu-id="680c9-175">"Uložení" může obsahovat uložením na disk a replikace, v závislosti na nastavení použít.</span><span class="sxs-lookup"><span data-stu-id="680c9-175">A "save" can include persisting to disk and replication, depending on the settings used.</span></span> <span data-ttu-id="680c9-176">Hodnoty, které nebyly upraveny nejsou jako trvalý, nebo replikovat.</span><span class="sxs-lookup"><span data-stu-id="680c9-176">Values that have not been modified are not persisted or replicated.</span></span> <span data-ttu-id="680c9-177">Pokud byly změněny žádné hodnoty, uložení operace se nic nestane.</span><span class="sxs-lookup"><span data-stu-id="680c9-177">If no values have been modified, the save operation does nothing.</span></span> <span data-ttu-id="680c9-178">Pokud ukládání selže, budou zahozeny upravený stav a je znovu původního stavu.</span><span class="sxs-lookup"><span data-stu-id="680c9-178">If saving fails, the modified state is discarded and the original state is reloaded.</span></span>

<span data-ttu-id="680c9-179">Můžete také uložit stav ručně voláním `SaveStateAsync` metodu objektu actor základní:</span><span class="sxs-lookup"><span data-stu-id="680c9-179">You can also save state manually by calling the `SaveStateAsync` method on the actor base:</span></span>

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

### <a name="removing-state"></a><span data-ttu-id="680c9-180">Odebrání stavu</span><span class="sxs-lookup"><span data-stu-id="680c9-180">Removing state</span></span>
<span data-ttu-id="680c9-181">Můžete odebrat stavu trvale ze Správce stavu objektu actor voláním *odebrat* metoda.</span><span class="sxs-lookup"><span data-stu-id="680c9-181">You can remove state permanently from an actor's state manager by calling the *Remove* method.</span></span> <span data-ttu-id="680c9-182">Tato metoda vyvolá `KeyNotFoundException`(C#) nebo `NoSuchElementException`(Java) při pokusu o odstranění klíče, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="680c9-182">This method throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) when it tries to remove a key that doesn't exist.</span></span>

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

<span data-ttu-id="680c9-183">Rovněž můžete odebrat stavu trvale pomocí *TryRemove* metoda.</span><span class="sxs-lookup"><span data-stu-id="680c9-183">You can also remove state permanently by using the *TryRemove* method.</span></span> <span data-ttu-id="680c9-184">Tato metoda nevyvolá výjimku při pokusu o odebrání klíče, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="680c9-184">This method does not throw when it tries to remove a key that does not exist.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="680c9-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="680c9-185">Next steps</span></span>

<span data-ttu-id="680c9-186">Musí být serializované stavu, který je uložen v Reliable Actors před jeho zapsaný na disk a replikované pro vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="680c9-186">State that's stored in Reliable Actors must be serialized before its written to disk and replicated for high availability.</span></span> <span data-ttu-id="680c9-187">Další informace o [serializace typu objektu Actor](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="680c9-187">Learn more about [Actor type serialization](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span>

<span data-ttu-id="680c9-188">V dalším kroku Další informace o [objektu Actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="680c9-188">Next, learn more about [Actor diagnostics and performance monitoring](service-fabric-reliable-actors-diagnostics.md).</span></span>
