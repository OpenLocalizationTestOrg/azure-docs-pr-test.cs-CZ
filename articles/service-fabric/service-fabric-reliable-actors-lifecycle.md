---
title: "aaaOverview životního cyklu na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Popisuje spolehlivé objektu Actor prostředků infrastruktury služby životního cyklu, uvolňování paměti a ručně odstranit aktéři a jejich stavu"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a><span data-ttu-id="964ae-103">Životní cyklus objektu actor, automatické uvolňování paměti a ruční delete</span><span class="sxs-lookup"><span data-stu-id="964ae-103">Actor lifecycle, automatic garbage collection, and manual delete</span></span>
<span data-ttu-id="964ae-104">Aktivaci objektu actor hello poprvé Přišla žádost o tooany její metody.</span><span class="sxs-lookup"><span data-stu-id="964ae-104">An actor is activated hello first time a call is made tooany of its methods.</span></span> <span data-ttu-id="964ae-105">Objekt actor je deaktivované (paměti shromažďují hello aktéři runtime), pokud se nepoužívá pro nastaveném časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="964ae-105">An actor is deactivated (garbage collected by hello Actors runtime) if it is not used for a configurable period of time.</span></span> <span data-ttu-id="964ae-106">Objekt actor a její stav lze také odstranit ručně kdykoli.</span><span class="sxs-lookup"><span data-stu-id="964ae-106">An actor and its state can also be deleted manually at any time.</span></span>

## <a name="actor-activation"></a><span data-ttu-id="964ae-107">Aktivace objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-107">Actor activation</span></span>
<span data-ttu-id="964ae-108">Pokud je objekt actor aktivován, dojde k následujícímu hello:</span><span class="sxs-lookup"><span data-stu-id="964ae-108">When an actor is activated, hello following occurs:</span></span>

* <span data-ttu-id="964ae-109">Při volání je teď dostupná pro objekt actor a už není aktivní, se vytvoří nový objekt actor.</span><span class="sxs-lookup"><span data-stu-id="964ae-109">When a call comes for an actor and one is not already active, a new actor is created.</span></span>
* <span data-ttu-id="964ae-110">Pokud se udržuje stav načtení stavu objektu actor Hello.</span><span class="sxs-lookup"><span data-stu-id="964ae-110">hello actor's state is loaded if it's maintaining state.</span></span>
* <span data-ttu-id="964ae-111">Hello `OnActivateAsync` (C#) nebo `onActivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor hello).</span><span class="sxs-lookup"><span data-stu-id="964ae-111">hello `OnActivateAsync` (C#) or `onActivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span>
* <span data-ttu-id="964ae-112">objektu actor Hello je nyní považována za aktivní.</span><span class="sxs-lookup"><span data-stu-id="964ae-112">hello actor is now considered active.</span></span>

## <a name="actor-deactivation"></a><span data-ttu-id="964ae-113">Deaktivace objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-113">Actor deactivation</span></span>
<span data-ttu-id="964ae-114">Při deaktivaci objektu actor, dojde k následujícímu hello:</span><span class="sxs-lookup"><span data-stu-id="964ae-114">When an actor is deactivated, hello following occurs:</span></span>

* <span data-ttu-id="964ae-115">Pokud objekt actor nepoužívá pro nějaké časové období, odebere se z tabulky Active aktéři hello.</span><span class="sxs-lookup"><span data-stu-id="964ae-115">When an actor is not used for some period of time, it is removed from hello Active Actors table.</span></span>
* <span data-ttu-id="964ae-116">Hello `OnDeactivateAsync` (C#) nebo `onDeactivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor hello).</span><span class="sxs-lookup"><span data-stu-id="964ae-116">hello `OnDeactivateAsync` (C#) or `onDeactivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span> <span data-ttu-id="964ae-117">Zruší všechny hello časovače pro objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="964ae-117">This clears all hello timers for hello actor.</span></span> <span data-ttu-id="964ae-118">Operace objektu actor jako stavu, ve kterém by neměl být volán změny z této metody.</span><span class="sxs-lookup"><span data-stu-id="964ae-118">Actor operations like state changes should not be called from this method.</span></span>

> [!TIP]
> <span data-ttu-id="964ae-119">Hello Fabric aktéři runtime vysílá některé [událostí souvisejících tooactor aktivace a deaktivace](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="964ae-119">hello Fabric Actors runtime emits some [events related tooactor activation and deactivation](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span></span> <span data-ttu-id="964ae-120">Jsou užitečné v Diagnostika a sledování výkonu.</span><span class="sxs-lookup"><span data-stu-id="964ae-120">They are useful in diagnostics and performance monitoring.</span></span>
>
>

### <a name="actor-garbage-collection"></a><span data-ttu-id="964ae-121">Uvolňování paměti objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-121">Actor garbage collection</span></span>
<span data-ttu-id="964ae-122">Po deaktivaci objektu actor vydávají objektu actor toohello odkazy a může být shromažďují normálně modul hello common language runtime (CLR) nebo java virtual machine (JVM) systém uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="964ae-122">When an actor is deactivated, references toohello actor object are released and it can be garbage collected normally by hello common language runtime (CLR) or java virtual machine (JVM) garbage collector.</span></span> <span data-ttu-id="964ae-123">Uvolňování paměti pouze vyčistí objektu actor hello; provede **není** odebrat stavu uložené v objektu actor hello správce stavu.</span><span class="sxs-lookup"><span data-stu-id="964ae-123">Garbage collection only cleans up hello actor object; it does **not** remove state stored in hello actor's State Manager.</span></span> <span data-ttu-id="964ae-124">Hello další čas hello objektu actor se aktivuje, se vytvoří nový objekt actor a obnovit jeho stav.</span><span class="sxs-lookup"><span data-stu-id="964ae-124">hello next time hello actor is activated, a new actor object is created and its state is restored.</span></span>

<span data-ttu-id="964ae-125">Co se počítá jako "se používá" hello za účelem deaktivace a systém kolekce paměti?</span><span class="sxs-lookup"><span data-stu-id="964ae-125">What counts as “being used” for hello purpose of deactivation and garbage collection?</span></span>

* <span data-ttu-id="964ae-126">Přijímá volání</span><span class="sxs-lookup"><span data-stu-id="964ae-126">Receiving a call</span></span>
* <span data-ttu-id="964ae-127">`IRemindable.ReceiveReminderAsync`metody volané (platí jenom v případě objektu actor hello používá připomenutí)</span><span class="sxs-lookup"><span data-stu-id="964ae-127">`IRemindable.ReceiveReminderAsync` method being invoked (applicable only if hello actor uses reminders)</span></span>

> [!NOTE]
> <span data-ttu-id="964ae-128">Pokud objektu actor hello používá časovače a jeho zpětné volání časovače je volána, nemá **není** , se počítají jako "se používá".</span><span class="sxs-lookup"><span data-stu-id="964ae-128">if hello actor uses timers and its timer callback is invoked, it does **not** count as "being used".</span></span>
>
>

<span data-ttu-id="964ae-129">Předtím, než jsme přejít do hello podrobnosti o deaktivaci, je důležité toodefine hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="964ae-129">Before we go into hello details of deactivation, it is important toodefine hello following terms:</span></span>

* <span data-ttu-id="964ae-130">*Interval sledování*.</span><span class="sxs-lookup"><span data-stu-id="964ae-130">*Scan interval*.</span></span> <span data-ttu-id="964ae-131">Toto je hello interval, ve které hello aktéři runtime hledá její tabulkou Active aktéři aktéři, které můžete deaktivovat a uklizeny.</span><span class="sxs-lookup"><span data-stu-id="964ae-131">This is hello interval at which hello Actors runtime scans its Active Actors table for actors that can be deactivated and garbage collected.</span></span> <span data-ttu-id="964ae-132">Hello výchozí hodnota je 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="964ae-132">hello default value for this is 1 minute.</span></span>
* <span data-ttu-id="964ae-133">*Časový limit nečinnosti*.</span><span class="sxs-lookup"><span data-stu-id="964ae-133">*Idle timeout*.</span></span> <span data-ttu-id="964ae-134">Toto je hello množství času, že objekt actor potřebuje tooremain nepoužívané (nečinný), než je možné deaktivovat a uklizeny.</span><span class="sxs-lookup"><span data-stu-id="964ae-134">This is hello amount of time that an actor needs tooremain unused (idle) before it can be deactivated and garbage collected.</span></span> <span data-ttu-id="964ae-135">Hello výchozí hodnota je 60 minut.</span><span class="sxs-lookup"><span data-stu-id="964ae-135">hello default value for this is 60 minutes.</span></span>

<span data-ttu-id="964ae-136">Obvykle není nutné toochange tyto výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="964ae-136">Typically, you do not need toochange these defaults.</span></span> <span data-ttu-id="964ae-137">Ale v případě potřeby tyto intervaly lze změnit prostřednictvím `ActorServiceSettings` při registraci vaší [služby objektu Actor](service-fabric-reliable-actors-platform.md):</span><span class="sxs-lookup"><span data-stu-id="964ae-137">However, if necessary, these intervals can be changed through `ActorServiceSettings` when registering your [Actor Service](service-fabric-reliable-actors-platform.md):</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
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
    }
}
```
<span data-ttu-id="964ae-138">Pro každou aktivní objektu actor uchovává informace hello objektu actor runtime o hello množství času, která byla nečinnosti (tzn. ne používá).</span><span class="sxs-lookup"><span data-stu-id="964ae-138">For each active actor, hello actor runtime keeps track of hello amount of time that it has been idle (i.e. not used).</span></span> <span data-ttu-id="964ae-139">modul runtime objektu actor Hello kontroluje každý hello aktéři každých `ScanIntervalInSeconds` toosee, pokud lze paměti shromážděných a shromažďuje ji, pokud to bylo nečinné `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="964ae-139">hello actor runtime checks each of hello actors every `ScanIntervalInSeconds` toosee if it can be garbage collected and collects it if it has been idle for `IdleTimeoutInSeconds`.</span></span>

<span data-ttu-id="964ae-140">Kdykoliv se používá objektu actor, je jeho doba nečinnosti, po resetování too0.</span><span class="sxs-lookup"><span data-stu-id="964ae-140">Anytime an actor is used, its idle time is reset too0.</span></span> <span data-ttu-id="964ae-141">Potom může být objektu actor hello uvolnění z paměti pouze v případě, že znovu zůstane neaktivní, pro `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="964ae-141">After this, hello actor can be garbage collected only if it again remains idle for `IdleTimeoutInSeconds`.</span></span> <span data-ttu-id="964ae-142">Odvolat, aby se považuje za objekt actor toohave byla použít, pokud se spustí metodu objektu actor rozhraní nebo zpětného volání objektu actor připomenutí.</span><span class="sxs-lookup"><span data-stu-id="964ae-142">Recall that an actor is considered toohave been used if either an actor interface method or an actor reminder callback is executed.</span></span> <span data-ttu-id="964ae-143">Objekt actor je **není** považována za toohave byla použít, pokud se spustí jeho zpětné volání časovače.</span><span class="sxs-lookup"><span data-stu-id="964ae-143">An actor is **not** considered toohave been used if its timer callback is executed.</span></span>

<span data-ttu-id="964ae-144">Hello následující diagram znázorňuje životní cyklus hello jednoho objektu actor tooillustrate tyto koncepty.</span><span class="sxs-lookup"><span data-stu-id="964ae-144">hello following diagram shows hello lifecycle of a single actor tooillustrate these concepts.</span></span>

![Příklad nečinnosti][1]

<span data-ttu-id="964ae-146">Hello příklad ukazuje na hello životnost tohoto objektu actor hello dopad volání metody objektu actor, připomenutí a časovače.</span><span class="sxs-lookup"><span data-stu-id="964ae-146">hello example shows hello impact of actor method calls, reminders, and timers on hello lifetime of this actor.</span></span> <span data-ttu-id="964ae-147">Hello následující body o příklad hello jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="964ae-147">hello following points about hello example are worth mentioning:</span></span>

* <span data-ttu-id="964ae-148">ScanInterval a IdleTimeout se nastavují too5 a 10 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="964ae-148">ScanInterval and IdleTimeout are set too5 and 10 respectively.</span></span> <span data-ttu-id="964ae-149">(Jednotky, není podstatné tady, protože naše účel je jenom koncept hello tooillustrate.)</span><span class="sxs-lookup"><span data-stu-id="964ae-149">(Units do not matter here, since our purpose is only tooillustrate hello concept.)</span></span>
* <span data-ttu-id="964ae-150">Hello kontrolu aktéři toobe uvolnění z paměti se odehrává na T = 0, 5, 10, 15, 20, 25, jak jsou definovány interval kontroly hello 5.</span><span class="sxs-lookup"><span data-stu-id="964ae-150">hello scan for actors toobe garbage collected happens at T=0,5,10,15,20,25, as defined by hello scan interval of 5.</span></span>
* <span data-ttu-id="964ae-151">Pravidelné časovač vyvolá v T = 4, 8, 12, 16, 20, 24, a provede zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="964ae-151">A periodic timer fires at T=4,8,12,16,20,24, and its callback executes.</span></span> <span data-ttu-id="964ae-152">Doba nečinnosti hello hello actor neměla vliv.</span><span class="sxs-lookup"><span data-stu-id="964ae-152">It does not impact hello idle time of hello actor.</span></span>
* <span data-ttu-id="964ae-153">Volání metody objektu actor v T = 7 obnoví hello doba nečinnosti, po too0 a zpozdí hello uvolňování actor hello.</span><span class="sxs-lookup"><span data-stu-id="964ae-153">An actor method call at T=7 resets hello idle time too0 and delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="964ae-154">Provede zpětné volání objektu actor připomenutí v T = 14 a další zpoždění hello uvolnění paměti objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="964ae-154">An actor reminder callback executes at T=14 and further delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="964ae-155">Při kontrole kolekce hello uvolňování paměti na T = 25 čas nečinnosti hello objektu actor nakonec překročí časový limit nečinnosti hello 10 a objektu actor hello je uvolnění z paměti.</span><span class="sxs-lookup"><span data-stu-id="964ae-155">During hello garbage collection scan at T=25, hello actor's idle time finally exceeds hello idle timeout of 10, and hello actor is garbage collected.</span></span>

<span data-ttu-id="964ae-156">Objekt actor nebude nikdy uvolnění z paměti, zatímco je prováděna jednu z jeho metod, bez ohledu na to, jak dlouho je věnovaný provedení této metody.</span><span class="sxs-lookup"><span data-stu-id="964ae-156">An actor will never be garbage collected while it is executing one of its methods, no matter how much time is spent in executing that method.</span></span> <span data-ttu-id="964ae-157">Jak už bylo zmíněno dříve, brání hello provádění metody rozhraní objektu actor a zpětná volání připomenutí uvolňování paměti resetováním too0 hello objektu actor doby nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="964ae-157">As mentioned earlier, hello execution of actor interface methods and reminder callbacks prevents garbage collection by resetting hello actor's idle time too0.</span></span> <span data-ttu-id="964ae-158">Hello provádění zpětných volání časovače neprovádí vynulování too0 hello doby nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="964ae-158">hello execution of timer callbacks does not reset hello idle time too0.</span></span> <span data-ttu-id="964ae-159">Uvolňování paměti hello actor hello je však odložené až zpětné volání časovače hello po dokončení provádění.</span><span class="sxs-lookup"><span data-stu-id="964ae-159">However, hello garbage collection of hello actor is deferred until hello timer callback has completed execution.</span></span>

## <a name="deleting-actors-and-their-state"></a><span data-ttu-id="964ae-160">Odstraňování aktéři a jejich stavu</span><span class="sxs-lookup"><span data-stu-id="964ae-160">Deleting actors and their state</span></span>
<span data-ttu-id="964ae-161">Uvolnění paměti deaktivované aktéři pouze vyčistí hello objektu actor objektu, ale neodebere data, která je uložená v objektu actor správce stavu.</span><span class="sxs-lookup"><span data-stu-id="964ae-161">Garbage collection of deactivated actors only cleans up hello actor object, but it does not remove data that is stored in an actor's State Manager.</span></span> <span data-ttu-id="964ae-162">Při opětovné aktivaci objektu actor přišla jeho data znovu k dispozici tooit prostřednictvím hello správce stavu.</span><span class="sxs-lookup"><span data-stu-id="964ae-162">When an actor is re-activated, its data is again made available tooit through hello State Manager.</span></span> <span data-ttu-id="964ae-163">V případech, kdy aktéři ukládání dat do Správce stavu a jsou deaktivována ale nikdy znovu aktivovat může být nutné tooclean zálohovat svá data.</span><span class="sxs-lookup"><span data-stu-id="964ae-163">In cases where actors store data in State Manager and are deactivated but never re-activated, it may be necessary tooclean up their data.</span></span>

<span data-ttu-id="964ae-164">Hello [služby objektu Actor](service-fabric-reliable-actors-platform.md) poskytuje funkce pro odstranění aktéři ze vzdáleného volající:</span><span class="sxs-lookup"><span data-stu-id="964ae-164">hello [Actor Service](service-fabric-reliable-actors-platform.md) provides a function for deleting actors from a remote caller:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="964ae-165">Odstranění objektu actor má následující důsledky v závislosti na tom, jestli je aktuálně aktivní objektu actor hello hello:</span><span class="sxs-lookup"><span data-stu-id="964ae-165">Deleting an actor has hello following effects depending on whether or not hello actor is currently active:</span></span>

* <span data-ttu-id="964ae-166">**Aktivní objektu Actor**</span><span class="sxs-lookup"><span data-stu-id="964ae-166">**Active Actor**</span></span>
  * <span data-ttu-id="964ae-167">Objektu actor se odebral ze seznamu active aktéři a je deaktivována.</span><span class="sxs-lookup"><span data-stu-id="964ae-167">Actor is removed from active actors list and is deactivated.</span></span>
  * <span data-ttu-id="964ae-168">Jeho stav se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="964ae-168">Its state is deleted permanently.</span></span>
* <span data-ttu-id="964ae-169">**Neaktivní objektu Actor**</span><span class="sxs-lookup"><span data-stu-id="964ae-169">**Inactive Actor**</span></span>
  * <span data-ttu-id="964ae-170">Jeho stav se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="964ae-170">Its state is deleted permanently.</span></span>

<span data-ttu-id="964ae-171">Všimněte si, že objekt actor nelze volat odstranit sám na sobě z jednoho z jeho metody objektu actor, protože objektu actor hello nelze odstranit, při provádění v rámci kontextu volání objektu actor, ve které hello runtime má získat zámek kolem hello objektu actor volání tooenforce jednovláknové přístup.</span><span class="sxs-lookup"><span data-stu-id="964ae-171">Note that an actor cannot call delete on itself from one of its actor methods because hello actor cannot be deleted while executing within an actor call context, in which hello runtime has obtained a lock around hello actor call tooenforce single-threaded access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="964ae-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="964ae-172">Next steps</span></span>
* [<span data-ttu-id="964ae-173">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="964ae-173">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="964ae-174">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-174">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="964ae-175">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-175">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="964ae-176">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="964ae-176">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="964ae-177">Referenční dokumentace rozhraní API objektu actor</span><span class="sxs-lookup"><span data-stu-id="964ae-177">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="964ae-178">C# ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="964ae-178">C# Sample code</span></span>](https://github.com/Azure/servicefabric-samples)
* [<span data-ttu-id="964ae-179">Java ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="964ae-179">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
