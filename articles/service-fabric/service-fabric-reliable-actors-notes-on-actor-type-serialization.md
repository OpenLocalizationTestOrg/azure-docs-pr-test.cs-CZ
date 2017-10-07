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
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a><span data-ttu-id="f3af7-103">Poznámky k Service Fabric Reliable Actors zadejte serializace</span><span class="sxs-lookup"><span data-stu-id="f3af7-103">Notes on Service Fabric Reliable Actors type serialization</span></span>
<span data-ttu-id="f3af7-104">typy výsledků hello úloh vrácených jednotlivých metod v objektu actor rozhraní Hello argumenty všech metod, a musí být objekty uložené ve Správci stavu objektu actor [kontraktů dat serializovatelný](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="f3af7-104">hello arguments of all methods, result types of hello tasks returned by each method in an actor interface, and objects stored in an actor's state manager must be [data contract serializable](https://msdn.microsoft.com/library/ms731923.aspx).</span></span> <span data-ttu-id="f3af7-105">To platí také toohello argumenty hello metody definované v [objektu actor událostí rozhraní](service-fabric-reliable-actors-events.md).</span><span class="sxs-lookup"><span data-stu-id="f3af7-105">This also applies toohello arguments of hello methods defined in [actor event interfaces](service-fabric-reliable-actors-events.md).</span></span> <span data-ttu-id="f3af7-106">(Metody rozhraní události objektu actor vždy vracet typ void.)</span><span class="sxs-lookup"><span data-stu-id="f3af7-106">(Actor event interface methods always return void.)</span></span>

## <a name="custom-data-types"></a><span data-ttu-id="f3af7-107">Vlastní datové typy</span><span class="sxs-lookup"><span data-stu-id="f3af7-107">Custom data types</span></span>
<span data-ttu-id="f3af7-108">V tomto příkladu hello následující rozhraní objektu actor definuje metodu, která vrátí vlastních dat typu s názvem `VoicemailBox`:</span><span class="sxs-lookup"><span data-stu-id="f3af7-108">In this example, hello following actor interface defines a method that returns a custom data type called `VoicemailBox`:</span></span>

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

<span data-ttu-id="f3af7-109">Hello rozhraní je implementováno modulem objektu actor, který používá hello stavu manager toostore `VoicemailBox` objektu:</span><span class="sxs-lookup"><span data-stu-id="f3af7-109">hello interface is implemented by an actor that uses hello state manager toostore a `VoicemailBox` object:</span></span>

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

<span data-ttu-id="f3af7-110">V tomto příkladu hello `VoicemailBox` objekt serializován při:</span><span class="sxs-lookup"><span data-stu-id="f3af7-110">In this example, hello `VoicemailBox` object is serialized when:</span></span>

* <span data-ttu-id="f3af7-111">objekt Hello přenosu mezi instanci objektu actor a volající.</span><span class="sxs-lookup"><span data-stu-id="f3af7-111">hello object is transmitted between an actor instance and a caller.</span></span>
* <span data-ttu-id="f3af7-112">objekt Hello je uložen v hello správce stavu, kde je trvalý toodisk a replikované tooother uzlů.</span><span class="sxs-lookup"><span data-stu-id="f3af7-112">hello object is saved in hello state manager where it is persisted toodisk and replicated tooother nodes.</span></span>

<span data-ttu-id="f3af7-113">framework spolehlivé objektu Actor Hello používá kontraktu serializace.</span><span class="sxs-lookup"><span data-stu-id="f3af7-113">hello Reliable Actor framework uses DataContract serialization.</span></span> <span data-ttu-id="f3af7-114">Proto hello vlastní datové objekty a jejich členové musí být opatřena hello poznámkou **kontraktu** a **DataMember** atributy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="f3af7-114">Therefore, hello custom data objects and their members must be annotated with hello **DataContract** and **DataMember** attributes, respectively.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="f3af7-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3af7-115">Next steps</span></span>
* [<span data-ttu-id="f3af7-116">Kolekce paměti a životního cyklu objektu actor</span><span class="sxs-lookup"><span data-stu-id="f3af7-116">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="f3af7-117">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="f3af7-117">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="f3af7-118">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="f3af7-118">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="f3af7-119">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="f3af7-119">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="f3af7-120">Polymorfismus objektu actor a vzory návrhu objektově orientované</span><span class="sxs-lookup"><span data-stu-id="f3af7-120">Actor polymorphism and object-oriented design patterns</span></span>](service-fabric-reliable-actors-polymorphism.md)
* [<span data-ttu-id="f3af7-121">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="f3af7-121">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
