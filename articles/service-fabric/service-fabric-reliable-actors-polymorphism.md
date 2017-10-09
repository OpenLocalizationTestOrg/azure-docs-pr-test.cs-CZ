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
# <a name="polymorphism-in-hello-reliable-actors-framework"></a><span data-ttu-id="e41f8-103">Polymorfismus v hello Reliable Actors framework</span><span class="sxs-lookup"><span data-stu-id="e41f8-103">Polymorphism in hello Reliable Actors framework</span></span>
<span data-ttu-id="e41f8-104">Hello Reliable Actors, které vám toobuild, kteří používají řadu umožní framework hello stejné techniky, které byste použili v objektově orientované návrhu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-104">hello Reliable Actors framework allows you toobuild actors using many of hello same techniques that you would use in object-oriented design.</span></span> <span data-ttu-id="e41f8-105">Jeden z těchto postupů je polymorfismus, která umožňuje typy a rozhraní tooinherit z více zobecněn nadřazené položky.</span><span class="sxs-lookup"><span data-stu-id="e41f8-105">One of those techniques is polymorphism, which allows types and interfaces tooinherit from more generalized parents.</span></span> <span data-ttu-id="e41f8-106">Dědičnost v hello Reliable Actors framework obecně následuje hello .NET model s několika další omezení.</span><span class="sxs-lookup"><span data-stu-id="e41f8-106">Inheritance in hello Reliable Actors framework generally follows hello .NET model with a few additional constraints.</span></span> <span data-ttu-id="e41f8-107">V případě Java či Linux postupuje hello Java modelu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-107">In case of Java/Linux, it follows hello Java model.</span></span>

## <a name="interfaces"></a><span data-ttu-id="e41f8-108">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="e41f8-108">Interfaces</span></span>
<span data-ttu-id="e41f8-109">Hello Reliable Actors framework vyžaduje, abyste toodefine toobe alespoň jedno rozhraní implementované vašeho typu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="e41f8-109">hello Reliable Actors framework requires you toodefine at least one interface toobe implemented by your actor type.</span></span> <span data-ttu-id="e41f8-110">Toto rozhraní je použité toogenerate proxy třídu, která mohou být využívána toocommunicate klienti se vaše účastníky.</span><span class="sxs-lookup"><span data-stu-id="e41f8-110">This interface is used toogenerate a proxy class that can be used by clients toocommunicate with your actors.</span></span> <span data-ttu-id="e41f8-111">Rozhraní může dědit vlastnosti z dalších rozhraní, dokud všechny rozhraní, které je implementované typ objektu actor a všech jejích nadřazených tříd nakonec odvozena od IActor(C#) nebo Actor(Java).</span><span class="sxs-lookup"><span data-stu-id="e41f8-111">Interfaces can inherit from other interfaces as long as every interface that is implemented by an actor type and all of its parents ultimately derive from IActor(C#) or Actor(Java) .</span></span> <span data-ttu-id="e41f8-112">IActor(C#) a Actor(Java) jsou hello definované platformy základní rozhraní pro aktéři v hello rozhraní .NET a Javu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-112">IActor(C#) and Actor(Java) are hello platform-defined base interfaces for actors in hello frameworks .NET and Java respectively.</span></span> <span data-ttu-id="e41f8-113">Příklad classic polymorfismus hello použití tvarů proto může vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e41f8-113">Thus, hello classic polymorphism example using shapes might look something like this:</span></span>

![Rozhraní hierarchie aktéři obrazce][shapes-interface-hierarchy]

## <a name="types"></a><span data-ttu-id="e41f8-115">Typy</span><span class="sxs-lookup"><span data-stu-id="e41f8-115">Types</span></span>
<span data-ttu-id="e41f8-116">Můžete také vytvořit hierarchii objektu actor typů, které jsou odvozeny od hello základní objektu Actor třídu, která poskytuje platformu hello.</span><span class="sxs-lookup"><span data-stu-id="e41f8-116">You can also create a hierarchy of actor types, which are derived from hello base Actor class that is provided by hello platform.</span></span> <span data-ttu-id="e41f8-117">V případě hello tvarů, můžete mít na základní `Shape`(C#) nebo `ShapeImpl`typu (Java):</span><span class="sxs-lookup"><span data-stu-id="e41f8-117">In hello case of shapes, you might have a base `Shape`(C#) or `ShapeImpl`(Java) type:</span></span>

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

<span data-ttu-id="e41f8-118">Subtypes z `Shape`(C#) nebo `ShapeImpl`(Java) můžete přepsat metody ze základní hello.</span><span class="sxs-lookup"><span data-stu-id="e41f8-118">Subtypes of `Shape`(C#) or `ShapeImpl`(Java) can override methods from hello base.</span></span>

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

<span data-ttu-id="e41f8-119">Poznámka: hello `ActorService` atribut typu objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="e41f8-119">Note hello `ActorService` attribute on hello actor type.</span></span> <span data-ttu-id="e41f8-120">Tento atribut informuje hello spolehlivé objektu Actor framework, že by měl automaticky vytvořit službu pro hostování aktéři tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-120">This attribute tells hello Reliable Actor framework that it should automatically create a service for hosting actors of this type.</span></span> <span data-ttu-id="e41f8-121">V některých případech může přát toocreate základní typ, který je určený výhradně pro sdílení funkce s podtypů a nebude nikdy použité tooinstantiate konkrétní aktéři.</span><span class="sxs-lookup"><span data-stu-id="e41f8-121">In some cases, you may wish toocreate a base type that is solely intended for sharing functionality with subtypes and will never be used tooinstantiate concrete actors.</span></span> <span data-ttu-id="e41f8-122">V takových případech byste měli použít hello `abstract` tooindicate – klíčové slovo, které nikdy vytvoříte objekt actor na základě tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-122">In those cases, you should use hello `abstract` keyword tooindicate that you will never create an actor based on that type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e41f8-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e41f8-123">Next steps</span></span>
* <span data-ttu-id="e41f8-124">V tématu [jak hello Reliable Actors framework využívá platformy Service Fabric hello](service-fabric-reliable-actors-platform.md) tooprovide spolehlivost, škálovatelnost a konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="e41f8-124">See [how hello Reliable Actors framework leverages hello Service Fabric platform](service-fabric-reliable-actors-platform.md) tooprovide reliability, scalability, and consistent state.</span></span>
* <span data-ttu-id="e41f8-125">Další informace o hello [životního cyklu objektu actor](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="e41f8-125">Learn about hello [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).</span></span>

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
