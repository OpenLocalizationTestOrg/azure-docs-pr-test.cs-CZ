---
title: "Povolení replikace pro virtuální počítače VMware replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky je nutné povolit replikaci do Azure pro virtuální počítače VMware pomocí služby Azure Site Recovery"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 470b9ddd8df4a4e74ec7174f79020c252323e502
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-to-azure"></a><span data-ttu-id="12b09-103">Krok 11: Povolení replikace pro virtuální počítače VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="12b09-103">Step 11: Enable replication for VMware virtual machines to Azure</span></span>


<span data-ttu-id="12b09-104">Tento článek popisuje, jak povolit replikaci pro místní virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="12b09-104">This article describes how to enable replication for on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="12b09-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="12b09-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="12b09-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="12b09-106">Before you start</span></span>

- <span data-ttu-id="12b09-107">Musí mít virtuální počítače VMware [nainstalovat službu mobility](vmware-walkthrough-install-mobility.md).</span><span class="sxs-lookup"><span data-stu-id="12b09-107">VMware VMs must have the [Mobility service component installed](vmware-walkthrough-install-mobility.md).</span></span> <span data-ttu-id="12b09-108">– Pokud je virtuální počítač připravený na nabízenou instalaci, procesový server automaticky nainstaluje službu Mobility, když aktivujete replikaci.</span><span class="sxs-lookup"><span data-stu-id="12b09-108">- If a VM is prepared for push installation, the process server automatically installs the Mobility service when you enable replication.</span></span>
- <span data-ttu-id="12b09-109">Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálních počítačů do Azure</span><span class="sxs-lookup"><span data-stu-id="12b09-109">Your Azure user account needs specific [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of a VM to Azure</span></span>
- <span data-ttu-id="12b09-110">Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až 15 minut nebo déle pro změny vstoupily v platnost a se objevily na portálu.</span><span class="sxs-lookup"><span data-stu-id="12b09-110">When you add or modify VMs, it can take up to 15 minutes or longer for changes to take effect, and for them to appear in the portal.</span></span>
- <span data-ttu-id="12b09-111">Čas poslední zjištěné můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.</span><span class="sxs-lookup"><span data-stu-id="12b09-111">You can check the last discovered time for VMs in **Configuration Servers** > **Last Contact At**.</span></span>
- <span data-ttu-id="12b09-112">Chcete-li přidat virtuální počítače bez čekání na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="12b09-112">To add VMs without waiting for the scheduled discovery, highlight the configuration server (don’t click it), and click **Refresh**.</span></span>



## <a name="exclude-disks-from-replication"></a><span data-ttu-id="12b09-113">Vyloučení disků z replikace</span><span class="sxs-lookup"><span data-stu-id="12b09-113">Exclude disks from replication</span></span>

<span data-ttu-id="12b09-114">Ve výchozím nastavení jsou všechny disky na počítači replikují.</span><span class="sxs-lookup"><span data-stu-id="12b09-114">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="12b09-115">Disky můžete z replikace vyloučit.</span><span class="sxs-lookup"><span data-stu-id="12b09-115">You can exclude disks from replication.</span></span> <span data-ttu-id="12b09-116">Například možná nebudete chtít replikovat disky s dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="12b09-116">For example you might not want to replicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="12b09-117">Další informace</span><span class="sxs-lookup"><span data-stu-id="12b09-117">Learn more</span></span>](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a><span data-ttu-id="12b09-118">Replikace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="12b09-118">Replicate VMs</span></span>

<span data-ttu-id="12b09-119">Než začnete, podívejte se na rychlé video s přehledem</span><span class="sxs-lookup"><span data-stu-id="12b09-119">Before you start, watch a quick video overview</span></span>

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. <span data-ttu-id="12b09-120">Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="12b09-120">Click **Step 2: Replicate application** > **Source**.</span></span>
2. <span data-ttu-id="12b09-121">V **zdroj**, vyberte konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="12b09-121">In **Source**, select the configuration server.</span></span>
3. <span data-ttu-id="12b09-122">V **počítač typ**, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="12b09-122">In **Machine type**, select **Virtual Machines**.</span></span>
4. <span data-ttu-id="12b09-123">V **vCenter vSphere Hypervisor**, vyberte server vCenter, který spravuje hostitelů vSphere nebo vyberte hostitele.</span><span class="sxs-lookup"><span data-stu-id="12b09-123">In **vCenter/vSphere Hypervisor**, select the vCenter server that manages the vSphere host, or select the host.</span></span>
5. <span data-ttu-id="12b09-124">Vyberte procesový server.</span><span class="sxs-lookup"><span data-stu-id="12b09-124">Select the process server.</span></span> <span data-ttu-id="12b09-125">Pokud jste nevytvořili žádné servery další proces bude konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="12b09-125">If you haven't created any additional process servers this will be the configuration server.</span></span> <span data-ttu-id="12b09-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12b09-126">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. <span data-ttu-id="12b09-128">V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit neúspěšný přes virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="12b09-128">In **Target**, select the subscription and the resource group in which you want to create the failed over VMs.</span></span> <span data-ttu-id="12b09-129">Vyberte model nasazení, kterou chcete použít v Azure (klasický nebo prostředek management), pro selhání virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="12b09-129">Choose the deployment model that you want to use in Azure (classic or resource management), for the failed over VMs.</span></span>


7. <span data-ttu-id="12b09-130">Vyberte účet úložiště Azure, který chcete použít pro replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="12b09-130">Select the Azure storage account you want to use for replicating data.</span></span> <span data-ttu-id="12b09-131">Pokud nechcete použít účet, který jste již nastavili, můžete vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="12b09-131">If you don't want to use an account you've already set up, you can create a new one.</span></span>

8. <span data-ttu-id="12b09-132">Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří.</span><span class="sxs-lookup"><span data-stu-id="12b09-132">Select the Azure network and subnet to which Azure VMs will connect, when they're created after failover.</span></span> <span data-ttu-id="12b09-133">Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="12b09-133">Select **Configure now for selected machines**, to apply the network setting to all machines you select for protection.</span></span> <span data-ttu-id="12b09-134">Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač.</span><span class="sxs-lookup"><span data-stu-id="12b09-134">Select **Configure later** to select the Azure network per machine.</span></span> <span data-ttu-id="12b09-135">Pokud nechcete použít stávající síť, můžete vytvořit jeden.</span><span class="sxs-lookup"><span data-stu-id="12b09-135">If you don't want to use an existing network, you can create one.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. <span data-ttu-id="12b09-137">V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="12b09-137">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want to replicate.</span></span> <span data-ttu-id="12b09-138">Můžete vybrat pouze počítače, pro které je možné povolit replikaci.</span><span class="sxs-lookup"><span data-stu-id="12b09-138">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="12b09-139">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12b09-139">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. <span data-ttu-id="12b09-141">V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.</span><span class="sxs-lookup"><span data-stu-id="12b09-141">In **Properties** > **Configure properties**, select the account that will be used by the process server to automatically install the Mobility service on the machine.</span></span>
11. <span data-ttu-id="12b09-142">Ve výchozím nastavení jsou všechny disky replikovat.</span><span class="sxs-lookup"><span data-stu-id="12b09-142">By default all disks are replicated.</span></span> <span data-ttu-id="12b09-143">Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete, aby k replikaci.</span><span class="sxs-lookup"><span data-stu-id="12b09-143">Click **All Disks** and clear any disks you don't want to replicate.</span></span> <span data-ttu-id="12b09-144">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12b09-144">Then click **OK**.</span></span> <span data-ttu-id="12b09-145">Později můžete nastavit další vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="12b09-145">You can set additional VM properties later.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. <span data-ttu-id="12b09-147">V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad.</span><span class="sxs-lookup"><span data-stu-id="12b09-147">In **Replication settings** > **Configure replication settings**, verify that the correct replication policy is selected.</span></span> <span data-ttu-id="12b09-148">Pokud změníte zásady, změny se použijí pro replikaci počítač a pro nové počítače.</span><span class="sxs-lookup"><span data-stu-id="12b09-148">If you modify a policy, changes will be applied to replicating machine, and to new machines.</span></span>
12. <span data-ttu-id="12b09-149">Povolit **konzistence pro víc Virtuálních** Pokud chcete shromažďovat počítače do replikační skupiny a zadejte název pro skupinu.</span><span class="sxs-lookup"><span data-stu-id="12b09-149">Enable **Multi-VM consistency** if you want to gather machines into a replication group, and specify a name for the group.</span></span> <span data-ttu-id="12b09-150">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="12b09-150">Then click **OK**.</span></span> <span data-ttu-id="12b09-151">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="12b09-151">Note that:</span></span>

    * <span data-ttu-id="12b09-152">Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="12b09-152">Machines in replication groups replicate together, and have shared crash-consistent and app-consistent recovery points when they fail over.</span></span>
    * <span data-ttu-id="12b09-153">Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="12b09-153">We recommend that you gather VMs and physical servers together so that they mirror your workloads.</span></span> <span data-ttu-id="12b09-154">Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží stejné zatížení a potřebujete konzistenci.</span><span class="sxs-lookup"><span data-stu-id="12b09-154">Enabling multi-VM consistency can impact workload performance, and should only be used if machines are running the same workload and you need consistency.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. <span data-ttu-id="12b09-156">Klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="12b09-156">Click **Enable Replication**.</span></span> <span data-ttu-id="12b09-157">Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="12b09-157">You can track progress of the **Enable Protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span> <span data-ttu-id="12b09-158">Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="12b09-158">After the **Finalize Protection** job runs the machine is ready for failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12b09-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12b09-159">Next steps</span></span>

<span data-ttu-id="12b09-160">Přejděte na [krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="12b09-160">Go to [Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
