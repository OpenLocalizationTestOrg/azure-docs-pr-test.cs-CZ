---
title: "Vytvoření aplikace Azure Service Fabric Reliable Actors v Javě v Linuxu | Dokumentace Microsoftu"
description: "Zjistěte, jak za pět minut vytvořit a nasadit aplikaci Service Fabric Reliable Actors v Javě."
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
ms.openlocfilehash: baf948587ede31fe3d5b4f6f0981269b4cfe4d3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="ece1f-103">Vytvoření první aplikace Service Fabric Reliable Actors v Javě v Linuxu</span><span class="sxs-lookup"><span data-stu-id="ece1f-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ece1f-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="ece1f-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="ece1f-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="ece1f-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="ece1f-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="ece1f-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="ece1f-107">Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="ece1f-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="ece1f-108">Až budete hotovi, budete mít jednoduchou jednoúčelovou aplikaci v Javě spuštěnou v místním vývojovém clusteru.</span><span class="sxs-lookup"><span data-stu-id="ece1f-108">When you are finished, you'll have a simple Java single-service application running on the local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="ece1f-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ece1f-109">Prerequisites</span></span>
<span data-ttu-id="ece1f-110">Než začnete, nainstalujte sadu Service SDK a Service Fabric CLI a nastavte vývojový cluster ve svém [linuxovém vývojovém prostředí](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-110">Before you get started, install the Service Fabric SDK, the Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="ece1f-111">Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="ece1f-112">Budete také chtít nainstalovat [Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-the-generators-for-java"></a><span data-ttu-id="ece1f-113">Instalace a nastavení generátorů pro Javu</span><span class="sxs-lookup"><span data-stu-id="ece1f-113">Install and set up the generators for Java</span></span>
<span data-ttu-id="ece1f-114">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric Java z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="ece1f-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="ece1f-115">Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric yeoman pro Javu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="ece1f-116">Instalace nodejs a NPM na počítači</span><span class="sxs-lookup"><span data-stu-id="ece1f-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="ece1f-117">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="ece1f-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="ece1f-118">Instalace generátoru aplikací Service Fabric Yeo Java z NPM</span><span class="sxs-lookup"><span data-stu-id="ece1f-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a><span data-ttu-id="ece1f-119">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="ece1f-119">Create the application</span></span>
<span data-ttu-id="ece1f-120">Aplikace Service Fabric obsahuje jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="ece1f-120">A Service Fabric application contains one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="ece1f-121">Generátor, který jste nainstalovali v poslední části, vám usnadní vytvoření první služby a případná další rozšíření později.</span><span class="sxs-lookup"><span data-stu-id="ece1f-121">The generator you installed in the last section, makes it easy to create your first service and to add more later.</span></span>  <span data-ttu-id="ece1f-122">Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ece1f-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="ece1f-123">Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="ece1f-124">Pro účely tohoto rychlého startu použijte Yeoman k vytvoření aplikace s jednou službou, která ukládá a získává hodnotu čítače.</span><span class="sxs-lookup"><span data-stu-id="ece1f-124">For this quick start, use Yeoman to create an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="ece1f-125">V terminálu zadejte ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="ece1f-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="ece1f-126">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece1f-126">Name your application.</span></span>
3. <span data-ttu-id="ece1f-127">Vyberte typ první služby a pojmenujte ji.</span><span class="sxs-lookup"><span data-stu-id="ece1f-127">Choose the type of your first service and name it.</span></span> <span data-ttu-id="ece1f-128">Pro účely tohoto kurzu zvolte službu Reliable Actor.</span><span class="sxs-lookup"><span data-stu-id="ece1f-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="ece1f-129">Další informace o ostatních typech služeb najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-129">For more information about the other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="ece1f-130">![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="ece1f-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-the-application"></a><span data-ttu-id="ece1f-131">Sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="ece1f-131">Build the application</span></span>
<span data-ttu-id="ece1f-132">Šablony Service Fabric Yeoman zahrnují skript sestavení pro [Gradle](https://gradle.org/), který můžete použít k sestavení aplikace z terminálu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-132">The Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use to build the application from the terminal.</span></span>
<span data-ttu-id="ece1f-133">Závislosti Service Fabric Java se získávají z Mavenu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="ece1f-134">Chcete-li sestavovat aplikace Service Fabric Java a pracovat s nimi, musíte zajistit, že máte nainstalovanou sadu JDK a Gradle.</span><span class="sxs-lookup"><span data-stu-id="ece1f-134">To build and work on the Service Fabric Java applications, you need to ensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="ece1f-135">Pokud ještě nejsou instalované, můžete nainstalovat sadu JDK(openjdk-8-jdk) a Gradle spuštěním následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="ece1f-135">If not yet installed, you can run the following to install JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="ece1f-136">Pokud chcete sestavit a zabalit aplikaci, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="ece1f-136">To build and package the application, run the following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="ece1f-137">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="ece1f-137">Deploy the application</span></span>
<span data-ttu-id="ece1f-138">Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru.</span><span class="sxs-lookup"><span data-stu-id="ece1f-138">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="ece1f-139">Připojte se k místnímu clusteru služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ece1f-139">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="ece1f-140">Spuštěním instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.</span><span class="sxs-lookup"><span data-stu-id="ece1f-140">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="ece1f-141">Nasazení sestavené aplikace je stejné jako u všech ostatních aplikací Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ece1f-141">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="ece1f-142">Podrobné pokyny najdete v dokumentaci s popisem [správy aplikace Service Fabric pomocí Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-142">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="ece1f-143">Parametry těchto příkazů najdete v generovaných manifestech uvnitř balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="ece1f-143">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="ece1f-144">Jakmile je aplikace nasazená, otevřete prohlížeč a přejděte k nástroji [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) na adrese [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="ece1f-144">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="ece1f-145">Pak rozbalte uzel **Aplikace** a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-145">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="ece1f-146">Spuštění klienta testování a převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="ece1f-146">Start the test client and perform a failover</span></span>
<span data-ttu-id="ece1f-147">Samotné objekty actor nic nedělají – vyžadují, aby jim jiná služba nebo klient posílali zprávy.</span><span class="sxs-lookup"><span data-stu-id="ece1f-147">Actors do not do anything on their own, they require another service or client to send them messages.</span></span> <span data-ttu-id="ece1f-148">Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.</span><span class="sxs-lookup"><span data-stu-id="ece1f-148">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="ece1f-149">Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.</span><span class="sxs-lookup"><span data-stu-id="ece1f-149">Run the script using the watch utility to see the output of the actor service.</span></span>  <span data-ttu-id="ece1f-150">Testovací skript volá metodu `setCountAsync()` objektu actor pro zvýšení čítače a metodu `getCountAsync()` objektu actor pro získání nové hodnoty čítače, kterou zobrazí v konzole.</span><span class="sxs-lookup"><span data-stu-id="ece1f-150">The test script calls the `setCountAsync()` method on the actor to increment a counter, calls the `getCountAsync()` method on the actor to get the new counter value, and displays that value to the console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="ece1f-151">V nástroji Service Fabric Explorer vyhledejte uzel, který je hostitelem primární repliky pro službu objektu actor.</span><span class="sxs-lookup"><span data-stu-id="ece1f-151">In Service Fabric Explorer, locate the node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="ece1f-152">Na snímku níže je to uzel 3.</span><span class="sxs-lookup"><span data-stu-id="ece1f-152">In the screenshot below, it is node 3.</span></span> <span data-ttu-id="ece1f-153">Primární replika služby zpracovává operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-153">The primary service replica handles read and write operations.</span></span>  <span data-ttu-id="ece1f-154">Změny stavu služby se následně replikují do sekundárních replik, které jsou na následujícím snímku obrazovku spuštěné na uzlech 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="ece1f-154">Changes in service state are then replicated out to the secondary replicas, running on nodes 0 and 1 in the screen shot below.</span></span>

    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]

3. <span data-ttu-id="ece1f-156">V části **Uzly** klikněte na uzel, který jste našli v předchozím kroku, a pak v nabídce Akce vyberte **Deaktivovat (restartovat)**.</span><span class="sxs-lookup"><span data-stu-id="ece1f-156">In **Nodes**, click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="ece1f-157">Tato akce restartuje uzel spuštěný v primární replice služby a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných na jiném uzlu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-157">This action restarts the node running the primary service replica and forces a failover to one of the secondary replicas running on another node.</span></span>  <span data-ttu-id="ece1f-158">Úroveň této sekundární repliky se zvýší na primární, v jiném uzlu se vytvoří jiná sekundární replika a primární replika začne přijímat operace čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-158">That secondary replica is promoted to primary, another secondary replica is created on a different node, and the primary replica begins to take read/write operations.</span></span> <span data-ttu-id="ece1f-159">Při restartování uzlu sledujte výstup z testovacího klienta a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="ece1f-159">As the node restarts, watch the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="remove-the-application"></a><span data-ttu-id="ece1f-160">Odebrání aplikace</span><span class="sxs-lookup"><span data-stu-id="ece1f-160">Remove the application</span></span>
<span data-ttu-id="ece1f-161">Pomocí odinstalačního skriptu, který je součástí šablony, odstraňte instanci aplikace, zrušte registraci balíčku aplikace a odeberete balíček aplikace z úložiště imagí clusteru.</span><span class="sxs-lookup"><span data-stu-id="ece1f-161">Use the uninstall script provided in the template to delete the application instance, unregister the application package, and remove the application package from the cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="ece1f-162">V nástroji Service Fabric Explorer uvidíte, že se aplikace a typ aplikace už nezobrazují v uzlu **Aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ece1f-162">In Service Fabric explorer you see that the application and application type no longer appear in the **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="ece1f-163">Knihovny Service Fabric Java v Mavenu</span><span class="sxs-lookup"><span data-stu-id="ece1f-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="ece1f-164">Hostitelem knihoven Service Fabric Java je Maven.</span><span class="sxs-lookup"><span data-stu-id="ece1f-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="ece1f-165">Můžete přidat závislosti do souborů ``pom.xml`` nebo ``build.gradle`` vašich projektů, aby se používaly knihovny Service Fabric Java z úložiště **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="ece1f-165">You can add the dependencies in the ``pom.xml`` or ``build.gradle`` of your projects to use Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="ece1f-166">Objekty actor</span><span class="sxs-lookup"><span data-stu-id="ece1f-166">Actors</span></span>

<span data-ttu-id="ece1f-167">Podpora Service Fabric Reliable Actor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece1f-167">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="ece1f-168">Služby</span><span class="sxs-lookup"><span data-stu-id="ece1f-168">Services</span></span>

<span data-ttu-id="ece1f-169">Podpora bezstavové služby Service Fabric pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ece1f-169">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="ece1f-170">Ostatní</span><span class="sxs-lookup"><span data-stu-id="ece1f-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="ece1f-171">Přenos</span><span class="sxs-lookup"><span data-stu-id="ece1f-171">Transport</span></span>

<span data-ttu-id="ece1f-172">Podpora přenosové vrstvy pro aplikace Service Fabric Java.</span><span class="sxs-lookup"><span data-stu-id="ece1f-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="ece1f-173">Pokud neprogramujete na úrovni přenosové vrstvy, nemusíte tuto závislost do aplikace Reliable Actor nebo aplikace služby explicitně přidávat.</span><span class="sxs-lookup"><span data-stu-id="ece1f-173">You do not need to explicitly add this dependency to your Reliable Actor or Service applications, unless you program at the transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="ece1f-174">Podpora prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="ece1f-174">Fabric support</span></span>

<span data-ttu-id="ece1f-175">Podpora na úrovni systému pro Service Fabric, která komunikuje s modulem runtime nativním pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ece1f-175">System level support for Service Fabric, which talks to native Service Fabric runtime.</span></span> <span data-ttu-id="ece1f-176">Tuto závislost nemusíte do aplikace Reliable Actor nebo aplikace služby explicitně přidávat.</span><span class="sxs-lookup"><span data-stu-id="ece1f-176">You do not need to explicitly add this dependency to your Reliable Actor or Service applications.</span></span> <span data-ttu-id="ece1f-177">Načte se z Mavenu automaticky, jakmile zahrnete ostatní závislosti uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="ece1f-177">This gets fetched automatically from Maven, when you include the other dependencies above.</span></span>

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

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="ece1f-178">Migrace starých aplikací Service Fabric Java pro použití s Mavenem</span><span class="sxs-lookup"><span data-stu-id="ece1f-178">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="ece1f-179">Nedávno jsme přesunuli knihovny Service Fabric Java ze sady Service Fabric Java SDK do úložiště Maven.</span><span class="sxs-lookup"><span data-stu-id="ece1f-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="ece1f-180">Zatímco nové aplikace, které vygenerujete pomocí Yeomanu nebo Eclipse, vygenerují nejnovější aktualizované projekty (které budou moct pracovat s Mavenem), můžete vaše stávající bezstavové aplikace nebo aplikace objektu actor v Javě pro Service Fabric, které dřív používali sadu Service Fabric Java SDK, aktualizovat tak, aby používaly závislosti Service Fabric Java z Mavenu.</span><span class="sxs-lookup"><span data-stu-id="ece1f-180">While the new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="ece1f-181">Abyste zajistili fungování starších aplikací s Mavenem, postupujte podle kroků uvedených [tady](service-fabric-migrate-old-javaapp-to-use-maven.md).</span><span class="sxs-lookup"><span data-stu-id="ece1f-181">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ece1f-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ece1f-182">Next steps</span></span>

* [<span data-ttu-id="ece1f-183">Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse</span><span class="sxs-lookup"><span data-stu-id="ece1f-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="ece1f-184">Další informace o Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="ece1f-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="ece1f-185">Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ece1f-185">Interact with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="ece1f-186">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="ece1f-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="ece1f-187">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="ece1f-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
