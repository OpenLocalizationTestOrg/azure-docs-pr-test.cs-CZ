---
title: "aaaCreate první aplikace Azure mikroslužeb v systému Linux pomocí jazyka C# | Microsoft Docs"
description: "Vytvoření a nasazení aplikace Service Fabric pomocí jazyka C#"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="02fe5-103">Vytvoření první aplikace Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="02fe5-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="02fe5-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="02fe5-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="02fe5-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="02fe5-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="02fe5-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="02fe5-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="02fe5-107">Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy.</span><span class="sxs-lookup"><span data-stu-id="02fe5-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="02fe5-108">V tomto kurzu se podíváme na to, jak toocreate aplikace pro systémy Linux a sestavení služby pomocí jazyka C# (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="02fe5-108">In this tutorial, we look at how toocreate an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02fe5-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="02fe5-109">Prerequisites</span></span>
<span data-ttu-id="02fe5-110">Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="02fe5-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="02fe5-111">Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="02fe5-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="02fe5-112">Můžete také tooinstall hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md)</span><span class="sxs-lookup"><span data-stu-id="02fe5-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-hello-generators-for-csharp"></a><span data-ttu-id="02fe5-113">Instalace a nastavení hello generátory pro CSharp</span><span class="sxs-lookup"><span data-stu-id="02fe5-113">Install and set up hello generators for CSharp</span></span>
<span data-ttu-id="02fe5-114">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric CSharp z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="02fe5-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="02fe5-115">Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro CSharp pracující na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="02fe5-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="02fe5-116">Instalace nodejs a NPM na počítači</span><span class="sxs-lookup"><span data-stu-id="02fe5-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="02fe5-117">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="02fe5-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="02fe5-118">Nainstalujte generátor aplikace Service Fabric jo s háčkem nad Java hello z NPM</span><span class="sxs-lookup"><span data-stu-id="02fe5-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a><span data-ttu-id="02fe5-119">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="02fe5-119">Create hello application</span></span>
<span data-ttu-id="02fe5-120">Aplikace Service Fabric může obsahovat jednu nebo více služeb, každý s určitou roli při poskytování funkcí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-120">A Service Fabric application can contain one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="02fe5-121">Hello Service Fabric [Yeoman](http://yeoman.io/) generátor pro CSharp, který jste nainstalovali v posledním kroku, umožňuje snadno toocreate první služby a tooadd více později.</span><span class="sxs-lookup"><span data-stu-id="02fe5-121">hello Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy toocreate your first service and tooadd more later.</span></span> <span data-ttu-id="02fe5-122">Jedinou službou použijeme Yeoman toocreate aplikace.</span><span class="sxs-lookup"><span data-stu-id="02fe5-122">Let's use Yeoman toocreate an application with a single service.</span></span>

1. <span data-ttu-id="02fe5-123">V terminálu zadejte následující příkaz toostart vytváření generování uživatelského rozhraní hello hello:`yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="02fe5-123">In a terminal, type hello following command toostart building hello scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="02fe5-124">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="02fe5-124">Name your application.</span></span>
3. <span data-ttu-id="02fe5-125">Vyberte typ hello vaší první služby a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="02fe5-125">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="02fe5-126">Pro účely hello tohoto kurzu vybereme možnost spolehlivé služby objektu Actor.</span><span class="sxs-lookup"><span data-stu-id="02fe5-126">For hello purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![Generátor Service Fabric Yeoman pro jazyk C#][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="02fe5-128">Další informace o možnostech hello najdete v tématu [Service Fabric přehled modelu programování](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="02fe5-128">For more information about hello options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-hello-application"></a><span data-ttu-id="02fe5-129">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="02fe5-129">Build hello application</span></span>
<span data-ttu-id="02fe5-130">šablony služby Fabric Yeoman Hello zahrňte skript sestavení, které můžete použít aplikace hello toobuild z terminálu hello (po přechodu složce toohello aplikace).</span><span class="sxs-lookup"><span data-stu-id="02fe5-130">hello Service Fabric Yeoman templates include a build script that you can use toobuild hello app from hello terminal (after navigating toohello application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="02fe5-131">Nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="02fe5-131">Deploy hello application</span></span>

<span data-ttu-id="02fe5-132">Po hello aplikace, můžete ho nasadit toohello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="02fe5-132">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="02fe5-133">Připojte toohello místní cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="02fe5-133">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="02fe5-134">Spusťte instalační skript hello zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-134">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="02fe5-135">Nasazení aplikace hello vytvořené je hello stejně jako všechny ostatní aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="02fe5-135">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="02fe5-136">Naleznete v dokumentaci k hello na [aplikace Service Fabric s hello Service Fabric rozhraní příkazového řádku pro správu](service-fabric-application-lifecycle-sfctl.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="02fe5-136">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="02fe5-137">Příkazy toothese parametrů naleznete v manifesty hello generuje uvnitř balíčku aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-137">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="02fe5-138">Po nasazení aplikace hello, otevřete prohlížeč a přejděte do [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) v [http://localhost: 19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="02fe5-138">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="02fe5-139">Potom rozbalte hello **aplikace** uzel a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="02fe5-139">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="02fe5-140">Spustit hello testovacího klienta a provést převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="02fe5-140">Start hello test client and perform a failover</span></span>
<span data-ttu-id="02fe5-141">Projekty Actor samy o sobě nedělají nic.</span><span class="sxs-lookup"><span data-stu-id="02fe5-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="02fe5-142">Vyžadují jiná služba nebo klienta toosend je zprávy.</span><span class="sxs-lookup"><span data-stu-id="02fe5-142">They require another service or client toosend them messages.</span></span> <span data-ttu-id="02fe5-143">Šablona objektu actor Hello obsahuje jednoduchá testovací skriptu, které můžete použít toointeract službou objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-143">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="02fe5-144">Spusťte skript hello pomocí výstup hello hello sledovat nástroj toosee služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-144">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="02fe5-145">V Service Fabric Exploreru najděte uzel, který je hostitelem primární repliky hello služby objektu actor hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-145">In Service Fabric Explorer, locate node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="02fe5-146">Na snímku obrazovky hello níže je uzel 3.</span><span class="sxs-lookup"><span data-stu-id="02fe5-146">In hello screenshot below, it is node 3.</span></span>

    ![Hledání hello primární repliky v Service Fabric Exploreru][sfx-primary]
3. <span data-ttu-id="02fe5-148">Klikněte na uzel hello v předchozím kroku hello najít a pak vyberte **deaktivovat (restartovat)** z nabídky akce hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-148">Click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="02fe5-149">Tato akce restartuje jeden uzel v místním clusteru vynucení převzetí služeb při selhání tooa sekundární repliky spuštěna na jiném uzlu.</span><span class="sxs-lookup"><span data-stu-id="02fe5-149">This action restarts one node in your local cluster forcing a failover tooa secondary replica running on another node.</span></span> <span data-ttu-id="02fe5-150">Jak provedete tuto akci, věnujte pozornost toohello výstup z hello testovacího klienta a Všimněte si, že tento čítač hello pokračuje tooincrement navzdory hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="02fe5-150">As you perform this action, pay attention toohello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="02fe5-151">Přidání další služby tooan existující aplikace</span><span class="sxs-lookup"><span data-stu-id="02fe5-151">Adding more services tooan existing application</span></span>

<span data-ttu-id="02fe5-152">tooadd jiná tooan aplikace služby již vytvořené pomocí `yo`, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="02fe5-152">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span>
1. <span data-ttu-id="02fe5-153">Změnit kořenový adresář toohello hello existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="02fe5-153">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="02fe5-154">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="02fe5-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="02fe5-155">Spusťte `yo azuresfcsharp:AddService`.</span><span class="sxs-lookup"><span data-stu-id="02fe5-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-toocsproj"></a><span data-ttu-id="02fe5-156">Migrace z project.json too.csproj</span><span class="sxs-lookup"><span data-stu-id="02fe5-156">Migrating from project.json too.csproj</span></span>
1. <span data-ttu-id="02fe5-157">Spuštění 'dotnet migraci"v kořenovém adresáři projektu bude migrovat všechny hello project.json toocsproj formátu.</span><span class="sxs-lookup"><span data-stu-id="02fe5-157">Running 'dotnet migrate' in project root directory will migrate all hello project.json toocsproj format.</span></span>
2. <span data-ttu-id="02fe5-158">Aktualizace hello projekt odkazuje na odpovídajícím způsobem toocsproj soubory v souborech projektu.</span><span class="sxs-lookup"><span data-stu-id="02fe5-158">Update hello project references accordingly toocsproj files in project files.</span></span>
3. <span data-ttu-id="02fe5-159">Aktualizujte hello toocsproj soubory názvy souborů projektu v build.sh.</span><span class="sxs-lookup"><span data-stu-id="02fe5-159">Update hello project file names toocsproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02fe5-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02fe5-160">Next steps</span></span>

* [<span data-ttu-id="02fe5-161">Další informace o Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="02fe5-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="02fe5-162">Interakci s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="02fe5-162">Interacting with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="02fe5-163">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="02fe5-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="02fe5-164">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="02fe5-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
