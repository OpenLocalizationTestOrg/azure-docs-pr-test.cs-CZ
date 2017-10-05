---
title: "Spustit testovací převzetí služeb pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak spustit testovací převzetí služeb pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality System Center VMM s Azure Site Recovery."
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
ms.openlocfilehash: 23d235d326273e7ec59feee6588a39f685401e52
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="6006a-103">Krok 10: Spuštění testovací převzetí služeb při selhání pro replikaci technologie Hyper-V do sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="6006a-103">Step 10: Run a test failover for Hyper-V replication to a secondary site</span></span>


<span data-ttu-id="6006a-104">Po povolení replikace pro virtuální počítače Hyper-V (VM) s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek spustit testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="6006a-104">After you enable replication for Hyper-V virtual machines (VMs) with [Azure Site Recovery](site-recovery-overview.md), use this article to run a test failover.</span></span> <span data-ttu-id="6006a-105">Testovací převzetí služeb ověřuje, že replikace pracuje bez dopadu na produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="6006a-105">A test failover verifies that replication is working, without impacting your production environment.</span></span> 


<span data-ttu-id="6006a-106">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6006a-106">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="6006a-107">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6006a-107">Before you start</span></span>

- <span data-ttu-id="6006a-108">Když se aktivuje testovací převzetí služeb, můžete v síti, ke kterému se připojí testovací repliku virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6006a-108">When you're triggering a test failover, you can specify the network to which the test replica VMs will connect.</span></span> <span data-ttu-id="6006a-109">[Další informace](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) o možnostech sítě.</span><span class="sxs-lookup"><span data-stu-id="6006a-109">[Learn more](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) about the network options.</span></span>
- <span data-ttu-id="6006a-110">Doporučujeme, abyste si zvolili síť, kterou jste vybrali při mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="6006a-110">We recommend that you don't choose a network that you selected during network mapping.</span></span>
- <span data-ttu-id="6006a-111">Podle pokynů v tomto článku popisují postup převzít jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6006a-111">The instructions in this article describe how to fail over a single VM.</span></span> <span data-ttu-id="6006a-112">Přečtěte si informace o [vytváření plánů obnovení](site-recovery-create-recovery-plans.md) Pokud chcete převzetí služeb při selhání víc virtuálních počítačů současně.</span><span class="sxs-lookup"><span data-stu-id="6006a-112">Read about [creating recovery plans](site-recovery-create-recovery-plans.md) if you want to fail over multiple VMs together.</span></span>
- <span data-ttu-id="6006a-113">Přečtěte si informace o [omezení pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span><span class="sxs-lookup"><span data-stu-id="6006a-113">Read about [limitations for test failovers](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).</span></span>
- <span data-ttu-id="6006a-114">IP adresa, zadaný pro virtuální počítač během testovacího převzetí služeb při selhání je stejnou IP adresu, která bude přijímat virtuálního počítače plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že IP adresa je k dispozici v testovací síti převzetí služeb při selhání).</span><span class="sxs-lookup"><span data-stu-id="6006a-114">The IP address given to a VM during test failover is the same IP address that the VM would receive for a planned or unplanned failover (presuming that the IP address is available in the test failover network).</span></span> <span data-ttu-id="6006a-115">Pokud IP adresa není k dispozici v testovací síti převzetí služeb při selhání, virtuální počítač přijímá jinou IP adresu, která je k dispozici v testovací síti převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6006a-115">If the IP address isn't available in the test failover network, the VM receives another IP address that is available in the test failover network.</span></span>
- <span data-ttu-id="6006a-116">Pokud chcete provést převzetí služeb při selhání v produkční sítě, pro úplné ověření počítače připojení k síti začátku do konce, Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="6006a-116">If you do want to do a test failover in your production network, for full validatation of end-to-end network connectivity machine, note that:</span></span>
    - <span data-ttu-id="6006a-117">Primární virtuální počítač by měl vypnout, pokud provádíte testu převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="6006a-117">The primary VM should be shut down when you're doing the test failover.</span></span> <span data-ttu-id="6006a-118">Dva virtuální počítače se stejnou identitou bude spuštěna ve stejné síti, v opačném ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6006a-118">Otherwise, two VMs with the same identity will be running in the same network at the same time.</span></span> 
    - <span data-ttu-id="6006a-119">Pokud provedete změny k testování virtuálních počítačů, tyto změny budou ztraceny, když vyčistit testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="6006a-119">If you make changes to test VMs, those changes are lost when you clean up the test failover.</span></span> <span data-ttu-id="6006a-120">Tyto změny nejsou replikovat zpět na primární virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6006a-120">These changes aren't replicated back to the primary VM.</span></span>
    - <span data-ttu-id="6006a-121">Testování produkční síť vede k výpadku pro produkční zatížení.</span><span class="sxs-lookup"><span data-stu-id="6006a-121">Testing a a production network leads to downtime for your production workloads.</span></span> <span data-ttu-id="6006a-122">Požádejte uživatele není pro použití aplikace v průběhu postupu zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="6006a-122">Ask users not to use the app when the disaster recovery drill is in progress.</span></span>  


## <a name="run-a-test-failover-for-a-vm"></a><span data-ttu-id="6006a-123">Spustit testovací převzetí služeb pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="6006a-123">Run a test failover for a VM</span></span>

1. <span data-ttu-id="6006a-124">K převzetí služeb při selhání jeden virtuální počítač, v **replikované položky**, klikněte na virtuální počítač > **testovací převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="6006a-124">To fail over a single VM, in **Replicated Items**, click the VM > **Test Failover**.</span></span>
2. <span data-ttu-id="6006a-125">V **testovací převzetí služeb při selhání**, zadejte, jak se připojí testovací virtuální počítače k sítím po převzetí služeb při selhání test.</span><span class="sxs-lookup"><span data-stu-id="6006a-125">In **Test Failover**, specify how test VMs will be connected to networks after the test failover.</span></span> 
3. <span data-ttu-id="6006a-126">Kliknutím na **OK** zahajte převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6006a-126">Click **OK** to begin the failover.</span></span> <span data-ttu-id="6006a-127">Sledovat průběh **úlohy** kartě.</span><span class="sxs-lookup"><span data-stu-id="6006a-127">Track progress on the **Jobs** tab.</span></span>
5. <span data-ttu-id="6006a-128">Po dokončení převzetí služeb při selhání ověřte, že testovací virtuální počítače úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="6006a-128">After failover is complete, verify that the test VMs start successfully.</span></span>
6. <span data-ttu-id="6006a-129">Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="6006a-129">When you're done, click **Cleanup test failover** on the recovery plan.</span></span>
7. <span data-ttu-id="6006a-130">V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="6006a-130">In **Notes**, record and save any observations associated with the test failover.</span></span> <span data-ttu-id="6006a-131">Tento krok odstraní virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6006a-131">This step deletes the virtual machines and networks that were created during test failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6006a-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6006a-132">Next steps</span></span>

<span data-ttu-id="6006a-133">Po nasazení otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="6006a-133">After you've tested the deployment, learn more about other types of [failover](site-recovery-failover.md).</span></span>
