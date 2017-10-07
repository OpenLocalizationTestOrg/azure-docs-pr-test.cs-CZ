---
title: "aaaCreate aplikace Java spolehlivé aktéři Azure Service Fabric v systému Linux | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení aplikace Java Service Fabric spolehlivé aktéři za pět minut."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.openlocfilehash: 11496b767811c89969c65d1682d843448eb6a922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="128cb-103">Vytvoření první aplikace Service Fabric Reliable Actors v Javě v Linuxu</span><span class="sxs-lookup"><span data-stu-id="128cb-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="128cb-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="128cb-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="128cb-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="128cb-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="128cb-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="128cb-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="128cb-107">Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="128cb-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="128cb-108">Jakmile budete hotovi, budete mít jednoduchou aplikaci Java jedním služby systémem hello místní vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="128cb-108">When you are finished, you'll have a simple Java single-service application running on hello local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="128cb-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="128cb-109">Prerequisites</span></span>
<span data-ttu-id="128cb-110">Než začnete, instalaci hello Service Fabric SDK, hello Service Fabric rozhraní příkazového řádku a nastavit cluster vývoj ve vaší [vývojového prostředí Linux](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="128cb-110">Before you get started, install hello Service Fabric SDK, hello Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="128cb-111">Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="128cb-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="128cb-112">Můžete také tooinstall hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="128cb-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-hello-generators-for-java"></a><span data-ttu-id="128cb-113">Instalace a nastavení hello generátory pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="128cb-113">Install and set up hello generators for Java</span></span>
<span data-ttu-id="128cb-114">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric Java z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="128cb-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="128cb-115">Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro jazyk Java pracující na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="128cb-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="128cb-116">Instalace nodejs a NPM na počítači</span><span class="sxs-lookup"><span data-stu-id="128cb-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="128cb-117">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="128cb-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="128cb-118">Nainstalujte generátor aplikace Service Fabric jo s háčkem nad Java hello z NPM</span><span class="sxs-lookup"><span data-stu-id="128cb-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a><span data-ttu-id="128cb-119">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="128cb-119">Create hello application</span></span>
<span data-ttu-id="128cb-120">Aplikace Service Fabric obsahuje jednu nebo více služeb, každý s určitou roli při poskytování funkcí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-120">A Service Fabric application contains one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="128cb-121">Generátor Hello jste nainstalovali v poslední části hello umožňuje snadno toocreate první služby a tooadd více později.</span><span class="sxs-lookup"><span data-stu-id="128cb-121">hello generator you installed in hello last section, makes it easy toocreate your first service and tooadd more later.</span></span>  <span data-ttu-id="128cb-122">Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="128cb-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="128cb-123">Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="128cb-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="128cb-124">Pro tento rychlý start použijte Yeoman toocreate aplikace jedinou službou, která uchovává a získá hodnotu čítače.</span><span class="sxs-lookup"><span data-stu-id="128cb-124">For this quick start, use Yeoman toocreate an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="128cb-125">V terminálu zadejte ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="128cb-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="128cb-126">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="128cb-126">Name your application.</span></span>
3. <span data-ttu-id="128cb-127">Vyberte typ hello vaší první služby a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="128cb-127">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="128cb-128">Pro účely tohoto kurzu zvolte službu Reliable Actor.</span><span class="sxs-lookup"><span data-stu-id="128cb-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="128cb-129">Další informace o hello jiné typy služeb, naleznete v části [Service Fabric přehled modelu programování](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="128cb-129">For more information about hello other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="128cb-130">![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="128cb-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="128cb-131">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="128cb-131">Build hello application</span></span>
<span data-ttu-id="128cb-132">šablony služby Fabric Yeoman Hello zahrnují sestavení skript pro [Gradle](https://gradle.org/), které můžete použít aplikaci hello toobuild z hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="128cb-132">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span>
<span data-ttu-id="128cb-133">Závislosti Service Fabric Java se získávají z Mavenu.</span><span class="sxs-lookup"><span data-stu-id="128cb-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="128cb-134">toobuild a práce na hello aplikací Service Fabric Java, je nutné tooensure, že máte JDK a Gradle nainstalována.</span><span class="sxs-lookup"><span data-stu-id="128cb-134">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="128cb-135">Pokud dosud není nainstalován, můžete spustit hello tooinstall JDK(openjdk-8-jdk) a Gradle -</span><span class="sxs-lookup"><span data-stu-id="128cb-135">If not yet installed, you can run hello following tooinstall JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="128cb-136">toobuild a balíček hello aplikace, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="128cb-136">toobuild and package hello application, run hello following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="128cb-137">Nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="128cb-137">Deploy hello application</span></span>
<span data-ttu-id="128cb-138">Po hello aplikace, můžete ho nasadit toohello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="128cb-138">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="128cb-139">Připojte toohello místní cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="128cb-139">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="128cb-140">Spusťte instalační skript hello zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-140">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="128cb-141">Nasazení aplikace hello vytvořené je hello stejně jako všechny ostatní aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="128cb-141">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="128cb-142">Naleznete v dokumentaci k hello na [aplikace Service Fabric s hello Service Fabric rozhraní příkazového řádku pro správu](service-fabric-application-lifecycle-sfctl.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="128cb-142">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="128cb-143">Příkazy toothese parametrů naleznete v manifesty hello generuje uvnitř balíčku aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-143">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="128cb-144">Po nasazení aplikace hello, otevřete prohlížeč a přejděte do [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) v [http://localhost: 19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="128cb-144">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="128cb-145">Potom rozbalte hello **aplikace** uzel a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="128cb-145">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="128cb-146">Spustit hello testovacího klienta a provést převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="128cb-146">Start hello test client and perform a failover</span></span>
<span data-ttu-id="128cb-147">Aktéři udělat nic samostatně, vyžadují jiná služba nebo klienta toosend je zprávy.</span><span class="sxs-lookup"><span data-stu-id="128cb-147">Actors do not do anything on their own, they require another service or client toosend them messages.</span></span> <span data-ttu-id="128cb-148">Šablona objektu actor Hello obsahuje jednoduchá testovací skriptu, které můžete použít toointeract službou objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-148">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="128cb-149">Spusťte skript hello pomocí výstup hello hello sledovat nástroj toosee služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-149">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>  <span data-ttu-id="128cb-150">Hello zkušební skript volá hello `setCountAsync()` volá metodu tooincrement objektu actor hello čítač, hello `getCountAsync()` metodu hello objektu actor tooget hello nová hodnota čítače a zobrazí, které hodnoty toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="128cb-150">hello test script calls hello `setCountAsync()` method on hello actor tooincrement a counter, calls hello `getCountAsync()` method on hello actor tooget hello new counter value, and displays that value toohello console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="128cb-151">V Service Fabric Explorer vyhledejte hello uzly hostování hello primární replika služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-151">In Service Fabric Explorer, locate hello node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="128cb-152">Na snímku obrazovky hello níže je uzel 3.</span><span class="sxs-lookup"><span data-stu-id="128cb-152">In hello screenshot below, it is node 3.</span></span> <span data-ttu-id="128cb-153">obslužné rutiny repliky primární služba Hello operacích čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="128cb-153">hello primary service replica handles read and write operations.</span></span>  <span data-ttu-id="128cb-154">Změny ve stavu service jsou poté replikovány na sekundárních replikách toohello, spuštěná na uzlech 0 a 1 v úvodní obrazovka snímek níže.</span><span class="sxs-lookup"><span data-stu-id="128cb-154">Changes in service state are then replicated out toohello secondary replicas, running on nodes 0 and 1 in hello screen shot below.</span></span>

    ![Hledání hello primární repliky v Service Fabric Exploreru][sfx-primary]

3. <span data-ttu-id="128cb-156">V **uzly**, klikněte na uzel hello v předchozím kroku hello najít a pak vyberte **deaktivovat (restartovat)** z nabídky akce hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-156">In **Nodes**, click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="128cb-157">Tato akce restartuje hello uzlu se systémem hello primární služba repliky a vynutí tooone převzetí služeb při selhání replik sekundární hello systémem v jiném uzlu.</span><span class="sxs-lookup"><span data-stu-id="128cb-157">This action restarts hello node running hello primary service replica and forces a failover tooone of hello secondary replicas running on another node.</span></span>  <span data-ttu-id="128cb-158">Tato sekundární replika je propagovaných tooprimary, jiné sekundární repliky se vytvoří na jiný uzel a primární repliky hello začne tootake operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="128cb-158">That secondary replica is promoted tooprimary, another secondary replica is created on a different node, and hello primary replica begins tootake read/write operations.</span></span> <span data-ttu-id="128cb-159">Při restartování uzlu hello, sledujte hello výstup hello testovacího klienta a Všimněte si, že tento čítač hello pokračuje tooincrement navzdory hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="128cb-159">As hello node restarts, watch hello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="remove-hello-application"></a><span data-ttu-id="128cb-160">Odebrání aplikace hello</span><span class="sxs-lookup"><span data-stu-id="128cb-160">Remove hello application</span></span>
<span data-ttu-id="128cb-161">Použít skript odinstalovat hello uvedený v instanci aplikace hello šablony toodelete hello, zrušení registrace balíčku aplikace hello a odebrání balíčku aplikace hello z úložiště bitových kopií hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="128cb-161">Use hello uninstall script provided in hello template toodelete hello application instance, unregister hello application package, and remove hello application package from hello cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="128cb-162">V Service Fabric explorer uvidíte, že aplikace hello a typ aplikace se v již hello **aplikace** uzlu.</span><span class="sxs-lookup"><span data-stu-id="128cb-162">In Service Fabric explorer you see that hello application and application type no longer appear in hello **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="128cb-163">Knihovny Service Fabric Java v Mavenu</span><span class="sxs-lookup"><span data-stu-id="128cb-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="128cb-164">Hostitelem knihoven Service Fabric Java je Maven.</span><span class="sxs-lookup"><span data-stu-id="128cb-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="128cb-165">Můžete přidat závislosti hello v hello ``pom.xml`` nebo ``build.gradle`` knihoven vaše projekty toouse Service Fabric Java z **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="128cb-165">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="128cb-166">Objekty actor</span><span class="sxs-lookup"><span data-stu-id="128cb-166">Actors</span></span>

<span data-ttu-id="128cb-167">Podpora Service Fabric Reliable Actor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="128cb-167">Service Fabric Reliable Actor support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a><span data-ttu-id="128cb-168">Služby</span><span class="sxs-lookup"><span data-stu-id="128cb-168">Services</span></span>

<span data-ttu-id="128cb-169">Podpora bezstavové služby Service Fabric pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="128cb-169">Service Fabric Stateless Service support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a><span data-ttu-id="128cb-170">Ostatní</span><span class="sxs-lookup"><span data-stu-id="128cb-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="128cb-171">Přenos</span><span class="sxs-lookup"><span data-stu-id="128cb-171">Transport</span></span>

<span data-ttu-id="128cb-172">Podpora přenosové vrstvy pro aplikace Service Fabric Java.</span><span class="sxs-lookup"><span data-stu-id="128cb-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="128cb-173">Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby, pokud program v přenosové vrstvě hello.</span><span class="sxs-lookup"><span data-stu-id="128cb-173">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a><span data-ttu-id="128cb-174">Podpora prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="128cb-174">Fabric support</span></span>

<span data-ttu-id="128cb-175">Systém úrovně podpory pro Service Fabric, která komunikuje toonative modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="128cb-175">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="128cb-176">Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="128cb-176">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="128cb-177">To získá automaticky načtených z Maven, jakmile zahrnete hello jiných závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="128cb-177">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="128cb-178">Migrace starého toobe aplikace Service Fabric Java použít s Maven</span><span class="sxs-lookup"><span data-stu-id="128cb-178">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="128cb-179">Nedávno jsme přesunuli knihovny Service Fabric Java z úložiště tooMaven Service Fabric Java SDK.</span><span class="sxs-lookup"><span data-stu-id="128cb-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="128cb-180">Při hello nové aplikace, který generovat pomocí Yeoman nebo Eclipse, vygeneruje nejnovější aktualizované projekty (které bude možné toowork s Maven), můžete aktualizovat existující Service Fabric bezstavové nebo objektu actor aplikací Java, které byly pomocí hello služby Fabric Java SDK dříve, toouse hello Service Fabric Java závislostí z Maven.</span><span class="sxs-lookup"><span data-stu-id="128cb-180">While hello new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="128cb-181">Postupujte podle kroků hello [sem](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure starší aplikace funguje s Maven.</span><span class="sxs-lookup"><span data-stu-id="128cb-181">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="128cb-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="128cb-182">Next steps</span></span>

* [<span data-ttu-id="128cb-183">Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="128cb-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="128cb-184">Další informace o Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="128cb-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="128cb-185">Komunikovat s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="128cb-185">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="128cb-186">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="128cb-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="128cb-187">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="128cb-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
