---
title: "Rozdíly Azure Service Fabric pro Linux a Windows | Dokumentace Microsoftu"
description: "Rozdíly mezi Azure Service Fabric Preview v Linuxu a Azure Service Fabric ve Windows"
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
ms.openlocfilehash: 7b80bb7d4a4e6a1b4cf47ce87200f47339785c53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a><span data-ttu-id="354e2-103">Rozdíly mezi Service Fabric v Linuxu (verze Preview) a ve Windows (obecně dostupná verze)</span><span class="sxs-lookup"><span data-stu-id="354e2-103">Differences between Service Fabric on Linux (preview) and Windows (generally available)</span></span>

<span data-ttu-id="354e2-104">Protože Service Fabric v Linuxu je ve verzi Preview, existují určité funkce, které jsou ve Windows podporované, ale v Linuxu zatím ne.</span><span class="sxs-lookup"><span data-stu-id="354e2-104">Since Service Fabric on Linux is a preview, there are some features that are supported on Windows, but not yet on Linux.</span></span> <span data-ttu-id="354e2-105">Nakonec si sady funkcí budou odpovídat, až bude Service Fabric v Linuxu v obecně dostupné verzi.</span><span class="sxs-lookup"><span data-stu-id="354e2-105">Eventually, the feature sets will be at parity when Service Fabric on Linux becomes generally available.</span></span> <span data-ttu-id="354e2-106">S budoucími verzemi se bude tato mezera zmenšovat.</span><span class="sxs-lookup"><span data-stu-id="354e2-106">With upcoming releases, this feature gap will shrink.</span></span> <span data-ttu-id="354e2-107">Mezi nejnovějšími dostupnými verzemi (tedy mezi verzí 5.6 pro Windows a verzí 5.5 pro Linux) existují následující rozdíly:</span><span class="sxs-lookup"><span data-stu-id="354e2-107">The following differences exist between the latest available releases (that is, between version 5.6 on Windows and version 5.5 on Linux):</span></span> 

* <span data-ttu-id="354e2-108">Reliable Collections (a Reliable Stateful Services)</span><span class="sxs-lookup"><span data-stu-id="354e2-108">Reliable Collections (and Reliable Stateful Services)</span></span> 
* <span data-ttu-id="354e2-109">ReverseProxy</span><span class="sxs-lookup"><span data-stu-id="354e2-109">ReverseProxy</span></span> 
* <span data-ttu-id="354e2-110">Samostatný instalační program</span><span class="sxs-lookup"><span data-stu-id="354e2-110">Standalone installer</span></span> 
* <span data-ttu-id="354e2-111">Ověření schématu XML pro soubory manifestu</span><span class="sxs-lookup"><span data-stu-id="354e2-111">XML schema validation for manifest files</span></span> 
* <span data-ttu-id="354e2-112">Přesměrování konzoly</span><span class="sxs-lookup"><span data-stu-id="354e2-112">Console redirection</span></span> 
* <span data-ttu-id="354e2-113">Fault Analysis Service (FAS)</span><span class="sxs-lookup"><span data-stu-id="354e2-113">The Fault Analysis Service (FAS)</span></span>
* <span data-ttu-id="354e2-114">Ovladače protokolování a svazku a Docker Compose pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="354e2-114">Docker compose and volume and logging drivers for containers</span></span> 
* <span data-ttu-id="354e2-115">Zásady správného řízení prostředků pro kontejnery a služby</span><span class="sxs-lookup"><span data-stu-id="354e2-115">Resource governance for containers and services</span></span> 
* <span data-ttu-id="354e2-116">Služba DNS</span><span class="sxs-lookup"><span data-stu-id="354e2-116">DNS service</span></span>
* <span data-ttu-id="354e2-117">Podpora Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="354e2-117">Azure Active Directory support</span></span>
* <span data-ttu-id="354e2-118">Ekvivalenty příkazů rozhraní příkazového řádku pro určité příkazy PowerShellu</span><span class="sxs-lookup"><span data-stu-id="354e2-118">CLI command equivalents of certain Powershell commands</span></span> 
* <span data-ttu-id="354e2-119">Proti clusteru Linux lze spustit pouze podmnožinu příkazů Powershellu (jak je rozvedeno v další části).</span><span class="sxs-lookup"><span data-stu-id="354e2-119">Only a subset of Powershell commands can be run against a Linux cluster (as expanded in the next section).</span></span>

>[!NOTE]
><span data-ttu-id="354e2-120">Přesměrování konzoly se nepodporuje v produkčních clusterech, dokonce ani ve Windows.</span><span class="sxs-lookup"><span data-stu-id="354e2-120">Console redirection is not supported in production clusters, even on Windows.</span></span>

<span data-ttu-id="354e2-121">Nástroje pro vývoj ve Windows a v Linuxu se také liší.</span><span class="sxs-lookup"><span data-stu-id="354e2-121">Development tooling is also different between Windows and Linux.</span></span> <span data-ttu-id="354e2-122">Ve Windows se používá sada Visual Studio, PowerShell, VSTS a Trasování událostí pro Windows, zatímco v Linuxu se používá Yeoman, Eclipse, Jenkins a LTTng.</span><span class="sxs-lookup"><span data-stu-id="354e2-122">VisualStudio, Powershell, VSTS, and ETW are used on Windows while Yeoman, Eclipse, Jenkins, and LTTng are used on Linux.</span></span>

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a><span data-ttu-id="354e2-123">Rutiny PowerShellu, které nefungují proti linuxovému clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="354e2-123">Powershell cmdlets that do not work against a Linux Service Fabric cluster</span></span>

* <span data-ttu-id="354e2-124">Invoke-ServiceFabricChaosTestScenario</span><span class="sxs-lookup"><span data-stu-id="354e2-124">Invoke-ServiceFabricChaosTestScenario</span></span>
* <span data-ttu-id="354e2-125">Invoke-ServiceFabricFailoverTestScenario</span><span class="sxs-lookup"><span data-stu-id="354e2-125">Invoke-ServiceFabricFailoverTestScenario</span></span>
* <span data-ttu-id="354e2-126">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="354e2-126">Invoke-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="354e2-127">Invoke-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="354e2-127">Invoke-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="354e2-128">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="354e2-128">Restart-ServiceFabricPartition</span></span>
* <span data-ttu-id="354e2-129">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="354e2-129">Start-ServiceFabricNode</span></span>
* <span data-ttu-id="354e2-130">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="354e2-130">Stop-ServiceFabricNode</span></span>
* <span data-ttu-id="354e2-131">Get-ServiceFabricImageStoreContent</span><span class="sxs-lookup"><span data-stu-id="354e2-131">Get-ServiceFabricImageStoreContent</span></span>
* <span data-ttu-id="354e2-132">Get-ServiceFabricChaosReport</span><span class="sxs-lookup"><span data-stu-id="354e2-132">Get-ServiceFabricChaosReport</span></span>
* <span data-ttu-id="354e2-133">Get-ServiceFabricNodeTransitionProgress</span><span class="sxs-lookup"><span data-stu-id="354e2-133">Get-ServiceFabricNodeTransitionProgress</span></span>
* <span data-ttu-id="354e2-134">Get-ServiceFabricPartitionDataLossProgress</span><span class="sxs-lookup"><span data-stu-id="354e2-134">Get-ServiceFabricPartitionDataLossProgress</span></span>
* <span data-ttu-id="354e2-135">Get-ServiceFabricPartitionQuorumLossProgress</span><span class="sxs-lookup"><span data-stu-id="354e2-135">Get-ServiceFabricPartitionQuorumLossProgress</span></span>
* <span data-ttu-id="354e2-136">Get-ServiceFabricPartitionRestartProgress</span><span class="sxs-lookup"><span data-stu-id="354e2-136">Get-ServiceFabricPartitionRestartProgress</span></span>
* <span data-ttu-id="354e2-137">Get-ServiceFabricTestCommandStatusList</span><span class="sxs-lookup"><span data-stu-id="354e2-137">Get-ServiceFabricTestCommandStatusList</span></span>
* <span data-ttu-id="354e2-138">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="354e2-138">Remove-ServiceFabricTestState</span></span>
* <span data-ttu-id="354e2-139">Start-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="354e2-139">Start-ServiceFabricChaos</span></span>
* <span data-ttu-id="354e2-140">Start-ServiceFabricNodeTransition</span><span class="sxs-lookup"><span data-stu-id="354e2-140">Start-ServiceFabricNodeTransition</span></span>
* <span data-ttu-id="354e2-141">Start-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="354e2-141">Start-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="354e2-142">Start-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="354e2-142">Start-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="354e2-143">Start-ServiceFabricPartitionRestart</span><span class="sxs-lookup"><span data-stu-id="354e2-143">Start-ServiceFabricPartitionRestart</span></span>
* <span data-ttu-id="354e2-144">Stop-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="354e2-144">Stop-ServiceFabricChaos</span></span>
* <span data-ttu-id="354e2-145">Stop-ServiceFabricTestCommand</span><span class="sxs-lookup"><span data-stu-id="354e2-145">Stop-ServiceFabricTestCommand</span></span>
* <span data-ttu-id="354e2-146">Cmd</span><span class="sxs-lookup"><span data-stu-id="354e2-146">Cmd</span></span>
* <span data-ttu-id="354e2-147">Get-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-147">Get-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="354e2-148">Get-ServiceFabricClusterConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-148">Get-ServiceFabricClusterConfiguration</span></span>
* <span data-ttu-id="354e2-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span><span class="sxs-lookup"><span data-stu-id="354e2-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span></span>
* <span data-ttu-id="354e2-150">Get-ServiceFabricPackageDebugParameters</span><span class="sxs-lookup"><span data-stu-id="354e2-150">Get-ServiceFabricPackageDebugParameters</span></span>
* <span data-ttu-id="354e2-151">New-ServiceFabricPackageDebugParameter</span><span class="sxs-lookup"><span data-stu-id="354e2-151">New-ServiceFabricPackageDebugParameter</span></span>
* <span data-ttu-id="354e2-152">New-ServiceFabricPackageSharingPolicy</span><span class="sxs-lookup"><span data-stu-id="354e2-152">New-ServiceFabricPackageSharingPolicy</span></span>
* <span data-ttu-id="354e2-153">Add-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="354e2-153">Add-ServiceFabricNode</span></span>
* <span data-ttu-id="354e2-154">Copy-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="354e2-154">Copy-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="354e2-155">Get-ServiceFabricRuntimeSupportedVersion</span><span class="sxs-lookup"><span data-stu-id="354e2-155">Get-ServiceFabricRuntimeSupportedVersion</span></span>
* <span data-ttu-id="354e2-156">Get-ServiceFabricRuntimeUpgradeVersion</span><span class="sxs-lookup"><span data-stu-id="354e2-156">Get-ServiceFabricRuntimeUpgradeVersion</span></span>
* <span data-ttu-id="354e2-157">New-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="354e2-157">New-ServiceFabricCluster</span></span>
* <span data-ttu-id="354e2-158">New-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-158">New-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="354e2-159">Remove-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="354e2-159">Remove-ServiceFabricCluster</span></span>
* <span data-ttu-id="354e2-160">Remove-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="354e2-160">Remove-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="354e2-161">Remove-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-161">Remove-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="354e2-162">Test-ServiceFabricClusterManifest</span><span class="sxs-lookup"><span data-stu-id="354e2-162">Test-ServiceFabricClusterManifest</span></span>
* <span data-ttu-id="354e2-163">Test-ServiceFabricConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-163">Test-ServiceFabricConfiguration</span></span>
* <span data-ttu-id="354e2-164">Update-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="354e2-164">Update-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="354e2-165">Approve-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-165">Approve-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-166">Complete-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-166">Complete-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-167">Get-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-167">Get-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-168">Invoke-ServiceFabricDecryptText</span><span class="sxs-lookup"><span data-stu-id="354e2-168">Invoke-ServiceFabricDecryptText</span></span>
* <span data-ttu-id="354e2-169">Invoke-ServiceFabricEncryptSecret</span><span class="sxs-lookup"><span data-stu-id="354e2-169">Invoke-ServiceFabricEncryptSecret</span></span>
* <span data-ttu-id="354e2-170">Invoke-ServiceFabricEncryptText</span><span class="sxs-lookup"><span data-stu-id="354e2-170">Invoke-ServiceFabricEncryptText</span></span>
* <span data-ttu-id="354e2-171">Invoke-ServiceFabricInfrastructureCommand</span><span class="sxs-lookup"><span data-stu-id="354e2-171">Invoke-ServiceFabricInfrastructureCommand</span></span>
* <span data-ttu-id="354e2-172">Invoke-ServiceFabricInfrastructureQuery</span><span class="sxs-lookup"><span data-stu-id="354e2-172">Invoke-ServiceFabricInfrastructureQuery</span></span>
* <span data-ttu-id="354e2-173">Remove-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-173">Remove-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-174">Start-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-174">Start-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-175">Stop-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="354e2-175">Stop-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="354e2-176">Update-ServiceFabricRepairTaskHealthPolicy</span><span class="sxs-lookup"><span data-stu-id="354e2-176">Update-ServiceFabricRepairTaskHealthPolicy</span></span>



## <a name="next-steps"></a><span data-ttu-id="354e2-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="354e2-177">Next steps</span></span>
* [<span data-ttu-id="354e2-178">Příprava vývojového prostředí v Linuxu</span><span class="sxs-lookup"><span data-stu-id="354e2-178">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="354e2-179">Příprava vývojového prostředí v OSX</span><span class="sxs-lookup"><span data-stu-id="354e2-179">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="354e2-180">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana</span><span class="sxs-lookup"><span data-stu-id="354e2-180">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="354e2-181">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="354e2-181">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="354e2-182">Vytvoření první aplikace v CSharp v Linuxu</span><span class="sxs-lookup"><span data-stu-id="354e2-182">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="354e2-183">Správa aplikací pomocí Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="354e2-183">Use the Service Fabric CLI to manage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
