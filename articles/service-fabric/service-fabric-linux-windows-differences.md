---
title: "aaaAzure Service Fabric rozdíly mezi systémy Linux a Windows | Microsoft Docs"
description: "Rozdíly mezi hello Azure Service Fabric Preview v systému Linux a Azure Service Fabric v systému Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7a16a440dfc8d9006e274f46951be1562e6f10d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a><span data-ttu-id="69fd6-103">Rozdíly mezi Service Fabric v Linuxu (verze Preview) a ve Windows (obecně dostupná verze)</span><span class="sxs-lookup"><span data-stu-id="69fd6-103">Differences between Service Fabric on Linux (preview) and Windows (generally available)</span></span>

<span data-ttu-id="69fd6-104">Protože Service Fabric v Linuxu je ve verzi Preview, existují určité funkce, které jsou ve Windows podporované, ale v Linuxu zatím ne.</span><span class="sxs-lookup"><span data-stu-id="69fd6-104">Since Service Fabric on Linux is a preview, there are some features that are supported on Windows, but not yet on Linux.</span></span> <span data-ttu-id="69fd6-105">Nakonec sady funkcí hello budou v parita, když je obecně dostupná Service Fabric v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="69fd6-105">Eventually, hello feature sets will be at parity when Service Fabric on Linux becomes generally available.</span></span> <span data-ttu-id="69fd6-106">S budoucími verzemi se bude tato mezera zmenšovat.</span><span class="sxs-lookup"><span data-stu-id="69fd6-106">With upcoming releases, this feature gap will shrink.</span></span> <span data-ttu-id="69fd6-107">Hello následující rozdíly mezi existovat nejnovější dostupné verze hello (tedy mezi verzemi 5.6 v systému Windows a verzi 5.5 v systému Linux):</span><span class="sxs-lookup"><span data-stu-id="69fd6-107">hello following differences exist between hello latest available releases (that is, between version 5.6 on Windows and version 5.5 on Linux):</span></span> 

* <span data-ttu-id="69fd6-108">Reliable Collections (a Reliable Stateful Services)</span><span class="sxs-lookup"><span data-stu-id="69fd6-108">Reliable Collections (and Reliable Stateful Services)</span></span> 
* <span data-ttu-id="69fd6-109">ReverseProxy</span><span class="sxs-lookup"><span data-stu-id="69fd6-109">ReverseProxy</span></span> 
* <span data-ttu-id="69fd6-110">Samostatný instalační program</span><span class="sxs-lookup"><span data-stu-id="69fd6-110">Standalone installer</span></span> 
* <span data-ttu-id="69fd6-111">Ověření schématu XML pro soubory manifestu</span><span class="sxs-lookup"><span data-stu-id="69fd6-111">XML schema validation for manifest files</span></span> 
* <span data-ttu-id="69fd6-112">Přesměrování konzoly</span><span class="sxs-lookup"><span data-stu-id="69fd6-112">Console redirection</span></span> 
* <span data-ttu-id="69fd6-113">Hello selhání Analysis Service (DM)</span><span class="sxs-lookup"><span data-stu-id="69fd6-113">hello Fault Analysis Service (FAS)</span></span>
* <span data-ttu-id="69fd6-114">Ovladače protokolování a svazku a Docker Compose pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="69fd6-114">Docker compose and volume and logging drivers for containers</span></span> 
* <span data-ttu-id="69fd6-115">Zásady správného řízení prostředků pro kontejnery a služby</span><span class="sxs-lookup"><span data-stu-id="69fd6-115">Resource governance for containers and services</span></span> 
* <span data-ttu-id="69fd6-116">Služba DNS</span><span class="sxs-lookup"><span data-stu-id="69fd6-116">DNS service</span></span>
* <span data-ttu-id="69fd6-117">Podpora Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="69fd6-117">Azure Active Directory support</span></span>
* <span data-ttu-id="69fd6-118">Ekvivalenty příkazů rozhraní příkazového řádku pro určité příkazy PowerShellu</span><span class="sxs-lookup"><span data-stu-id="69fd6-118">CLI command equivalents of certain Powershell commands</span></span> 
* <span data-ttu-id="69fd6-119">Oproti Linux clusteru (jak rozšířit v další části hello) lze spustit pouze podmnožinu příkazy prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="69fd6-119">Only a subset of Powershell commands can be run against a Linux cluster (as expanded in hello next section).</span></span>

>[!NOTE]
><span data-ttu-id="69fd6-120">Přesměrování konzoly se nepodporuje v produkčních clusterech, dokonce ani ve Windows.</span><span class="sxs-lookup"><span data-stu-id="69fd6-120">Console redirection is not supported in production clusters, even on Windows.</span></span>

<span data-ttu-id="69fd6-121">Nástroje pro vývoj ve Windows a v Linuxu se také liší.</span><span class="sxs-lookup"><span data-stu-id="69fd6-121">Development tooling is also different between Windows and Linux.</span></span> <span data-ttu-id="69fd6-122">Ve Windows se používá sada Visual Studio, PowerShell, VSTS a Trasování událostí pro Windows, zatímco v Linuxu se používá Yeoman, Eclipse, Jenkins a LTTng.</span><span class="sxs-lookup"><span data-stu-id="69fd6-122">VisualStudio, Powershell, VSTS, and ETW are used on Windows while Yeoman, Eclipse, Jenkins, and LTTng are used on Linux.</span></span>

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a><span data-ttu-id="69fd6-123">Rutiny PowerShellu, které nefungují proti linuxovému clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="69fd6-123">Powershell cmdlets that do not work against a Linux Service Fabric cluster</span></span>

* <span data-ttu-id="69fd6-124">Invoke-ServiceFabricChaosTestScenario</span><span class="sxs-lookup"><span data-stu-id="69fd6-124">Invoke-ServiceFabricChaosTestScenario</span></span>
* <span data-ttu-id="69fd6-125">Invoke-ServiceFabricFailoverTestScenario</span><span class="sxs-lookup"><span data-stu-id="69fd6-125">Invoke-ServiceFabricFailoverTestScenario</span></span>
* <span data-ttu-id="69fd6-126">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="69fd6-126">Invoke-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="69fd6-127">Invoke-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="69fd6-127">Invoke-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="69fd6-128">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="69fd6-128">Restart-ServiceFabricPartition</span></span>
* <span data-ttu-id="69fd6-129">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="69fd6-129">Start-ServiceFabricNode</span></span>
* <span data-ttu-id="69fd6-130">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="69fd6-130">Stop-ServiceFabricNode</span></span>
* <span data-ttu-id="69fd6-131">Get-ServiceFabricImageStoreContent</span><span class="sxs-lookup"><span data-stu-id="69fd6-131">Get-ServiceFabricImageStoreContent</span></span>
* <span data-ttu-id="69fd6-132">Get-ServiceFabricChaosReport</span><span class="sxs-lookup"><span data-stu-id="69fd6-132">Get-ServiceFabricChaosReport</span></span>
* <span data-ttu-id="69fd6-133">Get-ServiceFabricNodeTransitionProgress</span><span class="sxs-lookup"><span data-stu-id="69fd6-133">Get-ServiceFabricNodeTransitionProgress</span></span>
* <span data-ttu-id="69fd6-134">Get-ServiceFabricPartitionDataLossProgress</span><span class="sxs-lookup"><span data-stu-id="69fd6-134">Get-ServiceFabricPartitionDataLossProgress</span></span>
* <span data-ttu-id="69fd6-135">Get-ServiceFabricPartitionQuorumLossProgress</span><span class="sxs-lookup"><span data-stu-id="69fd6-135">Get-ServiceFabricPartitionQuorumLossProgress</span></span>
* <span data-ttu-id="69fd6-136">Get-ServiceFabricPartitionRestartProgress</span><span class="sxs-lookup"><span data-stu-id="69fd6-136">Get-ServiceFabricPartitionRestartProgress</span></span>
* <span data-ttu-id="69fd6-137">Get-ServiceFabricTestCommandStatusList</span><span class="sxs-lookup"><span data-stu-id="69fd6-137">Get-ServiceFabricTestCommandStatusList</span></span>
* <span data-ttu-id="69fd6-138">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="69fd6-138">Remove-ServiceFabricTestState</span></span>
* <span data-ttu-id="69fd6-139">Start-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="69fd6-139">Start-ServiceFabricChaos</span></span>
* <span data-ttu-id="69fd6-140">Start-ServiceFabricNodeTransition</span><span class="sxs-lookup"><span data-stu-id="69fd6-140">Start-ServiceFabricNodeTransition</span></span>
* <span data-ttu-id="69fd6-141">Start-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="69fd6-141">Start-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="69fd6-142">Start-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="69fd6-142">Start-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="69fd6-143">Start-ServiceFabricPartitionRestart</span><span class="sxs-lookup"><span data-stu-id="69fd6-143">Start-ServiceFabricPartitionRestart</span></span>
* <span data-ttu-id="69fd6-144">Stop-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="69fd6-144">Stop-ServiceFabricChaos</span></span>
* <span data-ttu-id="69fd6-145">Stop-ServiceFabricTestCommand</span><span class="sxs-lookup"><span data-stu-id="69fd6-145">Stop-ServiceFabricTestCommand</span></span>
* <span data-ttu-id="69fd6-146">Cmd</span><span class="sxs-lookup"><span data-stu-id="69fd6-146">Cmd</span></span>
* <span data-ttu-id="69fd6-147">Get-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-147">Get-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="69fd6-148">Get-ServiceFabricClusterConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-148">Get-ServiceFabricClusterConfiguration</span></span>
* <span data-ttu-id="69fd6-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span><span class="sxs-lookup"><span data-stu-id="69fd6-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span></span>
* <span data-ttu-id="69fd6-150">Get-ServiceFabricPackageDebugParameters</span><span class="sxs-lookup"><span data-stu-id="69fd6-150">Get-ServiceFabricPackageDebugParameters</span></span>
* <span data-ttu-id="69fd6-151">New-ServiceFabricPackageDebugParameter</span><span class="sxs-lookup"><span data-stu-id="69fd6-151">New-ServiceFabricPackageDebugParameter</span></span>
* <span data-ttu-id="69fd6-152">New-ServiceFabricPackageSharingPolicy</span><span class="sxs-lookup"><span data-stu-id="69fd6-152">New-ServiceFabricPackageSharingPolicy</span></span>
* <span data-ttu-id="69fd6-153">Add-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="69fd6-153">Add-ServiceFabricNode</span></span>
* <span data-ttu-id="69fd6-154">Copy-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="69fd6-154">Copy-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="69fd6-155">Get-ServiceFabricRuntimeSupportedVersion</span><span class="sxs-lookup"><span data-stu-id="69fd6-155">Get-ServiceFabricRuntimeSupportedVersion</span></span>
* <span data-ttu-id="69fd6-156">Get-ServiceFabricRuntimeUpgradeVersion</span><span class="sxs-lookup"><span data-stu-id="69fd6-156">Get-ServiceFabricRuntimeUpgradeVersion</span></span>
* <span data-ttu-id="69fd6-157">New-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="69fd6-157">New-ServiceFabricCluster</span></span>
* <span data-ttu-id="69fd6-158">New-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-158">New-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="69fd6-159">Remove-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="69fd6-159">Remove-ServiceFabricCluster</span></span>
* <span data-ttu-id="69fd6-160">Remove-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="69fd6-160">Remove-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="69fd6-161">Remove-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-161">Remove-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="69fd6-162">Test-ServiceFabricClusterManifest</span><span class="sxs-lookup"><span data-stu-id="69fd6-162">Test-ServiceFabricClusterManifest</span></span>
* <span data-ttu-id="69fd6-163">Test-ServiceFabricConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-163">Test-ServiceFabricConfiguration</span></span>
* <span data-ttu-id="69fd6-164">Update-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="69fd6-164">Update-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="69fd6-165">Approve-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-165">Approve-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-166">Complete-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-166">Complete-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-167">Get-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-167">Get-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-168">Invoke-ServiceFabricDecryptText</span><span class="sxs-lookup"><span data-stu-id="69fd6-168">Invoke-ServiceFabricDecryptText</span></span>
* <span data-ttu-id="69fd6-169">Invoke-ServiceFabricEncryptSecret</span><span class="sxs-lookup"><span data-stu-id="69fd6-169">Invoke-ServiceFabricEncryptSecret</span></span>
* <span data-ttu-id="69fd6-170">Invoke-ServiceFabricEncryptText</span><span class="sxs-lookup"><span data-stu-id="69fd6-170">Invoke-ServiceFabricEncryptText</span></span>
* <span data-ttu-id="69fd6-171">Invoke-ServiceFabricInfrastructureCommand</span><span class="sxs-lookup"><span data-stu-id="69fd6-171">Invoke-ServiceFabricInfrastructureCommand</span></span>
* <span data-ttu-id="69fd6-172">Invoke-ServiceFabricInfrastructureQuery</span><span class="sxs-lookup"><span data-stu-id="69fd6-172">Invoke-ServiceFabricInfrastructureQuery</span></span>
* <span data-ttu-id="69fd6-173">Remove-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-173">Remove-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-174">Start-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-174">Start-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-175">Stop-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="69fd6-175">Stop-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="69fd6-176">Update-ServiceFabricRepairTaskHealthPolicy</span><span class="sxs-lookup"><span data-stu-id="69fd6-176">Update-ServiceFabricRepairTaskHealthPolicy</span></span>



## <a name="next-steps"></a><span data-ttu-id="69fd6-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69fd6-177">Next steps</span></span>
* [<span data-ttu-id="69fd6-178">Příprava vývojového prostředí v Linuxu</span><span class="sxs-lookup"><span data-stu-id="69fd6-178">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="69fd6-179">Příprava vývojového prostředí v OSX</span><span class="sxs-lookup"><span data-stu-id="69fd6-179">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="69fd6-180">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana</span><span class="sxs-lookup"><span data-stu-id="69fd6-180">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="69fd6-181">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="69fd6-181">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="69fd6-182">Vytvoření první aplikace v CSharp v Linuxu</span><span class="sxs-lookup"><span data-stu-id="69fd6-182">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="69fd6-183">Pomocí aplikace hello toomanage Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="69fd6-183">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
