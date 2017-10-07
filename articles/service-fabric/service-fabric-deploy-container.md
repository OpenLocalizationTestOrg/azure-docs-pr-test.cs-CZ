---
title: "aaaService prostředků infrastruktury a nasazení kontejnerů | Microsoft Docs"
description: "Service Fabric a hello používat kontejnery toodeploy mikroslužbu aplikací. Tento článek popisuje hello možnosti, které Service Fabric nabízí pro kontejnery a jak toodeploy kontejner Windows image do clusteru."
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
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a><span data-ttu-id="117e6-104">Nasazení tooService kontejneru Windows Fabric</span><span class="sxs-lookup"><span data-stu-id="117e6-104">Deploy a Windows container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="117e6-105">Nasazení kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="117e6-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="117e6-106">Nasadit kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="117e6-106">Deploy Docker Container</span></span>](service-fabric-deploy-container-linux.md)
> 
> 

<span data-ttu-id="117e6-107">Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Windows hello proces.</span><span class="sxs-lookup"><span data-stu-id="117e6-107">This article walks you through hello process of building containerized services in Windows containers.</span></span>

<span data-ttu-id="117e6-108">Service Fabric má několik funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb běžících v rámci kontejnery.</span><span class="sxs-lookup"><span data-stu-id="117e6-108">Service Fabric has several capabilities that help you with building applications that are composed of microservices running inside containers.</span></span> 

<span data-ttu-id="117e6-109">Hello schopnosti zahrnují:</span><span class="sxs-lookup"><span data-stu-id="117e6-109">hello capabilities include:</span></span>

* <span data-ttu-id="117e6-110">Nasazení bitové kopie kontejneru a aktivace</span><span class="sxs-lookup"><span data-stu-id="117e6-110">Container image deployment and activation</span></span>
* <span data-ttu-id="117e6-111">Zásady správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="117e6-111">Resource governance</span></span>
* <span data-ttu-id="117e6-112">Úložiště ověřování</span><span class="sxs-lookup"><span data-stu-id="117e6-112">Repository authentication</span></span>
* <span data-ttu-id="117e6-113">Mapování port kontejneru hostitele a portu</span><span class="sxs-lookup"><span data-stu-id="117e6-113">Container port-to-host port mapping</span></span>
* <span data-ttu-id="117e6-114">Zjišťování – kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="117e6-114">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="117e6-115">Možnost tooconfigure a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="117e6-115">Ability tooconfigure and set environment variables</span></span>

<span data-ttu-id="117e6-116">Podíváme, jak každý z možností funguje, když jste balení kontejnerizované služby toobe, zahrnout do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="117e6-116">Let's look at how each of capabilities works when you're packaging a containerized service toobe included in your application.</span></span>

## <a name="package-a-windows-container"></a><span data-ttu-id="117e6-117">Balíček Windows kontejneru</span><span class="sxs-lookup"><span data-stu-id="117e6-117">Package a Windows container</span></span>
<span data-ttu-id="117e6-118">Když vytvoříte balíček kontejner, můžete toouse buď šablony sady Visual Studio projektu nebo [ručně vytvořit balíček aplikace hello](#manually).</span><span class="sxs-lookup"><span data-stu-id="117e6-118">When you package a container, you can choose toouse either a Visual Studio project template or [create hello application package manually](#manually).</span></span>  <span data-ttu-id="117e6-119">Pokud používáte Visual Studio, struktura balíčku aplikace hello a souborů manifestu vytvoří se nový projekt šablonou hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-119">When you use Visual Studio, hello application package structure and manifest files are created by hello New Project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="117e6-120">Nejjednodušší způsob, jak toopackage Hello stávající image kontejneru do služby je toouse Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="117e6-120">hello easiest way toopackage an existing container image into a service is toouse Visual Studio.</span></span>

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a><span data-ttu-id="117e6-121">Pomocí sady Visual Studio toopackage stávající image kontejneru</span><span class="sxs-lookup"><span data-stu-id="117e6-121">Use Visual Studio toopackage an existing container image</span></span>
<span data-ttu-id="117e6-122">Visual Studio poskytuje Service Fabric toohelp šablony služby můžete nasadit cluster Service Fabric tooa kontejneru.</span><span class="sxs-lookup"><span data-stu-id="117e6-122">Visual Studio provides a Service Fabric service template toohelp you deploy a container tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="117e6-123">Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="117e6-123">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="117e6-124">Zvolte **hosta kontejneru** jako šablonu služby hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-124">Choose **Guest Container** as hello service template.</span></span>
3. <span data-ttu-id="117e6-125">Zvolte **název bitové kopie** a zadejte hello cesta toohello bitové kopie v kontejneru úložiště.</span><span class="sxs-lookup"><span data-stu-id="117e6-125">Choose **Image Name** and provide hello path toohello image in your container repository.</span></span> <span data-ttu-id="117e6-126">Například `myrepo/myimage:v1` v https://hub.docker.com</span><span class="sxs-lookup"><span data-stu-id="117e6-126">For example, `myrepo/myimage:v1` in https://hub.docker.com</span></span>
4. <span data-ttu-id="117e6-127">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="117e6-127">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="117e6-128">Pokud vaše kontejnerizované služba musí koncový bod pro komunikaci, můžete nyní přidat hello protokol, port a typ toohello ServiceManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="117e6-128">If your containerized service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="117e6-129">Například:</span><span class="sxs-lookup"><span data-stu-id="117e6-129">For example:</span></span> 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    <span data-ttu-id="117e6-130">Tím, že poskytuje hello `UriScheme`, Service Fabric automaticky zaregistruje koncový bod hello kontejneru se hello Naming service pro možnosti rozpoznání.</span><span class="sxs-lookup"><span data-stu-id="117e6-130">By providing hello `UriScheme`, Service Fabric automatically registers hello container endpoint with hello Naming service for discoverability.</span></span> <span data-ttu-id="117e6-131">Hello port můžete být fixed (jak je uvedeno v předchozím příkladu hello) nebo přidělí dynamicky.</span><span class="sxs-lookup"><span data-stu-id="117e6-131">hello port can either be fixed (as shown in hello preceding example) or dynamically allocated.</span></span> <span data-ttu-id="117e6-132">Pokud port neurčíte, se přidělí dynamicky z rozsahu portů aplikace hello (protože by se stalo s jakoukoli službu,).</span><span class="sxs-lookup"><span data-stu-id="117e6-132">If you don't specify a port, it is dynamically allocated from hello application port range (as would happen with any service).</span></span>
    <span data-ttu-id="117e6-133">Musíte taky mapování portů tooconfigure hello kontejneru toohost zadáním `PortBinding` zásad v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-133">You also need tooconfigure hello container toohost port mapping by specifying a `PortBinding` policy in hello application manifest.</span></span> <span data-ttu-id="117e6-134">Další informace najdete v tématu [konfigurace mapování portů toohost kontejneru](#Portsection).</span><span class="sxs-lookup"><span data-stu-id="117e6-134">For more information, see [Configure container toohost port mapping](#Portsection).</span></span>
6. <span data-ttu-id="117e6-135">Pokud vaše kontejneru musí zásad správného řízení prostředků můžete přidat `ResourceGovernancePolicy`.</span><span class="sxs-lookup"><span data-stu-id="117e6-135">If your container needs resource governance then add a `ResourceGovernancePolicy`.</span></span>
8. <span data-ttu-id="117e6-136">Pokud vaše kontejneru potřebuje tooauthenticate s privátní úložiště, přidejte `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="117e6-136">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>
7. <span data-ttu-id="117e6-137">Pokud používáte v systému Windows Server 2016 počítač s povolenou podporou kontejneru, můžete použít balíček hello a publikovat akce toodeploy tooyour místní cluster.</span><span class="sxs-lookup"><span data-stu-id="117e6-137">If you are running on a Windows Server 2016 machine with container support enabled, you can use hello package and publish action toodeploy tooyour local cluster.</span></span> 
8. <span data-ttu-id="117e6-138">Až bude připravený, můžete publikovat hello aplikace tooa vzdálený cluster nebo zkontrolujte v ovládacím prvku toosource řešení hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-138">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span> 

<span data-ttu-id="117e6-139">Příklad najdete v článku věnovaném hello [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="117e6-139">For an example, checkout hello [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="creating-a-windows-server-2016-cluster"></a><span data-ttu-id="117e6-140">Vytvoření clusteru systému Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="117e6-140">Creating a Windows Server 2016 cluster</span></span>
<span data-ttu-id="117e6-141">toodeploy kontejnerizované aplikace, budete potřebovat toocreate clusteru se systémem Windows Server 2016 s podporou kontejneru povolena.</span><span class="sxs-lookup"><span data-stu-id="117e6-141">toodeploy your containerized application, you need toocreate a cluster running Windows Server 2016 with container support enabled.</span></span> <span data-ttu-id="117e6-142">Cluster může být spuštěn místně nebo nasadit pomocí Azure Resource Manageru v Azure.</span><span class="sxs-lookup"><span data-stu-id="117e6-142">Your cluster may be running locally, or deployed via Azure Resource Manager in Azure.</span></span> 

<span data-ttu-id="117e6-143">toodeploy clusteru pomocí Azure Resource Manager, zvolte hello **systému Windows Server 2016 s kontejnery** bitové kopie možnost v Azure.</span><span class="sxs-lookup"><span data-stu-id="117e6-143">toodeploy a cluster using Azure Resource Manager, choose hello **Windows Server 2016 with Containers** image option in Azure.</span></span> <span data-ttu-id="117e6-144">Najdete v článku hello [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="117e6-144">See hello article [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="117e6-145">Ujistěte se, že používáte hello následující nastavení Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="117e6-145">Ensure that you use hello following Azure Resource Manager settings:</span></span>

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
<span data-ttu-id="117e6-146">Můžete taky hello [šablony pět uzlu Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate clusteru.</span><span class="sxs-lookup"><span data-stu-id="117e6-146">You can also use hello [Five Node Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate a cluster.</span></span> <span data-ttu-id="117e6-147">Můžete také načíst komunita [příspěvku na blogu](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) na použití kontejnerů Service Fabric a Windows.</span><span class="sxs-lookup"><span data-stu-id="117e6-147">Alternatively read a community [blog post](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) on using Service Fabric and Windows containers.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="117e6-148">Ručně zabalení a nasazení bitové kopie kontejneru</span><span class="sxs-lookup"><span data-stu-id="117e6-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="117e6-149">proces Hello ručně balení kontejnerové služby je založena na hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="117e6-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="117e6-150">Publikujte hello kontejnery tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="117e6-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="117e6-151">Vytvořte strukturu adresáře balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="117e6-152">Upravte soubor manifestu služby hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="117e6-153">Upravte soubor manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="117e6-154">Nasazení a aktivaci bitovou kopii kontejneru</span><span class="sxs-lookup"><span data-stu-id="117e6-154">Deploy and activate a container image</span></span>
<span data-ttu-id="117e6-155">V hello Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky.</span><span class="sxs-lookup"><span data-stu-id="117e6-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="117e6-156">toodeploy a aktivujte kontejner a put hello název obrázku kontejneru hello do `ContainerHost` element v hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="117e6-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="117e6-157">V manifestu hello služby, přidat `ContainerHost` pro hello vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="117e6-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="117e6-158">Potom sadu hello `ImageName` toobe hello název hello kontejneru úložiště a bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="117e6-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="117e6-159">Hello následující částečné manifest ukazuje příklad jak toodeploy hello kontejner nazývá `myimage:v1` z úložiště volána `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="117e6-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="117e6-160">Můžete určit volitelné příkazy toorun při spuštění hello kontejneru hello `Commands` elementu.</span><span class="sxs-lookup"><span data-stu-id="117e6-160">You can specify optional commands toorun upon starting hello container under hello `Commands` element.</span></span> <span data-ttu-id="117e6-161">Pro více příkazů čárkami vymezení je.</span><span class="sxs-lookup"><span data-stu-id="117e6-161">For multiple commands, comma-delimit them.</span></span> 

## <a name="understand-resource-governance"></a><span data-ttu-id="117e6-162">Pochopení zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="117e6-162">Understand resource governance</span></span>
<span data-ttu-id="117e6-163">Zásady správného řízení prostředků je na hostiteli hello můžete použít funkce hello kontejneru, který omezuje hello prostředky, které hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="117e6-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="117e6-164">Hello `ResourceGovernancePolicy`, které je určené v manifestu aplikace hello je použité toodeclare prostředků limity pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="117e6-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="117e6-165">Omezení prostředků lze nastavit pro hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="117e6-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="117e6-166">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="117e6-166">Memory</span></span>
* <span data-ttu-id="117e6-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="117e6-167">MemorySwap</span></span>
* <span data-ttu-id="117e6-168">CpuShares (Relativní váha procesoru)</span><span class="sxs-lookup"><span data-stu-id="117e6-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="117e6-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="117e6-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="117e6-170">BlkioWeight (BlockIO relativní váhy).</span><span class="sxs-lookup"><span data-stu-id="117e6-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="117e6-171">Podpora pro zadání konkrétní blok limity vstupně-výstupní operace, jako je IOPs, počtu bitů za Sekundu pro čtení a zápis a dalších plánujeme pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="117e6-171">Support for specifying specific block IO limits such as IOPs, read/write BPS, and others are planned for a future release.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="117e6-172">Ověření úložiště</span><span class="sxs-lookup"><span data-stu-id="117e6-172">Authenticate a repository</span></span>
<span data-ttu-id="117e6-173">toodownload kontejner, můžete mít tooprovide přihlašovací údaje toohello kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="117e6-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="117e6-174">Hello přihlašovací údaje, zadaný v manifestu aplikace hello, jsou použité toospecify hello přihlašovací údaje nebo klíč SSH pro stahování hello kontejneru image z úložiště imagí hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="117e6-175">Hello následující příklad ukazuje účtu nazvaného *TestUser* společně s hello heslo jako prostý text (*není* doporučená):</span><span class="sxs-lookup"><span data-stu-id="117e6-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="117e6-176">Doporučujeme, abyste šifrování hesla hello pomocí certifikátu, která nasadila toohello počítače.</span><span class="sxs-lookup"><span data-stu-id="117e6-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="117e6-177">Hello následující příklad ukazuje účtu nazvaného *TestUser*, kde hello hesla byla zašifrována pomocí certifikát nazvaný *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="117e6-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="117e6-178">Můžete použít hello `Invoke-ServiceFabricEncryptText` prostředí PowerShell příkaz toocreate hello tajný šifrovaný text hello hesla.</span><span class="sxs-lookup"><span data-stu-id="117e6-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="117e6-179">Další informace najdete v článku hello [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="117e6-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="117e6-180">privátní klíč Hello hello certifikátu, který byl použit toodecrypt hello heslo musí být nasazené toohello v metodu out-of-band místního počítače.</span><span class="sxs-lookup"><span data-stu-id="117e6-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="117e6-181">(V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí počítač toohello balíček služby hello, ho dešifrovat tajný klíč hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="117e6-182">Pomocí hello tajný klíč společně s názvem účtu hello můžete poté ověřit pomocí hello kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="117e6-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

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

## <span data-ttu-id="117e6-183"><a name ="Portsection"></a>Konfigurace mapování portů toohost kontejneru</span><span class="sxs-lookup"><span data-stu-id="117e6-183"><a name ="Portsection"></a> Configure container toohost port mapping</span></span>
<span data-ttu-id="117e6-184">Můžete nakonfigurovat toocommunicate port používaný na hostiteli s hello kontejneru zadáním `PortBinding` v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="117e6-185">Hello port vazby mapy hello port toowhich hello služba naslouchá uvnitř port tooa hello kontejneru na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="117e6-186">Konfigurace zjišťování kontejnery a komunikace</span><span class="sxs-lookup"><span data-stu-id="117e6-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="117e6-187">Můžete použít hello `PortBinding` element toomap tooan koncový bod port kontejneru v hello service manifest.</span><span class="sxs-lookup"><span data-stu-id="117e6-187">You can use hello `PortBinding` element toomap a container port tooan endpoint in hello service manifest.</span></span> <span data-ttu-id="117e6-188">V následujícím příkladu hello, hello koncový bod `Endpoint1` určuje pevný port, 8905.</span><span class="sxs-lookup"><span data-stu-id="117e6-188">In hello following example, hello endpoint `Endpoint1` specifies a fixed port, 8905.</span></span> <span data-ttu-id="117e6-189">Toho může také specifikovat žádné port vůbec, v takovém případě je zvolen náhodných portu z rozsahu portů aplikace hello clusteru pro vás.</span><span class="sxs-lookup"><span data-stu-id="117e6-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>


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
<span data-ttu-id="117e6-190">Pokud zadáte koncový bod, pomocí hello `Endpoint` značky v service manifest hello kontejneru hosta, Service Fabric můžete automaticky publikovat tento koncový bod toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="117e6-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="117e6-191">Dalším službám, které jsou spuštěny v clusteru hello může zjišťovat proto tento kontejner pomocí hello REST dotazů pro řešení.</span><span class="sxs-lookup"><span data-stu-id="117e6-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

<span data-ttu-id="117e6-192">Po registraci hello Naming service, můžete provádět-kontejnery komunikace v rámci vašeho kontejneru pomocí hello [reverse proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="117e6-192">By registering with hello Naming service, you can perform container-to-container communication within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="117e6-193">Komunikace se provádí zadáním naslouchající port http hello reverzní proxy server a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="117e6-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="117e6-194">Další informace najdete v tématu hello další části.</span><span class="sxs-lookup"><span data-stu-id="117e6-194">For more information, see hello next section.</span></span> 

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="117e6-195">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="117e6-195">Configure and set environment variables</span></span>
<span data-ttu-id="117e6-196">Pro každý balíček kódu v hello service manifest lze zadat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="117e6-196">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="117e6-197">Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host.</span><span class="sxs-lookup"><span data-stu-id="117e6-197">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="117e6-198">Proměnné prostředí hodnoty v aplikaci hello manifest nebo je zadat během nasazování jako parametry aplikace můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="117e6-198">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="117e6-199">Hello následující služby manifestu XML fragment kódu ukazuje příklad toospecify proměnných prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="117e6-199">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

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

<span data-ttu-id="117e6-200">Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="117e6-200">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="117e6-201">V předchozím příkladu hello jsme zadali explicitní hodnotu pro hello `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu hello `BackendServiceName` parametr prostřednictvím hello `[BackendSvc]` parametr aplikace.</span><span class="sxs-lookup"><span data-stu-id="117e6-201">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="117e6-202">Tato nastavení umožňují hodnotu hello toospecify `BackendServiceName`hodnota při nasazení aplikace hello a v manifestu hello nemá pevnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="117e6-202">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="configure-isolation-mode"></a><span data-ttu-id="117e6-203">Konfigurace režimu izolace</span><span class="sxs-lookup"><span data-stu-id="117e6-203">Configure isolation mode</span></span>

<span data-ttu-id="117e6-204">Systém Windows podporuje dva režimy izolace kontejnery - proces a technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="117e6-204">Windows supports two isolation modes for containers - process and Hyper-V.</span></span>  <span data-ttu-id="117e6-205">V režimu izolace procesu hello hello všechny kontejnery hello systémem stejné hostitele počítače sdílenou složku hello jádra s hostitelem hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-205">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="117e6-206">V režimu izolace hello technologie Hyper-V jsou izolovány. mezi každou kontejneru technologie Hyper-V a hostitelem kontejneru hello hello jádra.</span><span class="sxs-lookup"><span data-stu-id="117e6-206">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="117e6-207">režimu izolace Hello je uveden v hello `ContainerHostPolicies` značky v souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-207">hello isolation mode is specified in hello `ContainerHostPolicies` tag in hello application manifest file.</span></span>  <span data-ttu-id="117e6-208">režim Hello izolaci, které lze zadat `process`, `hyperv`, a `default`.</span><span class="sxs-lookup"><span data-stu-id="117e6-208">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="117e6-209">Hello `default` režimu izolace výchozí příliš`process` v systému Windows Server hostuje a použije se výchozí hodnota příliš`hyperv` na hostitelích s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="117e6-209">hello `default` isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span>  <span data-ttu-id="117e6-210">Hello následující fragment kódu ukazuje, jak je režimu izolace hello zadaný v souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="117e6-210">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="117e6-211">Dokončit příklady pro aplikace a manifest služby</span><span class="sxs-lookup"><span data-stu-id="117e6-211">Complete examples for application and service manifest</span></span>

<span data-ttu-id="117e6-212">Manifest aplikace například takto:</span><span class="sxs-lookup"><span data-stu-id="117e6-212">An example application manifest follows:</span></span>

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

<span data-ttu-id="117e6-213">Manifest službu příkladu (zadané v předchozím manifest aplikace hello) zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="117e6-213">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="117e6-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="117e6-214">Next steps</span></span>
<span data-ttu-id="117e6-215">Teď, když jste nasadili kontejnerové služby, zjistěte, jak toomanage životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="117e6-215">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="117e6-216">Přehled Service Fabric a kontejnery</span><span class="sxs-lookup"><span data-stu-id="117e6-216">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* <span data-ttu-id="117e6-217">Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="117e6-217">For an example, checkout [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>
