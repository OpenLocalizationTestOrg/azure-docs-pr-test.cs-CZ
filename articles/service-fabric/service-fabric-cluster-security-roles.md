---
title: "Zabezpečení clusteru Service Fabric: rolí klienta | Microsoft Docs"
description: "Tento článek popisuje hello dva klientské role a oprávnění hello poskytuje toohello role."
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
ms.openlocfilehash: 4a4a9f93e91ea816005b730bebbcb317f8bab255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-for-service-fabric-clients"></a><span data-ttu-id="f1eab-103">Řízení přístupu na základě rolí pro klienty Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f1eab-103">Role-based access control for Service Fabric clients</span></span>
<span data-ttu-id="f1eab-104">Azure Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1eab-104">Azure Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="f1eab-105">Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1eab-105">Access control allows hello cluster administrator toolimit access toocertain cluster operations for different groups of users, making hello cluster more secure.</span></span>  

<span data-ttu-id="f1eab-106">**Správci** mají plný přístup toomanagement funkce (včetně možnosti pro čtení i zápis).</span><span class="sxs-lookup"><span data-stu-id="f1eab-106">**Administrators** have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="f1eab-107">Ve výchozím nastavení **uživatelé** mít pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.</span><span class="sxs-lookup"><span data-stu-id="f1eab-107">By default, **users** only have read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span>

<span data-ttu-id="f1eab-108">V době vytváření clusteru hello tím, že poskytuje samostatné certifikáty pro každou zadáte hello dva klientské role (správce a klient).</span><span class="sxs-lookup"><span data-stu-id="f1eab-108">You specify hello two client roles (administrator and client) at hello time of cluster creation by providing separate certificates for each.</span></span> <span data-ttu-id="f1eab-109">V tématu [zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md) podrobnosti o nastavení zabezpečení clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f1eab-109">See [Service Fabric cluster security](service-fabric-cluster-security.md) for details on setting up a secure Service Fabric cluster.</span></span>

## <a name="default-access-control-settings"></a><span data-ttu-id="f1eab-110">Výchozí nastavení řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="f1eab-110">Default access control settings</span></span>
<span data-ttu-id="f1eab-111">Hello správce přístupu – typ ovládacího prvku má úplný přístup tooall hello FabricClient rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f1eab-111">hello administrator access control type has full access tooall hello FabricClient APIs.</span></span> <span data-ttu-id="f1eab-112">Můžete provádět žádné čtení a zápisy na cluster Service Fabric hello, včetně hello následující operace:</span><span class="sxs-lookup"><span data-stu-id="f1eab-112">It can perform any reads and writes on hello Service Fabric cluster, including hello following operations:</span></span>

### <a name="application-and-service-operations"></a><span data-ttu-id="f1eab-113">Operace služby a aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-113">Application and service operations</span></span>
* <span data-ttu-id="f1eab-114">**CreateService**: vytváření služby</span><span class="sxs-lookup"><span data-stu-id="f1eab-114">**CreateService**: service creation</span></span>                             
* <span data-ttu-id="f1eab-115">**CreateServiceFromTemplate**: vytváření ze šablony služby</span><span class="sxs-lookup"><span data-stu-id="f1eab-115">**CreateServiceFromTemplate**: service creation from template</span></span>                             
* <span data-ttu-id="f1eab-116">**UpdateService**: servis aktualizací</span><span class="sxs-lookup"><span data-stu-id="f1eab-116">**UpdateService**: service updates</span></span>                             
* <span data-ttu-id="f1eab-117">**DeleteService**: Služba odstranění</span><span class="sxs-lookup"><span data-stu-id="f1eab-117">**DeleteService**: service deletion</span></span>                             
* <span data-ttu-id="f1eab-118">**ProvisionApplicationType**: zřizování typ aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-118">**ProvisionApplicationType**: application type provisioning</span></span>                             
* <span data-ttu-id="f1eab-119">**CreateApplication**: vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-119">**CreateApplication**: application creation</span></span>                               
* <span data-ttu-id="f1eab-120">**DeleteApplication**: odstranění aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-120">**DeleteApplication**: application deletion</span></span>                             
* <span data-ttu-id="f1eab-121">**UpgradeApplication**: spuštění nebo přerušení upgradů aplikací</span><span class="sxs-lookup"><span data-stu-id="f1eab-121">**UpgradeApplication**: starting or interrupting application upgrades</span></span>                             
* <span data-ttu-id="f1eab-122">**UnprovisionApplicationType**: rušení zajišťování typ aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-122">**UnprovisionApplicationType**: application type unprovisioning</span></span>                             
* <span data-ttu-id="f1eab-123">**MoveNextUpgradeDomain**: obnovení upgradů aplikací s doménou explicitní aktualizace</span><span class="sxs-lookup"><span data-stu-id="f1eab-123">**MoveNextUpgradeDomain**: resuming application upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="f1eab-124">**ReportUpgradeHealth**: obnovení upgradů aplikací s aktuální průběh upgradu hello</span><span class="sxs-lookup"><span data-stu-id="f1eab-124">**ReportUpgradeHealth**: resuming application upgrades with hello current upgrade progress</span></span>                             
* <span data-ttu-id="f1eab-125">**ReportHealth**: generování sestav stavu</span><span class="sxs-lookup"><span data-stu-id="f1eab-125">**ReportHealth**: reporting health</span></span>                             
* <span data-ttu-id="f1eab-126">**PredeployPackageToNode**: před nasazením rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f1eab-126">**PredeployPackageToNode**: predeployment API</span></span>                            
* <span data-ttu-id="f1eab-127">**CodePackageControl**: restartování balíčky kódu</span><span class="sxs-lookup"><span data-stu-id="f1eab-127">**CodePackageControl**: restarting code packages</span></span>                             
* <span data-ttu-id="f1eab-128">**RecoverPartition**: obnovení oddíl</span><span class="sxs-lookup"><span data-stu-id="f1eab-128">**RecoverPartition**: recovering a partition</span></span>                             
* <span data-ttu-id="f1eab-129">**RecoverPartitions**: obnovení oddíly</span><span class="sxs-lookup"><span data-stu-id="f1eab-129">**RecoverPartitions**: recovering partitions</span></span>                             
* <span data-ttu-id="f1eab-130">**RecoverServicePartitions**: obnovení oddílů služby</span><span class="sxs-lookup"><span data-stu-id="f1eab-130">**RecoverServicePartitions**: recovering service partitions</span></span>                             
* <span data-ttu-id="f1eab-131">**RecoverSystemPartitions**: obnovení systému oddílů služby</span><span class="sxs-lookup"><span data-stu-id="f1eab-131">**RecoverSystemPartitions**: recovering system service partitions</span></span>                             

### <a name="cluster-operations"></a><span data-ttu-id="f1eab-132">Operace clusteru</span><span class="sxs-lookup"><span data-stu-id="f1eab-132">Cluster operations</span></span>
* <span data-ttu-id="f1eab-133">**ProvisionFabric**: MSI nebo clusteru manifest zřizování</span><span class="sxs-lookup"><span data-stu-id="f1eab-133">**ProvisionFabric**: MSI and/or cluster manifest provisioning</span></span>                             
* <span data-ttu-id="f1eab-134">**UpgradeFabric**: spouštění upgrade clusteru</span><span class="sxs-lookup"><span data-stu-id="f1eab-134">**UpgradeFabric**: starting cluster upgrades</span></span>                             
* <span data-ttu-id="f1eab-135">**UnprovisionFabric**: MSI nebo clusteru manifest rušení zajišťování</span><span class="sxs-lookup"><span data-stu-id="f1eab-135">**UnprovisionFabric**: MSI and/or cluster manifest unprovisioning</span></span>                         
* <span data-ttu-id="f1eab-136">**MoveNextFabricUpgradeDomain**: obnovení upgradu clusteru s doménou explicitní aktualizace</span><span class="sxs-lookup"><span data-stu-id="f1eab-136">**MoveNextFabricUpgradeDomain**: resuming cluster upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="f1eab-137">**ReportFabricUpgradeHealth**: obnovení upgradu clusteru s aktuální průběh upgradu hello</span><span class="sxs-lookup"><span data-stu-id="f1eab-137">**ReportFabricUpgradeHealth**: resuming cluster upgrades with hello current upgrade progress</span></span>                             
* <span data-ttu-id="f1eab-138">**StartInfrastructureTask**: spouštění úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="f1eab-138">**StartInfrastructureTask**: starting infrastructure tasks</span></span>                             
* <span data-ttu-id="f1eab-139">**FinishInfrastructureTask**: dokončení úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="f1eab-139">**FinishInfrastructureTask**: finishing infrastructure tasks</span></span>                             
* <span data-ttu-id="f1eab-140">**InvokeInfrastructureCommand**: příkazy pro správu infrastruktury úloh</span><span class="sxs-lookup"><span data-stu-id="f1eab-140">**InvokeInfrastructureCommand**: infrastructure task management commands</span></span>                              
* <span data-ttu-id="f1eab-141">**ActivateNode**: Aktivace uzlu</span><span class="sxs-lookup"><span data-stu-id="f1eab-141">**ActivateNode**: activating a node</span></span>                             
* <span data-ttu-id="f1eab-142">**DeactivateNode**: deaktivace uzlu</span><span class="sxs-lookup"><span data-stu-id="f1eab-142">**DeactivateNode**: deactivating a node</span></span>                             
* <span data-ttu-id="f1eab-143">**DeactivateNodesBatch**: deaktivace několika uzly</span><span class="sxs-lookup"><span data-stu-id="f1eab-143">**DeactivateNodesBatch**: deactivating multiple nodes</span></span>                             
* <span data-ttu-id="f1eab-144">**RemoveNodeDeactivations**: navrácení deaktivace ve více uzlech</span><span class="sxs-lookup"><span data-stu-id="f1eab-144">**RemoveNodeDeactivations**: reverting deactivation on multiple nodes</span></span>                             
* <span data-ttu-id="f1eab-145">**GetNodeDeactivationStatus**: Kontrola stavu deaktivace</span><span class="sxs-lookup"><span data-stu-id="f1eab-145">**GetNodeDeactivationStatus**: checking deactivation status</span></span>                             
* <span data-ttu-id="f1eab-146">**NodeStateRemoved**: generování sestav stav uzlu odebrat</span><span class="sxs-lookup"><span data-stu-id="f1eab-146">**NodeStateRemoved**: reporting node state removed</span></span>                             
* <span data-ttu-id="f1eab-147">**ReportFault**: generování sestav chyb</span><span class="sxs-lookup"><span data-stu-id="f1eab-147">**ReportFault**: reporting fault</span></span>                             
* <span data-ttu-id="f1eab-148">**FileContent**: obrázek přenos souborů klienta úložiště (externí toocluster)</span><span class="sxs-lookup"><span data-stu-id="f1eab-148">**FileContent**: image store client file transfer (external toocluster)</span></span>                             
* <span data-ttu-id="f1eab-149">**FileDownload**: image store klienta soubor stažení spuštění (externí toocluster)</span><span class="sxs-lookup"><span data-stu-id="f1eab-149">**FileDownload**: image store client file download initiation (external toocluster)</span></span>                             
* <span data-ttu-id="f1eab-150">**InternalList**: image store klienta soubor seznamu operaci (interní)</span><span class="sxs-lookup"><span data-stu-id="f1eab-150">**InternalList**: image store client file list operation (internal)</span></span>                             
* <span data-ttu-id="f1eab-151">**Odstranit**: operace odstranění klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="f1eab-151">**Delete**: image store client delete operation</span></span>                              
* <span data-ttu-id="f1eab-152">**Nahrát**: operace nahrávání klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="f1eab-152">**Upload**: image store client upload operation</span></span>                             
* <span data-ttu-id="f1eab-153">**NodeControl**: spuštění, zastavení a restartování uzlů</span><span class="sxs-lookup"><span data-stu-id="f1eab-153">**NodeControl**: starting, stopping, and restarting nodes</span></span>                             
* <span data-ttu-id="f1eab-154">**MoveReplicaControl**: přechod z jednoho uzlu tooanother repliky</span><span class="sxs-lookup"><span data-stu-id="f1eab-154">**MoveReplicaControl**: moving replicas from one node tooanother</span></span>                             

### <a name="miscellaneous-operations"></a><span data-ttu-id="f1eab-155">Různým operacím</span><span class="sxs-lookup"><span data-stu-id="f1eab-155">Miscellaneous operations</span></span>
* <span data-ttu-id="f1eab-156">**Příkaz ping**: příkazy ping klienta</span><span class="sxs-lookup"><span data-stu-id="f1eab-156">**Ping**: client pings</span></span>                             
* <span data-ttu-id="f1eab-157">**Dotaz**: všechny dotazy na povoleno</span><span class="sxs-lookup"><span data-stu-id="f1eab-157">**Query**: all queries allowed</span></span>
* <span data-ttu-id="f1eab-158">**NameExists**: pojmenování URI existence kontroly</span><span class="sxs-lookup"><span data-stu-id="f1eab-158">**NameExists**: naming URI existence checks</span></span>                             

<span data-ttu-id="f1eab-159">Typ řízení přístupu uživatele Hello je ve výchozím nastavení, omezené toohello následující operace:</span><span class="sxs-lookup"><span data-stu-id="f1eab-159">hello user access control type is, by default, limited toohello following operations:</span></span> 

* <span data-ttu-id="f1eab-160">**EnumerateSubnames**: pojmenování URI – výčet</span><span class="sxs-lookup"><span data-stu-id="f1eab-160">**EnumerateSubnames**: naming URI enumeration</span></span>                             
* <span data-ttu-id="f1eab-161">**EnumerateProperties**: názvy vlastností – výčet</span><span class="sxs-lookup"><span data-stu-id="f1eab-161">**EnumerateProperties**: naming property enumeration</span></span>                             
* <span data-ttu-id="f1eab-162">**PropertyReadBatch**: názvy vlastností operace čtení</span><span class="sxs-lookup"><span data-stu-id="f1eab-162">**PropertyReadBatch**: naming property read operations</span></span>                             
* <span data-ttu-id="f1eab-163">**GetServiceDescription**: oznámení služby dlouho dotazování a čtení služby popisy</span><span class="sxs-lookup"><span data-stu-id="f1eab-163">**GetServiceDescription**: long-poll service notifications and reading service descriptions</span></span>                             
* <span data-ttu-id="f1eab-164">**ResolveService**: řešení služeb na základě předpisy</span><span class="sxs-lookup"><span data-stu-id="f1eab-164">**ResolveService**: complaint-based service resolution</span></span>                             
* <span data-ttu-id="f1eab-165">**ResolveNameOwner**: řešení pojmenování URI vlastníka</span><span class="sxs-lookup"><span data-stu-id="f1eab-165">**ResolveNameOwner**: resolving naming URI owner</span></span>                             
* <span data-ttu-id="f1eab-166">**ResolvePartition**: řešení systémových služeb</span><span class="sxs-lookup"><span data-stu-id="f1eab-166">**ResolvePartition**: resolving system services</span></span>                             
* <span data-ttu-id="f1eab-167">**ServiceNotifications**: oznámení služeb na základě událostí</span><span class="sxs-lookup"><span data-stu-id="f1eab-167">**ServiceNotifications**: event-based service notifications</span></span>                             
* <span data-ttu-id="f1eab-168">**GetUpgradeStatus**: dotazování na stav upgradu aplikace</span><span class="sxs-lookup"><span data-stu-id="f1eab-168">**GetUpgradeStatus**: polling application upgrade status</span></span>                             
* <span data-ttu-id="f1eab-169">**GetFabricUpgradeStatus**: dotazování na stav upgradu clusteru</span><span class="sxs-lookup"><span data-stu-id="f1eab-169">**GetFabricUpgradeStatus**: polling cluster upgrade status</span></span>                             
* <span data-ttu-id="f1eab-170">**InvokeInfrastructureQuery**: dotazování úlohy infrastruktury</span><span class="sxs-lookup"><span data-stu-id="f1eab-170">**InvokeInfrastructureQuery**: querying infrastructure tasks</span></span>                             
* <span data-ttu-id="f1eab-171">**Seznam**: operace seznamu soubor klienta úložiště image</span><span class="sxs-lookup"><span data-stu-id="f1eab-171">**List**: image store client file list operation</span></span>                             
* <span data-ttu-id="f1eab-172">**ResetPartitionLoad**: resetování zatížení jednotky převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="f1eab-172">**ResetPartitionLoad**: resetting load for a failover unit</span></span>                             
* <span data-ttu-id="f1eab-173">**ToggleVerboseServicePlacementHealthReporting**: přepnutím stavu umístění podrobné služby generování sestav</span><span class="sxs-lookup"><span data-stu-id="f1eab-173">**ToggleVerboseServicePlacementHealthReporting**: toggling verbose service placement health reporting</span></span>                             

<span data-ttu-id="f1eab-174">řízení přístupu správce Hello má také přístup toohello předcházející operace.</span><span class="sxs-lookup"><span data-stu-id="f1eab-174">hello admin access control also has access toohello preceding operations.</span></span>

## <a name="changing-default-settings-for-client-roles"></a><span data-ttu-id="f1eab-175">Změna výchozí nastavení klienta rolí</span><span class="sxs-lookup"><span data-stu-id="f1eab-175">Changing default settings for client roles</span></span>
<span data-ttu-id="f1eab-176">V souboru manifestu clusteru hello můžete v případě potřeby zadejte správce možnosti toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="f1eab-176">In hello cluster manifest file, you can provide admin capabilities toohello client if needed.</span></span> <span data-ttu-id="f1eab-177">Můžete změnit výchozí nastavení hello budete toohello **nastavení prostředků infrastruktury** možnost během [vytvoření clusteru](service-fabric-cluster-creation-via-portal.md)a poskytuje hello předcházející nastavení v hello **název**, **správce**, **uživatele**, a **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="f1eab-177">You can change hello defaults by going toohello **Fabric Settings** option during [cluster creation](service-fabric-cluster-creation-via-portal.md), and providing hello preceding settings in hello **name**, **admin**, **user**, and **value** fields.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1eab-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1eab-178">Next steps</span></span>
[<span data-ttu-id="f1eab-179">Zabezpečení clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f1eab-179">Service Fabric cluster security</span></span>](service-fabric-cluster-security.md)

[<span data-ttu-id="f1eab-180">Vytvoření clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f1eab-180">Service Fabric cluster creation</span></span>](service-fabric-cluster-creation-via-portal.md)

