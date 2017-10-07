---
title: "aaaCreate vaše první na základě objektu actor Azure mikroslužbu v Javě | Microsoft Docs"
description: "Tento kurz vás provede kroky hello při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
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
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="c5391-103">Začínáme s Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="c5391-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c5391-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="c5391-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="c5391-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="c5391-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="c5391-106">Tento článek vysvětluje základy hello Azure Service Fabric Reliable Actors a provede vás vytvořením a nasazením jednoduchou aplikaci spolehlivé objektu Actor v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="c5391-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating and deploying a simple Reliable Actor application in Java.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="c5391-107">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="c5391-107">Installation and setup</span></span>
<span data-ttu-id="c5391-108">Než začnete, ujistěte se, že máte hello Service Fabric vývojového prostředí nastavit na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c5391-108">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="c5391-109">Pokud potřebujete tooset ho, přejděte příliš[Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c5391-109">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="c5391-110">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="c5391-110">Basic concepts</span></span>
<span data-ttu-id="c5391-111">tooget začít s Reliable Actors, můžete pouze potřebovat toounderstand několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="c5391-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="c5391-112">**Služby objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="c5391-112">**Actor service**.</span></span> <span data-ttu-id="c5391-113">Spolehlivé služby, které můžou být nasazené v infrastruktuře Service Fabric hello jsou součástí Reliable Actors.</span><span class="sxs-lookup"><span data-stu-id="c5391-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="c5391-114">Instance objektu actor aktivují v instanci služby s názvem.</span><span class="sxs-lookup"><span data-stu-id="c5391-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="c5391-115">**Registrace objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="c5391-115">**Actor registration**.</span></span> <span data-ttu-id="c5391-116">Jako služba spolehlivé objektu Actor potřebuje se službami Reliable Services toobe zaregistrována modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="c5391-117">Typ objektu actor hello kromě toho musí toobe zaregistrována hello objektu Actor runtime.</span><span class="sxs-lookup"><span data-stu-id="c5391-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="c5391-118">**Rozhraní objektu actor**.</span><span class="sxs-lookup"><span data-stu-id="c5391-118">**Actor interface**.</span></span> <span data-ttu-id="c5391-119">rozhraní objektu actor Hello je použité toodefine silného typu veřejné rozhraní objektu actor.</span><span class="sxs-lookup"><span data-stu-id="c5391-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="c5391-120">Rozhraní objektu actor hello v hello terminologie modelu objektu Actor spolehlivé, definuje hello typy zpráv, které hello objektu actor můžete pochopit a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="c5391-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="c5391-121">rozhraní objektu actor Hello používá jiné aktéři a klientské aplikace příliš "Odeslat" (asynchronně) objektu actor toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="c5391-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="c5391-122">Reliable Actors můžete implementovat více rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5391-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="c5391-123">**Třída ActorProxy**.</span><span class="sxs-lookup"><span data-stu-id="c5391-123">**ActorProxy class**.</span></span> <span data-ttu-id="c5391-124">používá Hello ActorProxy třídy klienta aplikace tooinvoke hello metody vystavenou přes rozhraní objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="c5391-125">Hello ActorProxy třída poskytuje dvě důležité funkce:</span><span class="sxs-lookup"><span data-stu-id="c5391-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="c5391-126">Překlad názvů: je možné toolocate hello objektu actor v clusteru hello (Najít hello uzel hello clusteru, který je hostitelem).</span><span class="sxs-lookup"><span data-stu-id="c5391-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="c5391-127">Zpracování selhání: můžete opakujte volání metod a znovu přeložit umístění objektu actor hello po, například selhání, který vyžaduje hello objektu actor toobe přemístění tooanother uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="c5391-128">Hello následující pravidla, které se týkají tooactor rozhraní jsou důležité zmínit:</span><span class="sxs-lookup"><span data-stu-id="c5391-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="c5391-129">Metody rozhraní objektu actor nemohou být přetíženy.</span><span class="sxs-lookup"><span data-stu-id="c5391-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="c5391-130">Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="c5391-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="c5391-131">Obecná rozhraní nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="c5391-131">Generic interfaces are not supported.</span></span>

## <a name="create-an-actor-service"></a><span data-ttu-id="c5391-132">Vytvoření služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="c5391-132">Create an actor service</span></span>
<span data-ttu-id="c5391-133">Začněte vytvořením nové aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c5391-133">Start by creating a new Service Fabric application.</span></span> <span data-ttu-id="c5391-134">zahrnuje Hello Service Fabric SDK pro Linux Yeoman generování generátor tooprovide hello uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="c5391-134">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="c5391-135">Spusťte spuštěním hello následující Yeoman příkaz:</span><span class="sxs-lookup"><span data-stu-id="c5391-135">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="c5391-136">Postupujte podle pokynů toocreate hello **spolehlivé služby objektu Actor**.</span><span class="sxs-lookup"><span data-stu-id="c5391-136">Follow hello instructions toocreate a **Reliable Actor Service**.</span></span> <span data-ttu-id="c5391-137">V tomto kurzu název hello aplikace "HelloWorldActorApplication" a hello objektu actor "HelloWorldActor."</span><span class="sxs-lookup"><span data-stu-id="c5391-137">For this tutorial, name hello application "HelloWorldActorApplication" and hello actor "HelloWorldActor."</span></span> <span data-ttu-id="c5391-138">Vytvoří Hello následující generování uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="c5391-138">hello following scaffolding will be created:</span></span>

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

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="c5391-139">Spolehlivé aktéři základních stavebních bloků</span><span class="sxs-lookup"><span data-stu-id="c5391-139">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="c5391-140">základní koncepty Hello dříve popisované převede na hello základní stavební bloky služby objektu Actor spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="c5391-140">hello basic concepts described earlier translate into hello basic building blocks of a Reliable Actor service.</span></span>

### <a name="actor-interface"></a><span data-ttu-id="c5391-141">Rozhraní objektu actor</span><span class="sxs-lookup"><span data-stu-id="c5391-141">Actor interface</span></span>
<span data-ttu-id="c5391-142">Tato položka obsahuje definici rozhraní hello objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-142">This contains hello interface definition for hello actor.</span></span> <span data-ttu-id="c5391-143">Toto rozhraní definuje kontrakt objektu actor hello, který sdílí hello objektu actor implementace a volání objektu actor hello, takže obvykle má smysl toodefine jej do umístění, které je oddělené od implementace objektu actor hello a může být sdílen více jiných klientů hello služby nebo aplikace klienta.</span><span class="sxs-lookup"><span data-stu-id="c5391-143">This interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in a place that is separate from hello actor implementation and can be shared by multiple other services or client applications.</span></span>

<span data-ttu-id="c5391-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span><span class="sxs-lookup"><span data-stu-id="c5391-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span></span>

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a><span data-ttu-id="c5391-145">Služby objektu actor</span><span class="sxs-lookup"><span data-stu-id="c5391-145">Actor service</span></span>
<span data-ttu-id="c5391-146">Tato položka obsahuje implementace objektu actor a objektu actor registrační kód.</span><span class="sxs-lookup"><span data-stu-id="c5391-146">This contains your actor implementation and actor registration code.</span></span> <span data-ttu-id="c5391-147">Třída objektu actor Hello implementuje rozhraní objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-147">hello actor class implements hello actor interface.</span></span> <span data-ttu-id="c5391-148">Toto je, kde vaše objektu actor nemá svou práci.</span><span class="sxs-lookup"><span data-stu-id="c5391-148">This is where your actor does its work.</span></span>

<span data-ttu-id="c5391-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span><span class="sxs-lookup"><span data-stu-id="c5391-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span></span>

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

### <a name="actor-registration"></a><span data-ttu-id="c5391-150">Registrace objektu actor</span><span class="sxs-lookup"><span data-stu-id="c5391-150">Actor registration</span></span>
<span data-ttu-id="c5391-151">služby objektu actor Hello musí být zaregistrován s typem služby v modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-151">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="c5391-152">V pořadí pro hello služby objektu Actor toorun vaše instance objektu actor, vašeho typu objektu actor musí být zaregistrovaná taky hello služby objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="c5391-152">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="c5391-153">Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři.</span><span class="sxs-lookup"><span data-stu-id="c5391-153">hello `ActorRuntime` registration method performs this work for actors.</span></span>

<span data-ttu-id="c5391-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span><span class="sxs-lookup"><span data-stu-id="c5391-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span></span>

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

### <a name="test-client"></a><span data-ttu-id="c5391-155">Testovacího klienta</span><span class="sxs-lookup"><span data-stu-id="c5391-155">Test client</span></span>
<span data-ttu-id="c5391-156">Toto je jednoduchá testovací klientskou aplikaci můžete spustit samostatně z tootest aplikace Service Fabric hello služby objektu actor.</span><span class="sxs-lookup"><span data-stu-id="c5391-156">This is a simple test client application you can run separately from hello Service Fabric application tootest your actor service.</span></span> <span data-ttu-id="c5391-157">Jedná se o příklad kde hello ActorProxy můžou být použité tooactivate a komunikaci s instancí objektu actor.</span><span class="sxs-lookup"><span data-stu-id="c5391-157">This is an example of where hello ActorProxy can be used tooactivate and communicate with actor instances.</span></span> <span data-ttu-id="c5391-158">Získat není nasazené pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="c5391-158">It does not get deployed with your service.</span></span>

### <a name="hello-application"></a><span data-ttu-id="c5391-159">aplikace Hello</span><span class="sxs-lookup"><span data-stu-id="c5391-159">hello application</span></span>
<span data-ttu-id="c5391-160">Nakonec balíčky aplikací hello hello služby objektu actor a dalším službám, které byste mohli přidat v hello budoucí společně pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="c5391-160">Finally, hello application packages hello actor service and any other services you might add in hello future together for deployment.</span></span> <span data-ttu-id="c5391-161">Obsahuje hello *ApplicationManifest.xml* a zástupného pro balíček služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="c5391-161">It contains hello *ApplicationManifest.xml* and place holders for hello actor service package.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="c5391-162">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c5391-162">Run hello application</span></span>

<span data-ttu-id="c5391-163">Hello Yeoman generování uživatelského rozhraní obsahuje gradle skriptu toobuild hello aplikace a bash skripty toodeploy a aplikaci odebrat.</span><span class="sxs-lookup"><span data-stu-id="c5391-163">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="c5391-164">toodeploy hello aplikace, první aplikace hello sestavení s gradlem:</span><span class="sxs-lookup"><span data-stu-id="c5391-164">toodeploy hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="c5391-165">To vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí nástrojů příkazového řádku služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="c5391-165">This will produce a Service Fabric application package that can be deployed using Service Fabric CLI tools.</span></span>

### <a name="deploy-service-fabric-cli"></a><span data-ttu-id="c5391-166">Nasazení Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="c5391-166">Deploy Service Fabric CLI</span></span>

<span data-ttu-id="c5391-167">Hello install.sh skript obsahuje hello nezbytné Service Fabric rozhraní příkazového řádku (sfctl) příkazy toodeploy hello balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5391-167">hello install.sh script contains hello necessary Service Fabric CLI (sfctl) commands toodeploy hello application package.</span></span>
<span data-ttu-id="c5391-168">Spuštění aplikace hello hello install.sh skriptu toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c5391-168">Run hello install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="c5391-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c5391-169">Next steps</span></span>

* [<span data-ttu-id="c5391-170">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="c5391-170">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
