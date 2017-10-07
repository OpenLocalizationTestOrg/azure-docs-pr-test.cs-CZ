---
title: "použití aaaAdvanced spolehlivé služby | Microsoft Docs"
description: "Další informace o pokročilé využití spolehlivé služby Service Fabric přidané flexibilitu v službě."
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: e6d6310a4deae9edcfcd76551e1337f0e39e9e5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-usage-of-hello-reliable-services-programming-model"></a>Rozšířené použití hello spolehlivé služby programovací model
Azure Service Fabric zjednodušuje zápis a správu spolehlivé bezstavové a stavové služby. Tato příručka pojednává o Pokročilé použití spolehlivé služby toogain další kontrolu a flexibilitu přes vaše služby. Předchozí tooreading to průvodce, seznamte se s [hello spolehlivé služby programovací model](service-fabric-reliable-services-introduction.md).

Stavová a Bezstavová služby mají dvě primární vstupní body pro uživatelský kód:

* `RunAsync(C#) / runAsync(Java)`je pro obecné účely vstupní bod pro kód služby.
* `CreateServiceReplicaListeners(C#)`a `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` je pro otevření naslouchací procesy komunikace pro požadavky klientů.

Pro většinu služby jsou tyto dva vstupní body dostatečná. Ve výjimečných případech Pokud větší kontrolu nad životního cyklu služby je nutné, další životního cyklu události jsou k dispozici.

## <a name="stateless-service-instance-lifecycle"></a>Instance bezstavové služby životního cyklu
Životní cyklus bezstavové služby je velmi jednoduchý. Bezstavové služby můžete pouze otevřít, ukončeno nebo byl zrušen. `RunAsync`v bezstavové služby se spustí, až instance služby je otevřít a zrušit, pokud instance služby je uzavřen nebo přerušena.

I když `RunAsync` by mělo být dostatečné v téměř všech případech hello otevřená, zavřete a přerušení události v bezstavové služby jsou k dispozici také:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java`OnOpenAsync je volána, pokud je instance bezstavové služby hello o toobe použít. V tuto chvíli můžete spustit úlohy inicializace služby rozšířených.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java`OnCloseAsync je volána, když bude instance bezstavové služby hello toobe korektně vypnout dolů. Tato situace může nastat, pokud probíhá upgrade kódu hello služby, instance služby hello přesouvá z důvodu tooload vyrovnávání nebo se detekuje přechodná chyba. OnCloseAsync můžete použít toosafely zavřete všechny prostředky, zastavit zpracování na pozadí, dokončení ukládání externí stavu nebo ukončením dolů existující připojení.
* `void OnAbort() - C# / void onAbort() - Java`OnAbort je volána, když instance bezstavové služby hello je vynuceně vypnut. Obecně se používá v uzlu hello se zjistí trvalou chybou, nebo když Service Fabric se nedají spravovat spolehlivě životního cyklu instance služby hello z důvodu selhání toointernal.

## <a name="stateful-service-replica-lifecycle"></a>Životní cyklus repliky stavové služby

> [!NOTE]
> Stavová spolehlivé služby ještě nepodporuje v jazyce Java.
>
>

Životní cyklus repliku stavové služby je mnohem složitější než instance bezstavové služby. Kromě toho tooopen, zavřete a zrušení události, repliku stavové služby obsahuje změny role během celé jeho životnosti. Při změně role repliky stavové služby hello `OnChangeRoleAsync` je aktivována událost:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`OnChangeRoleAsync je volána, když se mění role, například tooprimary nebo sekundární repliky stavové služby hello. Primární repliky mají stav zápisu (toocreate jsou povoleny a zápis tooReliable kolekce). Sekundární repliky jsou uvedeny čtení stav (můžete číst jenom z existující spolehlivé kolekce). Většinu práce v stavové služby se provádí na primární replice hello. Sekundární repliky lze provést ověření jen pro čtení, generování sestav, dolování dat nebo jiné úlohy jen pro čtení.

Pouze primární replika hello v stavové služby, má přístup pro zápis toostate a proto je obecně při hello služby provádí samotnou práci. Hello `RunAsync` metoda v stavové služby se spustí jenom v případě, že je primární replika stavové služby hello. Hello `RunAsync` metoda se zruší, když primární repliky role změny z primární i během hello zavřete a zrušení události.

Pomocí hello `OnChangeRoleAsync` událostí vám umožní pracovní tooperform v závislosti na role repliky také jako odpověď toorole změnu.

Stavové služby rovněž poskytne události životního cyklu hello stejné čtyři jako bezstavové služby hello stejnou sémantiku a případy použití:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Další kroky
Pro pokročilejší témata související tooService prostředků infrastruktury najdete v části hello následující články:

* [Konfigurace stavové spolehlivé služby](service-fabric-reliable-services-configuration.md)
* [Úvod stavu Service Fabric](service-fabric-health-introduction.md)
* [Pomocí sestav o stavu systému pro řešení potíží](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Konfigurace služby pomocí hello správce prostředků clusteru Service Fabric](service-fabric-cluster-resource-manager-configure-services.md)
