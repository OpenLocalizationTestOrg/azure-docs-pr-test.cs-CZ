---
title: "aaaAdd nebo odebrat uzly tooa samostatný cluster Service Fabric | Microsoft Docs"
description: "Zjistěte, jak tooadd nebo odebrat uzly tooan Azure Service Fabric clusteru na fyzický nebo virtuální počítač se systémem Windows Server, který může být místní nebo v jakékoli cloudu."
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
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a><span data-ttu-id="8f878-103">Přidat nebo odebrat uzly tooa samostatný cluster Service Fabric systémem Windows Server</span><span class="sxs-lookup"><span data-stu-id="8f878-103">Add or remove nodes tooa standalone Service Fabric cluster running on Windows Server</span></span>
<span data-ttu-id="8f878-104">Až budete mít [vytvořen samostatný cluster Service Fabric na počítačích systému Windows Server](service-fabric-cluster-creation-for-windows-server.md), může změnit obchodních potřeb a může být nutné tooadd nebo odebrat uzly tooyour clusteru.</span><span class="sxs-lookup"><span data-stu-id="8f878-104">After you have [created your standalone Service Fabric cluster on Windows Server machines](service-fabric-cluster-creation-for-windows-server.md), your business needs may change and you might need tooadd or remove nodes tooyour cluster.</span></span> <span data-ttu-id="8f878-105">Tento článek obsahuje podrobné pokyny tooachieve to.</span><span class="sxs-lookup"><span data-stu-id="8f878-105">This article provides detailed steps tooachieve this.</span></span> <span data-ttu-id="8f878-106">Upozorňujeme, že přidat nebo odebrat uzel funkce není podporována v clusterech místní vývoj.</span><span class="sxs-lookup"><span data-stu-id="8f878-106">Please note that add/remove node functionality is not supported in local development clusters.</span></span>

## <a name="add-nodes-tooyour-cluster"></a><span data-ttu-id="8f878-107">Přidat uzly clusteru tooyour</span><span class="sxs-lookup"><span data-stu-id="8f878-107">Add nodes tooyour cluster</span></span>
1. <span data-ttu-id="8f878-108">Příprava hello virtuálního počítače nebo počítače chcete tooadd tooyour clusteru podle následujících kroků hello v hello [hello Příprava počítačů toomeet hello požadavky pro nasazení clusteru](service-fabric-cluster-creation-for-windows-server.md) části</span><span class="sxs-lookup"><span data-stu-id="8f878-108">Prepare hello VM/machine you want tooadd tooyour cluster by following hello steps mentioned in hello [Prepare hello machines toomeet hello prerequisites for cluster deployment](service-fabric-cluster-creation-for-windows-server.md) section</span></span>
2. <span data-ttu-id="8f878-109">Identifikovat, které domény selhání a upgradu domény jsou probíhající tooadd tohoto virtuálního počítače nebo počítače.</span><span class="sxs-lookup"><span data-stu-id="8f878-109">Identify which fault domain and upgrade domain you are going tooadd this VM/machine to</span></span>
3. <span data-ttu-id="8f878-110">Vzdálené plochy (RDP) do virtuálního počítače nebo počítače, které chcete tooadd toohello cluster hello</span><span class="sxs-lookup"><span data-stu-id="8f878-110">Remote desktop (RDP) into hello VM/machine that you want tooadd toohello cluster</span></span>
4. <span data-ttu-id="8f878-111">Kopírování nebo [stáhnout hello samostatný balíček pro Service Fabric pro systém Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello virtuálního počítače nebo počítače a rozbalte balíček hello</span><span class="sxs-lookup"><span data-stu-id="8f878-111">Copy or [download hello standalone package for Service Fabric for Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) toohello VM/machine and unzip hello package</span></span>
5. <span data-ttu-id="8f878-112">Spusťte prostředí Powershell se zvýšenými oprávněními a přejděte toohello umístění rozbalené balíčku hello</span><span class="sxs-lookup"><span data-stu-id="8f878-112">Run Powershell with elevated privileges, and navigate toohello location of hello unzipped package</span></span>
6. <span data-ttu-id="8f878-113">Spustit hello *AddNode.ps1* skriptu s parametry hello popisující nový uzel tooadd hello.</span><span class="sxs-lookup"><span data-stu-id="8f878-113">Run hello *AddNode.ps1* script with hello parameters describing hello new node tooadd.</span></span> <span data-ttu-id="8f878-114">Následující příklad Hello přidá nový uzel s názvem VM5, s typem NodeType0 a IP adres 182.17.34.52, do UD1 a fd: / dc1/r0.</span><span class="sxs-lookup"><span data-stu-id="8f878-114">hello example below adds a new node called VM5, with type NodeType0 and IP address 182.17.34.52, into UD1 and fd:/dc1/r0.</span></span> <span data-ttu-id="8f878-115">Hello *ExistingClusterConnectionEndPoint* je koncový bod připojení pro uzel již v hello stávajícího clusteru, může být IP adresa hello *žádné* uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8f878-115">hello *ExistingClusterConnectionEndPoint* is a connection endpoint for a node already in hello existing cluster, which can be hello IP address of *any* node in hello cluster.</span></span>

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    <span data-ttu-id="8f878-116">Po dokončení skriptu hello můžete zkontrolovat, zda byl přidán nový uzel hello spuštěním hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8f878-116">Once hello script finishes running, you can check if hello new node has been added by running hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet.</span></span>

7. <span data-ttu-id="8f878-117">tooensure konzistence mezi různými uzly v clusteru hello, je nutné inicializovat konfigurace upgradu.</span><span class="sxs-lookup"><span data-stu-id="8f878-117">tooensure consistency across different nodes in hello cluster, you must initiate a configuration upgrade.</span></span> <span data-ttu-id="8f878-118">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello nejnovější konfigurační soubor a přidejte hello příliš nově přidaná uzlu část "Uzlů".</span><span class="sxs-lookup"><span data-stu-id="8f878-118">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and add hello newly added node too"Nodes" section.</span></span> <span data-ttu-id="8f878-119">Je také vhodné tooalways mít hello nejnovější konfigurace clusteru k dispozici v případě hello, je nutné, aby tooredploy cluster s hello stejnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8f878-119">It is also recommended tooalways have hello latest cluster configuration available in hello case that you need tooredploy a cluster with hello same configuration.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. <span data-ttu-id="8f878-120">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="8f878-120">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="8f878-121">Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="8f878-121">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="8f878-122">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="8f878-122">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a><span data-ttu-id="8f878-123">Přidat uzly tooclusters nakonfigurováni pomocí gMSA zabezpečení systému Windows</span><span class="sxs-lookup"><span data-stu-id="8f878-123">Add nodes tooclusters configured with Windows Security using gMSA</span></span>
<span data-ttu-id="8f878-124">Pro clustery nakonfigurované skupiny spravované služby Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) můžete přidat nový uzel pomocí konfigurace upgradu:</span><span class="sxs-lookup"><span data-stu-id="8f878-124">For clusters configured with Group Managed Service Account(gMSA)(https://technet.microsoft.com/library/hh831782.aspx), a new node can be added using a configuration upgrade:</span></span>
1. <span data-ttu-id="8f878-125">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) na všech uzlech existujícího hello tooget hello nejnovější konfigurační soubor a přidat podrobnosti o novém uzlu hello chcete tooadd v části uzly"hello".</span><span class="sxs-lookup"><span data-stu-id="8f878-125">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) on any of hello existing nodes tooget hello latest configuration file and add details about hello new node you want tooadd in hello "Nodes" section.</span></span> <span data-ttu-id="8f878-126">Zajistěte, aby nový uzel hello je součástí systému hello stejné skupiny spravovaný účet.</span><span class="sxs-lookup"><span data-stu-id="8f878-126">Make sure hello new node is part of hello same group managed account.</span></span> <span data-ttu-id="8f878-127">Tento účet by měl být správce na všech počítačích.</span><span class="sxs-lookup"><span data-stu-id="8f878-127">This account should be an Administrator on all machines.</span></span>

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. <span data-ttu-id="8f878-128">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="8f878-128">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    <span data-ttu-id="8f878-129">Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="8f878-129">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="8f878-130">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="8f878-130">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

### <a name="add-node-types-tooyour-cluster"></a><span data-ttu-id="8f878-131">Přidání uzlu typy tooyour clusteru</span><span class="sxs-lookup"><span data-stu-id="8f878-131">Add node types tooyour cluster</span></span>
<span data-ttu-id="8f878-132">V pořadí tooadd nového typu uzlu, úpravě vaše konfigurace tooinclude hello nového typu uzlu v oddílu "NodeTypes" v části "Vlastnosti" a zahájit konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="8f878-132">In order tooadd a new node type, modify your configuration tooinclude hello new node type in "NodeTypes" section under "Properties" and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span> <span data-ttu-id="8f878-133">Po dokončení upgradu hello můžete přidat nový cluster tooyour uzly s tímto typem uzlu.</span><span class="sxs-lookup"><span data-stu-id="8f878-133">Once hello upgrade completes, you can add new nodes tooyour cluster with this node type.</span></span>

## <a name="remove-nodes-from-your-cluster"></a><span data-ttu-id="8f878-134">Odebrání uzlů z clusteru</span><span class="sxs-lookup"><span data-stu-id="8f878-134">Remove nodes from your cluster</span></span>
<span data-ttu-id="8f878-135">Uzel může být odebrán z clusteru pomocí upgrade konfigurace v hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8f878-135">A node can be removed from a cluster using a configuration upgrade, in hello following manner:</span></span>

1. <span data-ttu-id="8f878-136">Spustit [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello nejnovější konfigurační soubor a *odebrat* hello uzlu z části "Uzlů".</span><span class="sxs-lookup"><span data-stu-id="8f878-136">Run [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello latest configuration file and *remove* hello node from "Nodes" section.</span></span>
<span data-ttu-id="8f878-137">Přidat hello "NodesToBeRemoved" parametr příliš "nastavení" tématu v části "FabricSettings".</span><span class="sxs-lookup"><span data-stu-id="8f878-137">Add hello "NodesToBeRemoved" parameter too"Setup" section inside "FabricSettings" section.</span></span> <span data-ttu-id="8f878-138">Hello "value" by měl být čárkami oddělený seznam názvů uzlu uzlů, které je třeba toobe odebrat.</span><span class="sxs-lookup"><span data-stu-id="8f878-138">hello "value" should be a comma separated list of node names of nodes that need toobe removed.</span></span>

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
2. <span data-ttu-id="8f878-139">Spustit [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="8f878-139">Run [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello upgrade.</span></span>

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    <span data-ttu-id="8f878-140">Můžete sledovat průběh hello hello upgrade na Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="8f878-140">You can monitor hello progress of hello upgrade on Service Fabric Explorer.</span></span> <span data-ttu-id="8f878-141">Alternativně můžete spustit [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span><span class="sxs-lookup"><span data-stu-id="8f878-141">Alternatively, you can run [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)</span></span>

> [!NOTE]
> <span data-ttu-id="8f878-142">Odebrání uzlů může zahájit více upgradů.</span><span class="sxs-lookup"><span data-stu-id="8f878-142">Removal of nodes may initiate multiple upgrades.</span></span> <span data-ttu-id="8f878-143">Některé uzly jsou označené jako `IsSeedNode=”true”` značky a lze identifikovat podle dotazování hello clusteru manifestu pomocí `Get-ServiceFabricClusterManifest`.</span><span class="sxs-lookup"><span data-stu-id="8f878-143">Some nodes are marked with `IsSeedNode=”true”` tag and can be identified by querying hello cluster manifest using `Get-ServiceFabricClusterManifest`.</span></span> <span data-ttu-id="8f878-144">Odebrání těchto uzlů může trvat déle než jiné, vzhledem k tomu, že uzly počáteční hodnoty hello bude mít toobe přesunout v takových scénářů.</span><span class="sxs-lookup"><span data-stu-id="8f878-144">Removal of such nodes may take longer than others since hello seed nodes will have toobe moved around in such scenarios.</span></span> <span data-ttu-id="8f878-145">Hello clusteru musí zachovat minimálně 3 uzlů typu primárního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8f878-145">hello cluster must maintain a minimum of 3 primary node type nodes.</span></span>
> 
> 

### <a name="remove-node-types-from-your-cluster"></a><span data-ttu-id="8f878-146">Odebrat typy uzlů z clusteru</span><span class="sxs-lookup"><span data-stu-id="8f878-146">Remove node types from your cluster</span></span>
<span data-ttu-id="8f878-147">Před odebráním typ uzlu, prosím Překontrolujte, pokud jsou všechny uzly odkazující na typ uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="8f878-147">Before removing a node type, please double check if there are any nodes referencing hello node type.</span></span> <span data-ttu-id="8f878-148">Odeberte tyto uzly před odebráním hello odpovídající typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="8f878-148">Remove these nodes before removing hello corresponding node type.</span></span> <span data-ttu-id="8f878-149">Po odebrání všech odpovídajících uzlů můžete odebrat hello NodeType z hello konfiguraci clusteru a zahájit konfiguraci upgradovat pomocí [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="8f878-149">Once all corresponding nodes are removed, you can remove hello NodeType from hello cluster configuration and begin a configuration upgrade using [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).</span></span>


### <a name="replace-primary-nodes-of-your-cluster"></a><span data-ttu-id="8f878-150">Nahraďte primární uzlů ve vašem clusteru</span><span class="sxs-lookup"><span data-stu-id="8f878-150">Replace primary nodes of your cluster</span></span>
<span data-ttu-id="8f878-151">nahrazení Hello primární uzlů musí být provádí jeden uzel za druhým, místo odebráním a potom přidat v dávkách.</span><span class="sxs-lookup"><span data-stu-id="8f878-151">hello replacement of primary nodes should be performed one node after another, instead of removing and then adding in batches.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8f878-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f878-152">Next steps</span></span>
* [<span data-ttu-id="8f878-153">Nastavení konfigurace pro samostatný cluster Windows</span><span class="sxs-lookup"><span data-stu-id="8f878-153">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="8f878-154">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí X509 certifikáty</span><span class="sxs-lookup"><span data-stu-id="8f878-154">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)
* [<span data-ttu-id="8f878-155">Vytvořit cluster Service Fabric samostatné s virtuálními počítači Azure s Windows</span><span class="sxs-lookup"><span data-stu-id="8f878-155">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)

