---
title: aaaPolymorphism v hello Reliable Actors framework | Microsoft Docs
description: "Vytvoření hierarchie nástroje rozhraní .NET a typy v hello Reliable Actors framework tooreuse funkce a definice rozhraní API."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ed9ec442595bd9a5e48c9af1f6aac003439b81f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="polymorphism-in-hello-reliable-actors-framework"></a>Polymorfismus v hello Reliable Actors framework
Hello Reliable Actors, které vám toobuild, kteří používají řadu umožní framework hello stejné techniky, které byste použili v objektově orientované návrhu. Jeden z těchto postupů je polymorfismus, která umožňuje typy a rozhraní tooinherit z více zobecněn nadřazené položky. Dědičnost v hello Reliable Actors framework obecně následuje hello .NET model s několika další omezení. V případě Java či Linux postupuje hello Java modelu.

## <a name="interfaces"></a>Rozhraní
Hello Reliable Actors framework vyžaduje, abyste toodefine toobe alespoň jedno rozhraní implementované vašeho typu objektu actor. Toto rozhraní je použité toogenerate proxy třídu, která mohou být využívána toocommunicate klienti se vaše účastníky. Rozhraní může dědit vlastnosti z dalších rozhraní, dokud všechny rozhraní, které je implementované typ objektu actor a všech jejích nadřazených tříd nakonec odvozena od IActor(C#) nebo Actor(Java). IActor(C#) a Actor(Java) jsou hello definované platformy základní rozhraní pro aktéři v hello rozhraní .NET a Javu. Příklad classic polymorfismus hello použití tvarů proto může vypadat přibližně takto:

![Rozhraní hierarchie aktéři obrazce][shapes-interface-hierarchy]

## <a name="types"></a>Typy
Můžete také vytvořit hierarchii objektu actor typů, které jsou odvozeny od hello základní objektu Actor třídu, která poskytuje platformu hello. V případě hello tvarů, můžete mít na základní `Shape`(C#) nebo `ShapeImpl`typu (Java):

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

Subtypes z `Shape`(C#) nebo `ShapeImpl`(Java) můžete přepsat metody ze základní hello.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Poznámka: hello `ActorService` atribut typu objektu actor hello. Tento atribut informuje hello spolehlivé objektu Actor framework, že by měl automaticky vytvořit službu pro hostování aktéři tohoto typu. V některých případech může přát toocreate základní typ, který je určený výhradně pro sdílení funkce s podtypů a nebude nikdy použité tooinstantiate konkrétní aktéři. V takových případech byste měli použít hello `abstract` tooindicate – klíčové slovo, které nikdy vytvoříte objekt actor na základě tohoto typu.

## <a name="next-steps"></a>Další kroky
* V tématu [jak hello Reliable Actors framework využívá platformy Service Fabric hello](service-fabric-reliable-actors-platform.md) tooprovide spolehlivost, škálovatelnost a konzistentním stavu.
* Další informace o hello [životního cyklu objektu actor](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
