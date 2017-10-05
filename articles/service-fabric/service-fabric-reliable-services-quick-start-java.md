---
title: "Vytvoření vaší první mikroslužbu spolehlivé Azure v jazyce Java | Microsoft Docs"
description: "Úvod do vytváření aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 1ebabe4844732412e04bab8c277f7ebbc4a5737c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="52341-103">Začínáme s Reliable Services</span><span class="sxs-lookup"><span data-stu-id="52341-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="52341-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="52341-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="52341-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="52341-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
>
>

<span data-ttu-id="52341-106">Tento článek vysvětluje základy služby Azure Service Fabric spolehlivé a provede vás vytvořením a nasazením jednoduchou spolehlivá služba aplikaci napsanou v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="52341-106">This article explains the basics of Azure Service Fabric Reliable Services and walks you through creating and deploying a simple Reliable Service application written in Java.</span></span> <span data-ttu-id="52341-107">Video tento Microsoft Virtual Academy také ukazuje postup vytvoření bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span><span class="sxs-lookup"><span data-stu-id="52341-107">This Microsoft Virtual Academy video also shows you how to create a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a><span data-ttu-id="52341-108">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="52341-108">Installation and setup</span></span>
<span data-ttu-id="52341-109">Než začnete, ujistěte se, že máte vývojového prostředí Service Fabric na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="52341-109">Before you start, make sure you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="52341-110">Pokud potřebujete nastavit tak, přejděte na [Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="52341-110">If you need to set it up, go to [getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="52341-111">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="52341-111">Basic concepts</span></span>
<span data-ttu-id="52341-112">Pokud chcete začít se službami Reliable Services, potřebujete jenom pochopit několik základní koncepty:</span><span class="sxs-lookup"><span data-stu-id="52341-112">To get started with Reliable Services, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="52341-113">**Typ služby**: Toto je implementace služby.</span><span class="sxs-lookup"><span data-stu-id="52341-113">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="52341-114">Je definována v třídě napíšete, který rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.</span><span class="sxs-lookup"><span data-stu-id="52341-114">It is defined by the class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="52341-115">**S názvem instance služby**: ke spuštění služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy.</span><span class="sxs-lookup"><span data-stu-id="52341-115">**Named service instance**: To run your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="52341-116">Instance služby, které jsou ve skutečnosti instancí objektu možnosti vaší služby třídu, která můžete zapsat.</span><span class="sxs-lookup"><span data-stu-id="52341-116">Service instances are in fact object instantiations of your service class that you write.</span></span>
* <span data-ttu-id="52341-117">**Hostitel služby**: pojmenované instance vytvoříte muset spustit uvnitř hostitele.</span><span class="sxs-lookup"><span data-stu-id="52341-117">**Service host**: The named service instances you create need to run inside a host.</span></span> <span data-ttu-id="52341-118">Hostitel služby je právě proces, kde můžete spustit instance služby.</span><span class="sxs-lookup"><span data-stu-id="52341-118">The service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="52341-119">**Registrace služby**: registrace soustřeďuje všechny informace dohromady.</span><span class="sxs-lookup"><span data-stu-id="52341-119">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="52341-120">Typ služby musí být zaregistrován u modulu runtime Service Fabric v hostitele služby umožňující Service Fabric k vytvoření instance ho spustit.</span><span class="sxs-lookup"><span data-stu-id="52341-120">The service type must be registered with the Service Fabric runtime in a service host to allow Service Fabric to create instances of it to run.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="52341-121">Vytvoření bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="52341-121">Create a stateless service</span></span>
<span data-ttu-id="52341-122">Začněte vytvořením aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="52341-122">Start by creating a Service Fabric application.</span></span> <span data-ttu-id="52341-123">Sada Service Fabric SDK pro Linux zahrnuje Yeoman generátor zajistit generování uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="52341-123">The Service Fabric SDK for Linux includes a Yeoman generator to provide the scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="52341-124">Spusťte následující Yeoman spuštěním příkazu:</span><span class="sxs-lookup"><span data-stu-id="52341-124">Start by running the following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="52341-125">Postupujte podle pokynů vytvořte **spolehlivé bezstavové služby**.</span><span class="sxs-lookup"><span data-stu-id="52341-125">Follow the instructions to create a **Reliable Stateless Service**.</span></span> <span data-ttu-id="52341-126">V tomto kurzu, název aplikace "HelloWorldApplication" a "HelloWorld" service.</span><span class="sxs-lookup"><span data-stu-id="52341-126">For this tutorial, name the application "HelloWorldApplication" and the service "HelloWorld".</span></span> <span data-ttu-id="52341-127">Výsledek obsahuje adresářů pro `HelloWorldApplication` a `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="52341-127">The result includes directories for the `HelloWorldApplication` and `HelloWorld`.</span></span>

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a><span data-ttu-id="52341-128">Tuto službu implementovat</span><span class="sxs-lookup"><span data-stu-id="52341-128">Implement the service</span></span>
<span data-ttu-id="52341-129">Otevřete **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span><span class="sxs-lookup"><span data-stu-id="52341-129">Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span></span> <span data-ttu-id="52341-130">Tato třída definuje typ služby a můžete spustit kód.</span><span class="sxs-lookup"><span data-stu-id="52341-130">This class defines the service type, and can run any code.</span></span> <span data-ttu-id="52341-131">Rozhraní API služby obsahuje dvě vstupní body pro váš kód:</span><span class="sxs-lookup"><span data-stu-id="52341-131">The service API provides two entry points for your code:</span></span>

* <span data-ttu-id="52341-132">Metodu zprostředkovává vstupního bodu, názvem `runAsync()`, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="52341-132">An open-ended entry point method, called `runAsync()`, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* <span data-ttu-id="52341-133">Ke komunikaci vstupnímu bodu kde zařaďte vaší komunikačního balíku výběru.</span><span class="sxs-lookup"><span data-stu-id="52341-133">A communication entry point where you can plug in your communication stack of choice.</span></span> <span data-ttu-id="52341-134">Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="52341-134">This is where you can start receiving requests from users and other services.</span></span>

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

<span data-ttu-id="52341-135">V tomto kurzu budeme soustředit se na `runAsync()` metody vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="52341-135">In this tutorial, we focus on the `runAsync()` entry point method.</span></span> <span data-ttu-id="52341-136">Toto je, kde můžete okamžitě začít kód spuštěný.</span><span class="sxs-lookup"><span data-stu-id="52341-136">This is where you can immediately start running your code.</span></span>

### <a name="runasync"></a><span data-ttu-id="52341-137">RunAsync</span><span class="sxs-lookup"><span data-stu-id="52341-137">RunAsync</span></span>
<span data-ttu-id="52341-138">Platforma volá tuto metodu, když je umístěný a připravené ke spuštění instance služby.</span><span class="sxs-lookup"><span data-stu-id="52341-138">The platform calls this method when an instance of a service is placed and ready to execute.</span></span> <span data-ttu-id="52341-139">Bezstavové služby, která jednoduše znamená, když je otevřen v instanci služby.</span><span class="sxs-lookup"><span data-stu-id="52341-139">For a stateless service, that simply means when the service instance is opened.</span></span> <span data-ttu-id="52341-140">Token zrušení je k dispozici pro koordinaci při instanci služby musí být uzavřen.</span><span class="sxs-lookup"><span data-stu-id="52341-140">A cancellation token is provided to coordinate when your service instance needs to be closed.</span></span> <span data-ttu-id="52341-141">V Service Fabric tento cyklus otevření nebo uzavření instance služby může docházet k tolikrát, kolikrát za dobu existence služby jako celek.</span><span class="sxs-lookup"><span data-stu-id="52341-141">In Service Fabric, this open/close cycle of a service instance can occur many times over the lifetime of the service as a whole.</span></span> <span data-ttu-id="52341-142">Toto může nastat z různých důvodů, včetně:</span><span class="sxs-lookup"><span data-stu-id="52341-142">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="52341-143">Systém přesune vaší instance služby pro vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="52341-143">The system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="52341-144">K chybám dochází v kódu.</span><span class="sxs-lookup"><span data-stu-id="52341-144">Faults occur in your code.</span></span>
* <span data-ttu-id="52341-145">Aplikace nebo systému byla upgradována.</span><span class="sxs-lookup"><span data-stu-id="52341-145">The application or system is upgraded.</span></span>
* <span data-ttu-id="52341-146">Základní hardware dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="52341-146">The underlying hardware experiences an outage.</span></span>

<span data-ttu-id="52341-147">Tato orchestration je spravován pomocí Service Fabric a udržovat služby je vysoce dostupný a správně vyrovnáváním.</span><span class="sxs-lookup"><span data-stu-id="52341-147">This orchestration is managed by Service Fabric to keep your service highly available and properly balanced.</span></span>

<span data-ttu-id="52341-148">`runAsync()`by neměly blokovat synchronně.</span><span class="sxs-lookup"><span data-stu-id="52341-148">`runAsync()` should not block synchronously.</span></span> <span data-ttu-id="52341-149">Implementaci runAsync by měl vrátit CompletableFuture umožňující pokračovat modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="52341-149">Your implementation of runAsync should return a CompletableFuture to allow the runtime to continue.</span></span> <span data-ttu-id="52341-150">Pokud vaše úlohy musí implementovat dlouhotrvající úlohu, která se má provést uvnitř CompletableFuture.</span><span class="sxs-lookup"><span data-stu-id="52341-150">If your workload needs to implement a long running task that should be done inside the CompletableFuture.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="52341-151">Zrušení</span><span class="sxs-lookup"><span data-stu-id="52341-151">Cancellation</span></span>
<span data-ttu-id="52341-152">Zrušení úlohy je spolupráci úsilí řízená token poskytnutý zrušení.</span><span class="sxs-lookup"><span data-stu-id="52341-152">Cancellation of your workload is a cooperative effort orchestrated by the provided cancellation token.</span></span> <span data-ttu-id="52341-153">Systém čeká na ukončení (podle úspěšné dokončení, zrušení nebo selhání), než ji přesune vaše úlohy.</span><span class="sxs-lookup"><span data-stu-id="52341-153">The system waits for your task to end (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="52341-154">Je důležité respektovat token zrušení, Dokončit veškerou práci a ukončete `runAsync()` provést co nejrychleji, pokud systém požadavky zrušení.</span><span class="sxs-lookup"><span data-stu-id="52341-154">It is important to honor the cancellation token, finish any work, and exit `runAsync()` as quickly as possible when the system requests cancellation.</span></span> <span data-ttu-id="52341-155">Následující příklad ukazuje, jak se zpracovat událost zrušení:</span><span class="sxs-lookup"><span data-stu-id="52341-155">The following example demonstrates how to handle a cancellation event:</span></span>

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a><span data-ttu-id="52341-156">Registrace služby</span><span class="sxs-lookup"><span data-stu-id="52341-156">Service registration</span></span>
<span data-ttu-id="52341-157">Typů služeb musí být zaregistrované v modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="52341-157">Service types must be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="52341-158">Typ služby je definována v `ServiceManifest.xml` a třídě služby, který implementuje `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="52341-158">The service type is defined in the `ServiceManifest.xml` and your service class that implements `StatelessService`.</span></span> <span data-ttu-id="52341-159">Registrace služby se provádí v hlavní vstupního bodu procesu.</span><span class="sxs-lookup"><span data-stu-id="52341-159">Service registration is performed in the process main entry point.</span></span> <span data-ttu-id="52341-160">V tomto příkladu je hlavní vstupního bodu procesu `HelloWorldServiceHost.java`:</span><span class="sxs-lookup"><span data-stu-id="52341-160">In this example, the process main entry point is `HelloWorldServiceHost.java`:</span></span>

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-the-application"></a><span data-ttu-id="52341-161">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="52341-161">Run the application</span></span>

<span data-ttu-id="52341-162">Yeoman generování uživatelského rozhraní obsahuje skript gradle sestavení aplikace a skripty pro nasazení a odeberte aplikaci bash.</span><span class="sxs-lookup"><span data-stu-id="52341-162">The Yeoman scaffolding includes a gradle script to build the application and bash scripts to deploy and remove the application.</span></span> <span data-ttu-id="52341-163">Ke spuštění aplikace, nejprve sestavte aplikaci s gradlem:</span><span class="sxs-lookup"><span data-stu-id="52341-163">To run the application, first build the application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="52341-164">Tímto se vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="52341-164">This produces a Service Fabric application package that can be deployed using Service Fabric CLI.</span></span>

### <a name="deploy-with-service-fabric-cli"></a><span data-ttu-id="52341-165">Nasazení pomocí Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="52341-165">Deploy with Service Fabric CLI</span></span>

<span data-ttu-id="52341-166">Install.sh skript obsahuje potřebné příkazy Service Fabric rozhraní příkazového řádku pro nasazení balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="52341-166">The install.sh script contains the necessary Service Fabric CLI commands to deploy the application package.</span></span> <span data-ttu-id="52341-167">Spusťte skript install.sh k nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="52341-167">Run the install.sh script to deploy the application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="52341-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52341-168">Next steps</span></span>

* [<span data-ttu-id="52341-169">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="52341-169">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
