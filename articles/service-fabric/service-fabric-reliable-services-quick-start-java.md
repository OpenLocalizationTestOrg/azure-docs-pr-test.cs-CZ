---
title: "aaaCreate vaše první spolehlivé Azure mikroslužbu v Javě | Microsoft Docs"
description: "Úvod toocreating aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
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
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="8c93f-103">Začínáme s Reliable Services</span><span class="sxs-lookup"><span data-stu-id="8c93f-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8c93f-104">C# v systému Windows</span><span class="sxs-lookup"><span data-stu-id="8c93f-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="8c93f-105">Java v Linuxu</span><span class="sxs-lookup"><span data-stu-id="8c93f-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
>
>

<span data-ttu-id="8c93f-106">Tento článek vysvětluje hello Základy služby Azure Service Fabric spolehlivé a provede vás vytvořením a nasazením jednoduchou spolehlivá služba aplikaci napsanou v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="8c93f-106">This article explains hello basics of Azure Service Fabric Reliable Services and walks you through creating and deploying a simple Reliable Service application written in Java.</span></span> <span data-ttu-id="8c93f-107">Toto video Microsoft Virtual Academy také ukazuje, jak toocreate bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span><span class="sxs-lookup"><span data-stu-id="8c93f-107">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a><span data-ttu-id="8c93f-108">Instalace a nastavení</span><span class="sxs-lookup"><span data-stu-id="8c93f-108">Installation and setup</span></span>
<span data-ttu-id="8c93f-109">Než začnete, ujistěte se, že máte hello Service Fabric vývojového prostředí nastavit na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8c93f-109">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="8c93f-110">Pokud potřebujete tooset ho, přejděte příliš[Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8c93f-110">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="8c93f-111">Základní koncepty</span><span class="sxs-lookup"><span data-stu-id="8c93f-111">Basic concepts</span></span>
<span data-ttu-id="8c93f-112">spuštění se službami Reliable Services, můžete pouze tooget potřebovat toounderstand pár základních konceptech:</span><span class="sxs-lookup"><span data-stu-id="8c93f-112">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="8c93f-113">**Typ služby**: Toto je implementace služby.</span><span class="sxs-lookup"><span data-stu-id="8c93f-113">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="8c93f-114">Je definována pomocí třídy hello napíšete, která rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.</span><span class="sxs-lookup"><span data-stu-id="8c93f-114">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="8c93f-115">**S názvem instance služby**: toorun vaší služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy.</span><span class="sxs-lookup"><span data-stu-id="8c93f-115">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="8c93f-116">Instance služby, které jsou ve skutečnosti instancí objektu možnosti vaší služby třídu, která můžete zapsat.</span><span class="sxs-lookup"><span data-stu-id="8c93f-116">Service instances are in fact object instantiations of your service class that you write.</span></span>
* <span data-ttu-id="8c93f-117">**Hostitel služby**: hello s názvem instance služby vytvoříte toorun nutné v hostiteli.</span><span class="sxs-lookup"><span data-stu-id="8c93f-117">**Service host**: hello named service instances you create need toorun inside a host.</span></span> <span data-ttu-id="8c93f-118">Hostitel služby Hello je právě proces, kde můžete spustit instance služby.</span><span class="sxs-lookup"><span data-stu-id="8c93f-118">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="8c93f-119">**Registrace služby**: registrace soustřeďuje všechny informace dohromady.</span><span class="sxs-lookup"><span data-stu-id="8c93f-119">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="8c93f-120">Hello typ služby musí být zaregistrován s hello Service Fabric runtime ve službě hostitele tooallow Service Fabric toocreate instance ho toorun.</span><span class="sxs-lookup"><span data-stu-id="8c93f-120">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="8c93f-121">Vytvoření bezstavové služby</span><span class="sxs-lookup"><span data-stu-id="8c93f-121">Create a stateless service</span></span>
<span data-ttu-id="8c93f-122">Začněte vytvořením aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="8c93f-122">Start by creating a Service Fabric application.</span></span> <span data-ttu-id="8c93f-123">zahrnuje Hello Service Fabric SDK pro Linux Yeoman generování generátor tooprovide hello uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="8c93f-123">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="8c93f-124">Spusťte spuštěním hello následující Yeoman příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c93f-124">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="8c93f-125">Postupujte podle pokynů toocreate hello **spolehlivé bezstavové služby**.</span><span class="sxs-lookup"><span data-stu-id="8c93f-125">Follow hello instructions toocreate a **Reliable Stateless Service**.</span></span> <span data-ttu-id="8c93f-126">V tomto kurzu aplikace hello název "HelloWorldApplication" a hello služby "HelloWorld".</span><span class="sxs-lookup"><span data-stu-id="8c93f-126">For this tutorial, name hello application "HelloWorldApplication" and hello service "HelloWorld".</span></span> <span data-ttu-id="8c93f-127">výsledek Hello zahrnuje adresářů pro hello `HelloWorldApplication` a `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="8c93f-127">hello result includes directories for hello `HelloWorldApplication` and `HelloWorld`.</span></span>

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

## <a name="implement-hello-service"></a><span data-ttu-id="8c93f-128">Implementace služby hello</span><span class="sxs-lookup"><span data-stu-id="8c93f-128">Implement hello service</span></span>
<span data-ttu-id="8c93f-129">Otevřete **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span><span class="sxs-lookup"><span data-stu-id="8c93f-129">Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span></span> <span data-ttu-id="8c93f-130">Tato třída definuje typ hello služby a všechny kód můžete spustit.</span><span class="sxs-lookup"><span data-stu-id="8c93f-130">This class defines hello service type, and can run any code.</span></span> <span data-ttu-id="8c93f-131">rozhraní API služby Hello poskytuje dva vstupní body kódu:</span><span class="sxs-lookup"><span data-stu-id="8c93f-131">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="8c93f-132">Metodu zprostředkovává vstupního bodu, názvem `runAsync()`, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.</span><span class="sxs-lookup"><span data-stu-id="8c93f-132">An open-ended entry point method, called `runAsync()`, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* <span data-ttu-id="8c93f-133">Ke komunikaci vstupnímu bodu kde zařaďte vaší komunikačního balíku výběru.</span><span class="sxs-lookup"><span data-stu-id="8c93f-133">A communication entry point where you can plug in your communication stack of choice.</span></span> <span data-ttu-id="8c93f-134">Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.</span><span class="sxs-lookup"><span data-stu-id="8c93f-134">This is where you can start receiving requests from users and other services.</span></span>

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

<span data-ttu-id="8c93f-135">V tomto kurzu budeme soustředit na hello `runAsync()` metody vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="8c93f-135">In this tutorial, we focus on hello `runAsync()` entry point method.</span></span> <span data-ttu-id="8c93f-136">Toto je, kde můžete okamžitě začít kód spuštěný.</span><span class="sxs-lookup"><span data-stu-id="8c93f-136">This is where you can immediately start running your code.</span></span>

### <a name="runasync"></a><span data-ttu-id="8c93f-137">RunAsync</span><span class="sxs-lookup"><span data-stu-id="8c93f-137">RunAsync</span></span>
<span data-ttu-id="8c93f-138">Platforma Hello volá tuto metodu, pokud instance služby je umístěný a připravené tooexecute.</span><span class="sxs-lookup"><span data-stu-id="8c93f-138">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="8c93f-139">Bezstavové služby, která jednoduše znamená po otevření hello instance služby.</span><span class="sxs-lookup"><span data-stu-id="8c93f-139">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="8c93f-140">Token zrušení získáte toocoordinate při instanci služby musí toobe uzavřen.</span><span class="sxs-lookup"><span data-stu-id="8c93f-140">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="8c93f-141">V Service Fabric může tento cyklus otevření nebo uzavření instance služby k mnohokrát průběhu životnosti hello hello služby jako celek.</span><span class="sxs-lookup"><span data-stu-id="8c93f-141">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="8c93f-142">Toto může nastat z různých důvodů, včetně:</span><span class="sxs-lookup"><span data-stu-id="8c93f-142">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="8c93f-143">Hello systém přesune vaší instance služby pro vyrovnávání prostředků.</span><span class="sxs-lookup"><span data-stu-id="8c93f-143">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="8c93f-144">K chybám dochází v kódu.</span><span class="sxs-lookup"><span data-stu-id="8c93f-144">Faults occur in your code.</span></span>
* <span data-ttu-id="8c93f-145">Hello aplikace nebo systému byla upgradována.</span><span class="sxs-lookup"><span data-stu-id="8c93f-145">hello application or system is upgraded.</span></span>
* <span data-ttu-id="8c93f-146">základní hardware Hello dojde k výpadku.</span><span class="sxs-lookup"><span data-stu-id="8c93f-146">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="8c93f-147">Tato orchestration spravuje Service Fabric tookeep služby vysoce dostupné a správně vyrovnáváním.</span><span class="sxs-lookup"><span data-stu-id="8c93f-147">This orchestration is managed by Service Fabric tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="8c93f-148">`runAsync()`by neměly blokovat synchronně.</span><span class="sxs-lookup"><span data-stu-id="8c93f-148">`runAsync()` should not block synchronously.</span></span> <span data-ttu-id="8c93f-149">Implementaci runAsync by měl vrátit toocontinue CompletableFuture tooallow hello modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="8c93f-149">Your implementation of runAsync should return a CompletableFuture tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="8c93f-150">Pokud vaše úlohy potřebuje tooimplement dlouhotrvající úlohu, která se má provést uvnitř hello CompletableFuture.</span><span class="sxs-lookup"><span data-stu-id="8c93f-150">If your workload needs tooimplement a long running task that should be done inside hello CompletableFuture.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="8c93f-151">Zrušení</span><span class="sxs-lookup"><span data-stu-id="8c93f-151">Cancellation</span></span>
<span data-ttu-id="8c93f-152">Zrušení úlohy je spolupráci úsilí řízená hello zadaný token zrušení.</span><span class="sxs-lookup"><span data-stu-id="8c93f-152">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="8c93f-153">systém Hello počká na vaše úloha tooend (podle úspěšné dokončení, zrušení nebo selhání) předtím, než ji přesune.</span><span class="sxs-lookup"><span data-stu-id="8c93f-153">hello system waits for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="8c93f-154">Je důležité toohonor hello zrušení tokenu, dokončit všechny fungovat a ukončete `runAsync()` provést co nejrychleji při hello systému požadavky zrušení.</span><span class="sxs-lookup"><span data-stu-id="8c93f-154">It is important toohonor hello cancellation token, finish any work, and exit `runAsync()` as quickly as possible when hello system requests cancellation.</span></span> <span data-ttu-id="8c93f-155">Hello následující příklad ukazuje, jak toohandle zrušení událostí:</span><span class="sxs-lookup"><span data-stu-id="8c93f-155">hello following example demonstrates how toohandle a cancellation event:</span></span>

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
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

### <a name="service-registration"></a><span data-ttu-id="8c93f-156">Registrace služby</span><span class="sxs-lookup"><span data-stu-id="8c93f-156">Service registration</span></span>
<span data-ttu-id="8c93f-157">Typů služeb musí být zaregistrované v modulu runtime Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="8c93f-157">Service types must be registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="8c93f-158">Typ služby Hello je definována v hello `ServiceManifest.xml` a třídě služby, který implementuje `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="8c93f-158">hello service type is defined in hello `ServiceManifest.xml` and your service class that implements `StatelessService`.</span></span> <span data-ttu-id="8c93f-159">Registrace služby se provádí v hello proces hlavní vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="8c93f-159">Service registration is performed in hello process main entry point.</span></span> <span data-ttu-id="8c93f-160">V tomto příkladu hello proces hlavní vstupní bod je `HelloWorldServiceHost.java`:</span><span class="sxs-lookup"><span data-stu-id="8c93f-160">In this example, hello process main entry point is `HelloWorldServiceHost.java`:</span></span>

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

## <a name="run-hello-application"></a><span data-ttu-id="8c93f-161">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8c93f-161">Run hello application</span></span>

<span data-ttu-id="8c93f-162">Hello Yeoman generování uživatelského rozhraní obsahuje gradle skriptu toobuild hello aplikace a bash skripty toodeploy a aplikaci odebrat.</span><span class="sxs-lookup"><span data-stu-id="8c93f-162">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="8c93f-163">toorun hello aplikace, první aplikace hello sestavení s gradlem:</span><span class="sxs-lookup"><span data-stu-id="8c93f-163">toorun hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="8c93f-164">Tímto se vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8c93f-164">This produces a Service Fabric application package that can be deployed using Service Fabric CLI.</span></span>

### <a name="deploy-with-service-fabric-cli"></a><span data-ttu-id="8c93f-165">Nasazení pomocí Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8c93f-165">Deploy with Service Fabric CLI</span></span>

<span data-ttu-id="8c93f-166">Hello install.sh skript obsahuje hello nezbytné Service Fabric rozhraní příkazového řádku příkazy toodeploy hello balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="8c93f-166">hello install.sh script contains hello necessary Service Fabric CLI commands toodeploy hello application package.</span></span> <span data-ttu-id="8c93f-167">Spusťte aplikaci install.sh skriptu toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="8c93f-167">Run the install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="8c93f-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c93f-168">Next steps</span></span>

* [<span data-ttu-id="8c93f-169">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="8c93f-169">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
