---
title: "aaaEvents v na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Úvod tooevents Service Fabric Reliable actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a51e41c35441a5fea508138968b36a35f0ba6699
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="actor-events"></a><span data-ttu-id="7c025-103">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="7c025-103">Actor events</span></span>
<span data-ttu-id="7c025-104">Události objektu actor poskytují způsob toosend best effort oznámení z hello objektu actor toohello klientů.</span><span class="sxs-lookup"><span data-stu-id="7c025-104">Actor events provide a way toosend best-effort notifications from hello actor toohello clients.</span></span> <span data-ttu-id="7c025-105">Události objektu actor navržených pro komunikaci objektu actor klienta a by se neměla používat pro komunikaci objektu actor actor.</span><span class="sxs-lookup"><span data-stu-id="7c025-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="7c025-106">Hello následující kód fragmenty zobrazit jak toouse objektu actor události ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c025-106">hello following code snippets show how toouse actor events in your application.</span></span>

<span data-ttu-id="7c025-107">Definujte rozhraní, které popisuje hello události, které zveřejnil objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="7c025-107">Define an interface that describes hello events published by hello actor.</span></span> <span data-ttu-id="7c025-108">Toto rozhraní musí být odvozen od hello `IActorEvents` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7c025-108">This interface must be derived from hello `IActorEvents` interface.</span></span> <span data-ttu-id="7c025-109">argumenty Hello hello metody musí být [kontraktů dat serializovatelný](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="7c025-109">hello arguments of hello methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="7c025-110">metody Hello musí vracet typ void, jako událost oznámení jsou jednou z možností a usilovně.</span><span class="sxs-lookup"><span data-stu-id="7c025-110">hello methods must return void, as event notifications are one way and best effort.</span></span>

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
<span data-ttu-id="7c025-111">Deklarování hello událostí, které zveřejnil hello objektu actor v rozhraní objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="7c025-111">Declare hello events published by hello actor in hello actor interface.</span></span>

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
<span data-ttu-id="7c025-112">Na straně klienta hello implementaci obslužné rutiny události hello.</span><span class="sxs-lookup"><span data-stu-id="7c025-112">On hello client side, implement hello event handler.</span></span>

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

<span data-ttu-id="7c025-113">V klientovi hello vytvořte toohello objektu actor proxy serveru, který publikuje hello událostí a přihlášení k odběru události tooits.</span><span class="sxs-lookup"><span data-stu-id="7c025-113">On hello client, create a proxy toohello actor that publishes hello event and subscribe tooits events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="7c025-114">V případě hello převzetí služeb při selhání objektu actor hello může převzetí služeb při selhání tooa jiným procesem nebo uzel.</span><span class="sxs-lookup"><span data-stu-id="7c025-114">In hello event of failovers, hello actor may fail over tooa different process or node.</span></span> <span data-ttu-id="7c025-115">proxy objektu actor Hello spravuje hello aktivní odběry a automaticky je odběratel znovu.</span><span class="sxs-lookup"><span data-stu-id="7c025-115">hello actor proxy manages hello active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="7c025-116">Můžete řídit interval opakovaného předplatné hello prostřednictvím hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7c025-116">You can control hello re-subscription interval through hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="7c025-117">toounsubscribe, použijte hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7c025-117">toounsubscribe, use hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="7c025-118">V objektu actor hello jednoduše publikování událostí hello při jejich provádění.</span><span class="sxs-lookup"><span data-stu-id="7c025-118">On hello actor, simply publish hello events as they happen.</span></span> <span data-ttu-id="7c025-119">Pokud existují toohello události odběratele, hello aktéři runtime odešle je hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="7c025-119">If there are subscribers toohello event, hello Actors runtime will send them hello notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="7c025-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c025-120">Next steps</span></span>
* [<span data-ttu-id="7c025-121">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="7c025-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="7c025-122">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="7c025-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="7c025-123">Referenční dokumentace rozhraní API objektu actor</span><span class="sxs-lookup"><span data-stu-id="7c025-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="7c025-124">C# ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="7c025-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="7c025-125">C# .NET Core ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="7c025-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="7c025-126">Java ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="7c025-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
