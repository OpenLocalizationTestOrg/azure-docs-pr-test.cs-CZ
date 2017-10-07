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
# <a name="reliable-actors-reentrancy"></a>Spolehlivé aktéři vícenásobný přístup
modul runtime Reliable Actors Hello, standardně umožňuje vícenásobný přístup na základě kontextu logické volání. To umožňuje vícenásobně aktéři toobe přístupné Pokud jsou v hello stejné volání kontextu řetězu. Například objektu Actor A odešle zprávu tooActor B, který odesílá zprávy tooActor C. Jako součást zpracování zprávy hello Pokud objektu Actor C volání objektu Actor A, uvítací zprávu je vícenásobné, takže bude možné. Všechny ostatní zprávy, které jsou součástí jiné volání kontextu se zablokuje na objektu Actor A její dokončení zpracování.

Existují dvě možnosti k dispozici pro opětovné zadání objektu actor definované v hello `ActorReentrancyMode` výčtu:

* `LogicalCallContext`(výchozí nastavení)
* `Disallowed`– Zakáže vícenásobný přístup

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
Vícenásobný přístup se dá nakonfigurovat v `ActorService`na nastavení během registrace. nastavení Hello platí instancí objektu actor tooall vytvořených pomocí služby objektu actor hello.

Hello následující příklad ukazuje služby objektu actor, která nastaví režim vícenásobný přístup hello příliš`ActorReentrancyMode.Disallowed`. V tomto případě, pokud objekt actor odešle vícenásobné zpráva tooanother objektu actor, k výjimce typu `FabricException` bude vyvolána.

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


## <a name="next-steps"></a>Další kroky
* Další informace o opětovné zadání v hello [referenční dokumentace rozhraní API objektu Actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)
