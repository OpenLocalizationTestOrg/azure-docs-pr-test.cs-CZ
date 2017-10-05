---
title: "Povolit replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak povolit replikaci pro virtuální počítače Hyper-V replikuje do sekundární lokality System Center VMM s Azure Site Recovery."
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
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a><span data-ttu-id="a2268-103">Krok 9: Povolení replikace do sekundární lokality pro virtuální počítače Hyper-V</span><span class="sxs-lookup"><span data-stu-id="a2268-103">Step 9: Enable replication to a secondary site for Hyper-V VMs</span></span>


<span data-ttu-id="a2268-104">Po nastavení zásad replikace, použijte tento článek k povolení replikace do sekundární lokality System Center Virtual Machine Manager (VMM) pro místní technologie Hyper-V virtuální počítače (VM), pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a2268-104">After setting up a replication policy, use this article to enable replication to a secondary System Center Virtual Machine Manager (VMM) site for on-premises Hyper-V virtual machines (VM), using [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="a2268-105">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a2268-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="select-vms-to-replicate"></a><span data-ttu-id="a2268-106">Vyberte virtuální počítače k replikaci</span><span class="sxs-lookup"><span data-stu-id="a2268-106">Select VMs to replicate</span></span>

1. <span data-ttu-id="a2268-107">Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="a2268-107">Click **Step 2: Replicate application** > **Source**.</span></span> 

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. <span data-ttu-id="a2268-109">V **zdroj**, vyberte VMM server a cloudu, ve které se nacházejí hostitelů Hyper-V, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="a2268-109">In **Source**, select the VMM server, and the cloud in which the Hyper-V hosts you want to replicate are located.</span></span> <span data-ttu-id="a2268-110">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2268-110">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. <span data-ttu-id="a2268-112">V **cíl**, ověřte sekundární server VMM a cloud.</span><span class="sxs-lookup"><span data-stu-id="a2268-112">In **Target**, verify the secondary VMM server and cloud.</span></span>
4. <span data-ttu-id="a2268-113">V **virtuální počítače**, vyberte virtuální počítače, které chcete chránit ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="a2268-113">In **Virtual machines**, select the VMs you want to protect from the list.</span></span>

    ![Povolení ochrany virtuálního počítače](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

<span data-ttu-id="a2268-115">Můžete sledovat průběh **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="a2268-115">You can track progress of the **Enable Protection** action in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="a2268-116">Po **dokončení ochrany** úloha dokončena počáteční replikace je dokončena a virtuální počítač je připraven k převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="a2268-116">After the **Finalize Protection** job completes, the initial replication is complete, and the virtual machine is ready for failover.</span></span>

<span data-ttu-id="a2268-117">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="a2268-117">Note that:</span></span>

- <span data-ttu-id="a2268-118">Můžete také povolit ochranu pro virtuální počítače v konzole nástroje VMM.</span><span class="sxs-lookup"><span data-stu-id="a2268-118">You can also enable protection for virtual machines in the VMM console.</span></span> <span data-ttu-id="a2268-119">Klikněte na tlačítko **povolení ochrany** na panelu nástrojů ve vlastnostech virtuálního počítače > **Azure Site Recovery** kartě.</span><span class="sxs-lookup"><span data-stu-id="a2268-119">Click **Enable Protection** on the toolbar in the virtual machine properties > **Azure Site Recovery** tab.</span></span>
- <span data-ttu-id="a2268-120">Po povolení replikace, můžete zobrazit vlastnosti pro virtuální počítač v **replikované položky**.</span><span class="sxs-lookup"><span data-stu-id="a2268-120">After you've enabled replication, you can view properties for the VM in **Replicated Items**.</span></span> <span data-ttu-id="a2268-121">Na **Essentials** řídicí panel, se zobrazí informace o zásadách replikace pro virtuální počítač a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="a2268-121">On the **Essentials** dashboard, you can see information about the replication policy for the VM and its status.</span></span> <span data-ttu-id="a2268-122">Klikněte na tlačítko **vlastnosti** další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a2268-122">Click **Properties** for more details.</span></span>

## <a name="onboard-existing-vms"></a><span data-ttu-id="a2268-123">Zařadit stávající virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="a2268-123">Onboard existing VMs</span></span>

<span data-ttu-id="a2268-124">Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a2268-124">If you have existing virtual machines in VMM that are replicating with Hyper-V Replica, you can onboard them for Azure Site Recovery replication as follows:</span></span>

1. <span data-ttu-id="a2268-125">Ujistěte se, že serveru technologie Hyper-V, který hostuje stávající virtuální počítač nachází v primárním cloudu, a že serveru technologie Hyper-V, který hostuje virtuální počítač repliky se nachází v sekundární cloudu.</span><span class="sxs-lookup"><span data-stu-id="a2268-125">Ensure that the Hyper-V server hosting the existing VM is located in the primary cloud, and that the Hyper-V server hosting the replica virtual machine is located in the secondary cloud.</span></span>
2. <span data-ttu-id="a2268-126">Ujistěte se, že zásady replikace je nakonfigurován pro primárního cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="a2268-126">Make sure a replication policy is configured for the primary VMM cloud.</span></span>
3. <span data-ttu-id="a2268-127">Povolení replikace pro primární virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2268-127">Enable replication for the primary virtual machine.</span></span> <span data-ttu-id="a2268-128">Azure Site Recovery a VMM zkontrolujte, zda je zjištěna stejného hostitele repliky a virtuální počítač a Azure Site Recovery bude opakovaně používat a opětovnému zavedení replikace použitím zadaného nastavení.</span><span class="sxs-lookup"><span data-stu-id="a2268-128">Azure Site Recovery and VMM ensure that the same replica host and virtual machine is detected, and Azure Site Recovery will reuse and reestablish replication using the specified settings.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a2268-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2268-129">Next steps</span></span>

<span data-ttu-id="a2268-130">Přejděte na [krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="a2268-130">Go to [Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
