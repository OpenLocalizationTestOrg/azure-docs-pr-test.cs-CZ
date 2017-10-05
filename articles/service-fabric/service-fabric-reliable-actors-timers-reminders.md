---
title: "Spolehlivé aktéři časovače a upomínek | Microsoft Docs"
description: "Úvod do časovače a upomínek pro Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 06b026ce06e0f16a77ac238de0af2263f272933c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="actor-timers-and-reminders"></a><span data-ttu-id="67984-103">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="67984-103">Actor timers and reminders</span></span>
<span data-ttu-id="67984-104">Aktéři můžete naplánovat pravidelné práce na samotných tak, že zaregistrujete časovače nebo připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-104">Actors can schedule periodic work on themselves by registering either timers or reminders.</span></span> <span data-ttu-id="67984-105">Tento článek ukazuje, jak použít časovačů a připomenutí a vysvětluje rozdíly mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="67984-105">This article shows how to use timers and reminders and explains the differences between them.</span></span>

## <a name="actor-timers"></a><span data-ttu-id="67984-106">Časovače objektu actor</span><span class="sxs-lookup"><span data-stu-id="67984-106">Actor timers</span></span>
<span data-ttu-id="67984-107">Časovače objektu actor poskytují jednoduché obálku kolem .NET nebo Java časovač zajistit, že metody zpětného volání respektují souběžnosti na základě zapnout zaručuje, že poskytuje modul runtime aktéři.</span><span class="sxs-lookup"><span data-stu-id="67984-107">Actor timers provide a simple wrapper around a .NET or Java timer to ensure that the callback methods respect the turn-based concurrency guarantees that the Actors runtime provides.</span></span>

<span data-ttu-id="67984-108">Můžete použít aktéři `RegisterTimer`(C#) nebo `registerTimer`(Java) a `UnregisterTimer`(C#) nebo `unregisterTimer`metody (Java) na jejich základní třída pro registraci a zrušit jejich časovače.</span><span class="sxs-lookup"><span data-stu-id="67984-108">Actors can use the `RegisterTimer`(C#) or `registerTimer`(Java)  and `UnregisterTimer`(C#) or `unregisterTimer`(Java) methods on their base class to register and unregister their timers.</span></span> <span data-ttu-id="67984-109">Následující příklad ukazuje použití časovače rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="67984-109">The example below shows the use of timer APIs.</span></span> <span data-ttu-id="67984-110">Rozhraní API jsou velmi podobné .NET časovačem nebo Java časovače.</span><span class="sxs-lookup"><span data-stu-id="67984-110">The APIs are very similar to the .NET timer or Java timer.</span></span> <span data-ttu-id="67984-111">V tomto příkladu po splatnosti, časovač runtime aktéři zavolá `MoveObject`(C#) nebo `moveObject`– metoda (Java).</span><span class="sxs-lookup"><span data-stu-id="67984-111">In this example, when the timer is due, the Actors runtime will call the `MoveObject`(C#) or `moveObject`(Java) method.</span></span> <span data-ttu-id="67984-112">Metoda záruku, respektují souběžnosti na základě vypněte.</span><span class="sxs-lookup"><span data-stu-id="67984-112">The method is guaranteed to respect the turn-based concurrency.</span></span> <span data-ttu-id="67984-113">To znamená, že žádné další metody objektu actor nebo zpětná volání časovače nebo připomenutí bude v průběhu až po dokončení provádění této zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="67984-113">This means that no other actor methods or timer/reminder callbacks will be in progress until this callback completes execution.</span></span>

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter to pass to the callback method
                            Duration.ofMillis(10),                            // Amount of time to delay before the callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of the callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

<span data-ttu-id="67984-114">Další období časovač spustí po dokončení provádění zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="67984-114">The next period of the timer starts after the callback completes execution.</span></span> <span data-ttu-id="67984-115">To znamená, že časovač je zastavena v průběhu zpětné volání provádí a jestli je spuštěná při dokončení zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="67984-115">This implies that the timer is stopped while the callback is executing and is started when the callback finishes.</span></span>

<span data-ttu-id="67984-116">Modul runtime aktéři uloží změny provedené správce stavu objektu actor po dokončení zpětné volání.</span><span class="sxs-lookup"><span data-stu-id="67984-116">The Actors runtime saves changes made to the actor's State Manager when the callback finishes.</span></span> <span data-ttu-id="67984-117">Pokud dojde k chybě při ukládání stavu, bude tento objekt actor deaktivovat a aktivuje novou instanci.</span><span class="sxs-lookup"><span data-stu-id="67984-117">If an error occurs in saving the state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="67984-118">Při deaktivaci objektu actor v rámci uvolňování paměti, zastaví se všechny časovače.</span><span class="sxs-lookup"><span data-stu-id="67984-118">All timers are stopped when the actor is deactivated as part of garbage collection.</span></span> <span data-ttu-id="67984-119">Poté jsou vyvolány žádná zpětná volání časovače.</span><span class="sxs-lookup"><span data-stu-id="67984-119">No timer callbacks are invoked after that.</span></span> <span data-ttu-id="67984-120">Modul runtime aktéři navíc nezachovává žádné informace o časovače, které byly spuštěné před deaktivace.</span><span class="sxs-lookup"><span data-stu-id="67984-120">Also, the Actors runtime does not retain any information about the timers that were running before deactivation.</span></span> <span data-ttu-id="67984-121">Je objektu actor k registraci všech časovače, které je nutné, když se znovu aktivuje v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="67984-121">It is up to the actor to register any timers that it needs when it is reactivated in the future.</span></span> <span data-ttu-id="67984-122">Další informace najdete v části na [uvolňování objektu actor](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="67984-122">For more information, see the section on [actor garbage collection](service-fabric-reliable-actors-lifecycle.md).</span></span>

## <a name="actor-reminders"></a><span data-ttu-id="67984-123">Připomenutí objektu actor</span><span class="sxs-lookup"><span data-stu-id="67984-123">Actor reminders</span></span>
<span data-ttu-id="67984-124">Připomenutí jsou mechanismus pro aktivaci trvalé zpětná volání v objektu actor časech.</span><span class="sxs-lookup"><span data-stu-id="67984-124">Reminders are a mechanism to trigger persistent callbacks on an actor at specified times.</span></span> <span data-ttu-id="67984-125">Jejich funkce je podobná časovače.</span><span class="sxs-lookup"><span data-stu-id="67984-125">Their functionality is similar to timers.</span></span> <span data-ttu-id="67984-126">Ale na rozdíl od časovače, připomenutí aktivaci za všech okolností dokud objektu actor explicitně Odregistruje nebo objektu actor je explicitně odstranit.</span><span class="sxs-lookup"><span data-stu-id="67984-126">But unlike timers, reminders are triggered under all circumstances until the actor explicitly unregisters them or the actor is explicitly deleted.</span></span> <span data-ttu-id="67984-127">Konkrétně připomenutí se aktivují napříč objektu actor deaktivací a převzetí služeb při selhání, protože runtime aktéři potrvají informace o objektu actor připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-127">Specifically, reminders are triggered across actor deactivations and failovers because the Actors runtime persists information about the actor's reminders.</span></span>

<span data-ttu-id="67984-128">K registraci, zobrazí se připomenutí, volání objektu actor `RegisterReminderAsync` metoda zadaný na základní třídy, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="67984-128">To register a reminder, an actor calls the `RegisterReminderAsync` method provided on the base class, as shown in the following example:</span></span>

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //The amount of time to delay before firing the reminder
            period);    //The time interval between firing of reminders
}
```

<span data-ttu-id="67984-129">V tomto příkladu `"Pay cell phone bill"` je název připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-129">In this example, `"Pay cell phone bill"` is the reminder name.</span></span> <span data-ttu-id="67984-130">Toto je řetězec, který objektu actor používá k jedinečné identifikaci připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-130">This is a string that the actor uses to uniquely identify a reminder.</span></span> <span data-ttu-id="67984-131">`BitConverter.GetBytes(amountInDollars)`(C#) je kontext, který je přidružen připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-131">`BitConverter.GetBytes(amountInDollars)`(C#) is the context that is associated with the reminder.</span></span> <span data-ttu-id="67984-132">Bude předáno zpět do objektu actor jako argument funkci zpětného volání připomenutí tj `IRemindable.ReceiveReminderAsync`(C#) nebo `Remindable.receiveReminderAsync`(Java).</span><span class="sxs-lookup"><span data-stu-id="67984-132">It will be passed back to the actor as an argument to the reminder callback, i.e. `IRemindable.ReceiveReminderAsync`(C#) or `Remindable.receiveReminderAsync`(Java).</span></span>

<span data-ttu-id="67984-133">Aktéři, které používají připomenutí musí implementovat `IRemindable` rozhraní, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="67984-133">Actors that use reminders must implement the `IRemindable` interface, as shown in the example below.</span></span>

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

<span data-ttu-id="67984-134">Když se aktivuje připomenutí bude vyvolání runtime Reliable Actors `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`(Java) metoda objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="67984-134">When a reminder is triggered, the Reliable Actors runtime will invoke the  `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method on the Actor.</span></span> <span data-ttu-id="67984-135">Objekt actor můžete zaregistrovat několik připomenutí a `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`(Java) metoda je volána při některé z těchto připomenutí se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="67984-135">An actor can register multiple reminders, and the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method is invoked when any of those reminders is triggered.</span></span> <span data-ttu-id="67984-136">Objektu actor můžete použít připomenutí název, který je předán `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`(Java) metoda zjistěte, které připomenutí byla aktivována.</span><span class="sxs-lookup"><span data-stu-id="67984-136">The actor can use the reminder name that is passed in to the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method to figure out which reminder was triggered.</span></span>

<span data-ttu-id="67984-137">Aktéři runtime uloží objektu actor stavu, kdy `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`dokončení volání (Java).</span><span class="sxs-lookup"><span data-stu-id="67984-137">The Actors runtime saves the actor's state when the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) call finishes.</span></span> <span data-ttu-id="67984-138">Pokud dojde k chybě při ukládání stavu, bude tento objekt actor deaktivovat a aktivuje novou instanci.</span><span class="sxs-lookup"><span data-stu-id="67984-138">If an error occurs in saving the state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="67984-139">Zrušení registrace připomenutí, volání objektu actor `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda, jak je znázorněno v následujících příkladech.</span><span class="sxs-lookup"><span data-stu-id="67984-139">To unregister a reminder, an actor calls the `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method, as shown in the examples below.</span></span>

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

<span data-ttu-id="67984-140">Jako v příkladu nahoře, `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda přijímá `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java).</span><span class="sxs-lookup"><span data-stu-id="67984-140">As shown above, the `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method accepts an `IActorReminder`(C#) or `ActorReminder`(Java) interface.</span></span> <span data-ttu-id="67984-141">Podporuje základní třída objektu actor `GetReminder`(C#) nebo `getReminder`(Java) metoda, která slouží k načtení `IActorReminder`(C#) nebo `ActorReminder`(Java) rozhraní předáním v názvu připomenutí.</span><span class="sxs-lookup"><span data-stu-id="67984-141">The actor base class supports a `GetReminder`(C#) or `getReminder`(Java) method that can be used to retrieve the `IActorReminder`(C#) or `ActorReminder`(Java) interface by passing in the reminder name.</span></span> <span data-ttu-id="67984-142">To je vhodné, protože není potřeba zachovat objektu actor `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java), který byl vrácen ze `RegisterReminder`(C#) nebo `registerReminder`volání metody (Java).</span><span class="sxs-lookup"><span data-stu-id="67984-142">This is convenient because the actor does not need to persist the `IActorReminder`(C#) or `ActorReminder`(Java) interface that was returned from the `RegisterReminder`(C#) or `registerReminder`(Java) method call.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67984-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67984-143">Next Steps</span></span>
<span data-ttu-id="67984-144">Další informace o události objektu Actor spolehlivé a vícenásobný přístup:</span><span class="sxs-lookup"><span data-stu-id="67984-144">Learn about Reliable Actor events and reentrancy:</span></span>
* [<span data-ttu-id="67984-145">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="67984-145">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="67984-146">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="67984-146">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
