---
title: "Odeberte servery a zakažte | Microsoft Docs"
description: "Tento článek popisuje, jak zrušit registraci serveru z trezoru Site Recovery a zakažte ochranu pro virtuální počítače a fyzické servery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="fd8fd-103">Odebrání serverů a zakázání ochrany</span><span class="sxs-lookup"><span data-stu-id="fd8fd-103">Remove servers and disable protection</span></span>

<span data-ttu-id="fd8fd-104">Služba Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-104">The Azure Site Recovery service contributes to your business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="fd8fd-105">Služba orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-105">The service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="fd8fd-106">Počítače je možné replikovat do Azure nebo do sekundárního místního datového centra.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-106">Machines can be replicated to Azure, or to a secondary on-premises data center.</span></span> <span data-ttu-id="fd8fd-107">Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="fd8fd-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="fd8fd-108">Tento článek popisuje, jak zrušit registraci serveru z trezoru služeb zotavení na portálu Azure a jak zakázat ochranu pro počítače chráněné službou Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-108">This article describes how to unregister servers from a Recovery Services vault in the Azure portal, and how to disable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="fd8fd-109">Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="fd8fd-109">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="fd8fd-110">Zrušit registraci připojené konfigurační server</span><span class="sxs-lookup"><span data-stu-id="fd8fd-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="fd8fd-111">Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do Azure, můžete zrušit registraci připojené konfigurační server z trezoru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-111">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="fd8fd-112">Zakažte ochranu počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-112">Disable machine protection.</span></span> <span data-ttu-id="fd8fd-113">V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-113">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="fd8fd-114">Zrušit přidružení žádné zásady.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-114">Disassociate any policies.</span></span> <span data-ttu-id="fd8fd-115">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **zásady replikace**, dvakrát klikněte na panel s přiřazenou třídou zásady.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="fd8fd-116">Klikněte pravým tlačítkem na konfigurační server > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-116">Right-click the configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="fd8fd-117">Odeberte všechny další místní proces a hlavních cílových serverů.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="fd8fd-118">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na serveru > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="fd8fd-119">Odstraňte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-119">Delete the configuration server.</span></span>
5. <span data-ttu-id="fd8fd-120">Ručně odinstalujte službu Mobility na hlavním cílovém serveru spuštěna (bude jím buď samostatný server, nebo spuštěné na konfiguračním serveru).</span><span class="sxs-lookup"><span data-stu-id="fd8fd-120">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
6. <span data-ttu-id="fd8fd-121">Odinstalujte všechny servery další proces.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="fd8fd-122">Odinstalujte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-122">Uninstall the configuration server.</span></span>
8. <span data-ttu-id="fd8fd-123">Na konfiguračním serveru odinstalujte instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-123">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="fd8fd-124">V registru konfigurační server odstranit klíč ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-124">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="fd8fd-125">Zrušit registraci serveru bez připojení konfigurace</span><span class="sxs-lookup"><span data-stu-id="fd8fd-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="fd8fd-126">Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do Azure, můžete zrušit registraci serveru bez připojení konfiguraci z úložiště následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-126">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="fd8fd-127">Zakažte ochranu počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-127">Disable machine protection.</span></span> <span data-ttu-id="fd8fd-128">V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-128">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span> <span data-ttu-id="fd8fd-129">Vyberte **přestat spravovat počítač**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-129">Select **Stop managing the machine**.</span></span>
2. <span data-ttu-id="fd8fd-130">Odeberte všechny další místní proces a hlavních cílových serverů.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="fd8fd-131">V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na serveru > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
3. <span data-ttu-id="fd8fd-132">Odstraňte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-132">Delete the configuration server.</span></span>
4. <span data-ttu-id="fd8fd-133">Ručně odinstalujte službu Mobility na hlavním cílovém serveru spuštěna (bude jím buď samostatný server, nebo spuštěné na konfiguračním serveru).</span><span class="sxs-lookup"><span data-stu-id="fd8fd-133">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
5. <span data-ttu-id="fd8fd-134">Odinstalujte všechny servery další proces.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="fd8fd-135">Odinstalujte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-135">Uninstall the configuration server.</span></span>
7. <span data-ttu-id="fd8fd-136">Na konfiguračním serveru odinstalujte instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-136">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="fd8fd-137">V registru konfigurační server odstranit klíč ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-137">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="fd8fd-138">Zrušení registrace připojený k serveru VMM</span><span class="sxs-lookup"><span data-stu-id="fd8fd-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="fd8fd-139">Jako osvědčený postup doporučujeme zrušit registraci serveru VMM, pokud je připojený k Azure.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-139">As a best practice, we recommend that you unregister the VMM server when it's connected to Azure.</span></span> <span data-ttu-id="fd8fd-140">Tím se zajistí, že jsou správně vyčistit nastavení na serveru VMM (a na jiných serverech VMM s spárované cloudy).</span><span class="sxs-lookup"><span data-stu-id="fd8fd-140">This ensures that settings on the VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="fd8fd-141">Bez připojení serveru byste měli odebrat, pouze pokud je trvalé potíže s připojením.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="fd8fd-142">Pokud VMM server není připojený, musíte ručně spustit skript k vyčištění nastavení.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-142">If the VMM server isn’t connected, you will need to manually run a script to clean up settings.</span></span>

1. <span data-ttu-id="fd8fd-143">Zastavení replikace virtuálních počítačů v cloudu na VMM serveru, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-143">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="fd8fd-144">Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-144">Delete any network mappings used by clouds on the VMM server you want to delete.</span></span> <span data-ttu-id="fd8fd-145">V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="fd8fd-146">Zrušit přidružení zásad replikace z cloudy na serveru VMM, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-146">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="fd8fd-147">V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="fd8fd-148">Klikněte pravým tlačítkem na cloudu > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-148">Right-click the cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="fd8fd-149">Odstraňte VMM server nebo aktivním uzlu VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-149">Delete the VMM server or active VMM node.</span></span> <span data-ttu-id="fd8fd-150">V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, pravým tlačítkem na server > **odstranit** .</span><span class="sxs-lookup"><span data-stu-id="fd8fd-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
5. <span data-ttu-id="fd8fd-151">Zprostředkovatel na serveru VMM ručně odinstalujte.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-151">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="fd8fd-152">Pokud máte cluster, odeberte ze všech uzlů.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="fd8fd-153">Pokud replikujete do Azure, ručně odeberte agenta služeb zotavení Microsoft z hostitelů Hyper-V v cloudech odstraněné.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-153">If you're replicating to Azure, manually remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="fd8fd-154">Zrušit registraci serveru VMM bez připojení</span><span class="sxs-lookup"><span data-stu-id="fd8fd-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="fd8fd-155">Zastavení replikace virtuálních počítačů v cloudu na VMM serveru, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-155">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="fd8fd-156">Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-156">Delete any network mappings used by clouds on the VMM server that you want to delete.</span></span> <span data-ttu-id="fd8fd-157">V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě > **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="fd8fd-158">Poznamenejte si ID serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-158">Note the ID of the VMM server.</span></span>
4. <span data-ttu-id="fd8fd-159">Zrušit přidružení zásad replikace z cloudy na serveru VMM, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-159">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="fd8fd-160">V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="fd8fd-161">Klikněte pravým tlačítkem na cloudu > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-161">Right-click the cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="fd8fd-162">Odstraňte VMM server nebo aktivní uzel.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-162">Delete the VMM server or active node.</span></span> <span data-ttu-id="fd8fd-163">V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, pravým tlačítkem na server > **odstranit** .</span><span class="sxs-lookup"><span data-stu-id="fd8fd-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
6. <span data-ttu-id="fd8fd-164">Stažení a spuštění [skript pro vyčištění](http://aka.ms/asr-cleanup-script-vmm) na serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-164">Download and run the [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on the VMM server.</span></span> <span data-ttu-id="fd8fd-165">Otevřete prostředí PowerShell se **spustit jako správce** možnost, chcete-li změnit zásady spouštění pro obor výchozí (LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="fd8fd-165">Open PowerShell with the **Run as Administrator** option, to change the execution policy for the default (LocalMachine) scope.</span></span> <span data-ttu-id="fd8fd-166">Ve skriptu zadejte ID serveru VMM, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-166">In the script, specify the ID of the VMM server you want to remove.</span></span> <span data-ttu-id="fd8fd-167">Skript odebere registrace a cloudového párování informací ze serveru.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-167">The script removes registration and cloud pairing information from the server.</span></span>
5. <span data-ttu-id="fd8fd-168">Spusťte skript vyčištění na jiných serverech VMM, které obsahují cloudy, které jsou spárovaný s cloudy na serveru VMM, který chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-168">Run the cleanup script on any other VMM servers that contain clouds that are paired with clouds on the VMM server you want to remove.</span></span>
6. <span data-ttu-id="fd8fd-169">Spusťte skript vyčištění na žádné jiné pasivní uzly clusteru VMM které mají nainstalovaného zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-169">Run the  cleanup script on any other passive VMM cluster nodes that have the Provider installed.</span></span>
7. <span data-ttu-id="fd8fd-170">Zprostředkovatel na serveru VMM ručně odinstalujte.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-170">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="fd8fd-171">Pokud máte cluster, odeberte ze všech uzlů.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="fd8fd-172">Pokud jste replikaci do Azure, můžete odebrat agenta služeb zotavení Microsoft z hostitelů Hyper-V v cloudech odstraněné.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-172">If you replicating to Azure, you can remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="fd8fd-173">Zrušit registraci hostitele Hyper-V v lokalitě Hyper-V</span><span class="sxs-lookup"><span data-stu-id="fd8fd-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="fd8fd-174">Hostitelé Hyper-V, které nejsou spravované přes VMM jsou shromážděná do lokality Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="fd8fd-175">Odeberte hostitele v lokalitě Hyper-V následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="fd8fd-176">Zakážete replikaci pro virtuální počítače Hyper-V umístěný na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-176">Disable replication for Hyper-V VMs located on the host.</span></span>
2. <span data-ttu-id="fd8fd-177">Zrušit přidružení zásad pro web Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-177">Disassociate policies for the Hyper-V site.</span></span> <span data-ttu-id="fd8fd-178">V **infrastruktura Site Recovery** > **pro weby technologie Hyper-V** >  **zásady replikace**, dvakrát klikněte na přidružené zásady.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="fd8fd-179">Klikněte pravým tlačítkem na webu > **zrušení spojení**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-179">Right-click the site > **Disassociate**.</span></span>
3. <span data-ttu-id="fd8fd-180">Odstraňte hostitele Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="fd8fd-181">V **infrastruktura Site Recovery** > **pro System Center VMM** > **hostitelů Hyper-V**, pravým tlačítkem na server >  **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="fd8fd-182">Po odebrání všech hostitelů z něj, odstraníte web Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-182">Delete the Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="fd8fd-183">V **infrastruktura Site Recovery** > **pro System Center VMM** > **lokalit Hyper-V**, klikněte pravým tlačítkem na webu > **odstranit** .</span><span class="sxs-lookup"><span data-stu-id="fd8fd-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click the site > **Delete**.</span></span>
5. <span data-ttu-id="fd8fd-184">Spusťte následující skript na každém hostiteli technologie Hyper-V, který jste odebrali.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-184">Run the following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="fd8fd-185">Skript vyčistí nastavení na serveru a zruší registraci v trezoru.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-185">The script cleans up settings on the server, and unregisters it from the vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="fd8fd-186">Zakažte ochranu pro virtuální počítač VMware nebo fyzických serverů</span><span class="sxs-lookup"><span data-stu-id="fd8fd-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="fd8fd-187">V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-187">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="fd8fd-188">V **odebrat počítač**, vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="fd8fd-189">**Zakažte ochranu pro počítač (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-189">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="fd8fd-190">Tuto možnost použijte k zastavení replikace na počítač.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-190">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="fd8fd-191">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="fd8fd-192">Zobrazí se jenom tato možnost v následujících případech:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-192">You will only see this option in the following circumstances:</span></span>
        - <span data-ttu-id="fd8fd-193">**Jste ke změně velikosti svazku virtuálního počítače**– při změně velikosti svazku virtuální počítač přejde do kritického stavu.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-193">**You've resized the VM volume**—When you resize a volume the virtual machine goes into a critical state.</span></span> <span data-ttu-id="fd8fd-194">Tato možnost zakáže ochrany a přitom zachovat body obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-194">Select this option to disables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="fd8fd-195">Pokud povolíte ochranu pro tento počítač znovu, data pro změněnou velikostí svazku se převede do Azure.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-195">When you enable protection for the machine again, the data for the resized volume will be transferred to Azure.</span></span>
        - <span data-ttu-id="fd8fd-196">**Nedávno jste si vyzkoušeli převzetí služeb při selhání**– po spuštění převzetí služeb při selhání pro testovací prostředí, vyberte tuto možnost, chcete-li začít s ochranou místní počítače znovu.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-196">**You've recently run a failover**—After you've run a failover to test your environment, select this option to start protecting on-premises machines again.</span></span> <span data-ttu-id="fd8fd-197">Zakáže každý virtuální počítač a pak budete muset znovu povolte ochranu pro ně.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-197">It disables each virtual machine, and then you need to enable protection for them again.</span></span> <span data-ttu-id="fd8fd-198">Vypnutí počítače se toto nastavení nemá vliv virtuální počítač repliky v Azure.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-198">Disabling the machine with this setting doesn't affect the replica virtual machine in Azure.</span></span> <span data-ttu-id="fd8fd-199">Nemáte odinstalujte službu Mobility z počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-199">Don't uninstall the Mobility service from the machine.</span></span>
    - <span data-ttu-id="fd8fd-200">**Přestat spravovat počítač**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-200">**Stop managing the machine**.</span></span> <span data-ttu-id="fd8fd-201">Pokud vyberete tuto možnost, počítač se jenom odeberou z trezoru.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-201">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="fd8fd-202">Místní nastavení ochrany pro počítač nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-202">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="fd8fd-203">K odebrání nastavení na počítači a odeberte počítač z předplatného Azure, budete muset vyčistit nastavení odinstalací služba Mobility.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-203">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings by uninstalling the Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="fd8fd-204">Zakažte ochranu pro virtuální počítač technologie Hyper-V v cloudu VMM</span><span class="sxs-lookup"><span data-stu-id="fd8fd-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="fd8fd-205">V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-205">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="fd8fd-206">V **odebrat počítač**, vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="fd8fd-207">**Zakažte ochranu pro počítač (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-207">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="fd8fd-208">Tuto možnost použijte k zastavení replikace na počítač.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-208">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="fd8fd-209">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="fd8fd-210">**Přestat spravovat počítač**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-210">**Stop managing the machine**.</span></span> <span data-ttu-id="fd8fd-211">Pokud vyberete tuto možnost, počítač se jenom odeberou z trezoru.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-211">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="fd8fd-212">Místní nastavení ochrany pro počítač nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-212">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="fd8fd-213">K odebrání nastavení na počítači a odeberte počítač z předplatného Azure, budete muset ručně vyčistit nastavení podle pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-213">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings up manually, using the instructions below.</span></span> <span data-ttu-id="fd8fd-214">Všimněte si, že pokud vyberete možnost odstranit virtuální počítač a jeho pevné disky, jejich budete odeberou z cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-214">Note that if you select to delete the virtual machine and its hard disks, they'll be removed from the target location.</span></span>

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a><span data-ttu-id="fd8fd-215">Vyčištění nastavení ochrany - replikace do sekundární lokality VMM</span><span class="sxs-lookup"><span data-stu-id="fd8fd-215">Clean up protection settings - replication to a secondary VMM site</span></span>

<span data-ttu-id="fd8fd-216">Pokud jste vybrali **přestat spravovat počítač** a replikace do sekundární lokality, spusťte tento skript na primárním serveru a vyčistit nastavení pro primární virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-216">If you selected **Stop managing the machine** and you replicating to a secondary site, run this script on the primary server to clean up the settings for the primary virtual machine.</span></span> <span data-ttu-id="fd8fd-217">V konzole VMM klikněte na tlačítko prostředí PowerShell pro otevření konzoly VMM PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-217">In the VMM console click the PowerShell button to open the VMM PowerShell console.</span></span> <span data-ttu-id="fd8fd-218">Nahraďte SQLVM1 název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-218">Replace SQLVM1 with the name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="fd8fd-219">Na sekundárním serveru VMM spuštěním tohoto skriptu vyčistit nastavení pro sekundární virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-219">On the secondary VMM server run this script to clean up the settings for the secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="fd8fd-220">Na sekundárním serveru VMM aktualizujte virtuální počítače na serveru hostitele technologie Hyper-V tak, aby sekundární virtuální počítač získá zjistil znovu v konzole nástroje VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-220">On the secondary VMM server, refresh the virtual machines on the Hyper-V host server, so that the secondary VM gets detected again in the VMM console.</span></span>
4. <span data-ttu-id="fd8fd-221">Výše uvedené kroky vymazat nastavení replikace na serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-221">The above steps clear up the replication settings on the VMM server.</span></span> <span data-ttu-id="fd8fd-222">Pokud chcete na zastavení replikace pro virtuální počítač, spusťte následující skript jejda primární a sekundární virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-222">If you want to stop replication for the virtual machine, run the following script oh the primary and secondary VMs.</span></span> <span data-ttu-id="fd8fd-223">Nahraďte SQLVM1 název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-223">Replace SQLVM1 with the name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a><span data-ttu-id="fd8fd-224">Vyčištění nastavení ochrany - replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-224">Clean up protection settings - replication to Azure</span></span>

1. <span data-ttu-id="fd8fd-225">Pokud jste vybrali **přestat spravovat počítač** a replikovat do Azure, spusťte tento skript na zdrojovém serveru VMM pomocí prostředí PowerShell pomocí konzoly VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-225">If you selected **Stop managing the machine** and you replicate to Azure, run this script on the source VMM server, using PowerShell from the VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="fd8fd-226">Výše uvedené kroky vymazat nastavení replikace na serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-226">The above steps clear the replication settings on the VMM server.</span></span> <span data-ttu-id="fd8fd-227">Na zastavení replikace pro virtuální počítač běží na serveru hostitele technologie Hyper-V, spusťte tento skript.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-227">To stop replication for the virtual machine running on the Hyper-V host server, run this script.</span></span> <span data-ttu-id="fd8fd-228">Nahraďte SQLVM1 název virtuálního počítače a host01.contoso.com s názvem serveru hostitele technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-228">Replace SQLVM1 with the name of your virtual machine, and host01.contoso.com with the name of the Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="fd8fd-229">Zakažte ochranu pro virtuální počítač technologie Hyper-V v lokalitě Hyper-V</span><span class="sxs-lookup"><span data-stu-id="fd8fd-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="fd8fd-230">Tento postup použijte, pokud replikujete virtuální počítače Hyper-V do Azure bez serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-230">Use this procedure if you're replicating Hyper-V VMs to Azure without a VMM server.</span></span>

1. <span data-ttu-id="fd8fd-231">V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-231">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="fd8fd-232">V **odebrat počítač**, můžete vybrat následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="fd8fd-232">In **Remove Machine**, you can select the following options:</span></span>

   - <span data-ttu-id="fd8fd-233">**Zakažte ochranu pro počítač (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-233">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="fd8fd-234">Tuto možnost použijte k zastavení replikace na počítač.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-234">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="fd8fd-235">Nastavení obnovení lokality se automaticky vyčistí.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="fd8fd-236">**Přestat spravovat počítač**.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-236">**Stop managing the machine**.</span></span> <span data-ttu-id="fd8fd-237">Pokud vyberete tuto možnost na počítač se jenom odeberou z trezoru.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-237">If you select this option the machine will only be removed from the vault.</span></span> <span data-ttu-id="fd8fd-238">Místní nastavení ochrany pro počítač nebude mít vliv.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-238">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="fd8fd-239">K odebrání nastavení na počítači a odeberte virtuální počítač z předplatného Azure, budete muset ručně vyčistit nastavení.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-239">To remove settings on the machine, and to remove the virtual machine from the Azure subscription, you need to clean the settings up manually.</span></span> <span data-ttu-id="fd8fd-240">Pokud vyberete možnost odstranit virtuální počítač a jeho pevné disky, které budou odebrány z cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-240">If you select to delete the virtual machine and its hard disks they will be removed from the target location.</span></span>
3. <span data-ttu-id="fd8fd-241">Pokud jste vybrali **přestat spravovat počítač**, spusťte tento skript na hostitelském serveru zdroj technologie Hyper-V, odebrat replikaci pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-241">If you selected **Stop managing the machine**, run this script on the source Hyper-V host server, to remove replication for the virtual machine.</span></span> <span data-ttu-id="fd8fd-242">Nahraďte SQLVM1 název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fd8fd-242">Replace SQLVM1 with the name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
