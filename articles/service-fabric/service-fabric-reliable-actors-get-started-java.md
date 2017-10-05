---
title: "Vytvoření vaší první na základě objektu actor Azure mikroslužbu v Javě | Microsoft Docs"
description: "Tento kurz vás provede kroky při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 288f1ed1016f50031065e66444d2562427194dc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="abeac-103">Začínáme s Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="abeac-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="abeac-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="abeac-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="abeac-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="abeac-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="abeac-106">Tento článek vysvětluje základy Azure Service Fabric Reliable Actors a provede vás vytvořením a nasazením jednoduchou aplikaci spolehlivé objektu Actor v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="abeac-106">This article explains the basics of Azure Service Fabric Reliable Actors and walks you through creating and deploying a simple Reliable Actor application in Java.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="abeac-107">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="abeac-107">Installation and setup</span></span>
<span data-ttu-id="abeac-108">Než začnete, ujistěte se, že máte vývojového prostředí Service Fabric na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="abeac-108">Before you start, make sure you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="abeac-109">Pokud potřebujete nastavit tak, přejděte na [Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="abeac-109">If you need to set it up, go to [getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="abeac-110">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="abeac-110">Basic concepts</span></span>
<span data-ttu-id="abeac-111">Začít s Reliable Actors, potřebujete jenom pochopit několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="abeac-111">To get started with Reliable Actors, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="abeac-112">**Služby objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="abeac-112">**Actor service**.</span></span> <span data-ttu-id="abeac-113">Spolehlivé služby, které můžou být nasazené v Service Fabric infrastruktury jsou součástí Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="abeac-113">Reliable Actors are packaged in Reliable Services that can be deployed in the Service Fabric infrastructure.</span></span> <span data-ttu-id="abeac-114">Instance objektu actor aktivují v instanci služby s názvem.</span><span class="sxs-lookup"><span data-stu-id="abeac-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="abeac-115">**Registrace objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="abeac-115">**Actor registration**.</span></span> <span data-ttu-id="abeac-116">Jako se službami Reliable Services spolehlivé objektu Actor služby musí být registrováno v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="abeac-116">As with Reliable Services, a Reliable Actor service needs to be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="abeac-117">Typ objektu actor kromě toho musí být registrováno s modulem runtime objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-117">In addition, the actor type needs to be registered with the Actor runtime.</span></span>
* <span data-ttu-id="abeac-118">**Rozhraní objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="abeac-118">**Actor interface**.</span></span> <span data-ttu-id="abeac-119">Rozhraní objektu actor se používá k definování silného typu veřejné rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-119">The actor interface is used to define a strongly typed public interface of an actor.</span></span> <span data-ttu-id="abeac-120">Rozhraní objektu actor v terminologii modelu objektu Actor spolehlivé, definuje typy zprávy, které můžete porozumět objektu actor a proces.</span><span class="sxs-lookup"><span data-stu-id="abeac-120">In the Reliable Actor model terminology, the actor interface defines the types of messages that the actor can understand and process.</span></span> <span data-ttu-id="abeac-121">Rozhraní objektu actor slouží ostatní aktéři a klientské aplikace "Odeslat" (asynchronně) zpráv do objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-121">The actor interface is used by other actors and client applications to "send" (asynchronously) messages to the actor.</span></span> <span data-ttu-id="abeac-122">Reliable Actors můžete implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="abeac-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="abeac-123">**Třída ActorProxy**.</span><span class="sxs-lookup"><span data-stu-id="abeac-123">**ActorProxy class**.</span></span> <span data-ttu-id="abeac-124">Třída ActorProxy se používá klientskými aplikacemi volat metody vystavenou přes rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-124">The ActorProxy class is used by client applications to invoke the methods exposed through the actor interface.</span></span> <span data-ttu-id="abeac-125">Třída ActorProxy poskytuje dvě důležité funkce:</span><span class="sxs-lookup"><span data-stu-id="abeac-125">The ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="abeac-126">Překlad názvů: je možné najít objektu actor v clusteru (Najít uzlu clusteru, který je hostitelem).</span><span class="sxs-lookup"><span data-stu-id="abeac-126">Name resolution: It is able to locate the actor in the cluster (find the node of the cluster where it is hosted).</span></span>
  * <span data-ttu-id="abeac-127">Zpracování selhání: mohou zkuste volání metod a znovu přeložit umístění objektu actor po, například selhání vyžadující objektu actor pro přemístit do jiného uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="abeac-127">Failure handling: It can retry method invocations and re-resolve the actor location after, for example, a failure that requires the actor to be relocated to another node in the cluster.</span></span>

<span data-ttu-id="abeac-128">Následující pravidla, které se týkají objektu actor rozhraní jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="abeac-128">The following rules that pertain to actor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="abeac-129">Metody rozhraní objektu actor nemohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="abeac-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="abeac-130">Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="abeac-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="abeac-131">Obecná rozhraní nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="abeac-131">Generic interfaces are not supported.</span></span>

## <a name="create-an-actor-service"></a><span data-ttu-id="abeac-132">Vytvoření služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="abeac-132">Create an actor service</span></span>
<span data-ttu-id="abeac-133">Začněte vytvořením nové aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="abeac-133">Start by creating a new Service Fabric application.</span></span> <span data-ttu-id="abeac-134">Sada Service Fabric SDK pro Linux zahrnuje Yeoman generátor zajistit generování uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="abeac-134">The Service Fabric SDK for Linux includes a Yeoman generator to provide the scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="abeac-135">Spusťte následující Yeoman spuštěním příkazu:</span><span class="sxs-lookup"><span data-stu-id="abeac-135">Start by running the following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="abeac-136">Postupujte podle pokynů vytvořte **spolehlivé služby objektu Actor**.</span><span class="sxs-lookup"><span data-stu-id="abeac-136">Follow the instructions to create a **Reliable Actor Service**.</span></span> <span data-ttu-id="abeac-137">V tomto kurzu, název aplikace "HelloWorldActorApplication" a "HelloWorldActor." objektu actor</span><span class="sxs-lookup"><span data-stu-id="abeac-137">For this tutorial, name the application "HelloWorldActorApplication" and the actor "HelloWorldActor."</span></span> <span data-ttu-id="abeac-138">Vytvoří se následující generování uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="abeac-138">The following scaffolding will be created:</span></span>

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="abeac-139">Spolehlivé aktéři základních stavebních bloků</span><span class="sxs-lookup"><span data-stu-id="abeac-139">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="abeac-140">Základní koncepty dříve popisované převede na základních stavebních bloků služby objektu Actor spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="abeac-140">The basic concepts described earlier translate into the basic building blocks of a Reliable Actor service.</span></span>

### <a name="actor-interface"></a><span data-ttu-id="abeac-141">Rozhraní objektu actor</span><span class="sxs-lookup"><span data-stu-id="abeac-141">Actor interface</span></span>
<span data-ttu-id="abeac-142">Tato položka obsahuje definici rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-142">This contains the interface definition for the actor.</span></span> <span data-ttu-id="abeac-143">Toto rozhraní definuje kontrakt objektu actor, který sdílí implementace objektu actor a klienti volání objektu actor, proto obvykle má smysl definovat na místě, která je oddělená od objektu actor implementaci a může být sdílen více jiných služeb nebo klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="abeac-143">This interface defines the actor contract that is shared by the actor implementation and the clients calling the actor, so it typically makes sense to define it in a place that is separate from the actor implementation and can be shared by multiple other services or client applications.</span></span>

<span data-ttu-id="abeac-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span><span class="sxs-lookup"><span data-stu-id="abeac-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span></span>

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a><span data-ttu-id="abeac-145">Služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="abeac-145">Actor service</span></span>
<span data-ttu-id="abeac-146">Tato položka obsahuje implementace objektu actor a objektu actor registrační kód.</span><span class="sxs-lookup"><span data-stu-id="abeac-146">This contains your actor implementation and actor registration code.</span></span> <span data-ttu-id="abeac-147">Třída objektu actor implementuje rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-147">The actor class implements the actor interface.</span></span> <span data-ttu-id="abeac-148">Toto je, kde vaše objektu actor nemá svou práci.</span><span class="sxs-lookup"><span data-stu-id="abeac-148">This is where your actor does its work.</span></span>

<span data-ttu-id="abeac-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span><span class="sxs-lookup"><span data-stu-id="abeac-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span></span>

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a><span data-ttu-id="abeac-150">Registrace objektu actor</span><span class="sxs-lookup"><span data-stu-id="abeac-150">Actor registration</span></span>
<span data-ttu-id="abeac-151">Služby objektu actor musí být zaregistrován s typem služby v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="abeac-151">The actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="abeac-152">V pořadí pro službu objektu Actor ke spuštění vaše instance objektu actor musí být typu vašeho objektu actor také zaregistrován u služby objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-152">In order for the Actor Service to run your actor instances, your actor type must also be registered with the Actor Service.</span></span> <span data-ttu-id="abeac-153">`ActorRuntime` Metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="abeac-153">The `ActorRuntime` registration method performs this work for actors.</span></span>

<span data-ttu-id="abeac-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span><span class="sxs-lookup"><span data-stu-id="abeac-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span></span>

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a><span data-ttu-id="abeac-155">Testovacího klienta</span><span class="sxs-lookup"><span data-stu-id="abeac-155">Test client</span></span>
<span data-ttu-id="abeac-156">Toto je jednoduchá testovací klientskou aplikaci můžete spustit samostatně z aplikace Service Fabric testování služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-156">This is a simple test client application you can run separately from the Service Fabric application to test your actor service.</span></span> <span data-ttu-id="abeac-157">Jedná se o příklad použití ActorProxy pro aktivaci a komunikaci s instancí objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-157">This is an example of where the ActorProxy can be used to activate and communicate with actor instances.</span></span> <span data-ttu-id="abeac-158">Získat není nasazené pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="abeac-158">It does not get deployed with your service.</span></span>

### <a name="the-application"></a><span data-ttu-id="abeac-159">Aplikace</span><span class="sxs-lookup"><span data-stu-id="abeac-159">The application</span></span>
<span data-ttu-id="abeac-160">Nakonec balíčky aplikace služby objektu actor a dalším službám, které byste mohli přidat v budoucnu společně pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="abeac-160">Finally, the application packages the actor service and any other services you might add in the future together for deployment.</span></span> <span data-ttu-id="abeac-161">Obsahuje *ApplicationManifest.xml* a zástupného pro balíček služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="abeac-161">It contains the *ApplicationManifest.xml* and place holders for the actor service package.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="abeac-162">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="abeac-162">Run the application</span></span>

<span data-ttu-id="abeac-163">Yeoman generování uživatelského rozhraní obsahuje skript gradle sestavení aplikace a skripty pro nasazení a odeberte aplikaci bash.</span><span class="sxs-lookup"><span data-stu-id="abeac-163">The Yeoman scaffolding includes a gradle script to build the application and bash scripts to deploy and remove the application.</span></span> <span data-ttu-id="abeac-164">Pokud chcete nasadit aplikaci, nejprve sestavení aplikace s gradlem:</span><span class="sxs-lookup"><span data-stu-id="abeac-164">To deploy the application, first build the application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="abeac-165">To vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí nástrojů příkazového řádku služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="abeac-165">This will produce a Service Fabric application package that can be deployed using Service Fabric CLI tools.</span></span>

### <a name="deploy-service-fabric-cli"></a><span data-ttu-id="abeac-166">Nasazení Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="abeac-166">Deploy Service Fabric CLI</span></span>

<span data-ttu-id="abeac-167">Install.sh skript obsahuje potřebné příkazy Service Fabric rozhraní příkazového řádku (sfctl) k nasazení balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="abeac-167">The install.sh script contains the necessary Service Fabric CLI (sfctl) commands to deploy the application package.</span></span>
<span data-ttu-id="abeac-168">Spusťte skript install.sh k nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="abeac-168">Run the install.sh script to deploy the application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="abeac-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abeac-169">Next steps</span></span>

* [<span data-ttu-id="abeac-170">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="abeac-170">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
