---
title: "Architektura hello aaaReview tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Tento článek obsahuje přehled o architektuře hello pro replikaci místní virtuální počítače Hyper-V tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
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
ms.openlocfilehash: 0de4b4e8601116c73e6fd710597ce4e561884368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="5a017-103">Krok 1: Posouzení hello Architektura technologie Hyper-V replikace tooa sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="5a017-103">Step 1: Review hello architecture for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="5a017-104">Tento článek popisuje hello součásti a procesů při replikaci místní virtuální počítače Hyper-V (VM) v cloudech System Center Virtual Machine Manager (VMM), sekundární lokalita VMM tooa pomocí hello [Azure Site Recovery](site-recovery-overview.md)služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5a017-104">This article describes hello components and processes involved when replicating on-premises Hyper-V virtual machines (VMs) in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM site using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="5a017-105">Odeslat všechny komentáře dole hello v tomto článku, nebo v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5a017-105">Post any comments at hello bottom of this article, or in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="5a017-106">Komponenty architektury</span><span class="sxs-lookup"><span data-stu-id="5a017-106">Architectural components</span></span>

<span data-ttu-id="5a017-107">Zde je, co potřebujete pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM.</span><span class="sxs-lookup"><span data-stu-id="5a017-107">Here's what you need for replicating Hyper-V VMs tooa secondary VMM site.</span></span>

<span data-ttu-id="5a017-108">**Komponenta**</span><span class="sxs-lookup"><span data-stu-id="5a017-108">**Component**</span></span> | <span data-ttu-id="5a017-109">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="5a017-109">**Location**</span></span> | <span data-ttu-id="5a017-110">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="5a017-110">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="5a017-111">**Azure**</span><span class="sxs-lookup"><span data-stu-id="5a017-111">**Azure**</span></span> | <span data-ttu-id="5a017-112">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5a017-112">Subscription in Azure.</span></span> | <span data-ttu-id="5a017-113">Vytvoření trezoru služeb zotavení v hello předplatné Azure, tooorchestrate a spravovat replikace mezi umístěními VMM.</span><span class="sxs-lookup"><span data-stu-id="5a017-113">You create a Recovery Services vault in hello Azure subscription, tooorchestrate and manage replication between VMM locations.</span></span>
<span data-ttu-id="5a017-114">**Server VMM**</span><span class="sxs-lookup"><span data-stu-id="5a017-114">**VMM server**</span></span> | <span data-ttu-id="5a017-115">Potřebujete primární a sekundární umístění VMM.</span><span class="sxs-lookup"><span data-stu-id="5a017-115">You need a VMM primary and secondary location.</span></span> | <span data-ttu-id="5a017-116">Doporučujeme, abyste server VMM v primární lokalitě hello a po jednom v sekundární lokalitě hello</span><span class="sxs-lookup"><span data-stu-id="5a017-116">We recommend a VMM server in hello primary site, and one in hello secondary site</span></span> 
<span data-ttu-id="5a017-117">**Server Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="5a017-117">**Hyper-V server**</span></span> |  <span data-ttu-id="5a017-118">Jeden nebo více servery Hyper-V host v hello primárních a sekundárních cloudech VMM.</span><span class="sxs-lookup"><span data-stu-id="5a017-118">One or more Hyper-V host servers in hello primary and secondary VMM clouds.</span></span> | <span data-ttu-id="5a017-119">Data se replikují mezi hello primární a sekundární servery Hyper-V hostiteli prostřednictvím hello LAN nebo VPN pomocí protokolu Kerberos nebo ověření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5a017-119">Data is replicated between hello primary and secondary Hyper-V host servers over hello LAN or VPN, using Kerberos or certificate authentication.</span></span>  
<span data-ttu-id="5a017-120">**Virtuální počítače Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="5a017-120">**Hyper-V VMs**</span></span> | <span data-ttu-id="5a017-121">Na hostitelském serveru Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5a017-121">On Hyper-V host server.</span></span> | <span data-ttu-id="5a017-122">Hello zdrojový hostitelský server by měl mít aspoň jeden virtuální počítač, který má tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="5a017-122">hello source host server should have at least one VM that you want tooreplicate.</span></span>

## <a name="replication-process"></a><span data-ttu-id="5a017-123">Proces replikace</span><span class="sxs-lookup"><span data-stu-id="5a017-123">Replication process</span></span>

1. <span data-ttu-id="5a017-124">Nastavit hello účet Azure, vytvořte trezor služeb zotavení a určit, co chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="5a017-124">You set up hello Azure account, create a Recovery Services vault, and specify what you want tooreplicate.</span></span>
2. <span data-ttu-id="5a017-125">Nakonfigurujete hello zdrojové a cílové nastavení replikace, které zahrnuje instalaci hello zprostředkovatele Azure Site Recovery na servery VMM a agenta služeb zotavení Microsoft Azure hello na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5a017-125">You configure hello source and target replication settings, which includes installing hello Azure Site Recovery Provider on VMM servers, and hello Microsoft Azure Recovery Services agent on each Hyper-V host.</span></span>
3. <span data-ttu-id="5a017-126">Můžete vytvořit zásadu replikace pro zdroj hello cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="5a017-126">You create a replication policy for hello source VMM cloud.</span></span> <span data-ttu-id="5a017-127">zásady Hello je použité tooall virtuální počítače umístěné na hostitelích v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5a017-127">hello policy is applied tooall VMs located on hosts in hello cloud.</span></span>
4. <span data-ttu-id="5a017-128">Povolení replikace pro každé VMM, a proběhne počáteční replikace virtuálního počítače, v souladu s nastavením text hello, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="5a017-128">You enable replication for each VMM, and initial replication of a VM occurs in accordance with hello settings you choose.</span></span>
5. <span data-ttu-id="5a017-129">Po počáteční replikaci se zahájí replikace rozdílových změn.</span><span class="sxs-lookup"><span data-stu-id="5a017-129">After initial replication, replication of delta changes begins.</span></span> <span data-ttu-id="5a017-130">Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.</span><span class="sxs-lookup"><span data-stu-id="5a017-130">Tracked changes for an item are held in a .hrl file.</span></span>


![Místní tooon místní](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a><span data-ttu-id="5a017-132">Proces převzetí služeb při selhání a navrácení služeb po obnovení</span><span class="sxs-lookup"><span data-stu-id="5a017-132">Failover and failback process</span></span>

1. <span data-ttu-id="5a017-133">Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami.</span><span class="sxs-lookup"><span data-stu-id="5a017-133">You can run a planned or unplanned [failover](site-recovery-failover.md) between on-premises sites.</span></span> <span data-ttu-id="5a017-134">Pokud spustíte plánované převzetí služeb při selhání, pak zdrojové virtuální počítače jsou vypnout tooensure nedošlo ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="5a017-134">If you run a planned failover, then source VMs are shut down tooensure no data loss.</span></span>
2. <span data-ttu-id="5a017-135">Můžete převzít jeden počítač nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) tooorchestrate převzetí služeb při selhání více počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a017-135">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) tooorchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="5a017-136">Pokud provádíte tooa sekundární lokality neplánované převzetí služeb při selhání, po převzetí služeb při selhání počítače hello v hello sekundárního umístění nejsou povolené pro ochranu nebo replikace.</span><span class="sxs-lookup"><span data-stu-id="5a017-136">If you perform an unplanned failover tooa secondary site, after hello failover machines in hello secondary location aren't enabled for protection or replication.</span></span> <span data-ttu-id="5a017-137">Pokud jste spustili plánované převzetí služeb při selhání, po převzetí služeb při selhání hello, jsou chráněné počítače v hello sekundárního umístění.</span><span class="sxs-lookup"><span data-stu-id="5a017-137">If you ran a planned failover, after hello failover, machines in hello secondary location are protected.</span></span>
5. <span data-ttu-id="5a017-138">Poté potvrďte hello převzetí služeb při selhání toostart přístupem hello zatížení z hello repliky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5a017-138">Then, you commit hello failover toostart accessing hello workload from hello replica VM.</span></span>
6. <span data-ttu-id="5a017-139">Při primární lokality je opět k dispozici, můžete zahájit tooreplicate zpětná replikace z primární toohello sekundární lokality hello.</span><span class="sxs-lookup"><span data-stu-id="5a017-139">When your primary site is available again, you initiate reverse replication tooreplicate from hello secondary site toohello primary.</span></span> <span data-ttu-id="5a017-140">Zpětná replikace přináší hello virtuální počítače v chráněném stavu, ale hello sekundárního datového centra je stále aktivní umístění hello.</span><span class="sxs-lookup"><span data-stu-id="5a017-140">Reverse replication brings hello virtual machines into a protected state, but hello secondary datacenter is still hello active location.</span></span>
7. <span data-ttu-id="5a017-141">toomake hello primární lokalitu do umístění služby active hello znovu spustíte plánované převzetí služeb při selhání z sekundární tooprimary, za nímž následuje jiný zpětné replikace.</span><span class="sxs-lookup"><span data-stu-id="5a017-141">toomake hello primary site into hello active location again, you initiate a planned failover from secondary tooprimary, followed by another reverse replication.</span></span>



## <a name="next-steps"></a><span data-ttu-id="5a017-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5a017-142">Next steps</span></span>

<span data-ttu-id="5a017-143">Přejděte příliš[krok 2: Přečtěte si hello předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="5a017-143">Go too[Step 2: Review hello prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>
