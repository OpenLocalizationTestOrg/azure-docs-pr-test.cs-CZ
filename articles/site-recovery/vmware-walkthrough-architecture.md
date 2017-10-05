---
title: Zkontrolujte architekturu replikace VMware do Azure | Microsoft Docs
description: "Tento článek obsahuje přehled součásti a architektura použít při replikaci místní virtuální počítače VMware do Azure se službou Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27.017
ms.author: raynew
ms.openlocfilehash: 4aae04a793bab11562c20ceec0e1ae8f1a035a0f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-1-review-the-architecture-for-vmware-replication-to-azure"></a><span data-ttu-id="245d8-103">Krok 1: Posouzení architekturu replikace VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="245d8-103">Step 1: Review the architecture for VMware replication to Azure</span></span>

<span data-ttu-id="245d8-104">Tento článek popisuje součásti a procesům používaným při replikaci virtuálních počítačů VMware místně na Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="245d8-104">This article describes the components and processes used when replicating on-premises VMware virtual machines to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="245d8-105">Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="245d8-105">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="architectural-components"></a><span data-ttu-id="245d8-106">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="245d8-106">Architectural components</span></span>

<span data-ttu-id="245d8-107">Tabulka shrnuje součásti, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="245d8-107">The table summarizes the components you need.</span></span>

<span data-ttu-id="245d8-108">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="245d8-108">**Component**</span></span> | <span data-ttu-id="245d8-109">**Požadavek**</span><span class="sxs-lookup"><span data-stu-id="245d8-109">**Requirement**</span></span> | <span data-ttu-id="245d8-110">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="245d8-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="245d8-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="245d8-111">**Azure**</span></span> | <span data-ttu-id="245d8-112">Potřebujete předplatné Azure, účet úložiště Azure a sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-112">You need an Azure subscription, an Azure storage account, and an Azure network.</span></span> | <span data-ttu-id="245d8-113">Replikovaná data se ukládají v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="245d8-113">Replicated data is stored in the storage account.</span></span> <span data-ttu-id="245d8-114">Virtuální počítače Azure vytvořené mají replikovaná data, když dojde k převzetí služeb při selhání z vaší místní lokalitě.</span><span class="sxs-lookup"><span data-stu-id="245d8-114">Azure VMs are created with the replicated data when failover from your on-premises site occurs.</span></span> <span data-ttu-id="245d8-115">Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.</span><span class="sxs-lookup"><span data-stu-id="245d8-115">The Azure VMs connect to the Azure virtual network when they're created.</span></span>
<span data-ttu-id="245d8-116">**Konfigurační server**</span><span class="sxs-lookup"><span data-stu-id="245d8-116">**Configuration server**</span></span> | <span data-ttu-id="245d8-117">Jeden server správy (virtuální počítač VMware), který spouští všechny místní součásti Site Recovery pro nasazení na místní.</span><span class="sxs-lookup"><span data-stu-id="245d8-117">A single on-premises management server (VMware VM) that runs all the on-premises Site Recovery components for the deployment.</span></span> <span data-ttu-id="245d8-118">Mezi ně patří, a konfigurační server, procesový server, hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="245d8-118">These include a configuration server, process server, master target server.</span></span> | <span data-ttu-id="245d8-119">Komponenta konfiguračního serveru koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="245d8-119">The configuration server component coordinates communications between on-premises and Azure, and manages data replication.</span></span>
 <span data-ttu-id="245d8-120">**Procesový server:**</span><span class="sxs-lookup"><span data-stu-id="245d8-120">**Process server**:</span></span>  | <span data-ttu-id="245d8-121">Obvykle se instaluje na konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="245d8-121">Installed by default on the configuration server.</span></span> | <span data-ttu-id="245d8-122">Funguje jako replikační brána.</span><span class="sxs-lookup"><span data-stu-id="245d8-122">Acts as a replication gateway.</span></span> <span data-ttu-id="245d8-123">Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-123">Receives replication data, optimizes it with caching, compression, and encryption, and sends it to Azure storage.</span></span><br/><br/> <span data-ttu-id="245d8-124">Procesový server se také stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware.</span><span class="sxs-lookup"><span data-stu-id="245d8-124">The process server also handles push installation of the Mobility service to protected machines, and performs automatic discovery of VMware VMs.</span></span><br/><br/> <span data-ttu-id="245d8-125">Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.</span><span class="sxs-lookup"><span data-stu-id="245d8-125">As your deployment grows you can add additional separate dedicated process servers to handle increasing volumes of replication traffic.</span></span>
 <span data-ttu-id="245d8-126">**Hlavní cílový server**</span><span class="sxs-lookup"><span data-stu-id="245d8-126">**Master target server**</span></span> | <span data-ttu-id="245d8-127">Obvykle se instaluje na místní konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="245d8-127">Installed by default on the on-premises configuration server.</span></span> | <span data-ttu-id="245d8-128">Zpracovává replikační data během navracení služeb z Azure po obnovení.</span><span class="sxs-lookup"><span data-stu-id="245d8-128">Handles replication data during failback from Azure.</span></span><br/><br/> <span data-ttu-id="245d8-129">Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="245d8-129">If volumes of failback traffic are high, you can deploy a separate master target server for failback.</span></span>
<span data-ttu-id="245d8-130">**Servery VMware**</span><span class="sxs-lookup"><span data-stu-id="245d8-130">**VMware servers**</span></span> | <span data-ttu-id="245d8-131">Virtuální počítače VMware jsou hostované na serverech vSphere ESXi a ke správě hostitelů doporučujeme použít server vCenter.</span><span class="sxs-lookup"><span data-stu-id="245d8-131">VMware VMs are hosted on vSphere ESXi servers, and we recommend a vCenter server to manage the hosts.</span></span> | <span data-ttu-id="245d8-132">Servery VMware přidáte do trezoru služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="245d8-132">You add VMware servers to your Recovery Services vault.</span></span>
<span data-ttu-id="245d8-133">**Replikované počítače**</span><span class="sxs-lookup"><span data-stu-id="245d8-133">**Replicated machines**</span></span> | <span data-ttu-id="245d8-134">Služba Mobility se nainstalují na jednotlivé virtuální počítače VMware můžete replikovat.</span><span class="sxs-lookup"><span data-stu-id="245d8-134">The Mobility service will be installed on each VMware VM you replicate.</span></span> <span data-ttu-id="245d8-135">Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.</span><span class="sxs-lookup"><span data-stu-id="245d8-135">It can be installed manually on each machine, or with a push installation from the process server.</span></span>

<span data-ttu-id="245d8-136">**Obr. 1: Komponenty pro replikaci z VMware do Azure**</span><span class="sxs-lookup"><span data-stu-id="245d8-136">**Figure 1: VMware to Azure components**</span></span>

![Komponenty](./media/vmware-walkthrough-architecture/arch-enhanced.png)

## <a name="replication-process"></a><span data-ttu-id="245d8-138">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="245d8-138">Replication process</span></span>

1. <span data-ttu-id="245d8-139">Nastavení nasazení, včetně místní a Azure součásti.</span><span class="sxs-lookup"><span data-stu-id="245d8-139">You set up the deployment, including on-premises and Azure components.</span></span> <span data-ttu-id="245d8-140">V trezoru služeb zotavení je třeba zadat replikace zdroje a cíle, nastavení konfigurace serveru, vytvořte zásadu replikace, nasazení služby Mobility, povolit replikaci a spustit testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="245d8-140">In the Recovery Services vault, you specify the replication source and target, set up the configuration server, create a replication policy, deploy the Mobility service, enable replication, and run a test failover.</span></span>
2. <span data-ttu-id="245d8-141">Replikovat počítače v souladu s zásady replikace a počáteční kopii data se replikují do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-141">Machines replicate in accordance with the replication policy, and an initial copy of the data is replicated to Azure storage.</span></span>
3. <span data-ttu-id="245d8-142">Po dokončení počáteční replikace začne replikace rozdílové změny do Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-142">After initial replication finishes, replication of delta changes to Azure begins.</span></span> <span data-ttu-id="245d8-143">Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.</span><span class="sxs-lookup"><span data-stu-id="245d8-143">Tracked changes for a machine are held in a .hrl file.</span></span>
    - <span data-ttu-id="245d8-144">Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.</span><span class="sxs-lookup"><span data-stu-id="245d8-144">Replicating machines communicate with the configuration server on port HTTPS 443 inbound, for replication management.</span></span>
    - <span data-ttu-id="245d8-145">Replikaci počítačů odesílat data replikace na procesový server na port HTTPS 9443 příchozí (lze upravit).</span><span class="sxs-lookup"><span data-stu-id="245d8-145">Replicating machines send replication data to the process server on port HTTPS 9443 inbound (can be modified).</span></span>
    - <span data-ttu-id="245d8-146">Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.</span><span class="sxs-lookup"><span data-stu-id="245d8-146">The configuration server orchestrates replication management with Azure over port HTTPS 443 outbound.</span></span>
    - <span data-ttu-id="245d8-147">Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.</span><span class="sxs-lookup"><span data-stu-id="245d8-147">The process server receives data from source machines, optimizes and encrypts it, and sends it to Azure storage over port 443 outbound.</span></span>
    - <span data-ttu-id="245d8-148">Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004.</span><span class="sxs-lookup"><span data-stu-id="245d8-148">If you enable multi-VM consistency, then machines in the replication group communicate with each other over port 20004.</span></span> <span data-ttu-id="245d8-149">Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="245d8-149">Multi-VM is used if you group multiple machines into replication groups that share crash-consistent and app-consistent recovery points when they fail over.</span></span> <span data-ttu-id="245d8-150">To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.</span><span class="sxs-lookup"><span data-stu-id="245d8-150">This is useful if machines are running the same workload and need to be consistent.</span></span>
4. <span data-ttu-id="245d8-151">Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-151">Traffic is replicated to Azure storage public endpoints, over the internet.</span></span> <span data-ttu-id="245d8-152">Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="245d8-152">Alternately, you can use Azure ExpressRoute [public peering](../expressroute/expressroute-circuit-peerings.md#public-peering).</span></span> <span data-ttu-id="245d8-153">Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.</span><span class="sxs-lookup"><span data-stu-id="245d8-153">Replicating traffic over a site-to-site VPN from an on-premises site to Azure isn't supported.</span></span>


<span data-ttu-id="245d8-154">**Obr. 2: Replikace z VMware do Azure**</span><span class="sxs-lookup"><span data-stu-id="245d8-154">**Figure 2: VMware to Azure replication**</span></span>

![Rozšířené](./media/vmware-walkthrough-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="245d8-156">Proces převzetí služeb při selhání a navrácení služeb po obnovení</span><span class="sxs-lookup"><span data-stu-id="245d8-156">Failover and failback process</span></span>

1. <span data-ttu-id="245d8-157">Po ověření správného fungování testovacího převzetí služeb při selhání můžete podle potřeby spouštět neplánovaná převzetí služeb při selhání do Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-157">After you verify that test failover is working as expected, you can run unplanned failovers to Azure as required.</span></span> <span data-ttu-id="245d8-158">Plánované převzetí služeb není podporované.</span><span class="sxs-lookup"><span data-stu-id="245d8-158">Planned failover isn't supported.</span></span>
2. <span data-ttu-id="245d8-159">Můžete převzít služby při selhání jednoho počítače nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) pro převzetí služeb při selhání více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="245d8-159">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md), to fail over multiple VMs.</span></span>
3. <span data-ttu-id="245d8-160">V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="245d8-160">When you run a failover, replica VMs are created in Azure.</span></span> <span data-ttu-id="245d8-161">Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-161">You commit a failover to start accessing the workload from the replica Azure VM.</span></span>
4. <span data-ttu-id="245d8-162">Až bude vaše místní lokalita opět dostupná, můžete službu navrátit.</span><span class="sxs-lookup"><span data-stu-id="245d8-162">When your primary on-premises site is available again, you can fail back.</span></span> <span data-ttu-id="245d8-163">Nastavíte infrastrukturu navrácení služeb po obnovení, zahájíte replikaci počítače ze sekundární lokality na primární a spustíte neplánované převzetí služeb při selhání ze sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="245d8-163">You set up a failback infrastructure, start replicating the machine from the secondary site to the primary, and run an unplanned failover from the secondary site.</span></span> <span data-ttu-id="245d8-164">Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-164">After you commit this failover, data will be back on-premises, and you need to enable replication to Azure again.</span></span> [<span data-ttu-id="245d8-165">Další informace</span><span class="sxs-lookup"><span data-stu-id="245d8-165">Learn more</span></span>](site-recovery-failback-azure-to-vmware.md)

<span data-ttu-id="245d8-166">Pro navrácení služby po obnovení existuje několik požadavků:</span><span class="sxs-lookup"><span data-stu-id="245d8-166">There are a few failback requirements:</span></span>


- <span data-ttu-id="245d8-167">**Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server.</span><span class="sxs-lookup"><span data-stu-id="245d8-167">**Temporary process server in Azure**: If you want to fail back from Azure after failover you'll need to set up an Azure VM configured as a process server, to handle replication from Azure.</span></span> <span data-ttu-id="245d8-168">Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.</span><span class="sxs-lookup"><span data-stu-id="245d8-168">You can delete this VM after failback finishes.</span></span>
- <span data-ttu-id="245d8-169">**Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.</span><span class="sxs-lookup"><span data-stu-id="245d8-169">**VPN connection**: For failback you'll need a VPN connection (or Azure ExpressRoute) set up from the Azure network to the on-premises site.</span></span>
- <span data-ttu-id="245d8-170">**Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="245d8-170">**Separate on-premises master target server**: The on-premises master target server handles failback.</span></span> <span data-ttu-id="245d8-171">Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="245d8-171">The master target server is installed by default on the management server, but if you're failing back larger volumes of traffic you should set up a separate on-premises master target server for this purpose.</span></span>
- <span data-ttu-id="245d8-172">**Zásady navrácení služeb po obnovení:** Pro zpětnou replikaci do vaší místní lokality budete potřebovat zásady navrácení služeb.</span><span class="sxs-lookup"><span data-stu-id="245d8-172">**Failback policy**: To replicate back to your on-premises site, you need a failback policy.</span></span> <span data-ttu-id="245d8-173">Ty se vytvoří automaticky při vytvoření zásad replikace.</span><span class="sxs-lookup"><span data-stu-id="245d8-173">This is automatically created when you created your replication policy.</span></span>
- <span data-ttu-id="245d8-174">**Infrastruktura VMware:** Musíte navrátit služby po obnovení do místního virtuálního počítače VMware.</span><span class="sxs-lookup"><span data-stu-id="245d8-174">**VMware infrastructure**: You must fail back to an on-premises VMware VM.</span></span> <span data-ttu-id="245d8-175">To znamená, že potřebujete místní infrastrukturu VMware, i když místní fyzické servery replikujete do Azure.</span><span class="sxs-lookup"><span data-stu-id="245d8-175">This means you need an on-premises VMware infrastructure, even if you're replicating on-premises physical servers to Azure.</span></span>

<span data-ttu-id="245d8-176">**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**</span><span class="sxs-lookup"><span data-stu-id="245d8-176">**Figure 3: VMware/physical failback**</span></span>

![Navrácení služeb po obnovení](./media/vmware-walkthrough-architecture/enhanced-failback.png)


## <a name="next-steps"></a><span data-ttu-id="245d8-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="245d8-178">Next steps</span></span>

<span data-ttu-id="245d8-179">Přejděte na [krok 2: ověření předpoklady a omezení](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="245d8-179">Go to [Step 2: Verify prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>