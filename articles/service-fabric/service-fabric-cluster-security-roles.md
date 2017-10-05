---
title: "Zabezpečení clusteru Service Fabric: rolí klienta | Microsoft Docs"
description: "Tento článek popisuje dva klientské role a oprávnění poskytované role."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: 
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 85935e60bba4b27972282700e2e9c9a22b403bdb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="role-based-access-control-for-service-fabric-clients"></a><span data-ttu-id="ce5da-103">Řízení přístupu na základě rolí pro klienty Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce5da-103">Role-based access control for Service Fabric clients</span></span>
<span data-ttu-id="ce5da-104">Azure Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, kteří jsou připojené ke clusteru Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="ce5da-104">Azure Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="ce5da-105">Řízení přístupu umožňuje omezit přístup k určité operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru pomocí Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce5da-105">Access control allows the cluster administrator to limit access to certain cluster operations for different groups of users, making the cluster more secure.</span></span>  

<span data-ttu-id="ce5da-106">**Správci** mají úplný přístup k funkcím správy (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="ce5da-106">**Administrators** have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="ce5da-107">Ve výchozím nastavení **uživatelé** mít pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="ce5da-107">By default, **users** only have read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span>

<span data-ttu-id="ce5da-108">Tím, že poskytuje samostatné certifikáty pro každou zadáte dva klientské role (správce a klient) v době vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce5da-108">You specify the two client roles (administrator and client) at the time of cluster creation by providing separate certificates for each.</span></span> <span data-ttu-id="ce5da-109">V tématu [zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md) podrobnosti o nastavení zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ce5da-109">See [Service Fabric cluster security](service-fabric-cluster-security.md) for details on setting up a secure Service Fabric cluster.</span></span>

## <a name="default-access-control-settings"></a><span data-ttu-id="ce5da-110">Výchozí nastavení řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="ce5da-110">Default access control settings</span></span>
<span data-ttu-id="ce5da-111">Typ ovládacího prvku správce přístupu má úplný přístup k rozhraní API FabricClient.</span><span class="sxs-lookup"><span data-stu-id="ce5da-111">The administrator access control type has full access to all the FabricClient APIs.</span></span> <span data-ttu-id="ce5da-112">Můžete provádět žádné čtení a zápisy na cluster Service Fabric, včetně následujících operací:</span><span class="sxs-lookup"><span data-stu-id="ce5da-112">It can perform any reads and writes on the Service Fabric cluster, including the following operations:</span></span>

### <a name="application-and-service-operations"></a><span data-ttu-id="ce5da-113">Operace služby a aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-113">Application and service operations</span></span>
* <span data-ttu-id="ce5da-114">**CreateService**: vytváření služby</span><span class="sxs-lookup"><span data-stu-id="ce5da-114">**CreateService**: service creation</span></span>                             
* <span data-ttu-id="ce5da-115">**CreateServiceFromTemplate**: vytváření ze šablony služby</span><span class="sxs-lookup"><span data-stu-id="ce5da-115">**CreateServiceFromTemplate**: service creation from template</span></span>                             
* <span data-ttu-id="ce5da-116">**UpdateService**: servis aktualizací</span><span class="sxs-lookup"><span data-stu-id="ce5da-116">**UpdateService**: service updates</span></span>                             
* <span data-ttu-id="ce5da-117">**DeleteService**: Služba odstranění</span><span class="sxs-lookup"><span data-stu-id="ce5da-117">**DeleteService**: service deletion</span></span>                             
* <span data-ttu-id="ce5da-118">**ProvisionApplicationType**: zřizování typ aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-118">**ProvisionApplicationType**: application type provisioning</span></span>                             
* <span data-ttu-id="ce5da-119">**CreateApplication**: vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-119">**CreateApplication**: application creation</span></span>                               
* <span data-ttu-id="ce5da-120">**DeleteApplication**: odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-120">**DeleteApplication**: application deletion</span></span>                             
* <span data-ttu-id="ce5da-121">**UpgradeApplication**: spuštění nebo přerušení upgradů aplikací</span><span class="sxs-lookup"><span data-stu-id="ce5da-121">**UpgradeApplication**: starting or interrupting application upgrades</span></span>                             
* <span data-ttu-id="ce5da-122">**UnprovisionApplicationType**: rušení zajišťování typ aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-122">**UnprovisionApplicationType**: application type unprovisioning</span></span>                             
* <span data-ttu-id="ce5da-123">**MoveNextUpgradeDomain**: obnovení upgradů aplikací s doménou explicitní aktualizace</span><span class="sxs-lookup"><span data-stu-id="ce5da-123">**MoveNextUpgradeDomain**: resuming application upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="ce5da-124">**ReportUpgradeHealth**: obnovení upgradů aplikací s aktuální průběh upgradu</span><span class="sxs-lookup"><span data-stu-id="ce5da-124">**ReportUpgradeHealth**: resuming application upgrades with the current upgrade progress</span></span>                             
* <span data-ttu-id="ce5da-125">**ReportHealth**: generování sestav stavu</span><span class="sxs-lookup"><span data-stu-id="ce5da-125">**ReportHealth**: reporting health</span></span>                             
* <span data-ttu-id="ce5da-126">**PredeployPackageToNode**: před nasazením rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ce5da-126">**PredeployPackageToNode**: predeployment API</span></span>                            
* <span data-ttu-id="ce5da-127">**CodePackageControl**: restartování balíčky kódu</span><span class="sxs-lookup"><span data-stu-id="ce5da-127">**CodePackageControl**: restarting code packages</span></span>                             
* <span data-ttu-id="ce5da-128">**RecoverPartition**: obnovení oddíl</span><span class="sxs-lookup"><span data-stu-id="ce5da-128">**RecoverPartition**: recovering a partition</span></span>                             
* <span data-ttu-id="ce5da-129">**RecoverPartitions**: obnovení oddíly</span><span class="sxs-lookup"><span data-stu-id="ce5da-129">**RecoverPartitions**: recovering partitions</span></span>                             
* <span data-ttu-id="ce5da-130">**RecoverServicePartitions**: obnovení oddílů služby</span><span class="sxs-lookup"><span data-stu-id="ce5da-130">**RecoverServicePartitions**: recovering service partitions</span></span>                             
* <span data-ttu-id="ce5da-131">**RecoverSystemPartitions**: obnovení systému oddílů služby</span><span class="sxs-lookup"><span data-stu-id="ce5da-131">**RecoverSystemPartitions**: recovering system service partitions</span></span>                             

### <a name="cluster-operations"></a><span data-ttu-id="ce5da-132">Operace clusteru</span><span class="sxs-lookup"><span data-stu-id="ce5da-132">Cluster operations</span></span>
* <span data-ttu-id="ce5da-133">**ProvisionFabric**: MSI nebo clusteru manifest zřizování</span><span class="sxs-lookup"><span data-stu-id="ce5da-133">**ProvisionFabric**: MSI and/or cluster manifest provisioning</span></span>                             
* <span data-ttu-id="ce5da-134">**UpgradeFabric**: spouštění upgrade clusteru</span><span class="sxs-lookup"><span data-stu-id="ce5da-134">**UpgradeFabric**: starting cluster upgrades</span></span>                             
* <span data-ttu-id="ce5da-135">**UnprovisionFabric**: MSI nebo clusteru manifest rušení zajišťování</span><span class="sxs-lookup"><span data-stu-id="ce5da-135">**UnprovisionFabric**: MSI and/or cluster manifest unprovisioning</span></span>                         
* <span data-ttu-id="ce5da-136">**MoveNextFabricUpgradeDomain**: obnovení upgradu clusteru s doménou explicitní aktualizace</span><span class="sxs-lookup"><span data-stu-id="ce5da-136">**MoveNextFabricUpgradeDomain**: resuming cluster upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="ce5da-137">**ReportFabricUpgradeHealth**: obnovení upgradu clusteru s aktuální průběh upgradu</span><span class="sxs-lookup"><span data-stu-id="ce5da-137">**ReportFabricUpgradeHealth**: resuming cluster upgrades with the current upgrade progress</span></span>                             
* <span data-ttu-id="ce5da-138">**StartInfrastructureTask**: spouštění úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="ce5da-138">**StartInfrastructureTask**: starting infrastructure tasks</span></span>                             
* <span data-ttu-id="ce5da-139">**FinishInfrastructureTask**: dokončení úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="ce5da-139">**FinishInfrastructureTask**: finishing infrastructure tasks</span></span>                             
* <span data-ttu-id="ce5da-140">**InvokeInfrastructureCommand**: příkazy pro správu infrastruktury úloh</span><span class="sxs-lookup"><span data-stu-id="ce5da-140">**InvokeInfrastructureCommand**: infrastructure task management commands</span></span>                              
* <span data-ttu-id="ce5da-141">**ActivateNode**: Aktivace uzlu</span><span class="sxs-lookup"><span data-stu-id="ce5da-141">**ActivateNode**: activating a node</span></span>                             
* <span data-ttu-id="ce5da-142">**DeactivateNode**: deaktivace uzlu</span><span class="sxs-lookup"><span data-stu-id="ce5da-142">**DeactivateNode**: deactivating a node</span></span>                             
* <span data-ttu-id="ce5da-143">**DeactivateNodesBatch**: deaktivace několika uzly</span><span class="sxs-lookup"><span data-stu-id="ce5da-143">**DeactivateNodesBatch**: deactivating multiple nodes</span></span>                             
* <span data-ttu-id="ce5da-144">**RemoveNodeDeactivations**: navrácení deaktivace ve více uzlech</span><span class="sxs-lookup"><span data-stu-id="ce5da-144">**RemoveNodeDeactivations**: reverting deactivation on multiple nodes</span></span>                             
* <span data-ttu-id="ce5da-145">**GetNodeDeactivationStatus**: Kontrola stavu deaktivace</span><span class="sxs-lookup"><span data-stu-id="ce5da-145">**GetNodeDeactivationStatus**: checking deactivation status</span></span>                             
* <span data-ttu-id="ce5da-146">**NodeStateRemoved**: generování sestav stav uzlu odebrat</span><span class="sxs-lookup"><span data-stu-id="ce5da-146">**NodeStateRemoved**: reporting node state removed</span></span>                             
* <span data-ttu-id="ce5da-147">**ReportFault**: generování sestav chyb</span><span class="sxs-lookup"><span data-stu-id="ce5da-147">**ReportFault**: reporting fault</span></span>                             
* <span data-ttu-id="ce5da-148">**FileContent**: obrázek přenos souborů klienta úložiště (externí do clusteru)</span><span class="sxs-lookup"><span data-stu-id="ce5da-148">**FileContent**: image store client file transfer (external to cluster)</span></span>                             
* <span data-ttu-id="ce5da-149">**FileDownload**: image store klienta soubor stažení spuštění (externí do clusteru)</span><span class="sxs-lookup"><span data-stu-id="ce5da-149">**FileDownload**: image store client file download initiation (external to cluster)</span></span>                             
* <span data-ttu-id="ce5da-150">**InternalList**: image store klienta soubor seznamu operaci (interní)</span><span class="sxs-lookup"><span data-stu-id="ce5da-150">**InternalList**: image store client file list operation (internal)</span></span>                             
* <span data-ttu-id="ce5da-151">**Odstranit**: operace odstranění klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="ce5da-151">**Delete**: image store client delete operation</span></span>                              
* <span data-ttu-id="ce5da-152">**Nahrát**: operace nahrávání klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="ce5da-152">**Upload**: image store client upload operation</span></span>                             
* <span data-ttu-id="ce5da-153">**NodeControl**: spuštění, zastavení a restartování uzlů</span><span class="sxs-lookup"><span data-stu-id="ce5da-153">**NodeControl**: starting, stopping, and restarting nodes</span></span>                             
* <span data-ttu-id="ce5da-154">**MoveReplicaControl**: přesunutí repliky z jednoho uzlu do jiného</span><span class="sxs-lookup"><span data-stu-id="ce5da-154">**MoveReplicaControl**: moving replicas from one node to another</span></span>                             

### <a name="miscellaneous-operations"></a><span data-ttu-id="ce5da-155">Různým operacím</span><span class="sxs-lookup"><span data-stu-id="ce5da-155">Miscellaneous operations</span></span>
* <span data-ttu-id="ce5da-156">**Příkaz ping**: příkazy ping klienta</span><span class="sxs-lookup"><span data-stu-id="ce5da-156">**Ping**: client pings</span></span>                             
* <span data-ttu-id="ce5da-157">**Dotaz**: všechny dotazy na povoleno</span><span class="sxs-lookup"><span data-stu-id="ce5da-157">**Query**: all queries allowed</span></span>
* <span data-ttu-id="ce5da-158">**NameExists**: pojmenování URI existence kontroly</span><span class="sxs-lookup"><span data-stu-id="ce5da-158">**NameExists**: naming URI existence checks</span></span>                             

<span data-ttu-id="ce5da-159">Typ řízení přístupu uživatele je ve výchozím nastavení omezen na následující operace:</span><span class="sxs-lookup"><span data-stu-id="ce5da-159">The user access control type is, by default, limited to the following operations:</span></span> 

* <span data-ttu-id="ce5da-160">**EnumerateSubnames**: pojmenování URI – výčet</span><span class="sxs-lookup"><span data-stu-id="ce5da-160">**EnumerateSubnames**: naming URI enumeration</span></span>                             
* <span data-ttu-id="ce5da-161">**EnumerateProperties**: názvy vlastností – výčet</span><span class="sxs-lookup"><span data-stu-id="ce5da-161">**EnumerateProperties**: naming property enumeration</span></span>                             
* <span data-ttu-id="ce5da-162">**PropertyReadBatch**: názvy vlastností operace čtení</span><span class="sxs-lookup"><span data-stu-id="ce5da-162">**PropertyReadBatch**: naming property read operations</span></span>                             
* <span data-ttu-id="ce5da-163">**GetServiceDescription**: oznámení služby dlouho dotazování a čtení služby popisy</span><span class="sxs-lookup"><span data-stu-id="ce5da-163">**GetServiceDescription**: long-poll service notifications and reading service descriptions</span></span>                             
* <span data-ttu-id="ce5da-164">**ResolveService**: řešení služeb na základě předpisy</span><span class="sxs-lookup"><span data-stu-id="ce5da-164">**ResolveService**: complaint-based service resolution</span></span>                             
* <span data-ttu-id="ce5da-165">**ResolveNameOwner**: řešení pojmenování URI vlastníka</span><span class="sxs-lookup"><span data-stu-id="ce5da-165">**ResolveNameOwner**: resolving naming URI owner</span></span>                             
* <span data-ttu-id="ce5da-166">**ResolvePartition**: řešení systémových služeb</span><span class="sxs-lookup"><span data-stu-id="ce5da-166">**ResolvePartition**: resolving system services</span></span>                             
* <span data-ttu-id="ce5da-167">**ServiceNotifications**: oznámení služeb na základě událostí</span><span class="sxs-lookup"><span data-stu-id="ce5da-167">**ServiceNotifications**: event-based service notifications</span></span>                             
* <span data-ttu-id="ce5da-168">**GetUpgradeStatus**: dotazování na stav upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="ce5da-168">**GetUpgradeStatus**: polling application upgrade status</span></span>                             
* <span data-ttu-id="ce5da-169">**GetFabricUpgradeStatus**: dotazování na stav upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="ce5da-169">**GetFabricUpgradeStatus**: polling cluster upgrade status</span></span>                             
* <span data-ttu-id="ce5da-170">**InvokeInfrastructureQuery**: dotazování úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="ce5da-170">**InvokeInfrastructureQuery**: querying infrastructure tasks</span></span>                             
* <span data-ttu-id="ce5da-171">**Seznam**: operace seznamu soubor klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="ce5da-171">**List**: image store client file list operation</span></span>                             
* <span data-ttu-id="ce5da-172">**ResetPartitionLoad**: resetování zatížení jednotky převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="ce5da-172">**ResetPartitionLoad**: resetting load for a failover unit</span></span>                             
* <span data-ttu-id="ce5da-173">**ToggleVerboseServicePlacementHealthReporting**: přepnutím stavu umístění podrobné služby generování sestav</span><span class="sxs-lookup"><span data-stu-id="ce5da-173">**ToggleVerboseServicePlacementHealthReporting**: toggling verbose service placement health reporting</span></span>                             

<span data-ttu-id="ce5da-174">Řízení přístupu správce má také přístup k předchozí operace.</span><span class="sxs-lookup"><span data-stu-id="ce5da-174">The admin access control also has access to the preceding operations.</span></span>

## <a name="changing-default-settings-for-client-roles"></a><span data-ttu-id="ce5da-175">Změna výchozí nastavení klienta rolí</span><span class="sxs-lookup"><span data-stu-id="ce5da-175">Changing default settings for client roles</span></span>
<span data-ttu-id="ce5da-176">V souboru manifestu clusteru můžete zadat možnosti Správce klienta v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="ce5da-176">In the cluster manifest file, you can provide admin capabilities to the client if needed.</span></span> <span data-ttu-id="ce5da-177">Výchozí nastavení můžete změnit tak, že přejdete do **nastavení prostředků infrastruktury** možnost během [vytvoření clusteru](service-fabric-cluster-creation-via-portal.md)a poskytuje předchozí nastavení v **název**,  **správce**, **uživatele**, a **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="ce5da-177">You can change the defaults by going to the **Fabric Settings** option during [cluster creation](service-fabric-cluster-creation-via-portal.md), and providing the preceding settings in the **name**, **admin**, **user**, and **value** fields.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce5da-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce5da-178">Next steps</span></span>
[<span data-ttu-id="ce5da-179">Zabezpečení clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce5da-179">Service Fabric cluster security</span></span>](service-fabric-cluster-security.md)

[<span data-ttu-id="ce5da-180">Vytvoření clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce5da-180">Service Fabric cluster creation</span></span>](service-fabric-cluster-creation-via-portal.md)

