---
title: "tooa replikace aaaEnable technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak tooenable replikace pro virtuální počítače Hyper-V replikuje tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a><span data-ttu-id="74bb3-103">Krok 9: Povolení sekundární lokality tooa replikace pro virtuální počítače Hyper-V</span><span class="sxs-lookup"><span data-stu-id="74bb3-103">Step 9: Enable replication tooa secondary site for Hyper-V VMs</span></span>


<span data-ttu-id="74bb3-104">Po nastavení zásad replikace, tento článek tooenable replikace tooa sekundární System Center Virtual Machine Manager (VMM) web použít pro místní technologie Hyper-V virtuální počítače (VM), pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="74bb3-104">After setting up a replication policy, use this article tooenable replication tooa secondary System Center Virtual Machine Manager (VMM) site for on-premises Hyper-V virtual machines (VM), using [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="74bb3-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="74bb3-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="select-vms-tooreplicate"></a><span data-ttu-id="74bb3-106">Vyberte tooreplicate virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="74bb3-106">Select VMs tooreplicate</span></span>

1. <span data-ttu-id="74bb3-107">Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="74bb3-107">Click **Step 2: Replicate application** > **Source**.</span></span> 

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. <span data-ttu-id="74bb3-109">V **zdroj**, vyberte hello VMM server a hello cloudu, ve které hello hostitelů Hyper-V, které chcete tooreplicate nacházejí.</span><span class="sxs-lookup"><span data-stu-id="74bb3-109">In **Source**, select hello VMM server, and hello cloud in which hello Hyper-V hosts you want tooreplicate are located.</span></span> <span data-ttu-id="74bb3-110">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="74bb3-110">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. <span data-ttu-id="74bb3-112">V **cíl**, ověřte hello sekundární server VMM a cloud.</span><span class="sxs-lookup"><span data-stu-id="74bb3-112">In **Target**, verify hello secondary VMM server and cloud.</span></span>
4. <span data-ttu-id="74bb3-113">V **virtuální počítače**, vyberte virtuální počítače hello chcete tooprotect hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="74bb3-113">In **Virtual machines**, select hello VMs you want tooprotect from hello list.</span></span>

    ![Povolení ochrany virtuálního počítače](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

<span data-ttu-id="74bb3-115">Můžete sledovat průběh hello **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="74bb3-115">You can track progress of hello **Enable Protection** action in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="74bb3-116">Po hello **dokončení ochrany** dokončení úlohy, hello počáteční replikace je dokončena a hello virtuálního počítače je připravený k převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="74bb3-116">After hello **Finalize Protection** job completes, hello initial replication is complete, and hello virtual machine is ready for failover.</span></span>

<span data-ttu-id="74bb3-117">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="74bb3-117">Note that:</span></span>

- <span data-ttu-id="74bb3-118">Můžete také povolit ochranu pro virtuální počítače v konzole VMM hello.</span><span class="sxs-lookup"><span data-stu-id="74bb3-118">You can also enable protection for virtual machines in hello VMM console.</span></span> <span data-ttu-id="74bb3-119">Klikněte na tlačítko **povolení ochrany** na panelu nástrojů hello ve vlastnostech virtuálního počítače hello > **Azure Site Recovery** kartě.</span><span class="sxs-lookup"><span data-stu-id="74bb3-119">Click **Enable Protection** on hello toolbar in hello virtual machine properties > **Azure Site Recovery** tab.</span></span>
- <span data-ttu-id="74bb3-120">Po povolení replikace, můžete zobrazit vlastnosti pro hello virtuálních počítačů v **replikované položky**.</span><span class="sxs-lookup"><span data-stu-id="74bb3-120">After you've enabled replication, you can view properties for hello VM in **Replicated Items**.</span></span> <span data-ttu-id="74bb3-121">Na hello **Essentials** řídicího panelu, zobrazí se informace o zásadách replikace hello hello virtuálního počítače a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="74bb3-121">On hello **Essentials** dashboard, you can see information about hello replication policy for hello VM and its status.</span></span> <span data-ttu-id="74bb3-122">Klikněte na tlačítko **vlastnosti** další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="74bb3-122">Click **Properties** for more details.</span></span>

## <a name="onboard-existing-vms"></a><span data-ttu-id="74bb3-123">Zařadit stávající virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="74bb3-123">Onboard existing VMs</span></span>

<span data-ttu-id="74bb3-124">Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="74bb3-124">If you have existing virtual machines in VMM that are replicating with Hyper-V Replica, you can onboard them for Azure Site Recovery replication as follows:</span></span>

1. <span data-ttu-id="74bb3-125">Zkontrolujte, zda server hello technologie Hyper-V, který hostuje hello existující virtuální počítač se nachází v primárním cloudu hello a tento server technologie Hyper-V hello hostování virtuální počítač repliky hello je umístěný v cloudu sekundární hello.</span><span class="sxs-lookup"><span data-stu-id="74bb3-125">Ensure that hello Hyper-V server hosting hello existing VM is located in hello primary cloud, and that hello Hyper-V server hosting hello replica virtual machine is located in hello secondary cloud.</span></span>
2. <span data-ttu-id="74bb3-126">Zajistěte, aby že zásady replikace je nakonfigurován pro hello primárního cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="74bb3-126">Make sure a replication policy is configured for hello primary VMM cloud.</span></span>
3. <span data-ttu-id="74bb3-127">Povolení replikace pro primární virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="74bb3-127">Enable replication for hello primary virtual machine.</span></span> <span data-ttu-id="74bb3-128">Azure Site Recovery a VMM zkontrolujte, zda hello stejného hostitele repliky a virtuální počítač je zjištěna a bude používat Azure Site Recovery a obnovit replikaci s využitím hello zadané nastavení.</span><span class="sxs-lookup"><span data-stu-id="74bb3-128">Azure Site Recovery and VMM ensure that hello same replica host and virtual machine is detected, and Azure Site Recovery will reuse and reestablish replication using hello specified settings.</span></span>


## <a name="next-steps"></a><span data-ttu-id="74bb3-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74bb3-129">Next steps</span></span>

<span data-ttu-id="74bb3-130">Přejděte příliš[krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="74bb3-130">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
