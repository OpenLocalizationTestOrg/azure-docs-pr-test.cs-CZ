---
title: "Zadejte aaaReliable aktéři poznámky k objektu actor serializace | Microsoft Docs"
description: "Popisuje základní požadavky pro definování serializovatelných tříd, které se dají použít toodefine stavy Service Fabric Reliable Actors a rozhraní"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: d8584e7d90fe1c68af38983e71e5d0a7554689bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Poznámky k Service Fabric Reliable Actors zadejte serializace
typy výsledků hello úloh vrácených jednotlivých metod v objektu actor rozhraní Hello argumenty všech metod, a musí být objekty uložené ve Správci stavu objektu actor [kontraktů dat serializovatelný](https://msdn.microsoft.com/library/ms731923.aspx). To platí také toohello argumenty hello metody definované v [objektu actor událostí rozhraní](service-fabric-reliable-actors-events.md). (Metody rozhraní události objektu actor vždy vracet typ void.)

## <a name="custom-data-types"></a>Vlastní datové typy
V tomto příkladu hello následující rozhraní objektu actor definuje metodu, která vrátí vlastních dat typu s názvem `VoicemailBox`:

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

Hello rozhraní je implementováno modulem objektu actor, který používá hello stavu manager toostore `VoicemailBox` objektu:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

V tomto příkladu hello `VoicemailBox` objekt serializován při:

* objekt Hello přenosu mezi instanci objektu actor a volající.
* objekt Hello je uložen v hello správce stavu, kde je trvalý toodisk a replikované tooother uzlů.

framework spolehlivé objektu Actor Hello používá kontraktu serializace. Proto hello vlastní datové objekty a jejich členové musí být opatřena hello poznámkou **kontraktu** a **DataMember** atributy v uvedeném pořadí.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a>Další kroky
* [Kolekce paměti a životního cyklu objektu actor](service-fabric-reliable-actors-lifecycle.md)
* [Časovače objektu actor a upomínek](service-fabric-reliable-actors-timers-reminders.md)
* [Události objektu actor](service-fabric-reliable-actors-events.md)
* [Vícenásobný přístup objektu actor](service-fabric-reliable-actors-reentrancy.md)
* [Polymorfismus objektu actor a vzory návrhu objektově orientované](service-fabric-reliable-actors-polymorphism.md)
* [Objektu actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md)
