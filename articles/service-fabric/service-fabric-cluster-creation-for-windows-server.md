---
title: "Vytvoření clusteru s podporou Azure Service Fabric samostatné | Microsoft Docs"
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
ms.openlocfilehash: 6aa2905a97ec6b8c125f2ab9572a8e40bf525b27
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a><span data-ttu-id="2f9c7-103">Vytvoření clusteru s podporou samostatné systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="2f9c7-103">Create a standalone cluster running on Windows Server</span></span>
<span data-ttu-id="2f9c7-104">Azure Service Fabric můžete použít k vytvoření clusterů Service Fabric na všechny virtuální počítače nebo počítače se systémem Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-104">You can use Azure Service Fabric to create Service Fabric clusters on any virtual machines or computers running Windows Server.</span></span> <span data-ttu-id="2f9c7-105">To znamená, můžete nasazovat a spouštět aplikace Service Fabric v jakémkoli prostředí, které obsahuje sadu vzájemně propojena počítačů Windows serveru, je-li jej v místním nebo se všechny poskytovatele cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-105">This means you can deploy and run Service Fabric applications in any environment that contains a set of interconnected Windows Server computers, be it on premises or with any cloud provider.</span></span> <span data-ttu-id="2f9c7-106">Service Fabric nabízí instalační balíček k vytvoření clusterů Service Fabric se nazývá samostatný balíček Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-106">Service Fabric provides a setup package to create Service Fabric clusters called the standalone Windows Server package.</span></span>

<span data-ttu-id="2f9c7-107">Tento článek vás provede kroky pro vytvoření samostatné clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-107">This article walks you through the steps for creating a Service Fabric standalone cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="2f9c7-108">Tento balíček samostatné systému Windows Server je dostupný a mohou být použity pro nasazení v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-108">This standalone Windows Server package is commercially available and may be used for production deployments.</span></span> <span data-ttu-id="2f9c7-109">Tento balíček může obsahovat nové funkce Service Fabric, které jsou v "Náhled".</span><span class="sxs-lookup"><span data-stu-id="2f9c7-109">This package may contain new Service Fabric features that are in "Preview".</span></span> <span data-ttu-id="2f9c7-110">Přejděte dolů k položce "[zobrazit náhled funkce obsažené v tomto balíčku](#previewfeatures_anchor)."</span><span class="sxs-lookup"><span data-stu-id="2f9c7-110">Scroll down to "[Preview features included in this package](#previewfeatures_anchor)."</span></span> <span data-ttu-id="2f9c7-111">části seznamu funkce verze preview.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-111">section for the list of the preview features.</span></span> <span data-ttu-id="2f9c7-112">Můžete [stáhnout kopii smlouvy EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nyní.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-112">You can [download a copy of the EULA](http://go.microsoft.com/fwlink/?LinkID=733084) now.</span></span>
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a><span data-ttu-id="2f9c7-113">Získat podporu pro balíček prostředků infrastruktury služby pro systém Windows Server</span><span class="sxs-lookup"><span data-stu-id="2f9c7-113">Get support for the Service Fabric for Windows Server package</span></span>
* <span data-ttu-id="2f9c7-114">Zeptejte se komunity pro systém Windows Server v o samostatný balíček Service Fabric [fórum Azure Service Fabric](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-114">Ask the community about the Service Fabric standalone package for Windows Server in the [Azure Service Fabric forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span></span>
* <span data-ttu-id="2f9c7-115">Otevřít lístek pro [Professional podporu pro Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-115">Open a ticket for [Professional Support for Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span></span>  <span data-ttu-id="2f9c7-116">Další informace o Professional podporu společnosti Microsoft [zde](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-116">Learn more about Professional Support from Microsoft [here](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span></span>
* <span data-ttu-id="2f9c7-117">Můžete také získat podporu pro tento balíček jako součást [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-117">You can also get support for this package as a part of [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span></span>
* <span data-ttu-id="2f9c7-118">Další podrobnosti najdete v tématu [možnosti podpory Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-118">For more details, please see [Azure Service Fabric support options](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span></span>
* <span data-ttu-id="2f9c7-119">Chcete-li shromažďovat protokoly pro účely podpory, spusťte [kolektor protokolů samostatné Service Fabric](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-119">To collect logs for support purposes, run the [Service Fabric Standalone Log collector](service-fabric-cluster-standalone-package-contents.md).</span></span>

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a><span data-ttu-id="2f9c7-120">Stáhněte si balíček prostředků infrastruktury služby pro systém Windows Server</span><span class="sxs-lookup"><span data-stu-id="2f9c7-120">Download the Service Fabric for Windows Server package</span></span>
<span data-ttu-id="2f9c7-121">Pokud chcete vytvořit cluster, použít balíček prostředků infrastruktury služby pro systém Windows Server (Windows Server 2012 R2 a novější) nachází tady:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-121">To create the cluster, use the Service Fabric for Windows Server package (Windows Server 2012 R2 and newer) found here:</span></span> <br><span data-ttu-id="2f9c7-122">
[Stáhněte si odkaz – samostatný balíček prostředků infrastruktury služby - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span><span class="sxs-lookup"><span data-stu-id="2f9c7-122">
[Download Link - Service Fabric Standalone Package - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span></span>

<span data-ttu-id="2f9c7-123">Najde podrobnosti o na obsah balíčku [zde](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-123">Find details on contents of the package [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="2f9c7-124">V době vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-124">The Service Fabric runtime package is automatically downloaded at time of cluster creation.</span></span> <span data-ttu-id="2f9c7-125">Pokud nasazení z počítače není připojený k Internetu, stáhněte balíček modulu runtime vzdálené správy z tohoto umístění:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-125">If deploying from a machine not connected to the internet, please download the runtime package out of band from here:</span></span> <br><span data-ttu-id="2f9c7-126">
[Stáhněte si odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span><span class="sxs-lookup"><span data-stu-id="2f9c7-126">
[Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span></span>

<span data-ttu-id="2f9c7-127">Najděte ukázky samostatné konfigurace clusteru na:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-127">Find Standalone Cluster Configuration samples at:</span></span> <br><span data-ttu-id="2f9c7-128">
[Konfigurace clusteru samostatné – ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span><span class="sxs-lookup"><span data-stu-id="2f9c7-128">
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span></span>

<a id="createcluster"></a>

## <a name="create-the-cluster"></a><span data-ttu-id="2f9c7-129">Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="2f9c7-129">Create the cluster</span></span>
<span data-ttu-id="2f9c7-130">Service Fabric lze nasadit do clusteru s podporou vývoj pro jeden počítač pomocí *ClusterConfig.Unsecure.DevCluster.json* zahrnutý v [ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-130">Service Fabric can be deployed to a one-machine development cluster by using the *ClusterConfig.Unsecure.DevCluster.json* file included in [Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span></span>

<span data-ttu-id="2f9c7-131">Rozbalte balíček samostatné k počítači, zkopírujte vzorový konfigurační soubor do místního počítače, a pak spustit *CreateServiceFabricCluster.ps1* skriptu prostřednictvím relaci prostředí PowerShell správce ze složky balíčku samostatné:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-131">Unpack the standalone package to your machine, copy the sample config file to the local machine, then run the *CreateServiceFabricCluster.ps1* script through an administrator PowerShell session, from the standalone package folder:</span></span>
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a><span data-ttu-id="2f9c7-132">Krok 1A: vytvoření zabezpečená místního vývojového clusteru</span><span class="sxs-lookup"><span data-stu-id="2f9c7-132">Step 1A: Create an unsecured local development cluster</span></span>
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="2f9c7-133">Najdete v části nastavení prostředí na [plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md) pro řešení potíží s podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-133">See the Environment Setup section at [Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md) for troubleshooting details.</span></span>

<span data-ttu-id="2f9c7-134">Pokud dokončíte spuštěné vývojové scénáře, můžete odebrat cluster Service Fabric z počítače tím, že odkazuje na postup v části "[odebrat cluster](#removecluster_anchor)".</span><span class="sxs-lookup"><span data-stu-id="2f9c7-134">If you're finished running development scenarios, you can remove the Service Fabric cluster from the machine by referring to steps in section "[Remove a cluster](#removecluster_anchor)".</span></span> 

### <a name="step-1b-create-a-multi-machine-cluster"></a><span data-ttu-id="2f9c7-135">Krok 1B: vytvoření clusteru s více počítači</span><span class="sxs-lookup"><span data-stu-id="2f9c7-135">Step 1B: Create a multi-machine cluster</span></span>
<span data-ttu-id="2f9c7-136">Poté, co jste prošli plánování a přípravné kroky popsané v následujícím odkazu, budete chtít vytvořit cluster produkční pomocí konfiguračního souboru clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-136">After you have gone through the planning and preparation steps detailed at the below link, you are ready to create your production cluster using your cluster configuration file.</span></span> <br><span data-ttu-id="2f9c7-137">
[Plánování a příprava nasazení clusteru](service-fabric-cluster-standalone-deployment-preparation.md)</span><span class="sxs-lookup"><span data-stu-id="2f9c7-137">
[Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md)</span></span>

1. <span data-ttu-id="2f9c7-138">Ověřit konfigurační soubor, který jste napsali spuštěním *TestConfiguration.ps1* skript ve složce samostatný balíček:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-138">Validate the configuration file you have written by running the *TestConfiguration.ps1* script from the standalone package folder:</span></span>  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    <span data-ttu-id="2f9c7-139">Zobrazený výstup by měl jako níže.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-139">You should see output like below.</span></span> <span data-ttu-id="2f9c7-140">Pokud pole dolní "Předání" se vrátí jako "PRAVDA", mají zkontrolovány správností a cluster vypadá jako nasadit na základě vstupních konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-140">If the bottom field "Passed" is returned as "True", sanity checks have passed and the cluster looks to be deployable based on the input configuration.</span></span>

    ```
    Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
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

2. <span data-ttu-id="2f9c7-141">Vytvoření clusteru: spuštění *CreateServiceFabricCluster.ps1* skriptu nasazení clusteru Service Fabric v rámci každého počítače v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-141">Create the cluster:  Run the *CreateServiceFabricCluster.ps1* script to deploy the Service Fabric cluster across each machine in the configuration.</span></span> 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> <span data-ttu-id="2f9c7-142">Trasování nasazení se zapisují do virtuálního počítače nebo počítače, na kterém jste spustili skript prostředí PowerShell CreateServiceFabricCluster.ps1.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-142">Deployment traces are written to the VM/machine on which you ran the CreateServiceFabricCluster.ps1 PowerShell script.</span></span> <span data-ttu-id="2f9c7-143">Ty lze najít v podsložce DeploymentTraces, na základě v adresáři, ze kterého se spouštěl skript.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-143">These can be found in the subfolder DeploymentTraces, based in the directory from which the script was run.</span></span> <span data-ttu-id="2f9c7-144">Pokud chcete zobrazit, pokud se k počítači správně nasazena Service Fabric, najít nainstalované soubory v adresáři FabricDataRoot adresáři, podle popisu v části FabricSettings clusteru konfigurační soubor (ve výchozím nastavení c:\ProgramData\SF).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-144">To see if Service Fabric was deployed correctly to a machine, find the installed files in the FabricDataRoot directory, as detailed in the cluster configuration file FabricSettings section (by default c:\ProgramData\SF).</span></span> <span data-ttu-id="2f9c7-145">FabricHost.exe a Fabric.exe procesy je možné taky zobrazit spuštěná ve Správci úloh.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-145">As well, FabricHost.exe and Fabric.exe processes can be seen running in Task Manager.</span></span>
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a><span data-ttu-id="2f9c7-146">Krok 1C: vytvoření clusteru do režimu offline (internet odpojení)</span><span class="sxs-lookup"><span data-stu-id="2f9c7-146">Step 1C: Create an offline (internet-disconnected) cluster</span></span>
<span data-ttu-id="2f9c7-147">Při vytváření clusteru automaticky stažení balíčku modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-147">The Service Fabric runtime package is automatically downloaded at cluster creation.</span></span> <span data-ttu-id="2f9c7-148">Při nasazení v clusteru s podporou počítače nejsou připojené k Internetu, musíte stáhnout balíček modulu runtime Service Fabric samostatně a zadat cestu k němu při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-148">When deploying a cluster to machines not connected to the internet, you will need to download the Service Fabric runtime package separately, and provide the path to it at cluster creation.</span></span>
<span data-ttu-id="2f9c7-149">Balíček modulu runtime lze stáhnout samostatně z jiného počítače připojené k Internetu, v [stáhnout odkaz - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-149">The runtime package can be downloaded separately, from another machine connected to the internet, at [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span></span> <span data-ttu-id="2f9c7-150">Zkopírujte balíček modulu runtime kde nasazujete offline clusteru a vytvoření clusteru spuštěním `CreateServiceFabricCluster.ps1` s `-FabricRuntimePackagePath` parametr zahrnuty, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-150">Copy the runtime package to where you are deploying the offline cluster from, and create the cluster by running `CreateServiceFabricCluster.ps1` with the `-FabricRuntimePackagePath` parameter included, as shown below:</span></span> 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
<span data-ttu-id="2f9c7-151">kde `.\ClusterConfig.json` a `.\MicrosoftAzureServiceFabric.cab` jsou cesty k konfiguraci clusteru a soubor .cab do modulu runtime v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-151">where `.\ClusterConfig.json` and `.\MicrosoftAzureServiceFabric.cab` are the paths to the cluster configuration and the runtime .cab file respectively.</span></span>


### <a name="step-2-connect-to-the-cluster"></a><span data-ttu-id="2f9c7-152">Krok 2: Připojte se ke clusteru</span><span class="sxs-lookup"><span data-stu-id="2f9c7-152">Step 2: Connect to the cluster</span></span>
<span data-ttu-id="2f9c7-153">Pokud chcete připojit ke clusteru s podporou zabezpečení, najdete v části [ke zabezpečení clusteru Service fabric připojit](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-153">To connect to a secure cluster, see [Service fabric connect to secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="2f9c7-154">Pro připojení k nezabezpečená clusteru, spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-154">To connect to an unsecure cluster, run the following PowerShell command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
<span data-ttu-id="2f9c7-155">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-155">Example:</span></span>
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a><span data-ttu-id="2f9c7-156">Krok 3: Zprovoznit Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="2f9c7-156">Step 3: Bring up Service Fabric Explorer</span></span>
<span data-ttu-id="2f9c7-157">Teď můžete připojit ke clusteru pomocí Service Fabric Exploreru buď přímo z jednoho z počítačů s http://localhost:19080/Explorer/index.html nebo vzdáleně http://<*IPAddressofaMachine*>: 19080/Explorer/index.html.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-157">Now you can connect to the cluster with Service Fabric Explorer either directly from one of the machines with http://localhost:19080/Explorer/index.html or remotely with http://<*IPAddressofaMachine*>:19080/Explorer/index.html.</span></span>

## <a name="add-and-remove-nodes"></a><span data-ttu-id="2f9c7-158">Přidání a odebrání uzlů</span><span class="sxs-lookup"><span data-stu-id="2f9c7-158">Add and remove nodes</span></span>
<span data-ttu-id="2f9c7-159">Můžete přidávat nebo odebírat uzly do clusteru Service Fabric samostatné, podle potřeb organizace změnu.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-159">You can add or remove nodes to your standalone Service Fabric cluster as your business needs change.</span></span> <span data-ttu-id="2f9c7-160">V tématu [přidávat nebo odebírat uzly do clusteru Service Fabric samostatné](service-fabric-cluster-windows-server-add-remove-nodes.md) podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-160">See [Add or Remove nodes to a Service Fabric standalone cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) for detailed steps.</span></span>

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a><span data-ttu-id="2f9c7-161">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="2f9c7-161">Remove a cluster</span></span>
<span data-ttu-id="2f9c7-162">Chcete-li cluster odebrat, spusťte skript *RemoveServiceFabricCluster.ps1* prostředí PowerShell ze složky balíčku a předejte mu cestu ke konfiguračnímu souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-162">To remove a cluster, run the *RemoveServiceFabricCluster.ps1* PowerShell script from the package folder and pass in the path to the JSON configuration file.</span></span> <span data-ttu-id="2f9c7-163">Volitelně můžete určit umístění pro protokol odstranění.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-163">You can optionally specify a location for the log of the deletion.</span></span>

<span data-ttu-id="2f9c7-164">Tento skript můžete spustit na jakýkoli počítač, který má práva správce pro všechny počítače, které jsou označeny jako uzly v souboru konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-164">This script can be run on any machine that has administrator access to all the machines that are listed as nodes in the cluster configuration file.</span></span> <span data-ttu-id="2f9c7-165">Na počítač, který tento skript je spuštěn na nemá být součástí clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-165">The machine that this script is run on does not have to be part of the cluster.</span></span>

```
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a><span data-ttu-id="2f9c7-166">Mezi shromažďovaná telemetrická data a jak pro vyjádření výslovného nesouhlasu se</span><span class="sxs-lookup"><span data-stu-id="2f9c7-166">Telemetry data collected and how to opt out of it</span></span>
<span data-ttu-id="2f9c7-167">Ve výchozím produktu shromažďuje telemetrická data na Service Fabric použití k vylepšení produktu.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-167">As a default, the product collects telemetry on the Service Fabric usage to improve the product.</span></span> <span data-ttu-id="2f9c7-168">Analyzátor osvědčených postupů, který běží jako součást instalace zkontroluje připojení k [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span><span class="sxs-lookup"><span data-stu-id="2f9c7-168">The Best Practice Analyzer that runs as a part of the setup checks for connectivity to [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span></span> <span data-ttu-id="2f9c7-169">Pokud není dostupný, instalace se nezdaří, pokud vyjádření výslovného nesouhlasu telemetrie.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-169">If it is not reachable, the setup fails unless you opt out of telemetry.</span></span>

1. <span data-ttu-id="2f9c7-170">Kanál telemetrie se pokusí odeslat následující data, která mají [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) jednou denně.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-170">The telemetry pipeline tries to upload the following data to [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) once every day.</span></span> <span data-ttu-id="2f9c7-171">Je načtení typu best effort a nemá žádný vliv na fungování clusteru.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-171">It is a best-effort upload and has no impact on the cluster functionality.</span></span> <span data-ttu-id="2f9c7-172">Telemetrie je odeslán, pouze z uzlu, který běží převzetí služeb při selhání primární správce.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-172">The telemetry is only sent from the node that runs the failover manager primary.</span></span> <span data-ttu-id="2f9c7-173">Žádné další uzly k odeslání telemetrie.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-173">No other nodes send out telemetry.</span></span>
2. <span data-ttu-id="2f9c7-174">Telemetrie se skládá z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2f9c7-174">The telemetry consists of the following:</span></span>

* <span data-ttu-id="2f9c7-175">Počet služeb</span><span class="sxs-lookup"><span data-stu-id="2f9c7-175">Number of services</span></span>
* <span data-ttu-id="2f9c7-176">Počet ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="2f9c7-176">Number of ServiceTypes</span></span>
* <span data-ttu-id="2f9c7-177">Počet aplikací</span><span class="sxs-lookup"><span data-stu-id="2f9c7-177">Number of Applications</span></span>
* <span data-ttu-id="2f9c7-178">Počet ApplicationUpgrades</span><span class="sxs-lookup"><span data-stu-id="2f9c7-178">Number of ApplicationUpgrades</span></span>
* <span data-ttu-id="2f9c7-179">Počet FailoverUnits</span><span class="sxs-lookup"><span data-stu-id="2f9c7-179">Number of FailoverUnits</span></span>
* <span data-ttu-id="2f9c7-180">Počet InBuildFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="2f9c7-180">Number of InBuildFailoverUnits</span></span>
* <span data-ttu-id="2f9c7-181">Počet UnhealthyFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="2f9c7-181">Number of UnhealthyFailoverUnits</span></span>
* <span data-ttu-id="2f9c7-182">Počet replik</span><span class="sxs-lookup"><span data-stu-id="2f9c7-182">Number of Replicas</span></span>
* <span data-ttu-id="2f9c7-183">Počet InBuildReplicas</span><span class="sxs-lookup"><span data-stu-id="2f9c7-183">Number of InBuildReplicas</span></span>
* <span data-ttu-id="2f9c7-184">Počet StandByReplicas</span><span class="sxs-lookup"><span data-stu-id="2f9c7-184">Number of StandByReplicas</span></span>
* <span data-ttu-id="2f9c7-185">Počet OfflineReplicas</span><span class="sxs-lookup"><span data-stu-id="2f9c7-185">Number of OfflineReplicas</span></span>
* <span data-ttu-id="2f9c7-186">CommonQueueLength</span><span class="sxs-lookup"><span data-stu-id="2f9c7-186">CommonQueueLength</span></span>
* <span data-ttu-id="2f9c7-187">QueryQueueLength</span><span class="sxs-lookup"><span data-stu-id="2f9c7-187">QueryQueueLength</span></span>
* <span data-ttu-id="2f9c7-188">FailoverUnitQueueLength</span><span class="sxs-lookup"><span data-stu-id="2f9c7-188">FailoverUnitQueueLength</span></span>
* <span data-ttu-id="2f9c7-189">CommitQueueLength</span><span class="sxs-lookup"><span data-stu-id="2f9c7-189">CommitQueueLength</span></span>
* <span data-ttu-id="2f9c7-190">Počet uzlů</span><span class="sxs-lookup"><span data-stu-id="2f9c7-190">Number of Nodes</span></span>
* <span data-ttu-id="2f9c7-191">IsContextComplete: True nebo False</span><span class="sxs-lookup"><span data-stu-id="2f9c7-191">IsContextComplete: True/False</span></span>
* <span data-ttu-id="2f9c7-192">ClusterId: Toto je identifikátor GUID náhodně vygenerované pro každý cluster</span><span class="sxs-lookup"><span data-stu-id="2f9c7-192">ClusterId: This is a GUID randomly generated for each cluster</span></span>
* <span data-ttu-id="2f9c7-193">ServiceFabricVersion</span><span class="sxs-lookup"><span data-stu-id="2f9c7-193">ServiceFabricVersion</span></span>
* <span data-ttu-id="2f9c7-194">IP adresu virtuálního počítače nebo počítače, ze kterého se nahraje telemetrii</span><span class="sxs-lookup"><span data-stu-id="2f9c7-194">IP address of the virtual machine or machine from which the telemetry is uploaded</span></span>

<span data-ttu-id="2f9c7-195">Zakázat telemetrii, přidejte následující *vlastnosti* v souboru config clusteru: *enableTelemetry: false*.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-195">To disable telemetry, add the following to *properties* in your cluster config: *enableTelemetry: false*.</span></span>

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a><span data-ttu-id="2f9c7-196">Funkce Preview obsažené v tomto balíčku</span><span class="sxs-lookup"><span data-stu-id="2f9c7-196">Preview features included in this package</span></span>
<span data-ttu-id="2f9c7-197">Žádné.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-197">None.</span></span>


> [!NOTE]
> <span data-ttu-id="2f9c7-198">Počínaje nové [verzí GA samostatné clusteru pro systém Windows Server (verze 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), můžete provést upgrade clusteru na budoucích verzích ručně nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-198">Starting with the new [GA version of the standalone cluster for Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), you can upgrade your cluster to future releases, manually or automatically.</span></span> <span data-ttu-id="2f9c7-199">Odkazovat na [upgradovat na verzi clusteru Service Fabric samostatné](service-fabric-cluster-upgrade-windows-server.md) v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2f9c7-199">Refer to [Upgrade a standalone Service Fabric cluster version](service-fabric-cluster-upgrade-windows-server.md) document for details.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2f9c7-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f9c7-200">Next steps</span></span>
* [<span data-ttu-id="2f9c7-201">Nasazení a odebírat aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f9c7-201">Deploy and remove applications using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="2f9c7-202">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="2f9c7-202">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="2f9c7-203">Přidávat nebo odebírat uzly do clusteru s podporou samostatné Service Fabric</span><span class="sxs-lookup"><span data-stu-id="2f9c7-203">Add or remove nodes to a standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="2f9c7-204">Upgrade na verzi clusteru Service Fabric samostatné</span><span class="sxs-lookup"><span data-stu-id="2f9c7-204">Upgrade a standalone Service Fabric cluster version</span></span>](service-fabric-cluster-upgrade-windows-server.md)
* [<span data-ttu-id="2f9c7-205">Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="2f9c7-205">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [<span data-ttu-id="2f9c7-206">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="2f9c7-206">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="2f9c7-207">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="2f9c7-207">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
