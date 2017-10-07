---
title: aaaAzure aplikace Service Fabric oprava orchestration | Microsoft Docs
description: "Operační systém opravy na cluster Service Fabric tooautomate aplikace."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a><span data-ttu-id="a497d-103">Oprava operačního systému Windows hello v clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a497d-103">Patch hello Windows operating system in your Service Fabric cluster</span></span>

<span data-ttu-id="a497d-104">Hello oprava orchestration aplikace je aplikace Azure Service Fabric, která automatizuje operačního systému opravy na cluster Service Fabric v Azure bez výpadků.</span><span class="sxs-lookup"><span data-stu-id="a497d-104">hello patch orchestration application is an Azure Service Fabric application that automates operating system patching on a Service Fabric cluster on Azure without downtime.</span></span>

<span data-ttu-id="a497d-105">Hello oprava orchestration aplikaci poskytuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="a497d-105">hello patch orchestration app provides hello following:</span></span>

- <span data-ttu-id="a497d-106">**Automatická instalace operačního systému aktualizaci**.</span><span class="sxs-lookup"><span data-stu-id="a497d-106">**Automatic operating system update installation**.</span></span> <span data-ttu-id="a497d-107">Aktualizace operačního systému se automaticky stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="a497d-107">Operating system updates are automatically downloaded and installed.</span></span> <span data-ttu-id="a497d-108">Uzly se restartují podle potřeby a bez výpadku clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-108">Cluster nodes are rebooted as needed without cluster downtime.</span></span>

- <span data-ttu-id="a497d-109">**Clustery opravy a stavu integrace**.</span><span class="sxs-lookup"><span data-stu-id="a497d-109">**Cluster-aware patching and health integration**.</span></span> <span data-ttu-id="a497d-110">Při použití aktualizací, monitoruje hello oprava orchestration aplikace hello stav hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-110">While applying updates, hello patch orchestration app monitors hello health of hello cluster nodes.</span></span> <span data-ttu-id="a497d-111">Upgradovaná jeden uzel nebo jednu upgradovací doménu jsou uzly clusteru současně.</span><span class="sxs-lookup"><span data-stu-id="a497d-111">Cluster nodes are upgraded one node at a time or one upgrade domain at a time.</span></span> <span data-ttu-id="a497d-112">Pokud stav hello hello clusteru ocitne mimo provoz z důvodu toohello oprav procesu, opravy je zastaven tooprevent zhorší hello problém.</span><span class="sxs-lookup"><span data-stu-id="a497d-112">If hello health of hello cluster goes down due toohello patching process, patching is stopped tooprevent aggravating hello problem.</span></span>

## <a name="internal-details-of-hello-app"></a><span data-ttu-id="a497d-113">Interní podrobnosti aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-113">Internal details of hello app</span></span>

<span data-ttu-id="a497d-114">Hello oprava orchestration aplikace se skládá z hello následující dílčí součásti:</span><span class="sxs-lookup"><span data-stu-id="a497d-114">hello patch orchestration app is composed of hello following subcomponents:</span></span>

- <span data-ttu-id="a497d-115">**Služba Koordinátor**: Tato stavová služba je zodpovědná za:</span><span class="sxs-lookup"><span data-stu-id="a497d-115">**Coordinator Service**: This stateful service is responsible for:</span></span>
    - <span data-ttu-id="a497d-116">Úloha aktualizace Windows hello na celý cluster hello spolupráci.</span><span class="sxs-lookup"><span data-stu-id="a497d-116">Coordinating hello Windows Update job on hello entire cluster.</span></span>
    - <span data-ttu-id="a497d-117">Ukládání hello výsledek dokončené operace aplikace Windows Update.</span><span class="sxs-lookup"><span data-stu-id="a497d-117">Storing hello result of completed Windows Update operations.</span></span>
- <span data-ttu-id="a497d-118">**Služba agenta uzlu**: Tento bezstavové služby běží na všech uzlech clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a497d-118">**Node Agent Service**: This stateless service runs on all Service Fabric cluster nodes.</span></span> <span data-ttu-id="a497d-119">Hello služba je zodpovědná za:</span><span class="sxs-lookup"><span data-stu-id="a497d-119">hello service is responsible for:</span></span>
    - <span data-ttu-id="a497d-120">Zavedení spouštěcího programu hello NTService agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-120">Bootstrapping hello Node Agent NTService.</span></span>
    - <span data-ttu-id="a497d-121">Monitorování hello NTService agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-121">Monitoring hello Node Agent NTService.</span></span>
- <span data-ttu-id="a497d-122">**Uzel agenta NTService**: Tento systém Windows NT služba běží na vyšší úrovni oprávnění (systém).</span><span class="sxs-lookup"><span data-stu-id="a497d-122">**Node Agent NTService**: This Windows NT service runs at a higher-level privilege (SYSTEM).</span></span> <span data-ttu-id="a497d-123">Naproti tomu hello Služba agenta uzlu a hello Služba koordinátora spouštět na nižší úrovni oprávnění (síťová služba).</span><span class="sxs-lookup"><span data-stu-id="a497d-123">In contrast, hello Node Agent Service and hello Coordinator Service run at a lower-level privilege (NETWORK SERVICE).</span></span> <span data-ttu-id="a497d-124">Hello služba je zodpovědná za provádění následujících úloh aktualizace systému Windows na všech uzlech clusteru hello hello:</span><span class="sxs-lookup"><span data-stu-id="a497d-124">hello service is responsible for performing hello following Windows Update jobs on all hello cluster nodes:</span></span>
    - <span data-ttu-id="a497d-125">Vypnutí automatických aktualizací Windows hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-125">Disabling automatic Windows Update on hello node.</span></span>
    - <span data-ttu-id="a497d-126">Podle uživatele hello zásad toohello poskytl stahování a instalace služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="a497d-126">Downloading and installing Windows Update according toohello policy hello user has provided.</span></span>
    - <span data-ttu-id="a497d-127">Restartování počítače post hello instalace služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="a497d-127">Restarting hello machine post Windows Update installation.</span></span>
    - <span data-ttu-id="a497d-128">Odesílání hello výsledky toohello aktualizace systému Windows služby Coordinator.</span><span class="sxs-lookup"><span data-stu-id="a497d-128">Uploading hello results of Windows updates toohello Coordinator Service.</span></span>
    - <span data-ttu-id="a497d-129">Vytváření sestav stavu hlásí v případě, že operace se nezdařilo po jejím vyčerpání všechny opakování.</span><span class="sxs-lookup"><span data-stu-id="a497d-129">Reporting health reports in case an operation has failed after exhausting all retries.</span></span>

> [!NOTE]
> <span data-ttu-id="a497d-130">Hello oprava orchestration aplikace používá hello Service Fabric opravit toodisable služby správce systému nebo povolit hello uzlu a provádět kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="a497d-130">hello patch orchestration app uses hello Service Fabric repair manager system service toodisable or enable hello node and perform health checks.</span></span> <span data-ttu-id="a497d-131">Úloha opravy Hello vytvořené hello oprava orchestration aplikace sleduje hello průběh Windows Update pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="a497d-131">hello repair task created by hello patch orchestration app tracks hello Windows Update progress for each node.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a497d-132">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a497d-132">Prerequisites</span></span>

### <a name="minimum-supported-service-fabric-runtime-version"></a><span data-ttu-id="a497d-133">Minimální podporovaná verze modulu runtime Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a497d-133">Minimum supported Service Fabric runtime version</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="a497d-134">Azure clustery</span><span class="sxs-lookup"><span data-stu-id="a497d-134">Azure clusters</span></span>
<span data-ttu-id="a497d-135">Hello oprava orchestration aplikace musí být spuštěn na Azure clustery, které mají v5.5 verzi modulu runtime Service Fabric nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a497d-135">hello patch orchestration app must be run on Azure clusters that have Service Fabric runtime version v5.5 or later.</span></span>

#### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="a497d-136">Samostatné místní clustery</span><span class="sxs-lookup"><span data-stu-id="a497d-136">Standalone on-premises Clusters</span></span>
<span data-ttu-id="a497d-137">Hello oprava orchestration aplikace musí být spuštěn na samostatné clustery, které mají verze 5.6 verzi modulu runtime Service Fabric nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a497d-137">hello patch orchestration app must be run on Standalone clusters that have Service Fabric runtime version v5.6 or later.</span></span>

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a><span data-ttu-id="a497d-138">Povolení hello opravy Správce služby (Pokud již není spuštěn)</span><span class="sxs-lookup"><span data-stu-id="a497d-138">Enable hello repair manager service (if it's not running already)</span></span>

<span data-ttu-id="a497d-139">Hello oprava orchestration aplikace vyžaduje hello opravy správce systému služby toobe na hello clusteru povolena.</span><span class="sxs-lookup"><span data-stu-id="a497d-139">hello patch orchestration app requires hello repair manager system service toobe enabled on hello cluster.</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="a497d-140">Azure clustery</span><span class="sxs-lookup"><span data-stu-id="a497d-140">Azure clusters</span></span>

<span data-ttu-id="a497d-141">Azure clustery ve vrstvě stříbrným odolnost hello mají hello opravit service manager ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="a497d-141">Azure clusters in hello silver durability tier have hello repair manager service enabled by default.</span></span> <span data-ttu-id="a497d-142">Azure clustery ve vrstvě gold odolnost hello mohou nebo nemusí mít službu správce opravy hello povoleno, v závislosti na tom, kdy byly vytvořeny těchto clusterů.</span><span class="sxs-lookup"><span data-stu-id="a497d-142">Azure clusters in hello gold durability tier might or might not have hello repair manager service enabled, depending on when those clusters were created.</span></span> <span data-ttu-id="a497d-143">Azure clustery ve vrstvě hello bronzovým odolnost, ve výchozím nastavení, není nutné hello opravit service manager povolena.</span><span class="sxs-lookup"><span data-stu-id="a497d-143">Azure clusters in hello bronze durability tier, by default, do not have hello repair manager service enabled.</span></span> <span data-ttu-id="a497d-144">Pokud už je povolena služba hello, uvidíte jeho spuštění v části služby systému hello v hello Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="a497d-144">If hello service is already enabled, you can see it running in hello system services section in hello Service Fabric Explorer.</span></span>

##### <a name="azure-portal"></a><span data-ttu-id="a497d-145">portál Azure</span><span class="sxs-lookup"><span data-stu-id="a497d-145">Azure portal</span></span>
<span data-ttu-id="a497d-146">Opravte manager z portálu Azure můžete povolit v době hello nastavení clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-146">You can enable repair manager from Azure portal at hello time of setting up of cluster.</span></span> <span data-ttu-id="a497d-147">Vyberte `Include Repair Manager` možnost pod `Add on features` během hello konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-147">Select `Include Repair Manager` option under `Add on features` at hello time of Cluster configuration.</span></span>
<span data-ttu-id="a497d-148">![Image Manager opravy povolení z portálu Azure](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span><span class="sxs-lookup"><span data-stu-id="a497d-148">![Image of Enabling Repair Manager from Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span></span>

##### <a name="azure-resource-manager-template"></a><span data-ttu-id="a497d-149">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="a497d-149">Azure Resource Manager template</span></span>
<span data-ttu-id="a497d-150">Případně můžete také použít hello [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello opravy Správce služby v nových nebo stávajících clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a497d-150">Alternatively you can use hello [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello repair manager service on new and existing Service Fabric clusters.</span></span> <span data-ttu-id="a497d-151">Získáte hello šablony pro hello clusteru, které chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="a497d-151">Get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="a497d-152">Můžete buď použít hello ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a497d-152">You can either use hello sample templates or create a custom Resource Manager template.</span></span> 

<span data-ttu-id="a497d-153">tooenable hello opravy Správce služby pomocí [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span><span class="sxs-lookup"><span data-stu-id="a497d-153">tooenable hello repair manager service using [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span></span>

1. <span data-ttu-id="a497d-154">Nejdřív zkontrolujte, že hello `apiversion` je nastaven příliš`2017-07-01-preview` pro hello `Microsoft.ServiceFabric/clusters` prostředků, jak je znázorněno v následujícím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-154">First check that hello `apiversion` is set too`2017-07-01-preview` for hello `Microsoft.ServiceFabric/clusters` resource, as shown in hello following snippet.</span></span> <span data-ttu-id="a497d-155">Pokud se liší, pak musíte tooupdate hello `apiVersion` toohello hodnotu `2017-07-01-preview`:</span><span class="sxs-lookup"><span data-stu-id="a497d-155">If it is different, then you need tooupdate hello `apiVersion` toohello value `2017-07-01-preview`:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="a497d-156">Teď povolit service manager hello opravy přidáním následující hello `addonFeatures` po provedení hello `fabricSettings` části:</span><span class="sxs-lookup"><span data-stu-id="a497d-156">Now enable hello repair manager service by adding hello following `addonFeatures` section after hello `fabricSettings` section:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="a497d-157">Po aktualizaci šablony clusteru se tyto změny použít, je a nechat hello upgrade dokončit.</span><span class="sxs-lookup"><span data-stu-id="a497d-157">After you have updated your cluster template with these changes, apply them and let hello upgrade finish.</span></span> <span data-ttu-id="a497d-158">Nyní můžete vidět hello opravy správce systému služby běžící v clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-158">You can now see hello repair manager system service running in your cluster.</span></span> <span data-ttu-id="a497d-159">Je volána `fabric:/System/RepairManagerService` v části služby systému hello v hello Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="a497d-159">It is called `fabric:/System/RepairManagerService` in hello system services section in hello Service Fabric Explorer.</span></span> 

### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="a497d-160">Samostatné místní clustery</span><span class="sxs-lookup"><span data-stu-id="a497d-160">Standalone on-premises clusters</span></span>

<span data-ttu-id="a497d-161">Můžete použít hello [nastavení konfigurace pro samostatný cluster Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello opravy Správce služby v nových nebo stávajících clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a497d-161">You can use hello [Configuration settings for standalone Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello repair manager service on new and existing Service Fabric cluster.</span></span>

<span data-ttu-id="a497d-162">Služba Správce oprava tooenable hello:</span><span class="sxs-lookup"><span data-stu-id="a497d-162">tooenable hello repair manager service:</span></span>

1. <span data-ttu-id="a497d-163">Nejdřív zkontrolujte, že hello `apiversion` v [konfigurace obecné clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) je nastaven příliš`04-2017` nebo vyšší:</span><span class="sxs-lookup"><span data-stu-id="a497d-163">First check that hello `apiversion` in [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is set too`04-2017` or higher:</span></span>

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. <span data-ttu-id="a497d-164">Teď povolit service manager opravy přidáním následující hello `addonFeaturres` po provedení hello `fabricSettings` části, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="a497d-164">Now enable repair manager service by adding hello following `addonFeaturres` section after hello `fabricSettings` section as shown below:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="a497d-165">Aktualizovat manifestu clusteru se tyto změny pomocí manifestu clusteru hello aktualizovat [vytvoření nového clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) nebo [konfigurace clusteru upgradu hello](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span><span class="sxs-lookup"><span data-stu-id="a497d-165">Update your cluster manifest with these changes, using hello updated cluster manifest [create a new cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) or [upgrade hello cluster configuration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span></span> <span data-ttu-id="a497d-166">Jakmile hello clusteru je spuštěn s manifestu aktualizované clusteru, se zobrazí hello opravy správce systému služby běžící v clusteru, který se nazývá `fabric:/System/RepairManagerService`v systému služby v Service Fabric explorer hello části.</span><span class="sxs-lookup"><span data-stu-id="a497d-166">Once hello cluster is running with updated cluster manifest, you can now see hello repair manager system service running in your cluster, which is called `fabric:/System/RepairManagerService`, under system services section in hello Service Fabric explorer.</span></span>

### <a name="disable-automatic-windows-update-on-all-nodes"></a><span data-ttu-id="a497d-167">Zakázat automatické aktualizace systému Windows na všech uzlech</span><span class="sxs-lookup"><span data-stu-id="a497d-167">Disable automatic Windows Update on all nodes</span></span>

<span data-ttu-id="a497d-168">Automatické aktualizace systému Windows může vést ke ztrátě tooavailability, protože více uzlech clusteru můžete restartovat hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="a497d-168">Automatic Windows updates might lead tooavailability loss because multiple cluster nodes can restart at hello same time.</span></span> <span data-ttu-id="a497d-169">Hello oprava orchestration aplikace, ve výchozím nastavení pokusů toodisable hello automatických aktualizací Windows na všech uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-169">hello patch orchestration app, by default, tries toodisable hello automatic Windows Update on each cluster node.</span></span> <span data-ttu-id="a497d-170">Pokud hello nastavení spravuje správce nebo zásad skupiny, ale doporučujeme hello nastavení zásad příliš "oznámit před stažení" explicitně služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="a497d-170">However, if hello settings are managed by an administrator or group policy, we recommend setting hello Windows Update policy too“Notify before Download” explicitly.</span></span>

### <a name="optional-enable-azure-diagnostics"></a><span data-ttu-id="a497d-171">Volitelné: Povolení Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="a497d-171">Optional: Enable Azure Diagnostics</span></span>

<span data-ttu-id="a497d-172">Clustery se systémem verzi modulu runtime Service Fabric `5.6.220.9494` a výše shromažďování oprava orchestration aplikace protokoly jako součást Service Fabric protokoly.</span><span class="sxs-lookup"><span data-stu-id="a497d-172">Clusters running Service Fabric runtime version `5.6.220.9494` and above collect patch orchestration app logs as part of Service Fabric logs.</span></span>
<span data-ttu-id="a497d-173">Pokud váš cluster běží na verzi modulu runtime Service Fabric, můžete tento krok přeskočit `5.6.220.9494` a vyšší.</span><span class="sxs-lookup"><span data-stu-id="a497d-173">You can skip this step if your cluster is running on Service Fabric runtime version `5.6.220.9494` and above.</span></span>

<span data-ttu-id="a497d-174">Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, na každém uzlu clusteru hello místně shromáždí protokoly pro aplikaci orchestration oprava hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-174">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs for hello patch orchestration app are collected locally on each of hello cluster nodes.</span></span>
<span data-ttu-id="a497d-175">Doporučujeme nakonfigurovat Azure Diagnostics tooupload protokoly ze všech uzlů tooa centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="a497d-175">We recommend that you configure Azure Diagnostics tooupload logs from all nodes tooa central location.</span></span>

<span data-ttu-id="a497d-176">Informace o povolení Azure Diagnostics najdete v tématu [shromažďování protokolů pomocí Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span><span class="sxs-lookup"><span data-stu-id="a497d-176">For information on enabling Azure Diagnostics, see [Collect logs by using Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span></span>

<span data-ttu-id="a497d-177">Protokoly pro hello oprava orchestration aplikace jsou generovány na následující pevný zprostředkovatele ID hello:</span><span class="sxs-lookup"><span data-stu-id="a497d-177">Logs for hello patch orchestration app are generated on hello following fixed provider IDs:</span></span>

- <span data-ttu-id="a497d-178">e39b723c-590c-4090-abb0-11e3e6616346</span><span class="sxs-lookup"><span data-stu-id="a497d-178">e39b723c-590c-4090-abb0-11e3e6616346</span></span>
- <span data-ttu-id="a497d-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span><span class="sxs-lookup"><span data-stu-id="a497d-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span></span>
- <span data-ttu-id="a497d-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span><span class="sxs-lookup"><span data-stu-id="a497d-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span></span>
- <span data-ttu-id="a497d-181">05dc046c-60e9-4ef7-965e-91660adffa68</span><span class="sxs-lookup"><span data-stu-id="a497d-181">05dc046c-60e9-4ef7-965e-91660adffa68</span></span>

<span data-ttu-id="a497d-182">V goto šablony Resource Manageru `EtwEventSourceProviderConfiguration` oddílu pod `WadCfg` a přidejte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a497d-182">In Resource Manager template goto `EtwEventSourceProviderConfiguration` section under `WadCfg` and add hello following entries:</span></span>

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> <span data-ttu-id="a497d-183">Pokud váš cluster Service Fabric má více typů uzlu, pak hello předchozí části, je třeba přidat k všechny hello `WadCfg` oddíly.</span><span class="sxs-lookup"><span data-stu-id="a497d-183">If your Service Fabric cluster has multiple node types, then hello previous section must be added for all hello `WadCfg` sections.</span></span>

## <a name="download-hello-app-package"></a><span data-ttu-id="a497d-184">Stáhněte si balíček aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-184">Download hello app package</span></span>

<span data-ttu-id="a497d-185">Stažení aplikace hello z hello [stáhnout odkaz](https://go.microsoft.com/fwlink/P/?linkid=849590).</span><span class="sxs-lookup"><span data-stu-id="a497d-185">Download hello application from hello [download link](https://go.microsoft.com/fwlink/P/?linkid=849590).</span></span>

## <a name="configure-hello-app"></a><span data-ttu-id="a497d-186">Konfigurace aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-186">Configure hello app</span></span>

<span data-ttu-id="a497d-187">Hello chování hello oprava orchestration aplikace může být nakonfigurované toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="a497d-187">hello behavior of hello patch orchestration app can be configured toomeet your needs.</span></span> <span data-ttu-id="a497d-188">Přepište výchozí hodnoty hello předávání v parametru aplikace hello při vytvoření aplikace nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a497d-188">Override hello default values by passing in hello application parameter during application creation or update.</span></span> <span data-ttu-id="a497d-189">Parametry aplikačního lze zadat zadáním `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` nebo `New-ServiceFabricApplication` rutiny.</span><span class="sxs-lookup"><span data-stu-id="a497d-189">Application parameters can be provided by specifying `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` or `New-ServiceFabricApplication` cmdlets.</span></span>

|<span data-ttu-id="a497d-190">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="a497d-190">**Parameter**</span></span>        |<span data-ttu-id="a497d-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="a497d-191">**Type**</span></span>                          | <span data-ttu-id="a497d-192">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="a497d-192">**Details**</span></span>|
|:-|-|-|
|<span data-ttu-id="a497d-193">MaxResultsToCache</span><span class="sxs-lookup"><span data-stu-id="a497d-193">MaxResultsToCache</span></span>    |<span data-ttu-id="a497d-194">Dlouhé</span><span class="sxs-lookup"><span data-stu-id="a497d-194">Long</span></span>                              | <span data-ttu-id="a497d-195">Maximální počet výsledků Windows Update, které by měl být mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="a497d-195">Maximum number of Windows Update results, which should be cached.</span></span> <br><span data-ttu-id="a497d-196">Výchozí hodnota je 3000 za předpokladu, že:</span><span class="sxs-lookup"><span data-stu-id="a497d-196">Default value is 3000 assuming the:</span></span> <br> <span data-ttu-id="a497d-197">-Počet uzlů je 20.</span><span class="sxs-lookup"><span data-stu-id="a497d-197">- Number of nodes is 20.</span></span> <br> <span data-ttu-id="a497d-198">-Počet aktualizací děje na uzlu za měsíc je pět.</span><span class="sxs-lookup"><span data-stu-id="a497d-198">- Number of updates happening on a node per month is five.</span></span> <br> <span data-ttu-id="a497d-199">-Počet výsledků na operace může být 10.</span><span class="sxs-lookup"><span data-stu-id="a497d-199">- Number of results per operation can be 10.</span></span> <br> <span data-ttu-id="a497d-200">-Výsledky pro hello posledních tří měsíců by měly být uložené.</span><span class="sxs-lookup"><span data-stu-id="a497d-200">- Results for hello past three months should be stored.</span></span> |
|<span data-ttu-id="a497d-201">TaskApprovalPolicy</span><span class="sxs-lookup"><span data-stu-id="a497d-201">TaskApprovalPolicy</span></span>   |<span data-ttu-id="a497d-202">výčet</span><span class="sxs-lookup"><span data-stu-id="a497d-202">Enum</span></span> <br> <span data-ttu-id="a497d-203">{NodeWise, UpgradeDomainWise}</span><span class="sxs-lookup"><span data-stu-id="a497d-203">{ NodeWise, UpgradeDomainWise }</span></span>                          |<span data-ttu-id="a497d-204">TaskApprovalPolicy označuje hello zásady, které toobe používané aktualizace systému Windows tooinstall Služba koordinátora hello mezi uzly clusteru Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-204">TaskApprovalPolicy indicates hello policy that is toobe used by hello Coordinator Service tooinstall Windows updates across hello Service Fabric cluster nodes.</span></span><br>                         <span data-ttu-id="a497d-205">Povolené hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="a497d-205">Allowed values are:</span></span> <br>                                                           <span data-ttu-id="a497d-206"><b>NodeWise</b>.</span><span class="sxs-lookup"><span data-stu-id="a497d-206"><b>NodeWise</b>.</span></span> <span data-ttu-id="a497d-207">Windows Update je nainstalovaná jednoho uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="a497d-207">Windows Update is installed one node at a time.</span></span> <br>                                                           <span data-ttu-id="a497d-208"><b>UpgradeDomainWise</b>.</span><span class="sxs-lookup"><span data-stu-id="a497d-208"><b>UpgradeDomainWise</b>.</span></span> <span data-ttu-id="a497d-209">Windows Update je nainstalovaná jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="a497d-209">Windows Update is installed one upgrade domain at a time.</span></span> <span data-ttu-id="a497d-210">(Na maximální hello, můžete přejít všechny hello uzlech náležících tooan domény upgradu pro Windows Update.)</span><span class="sxs-lookup"><span data-stu-id="a497d-210">(At hello maximum, all hello nodes belonging tooan upgrade domain can go for Windows Update.)</span></span>
|<span data-ttu-id="a497d-211">LogsDiskQuotaInMB</span><span class="sxs-lookup"><span data-stu-id="a497d-211">LogsDiskQuotaInMB</span></span>   |<span data-ttu-id="a497d-212">Dlouhé</span><span class="sxs-lookup"><span data-stu-id="a497d-212">Long</span></span>  <br> <span data-ttu-id="a497d-213">(Výchozí: 1024)</span><span class="sxs-lookup"><span data-stu-id="a497d-213">(Default: 1024)</span></span>               |<span data-ttu-id="a497d-214">Maximální velikost oprava orchestration aplikace přihlásí MB, který můžete nastavit jako trvalý, místně na uzlech.</span><span class="sxs-lookup"><span data-stu-id="a497d-214">Maximum size of patch orchestration app logs in MB, which can be persisted locally on nodes.</span></span>
| <span data-ttu-id="a497d-215">WUQuery</span><span class="sxs-lookup"><span data-stu-id="a497d-215">WUQuery</span></span>               | <span data-ttu-id="a497d-216">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a497d-216">string</span></span><br><span data-ttu-id="a497d-217">(Výchozí: "IsInstalled = 0")</span><span class="sxs-lookup"><span data-stu-id="a497d-217">(Default: "IsInstalled=0")</span></span>                | <span data-ttu-id="a497d-218">Dotaz tooget aktualizace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a497d-218">Query tooget Windows updates.</span></span> <span data-ttu-id="a497d-219">Další informace najdete v tématu [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="a497d-219">For more information, see [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span></span>
| <span data-ttu-id="a497d-220">InstallWindowsOSOnlyUpdates</span><span class="sxs-lookup"><span data-stu-id="a497d-220">InstallWindowsOSOnlyUpdates</span></span> | <span data-ttu-id="a497d-221">BOOL</span><span class="sxs-lookup"><span data-stu-id="a497d-221">Bool</span></span> <br> <span data-ttu-id="a497d-222">(výchozí: True)</span><span class="sxs-lookup"><span data-stu-id="a497d-222">(default: True)</span></span>                 | <span data-ttu-id="a497d-223">Tento příznak umožňuje toobe aktualizace systému nainstalován operační systém Windows.</span><span class="sxs-lookup"><span data-stu-id="a497d-223">This flag allows Windows operating system updates toobe installed.</span></span>            |
| <span data-ttu-id="a497d-224">WUOperationTimeOutInMinutes</span><span class="sxs-lookup"><span data-stu-id="a497d-224">WUOperationTimeOutInMinutes</span></span> | <span data-ttu-id="a497d-225">celá čísla</span><span class="sxs-lookup"><span data-stu-id="a497d-225">Int</span></span> <br><span data-ttu-id="a497d-226">(Výchozí: 90).</span><span class="sxs-lookup"><span data-stu-id="a497d-226">(Default: 90)</span></span>                   | <span data-ttu-id="a497d-227">Určuje časový limit hello pro všechny operace služby Windows Update (hledání nebo stáhnout nebo nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="a497d-227">Specifies hello timeout for any Windows Update operation (search or download or install).</span></span> <span data-ttu-id="a497d-228">Pokud hello operaci nelze dokončit v rámci zadaného časového limitu Dobrý den, byl přerušen.</span><span class="sxs-lookup"><span data-stu-id="a497d-228">If hello operation is not completed within hello specified timeout, it is aborted.</span></span>       |
| <span data-ttu-id="a497d-229">WURescheduleCount</span><span class="sxs-lookup"><span data-stu-id="a497d-229">WURescheduleCount</span></span>     | <span data-ttu-id="a497d-230">celá čísla</span><span class="sxs-lookup"><span data-stu-id="a497d-230">Int</span></span> <br> <span data-ttu-id="a497d-231">(Výchozí: 5).</span><span class="sxs-lookup"><span data-stu-id="a497d-231">(Default: 5)</span></span>                  | <span data-ttu-id="a497d-232">Hello maximálním počtem pokusů hello přeplánuje hello služby Windows update v případě, že operace selže trvalé.</span><span class="sxs-lookup"><span data-stu-id="a497d-232">hello maximum number of times hello service reschedules hello Windows update in case an operation fails persistently.</span></span>          |
| <span data-ttu-id="a497d-233">WURescheduleTimeInMinutes</span><span class="sxs-lookup"><span data-stu-id="a497d-233">WURescheduleTimeInMinutes</span></span> | <span data-ttu-id="a497d-234">celá čísla</span><span class="sxs-lookup"><span data-stu-id="a497d-234">Int</span></span> <br><span data-ttu-id="a497d-235">(Výchozí: 30).</span><span class="sxs-lookup"><span data-stu-id="a497d-235">(Default: 30)</span></span> | <span data-ttu-id="a497d-236">Hello interval, ve které hello služby přeplánuje hello Windows update v případě, že chyba přetrvává.</span><span class="sxs-lookup"><span data-stu-id="a497d-236">hello interval at which hello service reschedules hello Windows update in case failure persists.</span></span> |
| <span data-ttu-id="a497d-237">WUFrequency</span><span class="sxs-lookup"><span data-stu-id="a497d-237">WUFrequency</span></span>           | <span data-ttu-id="a497d-238">Řetězce s hodnotami oddělenými čárkou (výchozí: "Každý týden, středu, 7:00:00")</span><span class="sxs-lookup"><span data-stu-id="a497d-238">Comma-separated string (Default: "Weekly, Wednesday, 7:00:00")</span></span>     | <span data-ttu-id="a497d-239">Hello četnost pro instalaci služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="a497d-239">hello frequency for installing Windows Update.</span></span> <span data-ttu-id="a497d-240">Hello formátu a možné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="a497d-240">hello format and possible values are:</span></span> <br><span data-ttu-id="a497d-241">-Měsíců, DD, hh, například každý měsíc, 5, 12: 22:32.</span><span class="sxs-lookup"><span data-stu-id="a497d-241">-   Monthly, DD,HH:MM:SS, for example, Monthly, 5,12:22:32.</span></span> <br> <span data-ttu-id="a497d-242">-Každý týden, den, hh: mm:, pro příklad, týdně, úterý, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="a497d-242">-   Weekly, DAY,HH:MM:SS, for example, Weekly, Tuesday, 12:22:32.</span></span>  <br> <span data-ttu-id="a497d-243">-Denní, hh: mm:, např. denně, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="a497d-243">-   Daily, HH:MM:SS, for example, Daily, 12:22:32.</span></span>  <br> <span data-ttu-id="a497d-244">-Žádný označuje, že by neměl Windows Update provést.</span><span class="sxs-lookup"><span data-stu-id="a497d-244">-  None indicates that Windows Update shouldn't be done.</span></span>  <br><br> <span data-ttu-id="a497d-245">Všimněte si, že jsou všechny časy hello v čase UTC.</span><span class="sxs-lookup"><span data-stu-id="a497d-245">Note that all hello times are in UTC.</span></span>|
| <span data-ttu-id="a497d-246">AcceptWindowsUpdateEula</span><span class="sxs-lookup"><span data-stu-id="a497d-246">AcceptWindowsUpdateEula</span></span> | <span data-ttu-id="a497d-247">BOOL</span><span class="sxs-lookup"><span data-stu-id="a497d-247">Bool</span></span> <br><span data-ttu-id="a497d-248">(Výchozí: true)</span><span class="sxs-lookup"><span data-stu-id="a497d-248">(Default: true)</span></span> | <span data-ttu-id="a497d-249">Nastavením tohoto příznaku hello aplikace přijímá hello koncového uživatele licenční smlouvu pro Windows Update jménem vlastníka hello hello počítače.</span><span class="sxs-lookup"><span data-stu-id="a497d-249">By setting this flag, hello application accepts hello End-User License Agreement for Windows Update on behalf of hello owner of hello machine.</span></span>              |

> [!TIP]
> <span data-ttu-id="a497d-250">Pokud chcete okamžitě toohappen služby Windows Update, nastavte `WUFrequency` relativní toohello času nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a497d-250">If you want Windows Update toohappen immediately, set `WUFrequency` relative toohello application deployment time.</span></span> <span data-ttu-id="a497d-251">Například předpokládejme, že máte testovací pěti uzly clusteru a plán toodeploy hello aplikace v přibližně 5:00 PM UTC.</span><span class="sxs-lookup"><span data-stu-id="a497d-251">For example, suppose that you have a five-node test cluster and plan toodeploy hello app at around 5:00 PM UTC.</span></span> <span data-ttu-id="a497d-252">Pokud budete předpokládat, že nasazení nebo upgradu aplikace hello trvá 30 minut v hello maximální, nastavte hello WUFrequency jako "Denně, 17:30:00."</span><span class="sxs-lookup"><span data-stu-id="a497d-252">If you assume that hello application upgrade or deployment takes 30 minutes at hello maximum, set hello WUFrequency as "Daily, 17:30:00."</span></span>

## <a name="deploy-hello-app"></a><span data-ttu-id="a497d-253">Nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-253">Deploy hello app</span></span>

1. <span data-ttu-id="a497d-254">Dokončete všechny hello požadovaných krocích tooprepare hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="a497d-254">Finish all hello prerequisite steps tooprepare hello cluster.</span></span>
2. <span data-ttu-id="a497d-255">Nasaďte aplikaci orchestration oprava hello stejně jako jiná aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a497d-255">Deploy hello patch orchestration app like any other Service Fabric app.</span></span> <span data-ttu-id="a497d-256">Hello aplikaci můžete nasadit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a497d-256">You can deploy hello app by using PowerShell.</span></span> <span data-ttu-id="a497d-257">Postupujte podle kroků hello v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="a497d-257">Follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>
3. <span data-ttu-id="a497d-258">aplikace hello tooconfigure v době nasazení, pass hello hello `ApplicationParamater` toohello `New-ServiceFabricApplication` rutiny.</span><span class="sxs-lookup"><span data-stu-id="a497d-258">tooconfigure hello application at hello time of deployment, pass hello `ApplicationParamater` toohello `New-ServiceFabricApplication` cmdlet.</span></span> <span data-ttu-id="a497d-259">Pro usnadnění nabízíme hello skriptu Deploy.ps1 společně s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-259">For your convenience, we’ve provided hello script Deploy.ps1 along with hello application.</span></span> <span data-ttu-id="a497d-260">toouse hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="a497d-260">toouse hello script:</span></span>

    - <span data-ttu-id="a497d-261">Připojit cluster Service Fabric tooa pomocí `Connect-ServiceFabricCluster`.</span><span class="sxs-lookup"><span data-stu-id="a497d-261">Connect tooa Service Fabric cluster by using `Connect-ServiceFabricCluster`.</span></span>
    - <span data-ttu-id="a497d-262">Spustit skript prostředí PowerShell hello Deploy.ps1 s hello odpovídající `ApplicationParameter` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a497d-262">Execute hello PowerShell script Deploy.ps1 with hello appropriate `ApplicationParameter` value.</span></span>

> [!NOTE]
> <span data-ttu-id="a497d-263">Mějte hello hello skript a složce aplikace hello PatchOrchestrationApplication stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="a497d-263">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="upgrade-hello-app"></a><span data-ttu-id="a497d-264">Upgrade aplikace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-264">Upgrade hello app</span></span>

<span data-ttu-id="a497d-265">tooupgrade stávající aplikace orchestration oprava pomocí prostředí PowerShell, postupujte podle kroků hello v [upgradu aplikace Service Fabric pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span><span class="sxs-lookup"><span data-stu-id="a497d-265">tooupgrade an existing patch orchestration app by using PowerShell, follow hello steps in [Service Fabric application upgrade using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span></span>

## <a name="remove-hello-app"></a><span data-ttu-id="a497d-266">Odeberte aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="a497d-266">Remove hello app</span></span>

<span data-ttu-id="a497d-267">tooremove hello aplikace, postupujte podle kroků hello v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="a497d-267">tooremove hello application, follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>

<span data-ttu-id="a497d-268">Pro usnadnění nabízíme hello skriptu Undeploy.ps1 společně s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-268">For your convenience, we've provided hello script Undeploy.ps1 along with hello application.</span></span> <span data-ttu-id="a497d-269">toouse hello skriptu:</span><span class="sxs-lookup"><span data-stu-id="a497d-269">toouse hello script:</span></span>

  - <span data-ttu-id="a497d-270">Připojit cluster Service Fabric tooa pomocí ```Connect-ServiceFabricCluster```.</span><span class="sxs-lookup"><span data-stu-id="a497d-270">Connect tooa Service Fabric cluster by using ```Connect-ServiceFabricCluster```.</span></span>

  - <span data-ttu-id="a497d-271">Spusťte skript prostředí PowerShell hello Undeploy.ps1.</span><span class="sxs-lookup"><span data-stu-id="a497d-271">Execute hello PowerShell script Undeploy.ps1.</span></span>

> [!NOTE]
> <span data-ttu-id="a497d-272">Mějte hello hello skript a složce aplikace hello PatchOrchestrationApplication stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="a497d-272">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="view-hello-windows-update-results"></a><span data-ttu-id="a497d-273">Zobrazení výsledků aktualizace Windows hello</span><span class="sxs-lookup"><span data-stu-id="a497d-273">View hello Windows Update results</span></span>

<span data-ttu-id="a497d-274">Hello oprava orchestration aplikace zpřístupňuje rozhraní REST API toodisplay hello historických výsledky toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="a497d-274">hello patch orchestration app exposes REST APIs toodisplay hello historical results toohello user.</span></span> <span data-ttu-id="a497d-275">Příklad hello výsledku JSON:</span><span class="sxs-lookup"><span data-stu-id="a497d-275">An example of hello result JSON:</span></span>
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
<span data-ttu-id="a497d-276">Pokud žádná aktualizace je ještě naplánováno, výsledkem hello JSON je prázdný.</span><span class="sxs-lookup"><span data-stu-id="a497d-276">If no update is scheduled yet, hello result JSON is empty.</span></span>

<span data-ttu-id="a497d-277">Přihlaste se tooquery toohello clusteru služby Windows Update výsledky.</span><span class="sxs-lookup"><span data-stu-id="a497d-277">Log in toohello cluster tooquery Windows Update results.</span></span> <span data-ttu-id="a497d-278">Poté zjistit adresu hello repliky pro primární hello Dobrý den Služba koordinátora a stiskněte tlačítko hello adresu URL z prohlížeče hello: http://&lt;REPLIKY IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="a497d-278">Then find out hello replica address for hello primary of hello Coordinator Service, and hit hello URL from hello browser: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="a497d-279">koncový bod REST Hello pro hello Služba koordinátora má dynamický port.</span><span class="sxs-lookup"><span data-stu-id="a497d-279">hello REST endpoint for hello Coordinator Service has a dynamic port.</span></span> <span data-ttu-id="a497d-280">toocheck hello přesnou adresu URL, najdete v toohello Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="a497d-280">toocheck hello exact URL, refer toohello Service Fabric Explorer.</span></span> <span data-ttu-id="a497d-281">Například jsou k dispozici na výsledky hello `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span><span class="sxs-lookup"><span data-stu-id="a497d-281">For example, hello results are available at `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span></span>

![Obrázek koncový bod REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


<span data-ttu-id="a497d-283">Pokud je v clusteru hello povoleno hello reverzní proxy server, můžete přístup k adrese URL hello z mimo hello clusteru také.</span><span class="sxs-lookup"><span data-stu-id="a497d-283">If hello reverse proxy is enabled on hello cluster, you can access hello URL from outside of hello cluster as well.</span></span>
<span data-ttu-id="a497d-284">Hello koncový bod, který potřebuje toobe podle je http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="a497d-284">hello endpoint that needs toobe hit is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="a497d-285">tooenable hello reverzní proxy server na hello clusteru, postupujte podle kroků hello v [reverzní proxy server v Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span><span class="sxs-lookup"><span data-stu-id="a497d-285">tooenable hello reverse proxy on hello cluster, follow hello steps in [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span></span> 

> 
> [!WARNING]
> <span data-ttu-id="a497d-286">Po dokončení konfigurace hello reverzní proxy server, jsou adresovatelné z mimo hello cluster všechny malých služby v hello clusteru, které zveřejňují koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="a497d-286">After hello reverse proxy is configured, all micro services in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>

## <a name="diagnosticshealth-events"></a><span data-ttu-id="a497d-287">Diagnostika stavu události</span><span class="sxs-lookup"><span data-stu-id="a497d-287">Diagnostics/health events</span></span>

### <a name="collect-patch-orchestration-app-logs"></a><span data-ttu-id="a497d-288">Protokolů shromažďování oprava orchestration aplikace</span><span class="sxs-lookup"><span data-stu-id="a497d-288">Collect patch orchestration app logs</span></span>

<span data-ttu-id="a497d-289">Oprava orchestration aplikace protokoly se shromažďují jako součást Service Fabric protokoly z modulu runtime verze `5.6.220.9494` a vyšší.</span><span class="sxs-lookup"><span data-stu-id="a497d-289">Patch orchestration app logs are collected as part of Service Fabric logs from runtime version `5.6.220.9494` and above.</span></span>
<span data-ttu-id="a497d-290">Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, pomocí jedné z následujících metod hello se můžou shromažďovat protokoly.</span><span class="sxs-lookup"><span data-stu-id="a497d-290">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs can be collected by using one of hello following methods.</span></span>

#### <a name="locally-on-each-node"></a><span data-ttu-id="a497d-291">Místně na každém uzlu</span><span class="sxs-lookup"><span data-stu-id="a497d-291">Locally on each node</span></span>

<span data-ttu-id="a497d-292">Protokoly se shromažďují místně na každém uzlu clusteru Service Fabric Pokud verzi modulu runtime Service Fabric je menší než `5.6.220.9494`.</span><span class="sxs-lookup"><span data-stu-id="a497d-292">Logs are collected locally on each Service Fabric cluster node if Service Fabric runtime version is less than `5.6.220.9494`.</span></span> <span data-ttu-id="a497d-293">je technologie Hello umístění tooaccess hello protokoly \[Service Fabric\_instalace\_jednotka\]:\\PatchOrchestrationApplication\\protokoly.</span><span class="sxs-lookup"><span data-stu-id="a497d-293">hello location tooaccess hello logs is \[Service Fabric\_Installation\_Drive\]:\\PatchOrchestrationApplication\\logs.</span></span>

<span data-ttu-id="a497d-294">Například pokud Service Fabric je nainstalován na jednotce D, cesta hello je D:\\PatchOrchestrationApplication\\protokoly.</span><span class="sxs-lookup"><span data-stu-id="a497d-294">For example, if Service Fabric is installed on drive D, hello path is D:\\PatchOrchestrationApplication\\logs.</span></span>

#### <a name="central-location"></a><span data-ttu-id="a497d-295">Centrální umístění</span><span class="sxs-lookup"><span data-stu-id="a497d-295">Central location</span></span>

<span data-ttu-id="a497d-296">Pokud Azure Diagnostics je nakonfigurovaný jako součást požadovaných krocích, protokoly pro hello oprava orchestration aplikace jsou dostupné ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a497d-296">If Azure Diagnostics is configured as part of prerequisite steps, logs for hello patch orchestration app are available in Azure Storage.</span></span>

### <a name="health-reports"></a><span data-ttu-id="a497d-297">Sestavy o stavu</span><span class="sxs-lookup"><span data-stu-id="a497d-297">Health reports</span></span>

<span data-ttu-id="a497d-298">Hello oprava orchestration aplikace, publikuje také sestavy stavu proti hello Služba koordinátora nebo hello Služba agenta uzlu v následujících případech hello:</span><span class="sxs-lookup"><span data-stu-id="a497d-298">hello patch orchestration app also publishes health reports against hello Coordinator Service or hello Node Agent Service in hello following cases:</span></span>

#### <a name="a-windows-update-operation-failed"></a><span data-ttu-id="a497d-299">Windows Update operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="a497d-299">A Windows Update operation failed</span></span>

<span data-ttu-id="a497d-300">Pokud na uzlu selže operace služby Windows Update, sestavy stavu se vygeneruje proti hello Služba agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-300">If a Windows Update operation fails on a node, a health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="a497d-301">Podrobnosti sestavy stavu hello obsahují název problematické uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-301">Details of hello health report contain hello problematic node name.</span></span>

<span data-ttu-id="a497d-302">Po použití dílčích oprav úspěšném dokončení v uzlu hello problematické, jsou automaticky vymazány hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="a497d-302">After patching is successfully completed on hello problematic node, hello report is automatically cleared.</span></span>

#### <a name="hello-node-agent-ntservice-is-down"></a><span data-ttu-id="a497d-303">Hello NTService agenta uzlu je mimo provoz</span><span class="sxs-lookup"><span data-stu-id="a497d-303">hello Node Agent NTService is down</span></span>

<span data-ttu-id="a497d-304">Pokud hello NTService agenta uzlu je vypnutý na uzlu, vygeneruje se sestavy stavu úroveň pro upozornění před hello Služba agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-304">If hello Node Agent NTService is down on a node, a warning-level health report is generated against hello Node Agent Service.</span></span>

#### <a name="hello-repair-manager-service-is-not-enabled"></a><span data-ttu-id="a497d-305">není povolená služba manager oprava Hello</span><span class="sxs-lookup"><span data-stu-id="a497d-305">hello repair manager service is not enabled</span></span>

<span data-ttu-id="a497d-306">Pokud hello opravy Správce služby nebyla nalezena v clusteru hello, vygeneruje se pro hello Služba koordinátora sestavy stavu úroveň pro upozornění.</span><span class="sxs-lookup"><span data-stu-id="a497d-306">If hello repair manager service is not found on hello cluster, a warning-level health report is generated for hello Coordinator Service.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="a497d-307">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a497d-307">Frequently asked questions</span></span>

<span data-ttu-id="a497d-308">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="a497d-308">Q.</span></span> <span data-ttu-id="a497d-309">**Proč se při spuštěné aplikaci orchestration oprava hello vidí Moje clusteru v chybovém stavu?**</span><span class="sxs-lookup"><span data-stu-id="a497d-309">**Why do I see my cluster in an error state when hello patch orchestration app is running?**</span></span>

<span data-ttu-id="a497d-310">A.</span><span class="sxs-lookup"><span data-stu-id="a497d-310">A.</span></span> <span data-ttu-id="a497d-311">Během procesu instalace hello hello oprava orchestration aplikace zakáže nebo restartování uzlů, které mohou dočasně hello stav clusteru hello směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="a497d-311">During hello installation process, hello patch orchestration app disables or restarts nodes, which can temporarily result in hello health of hello cluster going down.</span></span>

<span data-ttu-id="a497d-312">Na základě hello zásady pro aplikace hello buď jeden uzel můžete přejděte během operace oprav *nebo* celá doména upgradu můžete současně přejděte.</span><span class="sxs-lookup"><span data-stu-id="a497d-312">Based on hello policy for hello application, either one node can go down during a patching operation *or* an entire upgrade domain can go down simultaneously.</span></span>

<span data-ttu-id="a497d-313">Hello konec hello instalace aktualizace Windows hello, které jsou opětovně uzly povolena post restartování.</span><span class="sxs-lookup"><span data-stu-id="a497d-313">By hello end of hello Windows Update installation, hello nodes are reenabled post restart.</span></span>

<span data-ttu-id="a497d-314">V následujícím příkladu hello hello clusteru se, že tooan chybový stav dočasně vzhledem k tomu, že dva uzly byly dolů a hello MaxPercentageUnhealthNodes zásad byla porušena.</span><span class="sxs-lookup"><span data-stu-id="a497d-314">In hello following example, hello cluster went tooan error state temporarily because two nodes were down and hello MaxPercentageUnhealthNodes policy was violated.</span></span> <span data-ttu-id="a497d-315">Chyba Hello je dočasný, dokud probíhá hello opravy operaci.</span><span class="sxs-lookup"><span data-stu-id="a497d-315">hello error is temporary until hello patching operation is ongoing.</span></span>

![Bitové kopie není v pořádku clusteru](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

<span data-ttu-id="a497d-317">Pokud hello problém přetrvá, podívejte se na část věnovanou řešení potíží toohello.</span><span class="sxs-lookup"><span data-stu-id="a497d-317">If hello issue persists, refer toohello Troubleshooting section.</span></span>

<span data-ttu-id="a497d-318">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="a497d-318">Q.</span></span> <span data-ttu-id="a497d-319">**Oprava aplikace orchestration je ve stavu upozornění**</span><span class="sxs-lookup"><span data-stu-id="a497d-319">**Patch orchestration app is in warning state**</span></span>

<span data-ttu-id="a497d-320">A.</span><span class="sxs-lookup"><span data-stu-id="a497d-320">A.</span></span> <span data-ttu-id="a497d-321">Zkontrolujte toosee v případě sestavy stavu odeslány proti hello aplikace hello hlavní příčinu.</span><span class="sxs-lookup"><span data-stu-id="a497d-321">Check toosee if a health report posted against hello application is hello root cause.</span></span> <span data-ttu-id="a497d-322">Upozornění hello obvykle obsahuje podrobnosti o problému hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-322">Usually, hello warning contains details of hello problem.</span></span> <span data-ttu-id="a497d-323">Pokud je přechodný problém hello, aplikace hello je očekávané tooauto obnovit z tohoto stavu.</span><span class="sxs-lookup"><span data-stu-id="a497d-323">If hello issue is transient, hello application is expected tooauto-recover from this state.</span></span>

<span data-ttu-id="a497d-324">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="a497d-324">Q.</span></span> <span data-ttu-id="a497d-325">**Co mám dělat, když můj clusteru není v pořádku a je potřeba toodo aktualizaci naléhavé operačního systému?**</span><span class="sxs-lookup"><span data-stu-id="a497d-325">**What can I do if my cluster is unhealthy and I need toodo an urgent operating system update?**</span></span>

<span data-ttu-id="a497d-326">A.</span><span class="sxs-lookup"><span data-stu-id="a497d-326">A.</span></span> <span data-ttu-id="a497d-327">Hello oprava orchestration aplikace neinstaluje aktualizace při hello clusteru není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a497d-327">hello patch orchestration app does not install updates while hello cluster is unhealthy.</span></span> <span data-ttu-id="a497d-328">Zkuste toobring clusteru tooa stavu v pořádku toounblock hello oprava orchestration aplikace pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a497d-328">Try toobring your cluster tooa healthy state toounblock hello patch orchestration app workflow.</span></span>

<span data-ttu-id="a497d-329">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="a497d-329">Q.</span></span> <span data-ttu-id="a497d-330">**Proč opravy napříč clustery trvá tak dlouho toorun?**</span><span class="sxs-lookup"><span data-stu-id="a497d-330">**Why does patching across clusters take so long toorun?**</span></span>

<span data-ttu-id="a497d-331">A.</span><span class="sxs-lookup"><span data-stu-id="a497d-331">A.</span></span> <span data-ttu-id="a497d-332">Hello čas potřebný hello oprava orchestration aplikace je ve většině případů závisí na hello následující faktory:</span><span class="sxs-lookup"><span data-stu-id="a497d-332">hello time needed by hello patch orchestration app is mostly dependent on hello following factors:</span></span>

- <span data-ttu-id="a497d-333">zásady Hello hello Služba koordinátora.</span><span class="sxs-lookup"><span data-stu-id="a497d-333">hello policy of hello Coordinator Service.</span></span> 
  - <span data-ttu-id="a497d-334">Hello výchozí zásady `NodeWise`, výsledkem opravy jenom jeden uzel v čase.</span><span class="sxs-lookup"><span data-stu-id="a497d-334">hello default policy, `NodeWise`, results in patching only one node at a time.</span></span> <span data-ttu-id="a497d-335">Zejména v případě hello větší clusterů, doporučujeme použít hello `UpgradeDomainWise` zásad tooachieve rychlejší opravy napříč clustery.</span><span class="sxs-lookup"><span data-stu-id="a497d-335">Especially in hello case of bigger clusters, we recommend that you use hello `UpgradeDomainWise` policy tooachieve faster patching across clusters.</span></span>
- <span data-ttu-id="a497d-336">Hello počet aktualizací, které jsou k dispozici ke stažení a instalaci.</span><span class="sxs-lookup"><span data-stu-id="a497d-336">hello number of updates available for download and installation.</span></span> 
- <span data-ttu-id="a497d-337">Hello průměrný čas potřebný toodownload a nainstalovat aktualizace, které by neměl překročit několik hodin.</span><span class="sxs-lookup"><span data-stu-id="a497d-337">hello average time needed toodownload and install an update, which should not exceed a couple of hours.</span></span>
- <span data-ttu-id="a497d-338">výkon Hello hello virtuálních počítačů a šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="a497d-338">hello performance of hello VM and network bandwidth.</span></span>

<span data-ttu-id="a497d-339">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="a497d-339">Q.</span></span> <span data-ttu-id="a497d-340">**Proč vidí některé aktualizace ve výsledcích Windows Update získat prostřednictvím rozhraní api REST, ale není v rámci hello historii služby Windows Update v počítači?**</span><span class="sxs-lookup"><span data-stu-id="a497d-340">**Why do I see some updates in Windows Update results obtained via REST api's, but not under hello Windows Update history on machine?**</span></span>

<span data-ttu-id="a497d-341">A.</span><span class="sxs-lookup"><span data-stu-id="a497d-341">A.</span></span> <span data-ttu-id="a497d-342">Některé aktualizace produktu potřebovat toobe změnami svoji historii příslušné aktualizace nebo opravy.</span><span class="sxs-lookup"><span data-stu-id="a497d-342">Some product updates need toobe checked in their respective update/patch history.</span></span> <span data-ttu-id="a497d-343">Např: Aktualizace programu Windows Defender se nezobrazí v historii služby Windows Update v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="a497d-343">Eg: Windows Defender updates do not show up in Windows Update history on Windows Server 2016.</span></span>

## <a name="disclaimers"></a><span data-ttu-id="a497d-344">Právní omezení</span><span class="sxs-lookup"><span data-stu-id="a497d-344">Disclaimers</span></span>

- <span data-ttu-id="a497d-345">Hello oprava orchestration aplikace přijímá hello koncového uživatele licenční smlouvy služby Windows Update jménem uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-345">hello patch orchestration app accepts hello End-User License Agreement of Windows Update on behalf of hello user.</span></span> <span data-ttu-id="a497d-346">Volitelně může být vypnuto hello nastavení v konfiguraci hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-346">Optionally, hello setting can be turned off in hello configuration of hello application.</span></span>

- <span data-ttu-id="a497d-347">Hello oprava orchestration aplikace shromažďuje telemetrická data tootrack využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="a497d-347">hello patch orchestration app collects telemetry tootrack usage and performance.</span></span> <span data-ttu-id="a497d-348">telemetrie aplikace Hello následuje hello nastavení runtime Service Fabric hello nastavení telemetrie, (který je ve výchozím).</span><span class="sxs-lookup"><span data-stu-id="a497d-348">hello application’s telemetry follows hello setting of hello Service Fabric runtime’s telemetry setting (which is on by default).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a497d-349">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a497d-349">Troubleshooting</span></span>

### <a name="a-node-is-not-coming-back-tooup-state"></a><span data-ttu-id="a497d-350">Uzel není vracející se zpět tooup stavu</span><span class="sxs-lookup"><span data-stu-id="a497d-350">A node is not coming back tooup state</span></span>

<span data-ttu-id="a497d-351">**uzel Hello se mohla zastavit v zakázání stavu, protože**:</span><span class="sxs-lookup"><span data-stu-id="a497d-351">**hello node might be stuck in a disabling state because**:</span></span>

<span data-ttu-id="a497d-352">Kontrola zabezpečení čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="a497d-352">A safety check is pending.</span></span> <span data-ttu-id="a497d-353">tooremedy této situaci, ujistěte se, že dostatek uzlů, jsou k dispozici v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="a497d-353">tooremedy this situation, ensure that enough nodes are available in a healthy state.</span></span>

<span data-ttu-id="a497d-354">**uzel Hello se mohla zastavit v zakázaném stavu, protože**:</span><span class="sxs-lookup"><span data-stu-id="a497d-354">**hello node might be stuck in a disabled state because**:</span></span>

- <span data-ttu-id="a497d-355">Hello uzlu zakázal ručně.</span><span class="sxs-lookup"><span data-stu-id="a497d-355">hello node was disabled manually.</span></span>
- <span data-ttu-id="a497d-356">Hello uzel byl zakázán z důvodu tooan probíhající infrastrukturu Azure úlohy.</span><span class="sxs-lookup"><span data-stu-id="a497d-356">hello node was disabled due tooan ongoing Azure infrastructure job.</span></span>
- <span data-ttu-id="a497d-357">uzel Hello byla dočasně zakázána správcem hello oprava orchestration aplikace toopatch hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-357">hello node was disabled temporarily by hello patch orchestration app toopatch hello node.</span></span>

<span data-ttu-id="a497d-358">**Hello uzlu se mohla zastavit v dolní stav protože**:</span><span class="sxs-lookup"><span data-stu-id="a497d-358">**hello node might be stuck in a down state because**:</span></span>

- <span data-ttu-id="a497d-359">uzel Hello byla zařazena v dolů stavu ručně.</span><span class="sxs-lookup"><span data-stu-id="a497d-359">hello node was put in a down state manually.</span></span>
- <span data-ttu-id="a497d-360">Hello uzlu právě probíhá restartování počítače (který může být aktivovány hello oprava orchestration aplikace).</span><span class="sxs-lookup"><span data-stu-id="a497d-360">hello node is undergoing a restart (which might be triggered by hello patch orchestration app).</span></span>
- <span data-ttu-id="a497d-361">uzel Hello je mimo provoz z důvodu tooa vadný virtuálního počítače nebo počítače nebo síťové problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="a497d-361">hello node is down due tooa faulty VM or machine or network connectivity issues.</span></span>

### <a name="updates-were-skipped-on-some-nodes"></a><span data-ttu-id="a497d-362">Aktualizace, které byly přeskočeny na některých uzlech</span><span class="sxs-lookup"><span data-stu-id="a497d-362">Updates were skipped on some nodes</span></span>

<span data-ttu-id="a497d-363">Hello oprava orchestration aplikace pokusí tooinstall aktualizace podle toohello přeplánování zásad pro Windows.</span><span class="sxs-lookup"><span data-stu-id="a497d-363">hello patch orchestration app tries tooinstall a Windows update according toohello rescheduling policy.</span></span> <span data-ttu-id="a497d-364">Služba Hello pokusí toorecover hello uzlu a přeskočit zásady podle toohello aplikace hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a497d-364">hello service tries toorecover hello node and skip hello update according toohello application policy.</span></span>

<span data-ttu-id="a497d-365">V takovém případě se vygeneruje sestavy stavu úroveň pro upozornění před hello Služba agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="a497d-365">In such a case, a warning-level health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="a497d-366">Hello výsledek pro služby Windows Update také obsahuje hello možných příčin selhání hello.</span><span class="sxs-lookup"><span data-stu-id="a497d-366">hello result for Windows Update also contains hello possible reason for hello failure.</span></span>

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a><span data-ttu-id="a497d-367">Hello stav clusteru hello přejde tooerror při instalaci aktualizace hello</span><span class="sxs-lookup"><span data-stu-id="a497d-367">hello health of hello cluster goes tooerror while hello update installs</span></span>

<span data-ttu-id="a497d-368">Vadný služby Windows update můžete zahrnout dolů hello stavu aplikace nebo clusteru v konkrétním uzlu nebo doméně pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="a497d-368">A faulty Windows update can bring down hello health of an application or cluster on a particular node or upgrade domain.</span></span> <span data-ttu-id="a497d-369">Hello oprava orchestration aplikace ze všechny následné operace služby Windows Update, dokud nebude hello clusteru znovu v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a497d-369">hello patch orchestration app discontinues any subsequent Windows Update operation until hello cluster is healthy again.</span></span>

<span data-ttu-id="a497d-370">Správce musí zasáhnout a zjistit, proč k problému z důvodu aplikace hello nebo clusteru tooWindows aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a497d-370">An administrator must intervene and determine why hello application or cluster became unhealthy due tooWindows Update.</span></span>

## <a name="release-notes-"></a><span data-ttu-id="a497d-371">Poznámky k verzi:</span><span class="sxs-lookup"><span data-stu-id="a497d-371">Release Notes :</span></span>

### <a name="version-110"></a><span data-ttu-id="a497d-372">Verze 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a497d-372">Version 1.1.0</span></span>
- <span data-ttu-id="a497d-373">Veřejné verze</span><span class="sxs-lookup"><span data-stu-id="a497d-373">Public release</span></span>

### <a name="version-111"></a><span data-ttu-id="a497d-374">Verze 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a497d-374">Version 1.1.1</span></span>
- <span data-ttu-id="a497d-375">Pevná chyby ve SetupEntryPoint z NodeAgentService, která zabránila instalace NodeAgentNTService.</span><span class="sxs-lookup"><span data-stu-id="a497d-375">Fixed a bug in SetupEntryPoint of NodeAgentService that prevented installation of NodeAgentNTService.</span></span>

### <a name="version-120-latest"></a><span data-ttu-id="a497d-376">Verze 1.2.0 (nejnovější)</span><span class="sxs-lookup"><span data-stu-id="a497d-376">Version 1.2.0 (Latest)</span></span>

- <span data-ttu-id="a497d-377">Opravy chyb kolem systém restartovat pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a497d-377">Bug fixes around system restart workflow.</span></span>
- <span data-ttu-id="a497d-378">Oprava chyby při vytváření úlohy RM kvůli toowhich Kontrola stavu během přípravy úlohy oprava nebyla děje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="a497d-378">Bug fix in creation of RM tasks due toowhich health check during preparing repair tasks wasn't happening as expected.</span></span>
- <span data-ttu-id="a497d-379">Změněné hello režimu spuštění pro službu systému windows POANodeSvc z toodelayed automaticky automaticky.</span><span class="sxs-lookup"><span data-stu-id="a497d-379">Changed hello startup mode for windows service POANodeSvc from auto toodelayed-auto.</span></span>
