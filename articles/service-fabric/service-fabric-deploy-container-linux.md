---
title: "Služba Fabric a nasazení kontejnerů v systému Linux | Microsoft Docs"
description: "Service Fabric a použití kontejnery Linux k nasazení aplikací mikroslužby. Tento článek popisuje možnosti, které poskytuje služby infrastruktury pro kontejnery a nasazení bitové kopie kontejneru Linux do clusteru"
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
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a><span data-ttu-id="235a4-104">Nasadit kontejner Linux do Service Fabric</span><span class="sxs-lookup"><span data-stu-id="235a4-104">Deploy a Linux container to Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="235a4-105">Nasazení kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="235a4-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="235a4-106">Nasazení kontejneru Linux</span><span class="sxs-lookup"><span data-stu-id="235a4-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="235a4-107">Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Docker v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="235a4-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="235a4-108">Service Fabric má několik kontejneru funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb, která jsou kontejnerizované.</span><span class="sxs-lookup"><span data-stu-id="235a4-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="235a4-109">Tyto služby se nazývají kontejnerové služby.</span><span class="sxs-lookup"><span data-stu-id="235a4-109">These services are called containerized services.</span></span>

<span data-ttu-id="235a4-110">Schopnosti zahrnují;</span><span class="sxs-lookup"><span data-stu-id="235a4-110">The capabilities include;</span></span>

* <span data-ttu-id="235a4-111">Nasazení bitové kopie kontejneru a aktivace</span><span class="sxs-lookup"><span data-stu-id="235a4-111">Container image deployment and activation</span></span>
* <span data-ttu-id="235a4-112">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="235a4-112">Resource governance</span></span>
* <span data-ttu-id="235a4-113">Úložiště ověřování</span><span class="sxs-lookup"><span data-stu-id="235a4-113">Repository authentication</span></span>
* <span data-ttu-id="235a4-114">Port kontejneru na mapování portů hostitele</span><span class="sxs-lookup"><span data-stu-id="235a4-114">Container port to host port mapping</span></span>
* <span data-ttu-id="235a4-115">Zjišťování – kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="235a4-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="235a4-116">Možnost konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="235a4-116">Ability to configure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="235a4-117">Balení kontejner docker s yeoman</span><span class="sxs-lookup"><span data-stu-id="235a4-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="235a4-118">Při balení kontejner v systému Linux, můžete buď používat šablony yeoman nebo [ručně vytvořit balíček aplikace](#manually).</span><span class="sxs-lookup"><span data-stu-id="235a4-118">When packaging a container on Linux, you can choose either to use a yeoman template or [create the application package manually](#manually).</span></span>

<span data-ttu-id="235a4-119">Aplikace Service Fabric může obsahovat jeden nebo více kontejnerů, každý s určitou roli při poskytování funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="235a4-120">Sada Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který usnadňuje vytvoření aplikace a přidání image kontejneru.</span><span class="sxs-lookup"><span data-stu-id="235a4-120">The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image.</span></span> <span data-ttu-id="235a4-121">Použijme Yeomana k vytvoření aplikace *SimpleContainerApp* s jediným kontejnerem Dockeru.</span><span class="sxs-lookup"><span data-stu-id="235a4-121">Let's use Yeoman to create an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="235a4-122">Můžete přidat že další služby později úpravou vygenerovaného manifest soubory.</span><span class="sxs-lookup"><span data-stu-id="235a4-122">You can add more services later by editing the generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="235a4-123">Nainstalujte na vaše pole vývoj Docker</span><span class="sxs-lookup"><span data-stu-id="235a4-123">Install Docker on your development box</span></span>

<span data-ttu-id="235a4-124">Spusťte následující příkazy instalace docker na vaše pole vývoj Linux (Pokud používáte vagrant bitové kopie na OSX, docker je již nainstalována):</span><span class="sxs-lookup"><span data-stu-id="235a4-124">Run the following commands to install docker on your Linux development box (if you are using the vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a><span data-ttu-id="235a4-125">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="235a4-125">Create the application</span></span>
1. <span data-ttu-id="235a4-126">V terminálu zadejte `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="235a4-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="235a4-127">Název aplikace - například mycontainerap</span><span class="sxs-lookup"><span data-stu-id="235a4-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="235a4-128">Zadejte adresu URL pro kontejner bitovou kopii z DockerHub úložišti.</span><span class="sxs-lookup"><span data-stu-id="235a4-128">Provide the URL for the container image from a DockerHub repo.</span></span> <span data-ttu-id="235a4-129">Parametr image má podobu [úložišti] / [název image]</span><span class="sxs-lookup"><span data-stu-id="235a4-129">The image parameter takes the form [repo]/[image name]</span></span>
4. <span data-ttu-id="235a4-130">Pokud bitovou kopii nemá zatížení vstupní bod definované, pak je třeba explicitně zadat vstupní příkazy s oddělovači sadu příkazů pro spuštění uvnitř kontejneru, který uchová kontejneru spuštění po spuštění.</span><span class="sxs-lookup"><span data-stu-id="235a4-130">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="deploy-the-application"></a><span data-ttu-id="235a4-132">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="235a4-132">Deploy the application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="235a4-133">Použití XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="235a4-133">Using XPlat CLI</span></span>
<span data-ttu-id="235a4-134">Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="235a4-134">Once the application is built, you can deploy it to the local cluster using the Azure CLI.</span></span>

1. <span data-ttu-id="235a4-135">Připojte se k místnímu clusteru služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="235a4-135">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="235a4-136">Pomocí instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.</span><span class="sxs-lookup"><span data-stu-id="235a4-136">Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="235a4-137">Otevřete prohlížeč a přejdete k Service Fabric Exploreru na adrese http://localhost:19080/Explorer (pokud používáte Vagrant v Mac OS X, místo localhost použijte privátní IP adresu virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="235a4-137">Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="235a4-138">Rozbalte uzel Aplikace a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="235a4-138">Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>
5. <span data-ttu-id="235a4-139">Odinstalační skript pro zadané v šabloně použijte k odstranění instance aplikace a zrušení registrace typu aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-139">Use the uninstall script provided in the template to delete the application instance and unregister the application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="235a4-140">Použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="235a4-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="235a4-141">Na správu v tématu referenční dokumentace [životního cyklu aplikace pomocí Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="235a4-141">See the reference doc on managing an [application life cycle using the Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="235a4-142">Ukázkovou aplikaci [ukázky najdete v článku věnovaném kód kontejneru Service Fabric na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="235a4-142">For an example application, [checkout the Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="235a4-143">Přidání více služeb do stávající aplikace</span><span class="sxs-lookup"><span data-stu-id="235a4-143">Adding more services to an existing application</span></span>

<span data-ttu-id="235a4-144">Pro přidání do aplikace již vytvořené pomocí jiné služby kontejneru `yo`, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="235a4-144">To add another container service to an application already created using `yo`, perform the following steps:</span></span>

1. <span data-ttu-id="235a4-145">Změňte adresář na kořenovou složku stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-145">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="235a4-146">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.</span><span class="sxs-lookup"><span data-stu-id="235a4-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="235a4-147">Spusťte `yo azuresfcontainer:AddService`.</span><span class="sxs-lookup"><span data-stu-id="235a4-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="235a4-148">Ručně zabalení a nasazení bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="235a4-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="235a4-149">Proces ručně balení kontejnerové služby podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="235a4-149">The process of manually packaging a containerized service is based on the following steps:</span></span>

1. <span data-ttu-id="235a4-150">Kontejnery publikujte do úložiště.</span><span class="sxs-lookup"><span data-stu-id="235a4-150">Publish the containers to your repository.</span></span>
2. <span data-ttu-id="235a4-151">Vytvořte strukturu adresáře balíčku.</span><span class="sxs-lookup"><span data-stu-id="235a4-151">Create the package directory structure.</span></span>
3. <span data-ttu-id="235a4-152">Upravte soubor manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="235a4-152">Edit the service manifest file.</span></span>
4. <span data-ttu-id="235a4-153">Upravte soubor manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-153">Edit the application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="235a4-154">Nasazení a aktivaci bitovou kopii kontejneru</span><span class="sxs-lookup"><span data-stu-id="235a4-154">Deploy and activate a container image</span></span>
<span data-ttu-id="235a4-155">V Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky.</span><span class="sxs-lookup"><span data-stu-id="235a4-155">In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="235a4-156">K nasazení a aktivaci kontejner, uveďte název kontejneru bitové kopie do `ContainerHost` element v service manifest.</span><span class="sxs-lookup"><span data-stu-id="235a4-156">To deploy and activate a container, put the name of the container image into a `ContainerHost` element in the service manifest.</span></span>

<span data-ttu-id="235a4-157">V manifestu služby, přidat `ContainerHost` pro vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="235a4-157">In the service manifest, add a `ContainerHost` for the entry point.</span></span> <span data-ttu-id="235a4-158">Nastavte `ImageName` jako název kontejneru úložiště a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="235a4-158">Then set the `ImageName` to be the name of the container repository and image.</span></span> <span data-ttu-id="235a4-159">Následující částečné manifest ukazuje příklad nasazení kontejneru názvem `myimage:v1` z úložiště volána `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="235a4-159">The following partial manifest shows an example of how to deploy the container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="235a4-160">Můžete zadat vstupní příkazy zadáním volitelného `Commands` element s oddělovači sadu příkazů pro spuštění uvnitř kontejneru.</span><span class="sxs-lookup"><span data-stu-id="235a4-160">You can provide input commands by specifying the optional `Commands` element with a comma-delimited set of commands to run inside the container.</span></span>

> [!NOTE]
> <span data-ttu-id="235a4-161">Pokud bitovou kopii nemá zatížení vstupní bod definované, pak je třeba explicitně zadat vstupní příkazy uvnitř `Commands` element s oddělovači sadu příkazů pro spuštění uvnitř kontejneru, který uchová kontejneru spuštění po spuštění.</span><span class="sxs-lookup"><span data-stu-id="235a4-161">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands inside `Commands` element with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="235a4-162">Pochopení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="235a4-162">Understand resource governance</span></span>
<span data-ttu-id="235a4-163">Zásady správného řízení prostředků je funkce kontejneru, který omezuje prostředky, které můžete použít kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="235a4-163">Resource governance is a capability of the container that restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="235a4-164">`ResourceGovernancePolicy`, Které je určené v manifestu aplikace se používá k deklaraci limitů prostředků pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="235a4-164">The `ResourceGovernancePolicy`, which is specified in the application manifest is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="235a4-165">Omezení prostředků můžete nastavit pro následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="235a4-165">Resource limits can be set for the following resources:</span></span>

* <span data-ttu-id="235a4-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="235a4-166">Memory</span></span>
* <span data-ttu-id="235a4-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="235a4-167">MemorySwap</span></span>
* <span data-ttu-id="235a4-168">CpuShares (Relativní váha procesoru)</span><span class="sxs-lookup"><span data-stu-id="235a4-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="235a4-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="235a4-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="235a4-170">BlkioWeight (BlockIO relativní váhy).</span><span class="sxs-lookup"><span data-stu-id="235a4-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="235a4-171">V budoucí verzi, podporu pro konkrétní bloku limity vstupně-výstupní operace jako je například IOPs, zadání počtu bitů za Sekundu pro čtení a zápis a ostatní budou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="235a4-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="235a4-172">Ověření úložiště</span><span class="sxs-lookup"><span data-stu-id="235a4-172">Authenticate a repository</span></span>
<span data-ttu-id="235a4-173">Pokud chcete stáhnout kontejner, možná muset zadat přihlašovací údaje k úložišti kontejneru.</span><span class="sxs-lookup"><span data-stu-id="235a4-173">To download a container, you might have to provide sign-in credentials to the container repository.</span></span> <span data-ttu-id="235a4-174">Přihlašovací údaje, zadaný v manifestu aplikace, se používají a zadejte přihlašovací údaje, nebo klíč SSH pro stažení image kontejneru z úložiště imagí.</span><span class="sxs-lookup"><span data-stu-id="235a4-174">The sign-in credentials, specified in the application manifest, are used to specify the sign-in information, or SSH key, for downloading the container image from the image repository.</span></span> <span data-ttu-id="235a4-175">Následující příklad ukazuje účtu nazvaného *TestUser* společně s heslo jako prostý text (*není* doporučená):</span><span class="sxs-lookup"><span data-stu-id="235a4-175">The following example shows an account called *TestUser* along with the password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="235a4-176">Doporučujeme, abyste heslo šifrovat pomocí certifikátu, který je nasazen do počítače.</span><span class="sxs-lookup"><span data-stu-id="235a4-176">We recommend that you encrypt the password by using a certificate that's deployed to the machine.</span></span>

<span data-ttu-id="235a4-177">Následující příklad ukazuje účtu nazvaného *TestUser*, kde byla zašifrována heslem pomocí certifikát nazvaný *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="235a4-177">The following example shows an account called *TestUser*, where the password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="235a4-178">Můžete použít `Invoke-ServiceFabricEncryptText` příkaz prostředí PowerShell k vytvoření tajný šifrovaného textu pro heslo.</span><span class="sxs-lookup"><span data-stu-id="235a4-178">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text for the password.</span></span> <span data-ttu-id="235a4-179">Další informace najdete v článku [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="235a4-179">For more information, see the article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="235a4-180">Privátní klíč certifikátu, který se používá k dešifrování hesla musí být nasazený v metodu out-of-band v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="235a4-180">The private key of the certificate that's used to decrypt the password must be deployed to the local machine in an out-of-band method.</span></span> <span data-ttu-id="235a4-181">(V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí balíček služby k počítači, může dešifrovat tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="235a4-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys the service package to the machine, it can decrypt the secret.</span></span> <span data-ttu-id="235a4-182">Pomocí tajný klíč s názvem účtu může pak ověřit s úložištěm kontejneru.</span><span class="sxs-lookup"><span data-stu-id="235a4-182">By using the secret along with the account name, it can then authenticate with the container repository.</span></span>

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

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="235a4-183">Konfigurace mapování port kontejneru hostitele a portu</span><span class="sxs-lookup"><span data-stu-id="235a4-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="235a4-184">Můžete nakonfigurovat port hostitele používá ke komunikaci s kontejneru zadáním `PortBinding` v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-184">You can configure a host port used to communicate with the container by specifying a `PortBinding` in the application manifest.</span></span> <span data-ttu-id="235a4-185">Vazbou portu mapuje port, na kterém služba naslouchá uvnitř kontejneru na port na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="235a4-185">The port binding maps the port to which the service is listening inside the container to a port on the host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="235a4-186">Konfigurace zjišťování kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="235a4-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="235a4-187">Pomocí `PortBinding` zásady, můžete namapovat port kontejneru na `Endpoint` v service manifest.</span><span class="sxs-lookup"><span data-stu-id="235a4-187">By using the `PortBinding` policy, you can map a container port to an `Endpoint` in the service manifest.</span></span> <span data-ttu-id="235a4-188">Koncový bod `Endpoint1` můžete zadat pevné číslo portu (například port 80).</span><span class="sxs-lookup"><span data-stu-id="235a4-188">The endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="235a4-189">Ho můžete také zadat žádné port vůbec, v takovém případě je pro vás zvolen náhodných portu z rozsahu portů aplikace clusteru.</span><span class="sxs-lookup"><span data-stu-id="235a4-189">It can also specify no port at all, in which case a random port from the cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="235a4-190">Pokud zadáte koncový bod, pomocí `Endpoint` značky v service manifest kontejner hosta, Service Fabric můžete automaticky publikovat tento koncový bod služby pojmenování.</span><span class="sxs-lookup"><span data-stu-id="235a4-190">If you specify an endpoint, using the `Endpoint` tag in the service manifest of a guest container, Service Fabric can automatically publish this endpoint to the Naming service.</span></span> <span data-ttu-id="235a4-191">Dalším službám, které jsou spuštěny v clusteru může zjišťovat proto tento kontejner pomocí dotazů REST pro řešení.</span><span class="sxs-lookup"><span data-stu-id="235a4-191">Other services that are running in the cluster can thus discover this container using the REST queries for resolving.</span></span>

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

<span data-ttu-id="235a4-192">Když si zaregistrujete ve službě pojmenování, snadno to komunikace kontejnery v kódu v rámci vašeho kontejneru pomocí [reverzní proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="235a4-192">By registering with the Naming service, you can easily do container-to-container communication in the code within your container by using the [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="235a4-193">Komunikace se provádí zadáním naslouchající port http reverzní proxy server a název služby, které chcete ke komunikaci s jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="235a4-193">Communication is performed by providing the reverse proxy http listening port and the name of the services that you want to communicate with as environment variables.</span></span> <span data-ttu-id="235a4-194">Další informace najdete v další části.</span><span class="sxs-lookup"><span data-stu-id="235a4-194">For more information, see the next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="235a4-195">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="235a4-195">Configure and set environment variables</span></span>
<span data-ttu-id="235a4-196">Proměnné prostředí lze zadat pro každý balíček kódu v service manifest, jak pro služby, které jsou nasazeny v kontejnerech nebo pro služby, které jsou nasazeny jako spustitelné soubory procesy nebo hosta.</span><span class="sxs-lookup"><span data-stu-id="235a4-196">Environment variables can be specified for each code package in the service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="235a4-197">Tyto hodnoty proměnné prostředí můžete přepsat konkrétně v manifestu aplikace nebo zadávají během nasazení jako parametry aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-197">These environment variable values can be overridden specifically in the application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="235a4-198">Následující fragment kódu XML manifestu služby ukazuje příklad toho, jak zadat proměnné prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="235a4-198">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>

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

<span data-ttu-id="235a4-199">Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace:</span><span class="sxs-lookup"><span data-stu-id="235a4-199">These environment variables can be overridden at the application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="235a4-200">V předchozím příkladu jsme zadali explicitní hodnotu pro `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu pro `BackendServiceName` parametr prostřednictvím `[BackendSvc]` parametr aplikace.</span><span class="sxs-lookup"><span data-stu-id="235a4-200">In the previous example, we specified an explicit value for the `HttpGateway` environment variable (19000), while we set the value for `BackendServiceName` parameter via the `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="235a4-201">Tato nastavení umožňují zadat hodnotu pro `BackendServiceName`hodnota při nasazení aplikace a nemá pevnou hodnotu v manifestu.</span><span class="sxs-lookup"><span data-stu-id="235a4-201">These settings enable you to specify the value for `BackendServiceName`value when you deploy the application and not have a fixed value in the manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="235a4-202">Dokončit příklady pro aplikace a manifest služby</span><span class="sxs-lookup"><span data-stu-id="235a4-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="235a4-203">Manifest aplikace například takto:</span><span class="sxs-lookup"><span data-stu-id="235a4-203">An example application manifest follows:</span></span>

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

<span data-ttu-id="235a4-204">Následuje manifest službu příkladu (zadané v předchozím manifest aplikace):</span><span class="sxs-lookup"><span data-stu-id="235a4-204">An example service manifest (specified in the preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="235a4-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="235a4-205">Next steps</span></span>
<span data-ttu-id="235a4-206">Teď, když jste nasadili kontejnerové služby, zjistěte, jak Správa životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="235a4-206">Now that you have deployed a containerized service, learn how to manage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="235a4-207">Přehled Service Fabric a kontejnery</span><span class="sxs-lookup"><span data-stu-id="235a4-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="235a4-208">Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="235a4-208">Interacting with Service Fabric clusters using the Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="235a4-209">Související články</span><span class="sxs-lookup"><span data-stu-id="235a4-209">Related articles</span></span>

* [<span data-ttu-id="235a4-210">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="235a4-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="235a4-211">Začínáme se Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="235a4-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
