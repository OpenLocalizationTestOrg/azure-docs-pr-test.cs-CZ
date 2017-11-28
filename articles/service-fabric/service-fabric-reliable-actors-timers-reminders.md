---
title: "aaaReliable aktéři časovače a upomínek | Microsoft Docs"
description: "Tootimers úvod a připomenutí pro Service Fabric Reliable Actors."
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
ms.openlocfilehash: c5116ec1923014e131130b9f4e86dd1e133bbf7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="actor-timers-and-reminders"></a><span data-ttu-id="60a96-103">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="60a96-103">Actor timers and reminders</span></span>
<span data-ttu-id="60a96-104">Aktéři můžete naplánovat pravidelné práce na samotných tak, že zaregistrujete časovače nebo připomenutí.</span><span class="sxs-lookup"><span data-stu-id="60a96-104">Actors can schedule periodic work on themselves by registering either timers or reminders.</span></span> <span data-ttu-id="60a96-105">Tento článek ukazuje, jak toouse časovače a připomenutí a vysvětluje hello rozdíly mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="60a96-105">This article shows how toouse timers and reminders and explains hello differences between them.</span></span>

## <a name="actor-timers"></a><span data-ttu-id="60a96-106">Časovače objektu actor</span><span class="sxs-lookup"><span data-stu-id="60a96-106">Actor timers</span></span>
<span data-ttu-id="60a96-107">Časovače objektu actor poskytují jednoduché obálku kolem časovač tooensure .NET nebo Java, metody zpětného volání hello respektují hello na základě zapnout souběžnosti záruky, které hello aktéři poskytuje modul runtime.</span><span class="sxs-lookup"><span data-stu-id="60a96-107">Actor timers provide a simple wrapper around a .NET or Java timer tooensure that hello callback methods respect hello turn-based concurrency guarantees that hello Actors runtime provides.</span></span>

<span data-ttu-id="60a96-108">Aktéři můžete použít hello `RegisterTimer`(C#) nebo `registerTimer`(Java) a `UnregisterTimer`(C#) nebo `unregisterTimer`metody (Java) na jejich základní třídy tooregister a zrušit jejich časovače.</span><span class="sxs-lookup"><span data-stu-id="60a96-108">Actors can use hello `RegisterTimer`(C#) or `registerTimer`(Java)  and `UnregisterTimer`(C#) or `unregisterTimer`(Java) methods on their base class tooregister and unregister their timers.</span></span> <span data-ttu-id="60a96-109">Následující příklad Hello ukazuje použití hello časovače rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="60a96-109">hello example below shows hello use of timer APIs.</span></span> <span data-ttu-id="60a96-110">Hello rozhraní API jsou velmi podobné časovače .NET toohello nebo Java časovače.</span><span class="sxs-lookup"><span data-stu-id="60a96-110">hello APIs are very similar toohello .NET timer or Java timer.</span></span> <span data-ttu-id="60a96-111">V tomto příkladu po splatnosti, hello časovače hello aktéři runtime zavolá hello `MoveObject`(C#) nebo `moveObject`– metoda (Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-111">In this example, when hello timer is due, hello Actors runtime will call hello `MoveObject`(C#) or `moveObject`(Java) method.</span></span> <span data-ttu-id="60a96-112">Metoda Hello záruku, že toorespect hello na základě zapnout souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="60a96-112">hello method is guaranteed toorespect hello turn-based concurrency.</span></span> <span data-ttu-id="60a96-113">To znamená, že žádné další metody objektu actor nebo zpětná volání časovače nebo připomenutí bude v průběhu až po dokončení provádění této zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="60a96-113">This means that no other actor methods or timer/reminder callbacks will be in progress until this callback completes execution.</span></span>

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
            null,                           // Parameter toopass toohello callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time toodelay before hello callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of hello callback method

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
                            null,                                             // Parameter toopass toohello callback method
                            Duration.ofMillis(10),                            // Amount of time toodelay before hello callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of hello callback method
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

<span data-ttu-id="60a96-114">Hello další období hello časovače spustí po dokončení provádění zpětného volání hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-114">hello next period of hello timer starts after hello callback completes execution.</span></span> <span data-ttu-id="60a96-115">To znamená, že tento časovač hello je zastavena v průběhu hello zpětného volání provádí a jestli je spuštěná při dokončení zpětného volání hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-115">This implies that hello timer is stopped while hello callback is executing and is started when hello callback finishes.</span></span>

<span data-ttu-id="60a96-116">modul runtime aktéři Hello uloží změny provedené správce stavu objektu actor toohello po dokončení zpětné volání hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-116">hello Actors runtime saves changes made toohello actor's State Manager when hello callback finishes.</span></span> <span data-ttu-id="60a96-117">Pokud dojde k chybě při ukládání stavu hello, bude tento objekt actor deaktivovat a aktivuje novou instanci.</span><span class="sxs-lookup"><span data-stu-id="60a96-117">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="60a96-118">Při deaktivaci objektu actor hello jako součást uvolňování paměti, zastaví se všechny časovače.</span><span class="sxs-lookup"><span data-stu-id="60a96-118">All timers are stopped when hello actor is deactivated as part of garbage collection.</span></span> <span data-ttu-id="60a96-119">Poté jsou vyvolány žádná zpětná volání časovače.</span><span class="sxs-lookup"><span data-stu-id="60a96-119">No timer callbacks are invoked after that.</span></span> <span data-ttu-id="60a96-120">Navíc hello aktéři runtime nezachovává žádné informace o hello časovače, které byly spuštěné před deaktivace.</span><span class="sxs-lookup"><span data-stu-id="60a96-120">Also, hello Actors runtime does not retain any information about hello timers that were running before deactivation.</span></span> <span data-ttu-id="60a96-121">Je to toohello objektu actor tooregister žádné časovače, které je nutné při opětovné aktivaci v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-121">It is up toohello actor tooregister any timers that it needs when it is reactivated in hello future.</span></span> <span data-ttu-id="60a96-122">Další informace najdete v tématu část hello na [uvolňování objektu actor](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="60a96-122">For more information, see hello section on [actor garbage collection](service-fabric-reliable-actors-lifecycle.md).</span></span>

## <a name="actor-reminders"></a><span data-ttu-id="60a96-123">Připomenutí objektu actor</span><span class="sxs-lookup"><span data-stu-id="60a96-123">Actor reminders</span></span>
<span data-ttu-id="60a96-124">Připomenutí jsou tootrigger mechanismus trvalé zpětná volání v objektu actor v zadán vícekrát.</span><span class="sxs-lookup"><span data-stu-id="60a96-124">Reminders are a mechanism tootrigger persistent callbacks on an actor at specified times.</span></span> <span data-ttu-id="60a96-125">Podobně jako tootimers je jejich funkce.</span><span class="sxs-lookup"><span data-stu-id="60a96-125">Their functionality is similar tootimers.</span></span> <span data-ttu-id="60a96-126">Ale na rozdíl od časovače, připomenutí aktivaci za všech okolností dokud objektu actor hello explicitně zrušení registrace je nebo je explicitně odstranit objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-126">But unlike timers, reminders are triggered under all circumstances until hello actor explicitly unregisters them or hello actor is explicitly deleted.</span></span> <span data-ttu-id="60a96-127">Konkrétně připomenutí se aktivují napříč objektu actor deaktivací a převzetí služeb při selhání, protože hello aktéři runtime potrvají informace o objektu actor hello připomenutí.</span><span class="sxs-lookup"><span data-stu-id="60a96-127">Specifically, reminders are triggered across actor deactivations and failovers because hello Actors runtime persists information about hello actor's reminders.</span></span>

<span data-ttu-id="60a96-128">tooregister připomenutí volání objektu actor hello `RegisterReminderAsync` metoda zadaný na hello základní třídu, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="60a96-128">tooregister a reminder, an actor calls hello `RegisterReminderAsync` method provided on hello base class, as shown in hello following example:</span></span>

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
            dueTime,    //hello amount of time toodelay before firing hello reminder
            period);    //hello time interval between firing of reminders
}
```

<span data-ttu-id="60a96-129">V tomto příkladu `"Pay cell phone bill"` je název připomenutí hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-129">In this example, `"Pay cell phone bill"` is hello reminder name.</span></span> <span data-ttu-id="60a96-130">Toto je řetězec, který hello používá objektu actor toouniquely identifikovat připomenutí.</span><span class="sxs-lookup"><span data-stu-id="60a96-130">This is a string that hello actor uses toouniquely identify a reminder.</span></span> <span data-ttu-id="60a96-131">`BitConverter.GetBytes(amountInDollars)`(C#) je hello kontext, který je přidružen hello připomenutí.</span><span class="sxs-lookup"><span data-stu-id="60a96-131">`BitConverter.GetBytes(amountInDollars)`(C#) is hello context that is associated with hello reminder.</span></span> <span data-ttu-id="60a96-132">Bude předáno objektu actor back toohello jako argumentu toohello připomenutí zpětného volání, tj. `IRemindable.ReceiveReminderAsync`(C#) nebo `Remindable.receiveReminderAsync`(Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-132">It will be passed back toohello actor as an argument toohello reminder callback, i.e. `IRemindable.ReceiveReminderAsync`(C#) or `Remindable.receiveReminderAsync`(Java).</span></span>

<span data-ttu-id="60a96-133">Aktéři, které používají připomenutí musí implementovat hello `IRemindable` rozhraní, jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-133">Actors that use reminders must implement hello `IRemindable` interface, as shown in hello example below.</span></span>

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

<span data-ttu-id="60a96-134">Když se aktivuje připomenutí hello Reliable Actors runtime vyvolá hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`na hello objektu Actor – metoda (Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-134">When a reminder is triggered, hello Reliable Actors runtime will invoke hello  `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method on hello Actor.</span></span> <span data-ttu-id="60a96-135">Objekt actor můžete zaregistrovat několik připomenutí a hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`(Java) metoda je volána při některé z těchto připomenutí se aktivuje.</span><span class="sxs-lookup"><span data-stu-id="60a96-135">An actor can register multiple reminders, and hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method is invoked when any of those reminders is triggered.</span></span> <span data-ttu-id="60a96-136">Hello objektu actor můžete použít hello připomenutí název, který se předává v toohello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`toofigure – metoda (Java), které položky byla aktivována připomenutí.</span><span class="sxs-lookup"><span data-stu-id="60a96-136">hello actor can use hello reminder name that is passed in toohello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method toofigure out which reminder was triggered.</span></span>

<span data-ttu-id="60a96-137">Hello aktéři runtime uloží stav objektu actor hello při hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`dokončení volání (Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-137">hello Actors runtime saves hello actor's state when hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) call finishes.</span></span> <span data-ttu-id="60a96-138">Pokud dojde k chybě při ukládání stavu hello, bude tento objekt actor deaktivovat a aktivuje novou instanci.</span><span class="sxs-lookup"><span data-stu-id="60a96-138">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="60a96-139">toounregister připomenutí volání objektu actor hello `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda, jak je znázorněno v následující příklady hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-139">toounregister a reminder, an actor calls hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method, as shown in hello examples below.</span></span>

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

<span data-ttu-id="60a96-140">Jako v příkladu nahoře, hello `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda přijímá `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-140">As shown above, hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method accepts an `IActorReminder`(C#) or `ActorReminder`(Java) interface.</span></span> <span data-ttu-id="60a96-141">Hello podporuje základní třída objektu actor `GetReminder`(C#) nebo `getReminder`(Java) metoda, kterou lze použít tooretrieve hello `IActorReminder`(C#) nebo `ActorReminder`(Java) rozhraní předáním v názvu připomenutí hello.</span><span class="sxs-lookup"><span data-stu-id="60a96-141">hello actor base class supports a `GetReminder`(C#) or `getReminder`(Java) method that can be used tooretrieve hello `IActorReminder`(C#) or `ActorReminder`(Java) interface by passing in hello reminder name.</span></span> <span data-ttu-id="60a96-142">To je vhodné, protože objektu actor hello nemusí toopersist hello `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java), který byl vrácen ze hello `RegisterReminder`(C#) nebo `registerReminder`volání metody (Java).</span><span class="sxs-lookup"><span data-stu-id="60a96-142">This is convenient because hello actor does not need toopersist hello `IActorReminder`(C#) or `ActorReminder`(Java) interface that was returned from hello `RegisterReminder`(C#) or `registerReminder`(Java) method call.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60a96-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60a96-143">Next Steps</span></span>
<span data-ttu-id="60a96-144">Další informace o události objektu Actor spolehlivé a vícenásobný přístup:</span><span class="sxs-lookup"><span data-stu-id="60a96-144">Learn about Reliable Actor events and reentrancy:</span></span>
* [<span data-ttu-id="60a96-145">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="60a96-145">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="60a96-146">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="60a96-146">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
