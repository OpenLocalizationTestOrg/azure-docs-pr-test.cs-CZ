---
title: "Kontrola architektury pro replikaci Hyper-V do sekundární lokality s využitím služby Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled architektury pro replikaci místních virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="31493-103">Krok 1: Kontrola architektury pro replikaci Hyper-V do sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="31493-103">Step 1: Review the architecture for Hyper-V replication to a secondary site</span></span>

<span data-ttu-id="31493-104">Tento článek popisuje komponenty a procesy využívané při replikaci místních virtuálních počítačů Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do sekundární lokality VMM s využitím služby [Azure Site Recovery](site-recovery-overview.md) na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="31493-104">This article describes the components and processes involved when replicating on-premises Hyper-V virtual machines (VMs) in System Center Virtual Machine Manager (VMM) clouds, to a secondary VMM site using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="31493-105">Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="31493-105">Post any comments at the bottom of this article, or in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="31493-106">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="31493-106">Architectural components</span></span>

<span data-ttu-id="31493-107">Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů Hyper-V do sekundární lokality VMM.</span><span class="sxs-lookup"><span data-stu-id="31493-107">Here's what you need for replicating Hyper-V VMs to a secondary VMM site.</span></span>

<span data-ttu-id="31493-108">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="31493-108">**Component**</span></span> | <span data-ttu-id="31493-109">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="31493-109">**Location**</span></span> | <span data-ttu-id="31493-110">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="31493-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="31493-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="31493-111">**Azure**</span></span> | <span data-ttu-id="31493-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="31493-112">Subscription in Azure.</span></span> | <span data-ttu-id="31493-113">V předplatném Azure vytvoříte trezor služby Recovery Services sloužící k orchestraci a správě replikace mezi umístěními VMM.</span><span class="sxs-lookup"><span data-stu-id="31493-113">You create a Recovery Services vault in the Azure subscription, to orchestrate and manage replication between VMM locations.</span></span>
<span data-ttu-id="31493-114">**Server VMM**</span><span class="sxs-lookup"><span data-stu-id="31493-114">**VMM server**</span></span> | <span data-ttu-id="31493-115">Potřebujete primární a sekundární umístění VMM.</span><span class="sxs-lookup"><span data-stu-id="31493-115">You need a VMM primary and secondary location.</span></span> | <span data-ttu-id="31493-116">Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě.</span><span class="sxs-lookup"><span data-stu-id="31493-116">We recommend a VMM server in the primary site, and one in the secondary site</span></span> 
<span data-ttu-id="31493-117">**Server Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="31493-117">**Hyper-V server**</span></span> |  <span data-ttu-id="31493-118">Jeden nebo několik hostitelských serverů Hyper-V v primárním a sekundárním cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="31493-118">One or more Hyper-V host servers in the primary and secondary VMM clouds.</span></span> | <span data-ttu-id="31493-119">Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.</span><span class="sxs-lookup"><span data-stu-id="31493-119">Data is replicated between the primary and secondary Hyper-V host servers over the LAN or VPN, using Kerberos or certificate authentication.</span></span>  
<span data-ttu-id="31493-120">**Virtuální počítače Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="31493-120">**Hyper-V VMs**</span></span> | <span data-ttu-id="31493-121">Na hostitelském serveru Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="31493-121">On Hyper-V host server.</span></span> | <span data-ttu-id="31493-122">Zdrojový hostitelský server musí mít alespoň jeden virtuální počítač, který chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="31493-122">The source host server should have at least one VM that you want to replicate.</span></span>

## <a name="replication-process"></a><span data-ttu-id="31493-123">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="31493-123">Replication process</span></span>

1. <span data-ttu-id="31493-124">Nastavíte účet Azure, vytvoříte trezor služby Recovery Services a určíte, co chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="31493-124">You set up the Azure account, create a Recovery Services vault, and specify what you want to replicate.</span></span>
2. <span data-ttu-id="31493-125">Nakonfigurujete nastavení replikace zdroje a cíle, což zahrnuje instalaci zprostředkovatele služby Azure Site Recovery na serverech VMM a agenta Microsoft Azure Recovery Services na každém hostiteli Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="31493-125">You configure the source and target replication settings, which includes installing the Azure Site Recovery Provider on VMM servers, and the Microsoft Azure Recovery Services agent on each Hyper-V host.</span></span>
3. <span data-ttu-id="31493-126">Vytvoříte zásady replikace pro zdrojový cloud VMM.</span><span class="sxs-lookup"><span data-stu-id="31493-126">You create a replication policy for the source VMM cloud.</span></span> <span data-ttu-id="31493-127">Zásady se použijí pro všechny virtuální počítače na hostitelích v cloudu.</span><span class="sxs-lookup"><span data-stu-id="31493-127">The policy is applied to all VMs located on hosts in the cloud.</span></span>
4. <span data-ttu-id="31493-128">Povolíte replikaci pro každou službu VMM a dojde k počáteční replikaci virtuálního počítače podle zvoleného nastavení.</span><span class="sxs-lookup"><span data-stu-id="31493-128">You enable replication for each VMM, and initial replication of a VM occurs in accordance with the settings you choose.</span></span>
5. <span data-ttu-id="31493-129">Po počáteční replikaci se zahájí replikace rozdílových změn.</span><span class="sxs-lookup"><span data-stu-id="31493-129">After initial replication, replication of delta changes begins.</span></span> <span data-ttu-id="31493-130">Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.</span><span class="sxs-lookup"><span data-stu-id="31493-130">Tracked changes for an item are held in a .hrl file.</span></span>


![Z lokálního prostředí do lokálního prostředí](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="31493-132">Proces převzetí služeb při selhání a navrácení služeb po obnovení</span><span class="sxs-lookup"><span data-stu-id="31493-132">Failover and failback process</span></span>

1. <span data-ttu-id="31493-133">Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami.</span><span class="sxs-lookup"><span data-stu-id="31493-133">You can run a planned or unplanned [failover](site-recovery-failover.md) between on-premises sites.</span></span> <span data-ttu-id="31493-134">Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="31493-134">If you run a planned failover, then source VMs are shut down to ensure no data loss.</span></span>
2. <span data-ttu-id="31493-135">Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.</span><span class="sxs-lookup"><span data-stu-id="31493-135">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) to orchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="31493-136">Pokud proběhlo neplánované převzetí služeb při selhání sekundární lokalitou, po provedení převzetí služeb nebudou počítače v sekundární lokalitě chráněné pomocí replikace.</span><span class="sxs-lookup"><span data-stu-id="31493-136">If you perform an unplanned failover to a secondary site, after the failover machines in the secondary location aren't enabled for protection or replication.</span></span> <span data-ttu-id="31493-137">Pokud jste spustili plánované převzetí služeb při selhání, počítače v sekundárním umístění chráněné budou.</span><span class="sxs-lookup"><span data-stu-id="31493-137">If you ran a planned failover, after the failover, machines in the secondary location are protected.</span></span>
5. <span data-ttu-id="31493-138">Po potvrzení převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="31493-138">Then, you commit the failover to start accessing the workload from the replica VM.</span></span>
6. <span data-ttu-id="31493-139">Až bude primární lokalita opět dostupná, zahájíte zpětnou replikaci ze sekundární lokality do primární.</span><span class="sxs-lookup"><span data-stu-id="31493-139">When your primary site is available again, you initiate reverse replication to replicate from the secondary site to the primary.</span></span> <span data-ttu-id="31493-140">Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivním umístěním.</span><span class="sxs-lookup"><span data-stu-id="31493-140">Reverse replication brings the virtual machines into a protected state, but the secondary datacenter is still the active location.</span></span>
7. <span data-ttu-id="31493-141">Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.</span><span class="sxs-lookup"><span data-stu-id="31493-141">To make the primary site into the active location again, you initiate a planned failover from secondary to primary, followed by another reverse replication.</span></span>



## <a name="next-steps"></a><span data-ttu-id="31493-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31493-142">Next steps</span></span>

<span data-ttu-id="31493-143">Přejděte ke [kroku 2: Kontrola požadavků a omezení](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="31493-143">Go to [Step 2: Review the prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>
