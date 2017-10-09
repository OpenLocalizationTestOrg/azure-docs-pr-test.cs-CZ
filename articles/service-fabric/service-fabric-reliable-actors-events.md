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
# <a name="actor-events"></a>Události objektu actor
Události objektu actor poskytují způsob toosend best effort oznámení z hello objektu actor toohello klientů. Události objektu actor navržených pro komunikaci objektu actor klienta a by se neměla používat pro komunikaci objektu actor actor.

Hello následující kód fragmenty zobrazit jak toouse objektu actor události ve vaší aplikaci.

Definujte rozhraní, které popisuje hello události, které zveřejnil objektu actor hello. Toto rozhraní musí být odvozen od hello `IActorEvents` rozhraní. argumenty Hello hello metody musí být [kontraktů dat serializovatelný](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). metody Hello musí vracet typ void, jako událost oznámení jsou jednou z možností a usilovně.

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
Deklarování hello událostí, které zveřejnil hello objektu actor v rozhraní objektu actor hello.

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
Na straně klienta hello implementaci obslužné rutiny události hello.

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

V klientovi hello vytvořte toohello objektu actor proxy serveru, který publikuje hello událostí a přihlášení k odběru události tooits.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

V případě hello převzetí služeb při selhání objektu actor hello může převzetí služeb při selhání tooa jiným procesem nebo uzel. proxy objektu actor Hello spravuje hello aktivní odběry a automaticky je odběratel znovu. Můžete řídit interval opakovaného předplatné hello prostřednictvím hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` rozhraní API. toounsubscribe, použijte hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` rozhraní API.

V objektu actor hello jednoduše publikování událostí hello při jejich provádění. Pokud existují toohello události odběratele, hello aktéři runtime odešle je hello oznámení.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Další kroky
* [Vícenásobný přístup objektu actor](service-fabric-reliable-actors-reentrancy.md)
* [Objektu actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md)
* [Referenční dokumentace rozhraní API objektu actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# ukázkový kód](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core ukázkový kód](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java ukázkový kód](http://github.com/Azure-Samples/service-fabric-java-getting-started)
