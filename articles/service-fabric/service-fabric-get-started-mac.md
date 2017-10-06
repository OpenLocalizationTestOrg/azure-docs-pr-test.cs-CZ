---
title: "aaaSet vývojového prostředí v systému Mac OS X toowork s Azure Service Fabric | Microsoft Docs"
description: "Nainstalujte hello runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace, bude připravená toobuild aplikace v systému Mac OS X."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a><span data-ttu-id="6e3cc-104">Nastavení vývojového prostředí v Mac OS X</span><span class="sxs-lookup"><span data-stu-id="6e3cc-104">Set up your development environment on Mac OS X</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e3cc-105">Windows</span><span class="sxs-lookup"><span data-stu-id="6e3cc-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="6e3cc-106">Linux</span><span class="sxs-lookup"><span data-stu-id="6e3cc-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="6e3cc-107">OSX</span><span class="sxs-lookup"><span data-stu-id="6e3cc-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="6e3cc-108">Můžete vytvořit toorun aplikace Service Fabric na clusterech Linux pomocí Mac OS X. Tento článek popisuje jak tooset si svůj počítač Mac pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-108">You can build Service Fabric applications toorun on Linux clusters using Mac OS X. This article covers how tooset up your Mac for development.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e3cc-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e3cc-109">Prerequisites</span></span>
<span data-ttu-id="6e3cc-110">Service Fabric nefunguje nativně na OS X. toorun místní cluster Service Fabric, poskytujeme předem nakonfigurovaná virtuálního počítače s Ubuntu pomocí Vagrant a VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-110">Service Fabric does not run natively on OS X. toorun a local Service Fabric cluster, we provide a pre-configured Ubuntu virtual machine using Vagrant and VirtualBox.</span></span> <span data-ttu-id="6e3cc-111">Než začnete, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6e3cc-111">Before you get started, you need:</span></span>

* [<span data-ttu-id="6e3cc-112">Vagrant (verzi 1.8.4 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="6e3cc-112">Vagrant (v1.8.4 or later)</span></span>](http://www.vagrantup.com/downloads.html)
* [<span data-ttu-id="6e3cc-113">VirtualBox</span><span class="sxs-lookup"><span data-stu-id="6e3cc-113">VirtualBox</span></span>](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> <span data-ttu-id="6e3cc-114">Je nutné toouse vzájemně podporované verze Vagrant a VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-114">You need toouse mutually supported versions of Vagrant and VirtualBox.</span></span> <span data-ttu-id="6e3cc-115">Vagrant se může chovat chybně v nepodporované verzi VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-115">Vagrant might behave erratically on an unsupported VirtualBox version.</span></span>
>

## <a name="create-hello-local-vm"></a><span data-ttu-id="6e3cc-116">Vytvoření hello Virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6e3cc-116">Create hello local VM</span></span>
<span data-ttu-id="6e3cc-117">toocreate hello obsahující cluster Service Fabric 5 uzlu Virtuálního počítače, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6e3cc-117">toocreate hello local VM containing a 5-node Service Fabric cluster, perform hello following steps:</span></span>

1. <span data-ttu-id="6e3cc-118">Klon hello `Vagrantfile` úložišti</span><span class="sxs-lookup"><span data-stu-id="6e3cc-118">Clone hello `Vagrantfile` repo</span></span>

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    <span data-ttu-id="6e3cc-119">Tento postup přineste seznamy hello soubor `Vagrantfile` obsahující hello virtuálních počítačů konfigurace společně s hello hello umístění virtuálních počítačů se stáhne ze.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-119">This steps bring downs hello file `Vagrantfile` containing hello VM configuration along with hello location hello VM is downloaded from.</span></span>

2. <span data-ttu-id="6e3cc-120">Přejděte toohello místní klon hello úložišti</span><span class="sxs-lookup"><span data-stu-id="6e3cc-120">Navigate toohello local clone of hello repo</span></span>

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. <span data-ttu-id="6e3cc-121">(Volitelné) Úprava nastavení virtuálního počítače výchozí hello</span><span class="sxs-lookup"><span data-stu-id="6e3cc-121">(Optional) Modify hello default VM settings</span></span>

    <span data-ttu-id="6e3cc-122">Ve výchozím nastavení, hello místní virtuální počítač je nakonfigurovaný následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6e3cc-122">By default, hello local VM is configured as follows:</span></span>

   * <span data-ttu-id="6e3cc-123">3 GB přidělené paměti</span><span class="sxs-lookup"><span data-stu-id="6e3cc-123">3 GB of memory allocated</span></span>
   * <span data-ttu-id="6e3cc-124">Síť privátní hostitele nakonfigurované na IP Adrese 192.168.50.50 povolení průchodu provozu z hostitele Mac hello</span><span class="sxs-lookup"><span data-stu-id="6e3cc-124">Private host network configured at IP 192.168.50.50 enabling passthrough of traffic from hello Mac host</span></span>

     <span data-ttu-id="6e3cc-125">Můžete buď z těchto nastavení změnit nebo přidat další toohello konfigurace virtuálních počítačů v hello `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-125">You can change either of these settings or add other configuration toohello VM in hello `Vagrantfile`.</span></span> <span data-ttu-id="6e3cc-126">V tématu hello [Vagrant dokumentace](http://www.vagrantup.com/docs) hello úplný seznam možností konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-126">See hello [Vagrant documentation](http://www.vagrantup.com/docs) for hello full list of configuration options.</span></span>
4. <span data-ttu-id="6e3cc-127">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6e3cc-127">Create hello VM</span></span>

    ```bash
    vagrant up
    ```

   <span data-ttu-id="6e3cc-128">Tento krok stáhne hello předkonfigurované image virtuálního počítače, spuštění, které ho místně a pak nastavte místní Service Fabric clusteru v ní.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-128">This step downloads hello preconfigured VM image, boot it locally, and then set up a local Service Fabric cluster in it.</span></span> <span data-ttu-id="6e3cc-129">Byste měli očekávat ho tootake několik minut.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-129">You should expect it tootake a few minutes.</span></span> <span data-ttu-id="6e3cc-130">Pokud se instalační program úspěšně dokončí, zobrazí se zpráva ve výstupu hello oznamující, že je tento cluster hello spuštění.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-130">If setup completes successfully, you see a message in hello output indicating that hello cluster is starting up.</span></span>

    ![Spouštění instalace clusteru po zřízení virtuálního počítače][cluster-setup-script]

    >[!TIP]
    > <span data-ttu-id="6e3cc-132">Pokud stahování virtuálních počítačů hello trvá dlouhou dobu, si můžete stáhnout pomocí wget nebo adresu curl nebo prostřednictvím prohlížeče tak, že přejdete toohello odkaz určeného **config.vm.box_url** v souboru hello `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-132">If hello VM download is taking a long time, you can download it using wget or curl or through a browser by navigating toohello link specified by **config.vm.box_url** in hello file `Vagrantfile`.</span></span> <span data-ttu-id="6e3cc-133">Po stažení místně, upravit `Vagrantfile` toopoint toohello místní cestu, kam jste stáhli hello image.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-133">After downloading it locally, edit `Vagrantfile` toopoint toohello local path where you downloaded hello image.</span></span> <span data-ttu-id="6e3cc-134">Například pokud jste stáhli hello too/home/users/test/azureservicefabric.tp8.box bitové kopie, pak nastavený **config.vm.box_url** toothat cesta.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-134">For example if you downloaded hello image too/home/users/test/azureservicefabric.tp8.box, then set **config.vm.box_url** toothat path.</span></span>
    >

5. <span data-ttu-id="6e3cc-135">Testování této hello clusteru má správně nastaven tak, že přejdete tooService Fabric Explorer na http://192.168.50.50:19080/Explorer (za předpokladu, že jste u hello výchozí privátní síť IP).</span><span class="sxs-lookup"><span data-stu-id="6e3cc-135">Test that hello cluster has been set up correctly by navigating tooService Fabric Explorer at http://192.168.50.50:19080/Explorer (assuming you kept hello default private network IP).</span></span>

    ![Zobrazit z hostitele hello Mac Service Fabric Exploreru][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a><span data-ttu-id="6e3cc-137">Vytvoření aplikace na počítači Mac pomocí Yeomanu</span><span class="sxs-lookup"><span data-stu-id="6e3cc-137">Create application on Mac using Yeoman</span></span>
<span data-ttu-id="6e3cc-138">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-138">Service Fabric provides scaffolding tools which will help you create a Service Fabric application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="6e3cc-139">Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pracující na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-139">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator working on your machine.</span></span>

1. <span data-ttu-id="6e3cc-140">Potřebujete toohave Node.js a NPM, nainstalovaná v systému mac je.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-140">You need toohave Node.js and NPM installed on you mac.</span></span> <span data-ttu-id="6e3cc-141">Pokud není můžete nainstalovat Node.js a NPM pomocí Homebrew pomocí následující hello.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-141">If not you can install Node.js and NPM using Homebrew using hello following.</span></span> <span data-ttu-id="6e3cc-142">toocheck hello verze Node.js a NPM nainstalovaná na počítači Mac, můžete použít hello ``-v`` možnost.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-142">toocheck hello versions of Node.js and NPM installed on your Mac, you can use hello ``-v`` option.</span></span>

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. <span data-ttu-id="6e3cc-143">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="6e3cc-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  npm install -g yo
  ```
3. <span data-ttu-id="6e3cc-144">Nainstalujte hello Yeoman generátor chcete toouse, proveďte kroky hello v hello Začínáme [dokumentaci](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6e3cc-144">Install hello Yeoman generator you want toouse, following hello steps in hello getting started [documentation](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="6e3cc-145">toocreate Service Fabric aplikací pomocí Yeoman, postupujte podle kroků hello-</span><span class="sxs-lookup"><span data-stu-id="6e3cc-145">toocreate Service Fabric Applications using Yeoman, follow hello steps -</span></span>

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. <span data-ttu-id="6e3cc-146">toobuild aplikace Service Fabric Java v systému Mac, potřebovali byste - JDK 1.8 a nainstalovat na počítač hello Gradle.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-146">toobuild a Service Fabric Java application on Mac, you would need - JDK 1.8 and Gradle installed on hello machine.</span></span>


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a><span data-ttu-id="6e3cc-147">Instalace modulu plug-in hello Service Fabric pro Neónová Eclipse</span><span class="sxs-lookup"><span data-stu-id="6e3cc-147">Install hello Service Fabric plugin for Eclipse Neon</span></span>

<span data-ttu-id="6e3cc-148">Service Fabric nabízí o modul plug-in pro hello **Eclipse Neónová pro Java IDE** , může zjednodušit proces vytváření, sestavování a nasazení služeb Java hello.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-148">Service Fabric provides a plugin for hello **Eclipse Neon for Java IDE** that can simplify hello process of creating, building, and deploying Java services.</span></span> <span data-ttu-id="6e3cc-149">Můžete provést kroky pro instalaci hello uvedených v této Obecné [dokumentace](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) o instalaci nebo aktualizaci modulů plug-in služby Fabric Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-149">You can follow hello installation steps mentioned in this general [documentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) about installing or updating Service Fabric Eclipse plugin.</span></span>

>[!TIP]
> <span data-ttu-id="6e3cc-150">Ve výchozím nastavení podporujeme hello výchozí IP jak je uvedeno v hello ``Vagrantfile`` v hello ``Local.json`` hello generované aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-150">By default we support hello default IP as mentioned in hello ``Vagrantfile`` in hello ``Local.json`` of hello generated application.</span></span> <span data-ttu-id="6e3cc-151">V případě, že změnit a nasadíte Vagrant s jinou IP Adresou, aktualizujte prosím odpovídající IP hello v ``Local.json`` také vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e3cc-151">In case you change that and deploy Vagrant with a different IP, please update hello corresponding IP in ``Local.json`` of your application as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e3cc-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e3cc-152">Next steps</span></span>
<!-- Links -->
* [<span data-ttu-id="6e3cc-153">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana</span><span class="sxs-lookup"><span data-stu-id="6e3cc-153">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="6e3cc-154">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="6e3cc-154">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="6e3cc-155">Vytvoření clusteru Service Fabric v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e3cc-155">Create a Service Fabric cluster in hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="6e3cc-156">Vytvořit cluster Service Fabric pomocí hello Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e3cc-156">Create a Service Fabric cluster using hello Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
* [<span data-ttu-id="6e3cc-157">Pochopení modelu aplikace Service Fabric hello</span><span class="sxs-lookup"><span data-stu-id="6e3cc-157">Understand hello Service Fabric application model</span></span>](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
