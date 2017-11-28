---
title: "aaaRun testovací převzetí služeb při selhání pro virtuální počítač Hyper-V replikace tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toorun testovací převzetí služeb při selhání pro virtuální počítač Hyper-V replikace tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: a60411541c2db001c573735bcb0d651f6618f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="75292-103">Krok 10: Spuštění testovací převzetí služeb při selhání pro sekundární lokalitě tooa replikace Hyper-V</span><span class="sxs-lookup"><span data-stu-id="75292-103">Step 10: Run a test failover for Hyper-V replication tooa secondary site</span></span>


<span data-ttu-id="75292-104">Po povolení replikace pro virtuální počítače Hyper-V (VM) s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek toorun testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="75292-104">After you enable replication for Hyper-V virtual machines (VMs) with [Azure Site Recovery](site-recovery-overview.md), use this article toorun a test failover.</span></span> <span data-ttu-id="75292-105">Testovací převzetí služeb ověřuje, že replikace pracuje bez dopadu na produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="75292-105">A test failover verifies that replication is working, without impacting your production environment.</span></span> 


<span data-ttu-id="75292-106">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="75292-106">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="75292-107">Než začnete</span><span class="sxs-lookup"><span data-stu-id="75292-107">Before you start</span></span>

- <span data-ttu-id="75292-108">Když se aktivuje testovací převzetí služeb, můžete zadat hello sítě toowhich hello testovací repliku, které se připojí virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="75292-108">When you're triggering a test failover, you can specify hello network toowhich hello test replica VMs will connect.</span></span> <span data-ttu-id="75292-109">[Další informace](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) o možnostech sítě hello.</span><span class="sxs-lookup"><span data-stu-id="75292-109">[Learn more](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) about hello network options.</span></span>
- <span data-ttu-id="75292-110">Doporučujeme, abyste si zvolili síť, kterou jste vybrali při mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="75292-110">We recommend that you don't choose a network that you selected during network mapping.</span></span>
- <span data-ttu-id="75292-111">Hello pokyny v tomto článku popisují, jak toofail přes jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="75292-111">hello instructions in this article describe how toofail over a single VM.</span></span> <span data-ttu-id="75292-112">Přečtěte si informace o [vytváření plánů obnovení](site-recovery-create-recovery-plans.md) Chcete-li toofail přes víc virtuálních počítačů současně.</span><span class="sxs-lookup"><span data-stu-id="75292-112">Read about [creating recovery plans](site-recovery-create-recovery-plans.md) if you want toofail over multiple VMs together.</span></span>
- <span data-ttu-id="75292-113">Přečtěte si informace o [omezení pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span><span class="sxs-lookup"><span data-stu-id="75292-113">Read about [limitations for test failovers](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span></span>
- <span data-ttu-id="75292-114">Hello IP adresa poskytnutý tooa virtuálních počítačů během testovacího převzetí služeb při selhání je hello stejnou IP adresu, která hello virtuálních počítačů by přijímat plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že hello IP adresa je k dispozici v hello testovací převzetí služeb při selhání sítě).</span><span class="sxs-lookup"><span data-stu-id="75292-114">hello IP address given tooa VM during test failover is hello same IP address that hello VM would receive for a planned or unplanned failover (presuming that hello IP address is available in hello test failover network).</span></span> <span data-ttu-id="75292-115">Pokud hello IP adresa není k dispozici v hello testovací převzetí služeb při selhání sítě, obdrží hello virtuálních počítačů jinou IP adresu, která je k dispozici v hello testovací převzetí služeb při selhání sítě.</span><span class="sxs-lookup"><span data-stu-id="75292-115">If hello IP address isn't available in hello test failover network, hello VM receives another IP address that is available in hello test failover network.</span></span>
- <span data-ttu-id="75292-116">Pokud chcete, toodo testovací převzetí služeb v produkční sítě, pro úplné ověření počítače připojení k síti začátku do konce, Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="75292-116">If you do want toodo a test failover in your production network, for full validatation of end-to-end network connectivity machine, note that:</span></span>
    - <span data-ttu-id="75292-117">Dobrý den, by měl být primární virtuální počítač vypnut, když provádíte hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-117">hello primary VM should be shut down when you're doing hello test failover.</span></span> <span data-ttu-id="75292-118">Jinak, dva virtuální počítače s hello stejnou identitu bude spuštěna v hello stejné sítě v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="75292-118">Otherwise, two VMs with hello same identity will be running in hello same network at hello same time.</span></span> 
    - <span data-ttu-id="75292-119">Pokud provedete změny tootest virtuálních počítačů, tyto změny budou ztraceny při čištění hello testování převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-119">If you make changes tootest VMs, those changes are lost when you clean up hello test failover.</span></span> <span data-ttu-id="75292-120">Tyto změny nejsou replikované back toohello primárního virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="75292-120">These changes aren't replicated back toohello primary VM.</span></span>
    - <span data-ttu-id="75292-121">Testování produkční síť vede toodowntime pro produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="75292-121">Testing a a production network leads toodowntime for your production workloads.</span></span> <span data-ttu-id="75292-122">Požádejte uživatele není toouse hello aplikace, když probíhá hello postupu zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="75292-122">Ask users not toouse hello app when hello disaster recovery drill is in progress.</span></span>  


## <a name="run-a-test-failover-for-a-vm"></a><span data-ttu-id="75292-123">Spustit testovací převzetí služeb pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="75292-123">Run a test failover for a VM</span></span>

1. <span data-ttu-id="75292-124">toofail přes jeden virtuální počítač, v **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **testovací převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="75292-124">toofail over a single VM, in **Replicated Items**, click hello VM > **Test Failover**.</span></span>
2. <span data-ttu-id="75292-125">V **testovací převzetí služeb při selhání**, zadejte, jak testovací virtuální počítače budou připojené toonetworks po hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-125">In **Test Failover**, specify how test VMs will be connected toonetworks after hello test failover.</span></span> 
3. <span data-ttu-id="75292-126">Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-126">Click **OK** toobegin hello failover.</span></span> <span data-ttu-id="75292-127">Průběh sledovat na hello **úlohy** kartě.</span><span class="sxs-lookup"><span data-stu-id="75292-127">Track progress on hello **Jobs** tab.</span></span>
5. <span data-ttu-id="75292-128">Po dokončení převzetí služeb při selhání ověřte úspěšně testu hello spuštění virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="75292-128">After failover is complete, verify that hello test VMs start successfully.</span></span>
6. <span data-ttu-id="75292-129">Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="75292-129">When you're done, click **Cleanup test failover** on hello recovery plan.</span></span>
7. <span data-ttu-id="75292-130">V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-130">In **Notes**, record and save any observations associated with hello test failover.</span></span> <span data-ttu-id="75292-131">Tento krok odstraní hello virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="75292-131">This step deletes hello virtual machines and networks that were created during test failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="75292-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75292-132">Next steps</span></span>

<span data-ttu-id="75292-133">Po nasazení hello otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="75292-133">After you've tested hello deployment, learn more about other types of [failover](site-recovery-failover.md).</span></span>
