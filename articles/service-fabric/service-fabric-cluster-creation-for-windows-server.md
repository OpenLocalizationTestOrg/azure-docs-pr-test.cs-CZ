---
title: "aaaCreate cluster Azure Service Fabric samostatné | Microsoft Docs"
description: "Vytvoření clusteru služby Azure Service Fabric z jakéhokoli počítače (fyzické nebo virtuální) s Windows serverem, ať už místní nebo v jakékoli cloudu."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a><span data-ttu-id="4a4c4-103">Vytvoření clusteru s podporou samostatné systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="4a4c4-103">Create a standalone cluster running on Windows Server</span></span>
<span data-ttu-id="4a4c4-104">Můžete vytvořit clustery infrastruktury služby Azure Service Fabric toocreate na všechny virtuální počítače nebo počítače se systémem Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-104">You can use Azure Service Fabric toocreate Service Fabric clusters on any virtual machines or computers running Windows Server.</span></span> <span data-ttu-id="4a4c4-105">To znamená, můžete nasazovat a spouštět aplikace Service Fabric v jakémkoli prostředí, které obsahuje sadu vzájemně propojena počítačů Windows serveru, je-li jej v místním nebo se všechny poskytovatele cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-105">This means you can deploy and run Service Fabric applications in any environment that contains a set of interconnected Windows Server computers, be it on premises or with any cloud provider.</span></span> <span data-ttu-id="4a4c4-106">Service Fabric poskytuje toocreate balíček Instalační program, který clusterů Service Fabric názvem balíček hello samostatné verze Windows serveru.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-106">Service Fabric provides a setup package toocreate Service Fabric clusters called hello standalone Windows Server package.</span></span>

<span data-ttu-id="4a4c4-107">Tento článek vás provede hello kroky pro vytvoření samostatné clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-107">This article walks you through hello steps for creating a Service Fabric standalone cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="4a4c4-108">Tento balíček samostatné systému Windows Server je dostupný a mohou být použity pro nasazení v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-108">This standalone Windows Server package is commercially available and may be used for production deployments.</span></span> <span data-ttu-id="4a4c4-109">Tento balíček může obsahovat nové funkce Service Fabric, které jsou v "Náhled".</span><span class="sxs-lookup"><span data-stu-id="4a4c4-109">This package may contain new Service Fabric features that are in "Preview".</span></span> <span data-ttu-id="4a4c4-110">Posuňte se dolů příliš"[zobrazit náhled funkce obsažené v tomto balíčku](#previewfeatures_anchor)."</span><span class="sxs-lookup"><span data-stu-id="4a4c4-110">Scroll down too"[Preview features included in this package](#previewfeatures_anchor)."</span></span> <span data-ttu-id="4a4c4-111">část hello seznam hello funkce verze preview.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-111">section for hello list of hello preview features.</span></span> <span data-ttu-id="4a4c4-112">Můžete [stáhnout kopii hello smlouvy EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nyní.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-112">You can [download a copy of hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) now.</span></span>
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="4a4c4-113">Získat podporu pro balíček prostředků infrastruktury služby pro systém Windows Server hello</span><span class="sxs-lookup"><span data-stu-id="4a4c4-113">Get support for hello Service Fabric for Windows Server package</span></span>
* <span data-ttu-id="4a4c4-114">Požádejte o samostatný balíček Service Fabric se hello hello komunity pro systém Windows Server v hello [fórum Azure Service Fabric](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-114">Ask hello community about hello Service Fabric standalone package for Windows Server in hello [Azure Service Fabric forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span></span>
* <span data-ttu-id="4a4c4-115">Otevřít lístek pro [Professional podporu pro Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-115">Open a ticket for [Professional Support for Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span></span>  <span data-ttu-id="4a4c4-116">Další informace o Professional podporu společnosti Microsoft [zde](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-116">Learn more about Professional Support from Microsoft [here](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span></span>
* <span data-ttu-id="4a4c4-117">Můžete také získat podporu pro tento balíček jako součást [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-117">You can also get support for this package as a part of [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span></span>
* <span data-ttu-id="4a4c4-118">Další podrobnosti najdete v tématu [možnosti podpory Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-118">For more details, please see [Azure Service Fabric support options](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span></span>
* <span data-ttu-id="4a4c4-119">protokoly toocollect pro účely podpory, spusťte hello [kolektor protokolů samostatné Service Fabric](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-119">toocollect logs for support purposes, run hello [Service Fabric Standalone Log collector](service-fabric-cluster-standalone-package-contents.md).</span></span>

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="4a4c4-120">Stáhněte si balíček prostředků infrastruktury služby pro systém Windows Server hello</span><span class="sxs-lookup"><span data-stu-id="4a4c4-120">Download hello Service Fabric for Windows Server package</span></span>
<span data-ttu-id="4a4c4-121">toocreate hello clusteru, použijte balíček prostředků infrastruktury služby pro systém Windows Server hello (Windows Server 2012 R2 a novější) nachází tady:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-121">toocreate hello cluster, use hello Service Fabric for Windows Server package (Windows Server 2012 R2 and newer) found here:</span></span> <br><span data-ttu-id="4a4c4-122">
[Stáhněte si odkaz – samostatný balíček prostředků infrastruktury služby - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span><span class="sxs-lookup"><span data-stu-id="4a4c4-122">
[Download Link - Service Fabric Standalone Package - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span></span>

<span data-ttu-id="4a4c4-123">Najde podrobnosti o na obsah balíčku hello [zde](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-123">Find details on contents of hello package [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="4a4c4-124">v době vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric Hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-124">hello Service Fabric runtime package is automatically downloaded at time of cluster creation.</span></span> <span data-ttu-id="4a4c4-125">Pokud nasazení z počítače není připojený toohello Internetu, stáhněte balíček modulu runtime hello vzdálené správy z tohoto umístění:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-125">If deploying from a machine not connected toohello internet, please download hello runtime package out of band from here:</span></span> <br><span data-ttu-id="4a4c4-126">
[Stáhněte si odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span><span class="sxs-lookup"><span data-stu-id="4a4c4-126">
[Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span></span>

<span data-ttu-id="4a4c4-127">Najděte ukázky samostatné konfigurace clusteru na:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-127">Find Standalone Cluster Configuration samples at:</span></span> <br><span data-ttu-id="4a4c4-128">
[Konfigurace clusteru samostatné – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span><span class="sxs-lookup"><span data-stu-id="4a4c4-128">
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span></span>

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a><span data-ttu-id="4a4c4-129">Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="4a4c4-129">Create hello cluster</span></span>
<span data-ttu-id="4a4c4-130">Service Fabric může být nasazený tooa jeden počítač vývojového clusteru pomocí hello *ClusterConfig.Unsecure.DevCluster.json* zahrnutý v [ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-130">Service Fabric can be deployed tooa one-machine development cluster by using hello *ClusterConfig.Unsecure.DevCluster.json* file included in [Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span></span>

<span data-ttu-id="4a4c4-131">Rozbalte hello samostatný balíček tooyour počítače, kopie hello ukázka konfigurační soubor toohello místního počítače a pak spusťte hello *CreateServiceFabricCluster.ps1* skriptu prostřednictvím relaci prostředí PowerShell správce ze samostatné hello složky balíčku:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-131">Unpack hello standalone package tooyour machine, copy hello sample config file toohello local machine, then run hello *CreateServiceFabricCluster.ps1* script through an administrator PowerShell session, from hello standalone package folder:</span></span>
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a><span data-ttu-id="4a4c4-132">Krok 1A: vytvoření zabezpečená místního vývojového clusteru</span><span class="sxs-lookup"><span data-stu-id="4a4c4-132">Step 1A: Create an unsecured local development cluster</span></span>
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="4a4c4-133">V tématu hello část nastavení prostředí v [plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md) pro řešení potíží s podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-133">See hello Environment Setup section at [Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md) for troubleshooting details.</span></span>

<span data-ttu-id="4a4c4-134">Pokud dokončíte spuštěné vývojové scénáře, můžete odebrat cluster Service Fabric hello z hello počítače tím, že odkazuje toosteps v části "[odebrat cluster](#removecluster_anchor)".</span><span class="sxs-lookup"><span data-stu-id="4a4c4-134">If you're finished running development scenarios, you can remove hello Service Fabric cluster from hello machine by referring toosteps in section "[Remove a cluster](#removecluster_anchor)".</span></span> 

### <a name="step-1b-create-a-multi-machine-cluster"></a><span data-ttu-id="4a4c4-135">Krok 1B: vytvoření clusteru s více počítači</span><span class="sxs-lookup"><span data-stu-id="4a4c4-135">Step 1B: Create a multi-machine cluster</span></span>
<span data-ttu-id="4a4c4-136">Poté, co jste prošli hello plánování a přípravné kroky popsané v hello následujícím odkazu, jste připraveni toocreate produkční clusteru pomocí konfiguračního souboru clusteru.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-136">After you have gone through hello planning and preparation steps detailed at hello below link, you are ready toocreate your production cluster using your cluster configuration file.</span></span> <br><span data-ttu-id="4a4c4-137">
[Plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md)</span><span class="sxs-lookup"><span data-stu-id="4a4c4-137">
[Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md)</span></span>

1. <span data-ttu-id="4a4c4-138">Ověření hello konfigurační soubor jste napsali spuštěním hello *TestConfiguration.ps1* skriptu ze složky balíčku samostatné hello:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-138">Validate hello configuration file you have written by running hello *TestConfiguration.ps1* script from hello standalone package folder:</span></span>  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    <span data-ttu-id="4a4c4-139">Zobrazený výstup by měl jako níže.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-139">You should see output like below.</span></span> <span data-ttu-id="4a4c4-140">Pokud pole dolní hello "Úspěšně dokončeno" se vrátí jako "PRAVDA", mít zkontrolovány správností a hello clusteru vypadá toobe nasadit na základě konfigurace vstupní hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-140">If hello bottom field "Passed" is returned as "True", sanity checks have passed and hello cluster looks toobe deployable based on hello input configuration.</span></span>

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. <span data-ttu-id="4a4c4-141">Vytvoření clusteru hello: Spusťte hello *CreateServiceFabricCluster.ps1* skriptu toodeploy hello cluster Service Fabric v každém počítači v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-141">Create hello cluster:  Run hello *CreateServiceFabricCluster.ps1* script toodeploy hello Service Fabric cluster across each machine in hello configuration.</span></span> 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> <span data-ttu-id="4a4c4-142">Nasazení trasování jsou zapsány toohello virtuálního počítače nebo počítače, na kterém jste spustili skript prostředí PowerShell CreateServiceFabricCluster.ps1 hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-142">Deployment traces are written toohello VM/machine on which you ran hello CreateServiceFabricCluster.ps1 PowerShell script.</span></span> <span data-ttu-id="4a4c4-143">Ty lze najít v podsložce hello DeploymentTraces, založené na hello adresáře, ze které hello se skript spouštěl.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-143">These can be found in hello subfolder DeploymentTraces, based in hello directory from which hello script was run.</span></span> <span data-ttu-id="4a4c4-144">tooa počítače správně nasazena toosee, pokud byl Service Fabric, vyhledejte hello nainstalované soubory v adresáři FabricDataRoot directory hello, jak je podrobně uvedeno v konfiguračním souboru na clusteru hello FabricSettings část (ve výchozím nastavení c:\ProgramData\SF).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-144">toosee if Service Fabric was deployed correctly tooa machine, find hello installed files in hello FabricDataRoot directory, as detailed in hello cluster configuration file FabricSettings section (by default c:\ProgramData\SF).</span></span> <span data-ttu-id="4a4c4-145">FabricHost.exe a Fabric.exe procesy je možné taky zobrazit spuštěná ve Správci úloh.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-145">As well, FabricHost.exe and Fabric.exe processes can be seen running in Task Manager.</span></span>
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a><span data-ttu-id="4a4c4-146">Krok 1C: vytvoření clusteru do režimu offline (internet odpojení)</span><span class="sxs-lookup"><span data-stu-id="4a4c4-146">Step 1C: Create an offline (internet-disconnected) cluster</span></span>
<span data-ttu-id="4a4c4-147">Při vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric Hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-147">hello Service Fabric runtime package is automatically downloaded at cluster creation.</span></span> <span data-ttu-id="4a4c4-148">Pokud není nasazení clusteru toomachines připojené toohello Internetu, budete potřebovat toodownload hello Service Fabric runtime balíček samostatně a zadejte cestu tooit hello při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-148">When deploying a cluster toomachines not connected toohello internet, you will need toodownload hello Service Fabric runtime package separately, and provide hello path tooit at cluster creation.</span></span>
<span data-ttu-id="4a4c4-149">Hello runtime balíčku stahování lze provádět samostatně, z jiného počítače připojené toohello Internetu, v [stáhnout odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-149">hello runtime package can be downloaded separately, from another machine connected toohello internet, at [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span></span> <span data-ttu-id="4a4c4-150">Zkopírujte hello runtime balíček toowhere nasazujete hello clusteru do režimu offline a vytvoření clusteru hello spuštěním `CreateServiceFabricCluster.ps1` s hello `-FabricRuntimePackagePath` parametr zahrnuty, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-150">Copy hello runtime package toowhere you are deploying hello offline cluster from, and create hello cluster by running `CreateServiceFabricCluster.ps1` with hello `-FabricRuntimePackagePath` parameter included, as shown below:</span></span> 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
<span data-ttu-id="4a4c4-151">kde `.\ClusterConfig.json` a `.\MicrosoftAzureServiceFabric.cab` jsou konfigurace clusteru toohello hello cesty a souboru .cab hello runtime v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-151">where `.\ClusterConfig.json` and `.\MicrosoftAzureServiceFabric.cab` are hello paths toohello cluster configuration and hello runtime .cab file respectively.</span></span>


### <a name="step-2-connect-toohello-cluster"></a><span data-ttu-id="4a4c4-152">Krok 2: Připojení toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="4a4c4-152">Step 2: Connect toohello cluster</span></span>
<span data-ttu-id="4a4c4-153">tooconnect tooa zabezpečení clusteru, najdete v části [Service fabric připojit toosecure cluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-153">tooconnect tooa secure cluster, see [Service fabric connect toosecure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="4a4c4-154">tooconnect tooan nezabezpečená clusteru, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-154">tooconnect tooan unsecure cluster, run hello following PowerShell command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
<span data-ttu-id="4a4c4-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-155">Example:</span></span>
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a><span data-ttu-id="4a4c4-156">Krok 3: Zprovoznit Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="4a4c4-156">Step 3: Bring up Service Fabric Explorer</span></span>
<span data-ttu-id="4a4c4-157">Teď můžete připojit toohello clusteru pomocí Service Fabric Exploreru buď přímo z jednoho z počítačů hello http://localhost:19080/Explorer/index.html nebo vzdáleně s http://<*IPAddressofaMachine*>: 19080 / Explorer/index.HTML.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-157">Now you can connect toohello cluster with Service Fabric Explorer either directly from one of hello machines with http://localhost:19080/Explorer/index.html or remotely with http://<*IPAddressofaMachine*>:19080/Explorer/index.html.</span></span>

## <a name="add-and-remove-nodes"></a><span data-ttu-id="4a4c4-158">Přidání a odebrání uzlů</span><span class="sxs-lookup"><span data-stu-id="4a4c4-158">Add and remove nodes</span></span>
<span data-ttu-id="4a4c4-159">Můžete přidat nebo odebrat cluster Service Fabric samostatné tooyour uzly, protože vaše obchodní potřeby změnit.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-159">You can add or remove nodes tooyour standalone Service Fabric cluster as your business needs change.</span></span> <span data-ttu-id="4a4c4-160">V tématu [přidat nebo odebrat uzly tooa Service Fabric samostatný cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-160">See [Add or Remove nodes tooa Service Fabric standalone cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) for detailed steps.</span></span>

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a><span data-ttu-id="4a4c4-161">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="4a4c4-161">Remove a cluster</span></span>
<span data-ttu-id="4a4c4-162">tooremove clusteru, spusťte hello *RemoveServiceFabricCluster.ps1* skript prostředí PowerShell ze složky balíčku hello a předejte jí hello cesta toohello JSON konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-162">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="4a4c4-163">Volitelně můžete zadat umístění pro protokol hello hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-163">You can optionally specify a location for hello log of hello deletion.</span></span>

<span data-ttu-id="4a4c4-164">Tento skript můžete spustit na jakýkoli počítač, který má správce přístup tooall hello počítače, které jsou označeny jako uzly v souboru konfigurace clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-164">This script can be run on any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster configuration file.</span></span> <span data-ttu-id="4a4c4-165">Hello počítač, který tento skript se spouští na nemá toobe součástí clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-165">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a><span data-ttu-id="4a4c4-166">Mezi shromažďovaná telemetrická data a jak tooopt mimo ho</span><span class="sxs-lookup"><span data-stu-id="4a4c4-166">Telemetry data collected and how tooopt out of it</span></span>
<span data-ttu-id="4a4c4-167">Ve výchozím hello produktu shromažďuje telemetrická data na hello Service Fabric využití tooimprove hello produktu.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-167">As a default, hello product collects telemetry on hello Service Fabric usage tooimprove hello product.</span></span> <span data-ttu-id="4a4c4-168">Hello Analyzátor osvědčených postupů, které běží jako součást instalace hello zkontroluje připojení příliš[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span><span class="sxs-lookup"><span data-stu-id="4a4c4-168">hello Best Practice Analyzer that runs as a part of hello setup checks for connectivity too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span></span> <span data-ttu-id="4a4c4-169">Pokud není dostupný, hello instalace se nezdaří, pokud vyjádření výslovného nesouhlasu telemetrie.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-169">If it is not reachable, hello setup fails unless you opt out of telemetry.</span></span>

1. <span data-ttu-id="4a4c4-170">Hello telemetrie kanálu pokusí tooupload hello následující data příliš[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) jednou denně.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-170">hello telemetry pipeline tries tooupload hello following data too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) once every day.</span></span> <span data-ttu-id="4a4c4-171">Je načtení typu best effort a nemá žádný vliv na fungování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-171">It is a best-effort upload and has no impact on hello cluster functionality.</span></span> <span data-ttu-id="4a4c4-172">telemetrie Hello je odeslán, pouze z uzlu hello, spustí hello primární Správce převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-172">hello telemetry is only sent from hello node that runs hello failover manager primary.</span></span> <span data-ttu-id="4a4c4-173">Žádné další uzly k odeslání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-173">No other nodes send out telemetry.</span></span>
2. <span data-ttu-id="4a4c4-174">telemetrie Hello se skládá z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="4a4c4-174">hello telemetry consists of hello following:</span></span>

* <span data-ttu-id="4a4c4-175">Počet služeb</span><span class="sxs-lookup"><span data-stu-id="4a4c4-175">Number of services</span></span>
* <span data-ttu-id="4a4c4-176">Počet ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="4a4c4-176">Number of ServiceTypes</span></span>
* <span data-ttu-id="4a4c4-177">Počet aplikací</span><span class="sxs-lookup"><span data-stu-id="4a4c4-177">Number of Applications</span></span>
* <span data-ttu-id="4a4c4-178">Počet ApplicationUpgrades</span><span class="sxs-lookup"><span data-stu-id="4a4c4-178">Number of ApplicationUpgrades</span></span>
* <span data-ttu-id="4a4c4-179">Počet FailoverUnits</span><span class="sxs-lookup"><span data-stu-id="4a4c4-179">Number of FailoverUnits</span></span>
* <span data-ttu-id="4a4c4-180">Počet InBuildFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="4a4c4-180">Number of InBuildFailoverUnits</span></span>
* <span data-ttu-id="4a4c4-181">Počet UnhealthyFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="4a4c4-181">Number of UnhealthyFailoverUnits</span></span>
* <span data-ttu-id="4a4c4-182">Počet replik</span><span class="sxs-lookup"><span data-stu-id="4a4c4-182">Number of Replicas</span></span>
* <span data-ttu-id="4a4c4-183">Počet InBuildReplicas</span><span class="sxs-lookup"><span data-stu-id="4a4c4-183">Number of InBuildReplicas</span></span>
* <span data-ttu-id="4a4c4-184">Počet StandByReplicas</span><span class="sxs-lookup"><span data-stu-id="4a4c4-184">Number of StandByReplicas</span></span>
* <span data-ttu-id="4a4c4-185">Počet OfflineReplicas</span><span class="sxs-lookup"><span data-stu-id="4a4c4-185">Number of OfflineReplicas</span></span>
* <span data-ttu-id="4a4c4-186">CommonQueueLength</span><span class="sxs-lookup"><span data-stu-id="4a4c4-186">CommonQueueLength</span></span>
* <span data-ttu-id="4a4c4-187">QueryQueueLength</span><span class="sxs-lookup"><span data-stu-id="4a4c4-187">QueryQueueLength</span></span>
* <span data-ttu-id="4a4c4-188">FailoverUnitQueueLength</span><span class="sxs-lookup"><span data-stu-id="4a4c4-188">FailoverUnitQueueLength</span></span>
* <span data-ttu-id="4a4c4-189">CommitQueueLength</span><span class="sxs-lookup"><span data-stu-id="4a4c4-189">CommitQueueLength</span></span>
* <span data-ttu-id="4a4c4-190">Počet uzlů</span><span class="sxs-lookup"><span data-stu-id="4a4c4-190">Number of Nodes</span></span>
* <span data-ttu-id="4a4c4-191">IsContextComplete: True nebo False</span><span class="sxs-lookup"><span data-stu-id="4a4c4-191">IsContextComplete: True/False</span></span>
* <span data-ttu-id="4a4c4-192">ClusterId: Toto je identifikátor GUID náhodně vygenerované pro každý cluster</span><span class="sxs-lookup"><span data-stu-id="4a4c4-192">ClusterId: This is a GUID randomly generated for each cluster</span></span>
* <span data-ttu-id="4a4c4-193">ServiceFabricVersion</span><span class="sxs-lookup"><span data-stu-id="4a4c4-193">ServiceFabricVersion</span></span>
* <span data-ttu-id="4a4c4-194">IP adresa hello virtuálního počítače nebo počítače, ze které hello je načtený telemetrie</span><span class="sxs-lookup"><span data-stu-id="4a4c4-194">IP address of hello virtual machine or machine from which hello telemetry is uploaded</span></span>

<span data-ttu-id="4a4c4-195">toodisable telemetrie, přidejte následující hello příliš*vlastnosti* v souboru config clusteru: *enableTelemetry: false*.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-195">toodisable telemetry, add hello following too*properties* in your cluster config: *enableTelemetry: false*.</span></span>

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a><span data-ttu-id="4a4c4-196">Funkce Preview obsažené v tomto balíčku</span><span class="sxs-lookup"><span data-stu-id="4a4c4-196">Preview features included in this package</span></span>
<span data-ttu-id="4a4c4-197">Žádné.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-197">None.</span></span>


> [!NOTE]
> <span data-ttu-id="4a4c4-198">Počínaje hello nové [verzí GA hello samostatné clusteru pro systém Windows Server (verze 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), toofuture verze vašeho clusteru, můžete upgradovat ručně nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-198">Starting with hello new [GA version of hello standalone cluster for Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), you can upgrade your cluster toofuture releases, manually or automatically.</span></span> <span data-ttu-id="4a4c4-199">Odkazovat příliš[upgradovat na verzi clusteru Service Fabric samostatné](service-fabric-cluster-upgrade-windows-server.md) v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4a4c4-199">Refer too[Upgrade a standalone Service Fabric cluster version](service-fabric-cluster-upgrade-windows-server.md) document for details.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4a4c4-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a4c4-200">Next steps</span></span>
* [<span data-ttu-id="4a4c4-201">Nasazení a odebírat aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a4c4-201">Deploy and remove applications using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="4a4c4-202">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="4a4c4-202">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="4a4c4-203">Přidat nebo odebrat cluster Service Fabric samostatné tooa uzly</span><span class="sxs-lookup"><span data-stu-id="4a4c4-203">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="4a4c4-204">Upgrade na verzi clusteru Service Fabric samostatné</span><span class="sxs-lookup"><span data-stu-id="4a4c4-204">Upgrade a standalone Service Fabric cluster version</span></span>](service-fabric-cluster-upgrade-windows-server.md)
* [<span data-ttu-id="4a4c4-205">Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="4a4c4-205">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [<span data-ttu-id="4a4c4-206">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="4a4c4-206">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="4a4c4-207">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="4a4c4-207">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
