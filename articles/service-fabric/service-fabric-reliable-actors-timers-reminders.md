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
# <a name="actor-timers-and-reminders"></a>Časovače objektu actor a upomínek
Aktéři můžete naplánovat pravidelné práce na samotných tak, že zaregistrujete časovače nebo připomenutí. Tento článek ukazuje, jak toouse časovače a připomenutí a vysvětluje hello rozdíly mezi nimi.

## <a name="actor-timers"></a>Časovače objektu actor
Časovače objektu actor poskytují jednoduché obálku kolem časovač tooensure .NET nebo Java, metody zpětného volání hello respektují hello na základě zapnout souběžnosti záruky, které hello aktéři poskytuje modul runtime.

Aktéři můžete použít hello `RegisterTimer`(C#) nebo `registerTimer`(Java) a `UnregisterTimer`(C#) nebo `unregisterTimer`metody (Java) na jejich základní třídy tooregister a zrušit jejich časovače. Následující příklad Hello ukazuje použití hello časovače rozhraní API. Hello rozhraní API jsou velmi podobné časovače .NET toohello nebo Java časovače. V tomto příkladu po splatnosti, hello časovače hello aktéři runtime zavolá hello `MoveObject`(C#) nebo `moveObject`– metoda (Java). Metoda Hello záruku, že toorespect hello na základě zapnout souběžnosti. To znamená, že žádné další metody objektu actor nebo zpětná volání časovače nebo připomenutí bude v průběhu až po dokončení provádění této zpětného volání.

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

Hello další období hello časovače spustí po dokončení provádění zpětného volání hello. To znamená, že tento časovač hello je zastavena v průběhu hello zpětného volání provádí a jestli je spuštěná při dokončení zpětného volání hello.

modul runtime aktéři Hello uloží změny provedené správce stavu objektu actor toohello po dokončení zpětné volání hello. Pokud dojde k chybě při ukládání stavu hello, bude tento objekt actor deaktivovat a aktivuje novou instanci.

Při deaktivaci objektu actor hello jako součást uvolňování paměti, zastaví se všechny časovače. Poté jsou vyvolány žádná zpětná volání časovače. Navíc hello aktéři runtime nezachovává žádné informace o hello časovače, které byly spuštěné před deaktivace. Je to toohello objektu actor tooregister žádné časovače, které je nutné při opětovné aktivaci v budoucnu hello. Další informace najdete v tématu část hello na [uvolňování objektu actor](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Připomenutí objektu actor
Připomenutí jsou tootrigger mechanismus trvalé zpětná volání v objektu actor v zadán vícekrát. Podobně jako tootimers je jejich funkce. Ale na rozdíl od časovače, připomenutí aktivaci za všech okolností dokud objektu actor hello explicitně zrušení registrace je nebo je explicitně odstranit objektu actor hello. Konkrétně připomenutí se aktivují napříč objektu actor deaktivací a převzetí služeb při selhání, protože hello aktéři runtime potrvají informace o objektu actor hello připomenutí.

tooregister připomenutí volání objektu actor hello `RegisterReminderAsync` metoda zadaný na hello základní třídu, jak je znázorněno v hello následující ukázka:

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

V tomto příkladu `"Pay cell phone bill"` je název připomenutí hello. Toto je řetězec, který hello používá objektu actor toouniquely identifikovat připomenutí. `BitConverter.GetBytes(amountInDollars)`(C#) je hello kontext, který je přidružen hello připomenutí. Bude předáno objektu actor back toohello jako argumentu toohello připomenutí zpětného volání, tj. `IRemindable.ReceiveReminderAsync`(C#) nebo `Remindable.receiveReminderAsync`(Java).

Aktéři, které používají připomenutí musí implementovat hello `IRemindable` rozhraní, jak je znázorněno v následujícím příkladu hello.

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

Když se aktivuje připomenutí hello Reliable Actors runtime vyvolá hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`na hello objektu Actor – metoda (Java). Objekt actor můžete zaregistrovat několik připomenutí a hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`(Java) metoda je volána při některé z těchto připomenutí se aktivuje. Hello objektu actor můžete použít hello připomenutí název, který se předává v toohello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`toofigure – metoda (Java), které položky byla aktivována připomenutí.

Hello aktéři runtime uloží stav objektu actor hello při hello `ReceiveReminderAsync`(C#) nebo `receiveReminderAsync`dokončení volání (Java). Pokud dojde k chybě při ukládání stavu hello, bude tento objekt actor deaktivovat a aktivuje novou instanci.

toounregister připomenutí volání objektu actor hello `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda, jak je znázorněno v následující příklady hello.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Jako v příkladu nahoře, hello `UnregisterReminderAsync`(C#) nebo `unregisterReminderAsync`(Java) metoda přijímá `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java). Hello podporuje základní třída objektu actor `GetReminder`(C#) nebo `getReminder`(Java) metoda, kterou lze použít tooretrieve hello `IActorReminder`(C#) nebo `ActorReminder`(Java) rozhraní předáním v názvu připomenutí hello. To je vhodné, protože objektu actor hello nemusí toopersist hello `IActorReminder`(C#) nebo `ActorReminder`rozhraní (Java), který byl vrácen ze hello `RegisterReminder`(C#) nebo `registerReminder`volání metody (Java).

## <a name="next-steps"></a>Další kroky
Další informace o události objektu Actor spolehlivé a vícenásobný přístup:
* [Události objektu actor](service-fabric-reliable-actors-events.md)
* [Vícenásobný přístup objektu actor](service-fabric-reliable-actors-reentrancy.md)
