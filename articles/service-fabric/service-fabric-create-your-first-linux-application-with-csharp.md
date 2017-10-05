---
title: "Vytvoření první aplikace mikroslužeb Azure v Linuxu pomocí jazyka C# | Dokumentace Microsoftu"
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
ms.openlocfilehash: adcafaa5522fcddc0a01eb1dc8deba04ebfc38f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="0fcb1-103">Vytvoření první aplikace Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0fcb1-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0fcb1-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="0fcb1-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="0fcb1-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="0fcb1-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="0fcb1-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="0fcb1-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="0fcb1-107">Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="0fcb1-108">V tomto kurzu si projdeme postup vytvoření aplikace pro Linux a vytvoření služby pomocí jazyka C# (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-108">In this tutorial, we look at how to create an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fcb1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fcb1-109">Prerequisites</span></span>
<span data-ttu-id="0fcb1-110">Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="0fcb1-111">Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="0fcb1-112">Budete také chtít nainstalovat [Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-the-generators-for-csharp"></a><span data-ttu-id="0fcb1-113">Instalace a nastavení generátorů pro CSharp</span><span class="sxs-lookup"><span data-stu-id="0fcb1-113">Install and set up the generators for CSharp</span></span>
<span data-ttu-id="0fcb1-114">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric CSharp z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="0fcb1-115">Postupujte podle následujících kroků, abyste zkontrolovali, že máte na svém počítači funkční generátor šablon Service Fabric yeoman pro CSharp.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="0fcb1-116">Instalace nodejs a NPM na počítači</span><span class="sxs-lookup"><span data-stu-id="0fcb1-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="0fcb1-117">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="0fcb1-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="0fcb1-118">Instalace generátoru aplikací Service Fabric Yeo Java z NPM</span><span class="sxs-lookup"><span data-stu-id="0fcb1-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-the-application"></a><span data-ttu-id="0fcb1-119">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="0fcb1-119">Create the application</span></span>
<span data-ttu-id="0fcb1-120">Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-120">A Service Fabric application can contain one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="0fcb1-121">Generátor šablon Service Fabric [Yeoman](http://yeoman.io/) pro CSharp, který jste nainstalovali v posledním kroku, vám usnadní vytvoření první služby a případná další rozšíření později.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-121">The Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy to create your first service and to add more later.</span></span> <span data-ttu-id="0fcb1-122">Pomocí generátoru Yeoman vytvoříme aplikaci s jedinou službou.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-122">Let's use Yeoman to create an application with a single service.</span></span>

1. <span data-ttu-id="0fcb1-123">V terminálu zadejte následující příkaz, který zahájí sestavování základní kostry aplikace: `yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="0fcb1-123">In a terminal, type the following command to start building the scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="0fcb1-124">Pojmenujte svoji aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-124">Name your application.</span></span>
3. <span data-ttu-id="0fcb1-125">Vyberte typ první služby a pojmenujte ji.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-125">Choose the type of your first service and name it.</span></span> <span data-ttu-id="0fcb1-126">Pro účely tohoto kurzu zvolíme službu Reliable Actor.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-126">For the purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![Generátor Service Fabric Yeoman pro jazyk C#][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="0fcb1-128">Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-128">For more information about the options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-the-application"></a><span data-ttu-id="0fcb1-129">Sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="0fcb1-129">Build the application</span></span>
<span data-ttu-id="0fcb1-130">Šablony generátoru Service Fabric Yeoman zahrnují skript sestavení, který můžete použít k sestavení aplikace z terminálu (po přejití do složky aplikace).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-130">The Service Fabric Yeoman templates include a build script that you can use to build the app from the terminal (after navigating to the application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="0fcb1-131">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="0fcb1-131">Deploy the application</span></span>

<span data-ttu-id="0fcb1-132">Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-132">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="0fcb1-133">Připojte se k místnímu clusteru služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-133">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="0fcb1-134">Spuštěním instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-134">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="0fcb1-135">Nasazení sestavené aplikace je stejné jako u všech ostatních aplikací Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-135">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="0fcb1-136">Podrobné pokyny najdete v dokumentaci s popisem [správy aplikace Service Fabric pomocí Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-136">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="0fcb1-137">Parametry těchto příkazů najdete v generovaných manifestech uvnitř balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-137">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="0fcb1-138">Jakmile je aplikace nasazená, otevřete prohlížeč a přejděte k nástroji [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) na adrese [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="0fcb1-138">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="0fcb1-139">Pak rozbalte uzel **Aplikace** a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-139">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="0fcb1-140">Spuštění klienta testování a převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="0fcb1-140">Start the test client and perform a failover</span></span>
<span data-ttu-id="0fcb1-141">Projekty Actor samy o sobě nedělají nic.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="0fcb1-142">Vyžadují, aby jim jiná služba nebo klient posílali zprávy.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-142">They require another service or client to send them messages.</span></span> <span data-ttu-id="0fcb1-143">Šablona actor zahrnuje jednoduchý testovací skript, který můžete použít k interakci se službou actor.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-143">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="0fcb1-144">Spusťte skript pomocí pomocného sledovacího programu a prohlédněte si výstup služby actor.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-144">Run the script using the watch utility to see the output of the actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="0fcb1-145">V Service Fabric Exploreru vyhledejte uzel, který je hostitelem primární repliky pro službu actor.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-145">In Service Fabric Explorer, locate node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="0fcb1-146">Na snímku níže je to uzel 3.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-146">In the screenshot below, it is node 3.</span></span>

    ![Vyhledání primární repliky v Service Fabric Exploreru][sfx-primary]
3. <span data-ttu-id="0fcb1-148">Klikněte na uzel, který jste našli v předchozím kroku, a potom v nabídce Akce vyberte **Deaktivovat (restartovat)**.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-148">Click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="0fcb1-149">Tato akce restartuje jeden uzel v místním clusteru a vynutí převzetí služeb při selhání jednou ze sekundárních replik spuštěných v jiném uzlu.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-149">This action restarts one node in your local cluster forcing a failover to a secondary replica running on another node.</span></span> <span data-ttu-id="0fcb1-150">Při provádění této akce věnujte pozornost výstupu z klienta testování a všimněte si, že se čítač bez ohledu na převzetí služeb při selhání pořád postupně zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-150">As you perform this action, pay attention to the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="0fcb1-151">Přidání více služeb do stávající aplikace</span><span class="sxs-lookup"><span data-stu-id="0fcb1-151">Adding more services to an existing application</span></span>

<span data-ttu-id="0fcb1-152">Pokud chcete přidat další službu do aplikace již vytvořené pomocí `yo`, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0fcb1-152">To add another service to an application already created using `yo`, perform the following steps:</span></span>
1. <span data-ttu-id="0fcb1-153">Změňte adresář na kořenovou složku stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-153">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="0fcb1-154">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="0fcb1-155">Spusťte `yo azuresfcsharp:AddService`.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-to-csproj"></a><span data-ttu-id="0fcb1-156">Migrace z project.json na .csproj</span><span class="sxs-lookup"><span data-stu-id="0fcb1-156">Migrating from project.json to .csproj</span></span>
1. <span data-ttu-id="0fcb1-157">Spuštění „dotnet migrate“ v kořenovém adresáři projektu provede migraci všech souborů project.json na formát csproj.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-157">Running 'dotnet migrate' in project root directory will migrate all the project.json to csproj format.</span></span>
2. <span data-ttu-id="0fcb1-158">V souborech projektu příslušně aktualizujte odkazy projektu na soubory csproj.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-158">Update the project references accordingly to csproj files in project files.</span></span>
3. <span data-ttu-id="0fcb1-159">V souboru build.sh aktualizujte názvy souborů projektu na soubory csproj.</span><span class="sxs-lookup"><span data-stu-id="0fcb1-159">Update the project file names to csproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0fcb1-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0fcb1-160">Next steps</span></span>

* [<span data-ttu-id="0fcb1-161">Další informace o Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="0fcb1-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="0fcb1-162">Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0fcb1-162">Interacting with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="0fcb1-163">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="0fcb1-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="0fcb1-164">Začínáme se Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="0fcb1-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
