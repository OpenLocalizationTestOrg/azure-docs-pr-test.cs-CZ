---
title: "Polymorfismus v rámci Reliable Actors | Microsoft Docs"
description: "Vytvoření hierarchie nástroje rozhraní .NET a typy v rozhraní framework Reliable Actors znovu použít funkce a definice rozhraní API."
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
ms.openlocfilehash: 36a59a41b2261369a2062c76ef90aebf7e24a221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a><span data-ttu-id="e18be-103">Polymorfismus v rámci Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="e18be-103">Polymorphism in the Reliable Actors framework</span></span>
<span data-ttu-id="e18be-104">Rozhraní framework Reliable Actors umožňuje vytvářet, kteří používají řadu se stejné techniky, které byste použili v objektově orientované návrhu.</span><span class="sxs-lookup"><span data-stu-id="e18be-104">The Reliable Actors framework allows you to build actors using many of the same techniques that you would use in object-oriented design.</span></span> <span data-ttu-id="e18be-105">Jeden z těchto postupů je polymorfismus, která umožňuje typy a rozhraní dědění z více zobecněn nadřazené položky.</span><span class="sxs-lookup"><span data-stu-id="e18be-105">One of those techniques is polymorphism, which allows types and interfaces to inherit from more generalized parents.</span></span> <span data-ttu-id="e18be-106">Dědičnost v rámci Reliable Actors obvykle následuje model rozhraní .NET s několika další omezení.</span><span class="sxs-lookup"><span data-stu-id="e18be-106">Inheritance in the Reliable Actors framework generally follows the .NET model with a few additional constraints.</span></span> <span data-ttu-id="e18be-107">V případě Java či Linux postupuje modelu Java.</span><span class="sxs-lookup"><span data-stu-id="e18be-107">In case of Java/Linux, it follows the Java model.</span></span>

## <a name="interfaces"></a><span data-ttu-id="e18be-108">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="e18be-108">Interfaces</span></span>
<span data-ttu-id="e18be-109">Rozhraní framework Reliable Actors, musíte definovat alespoň jedno rozhraní k implementaci podle vašeho typu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="e18be-109">The Reliable Actors framework requires you to define at least one interface to be implemented by your actor type.</span></span> <span data-ttu-id="e18be-110">Toto rozhraní se používá ke generování třídu proxy, které je možné ke komunikaci s vaší aktéři klienty.</span><span class="sxs-lookup"><span data-stu-id="e18be-110">This interface is used to generate a proxy class that can be used by clients to communicate with your actors.</span></span> <span data-ttu-id="e18be-111">Rozhraní může dědit vlastnosti z dalších rozhraní, dokud všechny rozhraní, které je implementované typ objektu actor a všech jejích nadřazených tříd nakonec odvozena od IActor(C#) nebo Actor(Java).</span><span class="sxs-lookup"><span data-stu-id="e18be-111">Interfaces can inherit from other interfaces as long as every interface that is implemented by an actor type and all of its parents ultimately derive from IActor(C#) or Actor(Java) .</span></span> <span data-ttu-id="e18be-112">IActor(C#) a Actor(Java) jsou definované platformy základní rozhraní pro aktéři v rozhraní .NET a Javu.</span><span class="sxs-lookup"><span data-stu-id="e18be-112">IActor(C#) and Actor(Java) are the platform-defined base interfaces for actors in the frameworks .NET and Java respectively.</span></span> <span data-ttu-id="e18be-113">Příklad classic polymorfismus pomocí tvarů proto může vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="e18be-113">Thus, the classic polymorphism example using shapes might look something like this:</span></span>

![Rozhraní hierarchie aktéři obrazce][shapes-interface-hierarchy]

## <a name="types"></a><span data-ttu-id="e18be-115">Typy</span><span class="sxs-lookup"><span data-stu-id="e18be-115">Types</span></span>
<span data-ttu-id="e18be-116">Můžete také vytvořit hierarchii objektu actor typů, které jsou odvozeny od základní třídy objektu Actor, kterou poskytuje platformu.</span><span class="sxs-lookup"><span data-stu-id="e18be-116">You can also create a hierarchy of actor types, which are derived from the base Actor class that is provided by the platform.</span></span> <span data-ttu-id="e18be-117">V případě tvarů, můžete mít na základní `Shape`(C#) nebo `ShapeImpl`typu (Java):</span><span class="sxs-lookup"><span data-stu-id="e18be-117">In the case of shapes, you might have a base `Shape`(C#) or `ShapeImpl`(Java) type:</span></span>

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

<span data-ttu-id="e18be-118">Subtypes z `Shape`(C#) nebo `ShapeImpl`(Java) můžete přepsat metody od základní.</span><span class="sxs-lookup"><span data-stu-id="e18be-118">Subtypes of `Shape`(C#) or `ShapeImpl`(Java) can override methods from the base.</span></span>

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

<span data-ttu-id="e18be-119">Poznámka: `ActorService` atributu pro typ objektu actor.</span><span class="sxs-lookup"><span data-stu-id="e18be-119">Note the `ActorService` attribute on the actor type.</span></span> <span data-ttu-id="e18be-120">Tento atribut informuje rozhraní spolehlivé objektu Actor, že by měl automaticky vytvořit službu pro hostování aktéři tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="e18be-120">This attribute tells the Reliable Actor framework that it should automatically create a service for hosting actors of this type.</span></span> <span data-ttu-id="e18be-121">V některých případech můžete chtít vytvořit základní typ, který je určený výhradně pro sdílení funkce s podtypů a nebude nikdy používat k vytváření instancí konkrétní aktéři.</span><span class="sxs-lookup"><span data-stu-id="e18be-121">In some cases, you may wish to create a base type that is solely intended for sharing functionality with subtypes and will never be used to instantiate concrete actors.</span></span> <span data-ttu-id="e18be-122">V takových případech byste měli použít `abstract` – klíčové slovo indikující, že se nikdy vytvoří objekt actor na základě tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="e18be-122">In those cases, you should use the `abstract` keyword to indicate that you will never create an actor based on that type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e18be-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e18be-123">Next steps</span></span>
* <span data-ttu-id="e18be-124">V tématu [jak rozhraní Reliable Actors využívá platformy Service Fabric](service-fabric-reliable-actors-platform.md) zajistit spolehlivost, škálovatelnost a konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="e18be-124">See [how the Reliable Actors framework leverages the Service Fabric platform](service-fabric-reliable-actors-platform.md) to provide reliability, scalability, and consistent state.</span></span>
* <span data-ttu-id="e18be-125">Další informace o [životního cyklu objektu actor](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="e18be-125">Learn about the [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).</span></span>

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
