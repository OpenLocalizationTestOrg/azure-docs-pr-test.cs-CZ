---
title: "aaaService prostředků infrastruktury a nasazení kontejnerů v systému Linux | Microsoft Docs"
description: "Service Fabric a hello pomocí aplikace pro Linux kontejnery toodeploy mikroslužby. Tento článek popisuje hello možnosti, které Service Fabric nabízí pro kontejnery a jak toodeploy kontejner Linux bitové kopie do clusteru"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a><span data-ttu-id="3bf16-104">Nasazení Linux kontejneru tooService prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="3bf16-104">Deploy a Linux container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3bf16-105">Nasazení kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="3bf16-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="3bf16-106">Nasazení kontejneru Linux</span><span class="sxs-lookup"><span data-stu-id="3bf16-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="3bf16-107">Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Docker v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="3bf16-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="3bf16-108">Service Fabric má několik kontejneru funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb, která jsou kontejnerizované.</span><span class="sxs-lookup"><span data-stu-id="3bf16-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="3bf16-109">Tyto služby se nazývají kontejnerové služby.</span><span class="sxs-lookup"><span data-stu-id="3bf16-109">These services are called containerized services.</span></span>

<span data-ttu-id="3bf16-110">Hello schopnosti zahrnují;</span><span class="sxs-lookup"><span data-stu-id="3bf16-110">hello capabilities include;</span></span>

* <span data-ttu-id="3bf16-111">Nasazení bitové kopie kontejneru a aktivace</span><span class="sxs-lookup"><span data-stu-id="3bf16-111">Container image deployment and activation</span></span>
* <span data-ttu-id="3bf16-112">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="3bf16-112">Resource governance</span></span>
* <span data-ttu-id="3bf16-113">Úložiště ověřování</span><span class="sxs-lookup"><span data-stu-id="3bf16-113">Repository authentication</span></span>
* <span data-ttu-id="3bf16-114">Mapování portů toohost port kontejneru</span><span class="sxs-lookup"><span data-stu-id="3bf16-114">Container port toohost port mapping</span></span>
* <span data-ttu-id="3bf16-115">Zjišťování – kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="3bf16-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="3bf16-116">Možnost tooconfigure a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="3bf16-116">Ability tooconfigure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="3bf16-117">Balení kontejner docker s yeoman</span><span class="sxs-lookup"><span data-stu-id="3bf16-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="3bf16-118">Při balení kontejner v systému Linux, můžete zvolit buď toouse šablonu yeoman nebo [ručně vytvořit balíček aplikace hello](#manually).</span><span class="sxs-lookup"><span data-stu-id="3bf16-118">When packaging a container on Linux, you can choose either toouse a yeoman template or [create hello application package manually](#manually).</span></span>

<span data-ttu-id="3bf16-119">Aplikace Service Fabric může obsahovat jeden nebo více kontejnerů, každý s určitou roli při poskytování funkcí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="3bf16-120">zahrnuje Hello Service Fabric SDK pro Linux [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno toocreate vaší aplikace a přidat bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3bf16-120">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="3bf16-121">Umožňuje použít Yeoman toocreate názvem aplikace s jediný kontejner Docker *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="3bf16-121">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="3bf16-122">Další služby můžete přidat později úpravou hello generované manifestu souborů.</span><span class="sxs-lookup"><span data-stu-id="3bf16-122">You can add more services later by editing hello generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="3bf16-123">Nainstalujte na vaše pole vývoj Docker</span><span class="sxs-lookup"><span data-stu-id="3bf16-123">Install Docker on your development box</span></span>

<span data-ttu-id="3bf16-124">Hello spusťte následující příkazy tooinstall docker na vaše pole vývoj Linux (Pokud používáte hello vagrant image na OSX, docker je již nainstalována):</span><span class="sxs-lookup"><span data-stu-id="3bf16-124">Run hello following commands tooinstall docker on your Linux development box (if you are using hello vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a><span data-ttu-id="3bf16-125">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3bf16-125">Create hello application</span></span>
1. <span data-ttu-id="3bf16-126">V terminálu zadejte `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="3bf16-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="3bf16-127">Název aplikace - například mycontainerap</span><span class="sxs-lookup"><span data-stu-id="3bf16-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="3bf16-128">Zadejte adresu URL hello hello kontejneru bitové kopie z DockerHub úložišti.</span><span class="sxs-lookup"><span data-stu-id="3bf16-128">Provide hello URL for hello container image from a DockerHub repo.</span></span> <span data-ttu-id="3bf16-129">Hello image parametr trvá hello formuláře [úložišti] / [název image]</span><span class="sxs-lookup"><span data-stu-id="3bf16-129">hello image parameter takes hello form [repo]/[image name]</span></span>
4. <span data-ttu-id="3bf16-130">Pokud hello obrázek nemá zatížení vstupní bod definované, pak musíte tooexplicitly zadat vstupní příkazy sadu příkazů toorun uvnitř hello kontejneru, který uchová hello kontejneru spuštění po spuštění oddělených čárkou.</span><span class="sxs-lookup"><span data-stu-id="3bf16-130">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="deploy-hello-application"></a><span data-ttu-id="3bf16-132">Nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="3bf16-132">Deploy hello application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="3bf16-133">Použití XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="3bf16-133">Using XPlat CLI</span></span>
<span data-ttu-id="3bf16-134">Po hello aplikace, můžete ho nasadit toohello místní cluster pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="3bf16-134">Once hello application is built, you can deploy it toohello local cluster using hello Azure CLI.</span></span>

1. <span data-ttu-id="3bf16-135">Připojte toohello místní cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3bf16-135">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="3bf16-136">Použití hello nainstalujete skript zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-136">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="3bf16-137">Otevřete prohlížeč a přejděte tooService Fabric Explorer na http://localhost: 19080/Explorer (nahraďte localhost s privátní IP hello hello virtuálních počítačů, když Vagrant na Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="3bf16-137">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="3bf16-138">Rozbalte uzel aplikace hello a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="3bf16-138">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>
5. <span data-ttu-id="3bf16-139">Odinstalační skript pro použití hello k dispozici v instanci aplikace hello šablony toodelete hello a zrušení registrace typu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-139">Use hello uninstall script provided in hello template toodelete hello application instance and unregister hello application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="3bf16-140">Použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3bf16-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="3bf16-141">Na správu najdete v části hello referenční dokumentace [životní cyklus aplikace pomocí Azure CLI 2.0 hello](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="3bf16-141">See hello reference doc on managing an [application life cycle using hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="3bf16-142">Ukázkovou aplikaci [ukázky najdete v článku věnovaném hello kód kontejneru Service Fabric na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="3bf16-142">For an example application, [checkout hello Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="3bf16-143">Přidání další služby tooan existující aplikace</span><span class="sxs-lookup"><span data-stu-id="3bf16-143">Adding more services tooan existing application</span></span>

<span data-ttu-id="3bf16-144">tooadd jiný kontejner služby již vytvořené pomocí aplikace tooan `yo`, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3bf16-144">tooadd another container service tooan application already created using `yo`, perform hello following steps:</span></span>

1. <span data-ttu-id="3bf16-145">Změnit kořenový adresář toohello hello existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bf16-145">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="3bf16-146">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="3bf16-147">Spusťte `yo azuresfcontainer:AddService`.</span><span class="sxs-lookup"><span data-stu-id="3bf16-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="3bf16-148">Ručně zabalení a nasazení bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="3bf16-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="3bf16-149">proces Hello ručně balení kontejnerové služby je založena na hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3bf16-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="3bf16-150">Publikujte hello kontejnery tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="3bf16-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="3bf16-151">Vytvořte strukturu adresáře balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="3bf16-152">Upravte soubor manifestu služby hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="3bf16-153">Upravte soubor manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="3bf16-154">Nasazení a aktivaci bitovou kopii kontejneru</span><span class="sxs-lookup"><span data-stu-id="3bf16-154">Deploy and activate a container image</span></span>
<span data-ttu-id="3bf16-155">V hello Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky.</span><span class="sxs-lookup"><span data-stu-id="3bf16-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="3bf16-156">toodeploy a aktivujte kontejner a put hello název obrázku kontejneru hello do `ContainerHost` element v hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="3bf16-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="3bf16-157">V manifestu hello služby, přidat `ContainerHost` pro hello vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="3bf16-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="3bf16-158">Potom sadu hello `ImageName` toobe hello název hello kontejneru úložiště a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="3bf16-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="3bf16-159">Hello následující částečné manifest ukazuje příklad jak toodeploy hello kontejner nazývá `myimage:v1` z úložiště volána `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="3bf16-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

<span data-ttu-id="3bf16-160">Můžete zadat vstupní příkazy zadáním hello volitelné `Commands` element sadu příkazů toorun uvnitř kontejneru hello oddělených čárkou.</span><span class="sxs-lookup"><span data-stu-id="3bf16-160">You can provide input commands by specifying hello optional `Commands` element with a comma-delimited set of commands toorun inside hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="3bf16-161">Pokud hello obrázek nemá zatížení vstupní bod definované, pak musíte tooexplicitly zadat vstupní příkazy uvnitř `Commands` element sadu příkazů toorun uvnitř hello kontejneru, který uchová hello kontejneru spuštění po oddělený čárkami spuštění.</span><span class="sxs-lookup"><span data-stu-id="3bf16-161">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands inside `Commands` element with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="3bf16-162">Pochopení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="3bf16-162">Understand resource governance</span></span>
<span data-ttu-id="3bf16-163">Zásady správného řízení prostředků je na hostiteli hello můžete použít funkce hello kontejneru, který omezuje hello prostředky, které hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3bf16-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="3bf16-164">Hello `ResourceGovernancePolicy`, které je určené v manifestu aplikace hello je použité toodeclare prostředků limity pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="3bf16-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="3bf16-165">Omezení prostředků lze nastavit pro hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="3bf16-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="3bf16-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="3bf16-166">Memory</span></span>
* <span data-ttu-id="3bf16-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="3bf16-167">MemorySwap</span></span>
* <span data-ttu-id="3bf16-168">CpuShares (Relativní váha procesoru)</span><span class="sxs-lookup"><span data-stu-id="3bf16-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="3bf16-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="3bf16-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="3bf16-170">BlkioWeight (BlockIO relativní váhy).</span><span class="sxs-lookup"><span data-stu-id="3bf16-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="3bf16-171">V budoucí verzi, podporu pro konkrétní bloku limity vstupně-výstupní operace jako je například IOPs, zadání počtu bitů za Sekundu pro čtení a zápis a ostatní budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="3bf16-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
>
>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a><span data-ttu-id="3bf16-172">Ověření úložiště</span><span class="sxs-lookup"><span data-stu-id="3bf16-172">Authenticate a repository</span></span>
<span data-ttu-id="3bf16-173">toodownload kontejner, můžete mít tooprovide přihlašovací údaje toohello kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="3bf16-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="3bf16-174">Hello přihlašovací údaje, zadaný v manifestu aplikace hello, jsou použité toospecify hello přihlašovací údaje nebo klíč SSH pro stahování hello kontejneru image z úložiště imagí hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="3bf16-175">Hello následující příklad ukazuje účtu nazvaného *TestUser* společně s hello heslo jako prostý text (*není* doporučená):</span><span class="sxs-lookup"><span data-stu-id="3bf16-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

<span data-ttu-id="3bf16-176">Doporučujeme, abyste šifrování hesla hello pomocí certifikátu, která nasadila toohello počítače.</span><span class="sxs-lookup"><span data-stu-id="3bf16-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="3bf16-177">Hello následující příklad ukazuje účtu nazvaného *TestUser*, kde hello hesla byla zašifrována pomocí certifikát nazvaný *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="3bf16-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="3bf16-178">Můžete použít hello `Invoke-ServiceFabricEncryptText` prostředí PowerShell příkaz toocreate hello tajný šifrovaný text hello hesla.</span><span class="sxs-lookup"><span data-stu-id="3bf16-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="3bf16-179">Další informace najdete v článku hello [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="3bf16-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="3bf16-180">privátní klíč Hello hello certifikátu, který byl použit toodecrypt hello heslo musí být nasazené toohello v metodu out-of-band místního počítače.</span><span class="sxs-lookup"><span data-stu-id="3bf16-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="3bf16-181">(V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí počítač toohello balíček služby hello, ho dešifrovat tajný klíč hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="3bf16-182">Pomocí hello tajný klíč společně s názvem účtu hello můžete poté ověřit pomocí hello kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="3bf16-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="3bf16-183">Konfigurace mapování port kontejneru hostitele a portu</span><span class="sxs-lookup"><span data-stu-id="3bf16-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="3bf16-184">Můžete nakonfigurovat toocommunicate port používaný na hostiteli s hello kontejneru zadáním `PortBinding` v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="3bf16-185">Hello port vazby mapy hello port toowhich hello služba naslouchá uvnitř port tooa hello kontejneru na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="3bf16-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="3bf16-186">Konfigurace zjišťování kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="3bf16-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="3bf16-187">Pomocí hello `PortBinding` zásady, můžete namapovat port kontejneru tooan `Endpoint` v hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="3bf16-187">By using hello `PortBinding` policy, you can map a container port tooan `Endpoint` in hello service manifest.</span></span> <span data-ttu-id="3bf16-188">Hello koncový bod `Endpoint1` můžete zadat pevné číslo portu (například port 80).</span><span class="sxs-lookup"><span data-stu-id="3bf16-188">hello endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="3bf16-189">Toho může také specifikovat žádné port vůbec, v takovém případě je zvolen náhodných portu z rozsahu portů aplikace hello clusteru pro vás.</span><span class="sxs-lookup"><span data-stu-id="3bf16-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="3bf16-190">Pokud zadáte koncový bod, pomocí hello `Endpoint` značky v service manifest hello kontejneru hosta, Service Fabric můžete automaticky publikovat tento koncový bod toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="3bf16-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="3bf16-191">Dalším službám, které jsou spuštěny v clusteru hello může zjišťovat proto tento kontejner pomocí hello REST dotazů pro řešení.</span><span class="sxs-lookup"><span data-stu-id="3bf16-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

<span data-ttu-id="3bf16-192">Po registraci hello Naming service, snadno to komunikace kontejnery v hello kódu v rámci vašeho kontejneru pomocí hello [reverse proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="3bf16-192">By registering with hello Naming service, you can easily do container-to-container communication in hello code within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="3bf16-193">Komunikace se provádí zadáním naslouchající port http hello reverzní proxy server a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bf16-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="3bf16-194">Další informace najdete v tématu hello další části.</span><span class="sxs-lookup"><span data-stu-id="3bf16-194">For more information, see hello next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="3bf16-195">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="3bf16-195">Configure and set environment variables</span></span>
<span data-ttu-id="3bf16-196">Proměnné prostředí lze zadat pro každý balíček kódu v manifestu hello služby, jak pro služby, které jsou nasazeny v kontejnerech nebo pro služby, které jsou nasazeny jako spustitelné soubory procesy nebo hosta.</span><span class="sxs-lookup"><span data-stu-id="3bf16-196">Environment variables can be specified for each code package in hello service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="3bf16-197">Tyto hodnoty proměnné prostředí můžete přepsat konkrétně v manifestu aplikace hello nebo zadat během nasazování jako parametry aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bf16-197">These environment variable values can be overridden specifically in hello application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="3bf16-198">Hello následující služby manifestu XML fragment kódu ukazuje příklad toospecify proměnných prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="3bf16-198">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

<span data-ttu-id="3bf16-199">Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="3bf16-199">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="3bf16-200">V předchozím příkladu hello jsme zadali explicitní hodnotu pro hello `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu hello `BackendServiceName` parametr prostřednictvím hello `[BackendSvc]` parametr aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bf16-200">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="3bf16-201">Tato nastavení umožňují hodnotu hello toospecify `BackendServiceName`hodnota při nasazení aplikace hello a v manifestu hello nemá pevnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3bf16-201">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="3bf16-202">Dokončit příklady pro aplikace a manifest služby</span><span class="sxs-lookup"><span data-stu-id="3bf16-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="3bf16-203">Manifest aplikace například takto:</span><span class="sxs-lookup"><span data-stu-id="3bf16-203">An example application manifest follows:</span></span>

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

<span data-ttu-id="3bf16-204">Manifest službu příkladu (zadané v předchozím manifest aplikace hello) zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="3bf16-204">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a><span data-ttu-id="3bf16-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bf16-205">Next steps</span></span>
<span data-ttu-id="3bf16-206">Teď, když jste nasadili kontejnerové služby, zjistěte, jak toomanage životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="3bf16-206">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="3bf16-207">Přehled Service Fabric a kontejnery</span><span class="sxs-lookup"><span data-stu-id="3bf16-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="3bf16-208">Interakci s clusterů Service Fabric pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="3bf16-208">Interacting with Service Fabric clusters using hello Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="3bf16-209">Související články</span><span class="sxs-lookup"><span data-stu-id="3bf16-209">Related articles</span></span>

* [<span data-ttu-id="3bf16-210">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3bf16-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="3bf16-211">Začínáme se Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="3bf16-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
