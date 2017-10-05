---
title: "Vícenásobný přístup v na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Úvod do vícenásobný přístup pro Service Fabric Reliable Actors"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 00fcccb379bf1ba3875fbaba57a05b00fa228622
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-actors-reentrancy"></a><span data-ttu-id="01cf1-103">Spolehlivé aktéři vícenásobný přístup</span><span class="sxs-lookup"><span data-stu-id="01cf1-103">Reliable Actors reentrancy</span></span>
<span data-ttu-id="01cf1-104">Modul runtime Reliable Actors standardně umožňuje vícenásobný přístup na základě kontextu logické volání.</span><span class="sxs-lookup"><span data-stu-id="01cf1-104">The Reliable Actors runtime, by default, allows logical call context-based reentrancy.</span></span> <span data-ttu-id="01cf1-105">To umožňuje aktéři být vícenásobné, pokud jsou v tomtéž řetězu volání kontextu.</span><span class="sxs-lookup"><span data-stu-id="01cf1-105">This allows for actors to be reentrant if they are in the same call context chain.</span></span> <span data-ttu-id="01cf1-106">Například objektu Actor A odešle zprávu do objektu Actor B, který odešle zprávu do objektu Actor C. Jako součást zpracování zpráv Pokud objektu Actor C volání objektu Actor A, zpráva je vícenásobné, takže bude možné.</span><span class="sxs-lookup"><span data-stu-id="01cf1-106">For example, Actor A sends a message to Actor B, who sends a message to Actor C. As part of the message processing, if Actor C calls Actor A, the message is reentrant, so it will be allowed.</span></span> <span data-ttu-id="01cf1-107">Všechny ostatní zprávy, které jsou součástí jiné volání kontextu se zablokuje na objektu Actor A její dokončení zpracování.</span><span class="sxs-lookup"><span data-stu-id="01cf1-107">Any other messages that are part of a different call context will be blocked on Actor A until it finishes processing.</span></span>

<span data-ttu-id="01cf1-108">Existují dvě možnosti k dispozici pro opětovné zadání objektu actor definovaný v `ActorReentrancyMode` výčtu:</span><span class="sxs-lookup"><span data-stu-id="01cf1-108">There are two options available for actor reentrancy defined in the `ActorReentrancyMode` enum:</span></span>

* <span data-ttu-id="01cf1-109">`LogicalCallContext`(výchozí nastavení)</span><span class="sxs-lookup"><span data-stu-id="01cf1-109">`LogicalCallContext` (default behavior)</span></span>
* <span data-ttu-id="01cf1-110">`Disallowed`– Zakáže vícenásobný přístup</span><span class="sxs-lookup"><span data-stu-id="01cf1-110">`Disallowed` - disables reentrancy</span></span>

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
<span data-ttu-id="01cf1-111">Vícenásobný přístup se dá nakonfigurovat v `ActorService`na nastavení během registrace.</span><span class="sxs-lookup"><span data-stu-id="01cf1-111">Reentrancy can be configured in an `ActorService`'s settings during registration.</span></span> <span data-ttu-id="01cf1-112">Toto nastavení platí pro všechny instance objektu actor vytvořen v rámci služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="01cf1-112">The setting applies to all actor instances created in the actor service.</span></span>

<span data-ttu-id="01cf1-113">Následující příklad ukazuje služby objektu actor, která nastaví režim vícenásobný přístup k `ActorReentrancyMode.Disallowed`.</span><span class="sxs-lookup"><span data-stu-id="01cf1-113">The following example shows an actor service that sets the reentrancy mode to `ActorReentrancyMode.Disallowed`.</span></span> <span data-ttu-id="01cf1-114">V tomto případě, pokud objekt actor odešle vícenásobné zprávu do jiného objektu actor, k výjimce typu `FabricException` bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="01cf1-114">In this case, if an actor sends a reentrant message to another actor, an exception of type `FabricException` will be thrown.</span></span>

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a><span data-ttu-id="01cf1-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01cf1-115">Next steps</span></span>
* <span data-ttu-id="01cf1-116">Další informace o opětovné zadání v [referenční dokumentace rozhraní API objektu Actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span><span class="sxs-lookup"><span data-stu-id="01cf1-116">Learn more about reentrancy in the [Actor API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span></span>
