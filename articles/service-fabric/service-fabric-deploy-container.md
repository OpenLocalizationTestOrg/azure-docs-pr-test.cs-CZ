---
title: "Service Fabric a nasazení kontejnerů | Microsoft Docs"
description: "Service Fabric a používání kontejnerů k nasazení aplikací mikroslužby. Tento článek popisuje možnosti, které poskytuje služby infrastruktury pro kontejnery a nasazení bitové kopie kontejneru systému Windows do clusteru."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 25d6b056421e71fa70ed20a39589f77dbbc25c69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-windows-container-to-service-fabric"></a><span data-ttu-id="4dfbb-104">Nasazení kontejneru systému Windows pro Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4dfbb-104">Deploy a Windows container to Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4dfbb-105">Nasazení kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="4dfbb-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="4dfbb-106">Nasadit kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="4dfbb-106">Deploy Docker Container</span></span>](service-fabric-deploy-container-linux.md)
> 
> 

<span data-ttu-id="4dfbb-107">Tento článek vás provede procesem vytváření kontejnerizované služeb v systému Windows kontejnerech.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-107">This article walks you through the process of building containerized services in Windows containers.</span></span>

<span data-ttu-id="4dfbb-108">Service Fabric má několik funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb běžících v rámci kontejnery.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-108">Service Fabric has several capabilities that help you with building applications that are composed of microservices running inside containers.</span></span> 

<span data-ttu-id="4dfbb-109">Schopnosti zahrnují:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-109">The capabilities include:</span></span>

* <span data-ttu-id="4dfbb-110">Nasazení bitové kopie kontejneru a aktivace</span><span class="sxs-lookup"><span data-stu-id="4dfbb-110">Container image deployment and activation</span></span>
* <span data-ttu-id="4dfbb-111">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="4dfbb-111">Resource governance</span></span>
* <span data-ttu-id="4dfbb-112">Úložiště ověřování</span><span class="sxs-lookup"><span data-stu-id="4dfbb-112">Repository authentication</span></span>
* <span data-ttu-id="4dfbb-113">Mapování port kontejneru hostitele a portu</span><span class="sxs-lookup"><span data-stu-id="4dfbb-113">Container port-to-host port mapping</span></span>
* <span data-ttu-id="4dfbb-114">Zjišťování – kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="4dfbb-114">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="4dfbb-115">Možnost konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="4dfbb-115">Ability to configure and set environment variables</span></span>

<span data-ttu-id="4dfbb-116">Podíváme, jak každou z možností funguje, když jste balení kontejnerové služby mají být zahrnuty do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-116">Let's look at how each of capabilities works when you're packaging a containerized service to be included in your application.</span></span>

## <a name="package-a-windows-container"></a><span data-ttu-id="4dfbb-117">Balíček Windows kontejneru</span><span class="sxs-lookup"><span data-stu-id="4dfbb-117">Package a Windows container</span></span>
<span data-ttu-id="4dfbb-118">Když vytvoříte balíček kontejner, můžete použít buď šablony sady Visual Studio projektu nebo [ručně vytvořit balíček aplikace](#manually).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-118">When you package a container, you can choose to use either a Visual Studio project template or [create the application package manually](#manually).</span></span>  <span data-ttu-id="4dfbb-119">Pokud používáte Visual Studio, struktury balíček aplikace a soubory manifestu jsou vytvořeny šablonou nový projekt pro vás.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-119">When you use Visual Studio, the application package structure and manifest files are created by the New Project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="4dfbb-120">Nejjednodušší způsob, jak zabalit stávající image kontejneru do služby je pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-120">The easiest way to package an existing container image into a service is to use Visual Studio.</span></span>

## <a name="use-visual-studio-to-package-an-existing-container-image"></a><span data-ttu-id="4dfbb-121">Balíček stávající image kontejneru pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4dfbb-121">Use Visual Studio to package an existing container image</span></span>
<span data-ttu-id="4dfbb-122">Visual Studio poskytuje šablony služby Service Fabric vám pomůžou nasadit kontejner pro cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-122">Visual Studio provides a Service Fabric service template to help you deploy a container to a Service Fabric cluster.</span></span>

1. <span data-ttu-id="4dfbb-123">Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-123">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="4dfbb-124">Zvolte **hosta kontejneru** jako šablonu služby.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-124">Choose **Guest Container** as the service template.</span></span>
3. <span data-ttu-id="4dfbb-125">Zvolte **název bitové kopie** a zadejte cestu k bitové kopii v úložišti kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-125">Choose **Image Name** and provide the path to the image in your container repository.</span></span> <span data-ttu-id="4dfbb-126">Například `myrepo/myimage:v1` v https://hub.docker.com</span><span class="sxs-lookup"><span data-stu-id="4dfbb-126">For example, `myrepo/myimage:v1` in https://hub.docker.com</span></span>
4. <span data-ttu-id="4dfbb-127">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-127">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="4dfbb-128">Pokud služby kontejnerizované potřebuje koncový bod pro komunikaci, můžete teď přidejte protokol, port a typ souboru ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-128">If your containerized service needs an endpoint for communication, you can now add the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="4dfbb-129">Například:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-129">For example:</span></span> 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    <span data-ttu-id="4dfbb-130">Tím, že poskytuje `UriScheme`, Service Fabric automaticky zaregistruje koncový bod kontejneru službou pojmenování pro možnosti rozpoznání.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-130">By providing the `UriScheme`, Service Fabric automatically registers the container endpoint with the Naming service for discoverability.</span></span> <span data-ttu-id="4dfbb-131">Port můžete být fixed (jak je uvedeno v předchozím příkladu) nebo přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-131">The port can either be fixed (as shown in the preceding example) or dynamically allocated.</span></span> <span data-ttu-id="4dfbb-132">Pokud port neurčíte, se přidělí dynamicky z rozsahu portů aplikace (protože by se stalo s jakoukoli službu,).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-132">If you don't specify a port, it is dynamically allocated from the application port range (as would happen with any service).</span></span>
    <span data-ttu-id="4dfbb-133">Musíte také nakonfigurovat kontejner, aby služba mapování portů hostitele zadáním `PortBinding` zásad v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-133">You also need to configure the container to host port mapping by specifying a `PortBinding` policy in the application manifest.</span></span> <span data-ttu-id="4dfbb-134">Další informace najdete v tématu [kontejneru konfigurace k mapování portů hostitele](#Portsection).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-134">For more information, see [Configure container to host port mapping](#Portsection).</span></span>
6. <span data-ttu-id="4dfbb-135">Pokud vaše kontejneru musí zásad správného řízení prostředků můžete přidat `ResourceGovernancePolicy`.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-135">If your container needs resource governance then add a `ResourceGovernancePolicy`.</span></span>
8. <span data-ttu-id="4dfbb-136">Pokud se váš kontejner potřebuje ověřovat v privátním úložišti, přidejte `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-136">If your container needs to authenticate with a private repository, then add `RepositoryCredentials`.</span></span>
7. <span data-ttu-id="4dfbb-137">Pokud používáte v systému Windows Server 2016 počítač s povolenou podporou kontejneru, můžete použít balíček a publikovat akce k nasazení na místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-137">If you are running on a Windows Server 2016 machine with container support enabled, you can use the package and publish action to deploy to your local cluster.</span></span> 
8. <span data-ttu-id="4dfbb-138">Až bude připravený, můžete publikovat aplikaci do vzdáleného clusteru nebo řešení správy zdrojového kódu se změnami.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-138">When ready, you can publish the application to a remote cluster or check in the solution to source control.</span></span> 

<span data-ttu-id="4dfbb-139">Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="4dfbb-139">For an example, checkout the [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="creating-a-windows-server-2016-cluster"></a><span data-ttu-id="4dfbb-140">Vytvoření clusteru systému Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="4dfbb-140">Creating a Windows Server 2016 cluster</span></span>
<span data-ttu-id="4dfbb-141">Chcete-li nasadit kontejnerizované aplikace, vytvoření clusteru se systémem Windows Server 2016 s povolenou podporou kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-141">To deploy your containerized application, you need to create a cluster running Windows Server 2016 with container support enabled.</span></span> <span data-ttu-id="4dfbb-142">Cluster může být spuštěn místně nebo nasadit pomocí Azure Resource Manageru v Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-142">Your cluster may be running locally, or deployed via Azure Resource Manager in Azure.</span></span> 

<span data-ttu-id="4dfbb-143">Chcete-li nasadit cluster pomocí Azure Resource Manager, zvolte **systému Windows Server 2016 s kontejnery** bitové kopie možnost v Azure.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-143">To deploy a cluster using Azure Resource Manager, choose the **Windows Server 2016 with Containers** image option in Azure.</span></span> <span data-ttu-id="4dfbb-144">Najdete v článku [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-144">See the article [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="4dfbb-145">Ujistěte se, že používáte následující nastavení Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-145">Ensure that you use the following Azure Resource Manager settings:</span></span>

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
<span data-ttu-id="4dfbb-146">Můžete také [šablony pět uzlu Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) k vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-146">You can also use the [Five Node Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) to create a cluster.</span></span> <span data-ttu-id="4dfbb-147">Můžete také načíst komunita [příspěvku na blogu](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) na použití kontejnerů Service Fabric a Windows.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-147">Alternatively read a community [blog post](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) on using Service Fabric and Windows containers.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="4dfbb-148">Ručně zabalení a nasazení bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="4dfbb-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="4dfbb-149">Proces ručně balení kontejnerové služby podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-149">The process of manually packaging a containerized service is based on the following steps:</span></span>

1. <span data-ttu-id="4dfbb-150">Kontejnery publikujte do úložiště.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-150">Publish the containers to your repository.</span></span>
2. <span data-ttu-id="4dfbb-151">Vytvořte strukturu adresáře balíčku.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-151">Create the package directory structure.</span></span>
3. <span data-ttu-id="4dfbb-152">Upravte soubor manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-152">Edit the service manifest file.</span></span>
4. <span data-ttu-id="4dfbb-153">Upravte soubor manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-153">Edit the application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="4dfbb-154">Nasazení a aktivaci bitovou kopii kontejneru</span><span class="sxs-lookup"><span data-stu-id="4dfbb-154">Deploy and activate a container image</span></span>
<span data-ttu-id="4dfbb-155">V Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-155">In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="4dfbb-156">K nasazení a aktivaci kontejner, uveďte název kontejneru bitové kopie do `ContainerHost` element v service manifest.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-156">To deploy and activate a container, put the name of the container image into a `ContainerHost` element in the service manifest.</span></span>

<span data-ttu-id="4dfbb-157">V manifestu služby, přidat `ContainerHost` pro vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-157">In the service manifest, add a `ContainerHost` for the entry point.</span></span> <span data-ttu-id="4dfbb-158">Nastavte `ImageName` jako název kontejneru úložiště a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-158">Then set the `ImageName` to be the name of the container repository and image.</span></span> <span data-ttu-id="4dfbb-159">Následující částečné manifest ukazuje příklad nasazení kontejneru názvem `myimage:v1` z úložiště volána `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-159">The following partial manifest shows an example of how to deploy the container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="4dfbb-160">Můžete zadat volitelné příkazy ke spuštění při spuštění kontejneru `Commands` elementu.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-160">You can specify optional commands to run upon starting the container under the `Commands` element.</span></span> <span data-ttu-id="4dfbb-161">Pro více příkazů čárkami vymezení je.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-161">For multiple commands, comma-delimit them.</span></span> 

## <a name="understand-resource-governance"></a><span data-ttu-id="4dfbb-162">Pochopení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="4dfbb-162">Understand resource governance</span></span>
<span data-ttu-id="4dfbb-163">Zásady správného řízení prostředků je funkce kontejneru, který omezuje prostředky, které můžete použít kontejneru na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-163">Resource governance is a capability of the container that restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="4dfbb-164">`ResourceGovernancePolicy`, Které je určené v manifestu aplikace se používá k deklaraci limitů prostředků pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-164">The `ResourceGovernancePolicy`, which is specified in the application manifest is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="4dfbb-165">Omezení prostředků můžete nastavit pro následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-165">Resource limits can be set for the following resources:</span></span>

* <span data-ttu-id="4dfbb-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="4dfbb-166">Memory</span></span>
* <span data-ttu-id="4dfbb-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="4dfbb-167">MemorySwap</span></span>
* <span data-ttu-id="4dfbb-168">CpuShares (Relativní váha procesoru)</span><span class="sxs-lookup"><span data-stu-id="4dfbb-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="4dfbb-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="4dfbb-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="4dfbb-170">BlkioWeight (BlockIO relativní váhy).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="4dfbb-171">Podpora pro zadání konkrétní blok limity vstupně-výstupní operace, jako je IOPs, počtu bitů za Sekundu pro čtení a zápis a dalších plánujeme pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-171">Support for specifying specific block IO limits such as IOPs, read/write BPS, and others are planned for a future release.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="4dfbb-172">Ověření úložiště</span><span class="sxs-lookup"><span data-stu-id="4dfbb-172">Authenticate a repository</span></span>
<span data-ttu-id="4dfbb-173">Pokud chcete stáhnout kontejner, možná muset zadat přihlašovací údaje k úložišti kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-173">To download a container, you might have to provide sign-in credentials to the container repository.</span></span> <span data-ttu-id="4dfbb-174">Přihlašovací údaje, zadaný v manifestu aplikace, se používají a zadejte přihlašovací údaje, nebo klíč SSH pro stažení image kontejneru z úložiště imagí.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-174">The sign-in credentials, specified in the application manifest, are used to specify the sign-in information, or SSH key, for downloading the container image from the image repository.</span></span> <span data-ttu-id="4dfbb-175">Následující příklad ukazuje účtu nazvaného *TestUser* společně s heslo jako prostý text (*není* doporučená):</span><span class="sxs-lookup"><span data-stu-id="4dfbb-175">The following example shows an account called *TestUser* along with the password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="4dfbb-176">Doporučujeme, abyste heslo šifrovat pomocí certifikátu, který je nasazen do počítače.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-176">We recommend that you encrypt the password by using a certificate that's deployed to the machine.</span></span>

<span data-ttu-id="4dfbb-177">Následující příklad ukazuje účtu nazvaného *TestUser*, kde byla zašifrována heslem pomocí certifikát nazvaný *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-177">The following example shows an account called *TestUser*, where the password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="4dfbb-178">Můžete použít `Invoke-ServiceFabricEncryptText` příkaz prostředí PowerShell k vytvoření tajný šifrovaného textu pro heslo.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-178">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text for the password.</span></span> <span data-ttu-id="4dfbb-179">Další informace najdete v článku [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-179">For more information, see the article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="4dfbb-180">Privátní klíč certifikátu, který se používá k dešifrování hesla musí být nasazený v metodu out-of-band v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-180">The private key of the certificate that's used to decrypt the password must be deployed to the local machine in an out-of-band method.</span></span> <span data-ttu-id="4dfbb-181">(V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí balíček služby k počítači, může dešifrovat tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys the service package to the machine, it can decrypt the secret.</span></span> <span data-ttu-id="4dfbb-182">Pomocí tajný klíč s názvem účtu může pak ověřit s úložištěm kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-182">By using the secret along with the account name, it can then authenticate with the container repository.</span></span>

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

## <span data-ttu-id="4dfbb-183"><a name ="Portsection"></a>Konfigurace kontejner, aby služba mapování portů hostitele</span><span class="sxs-lookup"><span data-stu-id="4dfbb-183"><a name ="Portsection"></a> Configure container to host port mapping</span></span>
<span data-ttu-id="4dfbb-184">Můžete nakonfigurovat port hostitele používá ke komunikaci s kontejneru zadáním `PortBinding` v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-184">You can configure a host port used to communicate with the container by specifying a `PortBinding` in the application manifest.</span></span> <span data-ttu-id="4dfbb-185">Vazbou portu mapuje port, na kterém služba naslouchá uvnitř kontejneru na port na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-185">The port binding maps the port to which the service is listening inside the container to a port on the host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="4dfbb-186">Konfigurace zjišťování kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="4dfbb-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="4dfbb-187">Můžete použít `PortBinding` elementu, který chcete namapovat port kontejneru na koncový bod v service manifest.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-187">You can use the `PortBinding` element to map a container port to an endpoint in the service manifest.</span></span> <span data-ttu-id="4dfbb-188">V následujícím příkladu, koncový bod `Endpoint1` určuje pevný port, 8905.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-188">In the following example, the endpoint `Endpoint1` specifies a fixed port, 8905.</span></span> <span data-ttu-id="4dfbb-189">Ho můžete také zadat žádné port vůbec, v takovém případě je pro vás zvolen náhodných portu z rozsahu portů aplikace clusteru.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-189">It can also specify no port at all, in which case a random port from the cluster's application port range is chosen for you.</span></span>


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
<span data-ttu-id="4dfbb-190">Pokud zadáte koncový bod, pomocí `Endpoint` značky v service manifest kontejner hosta, Service Fabric můžete automaticky publikovat tento koncový bod služby pojmenování.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-190">If you specify an endpoint, using the `Endpoint` tag in the service manifest of a guest container, Service Fabric can automatically publish this endpoint to the Naming service.</span></span> <span data-ttu-id="4dfbb-191">Dalším službám, které jsou spuštěny v clusteru může zjišťovat proto tento kontejner pomocí dotazů REST pro řešení.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-191">Other services that are running in the cluster can thus discover this container using the REST queries for resolving.</span></span>

<span data-ttu-id="4dfbb-192">Tím, že zaregistrujete ve službě pojmenování, můžete provádět-kontejnery komunikace v rámci vašeho kontejneru pomocí [reverse proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-192">By registering with the Naming service, you can perform container-to-container communication within your container by using the [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="4dfbb-193">Komunikace se provádí zadáním naslouchající port http reverzní proxy server a název služby, které chcete ke komunikaci s jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-193">Communication is performed by providing the reverse proxy http listening port and the name of the services that you want to communicate with as environment variables.</span></span> <span data-ttu-id="4dfbb-194">Další informace najdete v další části.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-194">For more information, see the next section.</span></span> 

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="4dfbb-195">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="4dfbb-195">Configure and set environment variables</span></span>
<span data-ttu-id="4dfbb-196">Proměnné prostředí je možné zadat pro každý balíček kódu v manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-196">Environment variables can be specified for each code package in the service manifest.</span></span> <span data-ttu-id="4dfbb-197">Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-197">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="4dfbb-198">Hodnoty proměnných prostředí můžete přepsat v manifestu aplikace, nebo je můžete zadat v průběhu nasazení jako parametry aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-198">You can override environment variable values in the application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="4dfbb-199">Následující fragment kódu XML manifestu služby ukazuje příklad toho, jak zadat proměnné prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-199">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>

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

<span data-ttu-id="4dfbb-200">Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-200">These environment variables can be overridden at the application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="4dfbb-201">V předchozím příkladu jsme zadali explicitní hodnotu pro `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu pro `BackendServiceName` parametr prostřednictvím `[BackendSvc]` parametr aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-201">In the previous example, we specified an explicit value for the `HttpGateway` environment variable (19000), while we set the value for `BackendServiceName` parameter via the `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="4dfbb-202">Tato nastavení umožňují zadat hodnotu pro `BackendServiceName`hodnota při nasazení aplikace a nemá pevnou hodnotu v manifestu.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-202">These settings enable you to specify the value for `BackendServiceName`value when you deploy the application and not have a fixed value in the manifest.</span></span>

## <a name="configure-isolation-mode"></a><span data-ttu-id="4dfbb-203">Konfigurace režimu izolace</span><span class="sxs-lookup"><span data-stu-id="4dfbb-203">Configure isolation mode</span></span>

<span data-ttu-id="4dfbb-204">Systém Windows podporuje dva režimy izolace kontejnery - proces a technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-204">Windows supports two isolation modes for containers - process and Hyper-V.</span></span>  <span data-ttu-id="4dfbb-205">V režimu izolace procesů všechny kontejnery spuštěné na stejném hostitelském počítači sdílejí jádro s hostitelem.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-205">With the process isolation mode, all the containers running on the same host machine share the kernel with the host.</span></span> <span data-ttu-id="4dfbb-206">V režimu izolace Hyper-V se jádra pro jednotlivé kontejnery Hyper-V a hostitele kontejneru izolují.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-206">With the Hyper-V isolation mode, the kernels are isolated between each Hyper-V container and the container host.</span></span> <span data-ttu-id="4dfbb-207">Režimu izolace je uveden v `ContainerHostPolicies` značky v souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-207">The isolation mode is specified in the `ContainerHostPolicies` tag in the application manifest file.</span></span>  <span data-ttu-id="4dfbb-208">Je možné zadat tyto režimy izolace: `process`, `hyperv` a `default`.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-208">The isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="4dfbb-209">`default` Výchozí nastavení režimu izolace `process` v hostitelích Windows Server a výchozí hodnota je `hyperv` na hostitelích s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-209">The `default` isolation mode defaults to `process` on Windows Server hosts, and defaults to `hyperv` on Windows 10 hosts.</span></span>  <span data-ttu-id="4dfbb-210">Následující fragment kódu ukazuje, jakým způsobem je režim izolace určený v souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4dfbb-210">The following snippet shows how the isolation mode is specified in the application manifest file.</span></span>

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="4dfbb-211">Dokončit příklady pro aplikace a manifest služby</span><span class="sxs-lookup"><span data-stu-id="4dfbb-211">Complete examples for application and service manifest</span></span>

<span data-ttu-id="4dfbb-212">Manifest aplikace například takto:</span><span class="sxs-lookup"><span data-stu-id="4dfbb-212">An example application manifest follows:</span></span>

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

<span data-ttu-id="4dfbb-213">Následuje manifest službu příkladu (zadané v předchozím manifest aplikace):</span><span class="sxs-lookup"><span data-stu-id="4dfbb-213">An example service manifest (specified in the preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4dfbb-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dfbb-214">Next steps</span></span>
<span data-ttu-id="4dfbb-215">Teď, když jste nasadili kontejnerové služby, zjistěte, jak Správa životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="4dfbb-215">Now that you have deployed a containerized service, learn how to manage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="4dfbb-216">Přehled Service Fabric a kontejnery</span><span class="sxs-lookup"><span data-stu-id="4dfbb-216">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* <span data-ttu-id="4dfbb-217">Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="4dfbb-217">For an example, checkout [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>
