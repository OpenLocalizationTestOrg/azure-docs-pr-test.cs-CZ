---
title: "aaaHow toocontainerize vaše Azure Service Fabric mikroslužeb (preview)"
description: "Azure Service Fabric přidal nové funkce toocontainerize vaše mikroslužeb Service Fabric. Tato funkce je aktuálně ve verzi Preview."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: 6edaff73c0828707c7fa736669ba8084663d31ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a><span data-ttu-id="5e23d-104">Jak toocontainerize vaší služby Fabric spolehlivé Services a Reliable Actors (Preview)</span><span class="sxs-lookup"><span data-stu-id="5e23d-104">How toocontainerize your Service Fabric Reliable Services and Reliable Actors (Preview)</span></span>

<span data-ttu-id="5e23d-105">Service Fabric podporuje containerizing mikroslužeb Service Fabric (spolehlivé služeb a služeb na základě spolehlivého Actor).</span><span class="sxs-lookup"><span data-stu-id="5e23d-105">Service Fabric supports containerizing Service Fabric microservices (Reliable Services, and Reliable Actor based services).</span></span> <span data-ttu-id="5e23d-106">Další informace najdete v tématu [služby fabric kontejnery](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e23d-106">For more information, see [service fabric containers](service-fabric-containers-overview.md).</span></span>


 <span data-ttu-id="5e23d-107">Tato funkce je ve verzi preview a tento článek obsahuje hello různé kroky tooget služby běží uvnitř kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5e23d-107">This feature is in preview and this article provides hello various steps tooget your service running inside a container.</span></span>  

> [!NOTE]
> <span data-ttu-id="5e23d-108">Tato funkce je ve verzi preview a není podporována v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e23d-108">This feature is in preview and is not supported in production.</span></span> <span data-ttu-id="5e23d-109">Tato funkce v současné době funkční pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="5e23d-109">Currently this feature only works for Windows.</span></span>

## <a name="steps-toocontainerize-your-service-fabric-application"></a><span data-ttu-id="5e23d-110">Kroky toocontainerize aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="5e23d-110">Steps toocontainerize your Service Fabric Application</span></span>

1. <span data-ttu-id="5e23d-111">Otevřete aplikaci Service Fabric v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e23d-111">Open your Service Fabric application in Visual Studio.</span></span>

2. <span data-ttu-id="5e23d-112">Přidání třídy [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="5e23d-112">Add class [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour project.</span></span> <span data-ttu-id="5e23d-113">Hello kód v této třídě je pomocné rutiny toocorrectly zatížení hello Service Fabric binární soubory modulu runtime v aplikaci při spuštění v rámci kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5e23d-113">hello code in this class is a helper toocorrectly load hello Service Fabric runtime binaries inside your application when running inside a container.</span></span>

3. <span data-ttu-id="5e23d-114">Pro každý balíček kódu chcete toocontainerize, inicializovat hello zavaděč v hello program Vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="5e23d-114">For each code package you would like toocontainerize, initialize hello loader at hello program entry point.</span></span> <span data-ttu-id="5e23d-115">Přidání statického konstruktoru hello uvedené v následující kód fragment kódu tooyour program Vstupní bod soubor hello.</span><span class="sxs-lookup"><span data-stu-id="5e23d-115">Add hello static constructor shown in hello following code snippet tooyour program entry point file.</span></span>

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is hello entry point of hello service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. <span data-ttu-id="5e23d-116">Sestavení a [balíček](service-fabric-package-apps.md#Package-App) projektu.</span><span class="sxs-lookup"><span data-stu-id="5e23d-116">Build and [package](service-fabric-package-apps.md#Package-App) your project.</span></span> <span data-ttu-id="5e23d-117">toobuild a vytvořit balíček, klikněte pravým tlačítkem na projekt aplikace hello v Průzkumníku řešení a zvolte hello **balíček** příkaz.</span><span class="sxs-lookup"><span data-stu-id="5e23d-117">toobuild and create a package, right-click hello application project in Solution Explorer and choose hello **Package** command.</span></span>

5. <span data-ttu-id="5e23d-118">Pro každý balíček kódu potřebujete toocontainerize hello spustit skript prostředí PowerShell [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span><span class="sxs-lookup"><span data-stu-id="5e23d-118">For every code package you need toocontainerize, run hello PowerShell script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span></span> <span data-ttu-id="5e23d-119">využití Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5e23d-119">hello usage is as follows:</span></span>
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  <span data-ttu-id="5e23d-120">skript Hello vytvoří složku s artefakty Docker v $dockerPackageOutputDirectoryPath.</span><span class="sxs-lookup"><span data-stu-id="5e23d-120">hello script creates a folder with Docker artifacts at $dockerPackageOutputDirectoryPath.</span></span> <span data-ttu-id="5e23d-121">Upravte soubor Docker tooexpose hello generované žádné porty, spusťte instalační program skripty atd. na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="5e23d-121">Modify hello generated Dockerfile tooexpose any ports, run setup scripts etc. based on your needs.</span></span>

6. <span data-ttu-id="5e23d-122">Dále je třeba příliš[sestavení](service-fabric-get-started-containers.md#Build-Containers) a [nabízené](service-fabric-get-started-containers.md#Push-Containers) tooyour úložiště Docker kontejneru balíčku.</span><span class="sxs-lookup"><span data-stu-id="5e23d-122">Next you need too[build](service-fabric-get-started-containers.md#Build-Containers) and [push](service-fabric-get-started-containers.md#Push-Containers) your Docker container package tooyour repository.</span></span>

7. <span data-ttu-id="5e23d-123">Upravte hello ApplicationManifest.xml a ServiceManifest.xml tooadd kontejneru bitové kopie, informace o úložišti, registru ověřování a port hostitele mapování.</span><span class="sxs-lookup"><span data-stu-id="5e23d-123">Modify hello ApplicationManifest.xml and ServiceManifest.xml tooadd your container image, repository information, registry authentication, and port-to-host mapping.</span></span> <span data-ttu-id="5e23d-124">Úprava hello manifesty, najdete v části [vytvoření kontejneru aplikace Azure Service Fabric](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="5e23d-124">For modifying hello manifests, see [Create an Azure Service Fabric container application](service-fabric-get-started-containers.md).</span></span> <span data-ttu-id="5e23d-125">Hello definice balíčku kódu v hello služby manifestu potřebám toobe nahradí bitovou kopii odpovídající kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5e23d-125">hello code package definition in hello service manifest needs toobe replaced with corresponding container image.</span></span> <span data-ttu-id="5e23d-126">Ujistěte se, že toochange hello EntryPoint tooa ContainerHost typu.</span><span class="sxs-lookup"><span data-stu-id="5e23d-126">Make sure toochange hello EntryPoint tooa ContainerHost type.</span></span>

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables tooyour container: -->    
</CodePackage>
  ```

8. <span data-ttu-id="5e23d-127">Přidáte mapování portů hostitel hello Replikátor a koncový bod služby.</span><span class="sxs-lookup"><span data-stu-id="5e23d-127">Add hello port-to-host mapping for your replicator and service endpoint.</span></span> <span data-ttu-id="5e23d-128">Vzhledem k tomu, že oba tyto porty jsou přiřazeny za běhu pomocí Service Fabric, hello ContainerPort nastavena hello toouse toozero přiřazené port pro mapování.</span><span class="sxs-lookup"><span data-stu-id="5e23d-128">Since both these ports are assigned at runtime by Service Fabric, hello ContainerPort is set toozero toouse hello assigned port for mapping.</span></span>

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. <span data-ttu-id="5e23d-129">tootest tuto aplikaci, musíte toodeploy ho tooa clusteru, který používá verzi 5.7 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="5e23d-129">tootest this application, you need toodeploy it tooa cluster that is running version 5.7 or higher.</span></span> <span data-ttu-id="5e23d-130">Kromě toho musí tooedit a aktualizujte hello clusteru nastavení tooenable této funkce ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="5e23d-130">In addition, you need tooedit and update hello cluster settings tooenable this preview feature.</span></span> <span data-ttu-id="5e23d-131">Postupujte podle kroků hello v tomto [článku](service-fabric-cluster-fabric-settings.md) tooadd hello nastavení zobrazí další.</span><span class="sxs-lookup"><span data-stu-id="5e23d-131">Follow hello steps in this [article](service-fabric-cluster-fabric-settings.md) tooadd hello setting shown next.</span></span>
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. <span data-ttu-id="5e23d-132">Další [nasazení](service-fabric-deploy-remove-applications.md) hello upravit clusteru toothis balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e23d-132">Next [deploy](service-fabric-deploy-remove-applications.md) hello edited application package toothis cluster.</span></span>

<span data-ttu-id="5e23d-133">Teď byste měli mít kontejnerizované aplikace Service Fabric spuštění clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e23d-133">You should now have a containerized Service Fabric application running your cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e23d-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e23d-134">Next steps</span></span>
* <span data-ttu-id="5e23d-135">Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="5e23d-135">Learn more about running [containers on Service Fabric](service-fabric-get-started-containers.md).</span></span>
* <span data-ttu-id="5e23d-136">Další informace o hello Service Fabric [životního cyklu aplikace](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="5e23d-136">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
