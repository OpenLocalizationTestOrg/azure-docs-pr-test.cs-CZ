---
title: "Zadejte spolehlivé aktéři poznámky k objektu actor serializace | Microsoft Docs"
description: "Popisuje základní požadavky pro definování serializovatelných tříd, které lze použít k definování stavy Service Fabric Reliable Actors a rozhraní"
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
ms.openlocfilehash: 4b48b893e5a3bf5620f00a336576efe1ad63def8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a><span data-ttu-id="5863e-103">Poznámky k Service Fabric Reliable Actors zadejte serializace</span><span class="sxs-lookup"><span data-stu-id="5863e-103">Notes on Service Fabric Reliable Actors type serialization</span></span>
<span data-ttu-id="5863e-104">Argumenty všech metod, typy výsledků úlohy vrácený jednotlivých metod v objektu actor rozhraní a musí být objekty uložené ve Správci stavu objektu actor [kontraktů dat serializovatelný](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="5863e-104">The arguments of all methods, result types of the tasks returned by each method in an actor interface, and objects stored in an actor's state manager must be [data contract serializable](https://msdn.microsoft.com/library/ms731923.aspx).</span></span> <span data-ttu-id="5863e-105">To platí také pro argumenty metody definované v [objektu actor událostí rozhraní](service-fabric-reliable-actors-events.md).</span><span class="sxs-lookup"><span data-stu-id="5863e-105">This also applies to the arguments of the methods defined in [actor event interfaces](service-fabric-reliable-actors-events.md).</span></span> <span data-ttu-id="5863e-106">(Metody rozhraní události objektu actor vždy vracet typ void.)</span><span class="sxs-lookup"><span data-stu-id="5863e-106">(Actor event interface methods always return void.)</span></span>

## <a name="custom-data-types"></a><span data-ttu-id="5863e-107">Vlastní datové typy</span><span class="sxs-lookup"><span data-stu-id="5863e-107">Custom data types</span></span>
<span data-ttu-id="5863e-108">V tomto příkladu následující rozhraní objektu actor definuje metodu, která vrátí vlastních dat typu s názvem `VoicemailBox`:</span><span class="sxs-lookup"><span data-stu-id="5863e-108">In this example, the following actor interface defines a method that returns a custom data type called `VoicemailBox`:</span></span>

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

<span data-ttu-id="5863e-109">Rozhraní je implementováno modulem objektu actor, který používá správce stavu k uložení `VoicemailBox` objektu:</span><span class="sxs-lookup"><span data-stu-id="5863e-109">The interface is implemented by an actor that uses the state manager to store a `VoicemailBox` object:</span></span>

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

<span data-ttu-id="5863e-110">V tomto příkladu `VoicemailBox` objekt serializován při:</span><span class="sxs-lookup"><span data-stu-id="5863e-110">In this example, the `VoicemailBox` object is serialized when:</span></span>

* <span data-ttu-id="5863e-111">Objekt se přenášejí mezi instancí objektu actor a volající.</span><span class="sxs-lookup"><span data-stu-id="5863e-111">The object is transmitted between an actor instance and a caller.</span></span>
* <span data-ttu-id="5863e-112">Objekt se uloží do Správce stavu, kde je uložit trvale na disk a replikované do dalších uzlů.</span><span class="sxs-lookup"><span data-stu-id="5863e-112">The object is saved in the state manager where it is persisted to disk and replicated to other nodes.</span></span>

<span data-ttu-id="5863e-113">Rozhraní objektu Actor spolehlivé používá kontraktu serializace.</span><span class="sxs-lookup"><span data-stu-id="5863e-113">The Reliable Actor framework uses DataContract serialization.</span></span> <span data-ttu-id="5863e-114">Proto vlastní datové objekty a jejich členové musí být opatřena poznámkou **kontraktu** a **DataMember** atributy v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5863e-114">Therefore, the custom data objects and their members must be annotated with the **DataContract** and **DataMember** attributes, respectively.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="5863e-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5863e-115">Next steps</span></span>
* [<span data-ttu-id="5863e-116">Kolekce paměti a životního cyklu objektu actor</span><span class="sxs-lookup"><span data-stu-id="5863e-116">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="5863e-117">Časovače objektu actor a upomínek</span><span class="sxs-lookup"><span data-stu-id="5863e-117">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="5863e-118">Události objektu actor</span><span class="sxs-lookup"><span data-stu-id="5863e-118">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="5863e-119">Vícenásobný přístup objektu actor</span><span class="sxs-lookup"><span data-stu-id="5863e-119">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="5863e-120">Polymorfismus objektu actor a vzory návrhu objektově orientované</span><span class="sxs-lookup"><span data-stu-id="5863e-120">Actor polymorphism and object-oriented design patterns</span></span>](service-fabric-reliable-actors-polymorphism.md)
* [<span data-ttu-id="5863e-121">Objektu actor Diagnostika a sledování výkonu</span><span class="sxs-lookup"><span data-stu-id="5863e-121">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
