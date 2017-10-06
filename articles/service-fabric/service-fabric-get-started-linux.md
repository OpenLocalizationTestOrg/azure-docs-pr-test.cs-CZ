---
title: "aaaSet vývojového prostředí v systému Linux | Microsoft Docs"
description: "Nainstalujte modul runtime hello a sady SDK a vytvoření místního vývojového clusteru v systému Linux. Po dokončení této instalace, bude připravená toobuild aplikace."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.openlocfilehash: 9d82c2015f9e2c6fb55f2052c7cdb1e906c5deeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment-on-linux"></a><span data-ttu-id="2a193-104">Příprava vývojového prostředí v Linuxu</span><span class="sxs-lookup"><span data-stu-id="2a193-104">Prepare your development environment on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a193-105">Windows</span><span class="sxs-lookup"><span data-stu-id="2a193-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="2a193-106">Linux</span><span class="sxs-lookup"><span data-stu-id="2a193-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="2a193-107">OSX</span><span class="sxs-lookup"><span data-stu-id="2a193-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="2a193-108">toodeploy a spusťte [aplikace Azure Service Fabric](service-fabric-application-model.md) na vývojovém počítači Linux, nainstalujte hello runtime a společné sady SDK.</span><span class="sxs-lookup"><span data-stu-id="2a193-108">toodeploy and run [Azure Service Fabric applications](service-fabric-application-model.md) on your Linux development machine, install hello runtime and common SDK.</span></span> <span data-ttu-id="2a193-109">Můžete také nainstalovat volitelné sady SDK pro Javu a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a193-109">You can also install optional SDKs for Java and .NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a193-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a193-110">Prerequisites</span></span>

<span data-ttu-id="2a193-111">pro vývoj jsou podporovány následující verze operačního systému Hello:</span><span class="sxs-lookup"><span data-stu-id="2a193-111">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="2a193-112">Ubuntu 16.04 (`Xenial Xerus`)</span><span class="sxs-lookup"><span data-stu-id="2a193-112">Ubuntu 16.04 (`Xenial Xerus`)</span></span>

## <a name="update-your-apt-sources"></a><span data-ttu-id="2a193-113">Aktualizace zdrojů APT</span><span class="sxs-lookup"><span data-stu-id="2a193-113">Update your APT sources</span></span>
<span data-ttu-id="2a193-114">tooinstall hello SDK a hello přidružené runtime balíčku pomocí nástroje příkazového řádku hello výstižný get, nejprve je nutné aktualizovat vaše zdroje byt pokročilé nástroje balení (č).</span><span class="sxs-lookup"><span data-stu-id="2a193-114">tooinstall hello SDK and hello associated runtime package via hello apt-get command-line tool, you must first update your Advanced Packaging Tool (APT) sources.</span></span>

1. <span data-ttu-id="2a193-115">Otevřete terminál.</span><span class="sxs-lookup"><span data-stu-id="2a193-115">Open a terminal.</span></span>
2. <span data-ttu-id="2a193-116">Přidejte hello Service Fabric úložišti tooyour zdroje seznamu.</span><span class="sxs-lookup"><span data-stu-id="2a193-116">Add hello Service Fabric repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. <span data-ttu-id="2a193-117">Přidat hello `dotnet` seznamu zdrojů tooyour úložišti.</span><span class="sxs-lookup"><span data-stu-id="2a193-117">Add hello `dotnet` repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. <span data-ttu-id="2a193-118">Přidat hello nové Gnu ochrana osobních údajů (GnuPG nebo GPG) klíče tooyour VÝSTIŽNÝ Správce klíčů.</span><span class="sxs-lookup"><span data-stu-id="2a193-118">Add hello new Gnu Privacy Guard (GnuPG, or GPG) key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. <span data-ttu-id="2a193-119">Přidejte hello oficiální Docker GPG klíče tooyour VÝSTIŽNÝ Správce klíčů.</span><span class="sxs-lookup"><span data-stu-id="2a193-119">Add hello official Docker GPG key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. <span data-ttu-id="2a193-120">Nastavení úložiště Docker hello.</span><span class="sxs-lookup"><span data-stu-id="2a193-120">Set up hello Docker repository.</span></span>

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. <span data-ttu-id="2a193-121">Aktualizace vašeho balíčku seznamy založené na hello nově přidány úložiště.</span><span class="sxs-lookup"><span data-stu-id="2a193-121">Refresh your package lists based on hello newly added repositories.</span></span>

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a><span data-ttu-id="2a193-122">Instalace a nastavení hello SDK pro nastavení místního clusteru</span><span class="sxs-lookup"><span data-stu-id="2a193-122">Install and set up hello SDK for local cluster setup</span></span>

<span data-ttu-id="2a193-123">Po dokončení aktualizace vašich zdrojů, můžete nainstalovat hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2a193-123">After you have updated your sources, you can install hello SDK.</span></span> <span data-ttu-id="2a193-124">Nainstalujte balíček Service Fabric SDK hello, potvrzení instalace hello a toohello licenční smlouvy souhlasím.</span><span class="sxs-lookup"><span data-stu-id="2a193-124">Install hello Service Fabric SDK package, confirm hello installation, and agree toohello license agreement.</span></span>

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   <span data-ttu-id="2a193-125">Hello následující příkazy automatizovat přijímá hello licenci pro Service Fabric balíčky:</span><span class="sxs-lookup"><span data-stu-id="2a193-125">hello following commands automate accepting hello license for Service Fabric packages:</span></span>
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a><span data-ttu-id="2a193-126">Nastavení místního clusteru</span><span class="sxs-lookup"><span data-stu-id="2a193-126">Set up a local cluster</span></span>
  <span data-ttu-id="2a193-127">Pokud hello instalace proběhne úspěšně, musí být schopný toostart místní cluster.</span><span class="sxs-lookup"><span data-stu-id="2a193-127">If hello installation is successful, you should be able toostart a local cluster.</span></span>

  1. <span data-ttu-id="2a193-128">Spusťte instalační skript clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="2a193-128">Run hello cluster setup script.</span></span>

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. <span data-ttu-id="2a193-129">Otevřete webový prohlížeč a přejděte příliš[Service Fabric Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="2a193-129">Open a web browser and go too[Service Fabric Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="2a193-130">Pokud byl zahájen hello clusteru, měli byste vidět řídicí panel hello Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="2a193-130">If hello cluster has started, you should see hello Service Fabric Explorer dashboard.</span></span>

      ![Service Fabric Explorer v Linuxu][sfx-linux]

  <span data-ttu-id="2a193-132">V tuto chvíli můžete nasadit předem sestavené balíčky aplikací Service Fabric nebo nové balíčky založené na kontejnerech nebo spustitelných souborech hostů.</span><span class="sxs-lookup"><span data-stu-id="2a193-132">At this point, you can deploy pre-built Service Fabric application packages or new ones based on guest containers or guest executables.</span></span> <span data-ttu-id="2a193-133">toobuild nové služby pomocí hello Java nebo .NET Core SDK, postupujte podle kroků hello volitelné nastavení, které jsou uvedené v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="2a193-133">toobuild new services by using hello Java or .NET Core SDKs, follow hello optional setup steps that are provided in subsequent sections.</span></span>


  > [!NOTE]
  > <span data-ttu-id="2a193-134">Samostatné clustery nejsou podporované v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2a193-134">Standalone clusters aren't supported in Linux.</span></span> <span data-ttu-id="2a193-135">Hello preview podporuje pouze jeden pole a clustery pro víc počítačů Azure Linux.</span><span class="sxs-lookup"><span data-stu-id="2a193-135">hello preview supports only one-box and Azure Linux multi-machine clusters.</span></span>
  >

## <a name="set-up-hello-service-fabric-cli"></a><span data-ttu-id="2a193-136">Nastavit hello Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2a193-136">Set up hello Service Fabric CLI</span></span>

<span data-ttu-id="2a193-137">Hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md) obsahuje příkazy pro interakci s entitami Service Fabric, včetně clusterů a aplikací.</span><span class="sxs-lookup"><span data-stu-id="2a193-137">hello [Service Fabric CLI](service-fabric-cli.md) has commands for interacting with Service Fabric entities, including clusters and applications.</span></span> <span data-ttu-id="2a193-138">Je založen na python, takže se toohave python a pip nainstalovat předtím, než budete pokračovat s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2a193-138">It is based on python, so be sure toohave python and pip installed before you proceed with hello following command:</span></span>

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a><span data-ttu-id="2a193-139">Instalace a nastavení hello generátory pro kontejnery a Host-spustitelné soubory</span><span class="sxs-lookup"><span data-stu-id="2a193-139">Install and set up hello generators for containers and guest-executables</span></span>
<span data-ttu-id="2a193-140">Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikace Service Fabric z terminálu pomocí generátoru šablon Yeoman.</span><span class="sxs-lookup"><span data-stu-id="2a193-140">Service Fabric provides scaffolding tools which will help you create a Service Fabric applications from terminal using Yeoman template generator.</span></span> <span data-ttu-id="2a193-141">Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro práci na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="2a193-141">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for working on your machine.</span></span>

1. <span data-ttu-id="2a193-142">Instalace nodejs a NPM na počítači</span><span class="sxs-lookup"><span data-stu-id="2a193-142">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="2a193-143">Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM</span><span class="sxs-lookup"><span data-stu-id="2a193-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="2a193-144">Instalace generátor kontejneru hello jo s háčkem nad Service Fabric a hosta execuatble generátor z NPM</span><span class="sxs-lookup"><span data-stu-id="2a193-144">Install hello Service Fabric Yeo container generator and guest execuatble generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

<span data-ttu-id="2a193-145">Po instalaci hello výše generátory, měli byste být aplikací mít toocreate s spustitelný soubor nebo kontejneru služby hosta spuštěním `yo azuresfguest` nebo `yo azuresfcontainer` v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2a193-145">After you have installed hello above generators, you should be able toocreate apps with guest executable or container services by running `yo azuresfguest` or `yo azuresfcontainer` respectively.</span></span>

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a><span data-ttu-id="2a193-146">Nainstalujte hello nezbytné Java artefakty (volitelné tam, pokud chcete toouse hello Java programovací modely)</span><span class="sxs-lookup"><span data-stu-id="2a193-146">Install hello necessary Java artifacts (optional, if you want toouse hello Java programming models)</span></span>

<span data-ttu-id="2a193-147">toobuild služeb Service Fabric pomocí Java, ujistěte se, že máte 1.8 JDK, které jsou nainstalovány společně s Gradle, který se používá pro spuštění úlohy sestavení.</span><span class="sxs-lookup"><span data-stu-id="2a193-147">toobuild Service Fabric services using Java, ensure you have JDK 1.8 installed along with Gradle which is used for running build tasks.</span></span> <span data-ttu-id="2a193-148">Následující fragment kódu Hello nainstaluje otevřete 1.8 JDK společně s Gradle.</span><span class="sxs-lookup"><span data-stu-id="2a193-148">hello following snippet installs Open JDK 1.8 along with Gradle.</span></span> <span data-ttu-id="2a193-149">knihovny Service Fabric Java Hello jsou vyžádány od Maven.</span><span class="sxs-lookup"><span data-stu-id="2a193-149">hello Service Fabric Java libraries are pulled from Maven.</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a><span data-ttu-id="2a193-150">Nainstalujte hello Eclipse Neónová modulu plug-in (volitelné)</span><span class="sxs-lookup"><span data-stu-id="2a193-150">Install hello Eclipse Neon plug-in (optional)</span></span>

<span data-ttu-id="2a193-151">Hello Eclipse modulu plug-in pro Service Fabric z můžete nainstalovat v rámci hello **IDE Eclipse pro vývojáře v jazyce Java**.</span><span class="sxs-lookup"><span data-stu-id="2a193-151">You can install hello Eclipse plug-in for Service Fabric from within hello **Eclipse IDE for Java Developers**.</span></span> <span data-ttu-id="2a193-152">V aplikacích Fabric Java tooService přidání můžete použít Eclipse toocreate Service Fabric Host spustitelný soubor aplikace a aplikace typu kontejner.</span><span class="sxs-lookup"><span data-stu-id="2a193-152">You can use Eclipse toocreate Service Fabric guest executable applications and container applications in addition tooService Fabric Java applications.</span></span>

1. <span data-ttu-id="2a193-153">V prostředí Eclipse, ujistěte se, že máte nejnovější Neónová Eclipse a hello nejnovější verzi Buildship (1.0.17 nebo novější) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="2a193-153">In Eclipse, ensure that you have latest Eclipse Neon and hello latest Buildship version (1.0.17 or later) installed.</span></span> <span data-ttu-id="2a193-154">Hello verze nainstalované součásti můžete zkontrolovat výběrem **pomoci** > **podrobné informace o instalaci**.</span><span class="sxs-lookup"><span data-stu-id="2a193-154">You can check hello versions of installed components by selecting **Help** > **Installation Details**.</span></span> <span data-ttu-id="2a193-155">Buildship můžete aktualizovat pomocí pokynů hello [Eclipse Buildship: Eclipse moduly plug-in pro Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="2a193-155">You can update Buildship by using hello instructions at [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>

2. <span data-ttu-id="2a193-156">Vyberte modul plug-in, Service Fabric hello tooinstall **pomoci** > **instalace nového softwaru**.</span><span class="sxs-lookup"><span data-stu-id="2a193-156">tooinstall hello Service Fabric plug-in, select **Help** > **Install New Software**.</span></span>

3. <span data-ttu-id="2a193-157">V hello **pracovat s** zadejte **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="2a193-157">In hello **Work with** box, type **http://dl.microsoft.com/eclipse**.</span></span>

4. <span data-ttu-id="2a193-158">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="2a193-158">Click **Add**.</span></span>

    ![stránku Hello dostupného softwaru][sf-eclipse-plugin]

5. <span data-ttu-id="2a193-160">Vyberte hello **ServiceFabric** modul plug-in a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2a193-160">Select hello **ServiceFabric** plug-in, and then click **Next**.</span></span>

6. <span data-ttu-id="2a193-161">Dokončete kroky instalace hello a přijměte licenční smlouvu hello.</span><span class="sxs-lookup"><span data-stu-id="2a193-161">Complete hello installation steps, and then accept hello end-user license agreement.</span></span>

<span data-ttu-id="2a193-162">Pokud již máte hello Service Fabric prostředí Eclipse modulu plug-in nainstalována, ujistěte se, že máte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="2a193-162">If you already have hello Service Fabric Eclipse plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="2a193-163">Můžete zkontrolovat výběrem **pomoci** > **podrobné informace o instalaci** a pak hledání Service Fabric hello seznamu nainstalovaných modulů plug-in. Pokud je k dispozici novější verze, vyberte **Update** (Aktualizovat).</span><span class="sxs-lookup"><span data-stu-id="2a193-163">You can check by selecting **Help** > **Installation Details** and then searching for Service Fabric in hello list of installed plug-ins. If a newer version is available, select **Update**.</span></span>

<span data-ttu-id="2a193-164">Další informace najdete v tématu [Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="2a193-164">For more information, see [Service Fabric plug-in for Eclipse Java application development](service-fabric-get-started-eclipse.md).</span></span>


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a><span data-ttu-id="2a193-165">Nainstalujte hello .NET Core SDK (volitelné tam, pokud chcete, aby toouse hello .NET Core programovací modely)</span><span class="sxs-lookup"><span data-stu-id="2a193-165">Install hello .NET Core SDK (optional, if you want toouse hello .NET Core programming models)</span></span>
<span data-ttu-id="2a193-166">Hello .NET Core SDK poskytuje šablony, které jsou požadované toobuild Service Fabric služby s .NET Core a hello knihovny.</span><span class="sxs-lookup"><span data-stu-id="2a193-166">hello .NET Core SDK provides hello libraries and templates that are required toobuild Service Fabric services with .NET Core.</span></span> <span data-ttu-id="2a193-167">Instalovat balíček .NET Core SDK hello ve spuštěné následující hello-</span><span class="sxs-lookup"><span data-stu-id="2a193-167">Install hello .NET Core SDK package by running hello following -</span></span>

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a><span data-ttu-id="2a193-168">Aktualizace hello SDK a prostředí runtime</span><span class="sxs-lookup"><span data-stu-id="2a193-168">Update hello SDK and runtime</span></span>

<span data-ttu-id="2a193-169">tooupdate toohello nejnovější verzi hello SDK a modul runtime, spusťte následující příkazy hello (hello sady SDK, které nechcete použít, zrušte výběr):</span><span class="sxs-lookup"><span data-stu-id="2a193-169">tooupdate toohello latest version of hello SDK and runtime, run hello following commands (deselect hello SDKs that you don't want):</span></span>

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
<span data-ttu-id="2a193-170">tooupdate binární soubory hello sady Java SDK z Maven, je nutné tooupdate hello verze podrobnosti odpovídající binární hello v hello ``build.gradle`` soubor toopoint toohello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="2a193-170">tooupdate hello Java SDK binaries from Maven, you need tooupdate hello version details of hello corresponding binary in hello ``build.gradle`` file toopoint toohello latest version.</span></span> <span data-ttu-id="2a193-171">tooknow přesně, kde potřebujete tooupdate hello verzi, naleznete v části tooany ``build.gradle`` souboru v Service Fabric Začínáme ukázky [zde](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="2a193-171">tooknow exactly where you need tooupdate hello version, you can refer tooany ``build.gradle`` file in Service Fabric getting-started samples [here](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="2a193-172">Aktualizaci balíčků hello může způsobit vaše místního vývojového clusteru toostop systémem.</span><span class="sxs-lookup"><span data-stu-id="2a193-172">Updating hello packages might cause your local development cluster toostop running.</span></span> <span data-ttu-id="2a193-173">Restartujte místním clusteru po upgradu podle pokynů hello na této stránce.</span><span class="sxs-lookup"><span data-stu-id="2a193-173">Restart your local cluster after an upgrade by following hello instructions on this page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a193-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a193-174">Next steps</span></span>

* [<span data-ttu-id="2a193-175">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana</span><span class="sxs-lookup"><span data-stu-id="2a193-175">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="2a193-176">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="2a193-176">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="2a193-177">Vytvoření první aplikace v CSharp v Linuxu</span><span class="sxs-lookup"><span data-stu-id="2a193-177">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="2a193-178">Příprava vývojového prostředí v OSX</span><span class="sxs-lookup"><span data-stu-id="2a193-178">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="2a193-179">Pomocí aplikace hello toomanage Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="2a193-179">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="2a193-180">Rozdíly Service Fabric pro Windows a Linux</span><span class="sxs-lookup"><span data-stu-id="2a193-180">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
* [<span data-ttu-id="2a193-181">Začínáme s rozhraním příkazového řádku Service Fabric</span><span class="sxs-lookup"><span data-stu-id="2a193-181">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
