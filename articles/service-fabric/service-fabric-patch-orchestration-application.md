---
title: Azure Service Fabric oprava orchestration aplikace | Microsoft Docs
description: "Aplikace pro automatizaci opravy operačního systému na cluster Service Fabric."
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
ms.openlocfilehash: 2c5842822e347113e388d570f6ae603a313944d6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a><span data-ttu-id="12f7d-103">Oprava operačního systému Windows v clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="12f7d-103">Patch the Windows operating system in your Service Fabric cluster</span></span>

<span data-ttu-id="12f7d-104">Oprava aplikace orchestration je aplikace Azure Service Fabric, která automatizuje operačního systému opravy na cluster Service Fabric v Azure bez výpadků.</span><span class="sxs-lookup"><span data-stu-id="12f7d-104">The patch orchestration application is an Azure Service Fabric application that automates operating system patching on a Service Fabric cluster on Azure without downtime.</span></span>

<span data-ttu-id="12f7d-105">Oprava aplikace orchestration obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="12f7d-105">The patch orchestration app provides the following:</span></span>

- <span data-ttu-id="12f7d-106">**Automatická instalace operačního systému aktualizaci**.</span><span class="sxs-lookup"><span data-stu-id="12f7d-106">**Automatic operating system update installation**.</span></span> <span data-ttu-id="12f7d-107">Aktualizace operačního systému se automaticky stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="12f7d-107">Operating system updates are automatically downloaded and installed.</span></span> <span data-ttu-id="12f7d-108">Uzly se restartují podle potřeby a bez výpadku clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-108">Cluster nodes are rebooted as needed without cluster downtime.</span></span>

- <span data-ttu-id="12f7d-109">**Clustery opravy a stavu integrace**.</span><span class="sxs-lookup"><span data-stu-id="12f7d-109">**Cluster-aware patching and health integration**.</span></span> <span data-ttu-id="12f7d-110">Při použití aktualizací, aplikace orchestration oprava monitoruje stav uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-110">While applying updates, the patch orchestration app monitors the health of the cluster nodes.</span></span> <span data-ttu-id="12f7d-111">Upgradovaná jeden uzel nebo jednu upgradovací doménu jsou uzly clusteru současně.</span><span class="sxs-lookup"><span data-stu-id="12f7d-111">Cluster nodes are upgraded one node at a time or one upgrade domain at a time.</span></span> <span data-ttu-id="12f7d-112">Pokud stav clusteru ocitne mimo provoz z důvodu proces oprav, opravy je zastavena, aby zhorší problém.</span><span class="sxs-lookup"><span data-stu-id="12f7d-112">If the health of the cluster goes down due to the patching process, patching is stopped to prevent aggravating the problem.</span></span>

## <a name="internal-details-of-the-app"></a><span data-ttu-id="12f7d-113">Interní podrobnosti o aplikaci</span><span class="sxs-lookup"><span data-stu-id="12f7d-113">Internal details of the app</span></span>

<span data-ttu-id="12f7d-114">Oprava aplikace orchestration se skládá z následujících tyto dílčí součásti:</span><span class="sxs-lookup"><span data-stu-id="12f7d-114">The patch orchestration app is composed of the following subcomponents:</span></span>

- <span data-ttu-id="12f7d-115">**Služba Koordinátor**: Tato stavová služba je zodpovědná za:</span><span class="sxs-lookup"><span data-stu-id="12f7d-115">**Coordinator Service**: This stateful service is responsible for:</span></span>
    - <span data-ttu-id="12f7d-116">Koordinace úlohu služby Windows Update na celý cluster.</span><span class="sxs-lookup"><span data-stu-id="12f7d-116">Coordinating the Windows Update job on the entire cluster.</span></span>
    - <span data-ttu-id="12f7d-117">Ukládání výsledek dokončené operace aplikace Windows Update.</span><span class="sxs-lookup"><span data-stu-id="12f7d-117">Storing the result of completed Windows Update operations.</span></span>
- <span data-ttu-id="12f7d-118">**Služba agenta uzlu**: Tento bezstavové služby běží na všech uzlech clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12f7d-118">**Node Agent Service**: This stateless service runs on all Service Fabric cluster nodes.</span></span> <span data-ttu-id="12f7d-119">Služba je zodpovědná za:</span><span class="sxs-lookup"><span data-stu-id="12f7d-119">The service is responsible for:</span></span>
    - <span data-ttu-id="12f7d-120">Zavádění NTService agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-120">Bootstrapping the Node Agent NTService.</span></span>
    - <span data-ttu-id="12f7d-121">Monitorování NTService agenta uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-121">Monitoring the Node Agent NTService.</span></span>
- <span data-ttu-id="12f7d-122">**Uzel agenta NTService**: Tento systém Windows NT služba běží na vyšší úrovni oprávnění (systém).</span><span class="sxs-lookup"><span data-stu-id="12f7d-122">**Node Agent NTService**: This Windows NT service runs at a higher-level privilege (SYSTEM).</span></span> <span data-ttu-id="12f7d-123">Naproti tomu Služba agenta uzlu a službu koordinátora spustit na nižší úrovni oprávnění (síťová služba).</span><span class="sxs-lookup"><span data-stu-id="12f7d-123">In contrast, the Node Agent Service and the Coordinator Service run at a lower-level privilege (NETWORK SERVICE).</span></span> <span data-ttu-id="12f7d-124">Služba je zodpovědná za provádění následujících úloh aktualizace systému Windows na všech uzlech clusteru:</span><span class="sxs-lookup"><span data-stu-id="12f7d-124">The service is responsible for performing the following Windows Update jobs on all the cluster nodes:</span></span>
    - <span data-ttu-id="12f7d-125">Zakázání automatické aktualizace systému Windows na uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-125">Disabling automatic Windows Update on the node.</span></span>
    - <span data-ttu-id="12f7d-126">Stažení a instalaci služby Windows Update podle zásad uživatel zadal.</span><span class="sxs-lookup"><span data-stu-id="12f7d-126">Downloading and installing Windows Update according to the policy the user has provided.</span></span>
    - <span data-ttu-id="12f7d-127">Restartování počítače post instalace služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="12f7d-127">Restarting the machine post Windows Update installation.</span></span>
    - <span data-ttu-id="12f7d-128">Výsledky aktualizace systému Windows se nahrávají na službu Koordinátor.</span><span class="sxs-lookup"><span data-stu-id="12f7d-128">Uploading the results of Windows updates to the Coordinator Service.</span></span>
    - <span data-ttu-id="12f7d-129">Vytváření sestav stavu hlásí v případě, že operace se nezdařilo po jejím vyčerpání všechny opakování.</span><span class="sxs-lookup"><span data-stu-id="12f7d-129">Reporting health reports in case an operation has failed after exhausting all retries.</span></span>

> [!NOTE]
> <span data-ttu-id="12f7d-130">Oprava aplikace orchestration používá službu Service Fabric opravy správce systému pro zakázání nebo povolení uzlu a provádění kontroly stavu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-130">The patch orchestration app uses the Service Fabric repair manager system service to disable or enable the node and perform health checks.</span></span> <span data-ttu-id="12f7d-131">Úloha opravy vytvořené aplikací orchestration oprava sleduje průběh Windows Update pro každý uzel.</span><span class="sxs-lookup"><span data-stu-id="12f7d-131">The repair task created by the patch orchestration app tracks the Windows Update progress for each node.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12f7d-132">Požadavky</span><span class="sxs-lookup"><span data-stu-id="12f7d-132">Prerequisites</span></span>

### <a name="minimum-supported-service-fabric-runtime-version"></a><span data-ttu-id="12f7d-133">Minimální podporovaná verze modulu runtime Service Fabric</span><span class="sxs-lookup"><span data-stu-id="12f7d-133">Minimum supported Service Fabric runtime version</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="12f7d-134">Azure clustery</span><span class="sxs-lookup"><span data-stu-id="12f7d-134">Azure clusters</span></span>
<span data-ttu-id="12f7d-135">Oprava orchestration aplikace musí být spuštěn na Azure clustery, které mají v5.5 verzi modulu runtime Service Fabric nebo novější.</span><span class="sxs-lookup"><span data-stu-id="12f7d-135">The patch orchestration app must be run on Azure clusters that have Service Fabric runtime version v5.5 or later.</span></span>

#### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="12f7d-136">Samostatné místní clustery</span><span class="sxs-lookup"><span data-stu-id="12f7d-136">Standalone on-premises Clusters</span></span>
<span data-ttu-id="12f7d-137">Oprava orchestration aplikace musí být spuštěn na samostatné clustery, které mají verze 5.6 verzi modulu runtime Service Fabric nebo novější.</span><span class="sxs-lookup"><span data-stu-id="12f7d-137">The patch orchestration app must be run on Standalone clusters that have Service Fabric runtime version v5.6 or later.</span></span>

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a><span data-ttu-id="12f7d-138">Povolit službu opravy správce (Pokud již není spuštěn)</span><span class="sxs-lookup"><span data-stu-id="12f7d-138">Enable the repair manager service (if it's not running already)</span></span>

<span data-ttu-id="12f7d-139">Aplikace orchestration oprava vyžaduje službu opravy správce systému povolení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-139">The patch orchestration app requires the repair manager system service to be enabled on the cluster.</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="12f7d-140">Azure clustery</span><span class="sxs-lookup"><span data-stu-id="12f7d-140">Azure clusters</span></span>

<span data-ttu-id="12f7d-141">Azure clustery ve vrstvě stříbrným odolnost mají službu opravy manager ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="12f7d-141">Azure clusters in the silver durability tier have the repair manager service enabled by default.</span></span> <span data-ttu-id="12f7d-142">Azure clusterů ve vrstvě gold odolnost může nebo nemusí mít službu správce oprava povoleno, v závislosti na tom, kdy byly vytvořeny těchto clusterů.</span><span class="sxs-lookup"><span data-stu-id="12f7d-142">Azure clusters in the gold durability tier might or might not have the repair manager service enabled, depending on when those clusters were created.</span></span> <span data-ttu-id="12f7d-143">Azure clusterů ve vrstvě bronzovým odolnost ve výchozím nastavení, není nutné službu opravy manager povolena.</span><span class="sxs-lookup"><span data-stu-id="12f7d-143">Azure clusters in the bronze durability tier, by default, do not have the repair manager service enabled.</span></span> <span data-ttu-id="12f7d-144">Pokud služba je již povolen, zobrazí se jeho spuštění v části systémové služby v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-144">If the service is already enabled, you can see it running in the system services section in the Service Fabric Explorer.</span></span>

##### <a name="azure-portal"></a><span data-ttu-id="12f7d-145">portál Azure</span><span class="sxs-lookup"><span data-stu-id="12f7d-145">Azure portal</span></span>
<span data-ttu-id="12f7d-146">Opravte manager z portálu Azure můžete povolit v době nastavování clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-146">You can enable repair manager from Azure portal at the time of setting up of cluster.</span></span> <span data-ttu-id="12f7d-147">Vyberte `Include Repair Manager` možnost pod `Add on features` v době konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-147">Select `Include Repair Manager` option under `Add on features` at the time of Cluster configuration.</span></span>
<span data-ttu-id="12f7d-148">![Image Manager opravy povolení z portálu Azure](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span><span class="sxs-lookup"><span data-stu-id="12f7d-148">![Image of Enabling Repair Manager from Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span></span>

##### <a name="azure-resource-manager-template"></a><span data-ttu-id="12f7d-149">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="12f7d-149">Azure Resource Manager template</span></span>
<span data-ttu-id="12f7d-150">Případně můžete použít [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) povolit službu opravy správce v nových nebo stávajících clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12f7d-150">Alternatively you can use the [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) to enable the repair manager service on new and existing Service Fabric clusters.</span></span> <span data-ttu-id="12f7d-151">Získáte šablonu pro cluster, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="12f7d-151">Get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="12f7d-152">Můžete použít ukázkové šablony, nebo vytvořit vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-152">You can either use the sample templates or create a custom Resource Manager template.</span></span> 

<span data-ttu-id="12f7d-153">Povolit pomocí service manager oprava [šablony Azure Resource Manageru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span><span class="sxs-lookup"><span data-stu-id="12f7d-153">To enable the repair manager service using [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span></span>

1. <span data-ttu-id="12f7d-154">Nejdřív zkontrolujte, zda `apiversion` je nastaven na `2017-07-01-preview` pro `Microsoft.ServiceFabric/clusters` prostředků, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-154">First check that the `apiversion` is set to `2017-07-01-preview` for the `Microsoft.ServiceFabric/clusters` resource, as shown in the following snippet.</span></span> <span data-ttu-id="12f7d-155">Pokud se liší, pak je potřeba aktualizovat `apiVersion` na hodnotu `2017-07-01-preview`:</span><span class="sxs-lookup"><span data-stu-id="12f7d-155">If it is different, then you need to update the `apiVersion` to the value `2017-07-01-preview`:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="12f7d-156">Teď povolit službu správce opravy přidáním následující `addonFeatures` části po `fabricSettings` části:</span><span class="sxs-lookup"><span data-stu-id="12f7d-156">Now enable the repair manager service by adding the following `addonFeatures` section after the `fabricSettings` section:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="12f7d-157">Po aktualizaci šablony clusteru se tyto změny použít, je a mohli dokončit upgrade.</span><span class="sxs-lookup"><span data-stu-id="12f7d-157">After you have updated your cluster template with these changes, apply them and let the upgrade finish.</span></span> <span data-ttu-id="12f7d-158">Nyní můžete vidět službu system manager oprava běžící v clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-158">You can now see the repair manager system service running in your cluster.</span></span> <span data-ttu-id="12f7d-159">Je volána `fabric:/System/RepairManagerService` v části systémové služby v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-159">It is called `fabric:/System/RepairManagerService` in the system services section in the Service Fabric Explorer.</span></span> 

### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="12f7d-160">Samostatné místní clustery</span><span class="sxs-lookup"><span data-stu-id="12f7d-160">Standalone on-premises clusters</span></span>

<span data-ttu-id="12f7d-161">Můžete použít [nastavení konfigurace pro samostatný cluster Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) povolit službu opravy správce na nové a stávající cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12f7d-161">You can use the [Configuration settings for standalone Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) to enable the repair manager service on new and existing Service Fabric cluster.</span></span>

<span data-ttu-id="12f7d-162">Chcete-li povolit službu opravy správce:</span><span class="sxs-lookup"><span data-stu-id="12f7d-162">To enable the repair manager service:</span></span>

1. <span data-ttu-id="12f7d-163">Nejdřív zkontrolujte, zda `apiversion` v [konfigurace obecné clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) je nastaven na `04-2017` nebo vyšší:</span><span class="sxs-lookup"><span data-stu-id="12f7d-163">First check that the `apiversion` in [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is set to `04-2017` or higher:</span></span>

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. <span data-ttu-id="12f7d-164">Teď povolit service manager opravy přidáním následující `addonFeaturres` části po `fabricSettings` části, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="12f7d-164">Now enable repair manager service by adding the following `addonFeaturres` section after the `fabricSettings` section as shown below:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="12f7d-165">Aktualizovat manifestu clusteru se tyto změny pomocí v manifestu clusteru aktualizované [vytvoření nového clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) nebo [upgradovat konfiguraci clusteru](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span><span class="sxs-lookup"><span data-stu-id="12f7d-165">Update your cluster manifest with these changes, using the updated cluster manifest [create a new cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) or [upgrade the cluster configuration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span></span> <span data-ttu-id="12f7d-166">Jakmile v clusteru je spuštěn s manifestu aktualizované clusteru, se zobrazí služba system manager oprava běžící v clusteru, který se označuje `fabric:/System/RepairManagerService`v systému služby v Service Fabric explorer části.</span><span class="sxs-lookup"><span data-stu-id="12f7d-166">Once the cluster is running with updated cluster manifest, you can now see the repair manager system service running in your cluster, which is called `fabric:/System/RepairManagerService`, under system services section in the Service Fabric explorer.</span></span>

### <a name="disable-automatic-windows-update-on-all-nodes"></a><span data-ttu-id="12f7d-167">Zakázat automatické aktualizace systému Windows na všech uzlech</span><span class="sxs-lookup"><span data-stu-id="12f7d-167">Disable automatic Windows Update on all nodes</span></span>

<span data-ttu-id="12f7d-168">Automatické aktualizace systému Windows můžou způsobit ztrátu dostupnosti, protože více uzlech clusteru můžete restartovat ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-168">Automatic Windows updates might lead to availability loss because multiple cluster nodes can restart at the same time.</span></span> <span data-ttu-id="12f7d-169">Oprava aplikace orchestration, ve výchozím nastavení, pokusí se zakázat automatické aktualizace systému Windows na všech uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-169">The patch orchestration app, by default, tries to disable the automatic Windows Update on each cluster node.</span></span> <span data-ttu-id="12f7d-170">Pokud nastavení spravuje správce nebo zásad skupiny, doporučujeme však explicitně nastavení zásad Windows Update "Oznámit před stáhnout".</span><span class="sxs-lookup"><span data-stu-id="12f7d-170">However, if the settings are managed by an administrator or group policy, we recommend setting the Windows Update policy to “Notify before Download” explicitly.</span></span>

### <a name="optional-enable-azure-diagnostics"></a><span data-ttu-id="12f7d-171">Volitelné: Povolení Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="12f7d-171">Optional: Enable Azure Diagnostics</span></span>

<span data-ttu-id="12f7d-172">Clustery se systémem verzi modulu runtime Service Fabric `5.6.220.9494` a výše shromažďování oprava orchestration aplikace protokoly jako součást Service Fabric protokoly.</span><span class="sxs-lookup"><span data-stu-id="12f7d-172">Clusters running Service Fabric runtime version `5.6.220.9494` and above collect patch orchestration app logs as part of Service Fabric logs.</span></span>
<span data-ttu-id="12f7d-173">Pokud váš cluster běží na verzi modulu runtime Service Fabric, můžete tento krok přeskočit `5.6.220.9494` a vyšší.</span><span class="sxs-lookup"><span data-stu-id="12f7d-173">You can skip this step if your cluster is running on Service Fabric runtime version `5.6.220.9494` and above.</span></span>

<span data-ttu-id="12f7d-174">Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, protokoly pro aplikaci orchestration opravy se shromažďují místně na každém uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-174">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs for the patch orchestration app are collected locally on each of the cluster nodes.</span></span>
<span data-ttu-id="12f7d-175">Doporučujeme vám, že nakonfigurujete Azure Diagnostics odeslat protokoly ze všech uzlů do centrálního umístění.</span><span class="sxs-lookup"><span data-stu-id="12f7d-175">We recommend that you configure Azure Diagnostics to upload logs from all nodes to a central location.</span></span>

<span data-ttu-id="12f7d-176">Informace o povolení Azure Diagnostics najdete v tématu [shromažďování protokolů pomocí Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span><span class="sxs-lookup"><span data-stu-id="12f7d-176">For information on enabling Azure Diagnostics, see [Collect logs by using Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span></span>

<span data-ttu-id="12f7d-177">Protokoly pro aplikaci orchestration opravy se generují u tohoto zprostředkovatele pevné ID:</span><span class="sxs-lookup"><span data-stu-id="12f7d-177">Logs for the patch orchestration app are generated on the following fixed provider IDs:</span></span>

- <span data-ttu-id="12f7d-178">e39b723c-590c-4090-abb0-11e3e6616346</span><span class="sxs-lookup"><span data-stu-id="12f7d-178">e39b723c-590c-4090-abb0-11e3e6616346</span></span>
- <span data-ttu-id="12f7d-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span><span class="sxs-lookup"><span data-stu-id="12f7d-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span></span>
- <span data-ttu-id="12f7d-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span><span class="sxs-lookup"><span data-stu-id="12f7d-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span></span>
- <span data-ttu-id="12f7d-181">05dc046c-60e9-4ef7-965e-91660adffa68</span><span class="sxs-lookup"><span data-stu-id="12f7d-181">05dc046c-60e9-4ef7-965e-91660adffa68</span></span>

<span data-ttu-id="12f7d-182">V goto šablony Resource Manageru `EtwEventSourceProviderConfiguration` oddílu pod `WadCfg` a přidejte následující položky:</span><span class="sxs-lookup"><span data-stu-id="12f7d-182">In Resource Manager template goto `EtwEventSourceProviderConfiguration` section under `WadCfg` and add the following entries:</span></span>

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
> <span data-ttu-id="12f7d-183">Pokud váš cluster Service Fabric má více typů uzlu, pak v předchozí části, je třeba přidat k všechny `WadCfg` oddíly.</span><span class="sxs-lookup"><span data-stu-id="12f7d-183">If your Service Fabric cluster has multiple node types, then the previous section must be added for all the `WadCfg` sections.</span></span>

## <a name="download-the-app-package"></a><span data-ttu-id="12f7d-184">Stáhněte si balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="12f7d-184">Download the app package</span></span>

<span data-ttu-id="12f7d-185">Stažení aplikace z [stáhnout odkaz](https://go.microsoft.com/fwlink/P/?linkid=849590).</span><span class="sxs-lookup"><span data-stu-id="12f7d-185">Download the application from the [download link](https://go.microsoft.com/fwlink/P/?linkid=849590).</span></span>

## <a name="configure-the-app"></a><span data-ttu-id="12f7d-186">Konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="12f7d-186">Configure the app</span></span>

<span data-ttu-id="12f7d-187">Chování aplikace orchestration oprava lze nakonfigurovat podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="12f7d-187">The behavior of the patch orchestration app can be configured to meet your needs.</span></span> <span data-ttu-id="12f7d-188">Přepište výchozí hodnoty předáním v aplikaci parametru během vytváření aplikace nebo aktualizace.</span><span class="sxs-lookup"><span data-stu-id="12f7d-188">Override the default values by passing in the application parameter during application creation or update.</span></span> <span data-ttu-id="12f7d-189">Parametry aplikačního lze zadat zadáním `ApplicationParameter` k `Start-ServiceFabricApplicationUpgrade` nebo `New-ServiceFabricApplication` rutiny.</span><span class="sxs-lookup"><span data-stu-id="12f7d-189">Application parameters can be provided by specifying `ApplicationParameter` to the `Start-ServiceFabricApplicationUpgrade` or `New-ServiceFabricApplication` cmdlets.</span></span>

|<span data-ttu-id="12f7d-190">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="12f7d-190">**Parameter**</span></span>        |<span data-ttu-id="12f7d-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="12f7d-191">**Type**</span></span>                          | <span data-ttu-id="12f7d-192">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="12f7d-192">**Details**</span></span>|
|:-|-|-|
|<span data-ttu-id="12f7d-193">MaxResultsToCache</span><span class="sxs-lookup"><span data-stu-id="12f7d-193">MaxResultsToCache</span></span>    |<span data-ttu-id="12f7d-194">Dlouhé</span><span class="sxs-lookup"><span data-stu-id="12f7d-194">Long</span></span>                              | <span data-ttu-id="12f7d-195">Maximální počet výsledků Windows Update, které by měl být mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="12f7d-195">Maximum number of Windows Update results, which should be cached.</span></span> <br><span data-ttu-id="12f7d-196">Výchozí hodnota je 3000 za předpokladu, že:</span><span class="sxs-lookup"><span data-stu-id="12f7d-196">Default value is 3000 assuming the:</span></span> <br> <span data-ttu-id="12f7d-197">-Počet uzlů je 20.</span><span class="sxs-lookup"><span data-stu-id="12f7d-197">- Number of nodes is 20.</span></span> <br> <span data-ttu-id="12f7d-198">-Počet aktualizací děje na uzlu za měsíc je pět.</span><span class="sxs-lookup"><span data-stu-id="12f7d-198">- Number of updates happening on a node per month is five.</span></span> <br> <span data-ttu-id="12f7d-199">-Počet výsledků na operace může být 10.</span><span class="sxs-lookup"><span data-stu-id="12f7d-199">- Number of results per operation can be 10.</span></span> <br> <span data-ttu-id="12f7d-200">-By měly být uložené výsledky pro poslední tři měsíce.</span><span class="sxs-lookup"><span data-stu-id="12f7d-200">- Results for the past three months should be stored.</span></span> |
|<span data-ttu-id="12f7d-201">TaskApprovalPolicy</span><span class="sxs-lookup"><span data-stu-id="12f7d-201">TaskApprovalPolicy</span></span>   |<span data-ttu-id="12f7d-202">výčet</span><span class="sxs-lookup"><span data-stu-id="12f7d-202">Enum</span></span> <br> <span data-ttu-id="12f7d-203">{NodeWise, UpgradeDomainWise}</span><span class="sxs-lookup"><span data-stu-id="12f7d-203">{ NodeWise, UpgradeDomainWise }</span></span>                          |<span data-ttu-id="12f7d-204">TaskApprovalPolicy označuje zásady, které má být používána službu koordinátora k instalaci aktualizací Windows mezi uzly clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12f7d-204">TaskApprovalPolicy indicates the policy that is to be used by the Coordinator Service to install Windows updates across the Service Fabric cluster nodes.</span></span><br>                         <span data-ttu-id="12f7d-205">Povolené hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="12f7d-205">Allowed values are:</span></span> <br>                                                           <span data-ttu-id="12f7d-206"><b>NodeWise</b>.</span><span class="sxs-lookup"><span data-stu-id="12f7d-206"><b>NodeWise</b>.</span></span> <span data-ttu-id="12f7d-207">Windows Update je nainstalovaná jednoho uzlu současně.</span><span class="sxs-lookup"><span data-stu-id="12f7d-207">Windows Update is installed one node at a time.</span></span> <br>                                                           <span data-ttu-id="12f7d-208"><b>UpgradeDomainWise</b>.</span><span class="sxs-lookup"><span data-stu-id="12f7d-208"><b>UpgradeDomainWise</b>.</span></span> <span data-ttu-id="12f7d-209">Windows Update je nainstalovaná jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="12f7d-209">Windows Update is installed one upgrade domain at a time.</span></span> <span data-ttu-id="12f7d-210">(Na maximum, můžete přejít všechny uzly, které patří k doméně upgradu pro Windows Update.)</span><span class="sxs-lookup"><span data-stu-id="12f7d-210">(At the maximum, all the nodes belonging to an upgrade domain can go for Windows Update.)</span></span>
|<span data-ttu-id="12f7d-211">LogsDiskQuotaInMB</span><span class="sxs-lookup"><span data-stu-id="12f7d-211">LogsDiskQuotaInMB</span></span>   |<span data-ttu-id="12f7d-212">Dlouhé</span><span class="sxs-lookup"><span data-stu-id="12f7d-212">Long</span></span>  <br> <span data-ttu-id="12f7d-213">(Výchozí: 1024)</span><span class="sxs-lookup"><span data-stu-id="12f7d-213">(Default: 1024)</span></span>               |<span data-ttu-id="12f7d-214">Maximální velikost oprava orchestration aplikace přihlásí MB, který můžete nastavit jako trvalý, místně na uzlech.</span><span class="sxs-lookup"><span data-stu-id="12f7d-214">Maximum size of patch orchestration app logs in MB, which can be persisted locally on nodes.</span></span>
| <span data-ttu-id="12f7d-215">WUQuery</span><span class="sxs-lookup"><span data-stu-id="12f7d-215">WUQuery</span></span>               | <span data-ttu-id="12f7d-216">Řetězec</span><span class="sxs-lookup"><span data-stu-id="12f7d-216">string</span></span><br><span data-ttu-id="12f7d-217">(Výchozí: "IsInstalled = 0")</span><span class="sxs-lookup"><span data-stu-id="12f7d-217">(Default: "IsInstalled=0")</span></span>                | <span data-ttu-id="12f7d-218">Dotaz pro získání aktualizace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="12f7d-218">Query to get Windows updates.</span></span> <span data-ttu-id="12f7d-219">Další informace najdete v tématu [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="12f7d-219">For more information, see [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span></span>
| <span data-ttu-id="12f7d-220">InstallWindowsOSOnlyUpdates</span><span class="sxs-lookup"><span data-stu-id="12f7d-220">InstallWindowsOSOnlyUpdates</span></span> | <span data-ttu-id="12f7d-221">BOOL</span><span class="sxs-lookup"><span data-stu-id="12f7d-221">Bool</span></span> <br> <span data-ttu-id="12f7d-222">(výchozí: True)</span><span class="sxs-lookup"><span data-stu-id="12f7d-222">(default: True)</span></span>                 | <span data-ttu-id="12f7d-223">Tento příznak umožňuje instalaci aktualizací operačního systému Windows.</span><span class="sxs-lookup"><span data-stu-id="12f7d-223">This flag allows Windows operating system updates to be installed.</span></span>            |
| <span data-ttu-id="12f7d-224">WUOperationTimeOutInMinutes</span><span class="sxs-lookup"><span data-stu-id="12f7d-224">WUOperationTimeOutInMinutes</span></span> | <span data-ttu-id="12f7d-225">celá čísla</span><span class="sxs-lookup"><span data-stu-id="12f7d-225">Int</span></span> <br><span data-ttu-id="12f7d-226">(Výchozí: 90).</span><span class="sxs-lookup"><span data-stu-id="12f7d-226">(Default: 90)</span></span>                   | <span data-ttu-id="12f7d-227">Určuje časový limit pro všechny operace služby Windows Update (hledání nebo stáhnout nebo nainstalovat).</span><span class="sxs-lookup"><span data-stu-id="12f7d-227">Specifies the timeout for any Windows Update operation (search or download or install).</span></span> <span data-ttu-id="12f7d-228">Pokud operaci nelze dokončit v rámci zadaného časového limitu, byl přerušen.</span><span class="sxs-lookup"><span data-stu-id="12f7d-228">If the operation is not completed within the specified timeout, it is aborted.</span></span>       |
| <span data-ttu-id="12f7d-229">WURescheduleCount</span><span class="sxs-lookup"><span data-stu-id="12f7d-229">WURescheduleCount</span></span>     | <span data-ttu-id="12f7d-230">celá čísla</span><span class="sxs-lookup"><span data-stu-id="12f7d-230">Int</span></span> <br> <span data-ttu-id="12f7d-231">(Výchozí: 5).</span><span class="sxs-lookup"><span data-stu-id="12f7d-231">(Default: 5)</span></span>                  | <span data-ttu-id="12f7d-232">Maximální počet časy službu přeplánuje Windows aktualizovat v případě, že operace selže trvalé.</span><span class="sxs-lookup"><span data-stu-id="12f7d-232">The maximum number of times the service reschedules the Windows update in case an operation fails persistently.</span></span>          |
| <span data-ttu-id="12f7d-233">WURescheduleTimeInMinutes</span><span class="sxs-lookup"><span data-stu-id="12f7d-233">WURescheduleTimeInMinutes</span></span> | <span data-ttu-id="12f7d-234">celá čísla</span><span class="sxs-lookup"><span data-stu-id="12f7d-234">Int</span></span> <br><span data-ttu-id="12f7d-235">(Výchozí: 30).</span><span class="sxs-lookup"><span data-stu-id="12f7d-235">(Default: 30)</span></span> | <span data-ttu-id="12f7d-236">Interval, ve kterém přeplánuje službu Windows update v případě, že chyba přetrvávat.</span><span class="sxs-lookup"><span data-stu-id="12f7d-236">The interval at which the service reschedules the Windows update in case failure persists.</span></span> |
| <span data-ttu-id="12f7d-237">WUFrequency</span><span class="sxs-lookup"><span data-stu-id="12f7d-237">WUFrequency</span></span>           | <span data-ttu-id="12f7d-238">Řetězce s hodnotami oddělenými čárkou (výchozí: "Každý týden, středu, 7:00:00")</span><span class="sxs-lookup"><span data-stu-id="12f7d-238">Comma-separated string (Default: "Weekly, Wednesday, 7:00:00")</span></span>     | <span data-ttu-id="12f7d-239">Četnost pro instalaci služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="12f7d-239">The frequency for installing Windows Update.</span></span> <span data-ttu-id="12f7d-240">Formát a možné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="12f7d-240">The format and possible values are:</span></span> <br><span data-ttu-id="12f7d-241">-Měsíců, DD, hh, například každý měsíc, 5, 12: 22:32.</span><span class="sxs-lookup"><span data-stu-id="12f7d-241">-   Monthly, DD,HH:MM:SS, for example, Monthly, 5,12:22:32.</span></span> <br> <span data-ttu-id="12f7d-242">-Každý týden, den, hh: mm:, pro příklad, týdně, úterý, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="12f7d-242">-   Weekly, DAY,HH:MM:SS, for example, Weekly, Tuesday, 12:22:32.</span></span>  <br> <span data-ttu-id="12f7d-243">-Denní, hh: mm:, např. denně, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="12f7d-243">-   Daily, HH:MM:SS, for example, Daily, 12:22:32.</span></span>  <br> <span data-ttu-id="12f7d-244">-Žádný označuje, že by neměl Windows Update provést.</span><span class="sxs-lookup"><span data-stu-id="12f7d-244">-  None indicates that Windows Update shouldn't be done.</span></span>  <br><br> <span data-ttu-id="12f7d-245">Všimněte si, že všechny časy jsou ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="12f7d-245">Note that all the times are in UTC.</span></span>|
| <span data-ttu-id="12f7d-246">AcceptWindowsUpdateEula</span><span class="sxs-lookup"><span data-stu-id="12f7d-246">AcceptWindowsUpdateEula</span></span> | <span data-ttu-id="12f7d-247">BOOL</span><span class="sxs-lookup"><span data-stu-id="12f7d-247">Bool</span></span> <br><span data-ttu-id="12f7d-248">(Výchozí: true)</span><span class="sxs-lookup"><span data-stu-id="12f7d-248">(Default: true)</span></span> | <span data-ttu-id="12f7d-249">Nastavením tohoto příznaku aplikace přijímá licenční smlouvy s koncovým uživatelem pro Windows Update jménem vlastník počítače.</span><span class="sxs-lookup"><span data-stu-id="12f7d-249">By setting this flag, the application accepts the End-User License Agreement for Windows Update on behalf of the owner of the machine.</span></span>              |

> [!TIP]
> <span data-ttu-id="12f7d-250">Pokud chcete, aby Windows Update provést okamžitě, nastavte `WUFrequency` podle času nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="12f7d-250">If you want Windows Update to happen immediately, set `WUFrequency` relative to the application deployment time.</span></span> <span data-ttu-id="12f7d-251">Předpokládejme například, že máte testovací pěti uzly clusteru a plánování nasazení aplikace na přibližně 5:00 PM UTC.</span><span class="sxs-lookup"><span data-stu-id="12f7d-251">For example, suppose that you have a five-node test cluster and plan to deploy the app at around 5:00 PM UTC.</span></span> <span data-ttu-id="12f7d-252">Pokud budete předpokládat, že upgradu aplikace nebo nasazení maximálně trvá 30 minut, nastavte WUFrequency jako "Denně, 17:30:00."</span><span class="sxs-lookup"><span data-stu-id="12f7d-252">If you assume that the application upgrade or deployment takes 30 minutes at the maximum, set the WUFrequency as "Daily, 17:30:00."</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="12f7d-253">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="12f7d-253">Deploy the app</span></span>

1. <span data-ttu-id="12f7d-254">Dokončete všechny požadované kroky při přípravě clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-254">Finish all the prerequisite steps to prepare the cluster.</span></span>
2. <span data-ttu-id="12f7d-255">Nasaďte aplikaci orchestration oprava stejně jako jiná aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="12f7d-255">Deploy the patch orchestration app like any other Service Fabric app.</span></span> <span data-ttu-id="12f7d-256">Aplikaci můžete nasadit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="12f7d-256">You can deploy the app by using PowerShell.</span></span> <span data-ttu-id="12f7d-257">Postupujte podle kroků v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="12f7d-257">Follow the steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>
3. <span data-ttu-id="12f7d-258">Konfigurace aplikace v době nasazení, předat `ApplicationParamater` k `New-ServiceFabricApplication` rutiny.</span><span class="sxs-lookup"><span data-stu-id="12f7d-258">To configure the application at the time of deployment, pass the `ApplicationParamater` to the `New-ServiceFabricApplication` cmdlet.</span></span> <span data-ttu-id="12f7d-259">Pro usnadnění nabízíme skript Deploy.ps1 spolu s aplikací.</span><span class="sxs-lookup"><span data-stu-id="12f7d-259">For your convenience, we’ve provided the script Deploy.ps1 along with the application.</span></span> <span data-ttu-id="12f7d-260">Použití skriptu:</span><span class="sxs-lookup"><span data-stu-id="12f7d-260">To use the script:</span></span>

    - <span data-ttu-id="12f7d-261">Připojení ke clusteru Service Fabric pomocí `Connect-ServiceFabricCluster`.</span><span class="sxs-lookup"><span data-stu-id="12f7d-261">Connect to a Service Fabric cluster by using `Connect-ServiceFabricCluster`.</span></span>
    - <span data-ttu-id="12f7d-262">Spustit skript prostředí PowerShell Deploy.ps1 s příslušnou `ApplicationParameter` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-262">Execute the PowerShell script Deploy.ps1 with the appropriate `ApplicationParameter` value.</span></span>

> [!NOTE]
> <span data-ttu-id="12f7d-263">Zachovat skript a složce aplikace PatchOrchestrationApplication ve stejném adresáři.</span><span class="sxs-lookup"><span data-stu-id="12f7d-263">Keep the script and the application folder PatchOrchestrationApplication in the same directory.</span></span>

## <a name="upgrade-the-app"></a><span data-ttu-id="12f7d-264">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="12f7d-264">Upgrade the app</span></span>

<span data-ttu-id="12f7d-265">Pokud chcete upgradovat stávající aplikace orchestration oprava pomocí prostředí PowerShell, postupujte podle kroků v [upgradu aplikace Service Fabric pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span><span class="sxs-lookup"><span data-stu-id="12f7d-265">To upgrade an existing patch orchestration app by using PowerShell, follow the steps in [Service Fabric application upgrade using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span></span>

## <a name="remove-the-app"></a><span data-ttu-id="12f7d-266">Odeberte aplikaci</span><span class="sxs-lookup"><span data-stu-id="12f7d-266">Remove the app</span></span>

<span data-ttu-id="12f7d-267">Chcete-li odebrat aplikace, postupujte podle kroků v [nasazení a odeberte aplikací pomocí prostředí PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="12f7d-267">To remove the application, follow the steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>

<span data-ttu-id="12f7d-268">Pro usnadnění nabízíme skript Undeploy.ps1 spolu s aplikací.</span><span class="sxs-lookup"><span data-stu-id="12f7d-268">For your convenience, we've provided the script Undeploy.ps1 along with the application.</span></span> <span data-ttu-id="12f7d-269">Použití skriptu:</span><span class="sxs-lookup"><span data-stu-id="12f7d-269">To use the script:</span></span>

  - <span data-ttu-id="12f7d-270">Připojení ke clusteru Service Fabric pomocí ```Connect-ServiceFabricCluster```.</span><span class="sxs-lookup"><span data-stu-id="12f7d-270">Connect to a Service Fabric cluster by using ```Connect-ServiceFabricCluster```.</span></span>

  - <span data-ttu-id="12f7d-271">Spusťte skript prostředí PowerShell Undeploy.ps1.</span><span class="sxs-lookup"><span data-stu-id="12f7d-271">Execute the PowerShell script Undeploy.ps1.</span></span>

> [!NOTE]
> <span data-ttu-id="12f7d-272">Zachovat skript a složce aplikace PatchOrchestrationApplication ve stejném adresáři.</span><span class="sxs-lookup"><span data-stu-id="12f7d-272">Keep the script and the application folder PatchOrchestrationApplication in the same directory.</span></span>

## <a name="view-the-windows-update-results"></a><span data-ttu-id="12f7d-273">Zobrazení výsledků Windows Update</span><span class="sxs-lookup"><span data-stu-id="12f7d-273">View the Windows Update results</span></span>

<span data-ttu-id="12f7d-274">Oprava aplikace orchestration zpřístupňuje rozhraní REST API zobrazit historická výsledky pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="12f7d-274">The patch orchestration app exposes REST APIs to display the historical results to the user.</span></span> <span data-ttu-id="12f7d-275">Příklad výsledku JSON:</span><span class="sxs-lookup"><span data-stu-id="12f7d-275">An example of the result JSON:</span></span>
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
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
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
<span data-ttu-id="12f7d-276">Pokud žádná aktualizace je ještě naplánováno, výsledkem JSON je prázdný.</span><span class="sxs-lookup"><span data-stu-id="12f7d-276">If no update is scheduled yet, the result JSON is empty.</span></span>

<span data-ttu-id="12f7d-277">Přihlaste se ke clusteru k dotazu služby Windows Update výsledky.</span><span class="sxs-lookup"><span data-stu-id="12f7d-277">Log in to the cluster to query Windows Update results.</span></span> <span data-ttu-id="12f7d-278">Poté zjistit adresu repliky pro primární službu koordinátora a stiskněte tlačítko adresu URL z prohlížeče: http://&lt;REPLIKY IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="12f7d-278">Then find out the replica address for the primary of the Coordinator Service, and hit the URL from the browser: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="12f7d-279">Koncový bod REST pro službu koordinátora má dynamický port.</span><span class="sxs-lookup"><span data-stu-id="12f7d-279">The REST endpoint for the Coordinator Service has a dynamic port.</span></span> <span data-ttu-id="12f7d-280">Pokud chcete zkontrolovat přesnou adresu URL, najdete v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-280">To check the exact URL, refer to the Service Fabric Explorer.</span></span> <span data-ttu-id="12f7d-281">Například jsou k dispozici na výsledky `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span><span class="sxs-lookup"><span data-stu-id="12f7d-281">For example, the results are available at `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span></span>

![Obrázek koncový bod REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


<span data-ttu-id="12f7d-283">Pokud je v clusteru je povoleno reverzní proxy server, můžete adresu URL z mimo cluster také přistupovat.</span><span class="sxs-lookup"><span data-stu-id="12f7d-283">If the reverse proxy is enabled on the cluster, you can access the URL from outside of the cluster as well.</span></span>
<span data-ttu-id="12f7d-284">Koncový bod, který musí být dosáhl je http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="12f7d-284">The endpoint that needs to be hit is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="12f7d-285">Pokud chcete povolit reverzní proxy server v clusteru, postupujte podle kroků v [reverzní proxy server v Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span><span class="sxs-lookup"><span data-stu-id="12f7d-285">To enable the reverse proxy on the cluster, follow the steps in [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span></span> 

> 
> [!WARNING]
> <span data-ttu-id="12f7d-286">Po dokončení konfigurace reverzní proxy server, všechny malých služby v clusteru, které zveřejňují koncový bod protokolu HTTP jsou adresovatelné z mimo cluster.</span><span class="sxs-lookup"><span data-stu-id="12f7d-286">After the reverse proxy is configured, all micro services in the cluster that expose an HTTP endpoint are addressable from outside the cluster.</span></span>

## <a name="diagnosticshealth-events"></a><span data-ttu-id="12f7d-287">Diagnostika stavu události</span><span class="sxs-lookup"><span data-stu-id="12f7d-287">Diagnostics/health events</span></span>

### <a name="collect-patch-orchestration-app-logs"></a><span data-ttu-id="12f7d-288">Protokolů shromažďování oprava orchestration aplikace</span><span class="sxs-lookup"><span data-stu-id="12f7d-288">Collect patch orchestration app logs</span></span>

<span data-ttu-id="12f7d-289">Oprava orchestration aplikace protokoly se shromažďují jako součást Service Fabric protokoly z modulu runtime verze `5.6.220.9494` a vyšší.</span><span class="sxs-lookup"><span data-stu-id="12f7d-289">Patch orchestration app logs are collected as part of Service Fabric logs from runtime version `5.6.220.9494` and above.</span></span>
<span data-ttu-id="12f7d-290">Pro clustery spuštěná verze modulu runtime Service Fabric menší než `5.6.220.9494`, protokoly se můžou shromažďovat pomocí jedné z následujících metod.</span><span class="sxs-lookup"><span data-stu-id="12f7d-290">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs can be collected by using one of the following methods.</span></span>

#### <a name="locally-on-each-node"></a><span data-ttu-id="12f7d-291">Místně na každém uzlu</span><span class="sxs-lookup"><span data-stu-id="12f7d-291">Locally on each node</span></span>

<span data-ttu-id="12f7d-292">Protokoly se shromažďují místně na každém uzlu clusteru Service Fabric Pokud verzi modulu runtime Service Fabric je menší než `5.6.220.9494`.</span><span class="sxs-lookup"><span data-stu-id="12f7d-292">Logs are collected locally on each Service Fabric cluster node if Service Fabric runtime version is less than `5.6.220.9494`.</span></span> <span data-ttu-id="12f7d-293">Umístění pro přístup k protokoly je \[Service Fabric\_instalace\_jednotka\]:\\PatchOrchestrationApplication\\protokoly.</span><span class="sxs-lookup"><span data-stu-id="12f7d-293">The location to access the logs is \[Service Fabric\_Installation\_Drive\]:\\PatchOrchestrationApplication\\logs.</span></span>

<span data-ttu-id="12f7d-294">Například pokud Service Fabric je nainstalován na jednotce D, cesta je D:\\PatchOrchestrationApplication\\protokoly.</span><span class="sxs-lookup"><span data-stu-id="12f7d-294">For example, if Service Fabric is installed on drive D, the path is D:\\PatchOrchestrationApplication\\logs.</span></span>

#### <a name="central-location"></a><span data-ttu-id="12f7d-295">Centrální umístění</span><span class="sxs-lookup"><span data-stu-id="12f7d-295">Central location</span></span>

<span data-ttu-id="12f7d-296">Pokud Azure Diagnostics je nakonfigurovaný jako součást požadovaných krocích, protokoly pro aplikaci orchestration opravy jsou k dispozici ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="12f7d-296">If Azure Diagnostics is configured as part of prerequisite steps, logs for the patch orchestration app are available in Azure Storage.</span></span>

### <a name="health-reports"></a><span data-ttu-id="12f7d-297">Sestavy o stavu</span><span class="sxs-lookup"><span data-stu-id="12f7d-297">Health reports</span></span>

<span data-ttu-id="12f7d-298">Oprava aplikace orchestration, publikuje také sestavy stavu proti službu koordinátora nebo službu Agent uzlu v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="12f7d-298">The patch orchestration app also publishes health reports against the Coordinator Service or the Node Agent Service in the following cases:</span></span>

#### <a name="a-windows-update-operation-failed"></a><span data-ttu-id="12f7d-299">Windows Update operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="12f7d-299">A Windows Update operation failed</span></span>

<span data-ttu-id="12f7d-300">Pokud na uzlu selže operace služby Windows Update, sestavy stavu se vygeneruje pro službu Agent uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-300">If a Windows Update operation fails on a node, a health report is generated against the Node Agent Service.</span></span> <span data-ttu-id="12f7d-301">Podrobnosti stavu sestavy obsahují název problematické uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-301">Details of the health report contain the problematic node name.</span></span>

<span data-ttu-id="12f7d-302">Po dokončení opravy je úspěšně na uzlu problematické, jsou automaticky vymazány sestavy.</span><span class="sxs-lookup"><span data-stu-id="12f7d-302">After patching is successfully completed on the problematic node, the report is automatically cleared.</span></span>

#### <a name="the-node-agent-ntservice-is-down"></a><span data-ttu-id="12f7d-303">NTService agenta uzlu je mimo provoz</span><span class="sxs-lookup"><span data-stu-id="12f7d-303">The Node Agent NTService is down</span></span>

<span data-ttu-id="12f7d-304">Pokud NTService agenta uzlu je vypnutý na uzlu, vygeneruje se na službu Agent uzlu sestavy stavu úroveň upozornění.</span><span class="sxs-lookup"><span data-stu-id="12f7d-304">If the Node Agent NTService is down on a node, a warning-level health report is generated against the Node Agent Service.</span></span>

#### <a name="the-repair-manager-service-is-not-enabled"></a><span data-ttu-id="12f7d-305">Služba Správce opravy není povolena.</span><span class="sxs-lookup"><span data-stu-id="12f7d-305">The repair manager service is not enabled</span></span>

<span data-ttu-id="12f7d-306">Pokud službu opravy manager nebyl nalezen v clusteru, se generuje sestavy stavu úroveň upozornění pro službu koordinátora.</span><span class="sxs-lookup"><span data-stu-id="12f7d-306">If the repair manager service is not found on the cluster, a warning-level health report is generated for the Coordinator Service.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="12f7d-307">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="12f7d-307">Frequently asked questions</span></span>

<span data-ttu-id="12f7d-308">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="12f7d-308">Q.</span></span> <span data-ttu-id="12f7d-309">**Proč se při spuštěné aplikaci orchestration oprava vidí Moje clusteru v chybovém stavu?**</span><span class="sxs-lookup"><span data-stu-id="12f7d-309">**Why do I see my cluster in an error state when the patch orchestration app is running?**</span></span>

<span data-ttu-id="12f7d-310">A.</span><span class="sxs-lookup"><span data-stu-id="12f7d-310">A.</span></span> <span data-ttu-id="12f7d-311">Během procesu instalace aplikace orchestration oprava zakáže nebo restartování uzlů, které mohou dočasně stav clusteru směrem dolů.</span><span class="sxs-lookup"><span data-stu-id="12f7d-311">During the installation process, the patch orchestration app disables or restarts nodes, which can temporarily result in the health of the cluster going down.</span></span>

<span data-ttu-id="12f7d-312">Na základě zásad pro aplikace, buď jeden uzel můžete přejděte během operace oprav *nebo* celá doména upgradu můžete současně přejděte.</span><span class="sxs-lookup"><span data-stu-id="12f7d-312">Based on the policy for the application, either one node can go down during a patching operation *or* an entire upgrade domain can go down simultaneously.</span></span>

<span data-ttu-id="12f7d-313">Na konci instalace služby Windows Update jsou opětovně uzly povolena post restartování.</span><span class="sxs-lookup"><span data-stu-id="12f7d-313">By the end of the Windows Update installation, the nodes are reenabled post restart.</span></span>

<span data-ttu-id="12f7d-314">V následujícím příkladu clusteru byl přesunut do chybový stav dočasně vzhledem k tomu, že dva uzly byly dolů a porušení zásady MaxPercentageUnhealthNodes.</span><span class="sxs-lookup"><span data-stu-id="12f7d-314">In the following example, the cluster went to an error state temporarily because two nodes were down and the MaxPercentageUnhealthNodes policy was violated.</span></span> <span data-ttu-id="12f7d-315">Chyba je dočasný, dokud probíhá oprav operaci.</span><span class="sxs-lookup"><span data-stu-id="12f7d-315">The error is temporary until the patching operation is ongoing.</span></span>

![Bitové kopie není v pořádku clusteru](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

<span data-ttu-id="12f7d-317">Pokud potíže potrvají, naleznete v části řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="12f7d-317">If the issue persists, refer to the Troubleshooting section.</span></span>

<span data-ttu-id="12f7d-318">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="12f7d-318">Q.</span></span> <span data-ttu-id="12f7d-319">**Oprava aplikace orchestration je ve stavu upozornění**</span><span class="sxs-lookup"><span data-stu-id="12f7d-319">**Patch orchestration app is in warning state**</span></span>

<span data-ttu-id="12f7d-320">A.</span><span class="sxs-lookup"><span data-stu-id="12f7d-320">A.</span></span> <span data-ttu-id="12f7d-321">Zkontrolujte, zda sestavy stavu odeslány vzhledem k aplikaci je hlavní příčinu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-321">Check to see if a health report posted against the application is the root cause.</span></span> <span data-ttu-id="12f7d-322">Upozornění obvykle obsahuje podrobnosti o problému.</span><span class="sxs-lookup"><span data-stu-id="12f7d-322">Usually, the warning contains details of the problem.</span></span> <span data-ttu-id="12f7d-323">Pokud přechodný problém se předpokládá, že aplikace je automaticky obnovit z tohoto stavu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-323">If the issue is transient, the application is expected to auto-recover from this state.</span></span>

<span data-ttu-id="12f7d-324">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="12f7d-324">Q.</span></span> <span data-ttu-id="12f7d-325">**Co mám dělat, když můj clusteru není v pořádku a je třeba provést aktualizaci naléhavé operačního systému?**</span><span class="sxs-lookup"><span data-stu-id="12f7d-325">**What can I do if my cluster is unhealthy and I need to do an urgent operating system update?**</span></span>

<span data-ttu-id="12f7d-326">A.</span><span class="sxs-lookup"><span data-stu-id="12f7d-326">A.</span></span> <span data-ttu-id="12f7d-327">Oprava aplikace orchestration neinstaluje aktualizace při clusteru není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="12f7d-327">The patch orchestration app does not install updates while the cluster is unhealthy.</span></span> <span data-ttu-id="12f7d-328">Zkuste a převeďte cluster do stavu v pořádku odblokovat pracovního postupu oprava orchestration aplikace.</span><span class="sxs-lookup"><span data-stu-id="12f7d-328">Try to bring your cluster to a healthy state to unblock the patch orchestration app workflow.</span></span>

<span data-ttu-id="12f7d-329">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="12f7d-329">Q.</span></span> <span data-ttu-id="12f7d-330">**Proč opravy napříč clustery trvá tak dlouho spustit?**</span><span class="sxs-lookup"><span data-stu-id="12f7d-330">**Why does patching across clusters take so long to run?**</span></span>

<span data-ttu-id="12f7d-331">A.</span><span class="sxs-lookup"><span data-stu-id="12f7d-331">A.</span></span> <span data-ttu-id="12f7d-332">Čas potřebný oprava aplikace orchestration je ve většině případů závislé na následujících faktorech:</span><span class="sxs-lookup"><span data-stu-id="12f7d-332">The time needed by the patch orchestration app is mostly dependent on the following factors:</span></span>

- <span data-ttu-id="12f7d-333">Zásada službu koordinátora.</span><span class="sxs-lookup"><span data-stu-id="12f7d-333">The policy of the Coordinator Service.</span></span> 
  - <span data-ttu-id="12f7d-334">Výchozí zásady `NodeWise`, výsledkem opravy jenom jeden uzel v čase.</span><span class="sxs-lookup"><span data-stu-id="12f7d-334">The default policy, `NodeWise`, results in patching only one node at a time.</span></span> <span data-ttu-id="12f7d-335">Zejména v případě větší clusterů, doporučujeme použít `UpgradeDomainWise` zásadu, abyste dosáhli rychlejší opravy napříč clustery.</span><span class="sxs-lookup"><span data-stu-id="12f7d-335">Especially in the case of bigger clusters, we recommend that you use the `UpgradeDomainWise` policy to achieve faster patching across clusters.</span></span>
- <span data-ttu-id="12f7d-336">Počet aktualizací, které jsou k dispozici ke stažení a instalaci.</span><span class="sxs-lookup"><span data-stu-id="12f7d-336">The number of updates available for download and installation.</span></span> 
- <span data-ttu-id="12f7d-337">Průměrný čas potřebný ke stažení a instalaci aktualizace, které by neměl překročit několik hodin.</span><span class="sxs-lookup"><span data-stu-id="12f7d-337">The average time needed to download and install an update, which should not exceed a couple of hours.</span></span>
- <span data-ttu-id="12f7d-338">Výkon virtuálního počítače a šířku pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="12f7d-338">The performance of the VM and network bandwidth.</span></span>

<span data-ttu-id="12f7d-339">OTÁZKY.</span><span class="sxs-lookup"><span data-stu-id="12f7d-339">Q.</span></span> <span data-ttu-id="12f7d-340">**Proč vidí některé aktualizace ve výsledcích Windows Update získat prostřednictvím rozhraní api REST, ale ne v historii služby Windows Update v počítači?**</span><span class="sxs-lookup"><span data-stu-id="12f7d-340">**Why do I see some updates in Windows Update results obtained via REST api's, but not under the Windows Update history on machine?**</span></span>

<span data-ttu-id="12f7d-341">A.</span><span class="sxs-lookup"><span data-stu-id="12f7d-341">A.</span></span> <span data-ttu-id="12f7d-342">Některé aktualizace produktu je třeba ověřit v historii jejich příslušné aktualizace nebo opravy.</span><span class="sxs-lookup"><span data-stu-id="12f7d-342">Some product updates need to be checked in their respective update/patch history.</span></span> <span data-ttu-id="12f7d-343">Např: Aktualizace programu Windows Defender se nezobrazí v historii služby Windows Update v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="12f7d-343">Eg: Windows Defender updates do not show up in Windows Update history on Windows Server 2016.</span></span>

## <a name="disclaimers"></a><span data-ttu-id="12f7d-344">Právní omezení</span><span class="sxs-lookup"><span data-stu-id="12f7d-344">Disclaimers</span></span>

- <span data-ttu-id="12f7d-345">Oprava aplikace orchestration přijme licenční smlouvy s koncovým uživatelem z Windows Update jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="12f7d-345">The patch orchestration app accepts the End-User License Agreement of Windows Update on behalf of the user.</span></span> <span data-ttu-id="12f7d-346">Volitelně může být vypnuto nastavení v konfiguraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="12f7d-346">Optionally, the setting can be turned off in the configuration of the application.</span></span>

- <span data-ttu-id="12f7d-347">Oprava aplikace orchestration shromažďuje telemetrická data pro sledování využití a výkonu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-347">The patch orchestration app collects telemetry to track usage and performance.</span></span> <span data-ttu-id="12f7d-348">Telemetrie aplikace odpovídá nastavení telemetrie nastavení modulu runtime Service Fabric, (který je ve výchozím).</span><span class="sxs-lookup"><span data-stu-id="12f7d-348">The application’s telemetry follows the setting of the Service Fabric runtime’s telemetry setting (which is on by default).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="12f7d-349">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="12f7d-349">Troubleshooting</span></span>

### <a name="a-node-is-not-coming-back-to-up-state"></a><span data-ttu-id="12f7d-350">Uzel není vracející se zpět do až stavu</span><span class="sxs-lookup"><span data-stu-id="12f7d-350">A node is not coming back to up state</span></span>

<span data-ttu-id="12f7d-351">**Uzel může zasekla v automatickém zakázání stavu, protože**:</span><span class="sxs-lookup"><span data-stu-id="12f7d-351">**The node might be stuck in a disabling state because**:</span></span>

<span data-ttu-id="12f7d-352">Kontrola zabezpečení čeká na vyřízení.</span><span class="sxs-lookup"><span data-stu-id="12f7d-352">A safety check is pending.</span></span> <span data-ttu-id="12f7d-353">Chcete-li opravit tuto situaci, zajistěte, aby byly dostatek uzlů k dispozici v dobrém stavu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-353">To remedy this situation, ensure that enough nodes are available in a healthy state.</span></span>

<span data-ttu-id="12f7d-354">**Uzel se mohla zastavit v zakázaném stavu, protože**:</span><span class="sxs-lookup"><span data-stu-id="12f7d-354">**The node might be stuck in a disabled state because**:</span></span>

- <span data-ttu-id="12f7d-355">Uzlu zakázal ručně.</span><span class="sxs-lookup"><span data-stu-id="12f7d-355">The node was disabled manually.</span></span>
- <span data-ttu-id="12f7d-356">Uzel byl zakázán z důvodu úlohu probíhající infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="12f7d-356">The node was disabled due to an ongoing Azure infrastructure job.</span></span>
- <span data-ttu-id="12f7d-357">Uzlu byla dočasně zakázána správcem aplikace oprava orchestration opravit uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-357">The node was disabled temporarily by the patch orchestration app to patch the node.</span></span>

<span data-ttu-id="12f7d-358">**Uzel může zasekla v automatickém dolů stavu, protože**:</span><span class="sxs-lookup"><span data-stu-id="12f7d-358">**The node might be stuck in a down state because**:</span></span>

- <span data-ttu-id="12f7d-359">Uzel byla zařazena v dolů stavu ručně.</span><span class="sxs-lookup"><span data-stu-id="12f7d-359">The node was put in a down state manually.</span></span>
- <span data-ttu-id="12f7d-360">Uzlu právě probíhá restartování počítače (který může být aktivovány oprava orchestration aplikace).</span><span class="sxs-lookup"><span data-stu-id="12f7d-360">The node is undergoing a restart (which might be triggered by the patch orchestration app).</span></span>
- <span data-ttu-id="12f7d-361">Uzel je mimo provoz z důvodu vadné virtuálního počítače nebo počítače nebo síťové problémy s připojením.</span><span class="sxs-lookup"><span data-stu-id="12f7d-361">The node is down due to a faulty VM or machine or network connectivity issues.</span></span>

### <a name="updates-were-skipped-on-some-nodes"></a><span data-ttu-id="12f7d-362">Aktualizace, které byly přeskočeny na některých uzlech</span><span class="sxs-lookup"><span data-stu-id="12f7d-362">Updates were skipped on some nodes</span></span>

<span data-ttu-id="12f7d-363">Oprava aplikace orchestration se pokusí o instalaci služby Windows update podle přeplánování zásad.</span><span class="sxs-lookup"><span data-stu-id="12f7d-363">The patch orchestration app tries to install a Windows update according to the rescheduling policy.</span></span> <span data-ttu-id="12f7d-364">Služba se pokusí obnovit uzel a přeskočit aktualizace podle zásad aplikací.</span><span class="sxs-lookup"><span data-stu-id="12f7d-364">The service tries to recover the node and skip the update according to the application policy.</span></span>

<span data-ttu-id="12f7d-365">V takovém případě se vygeneruje sestavy stavu úroveň upozornění pro službu Agent uzlu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-365">In such a case, a warning-level health report is generated against the Node Agent Service.</span></span> <span data-ttu-id="12f7d-366">Výsledek pro služby Windows Update také obsahuje možný důvod selhání.</span><span class="sxs-lookup"><span data-stu-id="12f7d-366">The result for Windows Update also contains the possible reason for the failure.</span></span>

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a><span data-ttu-id="12f7d-367">Stav clusteru přejde k chybě při instalaci aktualizace</span><span class="sxs-lookup"><span data-stu-id="12f7d-367">The health of the cluster goes to error while the update installs</span></span>

<span data-ttu-id="12f7d-368">Vadný služby Windows update můžete zahrnout dolů stav aplikace nebo clusteru v konkrétním uzlu nebo doméně pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="12f7d-368">A faulty Windows update can bring down the health of an application or cluster on a particular node or upgrade domain.</span></span> <span data-ttu-id="12f7d-369">Oprava aplikace orchestration ze všechny následné operace služby Windows Update, dokud znovu je v pořádku clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-369">The patch orchestration app discontinues any subsequent Windows Update operation until the cluster is healthy again.</span></span>

<span data-ttu-id="12f7d-370">Správce musí zasáhnout a zjistit, proč k problému z důvodu Windows Update aplikace nebo clusteru.</span><span class="sxs-lookup"><span data-stu-id="12f7d-370">An administrator must intervene and determine why the application or cluster became unhealthy due to Windows Update.</span></span>

## <a name="release-notes-"></a><span data-ttu-id="12f7d-371">Poznámky k verzi:</span><span class="sxs-lookup"><span data-stu-id="12f7d-371">Release Notes :</span></span>

### <a name="version-110"></a><span data-ttu-id="12f7d-372">Verze 1.1.0</span><span class="sxs-lookup"><span data-stu-id="12f7d-372">Version 1.1.0</span></span>
- <span data-ttu-id="12f7d-373">Veřejné verze</span><span class="sxs-lookup"><span data-stu-id="12f7d-373">Public release</span></span>

### <a name="version-111"></a><span data-ttu-id="12f7d-374">Verze 1.1.1</span><span class="sxs-lookup"><span data-stu-id="12f7d-374">Version 1.1.1</span></span>
- <span data-ttu-id="12f7d-375">Pevná chyby ve SetupEntryPoint z NodeAgentService, která zabránila instalace NodeAgentNTService.</span><span class="sxs-lookup"><span data-stu-id="12f7d-375">Fixed a bug in SetupEntryPoint of NodeAgentService that prevented installation of NodeAgentNTService.</span></span>

### <a name="version-120-latest"></a><span data-ttu-id="12f7d-376">Verze 1.2.0 (nejnovější)</span><span class="sxs-lookup"><span data-stu-id="12f7d-376">Version 1.2.0 (Latest)</span></span>

- <span data-ttu-id="12f7d-377">Opravy chyb kolem systém restartovat pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="12f7d-377">Bug fixes around system restart workflow.</span></span>
- <span data-ttu-id="12f7d-378">Oprava chyby při vytváření úlohy RM kvůli které stavu nebyl kontrola během přípravy úlohy oprava děje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="12f7d-378">Bug fix in creation of RM tasks due to which health check during preparing repair tasks wasn't happening as expected.</span></span>
- <span data-ttu-id="12f7d-379">Změnit režim spuštění pro služby systému windows POANodeSvc z automatické na odložené automaticky.</span><span class="sxs-lookup"><span data-stu-id="12f7d-379">Changed the startup mode for windows service POANodeSvc from auto to delayed-auto.</span></span>
