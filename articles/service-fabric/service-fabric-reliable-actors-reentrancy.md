---
title: "aaaReentrancy v na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Úvod tooreentrancy Service Fabric Reliable actors"
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
ms.openlocfilehash: 61c69bcf0f100e075d19ba155954c05789b71761
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-reentrancy"></a><span data-ttu-id="14c92-103">Spolehlivé aktéři vícenásobný přístup</span><span class="sxs-lookup"><span data-stu-id="14c92-103">Reliable Actors reentrancy</span></span>
<span data-ttu-id="14c92-104">modul runtime Reliable Actors Hello, standardně umožňuje vícenásobný přístup na základě kontextu logické volání.</span><span class="sxs-lookup"><span data-stu-id="14c92-104">hello Reliable Actors runtime, by default, allows logical call context-based reentrancy.</span></span> <span data-ttu-id="14c92-105">To umožňuje vícenásobně aktéři toobe přístupné Pokud jsou v hello stejné volání kontextu řetězu.</span><span class="sxs-lookup"><span data-stu-id="14c92-105">This allows for actors toobe reentrant if they are in hello same call context chain.</span></span> <span data-ttu-id="14c92-106">Například objektu Actor A odešle zprávu tooActor B, který odesílá zprávy tooActor C. Jako součást zpracování zprávy hello Pokud objektu Actor C volání objektu Actor A, uvítací zprávu je vícenásobné, takže bude možné.</span><span class="sxs-lookup"><span data-stu-id="14c92-106">For example, Actor A sends a message tooActor B, who sends a message tooActor C. As part of hello message processing, if Actor C calls Actor A, hello message is reentrant, so it will be allowed.</span></span> <span data-ttu-id="14c92-107">Všechny ostatní zprávy, které jsou součástí jiné volání kontextu se zablokuje na objektu Actor A její dokončení zpracování.</span><span class="sxs-lookup"><span data-stu-id="14c92-107">Any other messages that are part of a different call context will be blocked on Actor A until it finishes processing.</span></span>

<span data-ttu-id="14c92-108">Existují dvě možnosti k dispozici pro opětovné zadání objektu actor definované v hello `ActorReentrancyMode` výčtu:</span><span class="sxs-lookup"><span data-stu-id="14c92-108">There are two options available for actor reentrancy defined in hello `ActorReentrancyMode` enum:</span></span>

* <span data-ttu-id="14c92-109">`LogicalCallContext`(výchozí nastavení)</span><span class="sxs-lookup"><span data-stu-id="14c92-109">`LogicalCallContext` (default behavior)</span></span>
* <span data-ttu-id="14c92-110">`Disallowed`– Zakáže vícenásobný přístup</span><span class="sxs-lookup"><span data-stu-id="14c92-110">`Disallowed` - disables reentrancy</span></span>

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
<span data-ttu-id="14c92-111">Vícenásobný přístup se dá nakonfigurovat v `ActorService`na nastavení během registrace.</span><span class="sxs-lookup"><span data-stu-id="14c92-111">Reentrancy can be configured in an `ActorService`'s settings during registration.</span></span> <span data-ttu-id="14c92-112">nastavení Hello platí instancí objektu actor tooall vytvořených pomocí služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="14c92-112">hello setting applies tooall actor instances created in hello actor service.</span></span>

<span data-ttu-id="14c92-113">Hello následující příklad ukazuje služby objektu actor, která nastaví režim vícenásobný přístup hello příliš`ActorReentrancyMode.Disallowed`.</span><span class="sxs-lookup"><span data-stu-id="14c92-113">hello following example shows an actor service that sets hello reentrancy mode too`ActorReentrancyMode.Disallowed`.</span></span> <span data-ttu-id="14c92-114">V tomto případě, pokud objekt actor odešle vícenásobné zpráva tooanother objektu actor, k výjimce typu `FabricException` bude vyvolána.</span><span class="sxs-lookup"><span data-stu-id="14c92-114">In this case, if an actor sends a reentrant message tooanother actor, an exception of type `FabricException` will be thrown.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="14c92-115">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14c92-115">Next steps</span></span>
* <span data-ttu-id="14c92-116">Další informace o opětovné zadání v hello [referenční dokumentace rozhraní API objektu Actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span><span class="sxs-lookup"><span data-stu-id="14c92-116">Learn more about reentrancy in hello [Actor API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span></span>
