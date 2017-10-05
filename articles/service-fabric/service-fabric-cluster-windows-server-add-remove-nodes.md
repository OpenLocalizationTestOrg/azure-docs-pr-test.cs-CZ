---
title: "Přidávat nebo odebírat uzly do clusteru Service Fabric samostatné | Microsoft Docs"
description: "Naučte se přidávat nebo odebírat uzly do clusteru Azure Service Fabric na fyzický nebo virtuální počítač se systémem Windows Server, který může být místní nebo v jakékoli cloudu."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 9c6035e97de38ff63ef074109afd9f3c7484f828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="ebaaa-103">Přidávat nebo odebírat uzly do clusteru Service Fabric samostatné systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="ebaaa-103">Add or remove nodes to a standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="ebaaa-104">Až budete mít [vytvořen samostatný cluster Service Fabric na počítačích systému Windows Server](service-fabric-cluster-creation-for-windows-server.md), může vaše obchodní potřeby změnit a možná budete muset přidávat nebo odebírat uzly do clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need to add or remove nodes to your cluster.</span></span> <span data-ttu-id="ebaaa-105">Tento článek obsahuje podrobné pokyny k dosažení tohoto cíle.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-105">This article provides detailed steps to achieve this.</span></span> <span data-ttu-id="ebaaa-106">Upozorňujeme, že přidat nebo odebrat uzel funkce není podporována v clusterech místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-to-your-cluster"></a><span data-ttu-id="ebaaa-107">Přidat uzly do clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-107">Add nodes to your cluster</span></span>
1. <span data-ttu-id="ebaaa-108">Příprava virtuálního počítače nebo počítače, který chcete přidat do clusteru pomocí následujících kroků v [Příprava počítačů ke splnění předpokladů pro nasazení clusteru](service-fabric-cluster-creation-for-windows-server.md) části</span><span class="sxs-lookup"><span data-stu-id="ebaaa-108">Prepare the VM/machine you want to add to your cluster by following the steps mentioned in the [Prepare the machines to meet the prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="ebaaa-109">Identifikovat, které domény selhání a chcete přidat tento virtuální počítač nebo počítač k doméně pro upgrade</span><span class="sxs-lookup"><span data-stu-id="ebaaa-109">Identify which fault domain and upgrade domain you are going to add this VM/machine to</span></span>
3. <span data-ttu-id="ebaaa-110">Vzdálené plochy (RDP) do virtuálního počítače nebo počítač, který chcete přidat do clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-110">Remote desktop (RDP) into the VM/machine that you want to add to the cluster</span></span>
4. <span data-ttu-id="ebaaa-111">Kopírování nebo [stáhnout samostatný balíček pro prostředky infrastruktury služby pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) do virtuálního počítače nebo počítače a rozbalte balíček</span><span class="sxs-lookup"><span data-stu-id="ebaaa-111">Copy or [download the standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) to the VM/machine and unzip the package</span></span>
5. <span data-ttu-id="ebaaa-112">Spusťte prostředí Powershell se zvýšenými oprávněními a přejděte do umístění rozbalené balíčku</span><span class="sxs-lookup"><span data-stu-id="ebaaa-112">Run Powershell with elevated privileges, and navigate to the location of the unzipped package</span></span>
6. <span data-ttu-id="ebaaa-113">Spustit *AddNode.ps1* skriptu s parametry popisující přidat nový uzel.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-113">Run the *AddNode.ps1* script with the parameters describing the new node to add.</span></span> <span data-ttu-id="ebaaa-114">Následující příklad přidá nový uzel s názvem VM5, s typem NodeType0 a IP adres 182.17.34.52, do UD1 a fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-114">The example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="ebaaa-115">*ExistingClusterConnectionEndPoint* je koncový bod připojení pro uzel již v existujícím clusteru, který může být IP adresa *žádné* uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-115">The *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in the existing cluster, which can be the IP address of *any* node in the cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="ebaaa-116">Po dokončení skriptu můžete zkontrolovat, zda byl přidán nový uzel spuštěním [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-116">Once the script finishes running, you can check if the new node has been added by running the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="ebaaa-117">K zachování konzistence napříč různými uzly v clusteru, je nutné inicializovat konfigurace upgradu.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-117">To ensure consistency across different nodes in the cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="ebaaa-118">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) získat nejnovější konfigurační soubor a přidejte uzlu nově přidané do části "Uzlů".</span><span class="sxs-lookup"><span data-stu-id="ebaaa-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and add the newly added node to "Nodes" section.</span></span> <span data-ttu-id="ebaaa-119">Doporučujeme také mít vždy k dispozici v případě, že je potřeba provést redploy clusteru se stejnou konfigurací nejnovější konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-119">It is also recommended to always have the latest cluster configuration available in the case that you need to redploy a cluster with the same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="ebaaa-120">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) spustíte upgrade.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="ebaaa-121">Můžete sledovat průběh upgradu v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-121">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="ebaaa-122">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="ebaaa-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="ebaaa-123">Přidat uzly do clusterů, které jsou nakonfigurované pomocí gMSA zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="ebaaa-123">Add nodes to clusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="ebaaa-124">Pro clustery nakonfigurované skupiny spravované služby Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) můžete přidat nový uzel pomocí konfigurace upgradu:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="ebaaa-125">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) na žádném z existujících uzlů získat nejnovější konfigurační soubor a přidat podrobnosti o nový uzel, který chcete přidat v části "Uzlů".</span><span class="sxs-lookup"><span data-stu-id="ebaaa-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of the existing nodes to get the latest configuration file and add details about the new node you want to add in the "Nodes" section.</span></span> <span data-ttu-id="ebaaa-126">Zajistěte, aby byl nový uzel součástí stejného účtu skupiny spravované.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-126">Make sure the new node is part of the same group managed account.</span></span> <span data-ttu-id="ebaaa-127">Tento účet by měl být správce na všech počítačích.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="ebaaa-128">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) spustíte upgrade.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    <span data-ttu-id="ebaaa-129">Můžete sledovat průběh upgradu v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-129">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="ebaaa-130">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="ebaaa-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-to-your-cluster"></a><span data-ttu-id="ebaaa-131">Přidat typy uzlů do clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-131">Add node types to your cluster</span></span>
<span data-ttu-id="ebaaa-132">Chcete-li přidat nového typu uzlu, změňte konfiguraci zahrnout nového typu uzlu v "NodeTypes" tématu v části "Vlastnosti" a začněte konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="ebaaa-132">In order to add a new node type, modify your configuration to include the new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="ebaaa-133">Po dokončení upgradu, můžete přidat nové uzly do clusteru s tímto typem uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-133">Once the upgrade completes, you can add new nodes to your cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="ebaaa-134">Odebrání uzlů z clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="ebaaa-135">Uzel může být odebrán z clusteru pomocí konfigurace upgrade, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-135">A node can be removed from a cluster using a configuration upgrade, in the following manner:</span></span>

1. <span data-ttu-id="ebaaa-136">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) získat nejnovější konfigurační soubor a *odebrat* uzlu z části "Uzlů".</span><span class="sxs-lookup"><span data-stu-id="ebaaa-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) to get the latest configuration file and *remove* the node from "Nodes" section.</span></span>
<span data-ttu-id="ebaaa-137">Parametr "NodesToBeRemoved" přidáte do části "Nastavení" v části "FabricSettings".</span><span class="sxs-lookup"><span data-stu-id="ebaaa-137">Add the "NodesToBeRemoved" parameter to "Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="ebaaa-138">"value" by měl být čárkami oddělený seznam názvů uzlu uzlů, které je třeba je odebrat.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-138">The "value" should be a comma separated list of node names of nodes that need to be removed.</span></span>

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. <span data-ttu-id="ebaaa-139">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) spustíte upgrade.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) to begin the upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    <span data-ttu-id="ebaaa-140">Můžete sledovat průběh upgradu v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-140">You can monitor the progress of the upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="ebaaa-141">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="ebaaa-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="ebaaa-142">Odebrání uzlů může zahájit více upgradů.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="ebaaa-143">Některé uzly jsou označené jako `IsSeedNode=”true”` značky a lze identifikovat podle dotazování clusteru manifestu pomocí `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying the cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="ebaaa-144">Odebrání těchto uzlů může trvat déle než jiné, protože uzly počáteční hodnoty musí přesunout v těchto scénářích.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-144">Removal of such nodes may take longer than others since the seed nodes will have to be moved around in such scenarios.</span></span> <span data-ttu-id="ebaaa-145">Cluster musí zachovat minimálně 3 uzlů typu primárního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-145">The cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="ebaaa-146">Odebrat typy uzlů z clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-146">Remove node types from your cluster</span></span>
<span data-ttu-id="ebaaa-147">Před odebráním typ uzlu, prosím Překontrolujte, pokud jsou všechny uzly odkazující na typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-147">Before removing a node type, please double check if there are any nodes referencing the node type.</span></span> <span data-ttu-id="ebaaa-148">Odeberte tyto uzly před odebráním odpovídající typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-148">Remove these nodes before removing the corresponding node type.</span></span> <span data-ttu-id="ebaaa-149">Po odebrání všech odpovídajících uzlů můžete odebrat typ uzlu z clusteru konfigurace a zahájit konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="ebaaa-149">Once all corresponding nodes are removed, you can remove the NodeType from the cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="ebaaa-150">Nahraďte primární uzlů ve vašem clusteru</span><span class="sxs-lookup"><span data-stu-id="ebaaa-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="ebaaa-151">Nahrazení primární uzly by měl být provádí jeden uzel za druhým, místo odebráním a potom přidat v dávkách.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-151">The replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ebaaa-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebaaa-152">Next steps</span></span>
* [<span data-ttu-id="ebaaa-153">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="ebaaa-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="ebaaa-154">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="ebaaa-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="ebaaa-155">Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="ebaaa-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

