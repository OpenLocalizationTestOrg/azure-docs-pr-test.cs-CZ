---
title: "Přehled životního cyklu na základě objektu actor Azure mikroslužeb | Microsoft Docs"
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
ms.openlocfilehash: 75b7b77a0bef2051599a4f61183109cfb2ffff3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a><span data-ttu-id="5a9c4-103">Životní cyklus objektu actor, automatické uvolňování paměti a ruční delete</span><span class="sxs-lookup"><span data-stu-id="5a9c4-103">Actor lifecycle, automatic garbage collection, and manual delete</span></span>
<span data-ttu-id="5a9c4-104">Objekt actor se aktivuje při prvním volání Přišla žádost o jeho metod.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-104">An actor is activated the first time a call is made to any of its methods.</span></span> <span data-ttu-id="5a9c4-105">Objekt actor je deaktivované (paměti shromážděných modulem runtime aktéři), pokud se nepoužívá pro nastaveném časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-105">An actor is deactivated (garbage collected by the Actors runtime) if it is not used for a configurable period of time.</span></span> <span data-ttu-id="5a9c4-106">Objekt actor a její stav lze také odstranit ručně kdykoli.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-106">An actor and its state can also be deleted manually at any time.</span></span>

## <a name="actor-activation"></a><span data-ttu-id="5a9c4-107">Aktivace objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-107">Actor activation</span></span>
<span data-ttu-id="5a9c4-108">Když se aktivuje objektu actor, dojde k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-108">When an actor is activated, the following occurs:</span></span>

* <span data-ttu-id="5a9c4-109">Při volání je teď dostupná pro objekt actor a už není aktivní, se vytvoří nový objekt actor.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-109">When a call comes for an actor and one is not already active, a new actor is created.</span></span>
* <span data-ttu-id="5a9c4-110">Stav objektu actor je načtena, pokud se udržuje stavu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-110">The actor's state is loaded if it's maintaining state.</span></span>
* <span data-ttu-id="5a9c4-111">`OnActivateAsync` (C#) nebo `onActivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor).</span><span class="sxs-lookup"><span data-stu-id="5a9c4-111">The `OnActivateAsync` (C#) or `onActivateAsync` (Java) method (which can be overridden in the actor implementation) is called.</span></span>
* <span data-ttu-id="5a9c4-112">Objektu actor je nyní považována za aktivní.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-112">The actor is now considered active.</span></span>

## <a name="actor-deactivation"></a><span data-ttu-id="5a9c4-113">Deaktivace objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-113">Actor deactivation</span></span>
<span data-ttu-id="5a9c4-114">Když je objekt actor deaktivována, dojde k následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-114">When an actor is deactivated, the following occurs:</span></span>

* <span data-ttu-id="5a9c4-115">Pokud objekt actor nepoužívá pro nějaké časové období, odebere se z tabulky Active aktéři.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-115">When an actor is not used for some period of time, it is removed from the Active Actors table.</span></span>
* <span data-ttu-id="5a9c4-116">`OnDeactivateAsync` (C#) nebo `onDeactivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor).</span><span class="sxs-lookup"><span data-stu-id="5a9c4-116">The `OnDeactivateAsync` (C#) or `onDeactivateAsync` (Java) method (which can be overridden in the actor implementation) is called.</span></span> <span data-ttu-id="5a9c4-117">Zruší všechny časovače pro objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-117">This clears all the timers for the actor.</span></span> <span data-ttu-id="5a9c4-118">Operace objektu actor jako stavu, ve kterém by neměl být volán změny z této metody.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-118">Actor operations like state changes should not be called from this method.</span></span>

> [!TIP]
> <span data-ttu-id="5a9c4-119">Modul runtime Fabric aktéři vysílá některé [události související s objektu actor aktivace a deaktivace](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="5a9c4-119">The Fabric Actors runtime emits some [events related to actor activation and deactivation](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span></span> <span data-ttu-id="5a9c4-120">Jsou užitečné v Diagnostika a sledování výkonu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-120">They are useful in diagnostics and performance monitoring.</span></span>
>
>

### <a name="actor-garbage-collection"></a><span data-ttu-id="5a9c4-121">Uvolňování paměti objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-121">Actor garbage collection</span></span>
<span data-ttu-id="5a9c4-122">Po deaktivaci objektu actor odkazy na objekt actor vydání a může být shromažďují normálně modul common language runtime (CLR) nebo java virtual machine (JVM) systém uvolňování paměti.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-122">When an actor is deactivated, references to the actor object are released and it can be garbage collected normally by the common language runtime (CLR) or java virtual machine (JVM) garbage collector.</span></span> <span data-ttu-id="5a9c4-123">Uvolňování paměti pouze vyčistí objekt actor; provede **není** odebrat stavu uložené v objektu actor správce stavu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-123">Garbage collection only cleans up the actor object; it does **not** remove state stored in the actor's State Manager.</span></span> <span data-ttu-id="5a9c4-124">Při příštím aktivaci objektu actor se vytvoří nový objekt actor a obnovit jeho stav.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-124">The next time the actor is activated, a new actor object is created and its state is restored.</span></span>

<span data-ttu-id="5a9c4-125">Co se počítá jako "se používá" pro účely deaktivace a systém kolekce paměti?</span><span class="sxs-lookup"><span data-stu-id="5a9c4-125">What counts as “being used” for the purpose of deactivation and garbage collection?</span></span>

* <span data-ttu-id="5a9c4-126">Přijímá volání</span><span class="sxs-lookup"><span data-stu-id="5a9c4-126">Receiving a call</span></span>
* <span data-ttu-id="5a9c4-127">`IRemindable.ReceiveReminderAsync`metody volané (platí jenom v případě objektu actor používá připomenutí)</span><span class="sxs-lookup"><span data-stu-id="5a9c4-127">`IRemindable.ReceiveReminderAsync` method being invoked (applicable only if the actor uses reminders)</span></span>

> [!NOTE]
> <span data-ttu-id="5a9c4-128">Pokud objektu actor používá časovače a jeho zpětné volání časovače je volána, nemá **není** , se počítají jako "se používá".</span><span class="sxs-lookup"><span data-stu-id="5a9c4-128">if the actor uses timers and its timer callback is invoked, it does **not** count as "being used".</span></span>
>
>

<span data-ttu-id="5a9c4-129">Předtím, než jsme přejít na podrobné informace o deaktivaci, je důležité určit následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-129">Before we go into the details of deactivation, it is important to define the following terms:</span></span>

* <span data-ttu-id="5a9c4-130">*Interval sledování*.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-130">*Scan interval*.</span></span> <span data-ttu-id="5a9c4-131">Toto je interval, kdy modul runtime aktéři hledá její tabulkou Active aktéři aktéři, které můžete deaktivovat a uklizeny.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-131">This is the interval at which the Actors runtime scans its Active Actors table for actors that can be deactivated and garbage collected.</span></span> <span data-ttu-id="5a9c4-132">Výchozí hodnota je 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-132">The default value for this is 1 minute.</span></span>
* <span data-ttu-id="5a9c4-133">*Časový limit nečinnosti*.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-133">*Idle timeout*.</span></span> <span data-ttu-id="5a9c4-134">Toto je množství času, vyžadující objektu actor zůstat nepoužívané (nečinný), než je možné deaktivovat a uklizeny.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-134">This is the amount of time that an actor needs to remain unused (idle) before it can be deactivated and garbage collected.</span></span> <span data-ttu-id="5a9c4-135">Výchozí hodnota je 60 minut.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-135">The default value for this is 60 minutes.</span></span>

<span data-ttu-id="5a9c4-136">Obvykle není potřeba změnit toto výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-136">Typically, you do not need to change these defaults.</span></span> <span data-ttu-id="5a9c4-137">Ale v případě potřeby tyto intervaly lze změnit prostřednictvím `ActorServiceSettings` při registraci vaší [služby objektu Actor](service-fabric-reliable-actors-platform.md):</span><span class="sxs-lookup"><span data-stu-id="5a9c4-137">However, if necessary, these intervals can be changed through `ActorServiceSettings` when registering your [Actor Service](service-fabric-reliable-actors-platform.md):</span></span>

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
<span data-ttu-id="5a9c4-138">Pro každou aktivní objektu actor uchovává informace runtime objektu actor o množství času, která byla nečinnosti (tzn. ne používá).</span><span class="sxs-lookup"><span data-stu-id="5a9c4-138">For each active actor, the actor runtime keeps track of the amount of time that it has been idle (i.e. not used).</span></span> <span data-ttu-id="5a9c4-139">Modul runtime objektu actor kontroluje každý aktéři každých `ScanIntervalInSeconds` chcete zobrazit, pokud může být paměti shromážděných a shromažďuje ji, pokud to bylo nečinné `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-139">The actor runtime checks each of the actors every `ScanIntervalInSeconds` to see if it can be garbage collected and collects it if it has been idle for `IdleTimeoutInSeconds`.</span></span>

<span data-ttu-id="5a9c4-140">Kdykoliv se používá objektu actor, jeho doba nečinnosti, po nastaven na hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-140">Anytime an actor is used, its idle time is reset to 0.</span></span> <span data-ttu-id="5a9c4-141">Potom může být objektu actor uvolnění z paměti pouze v případě, že znovu zůstane neaktivní, pro `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-141">After this, the actor can be garbage collected only if it again remains idle for `IdleTimeoutInSeconds`.</span></span> <span data-ttu-id="5a9c4-142">Odvolat, aby nebyly použity, pokud je proveden metodu objektu actor rozhraní nebo zpětného volání objektu actor připomenutí považuje za objekt actor.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-142">Recall that an actor is considered to have been used if either an actor interface method or an actor reminder callback is executed.</span></span> <span data-ttu-id="5a9c4-143">Objekt actor je **není** považována za nebyly použity, pokud se spustí jeho zpětné volání časovače.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-143">An actor is **not** considered to have been used if its timer callback is executed.</span></span>

<span data-ttu-id="5a9c4-144">Následující diagram znázorňuje životní cyklus jednoho objektu actor pro ilustraci tyto koncepty.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-144">The following diagram shows the lifecycle of a single actor to illustrate these concepts.</span></span>

![Příklad nečinnosti][1]

<span data-ttu-id="5a9c4-146">Příklad ukazuje na dobu životnosti tohoto objektu actor dopad volání metody objektu actor, připomenutí a časovače.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-146">The example shows the impact of actor method calls, reminders, and timers on the lifetime of this actor.</span></span> <span data-ttu-id="5a9c4-147">Následující body o příkladu jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-147">The following points about the example are worth mentioning:</span></span>

* <span data-ttu-id="5a9c4-148">ScanInterval a IdleTimeout jsou nastaveny na hodnotu 5 a 10 v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-148">ScanInterval and IdleTimeout are set to 5 and 10 respectively.</span></span> <span data-ttu-id="5a9c4-149">(Jednotky, není podstatné tady, vzhledem k tomu, že je naše účel pouze k objasnění konceptu.)</span><span class="sxs-lookup"><span data-stu-id="5a9c4-149">(Units do not matter here, since our purpose is only to illustrate the concept.)</span></span>
* <span data-ttu-id="5a9c4-150">Vyhledávání aktéři na uvolnění z paměti se odehrává na T = 0, 5, 10, 15, 20, 25, jak jsou definovány kontrolu interval 5.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-150">The scan for actors to be garbage collected happens at T=0,5,10,15,20,25, as defined by the scan interval of 5.</span></span>
* <span data-ttu-id="5a9c4-151">Pravidelné časovač vyvolá v T = 4, 8, 12, 16, 20, 24, a provede zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-151">A periodic timer fires at T=4,8,12,16,20,24, and its callback executes.</span></span> <span data-ttu-id="5a9c4-152">Doba nečinnosti objektu actor nemělo vliv.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-152">It does not impact the idle time of the actor.</span></span>
* <span data-ttu-id="5a9c4-153">Volání metody objektu actor v T = 7 obnoví dobu nečinnosti, po na 0 a uvolňování objektu actor zpozdí.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-153">An actor method call at T=7 resets the idle time to 0 and delays the garbage collection of the actor.</span></span>
* <span data-ttu-id="5a9c4-154">Zpětné volání objektu actor připomenutí provede na T = 14 a další zpozdí uvolňování objektu actor.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-154">An actor reminder callback executes at T=14 and further delays the garbage collection of the actor.</span></span>
* <span data-ttu-id="5a9c4-155">Při kontrole kolekce paměti na T = 25 čas nečinnosti objektu actor nakonec překročí časový limit nečinnosti 10 a objektu actor je uvolnění z paměti.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-155">During the garbage collection scan at T=25, the actor's idle time finally exceeds the idle timeout of 10, and the actor is garbage collected.</span></span>

<span data-ttu-id="5a9c4-156">Objekt actor nebude nikdy uvolnění z paměti, zatímco je prováděna jednu z jeho metod, bez ohledu na to, jak dlouho je věnovaný provedení této metody.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-156">An actor will never be garbage collected while it is executing one of its methods, no matter how much time is spent in executing that method.</span></span> <span data-ttu-id="5a9c4-157">Jak už bylo zmíněno dříve, brání spuštění metody rozhraní objektu actor a zpětná volání připomenutí uvolňování resetováním čas nečinnosti objektu actor na hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-157">As mentioned earlier, the execution of actor interface methods and reminder callbacks prevents garbage collection by resetting the actor's idle time to 0.</span></span> <span data-ttu-id="5a9c4-158">Provádění zpětných volání časovače neprovádí vynulování dobu nečinnosti, po na hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-158">The execution of timer callbacks does not reset the idle time to 0.</span></span> <span data-ttu-id="5a9c4-159">Uvolňování objektu actor je však odložené až zpětné volání časovače po dokončení provádění.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-159">However, the garbage collection of the actor is deferred until the timer callback has completed execution.</span></span>

## <a name="deleting-actors-and-their-state"></a><span data-ttu-id="5a9c4-160">Odstraňování aktéři a jejich stavu</span><span class="sxs-lookup"><span data-stu-id="5a9c4-160">Deleting actors and their state</span></span>
<span data-ttu-id="5a9c4-161">Uvolnění paměti deaktivované aktéři pouze vyčistí objekt actor, ale neodebere data, která je uložená v objektu actor správce stavu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-161">Garbage collection of deactivated actors only cleans up the actor object, but it does not remove data that is stored in an actor's State Manager.</span></span> <span data-ttu-id="5a9c4-162">Při opětovné aktivaci objektu actor jeho data se znovu k dispozici k němu pomocí Správce stavu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-162">When an actor is re-activated, its data is again made available to it through the State Manager.</span></span> <span data-ttu-id="5a9c4-163">V případech, kdy aktéři ukládání dat do Správce stavu a jsou deaktivována ale nikdy znovu aktivovat může být nutné vyčistit svá data.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-163">In cases where actors store data in State Manager and are deactivated but never re-activated, it may be necessary to clean up their data.</span></span>

<span data-ttu-id="5a9c4-164">[Služby objektu Actor](service-fabric-reliable-actors-platform.md) poskytuje funkce pro odstranění aktéři ze vzdáleného volající:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-164">The [Actor Service](service-fabric-reliable-actors-platform.md) provides a function for deleting actors from a remote caller:</span></span>

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

<span data-ttu-id="5a9c4-165">Odstranění objektu actor má následující důsledky v závislosti na tom, jestli je aktuálně aktivní objektu actor:</span><span class="sxs-lookup"><span data-stu-id="5a9c4-165">Deleting an actor has the following effects depending on whether or not the actor is currently active:</span></span>

* <span data-ttu-id="5a9c4-166">**Aktivní objektu Actor**</span><span class="sxs-lookup"><span data-stu-id="5a9c4-166">**Active Actor**</span></span>
  * <span data-ttu-id="5a9c4-167">Objektu actor se odebral ze seznamu active aktéři a je deaktivována.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-167">Actor is removed from active actors list and is deactivated.</span></span>
  * <span data-ttu-id="5a9c4-168">Jeho stav se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-168">Its state is deleted permanently.</span></span>
* <span data-ttu-id="5a9c4-169">**Neaktivní objektu Actor**</span><span class="sxs-lookup"><span data-stu-id="5a9c4-169">**Inactive Actor**</span></span>
  * <span data-ttu-id="5a9c4-170">Jeho stav se trvale odstraní.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-170">Its state is deleted permanently.</span></span>

<span data-ttu-id="5a9c4-171">Všimněte si, že objekt actor nelze volat odstranit sám na sobě z jednoho z jeho metody objektu actor, protože objekt actor nelze odstranit, při provádění v rámci kontextu volání objektu actor, ve kterém má modul runtime získat zámek kolem volání objektu actor pro vynucení jednovláknové přístupu.</span><span class="sxs-lookup"><span data-stu-id="5a9c4-171">Note that an actor cannot call delete on itself from one of its actor methods because the actor cannot be deleted while executing within an actor call context, in which the runtime has obtained a lock around the actor call to enforce single-threaded access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a9c4-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a9c4-172">Next steps</span></span>
* [<span data-ttu-id="5a9c4-173">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="5a9c4-173">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="5a9c4-174">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-174">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="5a9c4-175">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-175">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="5a9c4-176">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="5a9c4-176">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="5a9c4-177">Referenční dokumentace rozhraní API objektu actor</span><span class="sxs-lookup"><span data-stu-id="5a9c4-177">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="5a9c4-178">C# ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="5a9c4-178">C# Sample code</span></span>](https://github.com/Azure/servicefabric-samples)
* [<span data-ttu-id="5a9c4-179">Java ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="5a9c4-179">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
