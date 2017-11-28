---
title: "Události v na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Úvod do události pro Service Fabric Reliable Actors."
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
ms.openlocfilehash: d936670c548ff709fc2e935d3f28d94e4bde8a04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="actor-events"></a><span data-ttu-id="11319-103">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="11319-103">Actor events</span></span>
<span data-ttu-id="11319-104">Události objektu actor poskytují způsob, jak odeslat oznámení best effort z objektu actor pro klienty.</span><span class="sxs-lookup"><span data-stu-id="11319-104">Actor events provide a way to send best-effort notifications from the actor to the clients.</span></span> <span data-ttu-id="11319-105">Události objektu actor navržených pro komunikaci objektu actor klienta a by se neměla používat pro komunikaci objektu actor actor.</span><span class="sxs-lookup"><span data-stu-id="11319-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="11319-106">Následující fragmenty kódu ukazují, jak pomocí objektu actor události ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="11319-106">The following code snippets show how to use actor events in your application.</span></span>

<span data-ttu-id="11319-107">Definujte rozhraní, které popisuje události, které zveřejnil objektu actor.</span><span class="sxs-lookup"><span data-stu-id="11319-107">Define an interface that describes the events published by the actor.</span></span> <span data-ttu-id="11319-108">Toto rozhraní musí být odvozen od `IActorEvents` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="11319-108">This interface must be derived from the `IActorEvents` interface.</span></span> <span data-ttu-id="11319-109">Argumenty metody musí být [kontraktů dat serializovatelný](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="11319-109">The arguments of the methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="11319-110">Metody musí vracet typ void, jako událost oznámení jsou jednou z možností a usilovně.</span><span class="sxs-lookup"><span data-stu-id="11319-110">The methods must return void, as event notifications are one way and best effort.</span></span>

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
<span data-ttu-id="11319-111">Události, které zveřejnil objektu actor v objektu actor rozhraní deklarujte.</span><span class="sxs-lookup"><span data-stu-id="11319-111">Declare the events published by the actor in the actor interface.</span></span>

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
<span data-ttu-id="11319-112">Na straně klienta Implementujte obslužné rutiny události.</span><span class="sxs-lookup"><span data-stu-id="11319-112">On the client side, implement the event handler.</span></span>

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

<span data-ttu-id="11319-113">Na klientovi vytvoření proxy server k objektu actor, který publikuje události a přihlásit se k události.</span><span class="sxs-lookup"><span data-stu-id="11319-113">On the client, create a proxy to the actor that publishes the event and subscribe to its events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="11319-114">V případě převzetí služeb při selhání objektu actor může převzetí služeb při selhání jiným procesem nebo uzel.</span><span class="sxs-lookup"><span data-stu-id="11319-114">In the event of failovers, the actor may fail over to a different process or node.</span></span> <span data-ttu-id="11319-115">Proxy objektu actor spravuje aktivní odběry a automaticky je odběratel znovu.</span><span class="sxs-lookup"><span data-stu-id="11319-115">The actor proxy manages the active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="11319-116">Můžete řídit interval opakovaného předplatné prostřednictvím `ActorProxyEventExtensions.SubscribeAsync<TEvent>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="11319-116">You can control the re-subscription interval through the `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="11319-117">Chcete-li odhlásit, použijte `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="11319-117">To unsubscribe, use the `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="11319-118">V objektu actor jednoduše publikujte události při jejich provádění.</span><span class="sxs-lookup"><span data-stu-id="11319-118">On the actor, simply publish the events as they happen.</span></span> <span data-ttu-id="11319-119">Pokud existují odběratele, kteří mají událost, modul runtime aktéři odešle je oznámení.</span><span class="sxs-lookup"><span data-stu-id="11319-119">If there are subscribers to the event, the Actors runtime will send them the notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="11319-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11319-120">Next steps</span></span>
* [<span data-ttu-id="11319-121">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="11319-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="11319-122">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="11319-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="11319-123">Referenční dokumentace rozhraní API objektu actor</span><span class="sxs-lookup"><span data-stu-id="11319-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="11319-124">C# ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="11319-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="11319-125">C# .NET Core ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="11319-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="11319-126">Java ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="11319-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
